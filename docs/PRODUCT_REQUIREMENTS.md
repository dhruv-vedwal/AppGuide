# Product Requirements — AppGuide V1

## Product statement
AppGuide builds an evidence-backed model of one host application per installation and provides a multilingual assistant that answers questions about that application's capabilities and workflows.

## Users
- Developers who install and configure AppGuide for an application.
- End users of that host application who ask how features work and what workflows are available.
- Administrators responsible for configuring repositories, integrations, model settings and access policies.

## V1 functional requirements
1. Configure one canonical host application identity per installation.
2. Register one or more repositories belonging to that application.
3. Ingest JavaScript/TypeScript source and supported documentation without executing repository code.
4. Extract symbols, imports, routes, API endpoints and verifiable relationships with source provenance.
5. Group extracted entities into candidate features and produce constrained, evidence-backed descriptions.
6. Store versioned knowledge in PostgreSQL; store embeddings in pgvector.
7. Retrieve using keyword search, vector search and verified graph-like relationship expansion.
8. Answer questions through a standalone chat interface and later an embeddable SDK widget.
9. Provide source citations and distinguish verified, inferred and unknown information.
10. Support English, Hindi, Hinglish and code-switching through evaluation rather than assuming translation-first behavior.
11. Accept passive, allowlisted runtime context from the SDK.
12. Support trusted host-backend integration for identity, organization entitlements, user permissions and resource-level authorization evidence.
13. Enforce access controls outside the LLM and before protected evidence retrieval.
14. Record ingestion runs, coverage, errors, model/version metadata and evaluation results.
15. Support incremental Git-based synchronization after the initial full scan.

## Non-functional requirements
- Local-first: core ingestion, embeddings and inference can operate without paid AI API credits.
- Reproducible: persist commit/version and model/configuration metadata for each published knowledge snapshot.
- Explainable: answers cite repository, commit, path and source span where possible.
- Secure by default: fail closed for protected data if authorization is unknown, stale or contradictory.
- Recoverable: failed ingestion must not replace the last known-good published snapshot.
- Observable: expose stage status, timings, counts, errors and coverage.
- Extensible: parser and host-authorization adapters must not encode one application's business rules in core.

## Explicit V1 non-goals
- Browser/computer-use agent or autonomous interaction.
- Active website crawling or UI exploration.
- Clicking, filling, submitting, navigating or manipulating host UI.
- Video generation, narration or product-demo recording.
- Cross-application knowledge search or app switching.
- Direct access to the host application's database.
- Arbitrary execution of repository code.
- Required cloud LLM, paid API or external telemetry.
- Universal language/framework support at launch.

## Initial validation target
Smartsevak CRM is the first validation application. Its frontend and relevant backend/service repositories may be registered as repositories belonging to one AppGuide application. Product-specific permission names and navigation labels must not be hardcoded into the engine.
