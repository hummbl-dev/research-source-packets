# Provider-Neutral Model and Reasoning Self-Research Playbook

Status: candidate companion artifact. This playbook is provider-neutral,
exploratory, and non-canon.

Date: July 13, 2026.

Companion example:
[OpenAI Codex Reasoning Levels: Operational Definitions](openai-codex-reasoning-levels-2026-07-13.md)

## Purpose

This playbook teaches an AI agent to research and explain the model,
reasoning, intelligence, thinking, or effort controls exposed by its own
provider and product surface.

It is designed for agents such as Claude, Gemini, Copilot, Devin, OpenCode,
local-model assistants, and future systems whose terminology may differ from
OpenAI Codex.

The method produces an evidence-bounded research note that answers:

- What exact choices does the product expose?
- What does the provider claim each choice changes?
- What tradeoffs does the provider associate with each choice?
- Are the choices a simple scale, different models, or different execution
  architectures?
- What is directly documented, locally observed, inferred, or still unknown?

## Core boundary

An agent researching "itself" does not have privileged introspective access to
its hidden reasoning process, training implementation, or internal compute
allocation.

The agent may inspect and report:

- Its exposed model identifier and product version.
- Selector labels and descriptions visible in the user interface or CLI.
- Configuration fields, supported values, and documented defaults.
- First-party provider documentation.
- Observable behavior from bounded tests, when clearly labeled as an
  experiment rather than a product guarantee.

The agent must not treat its own generated explanation, hidden
chain-of-thought, or unaudited model memory as authoritative evidence about the
provider's implementation.

## Required evidence classes

Every material claim should be assigned one of these classes.

### Observed

Directly captured from the current product surface, such as a model picker,
CLI help screen, settings page, status command, or configuration schema.

Record the product, surface, version, date, account context when relevant, and
the exact visible wording. Observation establishes what the interface says,
not whether the description is complete.

### Documented

Supported by current first-party provider documentation, API reference,
product manual, model card, release note, or official support article.

Record the page title, direct URL, relevant section, access date, and the
smallest accurate paraphrase of the claim.

### Experimentally observed

Produced by a bounded, reproducible comparison run. Record the prompt, model,
settings, environment, number of trials, outputs or metrics, and limitations.

A small experiment can show observed behavior in that test. It cannot prove a
universal implementation detail or service-level guarantee.

### Inferred

A conclusion derived from observations and documentation but not stated
directly by the provider. Explain the reasoning and keep the wording bounded.

Use phrases such as "this suggests," "operationally," or "the most supported
interpretation is."

### Unknown

Not established by available first-party evidence or reproducible tests.
Preserve the unknown rather than filling it with plausible speculation.

## Research workflow

### 1. Establish identity and scope

Record:

- Provider and product name.
- Product surface, such as CLI, IDE, desktop app, web app, or API.
- Installed product version or observed build date.
- Active model identifier, if exposed.
- Current reasoning or thinking setting, if exposed.
- Research date and timezone.

Do not assume that the product brand, model family, agent wrapper, and API
model identifier are the same thing.

### 2. Capture the exact selector

Open the model or reasoning selector without changing the active setting unless
the operator authorizes a change. Transcribe or capture:

- Every visible model name.
- Every visible level or mode.
- Default and current markers.
- Concise descriptions, warnings, eligibility notes, and usage notices.
- Nested or advanced menus.

If the agent cannot access its interactive selector, ask the operator to paste
the selector text or provide a screenshot. Treat operator-supplied text as an
observation with stated provenance.

### 3. Inventory adjacent controls

Determine whether the product exposes separate controls for:

- Model choice.
- Reasoning, thinking, intelligence, or effort.
- Visible response verbosity.
- Context length or memory.
- Speed, service, or priority tier.
- Tool access and permissions.
- Search, browsing, or connector access.
- Agent count, delegation, or parallel execution.
- Planning mode or autonomy.

Do not collapse these controls into one concept. A mode called "Advanced" or
"Deep" may alter orchestration rather than only increasing reasoning effort.

