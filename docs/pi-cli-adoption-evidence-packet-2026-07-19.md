# Pi CLI Adoption Evidence Packet

Status: candidate research note. This document records external adoption
signals for Pi, the terminal-native CLI coding agent, without treating
popularity claims, architectural conclusions, or HUMMBL integration decisions
as canon.

Date: July 19, 2026.

Issue: [hummbl-dev/research-source-packets#13](https://github.com/hummbl-dev/research-source-packets/issues/13)

## Stale-state warning

> **All GitHub item states (open, closed, merged, draft) were captured on
> 2026-07-19 and are not durable facts.** PRs can be merged, closed, reopened,
> or superseded at any time. Issue labels, review comments, and body text can
> change. Treat every state recorded below as a point-in-time observation, not
> a permanent property. Re-verify before any adoption, dependency, or
> integration decision.

## Source posture labels

- `SOURCE_CLAIM`: claimed by Pi first-party sources (pi.dev, earendil-works/pi
  repository, official documentation).
- `EXTERNALLY_REPORTED`: reported by third-party adopters, PR authors, or
  community members. Not first-party Pi claims.
- `LOCAL_AUDIT_REQUIRED`: must be verified locally before any adoption.
- `HUMMBL_INTERPRETATION`: interpretation after mapping evidence to HUMMBL /
  BaseN / Ownward governance concepts. Candidate only, non-canon.
- `DO_NOT_ADOPT_YET`: not adopted as dependency, standard, or canon by this
  packet.

## Strategic question

**Where and how is Pi being adopted across the agent tooling ecosystem, and
what does that adoption evidence reveal about Pi's runtime interfaces, safety
boundaries, failure semantics, and integration maturity?**

`HUMMBL_INTERPRETATION`: The adoption evidence shows Pi gaining traction as a
provider, runtime, evaluator target, transcript source, skills target,
packaging target, and observability surface. The evidence is dominated by
open, unmerged contributions, with one merged integration and several stale
drafts. This packet does not validate Pi as a HUMMBL dependency or recommend
adoption.

## Answer-first findings

1. **Pi is adopted across at least seven distinct lanes**, but only one
   secondary integration is merged (dustinblack/agent-dashboard#81). All five
   primary sources remain open or closed-without-merge. The adoption signal is
   real but immature.

2. **Three Pi interfaces are used in practice**: the JSON event stream
   (`--mode json`), the RPC JSONL protocol (`--mode rpc`), and session JSONL
   files on disk. Extensions, skills, and AGENTS.md are used as configuration
   and capability surfaces. No adopter confirmed direct SDK imports in
   production; promptfoo explicitly rejected the SDK in favor of CLI
   subprocess isolation.

3. **Every adopter that touches tool execution adds its own safety boundary**,
   because Pi intentionally ships without permission prompts, sandboxing, or
   MCP configuration. The patterns include: default-deny extension gates
   (t3code), read-only tool defaults with opt-in for destructive tools
   (promptfoo), path isolation and runtime detection (token-optimizer), and
   container mounting with no auth gate (agent-dashboard).

4. **Exit code 0 with failure inside JSON events is a confirmed, recurring
   failure semantic.** promptfoo and rix both document it independently.
   promptfoo mitigates by reading `stopReason` / `errorMessage` from the final
   assistant message. rix reports that an invalid API key causes Pi to swallow
   the error into its JSON stream and exit 0, producing a false "success with
   no PRs" result.

5. **The identifier migration from `badlogic` / `@mariozechner` to
   `earendil-works` is complete at the repository level** but incomplete in
   third-party references. The GitHub repo `badlogic/pi-mono` redirects to
   `earendil-works/pi`. The npm package is now
   `@earendil-works/pi-coding-agent`. However, dustinblack/agent-dashboard#51
   and pingdotgg/t3code#397 still reference the old `badlogic/pi-mono` and
   `@mariozechner/pi-coding-agent` identifiers.

6. **No HUMMBL / BaseN / Ownward integration decision is declared by this
   packet.** Pi is recorded as a candidate observation target only.

## Identifier-migration note

Pi has undergone an identifier migration. The following table preserves all
historical identifiers for searchability.

<!-- markdownlint-disable MD013 -->

| Identifier | Current status | Notes |
| --- | --- | --- |
| `badlogic/pi-mono` | Redirects to `earendil-works/pi` | GitHub repo transfer confirmed 2026-07-19. Original author: Mario Zechner (`badlogic`). |
| `@mariozechner/pi-coding-agent` | Superseded by `@earendil-works/pi-coding-agent` | npm package rename. Old package may still resolve but is not canonical. |
| `pi.dev` | Active | Canonical homepage. Domain donated by exe.dev. |
| `earendil-works/pi` | Canonical repository | Monorepo; coding agent lives at `packages/coding-agent`. Owner: Earendil Inc. (`earendil.com`). |
| `@earendil-works/pi-coding-agent` | Canonical npm package | Current install target. |
| `mariozechner.at` | Author blog | Mario Zechner's personal blog; hosts Pi design posts. |

<!-- markdownlint-enable MD013 -->

`LOCAL_AUDIT_REQUIRED`: Third-party sources that still reference
`badlogic/pi-mono` or `@mariozechner/pi-coding-agent` (e.g.,
dustinblack/agent-dashboard#51, pingdotgg/t3code#397) should be updated to the
current identifiers. The old identifiers are preserved here for search
continuity, not for adoption.

## Current Pi identity (first-party)

- Canonical project: `earendil-works/pi` (monorepo)
- Coding agent path: `packages/coding-agent`
- Current CLI package: `@earendil-works/pi-coding-agent`
- Homepage: <https://pi.dev>
- Company: Earendil Inc. (<https://earendil.com>)
- License: MIT
- Repository created: 2025-08-09 (as `badlogic/pi-mono`, now `earendil-works/pi`)

`SOURCE_CLAIM` (pi.dev, accessed 2026-07-19): Pi is described as "a minimal
agent harness" with four modes: interactive, print/JSON, RPC, and SDK. Pi
supports 15+ providers (Anthropic, OpenAI, Google, Azure, Bedrock, Mistral,
Groq, Cerebras, xAI, Hugging Face, Kimi, MiniMax, NVIDIA, OpenRouter, Ollama).
Pi intentionally omits MCP, sub-agents, permission popups, plan mode, built-in
to-dos, and background bash — all are buildable via extensions.

`SOURCE_CLAIM` (pi.dev, accessed 2026-07-19): Pi's "What we didn't build"
section explicitly states: "No MCP. Build CLI tools with READMEs (Skills), or
build an extension that adds MCP support." and "No permission popups. Run in a
container, or build your own confirmation flow with extensions."

## Adoption-lane taxonomy

<!-- markdownlint-disable MD013 -->

| Lane | Sources | Maturity |
| --- | --- | --- |
| Evaluator / eval provider | promptfoo/promptfoo#9707 | Open, not merged |
| GUI / runtime provider | pingdotgg/t3code#3818, #397, #402 | Closed without merge; issue #402 still open |
| Transcript / learning source | microsoft/SkillOpt#83 | Open, draft |
| CI coding agent | Tim-Pohlmann/rix#67 | Open, not merged |
| Extension / skill / AGENTS.md adapter | DeusData/codebase-memory-mcp#534 | Open, not merged |
| Skills target | addyosmani/agent-skills#355, #365; msitarzewski/agency-agents#470, #591; JuliusBrussee/caveman#161, #551 | All open; #355 is an issue, #365 is its PR |
| Packaging target / profile | dustinblack/agent-dashboard#51, #81; fletchgqc/agentbox#71 | #81 merged; #51 closed by #81; #71 open |
| Observability surface | dustinblack/agent-dashboard#81 (pi-otel); himattm/prism#167 (status line) | #81 merged; #167 open |
| Token optimization | shelbon/token-optimizer#1 | Open, not merged |

## Interface / capability matrix

| Source | Interactive CLI | `--mode json` | `--mode rpc` | Session JSONL | Extensions | Skills | AGENTS.md | SDK import |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| promptfoo#9707 | — | Yes (`--mode json --no-session`) | — | — | Disabled by default | Disabled by default | — | Explicitly rejected |
| t3code#3818 | — | — | Yes (JSONL over stdin/stdout) | — | Yes (default-deny approval gate) | — | — | Types-only devDependency |
| SkillOpt#83 | — | — | — | Yes (`~/.pi/agent/sessions/`) | — | — | — | — |
| rix#67 | — | Yes (JSON mode) | — | — | — | — | — | — |
| codebase-memory-mcp#534 | — | — | — | — | Yes (`cbmem.ts`) | Yes (`codebase-memory`) | Yes | — |
| agent-skills#365 | — | — | — | — | — | Yes (discovery) | Yes | — |
| agency-agents#591 | — | — | — | — | — | Yes (SKILL.md generation) | — | — |
| caveman#551 | — | — | — | — | — | Yes (symlink/copy install) | Yes | — |
| agent-dashboard#81 | Yes (profile) | — | — | — | Yes (pi-otel) | — | — | — |
| agentbox#71 | Yes (`--tool pi`) | — | — | — | — | — | — | — |
| prism#167 | — | — | — | — | Yes (status line shim) | — | — | — |
| token-optimizer#1 | — | — | — | Yes (session parsing) | Yes (`pi/extension.ts`) | Yes | — | — |

<!-- markdownlint-enable MD013 -->

`HUMMBL_INTERPRETATION`: The matrix shows that no single adopter uses all Pi
interfaces. JSON event stream and RPC are used for programmatic integration;
session JSONL is used for post-hoc transcript harvesting; extensions are the
primary safety and observability mechanism; skills and AGENTS.md are the
primary configuration and capability surfaces. Direct SDK imports are not
confirmed in any production integration.

## Primary source summaries

### 1. promptfoo/promptfoo#9707 — Pi as agent-evaluation provider

- URL: <https://github.com/promptfoo/promptfoo/pull/9707>
- State: **OPEN** (not draft, not merged). Retrieved 2026-07-19.
- Author: mldangelo (Michael). Created 2026-06-12, last updated 2026-07-09.
- Labels: none.

`EXTERNALLY_REPORTED`: Adds a provider for Pi to promptfoo, an agent
evaluation framework. Promptfoo spawns the `pi` CLI per call in one-shot JSON
event-stream mode (`pi --mode json --no-session`), so evals exercise the same
runtime users get in their terminal. No npm dependency is added: the provider
resolves the CLI from `pi_path`, a project-local
`@earendil-works/pi-coding-agent` install, or `PATH`.

**Provider IDs**: `pi` (default model) and `pi:<provider>/<model>` (explicit
model pattern, e.g. `pi:anthropic/claude-sonnet-4-5`).

**Design decisions** (author-reported):

- CLI subprocess over in-process SDK. Pi's SDK is ESM-only with restrictive
  exports, requires Node >= 22.19, and ships `undici@8` — embedding it
  in-process risks global dispatcher/state contamination. Process isolation
  means evals measure the real product surface.
- OpenCode-style safety defaults. Pi has no built-in permission or sandbox
  system, so: no `working_dir` → temp dir with all tools disabled (chat-only);
  with `working_dir` → read-only tools (`read`, `grep`, `find`, `ls`);
  `bash`/`edit`/`write` are explicit opt-in.
- Hermetic by default. `--offline`, `--no-session`, and extension/skill/
  prompt-template/context-file discovery disabled so eval prompts pass through
  verbatim and results are reproducible; each is re-enableable.
- Failure detection from `stopReason`. Pi exits 0 in JSON mode even when the
  agent run fails; the provider reads the final assistant message's
  `stopReason` / `errorMessage`. Nonzero/signal exits are never reported or
  cached as success.
- Credential hygiene. `apiKey` is injected via env var (or `api_key_env` for
  unrecognized providers) — never argv — keeping secrets out of `ps` output,
  debug logs, and the persisted cache key.
- Hang resistance. The run settles from process `exit` with a 1s stdio flush
  grace, so a descendant process holding the inherited stdout pipe cannot hang
  the eval row.

**QA** (author-reported, live with real API keys): basic example, working-dir
example, model-comparison (OpenAI + Anthropic + Gemini), bare `pi` provider id,
agent-rubric grading, cache behavior, env injection, nonexistent model, and
missing `pi` binary all validated. Head-to-head against Claude Agent SDK on a
security-audit task: both found 4/4 planted issues; Pi was ~2.5x cheaper
($0.067 vs $0.166) but slower (60.4s vs 35.6s).

**Review hardening** (author-reported): A multi-agent adversarial review
pre-merge confirmed and fixed 11 issues, including a P1 where an `--api-key`
argv fallback would have leaked the raw key into the persisted cache key.

**Test coverage**: 45 unit tests. Codecov reports 95.6% patch coverage.
Copilot AI review completed with 2 comments.

`LOCAL_AUDIT_REQUIRED`: The head-to-head comparison and QA results are
author-reported and not independently verified by this packet.

### 2. pingdotgg/t3code#3818 — Pi as first-class GUI/runtime provider

- URL: <https://github.com/pingdotgg/t3code/pull/3818>
- State: **CLOSED** (not merged). Closed 2026-07-19. Retrieved 2026-07-19.
- Author: ahmadaccino. Created 2026-07-09, closed 2026-07-19.
- Labels: `vouch:unvouched`, `size:XXL`.

`EXTERNALLY_REPORTED`: Adds Pi as a selectable coding-agent provider in T3
Code, alongside Codex / Claude / Cursor / Grok / OpenCode. Integrates via Pi's
native `pi --mode rpc` JSONL protocol over the shared Effect
`ChildProcessSpawner`. The Pi package is a types-only devDependency — the
user's installed `pi` binary is the runtime. Disabled by default.

**Architecture** (author-reported):

- `PiRpcClient` — typed JSONL transport over `ChildProcessSpawner`
  (stdin-writer + stdout-reader fibers, request/response correlation, scoped
  teardown) plus pure, unit-tested parse/extract helpers.
- `PiAdapter` — per-thread sessions, `text_delta` streaming, tool lifecycle,
  finalize on `agent_end`, mid-turn `steer`, image attachments, in-session
  model switch + thinking level, and `fork`-based rollback. Emits the same
  canonical runtime events as the other adapters (including `thread.started`
  for projection parity, and `exitKind: "error"` on unexpected exit).
- Tool-approval gate driven by `runtimeMode` (like Claude/Cursor): a bundled,
  default-deny Pi extension is injected via `--extension` and verified with a
  `get_commands` load handshake (fail-closed), bridging Pi's `extension_ui`
  requests to the canonical approval / user-input events. Bypass via
  `T3_PI_APPROVAL_MODE=auto-accept-edits`.
- `PiProvider` — `pi --version` probe, live model discovery
  (`get_available_models`), and a models-based auth verdict.
- `PiTextGeneration` — stateless RPC + `get_last_assistant_text`.

**Consolidation**: This PR consolidates earlier community attempts
(`#2748`, `#2800`, `#2831`, `#2856`, and `#2812`) into a single
consistency-first implementation.

**Review state**: CodeRabbit auto-review skipped (disabled for repo).
Macroscope approvability verdict: "Needs human review" — 4 blocking
correctness issues found, diff too large for automated approval. Cursor
Bugbot assessed as "Medium Risk."

`HUMMBL_INTERPRETATION`: The default-deny extension gate with fail-closed
`get_commands` handshake is the most sophisticated safety boundary in the
evidence set. It demonstrates that Pi's extension system can be used to
retrofit permission gates that Pi intentionally omits.

`LOCAL_AUDIT_REQUIRED`: The PR was closed without merge. The closure reason
is not documented in the retrieved data. The implementation may be superseded
by a future attempt or by issue #402's guidance.

### 3. microsoft/SkillOpt#83 — Pi transcript harvesting and learning source

- URL: <https://github.com/microsoft/SkillOpt/pull/83>
- State: **OPEN, DRAFT**. Retrieved 2026-07-19.
- Author: auspic7 (Myeongwon Choi). Created 2026-06-23, last updated
  2026-07-12.
- Labels: none.

`EXTERNALLY_REPORTED`: Adds `--source pi` to SkillOpt-Sleep, so it can harvest
sessions from the Pi coding agent (`~/.pi/agent/sessions/<slug>/*.jsonl`), on
par with existing `claude` / `codex` sources. Pi follows the open Agent Skills
standard and stores sessions as JSONL.

**Pi schema notes** (author-reported, verified against real transcripts):

- Entry discriminator is `type`; `cwd` lives on the single `session` entry
  (not on messages).
- Conversational turns are `type:"message"` with `message.role` in `{user,
  assistant, toolResult}`.
- Content blocks use `toolCall` (not Claude's `tool_use`) and carry `name`;
  `thinking` blocks are private reasoning and are skipped (never leak into
  `assistant_finals`).
- `toolResult` messages carry `toolName` (used to corroborate tool names) and
  `isError` (bool). `isError` is deliberately NOT surfaced as a feedback
  signal — intermediate tool errors are normal in agentic coding and are
  frequently followed by recovery and a successful final result.

**Design note** (author-reported): `isError` is not used as a feedback signal
because treating recovered errors as negative feedback would mislabel
successful sessions as failures and poison the miner's task-outcome labels.
Task outcome is inferred from the user's judgment of the final result.

**Review state**: Reviewer Yif-Yang (contributor) tested locally and confirmed
no regressions. Noted two issues: (1) `PiCliBackend._call()` swallows all
launch/timeout exceptions and ignores non-zero exit codes, so a missing `pi`
binary returns `""` — indistinguishable from an empty model answer (the
sibling `CodexCliBackend` records `last_call_error` + logs a warning); (2) the
section comment above the class still reads "Claude Code CLI backend."
Reviewer offered to help land it with fixes.

`LOCAL_AUDIT_REQUIRED`: The PR remains in draft. The reviewer-noted exception
swallowing is a failure-semantic risk that should be resolved before merge.

### 4. Tim-Pohlmann/rix#67 — Pi as CI coding-agent implementation

- URL: <https://github.com/Tim-Pohlmann/rix/pull/67>
- State: **OPEN** (not draft, not merged). Retrieved 2026-07-19.
- Author: Tim-Pohlmann (Tim Pohlmann). Created 2026-07-10, last updated
  2026-07-10.
- Labels: none.

> **Note**: The issue text references `Tim-Pohlermann/rix` (with an extra
> 'e'), but the actual repository is `Tim-Pohlmann/rix` (one 'n'). The
> canonical URL is <https://github.com/Tim-Pohlmann/rix>.

`EXTERNALLY_REPORTED`: Adds `pi` as a third `ICodingAgent` implementation in
rix, alongside `claude` and `opencode`, backed by the Pi coding agent CLI.
Like opencode, pi is multi-provider: `--model` is forwarded verbatim, and
since pi has no free default provider, `agent-api-key-env` is required when
`agent: pi` is used with `agent-api-key`.

**Failure semantics** (author-reported):

- Cost is read from the `agent_end` JSON event by summing `usage.cost.total`
  across all assistant messages — but pi's true last stdout line is always a
  payload-free `agent_settled` event, so under the current one-line-only
  architecture this reliably reports `$0`. This is a stronger version of the
  same known limitation `OpenCodeAgent` already documents for itself.
- No credentials → failure, diagnostic is a real `.md` doc path the package
  ships (not JSON — pi has no structured error protocol for this path, unlike
  claude/opencode).
- Invalid key → pi swallows the error into its own JSON stream and exits 0,
  so rix reports **success with no PRs**, not a failure.

**Test plan** (author-reported): `dotnet build` succeeds; `dotnet test` —
201/201 passing, including new `PiAgentTests` / `PiAgentCostTests`; two live
e2e bats cases run locally against the real `pi` CLI. Stacked on #64 (live e2e
job harness).

**Review state**: Copilot AI review completed (2 comments). SonarCloud
Quality Gate passed.

`HUMMBL_INTERPRETATION`: The rix PR independently confirms the exit-code-0
failure semantic that promptfoo also documents. The cost-reporting limitation
(`agent_settled` as the final line) is a distinct failure semantic not
reported elsewhere.

`LOCAL_AUDIT_REQUIRED`: The "success with no PRs" false-positive on invalid
keys is a CI reliability risk. Any CI integration using Pi must add its own
credential validation before invoking the Pi CLI.

### 5. DeusData/codebase-memory-mcp#534 — Pi extension/skill/AGENTS.md adapter

- URL: <https://github.com/DeusData/codebase-memory-mcp/pull/534>
- State: **OPEN** (not draft, not merged). Retrieved 2026-07-19.
- Author: Tensorboyalive (Manav Gupta). Created 2026-06-20, last updated
  2026-07-08.
- Labels: `enhancement`, `editor/integration`, `priority/backlog`.

`EXTERNALLY_REPORTED`: Adds Pi to the codebase-memory-mcp installer's
auto-detect and configure pipeline. Pi is wired differently from other agents
because it has no native MCP. Instead of an MCP server entry, the adapter
installs three things, all driven by the existing CLI mode
(`codebase-memory-mcp cli <tool> '<json>'`):

1. A Pi extension → `~/.pi/agent/extensions/cbmem.ts` — registers
   `cbm_search_graph`, `cbm_trace_path`, `cbm_get_architecture`,
   `cbm_semantic_query`, `cbm_detect_changes`, `cbm_query_graph`, `cbm_index`,
   `cbm_list_projects` as native Pi tools that shell out to the binary.
2. The `codebase-memory` skill → `~/.config/agents/skills/codebase-memory/`.
3. An AGENTS.md graph-first reminder, via the existing
   `cbm_upsert_instructions` sentinel upsert.

**Path handling**: `cbm_pi_config_dir` honors `$PI_CODING_AGENT_DIR`, falls
back to `~/.pi/agent` (mirrors how `cbm_claude_config_dir` honors
`$CLAUDE_CONFIG_DIR`).

**Validation** (author-reported): Clean build (`-Wall -Wextra -Werror`);
install → uninstall lifecycle verified against a fake HOME; memory handling
matches existing Codex/OpenCode patterns.

**Review state**: Maintainer DeusData (owner) acknowledged the PR but stated
"the maintainer shop is currently full, so this may sit for a bit before it
gets a proper review."

`HUMMBL_INTERPRETATION`: This PR is the clearest example of Pi's "no MCP, use
extensions and skills instead" philosophy being applied in practice. The
adapter shells out to a CLI binary from within a Pi extension, registering
native Pi tools. This pattern — extension as MCP-bridge — is likely to recur
for any tool that normally exposes MCP.

## Secondary source summaries

### addyosmani/agent-skills#355 (issue, OPEN)

- URL: <https://github.com/addyosmani/agent-skills/issues/355>
- State: **OPEN**. Retrieved 2026-07-19.
- Author: H3xKatana (0xkatana). Created 2026-07-06, last updated 2026-07-10.

`EXTERNALLY_REPORTED`: Requests a Pi setup guide, README section, and
AGENTS.md guidance. Notes that Pi discovers skills through convention
directories and loads them with the `read` tool (no dedicated skill tool).

### addyosmani/agent-skills#365 (PR, OPEN)

- URL: <https://github.com/addyosmani/agent-skills/pull/365>
- State: **OPEN** (not draft, not merged). Retrieved 2026-07-19.
- Author: H3xKatana (0xkatana). Created 2026-07-08, last updated 2026-07-10.
- Closes #355.

`EXTERNALLY_REPORTED`: Adds `docs/pi-setup.md` (setup guide with three install
methods), README Pi entry, and AGENTS.md instructions. Author reports Pi
v0.80.3 discovers all 24 skills from the cloned directory. Claims every
pi-specific claim verified against official pi docs. Built following the
repo's own skills (spec-driven-development, planning-and-task-breakdown,
source-driven-development, doubt-driven-development, humanizer).

### dustinblack/agent-dashboard#51 (issue, CLOSED)

- URL: <https://github.com/dustinblack/agent-dashboard/issues/51>
- State: **CLOSED** (resolved by #81). Retrieved 2026-07-19.
- Author: dustinblack (Dustin Black). Created 2026-04-30, closed 2026-07-13.

`EXTERNALLY_REPORTED`: Requests Pi as a built-in agent profile. References
`badlogic/pi-mono` and `@mariozechner/pi-coding-agent` (old identifiers).
Notes Pi has no MCP, no permission popups, config dir `~/.pi/agent`
(`PI_CODING_AGENT_DIR`). References `pi.dev/install.sh` and
`shittycodingagent.ai/` as alternate homepage.

`LOCAL_AUDIT_REQUIRED`: The `shittycodingagent.ai/` reference is unverified
and may be a joke or early branding. The current canonical homepage is
`pi.dev`.

### dustinblack/agent-dashboard#81 (PR, MERGED)

- URL: <https://github.com/dustinblack/agent-dashboard/pull/81>
- State: **MERGED**. Merged 2026-07-13. Retrieved 2026-07-19.
- Author: dustinblack (Dustin Black). Created 2026-06-22.
- Closes #51.

`EXTERNALLY_REPORTED`: **This is the only merged integration in the evidence
set.** Adds Pi as a bundled agent profile alongside Claude, Gemini, and Bash.
npm: `@earendil-works/pi-coding-agent` (current identifier). Resume via
`pi --continue` with fallback. No auth gate (provider-agnostic). No
permissions by design ("no popups" philosophy).

**Telemetry** (requires pi-otel extension): Pi uses the community
[pi-otel](https://www.npmjs.com/package/pi-otel) extension for OTLP telemetry.
It is not bundled in the container image — installed per-user via
`pi install npm:pi-otel` and persists in the `~/.pi` host volume mount. The
profile injects `OTEL_SERVICE_NAME`, `PI_OTEL_METRICS=1`, and
`OTEL_EXPORTER_OTLP_PROTOCOL=http/json` at spawn time.

**Known issue**: pi-otel v0.1.0 sends HTTP OTLP to `POST /` instead of
`/v1/{signal}` ([NikiforovAll/pi-otel#4](https://github.com/NikiforovAll/pi-otel/issues/4)).
The daemon has a root-path workaround. An upstream fix
([NikiforovAll/pi-otel#6](https://github.com/NikiforovAll/pi-otel/pull/6)) has
been submitted.

**Metric names** from pi-otel v0.1.0: `gen_ai.client.token.usage` (histogram),
`gen_ai.client.tool.calls` (counter), `gen_ai.client.operation.duration`
(histogram, seconds).

**Tests**: 70 total, 8 new.

`HUMMBL_INTERPRETATION`: This is the only evidence of Pi observability via
OpenTelemetry. The pi-otel extension pattern — community-built, per-user
installed, env-var configured — is the canonical observability surface for Pi.
The HTTP exporter path bug is a failure semantic worth tracking.

### msitarzewski/agency-agents#470 (issue, OPEN)

- URL: <https://github.com/msitarzewski/agency-agents/issues/470>
- State: **OPEN**. Retrieved 2026-07-19.
- Author: Lunatic83 (Gaspare Scherma). Created 2026-04-16, last updated
  2026-06-15.

`EXTERNALLY_REPORTED`: Simple request: "Would be great to have pi.dev
integration."

### msitarzewski/agency-agents#591 (PR, OPEN)

- URL: <https://github.com/msitarzewski/agency-agents/pull/591>
- State: **OPEN** (not draft, not merged). Retrieved 2026-07-19.
- Author: martian7777 (saqib). Created 2026-06-15.
- Fixes #470.

`EXTERNALLY_REPORTED`: Adds `convert_pi()` to generate Pi-compatible
`SKILL.md` files. Each agent is placed into its own skill directory with the
`agency-` prefix to prevent namespace collisions. Adds `pi` tool target to
`install.sh` with automatic detection (looks for `~/.pi`), interactive wizard
support, and bulk installation to `~/.pi/agent/skills/` (with `PI_SKILLS_DIR`
override support).

### himattm/prism#167 (PR, OPEN)

- URL: <https://github.com/himattm/prism/pull/167>
- State: **OPEN** (not draft, not merged). Retrieved 2026-07-19.
- Author: himattm (Matt McKenna). Created 2026-06-29, last updated 2026-06-30.

`EXTERNALLY_REPORTED`: Adds Pi coding agent support so Prism renders its
status line in Pi as well as Claude Code, from the same binary and config
files. Pi has no "run a command for the status line" hook like Claude Code —
status can only come from a TypeScript extension loaded into Pi's runtime.
Prism embeds a shim in the binary and installs it on demand.

**Implementation**: `prism install-pi` / `uninstall-pi` write the embedded
extension to `~/.pi/agent/extensions/prism/`. At runtime the extension
gathers Pi's session data (model, context usage, cwd, session id), runs
`prism` over stdin, and pushes output to Pi's footer via `ctx.ui.setStatus`.
Agent-aware rendering: Claude-only sections (`usage`, `cost`, `peakhours`)
are dropped for Pi; layout collapsed to a single line.

**Limitations** (author-reported): Needs a live Pi session to confirm. The Go
side, JSON contract, filtering, and rendering are verified; the in-Pi runtime
wiring is built from Pi's published extension API and a working community
extension. Event names and accessors should be smoke-tested against the
installed Pi version.

### fletchgqc/agentbox#71 (PR, OPEN)

- URL: <https://github.com/fletchgqc/agentbox/pull/71>
- State: **OPEN** (not draft, not merged). Retrieved 2026-07-19.
- Author: shrwnsan. Created 2026-06-09.

`EXTERNALLY_REPORTED`: Adds Pi coding agent as a built-in tool alongside
Claude and OpenCode. `--tool pi` CLI flag or `AGENTBOX_TOOL=pi` env var.
Mounts `~/.pi` directory for configuration persistence. Requires
`ANTHROPIC_API_KEY` in `.env` for authentication. Generated by Claude Code
with GLM 5.

`LOCAL_AUDIT_REQUIRED`: This PR requires `ANTHROPIC_API_KEY` specifically,
which contradicts Pi's multi-provider nature. This may be an agentbox
constraint rather than a Pi constraint.

### JuliusBrussee/caveman#161 (issue, OPEN)

- URL: <https://github.com/JuliusBrussee/caveman/issues/161>
- State: **OPEN**. Retrieved 2026-07-19.
- Author: vedang (Vedang Manerikar). Created 2026-04-14.

`EXTERNALLY_REPORTED`: Simple request: "support for a new agent harness: Pi."

### JuliusBrussee/caveman#551 (PR, OPEN)

- URL: <https://github.com/JuliusBrussee/caveman/pull/551>
- State: **OPEN** (not draft, not merged). Retrieved 2026-07-19.
- Author: morus246. Created 2026-06-22.

`EXTERNALLY_REPORTED`: Adds pi-coding-agent (earendil-works) to the provider
matrix. Installs Pi skills via symlink for local git checkouts, copy for
package/npx installs. Appends/removes fenced caveman AGENTS.md block safely.
8/8 Pi installer tests pass.

### shelbon/token-optimizer#1 (PR, OPEN)

- URL: <https://github.com/shelbon/token-optimizer/pull/1>
- State: **OPEN** (not draft, not merged). Retrieved 2026-07-19.
- Author: shelbon (Patrick Sheron Moucle). Created 2026-06-24, last updated
  2026-06-25.

`EXTERNALLY_REPORTED`: Adds native beta support for the Pi coding agent so
Token Optimizer can be installed via `pi install` and run locally with
`pi -e .` while reusing the existing Python optimization engine. Provides
strict Pi runtime detection and path isolation so all Pi data is stored under
the Pi agent directory and Token Optimizer never inspects or mutates
`~/.claude` when Pi is active.

**Implementation**: Pi extension in `pi/extension.ts` registers lifecycle
hooks, `read`/`bash` overrides, and commands (`/token-optimizer`,
`/token-status`, `/token-doctor`, `/token-dashboard`). TypeScript bridge
helper spawns the Python bridge safely (no shell, timeouts, bounded IO).
Python bridge implements a one-object JSON protocol. Pi session JSONL parsing
via `pi_session.py`.

**Testing** (author-reported): `pytest -q tests/test_pi_support.py` passed (6
passed). TypeScript check had non-fatal warnings.

### pingdotgg/t3code#397 (issue, CLOSED)

- URL: <https://github.com/pingdotgg/t3code/issues/397>
- State: **CLOSED**. Retrieved 2026-07-19.
- Author: gustavonline (Gustav). Created 2026-03-07, closed 2026-06-09.

`EXTERNALLY_REPORTED`: User request for Pi in the t3code GUI. References
`badlogic/pi-mono` and `@mariozechner` (old identifiers). Praises Pi's
minimalist philosophy.

### pingdotgg/t3code#402 (issue, OPEN)

- URL: <https://github.com/pingdotgg/t3code/issues/402>
- State: **OPEN**. Retrieved 2026-07-19.
- Author: IgorWarzocha (Howaboua). Created 2026-03-07, last updated
  2026-07-17.

`EXTERNALLY_REPORTED`: Meta-issue / specification for Pi as a first-class
provider in T3 Code. References a working implementation on the author's fork
([IgorWarzocha/t3code#1](https://github.com/IgorWarzocha/t3code/pull/1)) as
architecture guidance, not a direct merge candidate. Specifies RPC mode,
provider/orchestration model integration, and detailed pitfalls:

- Pi can emit intermediate tool/planning steps with no user-visible assistant
  text — those should not render as empty assistant messages.
- Pi runs can be multi-stage — do not assume the first assistant-side event is
  the final answer.
- Startup, prompt, model-switch, and child-process spawn failures must clean
  up correctly.
- Availability checks must respect the configured Pi binary path.
- Model discovery should be provider-scoped or on-demand.

`HUMMBL_INTERPRETATION`: This issue is the architectural specification that
PR #3818 attempts to implement. Its pitfall list is the most detailed
failure-mode catalog in the evidence set and should be treated as a reference
for any future Pi GUI/runtime integration.

## Claims

### Directly verified (by this packet's retrieval on 2026-07-19)

- `badlogic/pi-mono` redirects to `earendil-works/pi` on GitHub (confirmed via
  `gh repo view` and `gh api`).
- The canonical npm package is `@earendil-works/pi-coding-agent` (confirmed
  via pi.dev homepage).
- pi.dev describes Pi as having four modes: interactive, print/JSON, RPC, and
  SDK (confirmed via webfetch).
- pi.dev explicitly lists "No MCP," "No permission popups," "No sub-agents,"
  "No plan mode," "No built-in to-dos," "No background bash" as intentional
  omissions (confirmed via webfetch).
- PR/issue states as recorded in the source summaries above (confirmed via
  `gh pr view` / `gh issue view` on 2026-07-19).

### Author-reported (claimed by PR/issue authors, not independently verified)

- promptfoo#9707: 45 unit tests pass; live QA validated; head-to-head
  comparison results; 11 issues found and fixed in adversarial review.
- t3code#3818: `vp check`, `vp run typecheck`, `vp test` pass; live-validated
  against real `pi` install.
- SkillOpt#83: 54 passed, 1 skipped; end-to-end against real local pi
  sessions.
- rix#67: 201/201 tests passing; two live e2e bats cases confirmed passing.
- codebase-memory-mcp#534: Clean build; install/uninstall lifecycle verified.
- agent-skills#365: Pi v0.80.3 discovers all 24 skills.
- agent-dashboard#81: 70 tests total, 8 new; pi-otel v0.1.0 metric names.
- token-optimizer#1: 6 unit tests pass.
- caveman#551: 8/8 Pi installer tests pass.

### Inferred (derived from evidence, not stated directly)

- The exit-code-0 failure semantic is a Pi design choice, not a bug: Pi
  treats JSON mode as a stream protocol where process exit and agent failure
  are decoupled. Both promptfoo and rix independently discovered and
  documented this, suggesting it is a stable property of the JSON event
  stream interface.
- The `agent_settled` final-line pattern (reported by rix) may affect any
  consumer that reads only the last stdout line rather than parsing the full
  event stream.
- The extension-as-MCP-bridge pattern (codebase-memory-mcp#534) is likely to
  recur for any tool that normally exposes MCP, given Pi's explicit "no MCP"
  stance.
- The identifier migration is complete at the first-party level but
  incomplete in third-party references, creating a period of dual-identifier
  citation that will complicate search and dependency resolution.

### Unverified

- The `shittycodingagent.ai/` homepage referenced in agent-dashboard#51 is
  not verified. It may be early branding, a joke, or a dead link.
- The closure reason for t3code#3818 is not documented in the retrieved data.
- Whether any open PR has been silently superseded by a different
  implementation in the same repo.
- Whether the `pi-otel` extension is officially endorsed by Earendil Inc. or
  is purely community-maintained.

## Risk and failure semantics

### Exit code 0 with failure in JSON events

`EXTERNALLY_REPORTED` (promptfoo#9707, rix#67): Pi exits 0 in JSON mode even
when the agent run fails. This is the most significant recurring failure
semantic in the evidence set.

- promptfoo mitigates by reading `stopReason` / `errorMessage` from the final
  assistant message. Nonzero/signal exits are never reported or cached as
  success.
- rix reports that an invalid API key causes Pi to swallow the error into its
  JSON stream and exit 0, producing a false "success with no PRs" result.
- rix also reports that missing credentials produce a `.md` doc path, not
  JSON — Pi has no structured error protocol for this path, unlike
  claude/opencode.

`HUMMBL_INTERPRETATION`: Any consumer of Pi's JSON event stream must
implement its own failure detection by parsing event payloads, not by
checking exit codes. This is a design contract, not a bug, but it is a
footgun for integrators who assume exit code reflects agent success.

### Cost reporting limitation

`EXTERNALLY_REPORTED` (rix#67): Pi's true last stdout line is always a
payload-free `agent_settled` event, so any consumer that reads only the last
line will reliably report `$0` cost. This is a stronger version of a
limitation that `OpenCodeAgent` already documents for itself.

### Exception swallowing

`EXTERNALLY_REPORTED` (SkillOpt#83, reviewer comment): `PiCliBackend._call()`
swallows all launch/timeout exceptions and ignores non-zero exit codes, so a
missing `pi` binary returns `""` — indistinguishable from an empty model
answer. The sibling `CodexCliBackend` records `last_call_error` and logs a
warning.

### Telemetry exporter path bug

`EXTERNALLY_REPORTED` (agent-dashboard#81): pi-otel v0.1.0 sends HTTP OTLP
to `POST /` instead of `/v1/{signal}`. A daemon root-path workaround exists.
An upstream fix has been submitted but not yet released.

### Safety boundary gaps

`SOURCE_CLAIM` (pi.dev): Pi intentionally ships without permission prompts,
sandboxing, or MCP configuration. Every adopter that touches tool execution
must add its own safety boundary. The evidence shows four distinct patterns:

1. **Default-deny extension gate** (t3code#3818): bundled Pi extension
   injected via `--extension`, verified with `get_commands` load handshake
   (fail-closed). Bypass via `T3_PI_APPROVAL_MODE=auto-accept-edits`.
2. **Read-only tool defaults with opt-in** (promptfoo#9707): no `working_dir`
   → temp dir with all tools disabled; with `working_dir` → read-only tools;
   `bash`/`edit`/`write` explicit opt-in.
3. **Path isolation and runtime detection** (token-optimizer#1): strict Pi
   runtime detection, path isolation, never inspects `~/.claude` when Pi is
   active.
4. **Container mounting with no auth gate** (agent-dashboard#81): no
   permissions by design, no auth gate (provider-agnostic), `~/.pi` host
   volume mount.

`HUMMBL_INTERPRETATION`: The absence of built-in safety boundaries is a
consistent theme. Adopters compensate, but the compensation is non-uniform.
There is no standard Pi permission-gate extension; each integrator builds
their own. This creates a risk of inconsistent safety postures across Pi
deployments.

### Credential handling

`EXTERNALLY_REPORTED` (promptfoo#9707): Credentials must be injected via
environment variables, never argv, to avoid leaking into `ps` output, debug
logs, and persisted cache keys. promptfoo's adversarial review caught and
fixed a P1 issue where an `--api-key` argv fallback would have leaked the raw
key.

`LOCAL_AUDIT_REQUIRED`: Any Pi integration that passes API keys via
command-line arguments should be audited for credential leakage.

## Integration state summary

<!-- markdownlint-disable MD013 -->

| Source | Type | State | Maturity |
| --- | --- | --- | --- |
| promptfoo/promptfoo#9707 | PR | OPEN | Open, not merged, well-tested |
| pingdotgg/t3code#3818 | PR | CLOSED | Closed without merge, consolidated prior attempts |
| pingdotgg/t3code#397 | Issue | CLOSED | Closed, superseded by #402/#3818 |
| pingdotgg/t3code#402 | Issue | OPEN | Open, architectural spec for Pi provider |
| microsoft/SkillOpt#83 | PR | OPEN (draft) | Draft, reviewer-tested, needs fixes |
| Tim-Pohlmann/rix#67 | PR | OPEN | Open, not merged, tests pass |
| DeusData/codebase-memory-mcp#534 | PR | OPEN | Open, acknowledged by maintainer, backlog |
| addyosmani/agent-skills#355 | Issue | OPEN | Open, addressed by #365 |
| addyosmani/agent-skills#365 | PR | OPEN | Open, not merged |
| dustinblack/agent-dashboard#51 | Issue | CLOSED | Closed, resolved by #81 |
| dustinblack/agent-dashboard#81 | PR | MERGED | **Merged** — only merged integration |
| msitarzewski/agency-agents#470 | Issue | OPEN | Open, addressed by #591 |
| msitarzewski/agency-agents#591 | PR | OPEN | Open, not merged |
| himattm/prism#167 | PR | OPEN | Open, not merged, needs live Pi smoke test |
| fletchgqc/agentbox#71 | PR | OPEN | Open, not merged, minimal |
| JuliusBrussee/caveman#161 | Issue | OPEN | Open, addressed by #551 |
| JuliusBrussee/caveman#551 | PR | OPEN | Open, not merged |
| shelbon/token-optimizer#1 | PR | OPEN | Open, not merged |

<!-- markdownlint-enable MD013 -->

**Summary**: 1 merged, 2 closed (without merge), 1 closed (resolved), 14 open.
No primary source is merged. The only merged integration is a secondary source
(agent-dashboard#81, a packaging/observability profile).

<!-- markdownlint-disable MD029 -->

## sources.md

All sources retrieved on 2026-07-19 unless otherwise noted.

### First-party Pi sources

1. Pi homepage — <https://pi.dev> (accessed 2026-07-19)
2. Pi documentation — <https://pi.dev/docs/latest>
3. Pi GitHub repository (canonical) — <https://github.com/earendil-works/pi>
4. Pi coding agent package — <https://github.com/earendil-works/pi/tree/main/packages/coding-agent>
5. Pi npm package — <https://www.npmjs.com/package/@earendil-works/pi-coding-agent>
6. Pi RPC documentation — <https://github.com/earendil-works/pi/tree/main/packages/coding-agent/docs/rpc.md>
7. Pi blog post (Mario Zechner) — <https://mariozechner.at/posts/2025-11-30-pi-coding-agent/>
8. Pi "what if you don't need MCP" post — <https://mariozechner.at/posts/2025-11-02-what-if-you-dont-need-mcp/>
9. Earendil Inc. — <https://earendil.com>

### Primary GitHub sources

10. promptfoo/promptfoo#9707 — <https://github.com/promptfoo/promptfoo/pull/9707>
11. pingdotgg/t3code#3818 — <https://github.com/pingdotgg/t3code/pull/3818>
12. microsoft/SkillOpt#83 — <https://github.com/microsoft/SkillOpt/pull/83>
13. Tim-Pohlmann/rix#67 — <https://github.com/Tim-Pohlmann/rix/pull/67>
14. DeusData/codebase-memory-mcp#534 — <https://github.com/DeusData/codebase-memory-mcp/pull/534>

### Secondary GitHub sources

15. addyosmani/agent-skills#355 — <https://github.com/addyosmani/agent-skills/issues/355>
16. addyosmani/agent-skills#365 — <https://github.com/addyosmani/agent-skills/pull/365>
17. dustinblack/agent-dashboard#51 — <https://github.com/dustinblack/agent-dashboard/issues/51>
18. dustinblack/agent-dashboard#81 — <https://github.com/dustinblack/agent-dashboard/pull/81>
19. msitarzewski/agency-agents#470 — <https://github.com/msitarzewski/agency-agents/issues/470>
20. msitarzewski/agency-agents#591 — <https://github.com/msitarzewski/agency-agents/pull/591>
21. himattm/prism#167 — <https://github.com/himattm/prism/pull/167>
22. fletchgqc/agentbox#71 — <https://github.com/fletchgqc/agentbox/pull/71>
23. JuliusBrussee/caveman#161 — <https://github.com/JuliusBrussee/caveman/issues/161>
24. JuliusBrussee/caveman#551 — <https://github.com/JuliusBrussee/caveman/pull/551>
25. shelbon/token-optimizer#1 — <https://github.com/shelbon/token-optimizer/pull/1>
26. pingdotgg/t3code#397 — <https://github.com/pingdotgg/t3code/issues/397>
27. pingdotgg/t3code#402 — <https://github.com/pingdotgg/t3code/issues/402>

### Referenced upstream sources

28. NikiforovAll/pi-otel#4 — <https://github.com/NikiforovAll/pi-otel/issues/4>
29. NikiforovAll/pi-otel#6 — <https://github.com/NikiforovAll/pi-otel/pull/6>
30. IgorWarzocha/t3code#1 (reference fork) — <https://github.com/IgorWarzocha/t3code/pull/1>
31. OpenClaw (real-world Pi integration) — <https://github.com/OpenClaw/OpenClaw>

<!-- markdownlint-enable MD029 -->

## Candidate non-canon framing

- Candidate frame only: Pi is gaining multi-lane adoption as a provider,
  runtime, evaluator target, transcript source, skills target, packaging
  target, and observability surface — but the evidence is dominated by open,
  unmerged contributions.
- Candidate frame only: Pi's "primitives, not features" philosophy creates a
  consistent adopter burden: every integrator must build its own safety
  boundary, failure detection, and (if needed) MCP bridge.
- Candidate frame only: The exit-code-0 failure semantic is a stable property
  of Pi's JSON event stream that integrators must handle explicitly.

## Follow-up issues (recommended only)

1. Dedicated teardown: promptfoo/promptfoo#9707 — Pi as eval provider
2. Dedicated teardown: pingdotgg/t3code#3818 — Pi as GUI/runtime provider
3. Pi permission-gate extension survey — catalog all community-built safety
   boundaries and assess whether a standard pattern is emerging
4. Pi failure-semantics contract — document exit-code-0, `agent_settled`
   final-line, and missing-credential `.md` diagnostic as a formal consumer
   contract
5. Pi observability surface survey — assess pi-otel maturity and the
   extension-based telemetry pattern
6. Candidate bounded experiment: run the Pi CLI on Orange Pi hardware
   (naming symmetry is delightful, but hardware selection must remain
   workload-, architecture-, OS-, memory-, thermal-, and software-support-
   driven rather than name-driven)

## No integration decision declared

`DO_NOT_ADOPT_YET`: No HUMMBL / BaseN / Ownward integration decision is
declared by this packet. Pi is recorded as a candidate observation target.
No Pi component, extension, skill, or interface is adopted as a HUMMBL
dependency, standard, or canon. All claims are preserved with their source
posture labels and must be independently verified before any adoption.

## Receipt reference

Receipt: `receipts/pi-cli-adoption-evidence-2026-07-19-receipt.md`

## Refresh trigger

Recheck this packet when:

- Any primary source changes state (merged, closed, reopened, superseded).
- Pi releases a new version that changes the JSON event stream, RPC protocol,
  session JSONL schema, or extension API.
- Pi adds built-in permission prompts, MCP support, or sandboxing (which would
  invalidate the safety-boundary-gap findings).
- The identifier migration completes in all third-party references.
- A new adoption lane emerges that is not covered by the current taxonomy.
