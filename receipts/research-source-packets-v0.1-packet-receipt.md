# Packet Receipt — research-source-packets v0.1

This is a public receipt for the v0.1 packet work in this repository. It is a
record of what was produced, under what posture, and what review is expected.
It is **non-canon** and exploratory.

## Receipt metadata

- **Packet schema version:** v0.1
- **Posture:** candidate, non-canon
- **Producer:** hummbl-dev/research-source-packets
- **Base branch:** main
- **Working branch:** feat/devin/v0.1-packet

## Artifacts produced

- `docs/v0.1-boundary.md` — scope, non-goals, adoption boundary, review
  expectations, non-canon posture, required packet pieces, adjacent repo links.
- `docs/prior-art.md` — public prior art, adjacent ecosystem, vocabulary, and
  key concepts.
- `schemas/research-source-packets-v0.1.json` — JSON schema for a v0.1 packet.
- `examples/research-source-packet-v0.1.example.json` — a worked valid example.
- `fixtures/valid/research-source-packet-v0.1.valid.json` — a valid fixture.
- `fixtures/invalid/research-source-packet-v0.1.invalid.json` — an invalid
  fixture (missing required `sourceContract`).

## Issues addressed

- #1 — Define v0.1 scope and boundary
- #2 — Collect prior art and adjacent ecosystem references
- #3 — Design minimal schema/examples layout

## Review expectations

Reviewers should confirm:

- All required packet pieces are present in the schema and example.
- The invalid fixture fails for the stated reason (missing `sourceContract`).
- No private, internal, or secret operational content is present.
- Candidate/non-canon posture is marked explicitly throughout.
- Prior art and adjacent ecosystem entries link to public docs and make no
  ownership or endorsement claims.

## Non-canon statement

Nothing in this receipt or the artifacts it references is adopted guidance.
v0.1 is a candidate release. Adoption requires a reviewed governance path that
is not yet defined. The maintainers claim no authority over the broader field
or its terminology.
