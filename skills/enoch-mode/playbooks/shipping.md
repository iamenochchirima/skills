# Shipping

Use only when the user explicitly asks to merge, land, or ship a PR. If the request means deployment as well as merge, identify and follow the project's separate release process. A green CI badge alone is not a merge decision.

1. Resolve the exact PR or ordered stack and the intended base. Read the current head SHA, diff, issue or spec, approvals, required checks, unresolved threads, and mergeability. Check the project's branch protection and release rules. Do not assume an earlier review still covers a changed patch.
2. Verify that the submitted change still has sufficient evidence. Rerun or request verification when the head changed materially after the recorded proof. For a stack, check each PR against its actual base and land only a contiguous reviewed, verified sequence from the bottom. Stop at the first uncertain PR.
3. Merge one PR at a time using the project's required method. Do not force-push, retarget, enable auto-merge, or rewrite a stack merely because CI is pending. If a required approval, check, conflict resolution, or human decision is missing, stop and report it. A request to merge does not authorize bypassing branch rules.
4. After each merge, confirm the host reports it merged, record the merged commit, and inspect the next dependent PR afresh. Confirm tracker status only when the project workflow calls for it. Do not claim deployment occurred because a PR merged.
5. Report what merged, what did not, the evidence used, and any release or verification step still pending.

Prefer the project's forge tooling and conventions. No Cursor cloud agent, specific model, or watcher is required. An independent review is valuable for consequential changes when one is available, but never fabricate a verdict.
