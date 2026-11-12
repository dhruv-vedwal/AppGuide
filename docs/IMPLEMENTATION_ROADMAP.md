# Implementation Roadmap — AppGuide

## Execution rules
- One task ID per coding-agent assignment.
- Do not start a later phase until its exit criteria pass or an explicit decision records why.
- Each task must end with tests, a diff review and a status update.
- Build one working vertical slice before adding scale or broad framework support.

## Phase 0 — Foundation and development environment
**Goal:** reproducible repository, service skeleton and safe local environment.

Tasks:
- P0-01: Create monorepo layout (`backend/`, `dashboard/`, `sdk/`, `docs/`).
- P0-02: Add configuration loading, typed settings and safe example environment file.
- P0-03: Add Docker Compose for PostgreSQL/pgvector and backend; document local startup.
- P0-04: Add FastAPI app factory, health endpoint, structured error handling and request IDs.
- P0-05: Add database connection, migration tool and initial application/repository tables only.
- P0-06: Add lint, format, type-check and test commands for backend.
- P0-07: Add CI workflow for tests and static checks.
- P0-08: Add security baseline: no repository execution, path validation and secret-safe logging.

Exit criteria:
- A clean clone can start the documented local stack.
- Health endpoint and database connectivity tests pass.
- Migrations run from an empty database and are repeatable.
- CI passes.
- No model or SDK work is started in this phase.

## Phase 1 — One-repository deterministic ingestion
**Goal:** ingest one local JS/TS repository and produce a provenance-rich extraction report.

Tasks:
- P1-01: Define repository registration and scan configuration.
- P1-02: Implement safe file discovery with ignore rules, path/symlink protections and size limits.
- P1-03: Record repository commit/version and scan manifest.
- P1-04: Implement JS/TS parsing using TypeScript Compiler API and/or Tree-sitter as justified.
- P1-05: Extract files, symbols, imports/exports and source spans.
- P1-06: Extract supported frontend routes and API route declarations where statically detectable.
- P1-07: Extract backend handlers/controllers and service/model references where statically detectable.
- P1-08: Persist entities and verified relationships with source provenance.
- P1-09: Generate an ingestion report: files discovered/parsed/skipped, entities, edges, errors and coverage.
- P1-10: Add fixture repositories and deterministic parser tests.

Exit criteria:
- One configured JS/TS repository scans without executing its code.
- Every persisted entity has repository, commit, path and source-span provenance where applicable.
- Unsupported/dynamic patterns are reported as unknown, not fabricated.
- Repeated scans are deterministic for the same commit/configuration.

## Phase 2 — Evidence-backed feature intelligence
**Goal:** turn extracted code structure into candidate feature maps and grounded descriptions.

Tasks:
- P2-01: Define feature/entity/edge schemas and evidence references.
- P2-02: Build deterministic candidate grouping from routes, imports, calls and naming.
- P2-03: Add local LLM provider interface and Ollama adapter.
- P2-04: Add constrained structured enrichment prompt/output schema.
- P2-05: Validate every generated file, symbol, span and relationship against extraction records.
- P2-06: Persist verified/inferred/unknown labels and model/prompt versions.
- P2-07: Produce feature trace reports from UI entry point toward API/service/data layer.
- P2-08: Add hallucination and provenance validation tests.

Exit criteria:
- Feature summaries cite real evidence.
- LLM output cannot create unverified source references or authorization facts.
- Missing links remain explicitly unknown.
- A reviewer can inspect how a feature summary was derived.

## Phase 3 — Local knowledge retrieval and standalone chatbot
**Goal:** answer questions from the first application's ingested knowledge with citations.

