---
spec: env.spec.md
---

## User Stories

- As a developer, I want to detect missing or stale environment keys before committing changes.
- As a CI author, I want machine-readable results without leaking secret values.

## Acceptance Criteria

### REQ-env-001

The plugin SHALL compare environment key names against an explicit or detected example file.

### REQ-env-002

Check and diff SHALL exit non-zero when their key sets are not satisfied or synchronized.

### REQ-env-003

List and all JSON/human output SHALL expose key names only and never environment values.

Acceptance Criteria
- Existing list, JSON, and privacy smokes confirm values remain redacted without changing runtime behavior.

### REQ-env-004

JSON mode SHALL report schema version, action, file paths, relevant key lists/counts, and status.

## Constraints

- Reads dotenv-style assignments but does not evaluate shell expansion or load values into the process.

## Out of Scope

- Secret-value validation, dotenv interpolation, and environment mutation.
