# Agent Handoff/Continuation Patterns — Cross-Framework Survey

**Date:** 2026-07-23  
**Scope:** LangGraph, CrewAI, AutoGen, MCP, hummbl-dev patterns  
**Governance:** hummbl-dev/research-source-packets, schema v0.1, Krineia chain `hummbl-dev-primary`

---

## Executive Summary

This packet maps **agent handoff/continuation patterns** across four major frameworks plus hummbl-dev's existing governance schemas. The core finding: **all frameworks converge on a three-layer pattern** — (1) **state persistence** (checkpointers/stores), (2) **execution interruption/resumption** (interrupts/breakpoints/handoffs), (3) **cross-session memory** (stores/scopes/ledgers) — but differ in granularity, transport, and authority model.

**hummbl-dev already has the governance layer** (agent-handoffs-v0.1, conversation-work-session-handoff-profile-v0.1, control-plane vocab). The gap is **runtime integration** — wiring these schemas into LangGraph checkpointers, CrewAI Memory, AutoGen agentchat, MCP servers.

---

## 1. Framework Comparison Matrix

| Dimension | LangGraph | CrewAI | AutoGen 0.4.x | MCP | hummbl-dev |
|---|---|---|---|---|---|
| **State persistence** | Checkpointers (InMemory, SQLite, Postgres, CosmosDB) + Stores | LanceDB (default) + custom StorageBackend | autogen-core memory interface | Server-side (per MCP server) | KRINEIA append-only chains |
| **Interruption/resumption** | `interrupt()` + `Command(resume=...)` + `interrupt_before/after` | `checkpoint=True/CheckpointConfig` + CLI `replay` | Agent handoff in agentchat (v0.4) | `sampling/createMessage` (server→client) | Handoff packet + receipt chain |
| **Cross-thread memory** | Stores (namespace tuples, semantic search) | Memory scopes/slices (hierarchical tree) | Shared memory via autogen-core | Client aggregates across servers | Belief-aware handoffs + outcome ledger |
| **Authority model** | Thread-scoped, human-in-the-loop via interrupts | Crew/agent scoped, LLM-inferred scopes | Agent-scoped, delegation via handoff | Server capabilities + client sampling | Delegation token + expiry + receipt |
| **Transport** | In-process (Python) / LangSmith Agent Server | In-process / CLI | In-process / distributed (0.4 core) | STDIO / Streamable HTTP | Git + KRINEIA (append-only) |
| **Checkpoint granularity** | Super-step (configurable) + per-node writes | Task-level (after each task) | Agent-turn level | N/A (stateless protocol) | Packet + receipt (immutable) |
| **Durability modes** | `sync` / `async` / `exit` | JSON files or SQLite | Depends on memory backend | Server-dependent | Git commit (append-only) |
| **Time travel / replay** | `get_state_history` + `update_state` (fork) | `crewai replay -t <task_id>` | Not documented | N/A | Packet supersession references |
| **Schema validation** | Pydantic state + channel reducers | LanceDB vector index + LLM analysis | Pydantic models in autogen-core | JSON-RPC 2.0 + schema per primitive | JSON Schema (agent-handoffs-v0.1) |

---

## 2. Deep Dive: LangGraph (Most Mature)

### 2.1 Persistence Architecture
```
Checkpointer (per-thread)          Store (cross-thread)
├── InMemorySaver (dev)            ├── InMemoryStore
├── SqliteSaver (local)            ├── PostgresStore (+ pgvector)
├── PostgresSaver (prod)           ├── MongoDBStore
├── AsyncPostgresSaver             ├── RedisStore
└── CosmosDBSaver (Azure)          └── Custom (BaseStore)
```

**Key insight:** Dual system — checkpointer for *execution state* (thread-scoped), store for *semantic memory* (cross-thread, namespaced).

### 2.2 Interruption/Resumption
```python
# Dynamic interrupt (human-in-the-loop)
def node(state):
    decision = interrupt({"question": "Approve?", "details": state.details})
    return {"approved": decision}

# Resume
graph.stream_events(Command(resume=True), config={"configurable": {"thread_id": "t1"}}, version="v3")
```

**Rules:** No try/except around `interrupt()`, no conditional skip, no loops with interrupts — use conditional edges instead. Node re-runs from start on resume.

### 2.3 Memory (Stores)
- Namespaces: tuple `("user_id", "memories")` — prefix matching on search
- Semantic search via embedding index (`index={"embed": ..., "dims": 1536, "fields": [...]}`)
- `asearch(namespace, query, limit)` returns scored items
- `list_namespaces(prefix, max_depth)` for discovery

