# SKILL: harn-bitbucket-connector

Use `harn-bitbucket-connector` when wiring Harn triggers or outbound helpers for Bitbucket.

## What you get

- Provider id: `bitbucket`
- Trigger kinds: `webhook`
- Supported events: `repo:push, pullrequest:created, pullrequest:updated, pullrequest:fulfilled, pullrequest:rejected, issue:created, issue:updated, repo:commit_status_created, repo:commit_status_updated`
- Webhook verification: `bitbucket_hmac`
- Outbound helpers: `api.request`, `pull_requests.comment`, `pull_requests.update`, `issues.comment`, `commit_status.set`, `repository_file.get`

## Trigger recipe

```toml
[[triggers]]
id = "bitbucket-events"
kind = "webhook"
provider = "bitbucket"
match = { path = "/hooks/bitbucket", events = ["pullrequest.created"] }
handler = "handlers::on_bitbucket_event"
secrets = { signing_secret = "bitbucket/signing-secret" }
```

For SourceHut, use a configured `public_key` secret instead of `signing_secret`.
For SVN polling, add a `poll` trigger and run `harn connector check . --provider svn --run-poll-tick`.
