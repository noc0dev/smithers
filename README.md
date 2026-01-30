# Smithers

<p align="center">
  <img src="header.png" alt="Smithers" width="400">
</p>

Autonomous parallel implementation skill for coding agents. Dispatches workers to implement tasks in isolated git worktrees, creates PRs, handles automated review cycles, and only presents polished PRs for human review.

Whereas Ralph is depth-first, Smithers is breadth-first.

## What It Does

```
┌─────────────┐     ┌─────────────┐     ┌─────────────┐
│  Fetch N    │     │   Create    │     │  Dispatch   │
│   ready     │────▶│  worktrees  │────▶│  parallel   │
│   beads     │     │  (isolated) │     │  workers    │
└─────────────┘     └─────────────┘     └─────────────┘
                                               │
       ┌───────────────────────────────────────┘
       ▼
┌─────────────┐     ┌─────────────┐     ┌─────────────┐
│   Create    │     │ Poll every  │     │   Present   │
│   PRs +     │────▶│    60s      │────▶│  polished   │
│   reviews   │     │  until clean│     │    PRs      │
└─────────────┘     └─────────────┘     └─────────────┘
```

**Why use this**
You are tired of telling Claude Code: "PR review failed, plz fix". You want to move _fast_ but _safely_. You only want to look at the PR once CI and automated PR reviews pass. But you do want to look at PRs before merging. Smithers is obsequious, but not stupid.

This skill assumes you use [beads](https://github.com/steveyegge/beads) for your project as an issue tracker and have automated PR review agents like roborev, CodeRabbit, Codex, etc.

## Features

- **Parallel execution**: Multiple beads implemented simultaneously in isolated worktrees
- **Automated review handling**: Integrates with Codex, CodeRabbit, and roborev
- **Thread resolution**: Workers resolve PR conversation threads after addressing comments
- **CI verification**: PRs only presented after all checks pass
- **Human gating**: Confirms task selection before dispatch, presents final PRs for approval

## Installation

### Via skills.sh

```bash
npx skills add noc0/smithers
```

After installation, the skill will prompt you to copy the `smithers-worker` agent definition to `~/.claude/agents/smithers-worker.md`. This is required for parallel autonomous execution.

### Manual

1. Copy `SKILL.md` to `~/.claude/skills/smithers/SKILL.md`
2. Follow the Prerequisites section in SKILL.md to install the worker agent

## Requirements

- [beads](https://github.com/steveyegge/beads) - Git-backed issue tracker (`bd` CLI)
- [gh](https://cli.github.com/) - GitHub CLI
- Git worktree support
- Claude Code with Task tool support

### Optional

- [roborev](https://github.com/wesm/roborev) - Automated code review for AI-generated commits
- Codex or CodeRabbit configured at org level

## Usage

When you have ready beads (tasks with no blockers):

```
/smithers
```

### Workflow

1. **Fetch**: Gets ready beads without gates (no needs-spec, no needs-verification)
2. **Confirm**: Shows task list, waits for your approval
3. **Isolate**: Creates git worktree per bead
4. **Dispatch**: Launches parallel smithers-worker subagents
5. **PR**: Each worker implements, creates PR
6. **Poll**: Checks all PRs every 60 seconds for CI + review status
7. **Fix**: Dispatches workers to address review comments and resolve threads
8. **Present**: Shows PR links only when ALL pass CI and have no unresolved threads
9. **Cleanup**: Removes worktrees after merge, closes beads

## Iron Laws

1. Human confirms task selection before dispatch
2. One worktree per bead - no shared git state
3. CI must pass before presenting to human
4. All review comments must be addressed AND threads resolved
5. Never skip the review loop - iterate until clean

## Comparison

| Aspect | Smithers | Ralph Wiggum |
|--------|----------|--------------|
| Architecture | Parallel workers | Single iterative loop |
| Isolation | Separate worktrees | Same session |
| Completion | CI + reviews | Completion promise |
| Use case | Multiple independent tasks | Single task, TDD iteration |

## License

MIT