### 4. Search first-party sources

Search the provider's official documentation using the exact UI terms first.
Prefer this source order:

1. Current product manual or official product documentation.
2. Official API reference or configuration reference.
3. Official model page or model card.
4. Official release notes or support documentation.
5. Provider-authored technical paper or system card.

Use third-party sources only to identify questions or leads. Do not use them as
authority for what the provider means when a first-party source is available.

If first-party sources disagree, record the conflict, page dates, product
surfaces, and likely scope difference. Do not silently choose the more
convenient wording.

### 5. Map UI labels to documented controls

For each visible label, determine:

- Its configuration or API value, if documented.
- Whether it is supported by every model or only a subset.
- Whether the default is model-dependent, product-dependent, or universal.
- Whether it changes a scalar effort budget or selects a different mode.
- Whether it enables tools, agents, parallelism, or other architecture.
- Whether a product-facing mode maps to an ordinary API parameter value.
- The provider-stated latency, usage, cost, and quality tradeoffs.
- Recommended task types and any provider warnings.

Do not infer a numerical mapping from ordinal names such as Low, High, Max,
Fast, Deep, or Pro unless the provider publishes that mapping.

### 6. Define what each phrase means

Translate marketing or selector language into bounded operational terms.

For example:

- "Fast" normally indicates a latency preference, not a response-time
  guarantee.
- "Deeper" may indicate more deliberation, but not a documented multiplier.
- "Better quality" means an intended tradeoff, not guaranteed correctness.
- "More thinking" does not necessarily mean a longer visible response.
- "Multi-agent" indicates a different execution topology, not merely a larger
  single-agent budget.

Every translation must be traceable to documented language or labeled as an
inference.

### 7. Identify non-scalar modes

Test whether the choices form one ordered scale. Look specifically for modes
that introduce:

- Subagents or parallel workers.
- Proactive, automatic, permitted, or explicitly requested delegation.
- External search or research pipelines.
- Tool-use policies.
- Multiple candidate generations or internal verification.
- Longer-running asynchronous execution.
- Different models, routing, or service tiers.

Describe these separately from scalar reasoning levels. Do not place a
multi-agent mode at the top of a single-agent scale without qualification. A
mode that permits proactive delegation does not necessarily delegate every
request, and lower modes may still allow explicitly requested subagents.

### 8. State what the control does not change

Check and document whether the setting changes any of the following:

- Base model.
- Knowledge or training cutoff.
- Context capacity.
- Visible verbosity.
- Permissions.
- Tools and connectors.
- Data-handling policy.
- Correctness guarantees.

If the provider does not document the boundary, mark it Unknown.

### 9. Write the research artifact

Use the output contract below. Preserve exact selector wording separately from
interpretation, and place citations next to the claims they support.

### 10. Verify and refresh

Before closeout:

- Reopen every cited first-party page.
- Confirm that each citation supports the adjacent claim.
- Check that observations include version and date context.
- Search for unsupported absolutes such as "always," "guarantees," "exactly,"
  or "twice as much."
- Run the repository's Markdown or document validation.
- Record a refresh trigger for product, model, or selector changes.

## Reusable agent prompt

The operator can give the following prompt to another provider's agent.

```text
Research and document what your provider and current product surface mean by
every model, reasoning, intelligence, thinking, effort, or advanced mode shown
in your model selector.

First, report your provider, product surface, installed or visible version,
active model, and current setting. Capture the exact selector labels,
descriptions, defaults, warnings, nested menus, and usage notices. Do not change
the active setting unless I authorize it. If you cannot inspect the selector,
ask me to paste its text or attach a screenshot.

Then verify the terminology against current first-party provider documentation.
Prefer the product manual, API or configuration reference, official model page,
release notes, and system or model cards. Do not use your own model memory as
authority. Do not expose or claim access to hidden chain-of-thought.

Separate every material claim into Observed, Documented, Experimentally
observed, Inferred, or Unknown. Distinguish model choice from reasoning effort,
response verbosity, context, permissions, tools, service tier, planning mode,
and multi-agent orchestration. Identify any mode that changes execution
architecture rather than merely increasing a scalar effort level. Distinguish
proactive delegation from explicitly requested delegation, and do not assume a
mode will delegate every request.

For each option, define what the provider's description means operationally,
when to use it, its documented tradeoffs, and what it does not guarantee. Do
not invent fixed token, time, cost, quality, or capability multipliers.

Produce a dated, provider-neutral research artifact using this structure:
Research question; short answer; observed selector; control map; definition by
level or mode; exceptional modes; phrase decoder; selection guidance; evidence
and limitations; first-party sources with access dates; refresh trigger.
```

