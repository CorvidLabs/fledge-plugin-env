# fledge-plugin-env

Check, list, and diff project `.env` files against `.env.example`. Catches the "I added a new env var and forgot to update the example" class of bug.

Reference plugin demonstrating the [fledge-v1 plugin protocol](https://corvidlabs.github.io/fledge/plugin-protocol.html) with **no declared capabilities** — uses only the project info delivered in the `init` message.

## Install

```bash
fledge plugins install CorvidLabs/fledge-plugin-env
```

## Usage

```bash
fledge env                    # check (default): list keys missing from .env
fledge env check              # same
fledge env list               # list keys defined in .env (values are never printed)
fledge env diff               # show keys in one but not the other
fledge env --json             # machine-readable
```

Override the file paths:

```bash
fledge env check --example .env.template --env .env.local
```

## Exit codes

| Subcommand | 0 | 1 |
|------------|---|---|
| `check` | All example vars are set in `.env` | One or more are missing |
| `diff` | Files are in sync | Any keys differ |
| `list` | Always | Never |

This makes it composable in lanes:

```toml
[lanes.pre-commit]
steps = [
  { parallel = ["fmt", "lint"] },
  "test",
  { run = "fledge env check" },
]
```

## JSON output

`fledge env check --json`:

```json
{
  "schema_version": 1,
  "action": "env_check",
  "example_file": ".env.example",
  "env_file": ".env",
  "missing": ["DATABASE_URL", "REDIS_URL"],
  "missing_count": 2,
  "ok": false
}
```

`fledge env list --json`:

```json
{
  "schema_version": 1,
  "action": "env_list",
  "env_file": ".env",
  "keys": ["DATABASE_URL", "PORT", "LOG_LEVEL"],
  "count": 3
}
```

`fledge env diff --json`:

```json
{
  "schema_version": 1,
  "action": "env_diff",
  "example_file": ".env.example",
  "env_file": ".env",
  "missing": ["NEW_VAR"],
  "extra": ["LEGACY_VAR"],
  "in_sync": false
}
```

## Privacy

This plugin **only reads keys**, never values. Use it freely in CI without worrying about leaking secrets — the names of your environment variables are not secrets, the values are.

## Detected files

`.env` (configurable with `--env`) and the first match from `.env.example`, `.env.template`, `.env.sample` (configurable with `--example`).

## License

MIT
