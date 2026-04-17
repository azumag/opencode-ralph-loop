---
description: Start Ralph Loop - repeats the task after each completion
---

# Ralph Loop

Start an iterative development loop that automatically continues until the task is complete, then starts the same task again.

## Setup

Create the state file in the project directory:

```bash
mkdir -p .opencode && cat > .opencode/ralph-loop.local.md << 'EOF'
---
active: true
iteration: 0
maxIterations: 100
completedCycles: 0
---

$ARGUMENTS
EOF
```

## Task

Now begin working on the task: **$ARGUMENTS**

## Completion

When the current task cycle is FULLY completed, signal completion by outputting:

```
<promise>DONE</promise>
```

**IMPORTANT:** ONLY output this when the current cycle is COMPLETELY and VERIFIABLY finished. Do NOT output false promises to escape the loop. After DONE, the plugin will start the same task again.

## Cancellation

Use `/cancel-ralph` to stop early.
