# Research Source Packet Receipt

## Packet metadata

- Packet: `docs/pi-cli-adoption-evidence-packet-2026-07-19.md`
- Packet ID: `rsp:pi-cli:adoption-evidence:2026-07-19`
- Receipt ID: `receipt:rsp:pi-cli:adoption-evidence:2026-07-19`
- Schema version: `v0.1`
- Issuer: `hummbl-dev/research-source-packets`
- Manifest scope: Pi CLI adoption evidence across evaluator, GUI/runtime,
  transcript, CI, extension/skill, packaging, observability, and token
  optimization lanes.

## Receipt status

- Posture: candidate, non-canon
- Verification state: SOURCE_CLAIM and EXTERNALLY_REPORTED preserved; PR/issue
  states verified via `gh pr view` / `gh issue view` on 2026-07-19. No
  independent code-level verification performed in-repo.
- Canonical-structure fit: candidate non-adopted, bounded source-packet format.

## Discovery and synthesis provenance

- Discovery: Approved by Reuben in ChatGPT on 2026-07-10 after a live GitHub
  search and ranked synthesis (issue #13 provenance).
- Retrieval: Performed by Devin subagent on 2026-07-19 using `gh pr view`,
  `gh issue view`, `gh repo view`, `gh api`, and `webfetch` for pi.dev.
- Synthesis: Performed by Devin subagent on 2026-07-19. All primary sources
  received deeper architectural summaries. Secondary sources received
  state-checked summaries.
- Review: Pending operator review.

## Exact source list (captured on 2026-07-19)

### First-party

1. https://pi.dev
2. https://github.com/earendil-works/pi
3. https://www.npmjs.com/package/@earendil-works/pi-coding-agent

### Primary (5)

4. https://github.com/promptfoo/promptfoo/pull/9707
5. https://github.com/pingdotgg/t3code/pull/3818
6. https://github.com/microsoft/SkillOpt/pull/83
7. https://github.com/Tim-Pohlmann/rix/pull/67
8. https://github.com/DeusData/codebase-memory-mcp/pull/534

### Secondary (13)

9. https://github.com/addyosmani/agent-skills/issues/355
10. https://github.com/addyosmani/agent-skills/pull/365
11. https://github.com/dustinblack/agent-dashboard/issues/51
12. https://github.com/dustinblack/agent-dashboard/pull/81
13. https://github.com/msitarzewski/agency-agents/issues/470
14. https://github.com/msitarzewski/agency-agents/pull/591
15. https://github.com/himattm/prism/pull/167
16. https://github.com/fletchgqc/agentbox/pull/71
17. https://github.com/JuliusBrussee/caveman/issues/161
18. https://github.com/JuliusBrussee/caveman/pull/551
19. https://github.com/shelbon/token-optimizer/pull/1
20. https://github.com/pingdotgg/t3code/issues/397
21. https://github.com/pingdotgg/t3code/issues/402

### Referenced upstream

22. https://github.com/NikiforovAll/pi-otel/issues/4
23. https://github.com/NikiforovAll/pi-otel/pull/6
24. https://github.com/IgorWarzocha/t3code/pull/1
25. https://github.com/OpenClaw/OpenClaw

## Receipt posture

- `SOURCE_CLAIM`: Pi first-party claims (pi.dev, earendil-works/pi) preserved
  with explicit label.
- `EXTERNALLY_REPORTED`: All third-party adoption evidence (PRs, issues,
  community extensions) preserved separately.
- `DO_NOT_ADOPT_YET`: no Pi component, extension, skill, or interface is
  canonized or adopted as a HUMMBL dependency.
- `LOCAL_AUDIT_REQUIRED`: author-reported test results, QA validations, and
  head-to-head comparisons are not independently verified by this packet.

## Notes

- This is not a performance validation packet. No claim in this receipt is
  treated as independently validated.
- No secrets, tokens, private model artifacts, or private operational data
  were added.
- No new HUMMBL/BaseN/Ownward terms are canonized in this packet.
- The issue text references `Tim-Pohlermann/rix` (with an extra 'e'); the
  actual repository is `Tim-Pohlmann/rix`. This discrepancy is noted in the
  packet.
- Automated dependency-update noise was excluded: no Dependabot, Renovate, or
  bot-generated PRs were included in the evidence set.
