# Issue 5 Receipt - Web Article Example

## Scope

Issue #5 requested a concrete web-article source packet in `examples/` with:

- source metadata;
- a citation graph entry;
- evidence grade;
- provenance trail.

## Artifacts

- `examples/web-article-cool-uris.v0.1.example.json`
- `schemas/research-source-packets-v0.1.json`
- `docs/examples.md`

## Source

The example uses a public W3C web page:

- Title: `Cool URIs don't change`
- Author: Tim Berners-Lee
- Publisher/origin: World Wide Web Consortium public web page
- URL: `https://www.w3.org/Provider/Style/URI`
- Year: 1998

## Boundary

The packet is `candidate` and `authority.posture` is `candidate`.

The optional citation graph entry is a candidate output only:

```json
"candidateOnly": true
```

This receipt does not introduce canon, does not create HUMMBL/BaseN terminology,
and does not modify operator-authority surfaces.

## Validation

Expected validation for this change:

```bash
python -m json.tool schemas/research-source-packets-v0.1.json
python -m json.tool examples/web-article-cool-uris.v0.1.example.json
python -m json.tool examples/research-source-packet-v0.1.example.json
python -m json.tool fixtures/valid/research-source-packet-v0.1.valid.json
python -m json.tool fixtures/invalid/research-source-packet-v0.1.invalid.json
```