Tasks:
- P3-01: Define semantic chunking and metadata strategy.
- P3-02: Add PostgreSQL full-text search.
- P3-03: Add local multilingual embedding generation and pgvector storage.
- P3-04: Implement hybrid retrieval and verified relationship expansion.
- P3-05: Implement evidence package limits and source/version filtering.
- P3-06: Implement grounded answer generation through Ollama.
- P3-07: Add answer statuses, citations, abstention and uncertainty.
- P3-08: Build minimal standalone chat UI.
- P3-09: Test English, Hindi, Hinglish, code-switching and unsupported questions.

Exit criteria:
- Chat answers from ingested evidence and returns citations.
- It admits when evidence is insufficient.
- Retrieval and generation run locally without paid API credentials.
- No unauthorized host business records are in the knowledge base at this stage.

## Phase 4 — Runtime context, access context and React SDK
**Goal:** deliver passive page context and a secure integration seam without weakening host authorization.

Tasks:
- P4-01: Define runtime observation schema and strict allowlist.
- P4-02: Build framework-neutral TypeScript SDK core.
- P4-03: Add React provider/widget adapter.
- P4-04: Add route/page context collection with opt-out and redaction.
- P4-05: Define host-backend credential exchange and short-lived scoped token validation.
- P4-06: Define Access Context Engine interfaces and trust/provenance model.
- P4-07: Implement one reference host adapter for trusted identity and entitlement claims.
- P4-08: Implement permission and resource-scope checks through host-authoritative adapter calls.
- P4-09: Enforce access filtering before protected evidence retrieval and LLM context construction.
- P4-10: Add tests for full, partial and frontend-only integration modes.
- P4-11: Add access freshness, expiry and invalidation behavior.

Exit criteria:
- SDK collects only allowlisted metadata and never manipulates the UI.
- Browser-provided roles/permissions cannot grant access.
- Unknown/stale/contradictory access fails closed for protected data.
- No protected evidence reaches the LLM before authorization filtering.
- Resource-level checks are delegated to the host authority.

## Phase 5 — Multi-repository application and incremental sync
**Goal:** support multiple repos that together form one application, with consistent versioned snapshots.

Tasks:
- P5-01: Add multiple repository registrations under one application.
- P5-02: Build per-repository version manifest.
- P5-03: Add Git diff-based incremental scan.
- P5-04: Recompute affected reverse dependencies and feature mappings.
- P5-05: Invalidate affected chunks and embeddings.
- P5-06: Build candidate snapshot and publish atomically after validation.
- P5-07: Preserve last successful snapshot on partial/failed update.
- P5-08: Test cross-repository linking only when supported by API contracts/docs/evidence.

Exit criteria:
- Multiple repositories contribute to one application snapshot.
- Snapshot records exact commit for each repository.
- Failed updates do not corrupt the currently published snapshot.
- Incremental output matches a clean full scan for supported fixtures.

## Phase 6 — Evaluation, security hardening and operational readiness
**Goal:** prove quality and safety before wider release.

Tasks:
- P6-01: Create manually verified Smartsevak question set (initial target approximately 100 questions).
- P6-02: Measure extraction/trace accuracy, Recall@K, MRR, answer correctness, citation precision and abstention accuracy.
- P6-03: Measure latency and local resource use on modest hardware.
- P6-04: Add permission-isolation and stale-context test suites.
- P6-05: Add prompt-injection and untrusted-repository-content tests.
- P6-06: Add audit-safe ingestion/chat/access diagnostics.
- P6-07: Add backup, restore and retention/deletion procedures.
- P6-08: Add reproducibility metadata for model, prompt, parser and knowledge snapshot.
- P6-09: Document limitations and release readiness.

Exit criteria:
- Evaluation suite is repeatable.
- Critical authorization leakage tests pass.
- Citation and abstention regressions are detectable.
- Backup/restore and failure recovery are tested.
- Performance claims are based on measured hardware, not assumptions.

## Later, not V1
- Desktop/Tauri packaging.
- Additional language/framework adapters (Python, Java, PHP, Go).
- Optional hosted model providers.
- Computer-use capabilities, only after a separate product/security decision.
- Product demo/video generation.
