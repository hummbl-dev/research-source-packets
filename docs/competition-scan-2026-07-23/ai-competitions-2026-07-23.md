---
title: AI Competitions Research — 2026-07-23 snapshot
date: 2026-07-23
author: opencode session (MiniMax-M3)
scope: Active AI competitions surveyed as of 2026-07-23
status: DRAFT (freshness re-check required before planning decisions)
parent_path: C:\Users\Owner\PROJECTS\_research\
sibling_handoff: HANDOFF-ai-competitions-continuation.md
---

# AI Competitions — Deep Dive (snapshot 2026-07-23)

---

## 1. ARC PRIZE 2026 — $2,000,000 total (the flagship)

Source pages: arcprize.org/competitions/2026, /arc-agi-3, /arc-agi-2, /paper, /competitions/2025, /leaderboard

Overall timeline: started Mar 25, 2026 -> submissions due Nov 2, 2026 -> results Dec 4, 2026. Hosted as 3 separate Kaggle competitions. All winners must open-source under CC0/MIT-0/Apache-2.0+. No internet access during Kaggle evaluation.

### Track 1 — ARC-AGI-3 (Interactive Agents): $850,000
Objective: build agents that explore novel environments with zero instructions.

| Tier | Pool | Payouts |
|---|---|---|
| Grand Prize | $700K | First to score 100%. Rolls forward if unclaimed. |
| Top Score Award (guaranteed) | $75K | 1st $40K, 2nd $15K, 3rd $10K, 4th $5K, 5th $5K |
| Milestone #1 (by Jun 30, 2026) | $37.5K | 1st $25K, 2nd $10K, 3rd $2.5K |
| Milestone #2 (by Sep 30, 2026) | $37.5K | 1st $25K, 2nd $10K, 3rd $2.5K |

Tests four capabilities: Exploration, Modeling, Goal-setting, Planning & Execution. ARC-AGI-3 Developer Preview was the precursor.

### Track 2 — ARC-AGI-2 (Static Reasoning): $700,000
Objective: reach 85% accuracy on ARC-AGI-2 private set under Kaggle compute limits.

| Tier | Pool | Payouts |
|---|---|---|
| Progress Prizes | $275K | 1st $75K, 2nd $50K, 3rd $40K, 4th $35K, 5th $25K, 6th $20K, 7th $15K, 8th $15K |
| Grand Prize | $275K | Best Solution Writeup (6 criteria x 0-5 scale avg) |
| Bonus Prize | $150K | First eligible >=85% solution. Rolls forward to 2027 if unclaimed. |

Scoring: for each task predict 2 outputs per test input grid; score 1 if either matches ground truth exactly; final score = average.

### Track 3 — Paper Prize: $450,000

| Tier | Pool | Payouts |
|---|---|---|
| Top Paper (guaranteed) | $75K | 1st $50K, 2nd $20K, 3rd $5K |
| Outstanding Papers Pool (>=4.5/5 paper score) | $375K | At host discretion, multiple winners |

Required: Abstract, Intro, Prior Work, Approach, Results, Conclusion. Must link to a Kaggle code submission (ARC-AGI-2 or 3 track). Rubric (each 0-5): Accuracy, Universality, Progress, Theory, Completeness, Novelty.

### Leaderboard structure (arcprize.org/leaderboard)
Scatter plot of cost-per-task vs performance. Three solution classes: Reasoning Systems (trend lines), Base LLMs (frontier models), Kaggle Systems ($50 compute, 120 eval tasks). Only systems under $10K/run plotted. (Leaderboard table JS-rendered — scores not enumerable here.)

### Past winners (2025) — useful prior art
- 1st ($25K) — NVARC (24.0% on ARC-AGI-2, $0.20/task): Synthetic-data ensemble of improved Architects-style TTT model + TRM components.
- 2nd ($10K) — the ARChitects (16.5%): 2D-aware masked-diffusion LLM with recursive self-refinement + perspective scoring.
- 3rd ($5K) — MindsAI (12.6%): Heavily engineered TTT pipeline (TTFT + augmentation ensembles + tokenizer dropout).
- 4th ($5K) — Lonnie (6.7%); 5th ($5K) — G. Barbadillo (6.5%).

