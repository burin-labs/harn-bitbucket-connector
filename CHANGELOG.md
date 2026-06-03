# Changelog

## v0.2.0

- Surface PR merge/conflict state on `pullrequest:*` events. The
  normalized payload gains a `merge` block
  (`{merge_state, merge_commit, is_conflict?}`): `merge_state` mirrors the
  Bitbucket PR `state` (open/merged/declined/superseded), `merge_commit`
  carries the fulfilled merge hash when present, and `is_conflict` is set
  only when the payload exposes an explicit hint (Data Center
  `properties.mergeResult.outcome == "CONFLICTED"` or a boolean
  `has_conflicts`/`conflicted` flag); otherwise it is omitted so callers
  can fall back to an API probe.
- `@`-mention command extraction on PR-comment and issue-comment events.
  The normalized payload gains a `mention` block
  (`{actor, candidates: [{handle, command, rest}], target_kind,
  target_id, url}`) parsed from the comment body with CPU-only string
  scanning. Comments without a leading `@handle command` mention omit the
  block.
- Accept `pullrequest:comment_created`, `pullrequest:comment_updated`,
  and `issue:comment_created` webhook events (previously rejected as
  unsupported) so comment mentions can be normalized.

## v0.1.0

- Initial pre-alpha Bitbucket connector package implementing Harn Connector contract v1.
