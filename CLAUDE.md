# CLAUDE.md - harn-bitbucket-connector

Pure-Harn connector package for Bitbucket Cloud and Data Center.

Shared Harn connector authoring rules live in the canonical guide:

- https://github.com/burin-labs/harn/blob/main/docs/src/connectors/authoring.md

Keep this file limited to provider-specific notes and local hazards. Add shared connector guidance
to the Harn guide first.

## Provider notes

- Webhook routing keys come from `x-event-key`; delivery ids come from `x-hook-uuid` or `x-request-
  uuid`.
- HMAC verification uses `verify_hmac_signature` from `std/connectors/shared`
  against the raw request body. Both `sha256=<hex>` and bare-hex
  `x-hub-signature` headers are accepted. Unsigned events require an explicit
  `allow_unsigned: true` binding.
- Outbound calls target `https://api.bitbucket.org/2.0` by default and accept OAuth2 tokens, app
  passwords, or Data Center PATs through call args or `BITBUCKET_TOKEN`.
- Outbound rate-limit handling reads `X-RateLimit-Remaining`/`X-RateLimit-Reset` and `Retry-After`
  and retries once when the wait is within the per-call `rate_limit_max_wait_seconds` budget
  (default 60). `403`/`429`/`503` responses outside the budget surface as `rate_limited`.
- The `pagination.list` outbound method walks Bitbucket Cloud's `next` cursor URLs via the
  `paginate_cursor` helper from `std/connectors/shared`; pass `items_path` / `cursor_path` to
  override for Data Center responses that page via `nextPageStart`.