2025 Paper Awards:
- 1st $50K — A. Jolicoeur-Martineau, "Less is More: Recursive Reasoning with Tiny Networks" (TRM, ~7M params, ~45% ARC-AGI-1, ~8% ARC-AGI-2).
- 2nd $20K — Pourcel, Colas, Oudeyer, "SOAR" (self-improving LLM via evolutionary program synthesis -> 52% ARC-AGI-1).
- 3rd $5K — Liao & Gu, "ARC-AGI Without Pretraining" (CompressARC, MDL-based, ~20-34% ARC-AGI-1, ~4% ARC-AGI-2).
- 5x runners-up $2.5K each (Vector Symbolic Algebras, ARC-NCA, ArcMemo, From Parrots to Von Neumanns).
- Honorable mentions from Meta, Apple, CoreThink.

Practical: ARC Prize 2026 is won by tiny recursive models + synthetic data, not massive LLMs. Last year's grand prize remains unclaimed.

---

## 2. AIcrowd / WhestBench — $100,000 (Alignment Research Center)

Single page: aicrowd.com/challenges/arc-white-box-estimation-challenge-2026
Organizers: Paul Christiano, Jacob Hilton, Wilson Wu (ARC) + Sharada Mohanty, Dipam Chakraborty (AIcrowd). Companion paper: arxiv.org/abs/2605.05179 (Wu, Lecomte, Winer, Robinson, Hilton, Christiano, 2026).

Task: Given a neural network's weights, predict expected per-neuron post-ReLU activations more accurately than Monte-Carlo sampling — foundations of mechanistic interpretability. WhestBench focuses on randomly-init MLPs. Compute counted analytically via flopscope (github.com/AIcrowd/flopscope, NumPy drop-in). Pure CPU, no network at eval time.

Scoring: final-layer MSE vs MC reference, multiplied by max(0.5, Cm/Bm) — under-budget efficiency rewarded. Tie-breaks: extra private MLPs, then all-layer MSE.

| Tier | Payouts |
|---|---|
| Best Score $80K | 1st $50K, 2nd $20K, 3rd $10K |
| Best Algorithmic Contribution | 1st $20K |

| Phase | Dates |
|---|---|
| Warm-up round | May 28, 2026 |
| Phase 1 (open) | Jun 18 - Jul 31, 2026 |
| Phase 2 (final) | Aug 1 - Sep 19, 2026 |
| Final eval | Sep 20 - 30, 2026 |
| Results | Oct 1, 2026 |
| NeurIPS Comp Track workshop | Dec 11-12, 2026 |

LLM-assisted dev explicitly allowed. Submit executable Python via starter-kit ladder (local -> validate -> local-run -> subprocess -> docker -> package). Per-team submission cap during phases.

---

## 3. Conference / Academic Competitions (live or imminent)

### ICML 2026 — Seoul, COEX, July 6-11, 2026
Workshops July 10-11. LLM-use reviewing policy announced.

AI4Math @ ICML 2026 Challenge — Codabench #16104 (codabench.org/competitions/16104)
- Autoformalization problem: how can natural-language math be reliably translated into formal languages?
- Topics: Semantic Alignment in Autoformalization, Formal Theorem proving
- Prizes: 1st $1,000, 2nd $600, 3rd $400
- Key dates: Challenge open May 1, Paper submission May 25, Submission deadline June 15, 2026 (likely CLOSED; check archive)
- ai4math2026.github.io, @ai4mathworkshop

### CVPR 2026 — Completed deadline (end of May 2026)
- BuildingWorld 2026 + S23DR 2026 — reconstruct house roof wireframes from point clouds/segmentations. $14K total prize fund. Likely ended.

### NeurIPS 2026 — December 2026. Comps not yet formally announced.
- WhestBench will be at NeurIPS Comp Track Workshop Dec 11-12.

