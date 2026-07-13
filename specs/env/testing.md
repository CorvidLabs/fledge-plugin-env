---
spec: env.spec.md
---

## Test Plan

### Integration Tests

- `python3 -m py_compile bin/fledge-env`
- `ruff check bin/fledge-env`
- Exercise check with absent and incomplete examples, list, synchronized diff, and valid JSON output using temporary fixtures.
