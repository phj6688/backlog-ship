# backlog-ship

*Hand a whole Linear backlog to Claude Code and get back merge-ready PRs, each cleared by two independent reviewers, with nothing merged behind your back.*

`backlog-ship` is a superset of [`orchestrate-linear`](https://github.com/phj6688/orchestrate-linear). That plugin builds a backlog and auto-merges with no external review gate. This one reuses its build engine, holds every merge, and adds the review gate, merge policy, and cleanup it lacks.

## Install

In Claude Code, add the marketplace then install both plugins (the build engine is a dependency):

```
/plugin marketplace add phj6688/claude-marketplace
/plugin install orchestrate-linear@phj
/plugin install backlog-ship@phj
```

## Use

In any repo that maps to a Linear project:

```
/backlog-ship
```

Or point it at a project, a path, or an epic, from anywhere:

```
/backlog-ship war-room
/backlog-ship ABC-64
/backlog-ship "Full-Text Search"
```

Flags:

- `--auto-merge` opt in to merging cleared PRs. Off by default: a cleared PR is handed back for you to click-merge.
- `--dry-run` intake and plan only, no branches, PRs, or merges.

With no argument it infers the Linear project from the repo and scopes to the not-started issues (`Backlog` / `Todo`).

## What it does

1. **Intake.** Pulls the not-started issues, expands epics into child stories, and checks for work already sitting on a branch.
2. **Build.** Delegates to `orchestrate-linear` (plan, TDD implementation in isolated worktrees, in-house review, verification, loop until green), with every merge held.
3. **Gate.** For each green branch it opens a PR into the integration branch and clears it through two independent reviewers: a fresh in-house review subagent that sees only the diff and the issue (so it carries no implementer bias), plus CodeRabbit.
4. **Triage.** Classifies findings as fix / reject-with-reason / defer, auto-applies only reproducible Critical and Major in a single round, and aborts to a human hand-off the moment a fix reaches a second file or any auth, proxy, or compose path.
5. **Ship.** Hands every cleared PR back for a manual click-merge by default, or merges it with `--auto-merge` only when CI is green, CodeRabbit has no open Critical or Major, the in-house reviewer approved, and no sensitive path was touched.
6. **Clean up and report.** Deletes merged branches and worktrees, leaves everything else for you, posts one summary comment per PR, and prints a per-issue table.

## Requirements

- [`orchestrate-linear@phj`](https://github.com/phj6688/orchestrate-linear) for the build engine.
- The Linear MCP server, connected to your workspace.
- `gh` authenticated, and a GitHub `origin` remote on the target repo.
- CodeRabbit, either the GitHub App on the repo or the `coderabbit` CLI signed in for the local-review fallback.

## License

MIT, see [LICENSE](LICENSE).