### 2.4 DeltaChannel Optimization
- Stores only sentinel (`_DeltaSnapshot`) in checkpoint blob
- Reconstructs by replaying ancestor writes through reducer
- `get_delta_channel_history` walks ancestor chain to nearest snapshot
- **Critical:** `get_tuple` by specific `checkpoint_id` must work (O(1) lookup)

### 2.5 Durability Modes
| Mode | When persisted | Failure risk |
|---|---|---|
| `exit` | On graph exit | Lose in-flight state on crash |
| `async` | Background, non-blocking | Small window of loss |
| `sync` | Before next step | None (slowest) |

---

## 3. Deep Dive: CrewAI

### 3.1 Unified Memory System
```python
memory = Memory(
    recency_weight=0.5, semantic_weight=0.3, importance_weight=0.2,
    recency_half_life_days=7,
    llm="gpt-4o-mini",
    embedder={"provider": "openai", "config": {"model_name": "text-embedding-3-large"}},
    storage="./.crewai/memory"
)
```

**Hierarchical scopes:** `/project/alpha/architecture`, `/agent/researcher` — LLM infers scope on save, or explicit `scope="/agent/researcher"`.

**Slices (read-only views across scopes):**
```python
view = memory.slice(scopes=["/agent/researcher", "/company/knowledge"], read_only=True)
```

**Composite scoring:** `semantic * sim + recency * decay + importance * imp`

### 3.2 Checkpointing
```python
crew = Crew(agents=[...], tasks=[...], checkpoint=CheckpointConfig(
    location="./.checkpoints",
    on_events=["task_completed"],
    provider=JsonProvider(),  # or SqliteProvider()
    max_checkpoints=5
))
```
- CLI: `crewai replay -t <task_id>` to resume from task
- Logs: `output_log_file="logs.json"` (JSON or txt)

### 3.3 Memory Events (for observability)
- `MemoryQueryStartedEvent`, `MemoryQueryCompletedEvent`, `MemorySaveStartedEvent`, `MemorySaveFailedEvent`
- `MemoryRetrievalStartedEvent`, `MemoryRetrievalCompletedEvent` (agent context injection)

### 3.4 Consolidation/Dedup
- On save: compares against existing records (`consolidation_threshold=0.85`)
- LLM decides: `keep` / `update` / `delete` / `insert_new`
- Intra-batch dedup: cosine >= 0.98 drops later item (no LLM call)

---

## 4. Deep Dive: AutoGen 0.4.x

### 4.1 Architecture
```
autogen-core (runtime, memory, tracing, workbench)
  └── autogen-agentchat (AssistantAgent, UserProxyAgent, handoff tools)
  └── autogen-ext (OpenAI, MCP, etc.)
```

### 4.2 AgentChat Handoff (v0.4)
- **Agent-to-agent handoff** via structured tool: `Handoff(to_agent, context, reason)`
- **Context transfer:** Message history slice + state summary + pending actions
- **Delegation scope:** Defined in handoff tool parameters (what next agent can do)
- **Authority:** Implicit via tool schema — no token expiry, relies on agent compliance

### 4.3 Memory (autogen-core)
- `BaseMemory` interface: `get`, `set`, `delete`, `search`
- Implementations: `ListMemory`, `VectorMemory` (Chroma, Pinecone, etc.)
- Shared across agents via `Memory` instance passed to agents

### 4.4 Workbench (MCP integration)
- `McpWorkbench` wraps MCP server (STDIO/SSE)
- `list_tools()`, `call_tool(name, args)` — standard MCP primitives
- Can be used as tool provider for agents

### 4.5 Gap
- **No official agent_handoff docs page** (404 on microsoft.github.io)
- Checkpointing/persistence patterns not clearly documented for 0.4.x
- Need to inspect `packages/autogen-agentchat` source for handoff implementation

---

## 5. Deep Dive: MCP (Model Context Protocol)

### 5.1 Architecture
```
MCP Host (AI App) → MCP Client (per server) → MCP Server (tools/resources/prompts)
```

**Transports:** STDIO (local), Streamable HTTP (remote + SSE)

### 5.2 Primitives for Handoff
| Primitive | Direction | Use for handoff |
|---|---|---|
| `tools/list`, `tools/call` | Client → Server | Delegate action to specialized server |
| `resources/list`, `resources/read` | Client → Server | Transfer context (files, DB records) |
| `prompts/list`, `prompts/get` | Client → Server | Transfer prompt templates |
| `sampling/createMessage` | Server → Client | Server requests LLM completion from host |
| `elicitation/create` | Server → Client | Server asks user for input |

