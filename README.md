# harn-bitbucket-connector

Pure-Harn Bitbucket connector: Cloud/Data Center webhooks, HMAC
verification, and REST dispatch.

This package implements the Harn Connector interface contract v1 for `bitbucket`.
It normalizes inbound webhook payloads to the tagged `NormalizeResult` envelope,
verifies provider-specific webhook signatures, and exposes outbound raw API helpers
plus common PR/comment/status method aliases.

## Install

```sh
harn add github.com/burin-labs/harn-bitbucket-connector@v0.1.0
```

Use a path checkout for unreleased `main` or local multi-repo development:

```toml
[dependencies]
harn-bitbucket-connector = { path = "../harn-bitbucket-connector" }
```

## Webhook verification

Bitbucket webhooks must be signed unless a binding explicitly sets
`allow_unsigned: true`. Configure `signing_secret` or
`bitbucket/webhook-secret`; the connector verifies the `x-hub-signature`
HMAC-SHA256 header against the raw request body and rejects missing or invalid
signatures with `401`.

## Authentication

Outbound calls use app password, OAuth2 token, or Data Center PAT. Pass
`access_token`, `token`, `personal_access_token`, or `app_password` in the call
args, or set the `BITBUCKET_TOKEN` environment variable.

## Development

```sh
harn check src/lib.harn
harn fmt --check src/lib.harn tests/*.harn
for test in tests/*.harn; do harn run "$test" || exit 1; done
```

## License

Dual-licensed under MIT and Apache-2.0.
