# a2a-timeline-replay

**Deterministic time-travel explorer for a recorded A2A stream. Dumb math, honest numbers, zero dependencies.**

[![Tests](https://img.shields.io/badge/tests-20%20passed-brightgreen)]() [![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)]()

Eleventh in the a2a family: [a2a-tax](https://github.com/ZidoCode/a2a-tax), [a2a-stream-tax](https://github.com/ZidoCode/a2a-stream-tax), [a2a-reconnect-tax](https://github.com/ZidoCode/a2a-reconnect-tax), [a2a-artifact-audit](https://github.com/ZidoCode/a2a-artifact-audit), [a2a-task-state-audit](https://github.com/ZidoCode/a2a-task-state-audit), [a2a-context-audit](https://github.com/ZidoCode/a2a-context-audit), [a2a-webhook-audit](https://github.com/ZidoCode/a2a-webhook-audit), [a2a-card-lock](https://github.com/ZidoCode/a2a-card-lock), [mcp-collision](https://github.com/ZidoCode/mcp-collision), [a2a-doctor](https://github.com/ZidoCode/a2a-doctor).

The audit tools tell you **where** the violation is — "line 14: task resumed after terminal state". This tool tells you what it **looks like**: per-task state timelines, event windows around any task, and the full raw JSON at any line. The `git log` for agent sessions.

## Install

```bash
pip install a2a-timeline-replay   # or: copy the .py file
```

## Usage

```bash
$ a2a-timeline-replay timeline stream.jsonl
a2a-timeline-replay timeline — 6 task(s)

  good-task      submitted → working → completed
                 lines 1–4, 1 artifact(s)
  zombie-task    submitted → failed → working  [OPEN]
                 lines 5–14, 1 artifact(s)

$ a2a-timeline-replay around stream.jsonl zombie-task
a2a-timeline-replay around — task 'zombie-task' spans lines 5–14, showing 2–17

     line     2  status-update good-task → working
   < line     5  status-update zombie-task → submitted
   < line     6  status-update zombie-task → failed
   < line     7  status-update zombie-task → working     ← the resurrection
     line     8  status-update weird-task → submitted

use `at <line>` for the full JSON of any line above

$ a2a-timeline-replay at stream.jsonl 7        # full JSON at that line
$ a2a-timeline-replay grep stream.jsonl failed # find lines mentioning it
```

## Why offline replay

Live debuggers (proxy interceptors, dashboard UIs) need the agent running and add moving parts. When production misbehaves, what you have is a recorded JSONL — and you need answers from it in seconds, reproducibly, in CI if needed. Same stream in, same answer out, byte for byte. MIT license.
