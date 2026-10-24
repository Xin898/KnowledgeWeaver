# Development Plan

## 1. Goal

Build a local-first system that converts articles and technical books into an evidence-backed knowledge graph that can be reviewed in Obsidian and later queried in Neo4j.

The project succeeds when a user can run one command on a source document and receive:

1. validated structured graph data;
2. readable Obsidian notes and wikilinks;
3. a report of uncertain or rejected extractions;
4. reproducible output without duplicated concepts.

## 2. Scope

### MVP scope

- Inputs: UTF-8 Markdown and plain text.
- Processing: normalization, sectioning, LLM extraction, validation and entity resolution.
- Outputs: canonical JSON and an Obsidian vault.
- Operation: local Python CLI.
- Provenance: every generated node and edge carries document, section, evidence and confidence.
- Safety: no automatic database write before validation.

### Deferred scope

- PDF and EPUB ingestion.
- Website crawling and JavaScript rendering.
- Neo4j synchronization.
- Graph RAG and conversational query.
- Multi-user collaboration.
- Hosted SaaS and mobile application.
- Autonomous deletion or rewriting of human-authored notes.

## 3. Technical baseline

- Runtime: Python 3.12+
- CLI: Typer
- Models: Pydantic
- Schema: JSON Schema
- Templates: Jinja2
- Tests: pytest
- Lint/type checks: Ruff and mypy
- LLM provider: adapter interface; provider credentials stay in environment variables
- Primary storage in MVP: filesystem
- Human interface in MVP: Obsidian
- Optional graph database: Neo4j in Phase 3

These choices are proposals and may change through Architecture Decision Records.

## 4. Delivery phases

### Phase 0 — Foundation

Deliverables:

- Python package and CLI skeleton.
- Configuration model and example configuration.
- Domain models for documents, sections, nodes, relationships and evidence.
- Versioned JSON Schema.
- Test fixtures and CI.
- Initial extraction prompt.

Exit criteria:

- `knowledge-weaver --help` runs.
- Schema fixtures pass validation.
- CI runs unit tests, lint and type checks.
- No secrets are committed.

### Phase 1 — Single-document Obsidian MVP

Deliverables:

- Markdown and text ingestion.
- Stable paragraph/section identifiers.
- LLM extraction through a provider-neutral interface.
- JSON parsing, schema validation and repair retry.
- Confidence and explicit/inferred classification.
- Obsidian exporter with YAML frontmatter and wikilinks.
- Dry-run and validation report.

Exit criteria:

- One sample technical article produces a valid graph.
- Every relationship references existing nodes.
- Every accepted relationship has evidence and a source location.
- Re-running the same input does not create duplicate notes.
- Output opens as an Obsidian vault without manual repair.

### Phase 2 — Multi-document knowledge base

Deliverables:

- Canonical entity IDs and aliases.
- Exact and fuzzy duplicate detection.
- Merge proposals requiring human approval.
- Source aggregation across documents.
- Incremental processing manifest.
- Conflict and unresolved-item reports.

Exit criteria:

- Processing the same corpus twice is idempotent.
- Shared concepts from multiple documents are linked to all sources.
- Ambiguous merges are never silently accepted.
- A user can approve or reject merge proposals.

### Phase 3 — Neo4j adapter

Deliverables:

- Neo4j constraints and indexes.
- Transactional upsert exporter.
- Source and evidence nodes/properties.
- Incremental synchronization.
- Query examples for prerequisites, supporting evidence and concept paths.

Exit criteria:

- Export is repeatable without duplicates.
- Failed writes roll back cleanly.
- Obsidian and Neo4j derive from the same canonical graph.
- Integration tests run against an ephemeral Neo4j instance.

### Phase 4 — Graph RAG and visual application

Deliverables:

- Graph-aware retrieval.
- Answer citations back to source sections.
- Read-only API.
- Search and graph exploration UI.
- Evaluation dataset for retrieval and grounded answers.

Exit criteria:

- Answers cite graph evidence and original document positions.
- Unsupported claims are rejected or explicitly marked uncertain.
- Retrieval quality and latency are measured against defined targets.

## 5. Testing strategy

| Layer | Required tests |
|---|---|
| Domain | IDs, model validation, serialization |
| Ingestion | encodings, headings, paragraph positions |
| Extraction | mocked provider, malformed JSON, retry boundaries |
| Validation | dangling edges, unknown types, missing evidence |
| Resolution | aliases, duplicates, ambiguous candidates |
| Obsidian | snapshots, paths, frontmatter, wikilinks |
| Neo4j | constraints, idempotent upsert, transaction rollback |
| End-to-end | article to vault and report |

Golden fixtures must be small, reviewable and free of copyrighted full-text material.

## 6. Quality targets

- 100% of accepted relationships reference valid nodes.
- 100% of accepted claims and relationships contain provenance.
- Zero duplicate note creation on identical reruns.
- No API keys or source documents in logs by default.
- Deterministic filenames and IDs for identical normalized inputs.
- Extraction metrics reported separately by node type and relationship type.

## 7. Main risks

| Risk | Mitigation |
|---|---|
| Hallucinated entities or edges | Evidence requirement, schema validation, confidence threshold |
| Duplicate concepts | Canonicalization plus human-approved merge proposals |
| Prompt changes breaking output | Version prompts and keep golden tests |
| Vendor lock-in | Provider adapter and canonical JSON |
| Unsafe overwrites | Dry-run, generated-file marker, backups and atomic writes |
| Prompt injection in documents | Treat source as data, restrict tools, validate all output |
| Excessive graph noise | Node budget and allowlisted types |
| Neo4j complexity too early | Keep it outside the MVP |

## 8. Definition of done

A feature is done only when it has:

- an explicit acceptance criterion;
- automated tests;
- typed public interfaces;
- user-facing error messages;
- documentation and an example;
- no known high-severity data-loss path.
