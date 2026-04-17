# opencode-ralph-loop

Minimal Ralph Loop plugin for [opencode](https://opencode.ai) - auto-continues until task completion, then starts the same task again.

## Fork notice

This repository is a fork of [charfeng1/opencode-ralph-loop](https://github.com/charfeng1/opencode-ralph-loop).

Compared with the upstream plugin, this fork changes the completion behavior:

- Upstream stops the loop when the assistant outputs `<promise>DONE</promise>`.
- This fork treats `<promise>DONE</promise>` as the end of one cycle, then starts the same task again.
- The state file tracks `completedCycles` and resets `iteration` to `0` for each new cycle.
- Bundled commands and skills are synced on plugin startup so installed opencode files stay aligned with this fork.

Inspired by Anthropic's Ralph Wiggum technique for iterative, self-referential AI development loops.

## Why this plugin?

[oh-my-opencode](https://github.com/code-yeongyu/oh-my-opencode) is a fantastic, feature-rich plugin that includes Ralph Loop along with many other powerful capabilities like the Sisyphus orchestrator, background agents, and more.

However, we personally found the full suite a bit heavy for our workflow. We also noticed others in the community expressing interest in specific features without needing the complete package. So we extracted just the Ralph Loop functionality into this standalone, lightweight plugin.

If you want the full-featured experience, definitely check out oh-my-opencode. If you just want repeating auto-continuation loops with minimal overhead, this plugin is for you.

## Installation

Add to your `~/.config/opencode/opencode.json`:

```json
{
  "plugin": ["opencode-ralph-loop"]
}
```

Restart opencode. That's it!

On each run, the plugin will automatically sync skills and commands to your `~/.config/opencode/` directory.

## Usage

### Start a loop

```
/ralph-loop "Build a REST API with authentication"
```

The AI will work on your task, automatically continue until completion, then start the same task again.

### Cancel a loop

```
/cancel-ralph
```

### Get help

```
/help
```

## How it works

1. `/ralph-loop` creates a state file at `.opencode/ralph-loop.local.md`
2. When the AI goes idle, the plugin checks if `<promise>DONE</promise>` was output
3. If not found, it injects "Continue from where you left off"
4. If found, it resets the iteration count and injects a prompt to start the same task again
5. Loop continues until cancelled or max iterations (100) is reached during a cycle

### Completion Promise

When the AI finishes a task cycle, it outputs:

```
<promise>DONE</promise>
```

**Important:** The AI should ONLY output this when the current cycle is COMPLETELY and VERIFIABLY finished. False promises are not allowed. DONE starts the next cycle; it does not stop the loop.

## State File

The loop state is stored in your project directory:

```
.opencode/ralph-loop.local.md
```

Format (markdown with YAML frontmatter):

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

Add `.opencode/ralph-loop.local.md` to your `.gitignore`.

## Features

- **Plug-and-play**: Just add to config and restart - no manual setup
- **Auto-setup**: Skills and commands are automatically synced
- **Minimal**: ~300 lines, no bloat
- **Project-relative**: State file in `.opencode/`, not global
- **Completion detection**: Scans session messages for DONE promise and restarts the task
- **Progressive context**: Skills provide context only when needed
- **Commands**: `/ralph-loop`, `/cancel-ralph`, and `/help`

## Architecture

Following Anthropic's Claude Code plugin pattern:

```
opencode-ralph-loop/
├── src/
│   └── index.ts        # Main plugin with event hooks and tools
├── skills/
│   ├── ralph-loop/     # Progressive context for starting loops
│   ├── cancel-ralph/   # Context for cancellation
│   └── help/           # Plugin documentation
├── commands/
│   ├── ralph-loop.md   # Slash command for starting
│   ├── cancel-ralph.md # Slash command for cancelling
│   └── help.md         # Slash command for help
└── package.json
```

## Credits

- Inspired by [Anthropic's Ralph Wiggum](https://github.com/anthropics/claude-code/tree/main/plugins/ralph-wiggum) plugin for Claude Code
- Implementation pattern from [oh-my-opencode](https://github.com/code-yeongyu/oh-my-opencode)

## License

MIT