### 5.3 Handoff Patterns
1. **Tool delegation:** Agent calls tool on specialized MCP server (e.g., `filesystem`, `github`, `postgres`)
2. **Resource transfer:** Server exposes `resource://...` URIs; client reads and passes to next agent
3. **Prompt templating:** Server provides structured prompts; client fills and executes
4. **Sampling:** Server asks host LLM to reason — enables server-side intelligence without LLM dependency

### 5.4 Notifications for Dynamic Handoff
- `notifications/tools/list_changed` — server tells client new tools available
- Client can re-list and offer to agent dynamically

### 5.5 Authority Model
- **Capabilities negotiated at init:** `tools`, `resources`, `prompts`, `sampling`, `elicitation`
- **No built-in delegation tokens** — authority is connection-scoped
- **OAuth for remote servers** — tokens managed at transport layer

---

## 6. hummbl-dev Existing Patterns

### 6.1 agent-handoffs-v0.1 Schema
```json
{
  "handoffManifest": { "id", "title", "version", "scope" },
  "authority": { "issuer", "issuerType": "human|agent|system", "basis" },
  "handoffContract": {
    "fromAgent": { "id", "type", "name" },
    "toAgent": { "id", "type", "name" },
    "contextEnvelope": { "objective", "inScope[]", "outOfScope[]", "constraints[]", "priorContext[]" },
    "delegationScope": { "grantedAuthority[]", "limits[]" },
    "expiry": "ISO8601",
    "receiptChain": [{ "hop", "actor", "action", "at", "ref" }]
  },
  "receiptRequirements": { "acknowledgeBy", "produce[]" }
}
```

### 6.2 Conversation/Work-Session Handoff Profile
Extends base with:
- `outcomeLedger` (COMPLETED/IN_PROGRESS/BLOCKED...)
- `decisionClaimLedger` (USER_DECISION, ORG_DECISION, MODEL_INFERENCE...)
- `artifactLedger` (kind, classification, state, mutation permission)
- `openWorkLedger` (next action, dependencies, blockers, approvals, stop conditions)
- `capabilityGapLedger` (PASS/FAIL/UNAVAILABLE...)
- `authorityContract` (transferred/withheld/revoked authority)
- `resumeToken` (firstNextAction + minimumReferences)

### 6.3 Control Plane Patterns (agent-control-plane-patterns)
**Vocabulary:** desired state / observed state / drift / reconciliation / controller / operator / CRD / finalizer / status conditions / workqueues / leader election

**Mapping to agent handoff:**
| Control Plane | Agent Handoff |
|---|---|
| Desired state | Handoff contract (objective, scope, constraints) |
| Observed state | Resume token + outcome ledger |
| Drift | Capability gaps, blockers, expired delegations |
| Controller | Receiving agent (reconciles contract → action) |
| CRD | Handoff packet schema (agent-handoffs-v0.1) |
| Finalizer | Receipt chain (immutable audit) |
| Status conditions | Capability gap ledger, outcome ledger |
| Workqueue | Open work ledger (dependency-ordered) |

---

## 7. Integration Gap Analysis

| Need | LangGraph | CrewAI | AutoGen | MCP | hummbl-dev | Gap |
|---|---|---|---|---|---|---|
| **Structured handoff packet** | ❌ (manual) | ❌ (manual) | ✅ (tool) | ❌ (resources) | ✅ Schema exists | **No runtime validator** |
| **Delegation token with expiry** | ❌ | ❌ | ❌ | ❌ (OAuth only) | ✅ Schema | **No runtime enforcement** |
| **Receipt chain (append-only)** | ❌ | ❌ | ❌ | ❌ | ✅ KRINEIA | **No checkpointer integration** |
| **Capability gap ledger** | ❌ | ❌ | ❌ | ❌ | ✅ Schema | **No runtime reporter** |
| **Outcome/decision/artifact ledgers** | ❌ | Partial (CrewOutput) | ❌ | ❌ | ✅ Schema | **No storage backend** |
| **Resume token (firstNextAction)** | Via `update_state` | Via `replay` | ❌ | ❌ | ✅ Schema | **No cross-framework format** |
| **Cross-thread/cross-agent memory** | Stores (semantic) | Memory scopes/slices | VectorMemory | Server resources | ❌ | **No unified namespace** |
| **Human-in-the-loop approval** | `interrupt()` + `Command` | `checkpoint` + CLI | ❌ | `elicitation` | ✅ Schema | **No unified HITL protocol** |
| **Time travel / fork** | `get_state_history` + `update_state` | `replay -t` | ❌ | ❌ | ❌ | **No cross-framework fork** |

