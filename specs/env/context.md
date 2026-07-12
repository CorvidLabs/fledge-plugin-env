---
spec: env.spec.md
---

## Context

This capability-free Python plugin uses only project metadata and local file reads to prevent example/environment drift safely in CI.

## Related Modules

- fledge-v1 initialization metadata

## Design Decisions

- Parse only key names to make output safe for CI logs.
- Use exit status as the composable lane gate.
