# Features and Acceptance Criteria

## Feature status legend

- **MVP**: required for the first usable release.
- **Next**: required after the single-document workflow is stable.
- **Later**: intentionally deferred.

## 1. Source ingestion

| ID | Priority | Function | Acceptance criteria |
|---|---|---|---|
| ING-01 | MVP | Import Markdown | Preserves headings, code blocks and source path |
| ING-02 | MVP | Import plain text | Produces deterministic paragraph IDs |
| ING-03 | MVP | Normalize content | Identical normalized input produces the same hash |
| ING-04 | Next | Import HTML/article URL | Removes navigation while preserving headings and code |
| ING-05 | Later | Import PDF/EPUB | Reports page or chapter locations where available |

## 2. Knowledge extraction

| ID | Priority | Function | Acceptance criteria |
|---|---|---|---|
| EXT-01 | MVP | Extract typed nodes | Only configured node types are accepted |
| EXT-02 | MVP | Extract typed relationships | Only configured relation types are accepted |
| EXT-03 | MVP | Extract claims and evidence | Each accepted claim includes source location and evidence |
| EXT-04 | MVP | Mark explicit vs inferred | Inferred items are never presented as direct quotations |
| EXT-05 | MVP | Record confidence | Values outside 0..1 fail validation |
| EXT-06 | Next | Provider adapters | A provider can be changed without modifying domain code |
| EXT-07 | Next | Prompt versioning | Every graph records the prompt/schema version used |

Initial node types:

- Article
- Person
- Organization
- Technology
- Concept
- Event
- Claim
- Problem
- Method
- Evidence
- CodeExample
- Metric
- Dataset
- Location
- Date

Initial relation types:

- DISCUSSES
- DEFINES
- IS_A
- PART_OF
- DEPENDS_ON
- REQUIRES
- USES
- IMPLEMENTS
- SOLVES
- CAUSES
- AFFECTS
- SUPPORTS
- CONTRADICTS
- COMPARES_WITH
- EXAMPLE_OF
- CREATED_BY
- ASSOCIATED_WITH
- MENTIONS

## 3. Validation

| ID | Priority | Function | Acceptance criteria |
|---|---|---|---|
| VAL-01 | MVP | JSON Schema validation | Invalid fields produce machine-readable errors |
| VAL-02 | MVP | Graph integrity validation | Dangling relationships are rejected |
| VAL-03 | MVP | Evidence validation | Claims/edges without evidence are rejected |
| VAL-04 | MVP | Repair retry | At most the configured retries; no infinite loop |
| VAL-05 | MVP | Unresolved queue | Low-confidence items remain outside the accepted graph |
| VAL-06 | Next | Quality metrics | Reports counts, rejection reasons and confidence distribution |

## 4. Entity resolution

| ID | Priority | Function | Acceptance criteria |
|---|---|---|---|
| RES-01 | MVP | Stable identifiers | Same normalized entity receives the same ID |
| RES-02 | MVP | Exact alias matching | Configured aliases resolve deterministically |
| RES-03 | Next | Fuzzy matching | Similarity produces proposals, not automatic merges |
| RES-04 | Next | Human merge decisions | Decisions are persisted and reproducible |
| RES-05 | Next | Cross-document provenance | A canonical node retains every supporting source |

## 5. Obsidian export

| ID | Priority | Function | Acceptance criteria |
|---|---|---|---|
| OBS-01 | MVP | Generate Markdown notes | Output opens normally in Obsidian |
| OBS-02 | MVP | Generate YAML frontmatter | IDs, types, aliases and sources are present |
| OBS-03 | MVP | Generate wikilinks | All generated links resolve to generated or existing notes |
| OBS-04 | MVP | Preserve human files | Files without KnowledgeWeaver ownership marker are not overwritten |
| OBS-05 | MVP | Idempotent reruns | Reprocessing causes no duplicate files or links |
| OBS-06 | Next | Configurable templates | Users can customize note structure |
| OBS-07 | Later | Local REST API adapter | Disabled by default and limited to localhost |

## 6. CLI and reports

Proposed commands:

```text
knowledge-weaver ingest ARTICLE.md --output ./vault
knowledge-weaver validate graph.json
knowledge-weaver export graph.json --format obsidian --output ./vault
knowledge-weaver diff graph.json previous.json
knowledge-weaver review merge-proposals.json
```

| ID | Priority | Function | Acceptance criteria |
|---|---|---|---|
| CLI-01 | MVP | Dry run | Shows planned file changes without writing |
| CLI-02 | MVP | Clear failures | Non-zero exit code and actionable message |
| CLI-03 | MVP | Configuration file | CLI flags override file values |
| CLI-04 | MVP | Validation report | Contains accepted, rejected and unresolved counts |
| CLI-05 | Next | Graph diff | Reports added, changed and removed graph objects |

## 7. Neo4j export

| ID | Priority | Function | Acceptance criteria |
|---|---|---|---|
| NEO-01 | Later | Create constraints/indexes | Re-running setup is safe |
| NEO-02 | Later | Transactional upsert | Partial failures do not leave half-written graphs |
| NEO-03 | Later | Incremental synchronization | Unchanged objects are not duplicated |
| NEO-04 | Later | Query examples | Prerequisite, evidence and path queries are documented |

## 8. Graph RAG

| ID | Priority | Function | Acceptance criteria |
|---|---|---|---|
| RAG-01 | Later | Graph-aware retrieval | Returns nodes plus supporting evidence |
| RAG-02 | Later | Grounded answers | Each substantive answer cites source locations |
| RAG-03 | Later | Evaluation set | Retrieval and answer quality are measured |
| RAG-04 | Later | Uncertainty behavior | Unsupported answers are refused or marked uncertain |

## 9. Non-functional requirements

- Local-first and usable without Neo4j.
- Deterministic IDs, filenames and serialization.
- UTF-8 and multilingual text support.
- No secret values in logs or generated notes.
- Atomic filesystem writes and recoverable backups.
- Test coverage focused on data integrity and overwrite safety.
- Extensible adapters without changes to the canonical model.
