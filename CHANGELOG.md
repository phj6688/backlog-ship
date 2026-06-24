# Changelog

## 0.1.1

- `--auto-merge` now wires the sha-pinned merge gate (`block-unverified-merge.sh`,
  claude-homelab): before merging a gated homelab project it runs
  `~/.claude/gate/verify.sh` from the issue's worktree and merges from that same
  checkout, so the gate's `verified_sha == HEAD` check passes instead of blocking
  the merge. The default hand-off path is unaffected (a human UI merge does not
  pass through the hook). (HLB-488)

## 0.1.0 (2026-06-14)

Initial release.

- `/backlog-ship` command: ship a Linear backlog end to end.
- Delegates the build to `orchestrate-linear` with every merge held.
- Gates each PR through an independent in-house review subagent plus CodeRabbit.
- Triage follows the house CodeRabbit loop: fix only reproducible Critical/Major in one round, reject nits with reason, defer anything touching a second file or an auth/proxy/compose path.
- Merge policy: hand cleared PRs off for manual merge by default, `--auto-merge` to merge behind a green gate.
- `--dry-run` for plan-only runs.
- Cleanup of merged branches and worktrees, plus a per-issue report.
