# Web Article Source Packet Worked Example

Status: candidate example. This file illustrates a web-article source packet and does not create canon.

## Source Metadata

```text
Title: Cool URIs don't change
Author: Tim Berners-Lee
Publisher: W3C
Date: 1998
URL: https://www.w3.org/Provider/Style/URI
```

## Packet

```json
{
  "schemaVersion": "v0.1",
  "packetStatus": "candidate",
  "packetManifest": {
    "id": "rsp:example:cool-uris",
    "title": "Cool URIs don't change source packet",
    "version": "0.1.0",
    "scope": "Single public web article source packet example."
  },
  "authority": {
    "producer": "hummbl-dev/research-source-packets",
    "posture": "candidate",
    "orcid": "0000-0000-0000-0000"
  },
  "sourceContract": {
    "sourceType": "web-article",
    "citation": {
      "title": "Cool URIs don't change",
      "authors": ["Tim Berners-Lee"],
      "year": 1998,
      "url": "https://www.w3.org/Provider/Style/URI"
    },
    "citationGraph": [
      {
        "relation": "authored-by",
        "target": "Tim Berners-Lee"
      },
      {
        "relation": "published-by",
        "target": "W3C"
      }
    ],
    "provenance": {
      "origin": "W3C public web article",
      "capturedAt": "2026-07-06T00:00:00Z",
      "method": "manual metadata capture for example packet"
    },
    "evidenceGrade": "public-primary-web-source",
    "verificationStatus": "example-not-verified-live"
  },
  "receiptRequirements": {
    "onAccept": [
      "Record packet id and schemaVersion.",
      "Record capturedAt timestamp.",
      "Do not upgrade evidenceGrade without a live source check."
    ],
    "onReject": [
      "Record the rejection reason.",
      "Preserve the packet id and schemaVersion for audit."
    ],
    "onCite": [
      "Cite the underlying article rather than this packet.",
      "Preserve candidate posture in derivative artifacts."
    ]
  }
}
```

## Receipt

- Added a web-article source packet worked example with metadata, citation graph, evidence grade, and provenance trail.
- Marked live verification as not performed.
- Did not modify operator-authority surfaces.
- Did not introduce new HUMMBL/BaseN terminology.
