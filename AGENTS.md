# AGENTS.md

Pure-Harn connector package for Bitbucket Cloud and Data Center.

Shared connector authoring rules live in the Harn guide:

- [Connector authoring guide](https://github.com/burin-labs/harn/blob/main/docs/src/connectors/authoring.md)

Put shared connector guidance in the Harn guide and keep only
provider-specific notes and local hazards here.

`CLAUDE.md` points here. Edit `AGENTS.md` only.

## Provider notes

- Webhook routing keys come from `x-event-key`; delivery ids come from
  `x-hook-uuid` or `x-request-uuid`.
- HMAC verification uses `verify_hmac_signature` from `std/connectors/shared`
  against the raw request body. Both `sha256=<hex>` and bare-hex
  `x-hub-signature` headers are accepted. Unsigned events require an explicit
  `allow_unsigned: true` binding.
- Outbound calls target `https://api.bitbucket.org/2.0` by default. They accept
  OAuth2 tokens, app passwords, or Data Center PATs through call args or
  `BITBUCKET_TOKEN`.
- Outbound rate-limit handling reads `X-RateLimit-Remaining`,
  `X-RateLimit-Reset`, and `Retry-After`. It retries once when the wait is
  within the per-call `rate_limit_max_wait_seconds` budget (default 60).
  `403`, `429`, and `503` responses outside that budget surface as
  `rate_limited`.
- The `pagination.list` method follows Bitbucket Cloud `next` cursor URLs
  through `paginate_cursor` from `std/connectors/shared`. Pass `items_path` or
  `cursor_path` for Data Center responses that page via `nextPageStart`.

<!-- BEGIN HARN SHARED AGENT CONTRACT: managed by harn-bump-fleet -->

## Ecosystem working agreement

- Build ambitious outcomes behind small typed interfaces; give behavior one owner
  and generate or parity-test projections instead of duplicating policy.
- Work autonomously within approved scope. Pause for destructive or production effects,
  exceptional spend, material ambiguity, or new authority.
- Treat stop, wait, stand down, pivot, and steer as control events.
- Use the smallest owning product-path check. Add a falsifier for contested, load-bearing,
  or potentially vacuous claims; record controls, recovery, and blind spots.
- Evidence follows source/artifact identity. Reuse proof when relevant code, build inputs,
  and dependencies are unchanged. Repeat affected checks for relevant changes, failures,
  deployment, or packaging differences. Do not rebuild or recapture solely for main.
- Ship means owning-main integration with terminal merge and applicable release/deploy
  checks. Confirm landed content and result; an open PR is incomplete.
- Use `ship` with a deployed Smart Ship caller; otherwise use `gh pr merge --squash --auto`.
  Never use `--admin`; incidents use `bypass-ci`, `bypass-merge-queue`, or `force-merge`.

<!-- END HARN SHARED AGENT CONTRACT -->

## Pull request titles

Use `[Area] Sentence case summary`, for example
`[Connector] Verify the webhook signature before parsing the payload`. The
summary starts with a capital letter and does not end with a period. See
[CONTRIBUTING.md](CONTRIBUTING.md) for scope rules and the files this
repository does not accept hand edits to.
