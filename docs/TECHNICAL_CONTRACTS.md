# Initial Technical Contracts — AppGuide

These are starting contracts, not permission to freeze every implementation detail. Keep them versioned and revise through `DECISIONS.md`.

## Application boundary
- One installation has one `application_id`.
- Every repository registration belongs to that application.
- Every knowledge snapshot records the exact commit for each registered repository.
- Queries must resolve to the installation's application; no caller-supplied cross-app override.

## Initial HTTP API
| Method | Path | Purpose |
|---|---|---|
| POST | `/v1/ingestion-runs` | Start a scan for registered repositories |
| GET | `/v1/ingestion-runs/{run_id}` | Read stage, counts, errors and status |
| GET | `/v1/knowledge/features` | List feature summaries for authorized admin |
| POST | `/v1/chat/sessions` | Create a conversation session |
| POST | `/v1/chat/sessions/{session_id}/messages` | Submit a question and receive grounded answer |
| POST | `/v1/evaluations/runs` | Run a selected evaluation suite |
| GET | `/v1/health` | Liveness/readiness information |

All endpoints require explicit authentication/authorization design before production exposure. Do not treat this table as a complete security specification.

## Chat response shape (conceptual)
- `application_id`
- `knowledge_snapshot_id`
- `answer`
- `answer_status`: `verified | inferred | unknown`
- `citations[]`: repository ID, commit ID, path, start/end line or symbol ID, evidence ID
- `warnings[]`: stale knowledge, partial integration, unknown access dimension, etc.
- `trace_id`

Do not return protected evidence or sensitive access details to a caller merely because the model generated them.

## Access context (conceptual)
Represent dimensions independently:
- principal identity and tenant
- organization entitlements
- user grants/denials and attributes
- resource ownership/assignment/scope
- provenance and verification method
- observed-at, expiry and source version
- policy decision: `allow | deny | unknown`, with reason code and freshness

The host adapter must define which dimensions are authoritative and how policy decisions are checked. Do not invent a universal policy language prematurely.

## Integration modes
1. **Full integration:** trusted host backend provides identity, entitlements, permissions and resource authorization capability.
2. **Partial integration:** only some dimensions are trusted; unknown dimensions remain unknown and cannot be guessed.
3. **Frontend-only:** runtime UI observations may provide page context, but authorization remains unverified. No protected resource data may be disclosed.

## SDK contract principles
- Browser SDK receives a short-lived scoped credential from the host backend.
- Backend validates the credential; no backend secret is shipped to the browser.
- Runtime context payload is schema-validated, allowlisted and size-limited.
- SDK observations are hints, not grants.
- Host application can disable collection or restrict fields.
- SDK does not manipulate the UI.

## Ingestion job lifecycle
`queued → scanning → parsing → linking → enriching → indexing → validating → published`
Terminal alternatives: `failed`, `partial`. Persist stage timestamps, processed counts, errors and coverage. Publish only after validation.

## Data model candidates
`applications`, `repositories`, `repository_versions`, `knowledge_snapshots`, `ingestion_runs`, `code_entities`, `code_relationships`, `features`, `knowledge_chunks`, `embeddings`, `conversations`, `messages`, `runtime_observations`, `access_context_evidence`, `access_decisions`, `evaluation_runs`, `audit_events`.

Schema design should follow actual query patterns and migration tests. Avoid creating every table before the first vertical slice.