---

## 8. Recommended Integration Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                    HUMMBL Handoff Gateway                    │
│  (Validates agent-handoffs-v0.1, emits KRINEIA receipts)    │
└─────────────────────────────────────────────────────────────┘
         │                    │                    │
         ▼                    ▼                    ▼
┌──────────────────┐ ┌──────────────────┐ ┌──────────────────┐
│ LangGraph Adapter │ │ CrewAI Adapter   │ │ AutoGen Adapter  │
│ - Checkpointer    │ │ - Memory         │ │ - AgentChat      │
│ - Store           │ │ - Checkpointing  │ │ - MCP Workbench  │
│ - Interrupts      │ │ - Scopes/Slices  │ │ - Handoff Tool   │
└──────────────────┘ └──────────────────┘ └──────────────────┘
         │                    │                    │
         └────────────────────┼────────────────────┘
                              ▼
                    ┌────────────────────┐
                    │  KRINEIA Receipt   │
                    │  Chain (Git)       │
                    │  + hummbl-agent    │
                    │  Ledger            │
                    └────────────────────┘
```

### 8.1 Adapter Responsibilities
| Adapter | Input | Output | KRINEIA Action |
|---|---|---|---|
| **LangGraph** | `interrupt()` payload, `Command(resume)` | `handoffContract`, `receiptChain.append()` | `append` on interrupt + resume |
| **CrewAI** | `CheckpointConfig` events, `CrewOutput` | `outcomeLedger`, `decisionClaimLedger` | `append` on task complete + crew finish |
| **AutoGen** | `Handoff` tool calls, agent state | `contextEnvelope`, `delegationScope` | `append` on handoff emit + accept |
| **MCP** | `tools/call` + `resources/read` | `contextEnvelope.priorContext` refs | `append` on cross-server call |

### 8.2 KRINEIA Receipt Schema (for handoff)
```json
{
  "hop": 1,
  "actor": "langgraph-adapter",
  "action": "HANDOFF_ISSUED",
  "at": "2026-07-23T18:00:00Z",
  "ref": "handoff-packet-id: uuid",
  "payload_hash": "sha256:..."
}
```

### 8.3 Cross-Framework Resume Token Format
```json
{
  "handoff_id": "uuid",
  "firstNextAction": "Review PR #234 and approve if tests pass",
  "minimumReferences": [
    "handoffContract.contextEnvelope.objective",
    "outcomeLedger[0].artifactReferences[0]",
    "capabilityGapLedger[0].evidenceReference"
  ],
  "validUntil": "2026-07-30T18:00:00Z"
}
```

---

## 9. Immediate Next Steps

1. **Create KRINEIA receipt emitter** for LangGraph `interrupt`/`Command(resume)` events
2. **Build LangGraph checkpointer wrapper** that validates `agent-handoffs-v0.1` on `put`/`put_writes`
3. **Map CrewAI `CrewOutput` → `outcomeLedger` + `decisionClaimLedger`** auto-extraction
4. **Prototype AutoGen `Handoff` tool → KRINEIA receipt** in `autogen-ext`
5. **Define unified `ResumeToken` JSON format** + cross-framework parser
6. **Add `capabilityGapLedger` reporter** to LangGraph store (scan for missing tools/skills)
7. **Write ADR** for handoff gateway architecture in `hummbl-dev/agent-handoffs/docs/adr/`

---

## 10. References

- LangGraph: https://docs.langchain.com/oss/python/langgraph/checkpointers, /stores, /interrupts, /persistence
- CrewAI: https://docs.crewai.com/concepts/memory, /checkpointing
- AutoGen: https://github.com/microsoft/autogen/tree/main/python/packages
- MCP: https://modelcontextprotocol.io/docs/concepts/architecture
- hummbl-dev/agent-handoffs: schemas `agent-handoffs-v0.1.json`, `conversation-work-session-handoff-profile-v0.1.json`
- hummbl-dev/agent-control-plane-patterns: `docs/prior-art.md` (vocabulary mapping)
- KRINEIA: `hummbl-dev/hummbl-dev/KRINEIA.md` (append-only receipt chains)

---

**End of packet.** Ready for review and ADR authoring.