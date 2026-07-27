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

- Pursue the ambitious product outcome; make the seams boring with small typed
  interfaces, explicit invariants, and deterministic projections.
- Give each behavior one semantic owner. Generate or parity-test other surfaces
  instead of maintaining competing implementations.
- Work autonomously inside approved scope. Pause for destructive, production,
  high-spend, ambiguous, or authority-expanding actions—not routine reversible work.
- Treat stop, wait, stand down, and pivot as control events for long-lived work.
- Match evidence to the claim: exercise the canonical user path, state the
  falsifier, verify liveness and recovery, and record residual blind spots.
- "Ship" means landed on main with required deploy and post-merge checks complete.

<!-- END HARN SHARED AGENT CONTRACT -->