### KDD Cup 2026 — Dual Tracks
- Track 1 — Data Agents for Complex Data Analysis, ~$30K prize pool, autonomous AI agents for multi-step data workflows over heterogeneous sources. Sample/baselines Mar 15, Comp start Apr 1. Site: dataagent.top. HF discussion: discuss.huggingface.co/t/kdd-cup-2026-call/174314
- Track 2 — Tencent Recommender Systems, ~$885K prize (reports: Tsinghua/HKUST ~$30K sub-track)
- Twitter: @kdd_news Aug 2025 announcement

---

## 4. Mathematics / Olympiad AI — $2M+

AI Mathematical Olympiad — Progress Prize 3 (Kaggle)
- Hosted by AIMO Prize + XTX Markets. Over $2 million in prizes.
- Upgrade vs prior: fairer to non-frontier-lab teams (addresses large-lab resource skew).
- Kaggle: kaggle.com/competitions/ai-mathematical-olympiad-progress-prize-3
- $10M grand AIMO Prize (XTX Markets) targets gold-medal IMO-capable open-source AI.

---

## 5. Devpost Hackathons (full deep-dive)

| # | Hackathon | Deadline 2026 | Total | Top | Real Cash? | Tech Required | Team | Region | Participants |
|---|---|---|---|---|---|---|---|---|---|
| 1 | Gemini XPRIZE | Aug 17 | $2,000,000 | $500K | 100% cash | >=1 Google Cloud + 1 Gemini API call; AI-operated real business w/ revenue | No cap | excl. RU/CU/IR/NK | 21,383 |
| 2 | DataHub Agent | Aug 10 | $20,500 | $6K | Cash + badges | DataHub OSS + MCP/Agent Context Kit etc. | No cap | standard | 1,391 |
| 3 | CockroachDB x AWS | Aug 18 | $8,750 | $5K | Cash + swag | >=2 CockroachDB tools + >=1 AWS service | Max 5 | standard | 2,074 |
| 4 | Arm AI Optimization | Aug 14 | $8,000 | $3K | Cash + blog | Arm architecture (Physical/Cloud/Mobile AI) | No cap | standard | 1,720 |
| 5 | Backblaze Gen Media | Aug 3 | $10,000 | $7K | Cash + mentorship | Backblaze B2 + Genblaze SDK (both mandatory) | No cap | standard | 952 |
| 6 | ADTC Laptop LLM | Aug 24 (G1) | $16,500 | $8K | Cash + GPU credits | On-device LLM, no cloud, 8GB laptop baseline | Max 3 | Africa only | 1,126 |
| 7 | VoltHacks | Sep 5 | "$32,585" | $100 cash | Mostly credits | Hardware/IoT/AI | No cap | 13+ students | 902 |
| 8 | Code Carnage | Aug 18 | $10,000 | $5K | 100% cash | None (open innovation) | Required 2-4 | Students only | 213 |
| 9 | YouCam API | Aug 17 11:45am EDT | $6,000 + units | $5K | Cash top-2 | >=1 YouCam API | No cap | excl. China/Italy/Brazil | 620 |
| 10 | LaunchHacks V | Dec 11-15 | "$17,575" | $200 cash | Mostly domains/swag | None; AI assisted must be cited + human code | Max 4 | Students (HS) | 1,063 |

### Gemini XPRIZE — Key Rules
- "Build a real business, launched during the 90-day window, operated by AI agents." 5 categories: Education & Human Potential, Entrepreneurship & Job Creation, Small Business Services, Money & Financial Access, Professional Services Access.
- Payouts: 1x $500K, 1x $200K, 3x $100K, 15x $50K, 5x $50K category prizes. Max one prize per project.
- Submission requires: GitHub repo (pub or shared to testing@devpost.com + judging@hacker.fund), 3-min video, 500-1000-word narrative, Stripe/bank revenue evidence, expense disclosure, agent execution logs, customer testimonials, free judge access.
- Two-stage judging: pass/fail screen -> scored equally across Business Viability, AI-Native Operations, Category Impact. No minimum revenue threshold but $0 in a month isn't auto-DQ. Verification includes live demo calls; 2-business-day response rule.
- Eligibility: legal age of majority (minors CANNOT win); individuals or orgs <25 employees; standard OFAC exclusions; based on legal residence.
- Late start note: today is Jul 23 — only ~3.5 weeks left of the 90-day revenue window. This is the hardest entry; treat as a startup launch.

