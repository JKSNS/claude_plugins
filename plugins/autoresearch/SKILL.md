---
name: autoresearch
description: Autonomous research loop - program/methodology/evaluate cycle with self-improving methodology
trigger: /autoresearch
---

# /autoresearch

Run an autonomous research loop on the current project.

## Usage

```
/autoresearch              # start the loop
/autoresearch init         # create the three files (program.md, methodology.md, evaluate.py)
/autoresearch status       # show current sprint number, last score, methodology version
```

## What You Must Do When Invoked

### If "init" subcommand

Create three files in the project root if they don't exist:

1. `program.md` - template with sections for scope, constraints, targets, evaluation criteria
2. `methodology.md` - template with version 1.0, initial approach, time allocation
3. `evaluate.py` - template script that reads results and outputs a score

Tell the user to edit `program.md` with their goals, then run `/autoresearch` to start.

### If no subcommand (start the loop)

Check that all three files exist. If not, tell the user to run `/autoresearch init` first.

Then enter the loop:

```
LOOP:
  1. Read program.md for constraints and targets
  2. Read methodology.md for current approach and version
  3. Execute a time-bounded sprint using the methodology
  4. Run evaluate.py and capture the score
  5. Append results to results.tsv (sprint number, timestamp, score, notes)
  6. If sprint number is divisible by 3:
     - Review results.tsv for trends
     - Update methodology.md: promote what works, demote what doesn't
     - Adjust time allocation based on ROI
     - Increment methodology version
     - Commit methodology.md separately
  7. Commit sprint work
  8. Continue to next sprint
```

Never stop unless the user interrupts. Each sprint should be focused and
produce measurable output that evaluate.py can score.

### If "status" subcommand

Read results.tsv and methodology.md. Report:
- Total sprints completed
- Current methodology version
- Last 5 scores with trend direction
- Best score achieved
