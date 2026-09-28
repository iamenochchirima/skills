# Opening a PR

Use when the user asks for a PR or the project's authorized implementation workflow includes PR creation. Do not run it for a read-only investigation, a local-only request, or work whose submission path is undecided. Opening a PR does not authorize merging it.

1. Confirm the repository, target branch, current branch, and worktree state. Preserve unrelated edits. If work is mixed, isolate the intended change with a safe branch or worktree after identifying its exact files. Never reset a dirty tree to make a PR look clean.
2. Read the linked implementation issue or spec when one exists. Check the diff against its acceptance criteria and the project's instructions. Run the most relevant tests and real product path. Record the exact command, result, artifact, and any limitation. For substantial or unattended work, audit the `show-me-your-work` trail if that skill is installed.
3. Self-check the change for unintended files, secrets, generated noise, outdated docs, and release impact. Fix material findings and rerun affected verification. If the project has a code-review skill, hand the PR to that separate review flow at the project's designated point. Keep Standards and Spec findings distinct there. Do not describe this self-check as an independent review.
4. Commit coherent changes on the intended branch, respecting any existing commit or branch convention. Push only the branch needed for the PR. Do not force-push, rebase shared work, or publish unrelated changes as cleanup.
5. Open the PR against the intended base and link the implementation issue. Give the reviewer a concise account of why the change exists, what changed, what was actually verified, known risk, and unfinished work. Attach or link relevant screenshots, traces, test output, or a committed decision trail when useful. State checks that were not run. Confirm the resulting PR URL, base, head, and status.
6. Move a tracker item to In review only when the project's configured workflow calls for it and the PR is actually reviewable. Report the PR and remaining review or CI work. Do not merge or enable auto-merge under this playbook.

Prefer the repository's existing PR format over imposing a universal template. A short PR body is useful; the proof must still be reachable by its reviewer.
