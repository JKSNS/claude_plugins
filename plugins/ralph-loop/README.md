# Ralph Loop

Recurring autonomous task execution for Claude Code. Run any prompt or slash command on a repeating interval.

## Install

```
/install-plugin ralph-loop
```

## Usage

```
/ralph-loop              # start with default 10 minute interval
/loop 5m /preflight      # run /preflight every 5 minutes
/cancel-ralph            # stop the active loop
```

## When to use

Useful for polling deployment status, running periodic health checks, monitoring build pipelines, or executing background research that reports back on a schedule. The loop runs inside the Claude Code session and stops when you cancel it or end the session.
