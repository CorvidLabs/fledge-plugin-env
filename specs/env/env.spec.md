---
module: env
version: 2
status: active
files:
  - bin/fledge-env

db_tables: []
depends_on: []
---

# Env

## Purpose

Read environment-file keys without exposing values, compare a project `.env` with its example/template/sample, list defined keys, report missing/extra keys, and provide stable human and JSON output for CI gates.

## Public API

| Action | Behavior |
|--------|----------|
| check | Report example keys absent from the active environment file; this is the default. |
| list | List key names defined by the environment file without values. |
| diff | Report keys missing from and extra relative to the example. |
| JSON | Emit schema-versioned action, paths, key lists/counts, and status fields. |

## Invariants

1. Environment values are never included in output; only key names and file paths are reported.
2. Check exits 1 when any example key is missing and 0 when complete.
3. Diff exits 1 for any missing or extra key and 0 only when synchronized.
4. List always succeeds for a readable or absent environment file and returns only keys.
5. Explicit file options override automatic example detection.
6. Automatic example detection uses example, template, then sample precedence.
7. Parsing ignores blank lines and comments while accepting exported assignments.

## Behavioral Examples

```
Given an example with `DATABASE_URL` and `REDIS_URL` and an environment containing only `DATABASE_URL`
When check runs
Then it reports only `REDIS_URL` as missing, never prints the database value, and exits 1
```

## Error Cases

| Error | When | Behavior |
|-------|------|----------|
| Missing example file | No explicit or detected example exists | Report the absence without exposing environment values. |
| Missing environment file | Check or diff has no active file | Treat all example keys as missing. |
| Malformed init or options | Required project/argument data cannot be parsed | Report usage/protocol failure and exit non-zero. |
| Unsynchronized files | Missing or extra keys exist | Report key names and exit 1. |

## Dependencies

- Python 3.11 or later standard library
- fledge-v1 project metadata delivered in the initialization message

## Change Log

| Version | Date | Changes |
|---------|------|---------|
| 1 | 2026-07-12 | Document existing environment key comparison and privacy behavior for SpecSync 5 adoption. |
| 2 | 2026-07-13 | Reconciled existing privacy documentation and stable requirement IDs for SpecSync 5.0.1 governance; runtime behavior is unchanged. |
| 2026-07-13 | CHG-0001-adopt-specsync-5-0-1-and-trust-1-0-0-governance-for-the-env-fledge-plugin: Adopt SpecSync 5.0.1 and Trust 1.0.0 governance for the Env Fledge plugin |
