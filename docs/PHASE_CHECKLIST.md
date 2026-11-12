# Phase Checklist

Use this as a gate, not a list of aspirations. Mark an item complete only with evidence (test, command output, reviewed artifact or reproducible manual check).

## Phase 0 — Foundation
- [ ] Clean clone startup documented and verified.
- [ ] Docker Compose services start successfully.
- [ ] Database migrations run from empty database.
- [ ] Health/readiness checks work.
- [ ] Lint, format, type-check and tests run locally.
- [ ] CI passes.
- [ ] Secrets are excluded from repository and logs.
- [ ] Repository ingestion cannot execute project code.

## Phase 1 — Deterministic ingestion
- [ ] Safe file discovery and ignore rules.
- [ ] Path traversal and symlink tests.
- [ ] Commit/version recorded.
- [ ] JS/TS symbols and source spans extracted.
- [ ] Supported routes/API handlers extracted.
- [ ] Relationships carry evidence.
- [ ] Unsupported dynamic patterns reported as unknown.
- [ ] Fixture tests pass.
- [ ] Ingestion report exposes coverage and errors.

## Phase 2 — Feature intelligence
- [ ] Candidate feature grouping is inspectable.
- [ ] Structured LLM output schema enforced.
- [ ] Source references validated.
- [ ] Verified/inferred/unknown status persisted.
- [ ] Model and prompt versions recorded.
- [ ] No fabricated citations or relationships pass validation.

## Phase 3 — Chat and retrieval
- [ ] FTS retrieval works.
- [ ] Local embeddings and pgvector work.
- [ ] Hybrid ranking evaluated.
- [ ] Evidence is version-filtered.
- [ ] Answer citations resolve to source spans.
- [ ] Insufficient evidence causes abstention.
- [ ] English/Hindi/Hinglish tests exist.
- [ ] No paid API is required for core flow.

## Phase 4 — SDK and access context
- [ ] SDK allowlist and redaction tests pass.
- [ ] No arbitrary DOM/full HTML collection.
- [ ] No UI manipulation code exists in SDK.
- [ ] Short-lived scoped credentials validated server-side.
- [ ] Browser-provided role/permission values cannot authorize.
- [ ] Full/partial/frontend-only modes behave correctly.
- [ ] Access decisions happen outside the LLM.
- [ ] Protected retrieval is filtered before model context.
- [ ] Stale/unknown/contradictory access fails closed.
- [ ] Resource-level authorization uses host authority.

## Phase 5 — Multi-repository sync
- [ ] Multiple repositories belong to one app only.
- [ ] Commit manifest recorded per repository.
- [ ] Incremental updates identify affected dependencies.
- [ ] Candidate snapshots validate before publication.
- [ ] Last good snapshot survives failed updates.
- [ ] Incremental/full-scan consistency tests pass.

## Phase 6 — Quality and release
- [ ] Curated question set reviewed.
- [ ] Retrieval and answer metrics measured.
- [ ] Citation and abstention metrics measured.
- [ ] Permission isolation tests pass.
- [ ] Prompt injection tests pass.
- [ ] Backup/restore tested.
- [ ] Retention/deletion documented and tested.
- [ ] Local performance measured on target hardware.
- [ ] Known limitations documented.
