---
name: show-me-your-work
description: "Keep a reviewable, evidence-backed decision trail for long-running or unattended work. Use when another person must assess what an agent did after the fact."
---

# Show me your work

Keep one canonical log.

## The format

A single TSV file, one row per decision. Cells stay single-line. Evidence is a pointer, not prose.

Copy `references/decision-log-template.tsv` (the header row) to start a clean log. Columns:

- **ts.** ISO8601 timestamp.
- **phase.** The phase or workstream.
- **decision.** What was chosen or done, one line.
- **why.** The reason in plain words. If a principle drove it, say it plainly, not as a jargon tag.
- **evidence.** A link or path that proves it: commit SHA, PR number, `file:line`, or an artifact, trace, or screenshot path. Never a paragraph.
- **result.** The outcome or predicate state: `tests green`, `reverted`, `pixel-diff 0`, `INCONCLUSIVE`, `open`.

An example, plain-spoken so a reviewer reads it at a glance.

```
ts	phase	decision	why	evidence	result
2026-05-24T09:02:00Z	frame	counted the work first, about 100 components and roughly 75 hours	wanted to know the size before starting a long run	commit 3a9f1c2	found 5 things to sort out before starting
2026-05-24T09:40:00Z	harness	took screenshots of the old version before changing anything	so we can compare old against new and catch any visual change	scripts/snapshot.sh, baseline/	saved 120 reference screenshots
2026-05-24T11:15:00Z	widget	moved the widget styles over without changing how it looks	keep the change small and the result identical	commit 7c21e0a, pixel-diff 0	looks identical, tests pass
2026-05-24T12:30:00Z	widget	rejected a helper's patch because its screenshots were blank	checked the real files instead of trusting its summary	review note, screenshots/blank.png	patch not merged
```

## Logging a row

Write each entry the way you'd tell a teammate what you did. Use plain words and concrete actions.

Use the helper `scripts/log.sh <logfile> <phase> <decision> <why> <evidence> <result>`. It stamps `ts`, writes the header on first use, strips stray tabs/newlines, and prefixes any cell starting with `=`, `+`, `-`, or `@` with a single quote. A bare `printf` appending a row works too, but mind those same bytes if cells come from generated or user-supplied text.

Log decision points and checkpoints, not every action: a fork chosen, a unit completed with its verification result, a pivot or revert with its trigger, a blocker surfaced, a gate fixed. For loop runs, one row per iteration. Skip the trivial and self-evident.

A run is one agent conversation, including its later turns and any summary of it. A pickup, a replacement agent, or a new chat starts a new run. When a run adds to a log that already has rows, its first row has phase `start`, and so does its first row after another run's `start` row. So a run that comes back to a log in a later turn first reads the log's last rows to see whether another run wrote since. A `start` row names the `ts` range of the rows before it that this run did not write, and its evidence names this run, such as its agent id. Use phase `start` for nothing else.

## Where it lives

By default the log is a working artifact, not committed. Keep it at `decisions.tsv` in the work dir, or `.audit/<task-slug>.tsv` when several efforts run at once, and leave it out of git.

Commit it only when the work is ambitious enough that a reviewer needs the trail to trust the result.

## Rules

- Append-only. A wrong call gets a new row that supersedes it. Never edit or delete history.
- Prefer evidence a reviewer can rerun or inspect. A local file path is useful during the run but is not proof for a remote reviewer unless the artifact is preserved and shared.

## Audit the log

Before handing back, compare this run's rows with its actual actions and outputs. Use the current agent tool's accessible session transcript if it provides one. Do not assume a Cursor transcript path or search unrelated sessions. If no transcript is accessible, check the evidence you do have: tool results, commits, diffs, test output, screenshots, and other run artifacts. That is an evidence audit, not a full transcript audit. State which one you performed.

For each stretch of rows written by this run, starting at its first row or its later `start` row and ending before another run's `start` row:

- Check that every row maps to a real decision or action.
- Check that each row's evidence resolves and shows what the row claims.
- A fork, pivot, or abandoned approach that shaped the work but isn't logged is a gap. Add it when the available record supports it. Without a transcript, do not claim you found every omission.

Correct the log, not the story. Never edit or remove an old row. If a claim or evidence pointer is wrong, append a row that identifies and supersedes it with what actually happened. Do not audit unrelated runs unless asked, but supersede a known false claim when you encounter one.

## Independent review when available

For consequential work, seek an independent review if the active agent tool and budget support it. Prefer a reviewer from a different model family when available. Give the reviewer the trail and only the evidence it is permitted to access. Ask it to flag:

- Decisions logged with weak or absent evidence.
- Verification steps skipped or claimed without proof in the available record.
- Choices that look risky in hindsight (premature, scope-creeping, papering over a symptom).
- Gaps the user would otherwise miss on a casual skim.

If independent review is unavailable or disproportionate, complete the evidence audit yourself. Report that no independent review occurred; do not call a self-check cross-model review. In the handoff, state the audit scope, whether an independent reviewer ran, its findings if any, and any verification gap. Do not invent a reviewer or claim an inaccessible transcript was checked.

## Reviewing the trail

Read top to bottom, follow the evidence pointers, spot-check. GitHub renders a committed TSV as a table. `column -s$'\t' -t decisions.tsv` renders it in a terminal.

## Composing this skill

Other skills route their audit trail here instead of inventing one. Reference it by name and let it own the format. Don't restate the columns.
