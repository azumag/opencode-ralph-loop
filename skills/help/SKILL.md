---
name: help
description: Explain Ralph Loop plugin and available commands
---

# Ralph Loop Help

The Ralph Loop plugin provides auto-continuation for complex tasks in opencode.

## Available Commands

### `/ralph-loop <task>`
Start an iterative development loop that automatically continues until the task is complete, then starts the same task again.

Example:
```
/ralph-loop Build a REST API with authentication
```

The AI will work on your task and automatically continue. When a cycle completes, it starts the same task again.

### `/cancel-ralph`
Cancel an active Ralph Loop.

Example:
```
/cancel-ralph
```

## How It Works

1. **Start**: `/ralph-loop` creates a state file at `.opencode/ralph-loop.local.md`
2. **Loop**: When the AI goes idle, the plugin checks if `<promise>DONE</promise>` was output
3. **Continue**: If not found, it injects "Continue from where you left off"
4. **Repeat**: If DONE is found, it starts the same task again
5. **Stop**: Loop continues until cancelled or max iterations (100) is reached during a cycle

## Completion Signal

When the current task cycle is fully complete, the AI outputs:

```
<promise>DONE</promise>
```

This signals the loop to start the same task again. The AI should ONLY output this when the current cycle is truly complete.

## State File

Located at `.opencode/ralph-loop.local.md` (add to `.gitignore`):

```markdown
---
active: true
iteration: 3
maxIterations: 100
completedCycles: 2
sessionId: ses_abc123
---

Your original task prompt
```

## Credits

- Inspired by [Anthropic's Ralph Wiggum](https://github.com/anthropics/claude-code/tree/main/plugins/ralph-wiggum) plugin for Claude Code
- Standalone extraction from [oh-my-opencode](https://github.com/code-yeongyu/oh-my-opencode)
