# OpenAI Codex Reasoning Levels: Operational Definitions

Status: candidate research note. This document records current product wording
and an evidence-bounded interpretation; it does not create HUMMBL canon.

Date: July 13, 2026.

Review status: revised after independent ChatGPT review on July 13, 2026.

## Research question

What does OpenAI mean when Codex describes reasoning levels as Low, Medium,
High, Extra High, Max, and Ultra?

## Short answer

A reasoning level controls how much internal computational effort the selected
model may spend analyzing, planning, exploring alternatives, checking
assumptions, and verifying work before responding.

Low through Max do not select a different base model. They are relative effort
controls for the selected model. Higher settings generally trade more response
time and usage for a better chance of solving difficult problems well. They do
not guarantee correctness or prescribe a fixed amount of time or tokens.

The progression is:

`Low -> Medium -> High -> Extra High -> Max`

These levels progressively increase reasoning effort applied to the selected
model. They do not proactively delegate merely because of the level, although
the user can explicitly request subagents at non-Ultra levels. `Ultra` is
structurally different: it combines maximum reasoning with permission to
delegate suitable work proactively.

## Observed Codex selector context

The operator supplied a Codex CLI transcript from July 13, 2026. The installed
CLI version was independently verified as `codex-cli 0.144.3` on Anvil.

The observed model selector listed:

- `gpt-5.6-sol`: "Latest frontier agentic coding model."
- `gpt-5.6-terra`: "Balanced agentic coding model for everyday work."
- `gpt-5.6-luna`: "Fast and affordable agentic coding model."
- `gpt-5.5`: "Frontier model for complex coding, research, and real-world
  work."
- `gpt-5.4`: "Strong model for everyday coding."
- `gpt-5.4-mini`: "Small, fast, and cost-efficient model for simpler coding
  tasks."
- `gpt-5.3-codex-spark`: "Ultra-fast coding model."

For `gpt-5.6-sol`, the observed reasoning selector listed:

- Low: "Fast responses with lighter reasoning."
- Medium: "Balances speed and reasoning depth for everyday tasks."
- High: "Greater reasoning depth for complex problems."
- Extra High: "Extra high reasoning depth for complex problems."
- Max: "For difficult problems when quality matters more than speed; higher
  usage."
- Ultra: "For demanding work using multiple agents; highest usage."

This transcript is an observation of the product surface, not a specification
of numerical compute, latency, or token limits.

## Surface terminology

OpenAI uses related but non-identical terminology across surfaces:

- The Codex CLI uses **Low**, while the Codex app, ChatGPT Work, and IDE
  extension may label the corresponding setting **Light**.
- The app and CLI expose product-facing levels such as Medium, High, Extra
  High, Max, and Ultra when supported by the selected model and account.
- API `reasoning.effort` values are model-dependent. They can include `none`,
  `minimal`, `low`, `medium`, `high`, and `xhigh`; GPT-5.6 also supports `max`.
- **Extra High** is the product-facing label associated with `xhigh`.
- **Ultra** is a Codex or ChatGPT orchestration mode, not merely another
  ordinary API reasoning-effort value.

Medium was the observed default for `gpt-5.6-sol` in the supplied selector.
Defaults remain model-dependent and should not be generalized universally.

## Definition of reasoning effort

OpenAI documents `reasoning.effort` as guidance to the model about how much to
think while performing a task. Lower effort favors speed and lower token use;
higher effort allows more complete reasoning and can improve response quality.
Reasoning remains adaptive: a model may use less effort for a simple task and
more for a complex one, even at the same configured level.

Supported values and defaults are model-dependent. The labels therefore
describe relative operating points, not universal or directly comparable
quantities across every model generation.

Reasoning effort should not be confused with:

- **Model choice:** Sol, Terra, Luna, and other models have different baseline
  capability, speed, and cost profiles.
- **Response length:** higher reasoning effort does not necessarily produce a
  longer visible answer. GPT-5.6 exposes response detail separately through
  `text.verbosity` in the API.
- **Context capacity:** changing effort does not itself expand the model's
  context window.
- **Permissions or tools:** effort does not grant filesystem, network, or
  connector access.
- **Correctness:** more reasoning can improve difficult-task performance, but
  no level guarantees a correct result.

## Operational definition by level

### Low

"Fast responses with lighter reasoning" means Codex prioritizes latency and
efficiency over extensive deliberation. It can still plan and use tools, but it
is less strongly encouraged to explore alternatives or perform deep checking.

Use Low for clear, well-scoped tasks, routine transformations, simple edits,
fast information retrieval, or execution where the desired result is already
well specified.

### Medium

"Balances speed and reasoning depth" means Codex receives a moderate reasoning
allowance intended to sit near the practical balance among latency, quality,
reliability, and usage. OpenAI presents Medium as the normal starting point for
most agentic work.

Use Medium for everyday coding, research, tool use, planning, document work,
and tasks that require judgment but are not unusually difficult.

### High

"Greater reasoning depth" means Codex is encouraged to spend more effort
tracing complex logic, checking assumptions, considering edge cases, and
planning or validating multiple steps. Quality is prioritized more heavily
than response speed.

Use High for complex debugging, architecture, consequential review,
long-horizon research, ambiguous multi-step implementation, or work where
missed edge cases are expensive.

### Extra High (`xhigh`)

"Extra high reasoning depth" means a larger reasoning-effort setting for
especially challenging or long-running tasks. OpenAI associates `xhigh` with
deep research, asynchronous agentic work, security and code review, and
challenging coding workflows.

