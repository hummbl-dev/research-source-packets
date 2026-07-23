---
title: Handoff — AI Competitions Research & Entry Planning
date: 2026-07-23
session_origin: opencode CLI, model MiniMax-M3
parent_path: C:\Users\Owner\PROJECTS\_research\
companion_artifact: ai-competitions-2026-07-23.md
intended_handoff_to: ChatGPT (or any agent / human) continuing this work
governing: C:\Users\Owner\PROJECTS\AGENTS.md -> C:\Users\Owner\PROJECTS\CONSTITUTION.md
---

# Handoff — AI Competitions Research Continuation

## Mission
Pick up the AI-competitions research begun 2026-07-23 (snapshot artifact at `ai-competitions-2026-07-23.md`) and either (a) refresh + extend the survey, or (b) drive planning toward actual entry into one or more competitions.

## Current state
- Surveyed 11 platforms/sources via webfetch: arcprize.org, aicrowd.com, platform.k12-ai-infrastructure.org, drivendata.org, devpost.com (10 hackathons detailed), zindi.africa, huggingface.co, kaggle.com (via DDG search), DuckDuckGo searches for KDD Cup 2026, AIMO, ICML 2026, AIxCC, AI safety.
- Compiled ranked listing of ~25 competitions with deadlines, prize breakdowns, eligibility, judges, and a calendar view.
- Identified 8 categories of gaps in section 11 of the survey.
- Did NOT pick a competition; did NOT begin team formation; did NOT commit compute.

## Authority
- Top: C:\Users\Owner\PROJECTS\CONSTITUTION.md (per PROJECTS/AGENTS.md)
- This file obeys the Handoff Standard from C:\Users\Owner\PROJECTS\AGENTS.md (Mission / Current state / Authority / Task / Constraints / Commands / Acceptance criteria / Receipts / Rollback)
- No project-local doctrine overridden.

## Task (ordered)
1. Re-validate freshness before any commitment. Deadlines and prize amounts shift every 2-4 weeks; the snapshot is from 2026-07-23.
2. Choose 1-3 competitions to actually consider entering. Decision criteria should include: compute budget, team availability, regional eligibility, license comfort, novelty preference.
3. Pull full rules + prize terms from the canonical organizer URL, not Devpost/blog summaries (Devpost rules pages rendered empty in 1 case and have numerical inconsistencies in 2 others — see gap #8).
4. Plan the entry: outline of approach, compute requirements, data acquisition plan, submission format rehearsal.
5. Lint the plan against constraints below.

## Constraints
- No irreversible moves (no Kaggle team registration, no Kaggle commit, no Stripe account setup) without operator's explicit OK per tier.
- Receipts required: every cited prize amount or deadline must link to its primary source URL (organizer site > competition page > Wikipedia > blog). Cache the page snapshot if possible.
- Open-source licensing: most comps require MIT/Apache-2.0 or similar for winning submissions; check before reusing proprietary IP.
- Region restrictions: Russia/Cuba/Iran/North Korea + sometimes Brazil/Quebec/Italy/China are excluded on many comps; check per-comp.
- Privacy, health, and finance strictness: any comp touching sensitive domains (medical, financial, legal) requires extra care (AGENTS.md Sensitive Work section).
- Operator's scope: don't commit the user's time/money without consent.

## Commands
- Fetch a comp page: use the same parallel webfetch calls used to produce this snapshot.
- Search engine (DDG HTML): works for news/announcements but CAPTCHA-prone; have backup search or direct organizer URLs.
- For Kaggle (JS-rendered), use Kaggle's public RSS / competitor RSS feed, or `kaggle competitions list` via the Kaggle CLI via Bash.
- For Zindi (JS-rendered), use mlcontests.com with a Zindi filter, or scrape competitor aggregators.
- For Codabench competitions: `https://www.codabench.org/competitions/public/?page=N` paginates.

## Acceptance criteria for completion
- All picked competitions verified by primary source within 7 days of decision.
- Each picked comp has: deadline (with timezone), entry form link, eligibility statement verified, prize breakdown verified.
- Plan document with: team, compute, data, submission rehearsal, license check.
- No irreversible move taken without operator confirmation receipt (e.g., a markdown signed `_receipts/2026-07-23-decision.md`).

## Receipts required
- Save every fetched page snapshot (or its URL + fetched_at timestamp) to `_research/sources/` for audit.
- For each picked comp, a 1-page decision note to `_decisions/` citing the source URL and the user's OK.

## Rollback
- No commits made at this stage — no rollback needed for current state.
- If a competitive entry is actually submitted and then withdrawn: most comps allow deregistration up to a week before deadline. Verify per-comp rules.

## Open questions for the operator (or ChatGPT to gather)
- (a) What's your primary goal — prize money, prestige, research impact, learning, hiring signal?
- (b) Solo or team? If team, what's the regional constraint?
- (c) Compute budget — local GPU only, cloud credits, vendor credits (Google Cloud / Modal / GMI)?
- (d) Domain preference — agents, reasoning, multimodal, vision, math, science, safety?
- (e) Cash preference vs. experience/portfolio — e.g., you could enter many small Hackathons but only one big comp at a time.
- (f) Dates constraint: can you commit through Dec 4 (ARC results)?

## Recommended next research moves
1. Re-run ARC Prize 2026 / WhestBench / Trace the Ace / DaT Parkinson's date checks every Monday.
2. Pull AIMO Progress Prize 3 full rules from `kaggle.com/competitions/ai-mathematical-olympiad-progress-prize-3`.
3. Pin Code Carnage / YouCam / DataHub / ADTC for quick entries.
4. Wait for NeurIPS 2026 comp track formal announcement (likely Aug-Sep 2026).
5. Watch mlcontests.com weekly newsletter for new launches.

## What this handoff does NOT include
- A commitment to enter a specific competition.
- Compute or budget allocation.
- A team roster.
- The user's employer or contractual constraints (out of scope; flag if conventions apply).
