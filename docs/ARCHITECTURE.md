# Architecture — AppGuide

## Architectural style
A modular monolith with independent background workers. Keep clear module boundaries and contracts, but avoid premature microservices.

## High-level components
1. **Dashboard (Next.js + TypeScript):** installation setup, repository configuration, ingestion health, model settings, evaluation and administration.
2. **Backend API (Python + FastAPI):** authentication, application configuration, chat orchestration, SDK endpoints and management APIs.
3. **Intelligence Engine:** source ingestion, deterministic parsing, feature mapping, evidence validation, knowledge persistence, retrieval and answer grounding.
4. **Ingestion workers:** long-running parsing, enrichment, indexing and validation jobs.
5. **Local model runtime:** Ollama behind a provider abstraction; local multilingual embedding model.
6. **SDK:** framework-independent TypeScript core plus React adapter/widget. Passive runtime metadata collection only.
7. **Access Context Engine:** normalizes trusted host identity, entitlement, permission and resource-scope evidence. Authorization policy is enforced by backend code, never by the LLM.
8. **PostgreSQL + pgvector:** canonical records, relationships, conversations, access evidence, audit metadata and vector index.

## Recommended stack
- Dashboard: Next.js, TypeScript.
- Backend: Python, FastAPI, Pydantic.
- Parsing: Tree-sitter plus language-native analyzers; TypeScript Compiler API for JS/TS.
- Database: PostgreSQL with pgvector and full-text search.
- Local inference: Ollama initially, hidden behind provider interfaces.
- Embeddings: local multilingual Sentence Transformers model; benchmark model choices before locking one.
- SDK: TypeScript core and React adapter.
- Packaging: Docker Compose first; desktop/Tauri later.
- Queue: start with a simple durable job approach appropriate to the first deployment; introduce Redis only when concurrency/durability requirements justify it.

## Core subsystems
### Repository ingestion
- Register repositories and pin each scan to a commit.
- Enforce path boundaries, symlink restrictions, read-only mounts and resource limits.
- Never execute project code.
- Full scan first; incremental Git diff scans later.
- Extract files, symbols, imports/exports, routes, API endpoints, handlers, services, models and documentation.

### Feature intelligence
- Group candidate feature entities from deterministic evidence.
- Ask the LLM for constrained structured enrichment using only supplied evidence.
- Validate every claimed source path, symbol and span.
- Persist confidence/provenance and unknowns; reject fabricated edges and citations.
- Trace where evidence supports it: UI route/component → client request → API handler → service/domain logic → data model or external integration.

### Knowledge and retrieval
- PostgreSQL stores canonical entities, edges, chunks, versions and provenance.
- pgvector stores embedding vectors; PostgreSQL FTS supports lexical retrieval.
- Retrieve relevant evidence using hybrid search and verified relationship expansion.
- Apply application/version/authorization filters before protected evidence reaches the LLM.
- Generate concise answers with citations and explicit uncertainty.

### Runtime intelligence
- Runtime observations are separate from static application knowledge.
- SDK sends allowlisted route/page identifiers, page title/headings, approved navigation labels, feature identifiers and selected non-sensitive UI state.
- No raw full DOM, form values, credentials, tokens, message bodies or customer records by default.
- SDK does not click, navigate, submit, crawl or manipulate the UI.
- Backend LLM may interpret observations into candidate feature mappings; these remain inferred/observed, not authorization facts.

### Access Context Engine
- Normalize principal, tenant, subscription entitlements, feature flags, permissions, resource scope and provenance/freshness.
- Support RBAC, ABAC, permission collections, subscription entitlements, ownership/assignment and custom mixed models through adapters.
- Host backend is authoritative. Browser-supplied role/permission values are untrusted hints.
- For protected resource questions, require a trusted authorization decision, scoped data endpoint or safe scope claim.
- Unknown/stale/contradictory access fails closed.
- Access context is time-bounded and refreshed on relevant identity, tenant, permission or session changes.

## Trust classes
- **Trusted authorization evidence:** signed/validated host backend response or server-side adapter result.
- **Observed runtime evidence:** allowlisted SDK observation; useful context, not proof of access.
- **Static code evidence:** proves implementation/capability exists in a source version, not that it is enabled for a tenant/user.
- **LLM inference:** interpretation or hypothesis, never an authorization decision.
- **Unknown:** evidence missing, stale, conflicting or insufficient.

## Deployment boundary
Each AppGuide installation is bound to one host application. Multiple repositories may form one application knowledge snapshot. No cross-app workspace in V1. A production SDK must use a configurable reachable backend URL; browser `localhost` refers to the end user's machine and is not a general production endpoint.

## Ingestion publication
Maintain a manifest of repository commit IDs for a multi-repository application. Build and validate a candidate snapshot, then publish atomically. On failure retain the last successful snapshot and mark the candidate failed/partial/stale; never silently mix incompatible repository versions.
