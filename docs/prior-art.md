# Prior Art and Adjacent Ecosystem

This document collects public prior art and adjacent ecosystem references
relevant to research source packets. It is a discoverability aid, not an
endorsement. Nothing here is canonized by being listed.

## Non-canon note

Listing a project here does **not** imply endorsement, adoption, or any claim
of relationship. These are independent public projects with their own
governance, licenses, and communities. This repository claims no ownership of
their terminology, designs, or output. This list exists only to situate the
candidate packet format within the existing landscape.

## Public prior art

### Zotero

- **What it does:** Open-source reference manager. Collects, organizes, cites,
  and shares research sources with browser integration and a local library.
- **Relevance:** Core reference-management workflow that a source packet must
  interoperate with conceptually (citation capture, metadata, attachments).
- **Docs:** https://www.zotero.org/support/

### Mendeley

- **What it does:** Reference manager and academic social network owned by
  Elsevier. Manages PDFs, metadata, annotations, and citations.
- **Relevance:** Widely used reference-management workflow; relevant for
  metadata shape and citation export expectations.
- **Docs:** https://www.mendeley.com/guides/

### EndNote

- **What it does:** Commercial reference manager from Clarivate. Manages
  bibliographies and citations, with strong publisher style support.
- **Relevance:** Long-standing reference-management tool; relevant for citation
  style and bibliography expectations.
- **Docs:** https://endnote.com/

### JabRef

- **What it does:** Open-source reference manager built around BibTeX, popular
  in the LaTeX workflow.
- **Relevance:** Demonstrates a plain-text, version-controllable bibliography
  format — directly relevant to packet provenance and reviewability.
- **Docs:** https://docs.jabref.org/

### Paperpile

- **What it does:** Lightweight, web-based reference manager with strong Google
  Docs and search integration.
- **Relevance:** Modern reference-management UX; relevant for capture and
  metadata normalization expectations.
- **Docs:** https://paperpile.com/

### ReadCube

- **What it does:** Reference manager with PDF enhancement, annotation, and
  discovery features (Papers is a related product line).
- **Relevance:** Reference management plus enhanced-metadata workflows.
- **Docs:** https://www.readcube.com/

### Semantic Scholar

- **What it does:** Free academic search engine from AI2 that provides
  structured paper metadata, citation context, and influence signals.
- **Relevance:** Source of structured metadata and citation-graph signals that
  a packet's `sourceContract` could reference.
- **Docs:** https://www.semanticscholar.org/product/api

### Connected Papers

- **What it does:** Visual tool that builds a graph of prior work related to a
  given paper using co-citation and bibliographic coupling.
- **Relevance:** Demonstrates reference-graph navigation adjacent to a single
  source — relevant to how packets relate to surrounding literature.
- **Docs:** https://www.connectedpapers.com/

### Research Rabbit

- **What it does:** Discovery and visual citation-graph tool that recommends
  related work and shows relationships between papers and authors.
- **Relevance:** Reference-graph discovery UX; relevant to adjacency and
  provenance context for a source.
- **Docs:** https://www.researchrabbit.ai/

## Adjacent ecosystem

### ORCID

- **What it does:** Persistent digital identifier for researchers.
- **Relevance:** Authority and attribution layer; relevant to packet
  `authority` and provenance of who produced or cited a source.
- **Docs:** https://orcid.org/

### Crossref

- **What it does:** DOI registration and metadata infrastructure for scholarly
  content; provides citation and metadata APIs.
- **Relevance:** Canonical source of citation metadata and DOIs that a packet's
  `citation` field should be able to reference.
- **Docs:** https://www.crossref.org/

### DataCite

- **What it does:** DOI registration and metadata infrastructure for research
  data and other non-article outputs.
- **Relevance:** Provenance and citation for datasets — relevant when a packet
  represents a data source rather than a paper.
- **Docs:** https://datacite.org/

### OpenCitations

- **What it does:** Open infrastructure that provides open citation data using
  COCI and other services.
- **Relevance:** Open citation-graph data; relevant to provenance and
  reference-graph context for a packet.
- **Docs:** https://opencitations.net/

### I4OC (Initiative for Open Citations)

- **What it does:** Campaign and coalition to make scholarly citation data
  openly available.
- **Relevance:** Policy and community context for open citations that a packet
  format should align with in spirit.
- **Docs:** https://i4oc.org/

### Unpaywall

- **What it does:** Free service from Impactstory that finds open-access
  versions of paywalled articles using DOIs.
- **Relevance:** Access and provenance layer; relevant to where a packet's
  source can actually be retrieved.
- **Docs:** https://unpaywall.org/

### CORE

- **What it does:** Aggregator of open-access research papers from repositories
  and journals worldwide.
- **Relevance:** Source discovery and full-text access; relevant to packet
  source retrieval and provenance.
- **Docs:** https://core.ac.uk/

### arXiv

- **What it does:** Open preprint server for physics, math, CS, and other
  fields.
- **Relevance:** Common preprint source type; relevant to `sourceType` and
  version handling within a packet (preprints change).
- **Docs:** https://arxiv.org/

## Vocabulary

Working vocabulary for the candidate packet format. These are descriptive
labels, not claims of ownership:

- **source packet** — a bounded, versioned, reviewable unit of research source
  material with provenance and a source contract.
- **citation** — a structured reference to a source, minimally enough to
  retrieve and identify it.
- **provenance** — where the source came from, who captured it, and how it was
  transformed.
- **bibliography** — a collection of citations associated with a packet or
  claim.
- **evidence chain** — a sequence of sources and inferences supporting a claim.
  Out of scope for v0.1 beyond a single packet's evidence grade.
- **reference graph** — the network of relationships between sources. Out of
  scope for v0.1 beyond adjacency references.

## Key concepts

- **source metadata** — structured fields describing a source (type, citation,
  provenance, grade, verification status).
- **citation graph** — the network of citations among sources; adjacent to,
  but not required by, a single v0.1 packet.
- **evidence grading** — an explicit, stated grade for the strength/type of
  evidence a source provides.
- **provenance tracking** — recording the origin and handling of a source.
- **reproducibility** — the ability for a reviewer or agent to retrieve and
  re-derive the packet's source and claims.
- **reference management** — the established field of organizing, citing, and
  sharing research sources, which packets must interoperate with.
