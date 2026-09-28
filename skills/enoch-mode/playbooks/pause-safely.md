# Pause safely

Use on an explicit request to pause or when a handoff is needed before a session ends. Do not interpret "keep going" or a long autonomous task as a pause request.

1. Stop at a safe boundary. Finish or clearly mark the current atomic step. Leave no silent background process or delegated task running if its owner will disappear; report any task that must continue.
2. Inspect the worktree, branch, commits, tests, and open PR. Preserve user changes. Do not commit, push, open a PR, or reset merely to produce a tidy handoff. Do those only if already authorized by the task.
3. Record a short resume note in an appropriate location when needed. Include the objective, decisions already made, what changed, exact verification completed, current uncommitted state, blockers, and the next safe action. Link existing plans or decision trails rather than duplicating them.
4. Tell the user where the note and work live, whether changes are committed or only local, and what the next agent should do first.

Never call untested work complete or imply that a handoff note makes changes durable when they exist only in an ephemeral environment.