### Tactical notes
- Code Carnage = best field/prize ratio (213 entrants, $5K 1st, 1 month).
- ADTC has the most external constraints but biggest on-device LLM stakes ($8K + Africa-tailored).
- YouCam + ADTC have non-standard country exclusions.
- VoltHacks/$32K and LaunchHacks/$17K are credit-inflated headlines — actual cash pools tiny.
- Most judges not named ("panel") for sponsor-led comps; AIcrowd/DrivenData/Kaggle judges are named.

---

## 6. DrivenData (social impact)

Both active. External data allowed if permissively licensed; winners must release under MIT.

### Trace the Ace — $50,000 — National Tutoring Observatory
Source: platform.k12-ai-infrastructure.org/competitions/3/tutoring-outcomes/

- Task: Predict learning gains from a tutoring session using only the student-tutor transcript. Given a transcript + follow-up question, predict if the student answers correctly.
- Format: Code competition (not result upload). Wrap model + inference in containerized runtime. Test locally + smoke test env -> submit ZIP.
- Prizes: 1st $15K, 2nd $10K, 3rd $7K, 9x Publication Bonus $2K each = $50K.
- Dates: Model submissions close Aug 27, 2026; write-ups close Sep 15, 2026. Top 15 teams submit write-ups; judges combine leaderboard ranking + write-up quality.
- License: External data must be open (no NC/CC BY-NC). Winner solutions under MIT.
- Eligibility: Natural persons 18+; legal entities/orgs NOT eligible; standard OFAC + Russia exclusion.
- 580 joined.

### DaT Parkinson's Challenge — EUR 25,000 — French Society of Nuclear Medicine
Source: drivendata.org/competitions/311/dat-parkinsons-challenge/

