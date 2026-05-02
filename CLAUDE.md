# CLAUDE.md - harn-bitbucket-connector

Pure-Harn connector package for Bitbucket Cloud and Data Center.

Shared Harn connector authoring rules live in the canonical guide:

- https://github.com/burin-labs/harn/blob/main/docs/src/connectors/authoring.md

Keep this file limited to provider-specific notes and local hazards. Add shared connector guidance
to the Harn guide first.

## Provider Notes

- Webhook routing keys come from `x-event-key`; delivery ids come from `x-hook-uuid` or `x-request-
  uuid`.
- When a signing secret is configured, verify `x-hub-signature` against the raw request body with
  the Bitbucket HMAC scheme. Unsigned events are only valid when no verification material is
  configured.
- Outbound calls target `https://api.bitbucket.org/2.0` by default and accept OAuth2 tokens, app
  passwords, or Data Center PATs through call args or `BITBUCKET_TOKEN`.
