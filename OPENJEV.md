# OpenJEV support

This fork adds optional [OpenJEV](https://openjev.sh) support alongside the original TypeSafe provider. TypeSafe remains the default; OpenJEV is opt-in.

## What was added

- `internal/cli/config.go` — new `openjev` built-in provider profile (base URL `https://api.openjev.sh`, model `openjev`, key env `OPENJEV_API_KEY`); `openjev` added to reserved provider names so user config cannot shadow it.
- `internal/infra/typesafeapi/client.go` — HTTP 503 (Service Unavailable) added to retryable statuses alongside 429 and 529, both in the retry loop and in `classifyStatus`.
- `docs/guides/providers.md` — documentation section for the OpenJEV provider.
- `README.md` — short OpenJEV note after the project intro.

## Provider selection rule

1. Explicit choice wins: `JEQ_PROVIDER=openjev` or `default_provider: "openjev"` in config.
2. Otherwise, if `TYPESAFE_API_KEY` is set → TypeSafe (unchanged default).
3. Otherwise, if only `OPENJEV_API_KEY` is set → OpenJEV is still opt-in (use `JEQ_PROVIDER=openjev`).

Anyone with a TypeSafe key sees zero behaviour change.

## Configuration

```sh
export OPENJEV_API_KEY='your-openjev-key'
JEQ_PROVIDER=openjev jeq models
```

Get a key from https://openjev.sh/dashboard.

## Verification

- A live POST to `https://api.openjev.sh/v1/systemone` with model `openjev`, state `ping`, one noul question returned HTTP 200.
- `grep -r 'api.typesafe.ai'` confirms no hardcoded TypeSafe default was removed; TypeSafe remains the default provider.

## Upstream

Original project: https://github.com/cristianoliveira/jeq by @cristianoliveira.
