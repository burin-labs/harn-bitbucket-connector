# SKILL: harn-bitbucket-connector

Use `harn-bitbucket-connector` when wiring Harn triggers or outbound helpers
for Bitbucket.

## What you get

- Provider id: `bitbucket`
- Trigger kinds: `webhook`
- Supported events: `repo:push`, `pullrequest:created`,
  `pullrequest:updated`, `pullrequest:fulfilled`, `pullrequest:rejected`,
  `issue:created`, `issue:updated`, `repo:commit_status_created`, and
  `repo:commit_status_updated`
- Webhook verification: `bitbucket_hmac`
- Outbound helpers: `api.request`, `pull_requests.comment`,
  `pull_requests.update`, `issues.comment`, `commit_status.set`, and
  `repository_file.get`

## Trigger recipe

```toml
[[triggers]]
id = "bitbucket-events"
kind = "webhook"
provider = "bitbucket"
match = { path = "/hooks/bitbucket", events = ["pullrequest.created"] }
handler = "handlers::on_bitbucket_event"
secrets = { signing_secret = "bitbucket/webhook-secret" }
```

Configure `signing_secret` for the HMAC-protected route. Unsigned delivery is
an explicit binding opt-in through `allow_unsigned: true`.

Validate the package with `harn connector test . --provider bitbucket`.
