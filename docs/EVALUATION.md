# Evaluation Plan

## Purpose
Measure whether AppGuide understands the application accurately, retrieves relevant evidence, answers with grounded citations and respects access boundaries.

## Initial dataset
Create approximately 100 manually verified questions for Smartsevak CRM. Label expected answer, relevant features, supporting source paths/spans, language, access scope and whether the correct behavior is to abstain.

Question categories:
- Feature discovery and terminology.
- User workflows and prerequisites.
- UI-to-API-to-service implementation tracing.
- Cross-module and cross-repository relationships.
- Entitlements, permissions and resource scope.
- English, Hindi, Hinglish and code-switching.
- Unsupported or ambiguous questions.
- Stale or conflicting evidence.

## Metrics
- Extraction precision/recall for supported symbols, routes and API endpoints.
- Feature trace correctness.
- Retrieval Recall@K and MRR.
- Answer correctness (reviewed against ground truth).
- Citation precision and citation coverage.
- Abstention accuracy for unsupported questions.
- Permission isolation / unauthorized evidence exposure (must be zero in tests).
- End-to-end latency, token throughput and RAM/VRAM/CPU use.

## Evaluation discipline
- Keep a fixed, versioned dataset.
- Record repository commit manifest, parser version, model version, embedding model, prompt version and retrieval settings.
- Use deterministic validators for citations, source paths, schema and access filtering.
- Human review is authoritative for answer correctness; LLM-as-judge is supplementary only.
- Compare changes against a baseline and block critical regressions.
- Do not claim quality from a few hand-picked demonstrations.

## Mandatory security cases
- User A cannot retrieve User B's protected records.
- Tenant A cannot retrieve Tenant B's protected records.
- A role claim supplied only by browser SDK cannot grant access.
- UI-visible feature does not imply permission.
- Stale or expired permission context cannot expose protected data.
- LLM prompt injection cannot override access decisions.
- Frontend-only integration never claims verified authorization.
- No unauthorized record is included in the model prompt, even if the final answer omits it.