## Output contract

The resulting artifact should contain these sections.

### Header

- Title.
- Candidate or adopted status.
- Research date.
- Provider, product, surface, and version.

### Research question

State the exact terminology being investigated.

### Short answer

Explain the control in plain language and summarize the major tradeoff without
claiming undocumented precision.

### Observed selector

Record exact product wording, current and default markers, and observation
provenance.

### Control map

Separate model, reasoning, verbosity, context, permissions, tools, service
tier, planning, and orchestration controls.

### Definition by level or mode

For every option, include:

- Exact label and provider description.
- Documented mechanism or intended effect.
- Appropriate task shapes.
- Latency, usage, cost, or quality tradeoffs.
- Explicit non-guarantees.
- Evidence class and source.

### Exceptional modes

Explain modes that change model routing, tool access, delegation, parallelism,
or execution architecture.

### Evidence and limitations

Separate documented facts, local observations, experiment results, inferences,
conflicts, and unknowns.

### Sources

Use direct first-party links with page titles, relevant sections, and access
dates. Include local version or selector evidence without exposing credentials
or private account data.

### Refresh trigger

Name the product changes that require the artifact to be revalidated.

## Self-review questions

Before presenting the result, the researching agent should answer:

1. Did I verify my actual product surface and version?
2. Did I preserve the exact selector wording separately from interpretation?
3. Does every material provider claim have a first-party source?
4. Did I distinguish model selection from reasoning and orchestration?
5. Did I distinguish proactive delegation from explicitly requested agents?
6. Did I mistake a marketing adjective for a numerical specification?
7. Did I claim access to hidden reasoning or implementation details?
8. Did I label experiments and inferences rather than presenting them as
   provider facts?
9. Did I preserve contradictions and unknowns?
10. Did I avoid exposing account data, credentials, or private configuration?
11. Did I record when this artifact must be refreshed?

Any "no" answer is a blocker to an authoritative-sounding closeout.

## Common failure modes

- **Self-memory as authority:** the agent answers from training memory instead
  of current first-party sources.
- **Surface collapse:** model, reasoning, verbosity, permissions, and agent
  topology are described as one intelligence scale.
- **Ordinal overclaim:** High or Max is assigned an invented token or quality
  multiplier.
- **Architecture blindness:** a multi-agent or research mode is treated as only
  more single-agent thinking.
- **Orchestration exclusivity:** proactive delegation at one level is
  misreported as proof that lower levels cannot use explicitly requested
  agents.
- **Version blindness:** selector wording is recorded without product version
  or observation date.
- **UI-as-specification:** concise interface copy is treated as a complete
  technical contract.
- **Experiment overreach:** one prompt comparison is generalized to every task.
- **Source laundering:** unofficial commentary is cited as the provider's
  definition.
- **Chain-of-thought claim:** the agent presents hidden reasoning as inspectable
  evidence.
- **Stale permanence:** a time-sensitive product description is written as
  timeless fact.

## Minimum completion criteria

The companion research is complete only when:

- The selector or equivalent configuration surface is captured.
- The product and version context is recorded.
- Current first-party sources have been checked.
- Each label has a bounded operational definition.
- Scalar effort and architectural modes are separated.
- Claims are classified by evidence type.
- Unknowns and conflicts remain visible.
- The artifact contains sources, access dates, and a refresh trigger.
- Repository validation passes.
