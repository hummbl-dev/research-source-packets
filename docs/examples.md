# Examples

Examples in this repository are candidate/non-canon fixtures. They demonstrate
packet shape, not source endorsement or framework adoption.

## Current Examples

| File | Source type | Purpose |
| --- | --- | --- |
| `examples/research-source-packet-v0.1.example.json` | journal article | Minimal worked example for the required v0.1 packet core |
| `examples/web-article-cool-uris.v0.1.example.json` | web page | Public web-article packet with source metadata, provenance, evidence grade, and a candidate citation-graph entry |

## Example Rules

Examples must:

- avoid private operational content;
- mark candidate status clearly;
- avoid canonizing HUMMBL/BaseN/Ownward concepts;
- cite the underlying source rather than the packet in derivative work;
- preserve `candidateOutputs.*.candidateOnly=true` when optional candidate
  outputs are used.
