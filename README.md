<p align="center">
  <img src="header.png" alt="Smithers" width="400">
</p>

<h1 align="center">smithers</h1>

<p align="center">
  <strong>Ralph loops. Smithers parallelizes.</strong>
</p>

<p align="center">
  <a href="https://skills.sh"><img src="https://img.shields.io/badge/skills.sh-available-blue" alt="skills.sh"></a>
  <a href="https://opensource.org/licenses/MIT"><img src="https://img.shields.io/badge/License-MIT-green.svg" alt="License"></a>
</p>

---

## The Problem

You want to knock out several tasks once, and are tired of baby sitting agents:

```
You: "Implement this feature"
Agent: *creates PR*
CI: ❌ FAILED
You: "Fix it"
Agent: *pushes fix*
CodeRabbit: "Missing error handling"
You: "Address the review"
Agent: *pushes fix*
You: *switch to feature 2 and repeat*
```

## The Solution

```
You: /smithers "implement login, fix the navbar, and add API tests"
Smithers: "3 tasks parsed. Dispatch?"
You: "y"
*walks away*
Smithers: "All PRs ready for review:"
  - PR #123: ✅ CI passing, ✅ Reviews addressed
  - PR #124: ✅ CI passing, ✅ Reviews addressed
  - PR #125: ✅ CI passing, ✅ Reviews addressed
```

---

## Installation

```bash
npx skills add noc0dev/smithers
```

Then copy the `smithers-worker` agent from SKILL.md to `~/.claude/agents/`.

## Requirements

- [gh](https://cli.github.com/) - GitHub CLI
- Git worktree support

### Optional

- [beads](https://github.com/steveyegge/beads) - Dependency-aware issue tracker for agents
- [roborev](https://github.com/wesm/roborev) - Local code review
- CodeRabbit, Codex - Org-level PR review bots

## Usage

```bash
# Natural language
/smithers "implement login and fix the navbar"

# From GitHub issues
/smithers --from-issues label:ready

# From beads (auto-detected if bd CLI available)
/smithers
```

## How It Works

1. Gets tasks (natural language, GitHub issues, or beads)
2. Creates isolated git worktree per task
3. Dispatches parallel workers to implement
4. Polls every 60s until CI passes + reviews resolved
5. Presents polished PRs for your review

## See Also

- [Ralph Wiggum](https://github.com/anthropics/claude-code/blob/main/plugins/ralph-wiggum/README.md) - Depth-first iteration (single task loops)
- Smithers is breadth-first (parallel tasks, review gating)

## License

MIT
