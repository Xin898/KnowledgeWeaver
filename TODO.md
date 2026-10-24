# TODO

This is the executable project backlog. Check an item only when its acceptance criteria and tests are complete.

## P0 — Repository foundation

- [ ] Choose and add a license.
- [ ] Add `.gitignore`, `.editorconfig` and contribution guidance.
- [ ] Create Python 3.12+ project with `pyproject.toml`.
- [ ] Add `src/knowledge_weaver` package and CLI entry point.
- [ ] Configure Ruff, mypy and pytest.
- [ ] Add GitHub Actions for tests, lint and type checks.
- [ ] Add dependency and secret scanning.
- [ ] Record architectural decisions in `docs/adr/`.

## P0 — Canonical schema

- [ ] Define enums for node and relationship types.
- [ ] Implement Document, Section, GraphNode, Relationship and Evidence models.
- [ ] Define UnresolvedItem and MergeProposal models.
- [ ] Publish `schemas/knowledge-graph-v1.json`.
- [ ] Implement stable ID generation.
- [ ] Validate confidence range and extraction type.
- [ ] Reject dangling relationship endpoints.
- [ ] Require provenance for claims and relationships.
- [ ] Add valid and invalid JSON fixtures.

## P0 — Single-article pipeline

- [ ] Implement Markdown reader.
- [ ] Implement plain-text reader.
- [ ] Preserve headings and code blocks.
- [ ] Generate stable section and paragraph locations.
- [ ] Calculate normalized document hash.
- [ ] Create versioned extraction prompt.
- [ ] Define provider-neutral extractor interface.
- [ ] Implement the first LLM provider adapter.
- [ ] Parse JSON without accepting surrounding prose.
- [ ] Add bounded repair retry using structured validation errors.
- [ ] Route low-confidence results to unresolved items.
- [ ] Produce a machine-readable run report.

## P0 — Obsidian MVP

- [ ] Define default vault directory layout.
- [ ] Create Jinja templates for article, concept and claim notes.
- [ ] Generate YAML frontmatter.
- [ ] Generate typed relationship sections and wikilinks.
- [ ] Sanitize filenames across Windows, macOS and Linux.
- [ ] Mark KnowledgeWeaver-owned files.
- [ ] Refuse to overwrite unmanaged human-authored notes.
- [ ] Implement dry-run file plan.
- [ ] Use atomic file writes.
- [ ] Verify idempotent reruns with snapshot tests.
- [ ] Build an example vault from a short original test article.

## P1 — Entity resolution

- [ ] Normalize casing, whitespace and punctuation.
- [ ] Support explicit alias tables.
- [ ] Implement exact canonical match.
- [ ] Generate fuzzy merge proposals.
- [ ] Add interactive approve/reject workflow.
- [ ] Persist merge decisions.
- [ ] Aggregate provenance across documents.
- [ ] Add conflict report for incompatible claims.

## P1 — CLI usability

- [ ] Implement `ingest` command.
- [ ] Implement `validate` command.
- [ ] Implement `export` command.
- [ ] Implement `diff` command.
- [ ] Add TOML configuration support.
- [ ] Add clear exit codes and error messages.
- [ ] Add redacted structured logging.
- [ ] Document environment variables.
- [ ] Add quick-start tutorial.

## P2 — Neo4j adapter

- [ ] Add Neo4j driver as an optional dependency.
- [ ] Define labels, relationship types, constraints and indexes.
- [ ] Implement transactional node upsert.
- [ ] Implement transactional relationship upsert.
- [ ] Preserve evidence and source locations.
- [ ] Implement incremental synchronization.
- [ ] Add disposable Neo4j integration tests.
- [ ] Document useful Cypher queries.
- [ ] Verify repeated export creates no duplicates.

## P2 — Additional inputs

- [ ] Add HTML/article parser.
- [ ] Add safe URL fetching with timeouts and size limits.
- [ ] Add PDF parser with page references.
- [ ] Add EPUB parser with chapter references.
- [ ] Add configurable chunking for long documents.
- [ ] Add overlap and context rules without duplicated evidence.

## P3 — Graph RAG and UI

- [ ] Define graph retrieval API.
- [ ] Retrieve supporting evidence with every result.
- [ ] Add grounded question answering.
- [ ] Build evaluation dataset and metrics.
- [ ] Add read-only application API.
- [ ] Build search and graph exploration interface.
- [ ] Add filters for type, source, confidence and extraction type.

## Security and data safety

- [ ] Treat imported documents as untrusted data.
- [ ] Add prompt-injection test fixtures.
- [ ] Restrict all exports to configured output roots.
- [ ] Prevent path traversal in generated filenames.
- [ ] Keep provider keys out of configuration committed to Git.
- [ ] Redact sensitive content from logs.
- [ ] Add backups before managed-file replacement.
- [ ] Keep Obsidian Local REST API integration disabled by default.
- [ ] Never expose the Obsidian API outside localhost by default.

## Release milestones

- [ ] **v0.1.0:** one article → validated JSON → Obsidian vault.
- [ ] **v0.2.0:** multi-document deduplication and review.
- [ ] **v0.3.0:** Neo4j export and Cypher examples.
- [ ] **v0.4.0:** Graph RAG evaluation and read-only API.
- [ ] **v1.0.0:** stable schemas, migration policy and documented compatibility.
