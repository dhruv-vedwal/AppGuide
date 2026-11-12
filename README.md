# AppGuide

AppGuide is a local-first application intelligence platform. It builds an evidence-backed understanding of one host application per installation and exposes that knowledge through a multilingual chatbot.

## V1 boundary

- One application per AppGuide installation.
- Multiple repositories may contribute to that one application.
- V1 is a chatbot for explaining application capabilities, features and workflows.
- No computer use, browser control, autonomous clicking, form submission, active UI exploration or video generation.
- The host application remains the authority for authentication and authorization.
- Protected data must never be retrieved or sent to an LLM unless access is verified.
- Frontend SDK observations are passive, allowlisted and untrusted as authorization evidence.
- Local inference is the default; no paid API or external LLM call is required.

## Source-of-truth documents

- `docs/ARCHITECTURE.md` — system boundaries and component responsibilities.
- `docs/PRODUCT_REQUIREMENTS.md` — V1 requirements and non-goals.
- `docs/IMPLEMENTATION_ROADMAP.md` — phased implementation plan and task IDs.
- `docs/PHASE_CHECKLIST.md` — phase exit criteria.
- `docs/PROJECT_STATUS.md` — current state and next task.
- `docs/DECISIONS.md` — decision register.
- `docs/TECHNICAL_CONTRACTS.md` — initial API and internal contracts.
- `docs/EVALUATION.md` — quality evaluation plan.
- `docs/SECURITY.md` — security and privacy requirements.
- `AGENTS.md` — instructions for coding agents.

## Development principle

Build the smallest end-to-end slice first: ingest one JavaScript/TypeScript repository, produce evidence-backed feature knowledge, retrieve it locally, and answer questions with citations. Expand only after tests demonstrate the current slice works.