- Task: CV model on dopamine transporter (DaT) scans to classify normal/abnormal. ~20K DaT scans done annually in France; ~1 in 5 hard to interpret.
- Data: Multicenter, 10 French hospitals. Annotation by nuclear medicine experts. After comp, dataset -> open data on data.gouv.
- Prizes (paid in USD, FX as of Jul 22, 2026): 1st EUR 12,500, 2nd EUR 7,500, 3rd EUR 5,000.
- Deadline: Sept 16, 2026 ("7 weeks left" as of Jul 23).
- Winners released open-source, indexed in BOAS (Bibliotheque Ouverte d'Algorithmes en Sante).
- License: External data with rights rule + MIT for winning code.
- No Codex/ChatGPT web uploads (surgery on hosted data forbidden locally to comply with French regs).
- 54 joined (small field — good odds).

### Practice / completed (good stepping stones)
- What's Up, Docs? (LLM summarization of social-sciences preprints) — beginner LLM practice.
- Conser-vision Practice (camera-trap wildlife) — vision practice.
- Flu Shot Learning, DengAI, Richer's Predictor, Pump It Up — classic beginner to intermediate.

---

## 7. Zindi (Africa / global south)

Sources: zindi.africa, competehub.dev listings, f6s, news posts.

- Platform stats: 80,000+ practitioners, 460+ competitions, $1M+ prize pool. Hiring pipeline for African government Data Innovation Labs and corporate teams.
- Currently known active (from secondary sources):
  - Google WAXAL ASR Challenge — Zindi-hosted, speech-AI for Lingala/Shona/Luganda.
  - Multilingual Health QA in Low-Resource African Languages — $5K USD, ITU + Hub for AI in Maternal, Sexual & Reproductive Health (HASH).
  - EY Biodiversity ("Frog") Challenge — beginner/intermediate data-science, image/regression.
- Note: Zindi main listing page is JS-rendered; visit directly. mlcontests.com maintains a Zindi filter.

---

## 8. AI Safety / Cyber

### DARPA AIxCC — $29.5M (closed)
- Two-year AI Cyber Challenge (2024-2025), DARPA + ARPA-H + frontier labs.
- Winner: Team Atlanta at DEF CON 33, Aug 2025; their Cyber Reasoning System (CRS) won out of 7 finalists (prize pool $8.5M at finals; $7M earmarked for small businesses over the program).
- All 7 finalist CRSs are open-sourced at archive.aicyberchallenge.com — usable resource even though the comp is closed.

### Win4AISafety — Open Research Summer Challenge (Devpost)
- Research AI Safety; non-monetary prizes; deadline Aug 17, 2026. SAIN Utrecht-hosted.

### Other AI-safety venues (unverified — DDG captcha gated searches)
- Apart Research hackathons (Alignment Jams, AI Safety Camps)
- Apart / AGI Safety / MATS research programs
- Apollo Research / METR evals (benchmark-style, not competitive)

---

## 9. Hugging Face

### Build Small Hackathon 2026 — $40K-$48K (mostly completed)
- Build useful AI tools with small models under 32B params + Gradio Spaces.
- Sponsors: OpenAI Codex ($10K main prize pool + ChatGPT Pro subs) + Modal $250 compute credits/participant + HF $20 credits/participant.
- Closed: Registration Jun 3, 2026; dev window Jun 5-15, 2026.
- News: judyailab.com/en/posts/ai-news-20260607-sponsors..., keepingupwith.ai/articles/openai-codex-track-launches...

### HF Competitions org
- HF hosts competitions via the competitions GitHub toolkit; huggingface.co/competitions is the org profile. Most recent pure-comp listings require JS, so direct enumeration is limited here.

---

## 10. Calendar by Deadline (active / imminent as of Jul 23, 2026)

| Date | Event | Prize | Notes |
|---|---|---|---|
| Aug 3 | Backblaze Gen Media | $10K | Earliest deadline; B2 + Genblaze |
| Aug 10 | DataHub Agent | $20.5K | Apache 2.0 repo required |
| Aug 14 | Arm Create AI | $8K | Arm architecture 3 tracks |
| Aug 14 | XPRIZE milestone | Killer milestone | Last day of Month ~3 of 90-day revenue window |
| Aug 17 | Gemini XPRIZE | $2M | Heavy submission package |
| Aug 17 | YouCam | $6K | Skin AI + Apparel VTO |
| Aug 17 | Win4AISafety | Non-cash | AI safety research |
| Aug 18 | CockroachDB x AWS | $8.75K | Agentic memory |
| Aug 18 | Code Carnage | $10K | Open innovation, 2-4 person student teams |
| Aug 24 | ADTC Laptop LLM | $16.5K | On-device, Africa only |
| Aug 27 | Trace the Ace | $50K | Tutoring outcomes |
| Sep 1 | ADTC semifinalists announced | - | Multi-gate comp |
| Sep 5 | VoltHacks | ($100 cash) | Mostly credits |
| Sep 5 | ADTC semifinal submissions | - | |
| Sep 8 | DataHub winners | - | |
| Sep 12 | ADTC finalists announced | - | |
| Sep 15 | Trace the Ace write-ups | - | |
| Sep 16 | DaT Parkinson's | EUR 25K | SFMN medical imaging |
| Sep 17 | ADTC Top-10 defend (Oct 17 demos) | - | |
| Sep 19 | ARC WhestBench Phase 2 close | $100K | Alignment Research Center |
| Sep 20-30 | WhestBench final eval | - | |
| Sep 22 | ADTC Finalists Defense | - | |
| Sep 8 / 15 / 25 | XPRIZE judging -> finalists -> winners | $2M | |
| Sep 30 | ARC-AGI-3 Milestone #2 | $37.5K | Open-source by this date |
| Oct 1 | WhestBench results | $100K | |
| Oct 17 | ADTC Live Defense & Awards | $16.5K | |
| Nov 2 | ARC Prize 2026 submissions due | $2M | All 3 tracks |
| Nov 8 | ARC Papers due | $450K | |
| Dec 4 | ARC Prize 2026 results | $2M | |
| Dec 11-12 | NeurIPS Comp Track Workshop | - | WhestBench + others |
| Dec 11-15 | LaunchHacks V | $575 cash | Mostly domains |

---

## 11. Gaps & Could-Not-Verify

1. Kaggle live listings — JS-rendered; no enumerated list of every currently-open Kaggle feature comp. AIMO Progress Prize 3 and ARC Prize 2026 tracks confirmed live on Kaggle.
2. NeurIPS 2026 competition track — not yet formally announced as of Jul 23, 2026. WhestBench will be featured at NeurIPS Comp Track Workshop.
3. CVPR 2026 challenges — BuildingWorld 2026 / S23DR 2026 ($14K) had a May 2026 deadline — likely closed.
4. ICML 2026 AI4Math challenge — deadline was Jun 15, 2026; check codabench.org/competitions/16104 for archive.
5. Zindi full active list — site is JS-rendered; only 3 active comps confirmed via secondary sources.
6. AI-safety competitions — DuckDuckGo returned captchas on several safety-related searches; Apart Research alignment jams and similar events unverified.
7. Other platforms with low public signal — Tianchi (Alibaba), Bitgrit, ThinkOnward, CrunchDAO, Solafune, EvalAI, Antigranular — exist as live platforms (per mlcontests.com filters) but individual active comps not enumerated.
8. Devpost subtleties: VoltHacks and LaunchHacks V have credit-inflated headline numbers; Code Carnage submissions went past judging/announcement rules; YouCam has unusual country exclusions.

---

## 12. Picking the Right One (recommendation)

| If you want... | Enter |
|---|---|
| Highest upside, public prestige, research impact | ARC Prize 2026 (any track; open source required) |
| A real-money, fast deal for a quick prototype | Code Carnage (213 entrants, $5K 1st, 1 month) |
| A startup pressure-test + $500K possible | Gemini XPRIZE (only if you have a business to grow) |
| Neuro-symbolic / interpretability research | ARC White-Box (WhestBench) ($100K, closes Sep 19) |
| Medical / health impact + open dataset | DaT Parkinson's (EUR 25K, Sep 16) |
| Education / NLP impact | Trace the Ace ($50K, Aug 27) |
| Math reasoning LLM benchmarking | AIMO Progress Prize 3 ($2M+, on Kaggle) |
| Build with small models <32B | Hugging Face Build Small (closed; await Build Small 2 or alternatives) |
| On-device LLM for Africa | ADTC Laptop LLM ($16.5K, Aug 24 gate 1) |
| Invite-only, prestige networking | VoltHacks / LaunchHacks V (mostly student, credits-heavy) |
| Data-agent / heterogeneous-data research | KDD Cup 2026 Data Agents (started Apr 1) |

---

## Sources
arcprize.org/{competitions/2026/{arc-agi-3, arc-agi-2, paper}, leaderboard, competitions/2025}; aicrowd.com/challenges/arc-white-box-estimation-challenge-2026; platform.k12-ai-infrastructure.org/competitions/3/tutoring-outcomes/; drivendata.org/competitions/311/dat-parkinsons-challenge/; xprize.devpost.com, datahub.devpost.com, cockroachdb-ai.devpost.com, arm-ai-optimization-challenge.devpost.com, backblaze-generative-media.devpost.com, adtc-2026.devpost.com, volthacks.devpost.com, code-carnage.devpost.com, youcam-api.devpost.com, launchhacks-v.devpost.com; DuckDuckGo HTML searches for KDD Cup 2026, Hugging Face, AIMO 2026, ICML 2026, Zindi, DARPA AIxCC, AI safety; ai4math2026.github.io; dataagent.top; aimoprize.com; huggingface.co; zindi.africa; mlcontests.com; archive.aicyberchallenge.com.
