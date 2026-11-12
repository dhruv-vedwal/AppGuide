# Decision Register

| ID | Decision | Status | Rationale / constraint |
|---|---|---|---|
| D-001 | One application per installation | Accepted | Prevent cross-application knowledge mixing in V1. |
| D-002 | Multiple repositories may belong to one application | Accepted | Supports frontend/backend/service architecture. |
| D-003 | V1 is chatbot-only | Accepted | No computer use, UI manipulation, active crawling or video generation. |
| D-004 | Local-first; no required paid AI API | Accepted | Core ingestion, embeddings and inference should run locally. |
| D-005 | Modular monolith plus workers | Recommended | Avoid premature distributed-system complexity. |
| D-006 | PostgreSQL canonical store; pgvector index | Recommended | Keep structured facts, provenance and vectors together. |
| D-007 | Deterministic extraction before LLM enrichment | Accepted | LLM must not invent source facts or authorization. |
| D-008 | JS/TS first | Accepted | Bound initial parser scope; add adapters later. |
| D-009 | Ollama initial local inference provider | Recommended | Keep provider-neutral abstraction. |
| D-010 | Application and runtime intelligence are separate | Accepted | Code capability and current runtime state are different evidence. |
| D-011 | Access Context Engine is a first-class subsystem | Accepted | User and tenant access changes what can be safely explained or retrieved. |
| D-012 | Host application is authorization authority | Accepted | AppGuide must not infer or override host policy. |
| D-013 | Frontend SDK collection is deterministic and passive | Accepted | No browser-side LLM; no UI manipulation. |
| D-014 | UI visibility is not proof of authorization | Accepted | DOM observations can be incomplete or manipulated. |
| D-015 | Protected evidence is filtered before LLM context | Accepted | Prompt instructions are not a security boundary. |
| D-016 | Unknown/stale access fails closed for protected data | Accepted | Prevent leakage when access cannot be verified. |
| D-017 | Docker Compose before desktop packaging | Accepted | Simplify first installation and validation. |
| D-018 | Do not add graph DB, orchestration framework or Redis prematurely | Recommended | Add only after measurable need. |

## Open decisions
- Exact local LLM and embedding model/version.
- Host backend integration protocol, token format, expiry and refresh.
- Canonical permission representation and policy adapter interface.
- Runtime observation schema and framework adapters.
- Resource-level authorization contract.
- Permission freshness and invalidation triggers.
- Performance targets and minimum supported hardware.
- License review for source, dependencies, models and datasets.
