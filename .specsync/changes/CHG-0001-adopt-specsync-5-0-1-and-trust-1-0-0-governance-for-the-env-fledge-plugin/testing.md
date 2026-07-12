---
change: CHG-0001-adopt-specsync-5-0-1-and-trust-1-0-0-governance-for-the-env-fledge-plugin
artifact: testing
---

# Testing

- `python3 -m py_compile bin/fledge-env`
- `ruff check bin/fledge-env`
- Disposable incomplete and synchronized check/list/diff JSON fixtures
- Confirm no environment values appear in output
- `specsync check --strict --force` at advisory threshold 0
- `fledge trust doctor` and `fledge trust verify`
