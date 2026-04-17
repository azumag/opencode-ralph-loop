---
description: Show Ralph Loop plugin help and available commands
---

# Ralph Loop Help

## Available Commands

- `/ralph-loop <task>` - Start an auto-continuation loop for the given task
- `/cancel-ralph` - Stop an active Ralph Loop

## Quick Start

```
/ralph-loop Build a REST API with user authentication
```

The AI will work on your task and automatically continue until it outputs `<promise>DONE</promise>` to signal completion. After DONE, Ralph Loop starts the same task again.

## How It Works

1. Creates state file at `.opencode/ralph-loop.local.md`
2. Works on task until idle
3. If no `<promise>DONE</promise>` found, auto-continues
4. If `<promise>DONE</promise>` is found, starts the same task again
5. Repeats until cancelled or max iterations (100) is reached during a cycle

## Cancellation

To stop early:
```
/cancel-ralph
```

For more details, the AI can use the `help` skill.
