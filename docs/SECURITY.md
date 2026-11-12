# Security and Privacy Requirements

## Trust boundaries
- Host application backend: authority for identity and authorization.
- AppGuide backend: validates credentials, enforces access policy and controls retrieval.
- Browser SDK: untrusted observation source.
- Repository content and documentation: untrusted input.
- Local LLM: probabilistic processor, never a security authority.

## Repository ingestion
- Never execute repository code, install repository dependencies or run project scripts.
- Use read-only mounts, path allowlists, traversal/symlink protection, file-size limits and resource limits.
- Treat source comments, docs and embedded prompts as untrusted data.
- No network access from ingestion workers by default.

## Runtime SDK
- Collect only explicit allowlisted metadata.
- Redact and exclude secrets, tokens, passwords, form values, message bodies and customer records by default.
- No arbitrary full DOM/HTML collection.
- No click, fill, submit, navigation, crawl or other UI manipulation.
- Provide host-controlled opt-out and configurable collection policy.
- Browser fields describing roles/permissions are hints only.

## Authorization
- Authenticate the principal and resolve the one application installation.
- Verify host-issued short-lived scoped credentials server-side.
- Apply tenant, user, entitlement and resource-scope checks before protected retrieval.
- Recheck sensitive/live resource access when required by host policy or freshness.
- Unknown, stale, contradictory or unverified access fails closed.
- Never pass unauthorized evidence to the LLM.
- Never rely on prompt text to enforce authorization.
- Avoid revealing protected feature existence if host policy considers existence confidential.

## Secrets and logs
- Do not commit secrets or expose backend credentials in SDK bundles.
- Do not log tokens, credentials, raw customer data or sensitive payloads.
- Use trace IDs and privacy-safe metadata for diagnostics.
- No external telemetry or external LLM fallback by default.

## Operations
- Define retention, export and deletion behavior.
- Protect backups and test restore.
- Record security-relevant access decisions without storing unnecessary sensitive content.
- Document dependency and model license/security review before distribution.