Extra High does not proactively delegate merely because of the level. It also
does not prohibit subagents: the user can explicitly request them at non-Ultra
levels when the product supports subagents.

Use Extra High when High is insufficient and the expected improvement justifies
additional latency and usage. It should not be the automatic default for
ordinary work.

### Max

"Quality matters more than speed" means the selected model receives still more
time to reason about one task. OpenAI describes Max as appropriate for the
hardest problems, particularly when additional exploration and verification
matter more than latency or usage.

Max is defined by additional reasoning time for the selected model, not by an
exclusive ban on subagents. Use it for difficult integrated problems where
deep analysis is more important than a fast response. Explicit subagent use
can still be requested separately when supported.

### Ultra

"Demanding work using multiple agents" means more than increasing the primary
model's reasoning effort. Ultra uses maximum reasoning and permits Codex to
delegate suitable parts of the task to subagents, run those workstreams in
parallel, and synthesize their results proactively.

Ultra does not promise that every request will be delegated. Delegation depends
on whether Codex identifies suitable work that benefits from decomposition.

Use Ultra for large problems that divide into meaningful independent parts,
such as parallel repository reviews, multiple research lanes, or separable
implementation and verification work. Ultra is usually wasteful for small,
sequential, tightly coupled, or indivisible tasks.

## Max versus Ultra

| Question | Max | Ultra |
| --- | --- | --- |
| Mechanism | More time on one task | Maximum reasoning plus subagents |
| Best task | Difficult and integrated | Difficult and decomposable |
| Delegation | Explicitly requestable | Proactive when suitable |
| Tradeoff | More latency and usage | Highest usage; parallel work |
| Example | One diagnosis or proof | Independent investigations |

Ultra should not be interpreted as simply "one notch smarter than Max." It
permits a different execution topology through proactive multi-agent work.

## How to interpret the selector phrases

- **"Fast responses":** the setting favors lower latency; it is not a
  response-time guarantee.
- **"Lighter reasoning":** less internal deliberation relative to higher
  settings; not zero reasoning.
- **"Balances speed and reasoning depth":** a general-purpose tradeoff rather
  than a numerical midpoint.
- **"Greater reasoning depth":** more opportunity for planning, alternatives,
  checks, and verification.
- **"Quality matters more than speed":** accept greater latency and usage when
  difficult-task reliability is the priority.
- **"Using multiple agents":** Codex may decompose the task and delegate
  parallel work to subagents.
- **"Consumes usage limits faster":** higher-effort and multi-agent runs can
  consume more of the account's available usage.

## Selection guidance

1. Start with Medium for ordinary Codex work.
2. Choose Low when the task is narrow and latency matters.
3. Choose High for complex reasoning, debugging, planning, or review.
4. Choose Extra High when representative difficult tasks show a material gain
   over High.
5. Choose Max for an exceptionally difficult integrated problem where depth is
   worth the added time and usage.
6. Choose Ultra only when the task can be split into useful parallel
   workstreams and synthesis will add value.

OpenAI recommends using the lowest effort that reliably produces the needed
result. The appropriate setting should be evaluated on representative work
rather than inferred from the label alone.

## Evidence and limitations

### Directly supported by OpenAI documentation

- Reasoning effort guides how much the model thinks.
- Lower effort favors speed and lower token usage.
- Higher effort permits more complete reasoning and can improve quality.
- Reasoning is adaptive rather than a fixed token allocation.
- Effort support and defaults depend on the selected model.
- Max gives the selected model more time to reason about one task.
- Non-Ultra levels can use explicitly requested subagents when supported.
- Ultra permits proactive subagent use for suitable decomposable work.

### Interpretation rather than published guarantee

- The levels form useful relative operating points, but OpenAI does not publish
  a fixed multiplier between adjacent levels.
- No exact latency, token, quality, or correctness guarantee follows from a
  label.
- The same label should not be assumed to provide identical behavior across
  model families or future releases.
- Product wording, availability, entitlements, and usage accounting can change.

## Sources

All online sources below are first-party OpenAI documentation accessed on
July 13, 2026.

1. OpenAI, "Models" (Codex) - model selection, effort guidance, Max, and Ultra.
   <https://learn.chatgpt.com/docs/models>
2. OpenAI, "Reasoning models: Reasoning effort" - definition, adaptive
   reasoning, tradeoffs, and workload guidance for Low through `xhigh`.
   <https://developers.openai.com/api/docs/guides/reasoning#reasoning-effort>
3. OpenAI, "Subagents: Choosing models and reasoning" - effort selection and
   Ultra's proactive subagent delegation.
   <https://learn.chatgpt.com/docs/agent-configuration/subagents#choosing-models-and-reasoning>
4. OpenAI, "Model guidance: What is new" - GPT-5.6 Max reasoning and the
   relationship between multi-agent operation and Codex Ultra.
   <https://developers.openai.com/api/docs/guides/latest-model#what-is-new>
5. OpenAI, "Control verbosity from reasoning effort" - separate
   `text.verbosity` and `reasoning.effort` controls.
   <https://developers.openai.com/cookbook/examples/partners/aws/openai_models_with_amazon_bedrock#34-control-verbosity-from-reasoning-effort>
6. Operator-provided Codex CLI selector transcript, July 13, 2026 - exact local
   selector wording recorded above.
7. Local verification on Anvil, July 13, 2026 - `codex --version` returned
   `codex-cli 0.144.3`.

## Refresh trigger

Recheck this note against current first-party documentation when Codex changes
its model picker, reasoning labels, Max or Ultra behavior, account eligibility,
or usage-limit presentation.
