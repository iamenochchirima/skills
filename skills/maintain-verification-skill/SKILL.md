---
name: maintain-verification-skill
description: "Check a project's verification skill and feature map against code and live behavior. Use for a focused check of changed features or a requested full verification audit."
---

# Maintain a verification skill

Check the project-local verification skill and its one canonical feature map against source code and real behavior. This is a maintenance task, not permission to change product code or open a pull request unasked.

## Choose the scope

For an ordinary change, use a targeted pass. Read the issue, diff, and callers to identify the changed user-facing features and shared paths they affect. Check those features and their map entries. If the change has no user-facing effect, say why a live feature drive is not relevant; still check any changed harness instructions. If the affected set cannot be bounded with confidence, widen the pass instead of claiming a narrow check covered everything.

Use a full pass when the user asks for an audit, when the map's accuracy is in doubt, or when widespread changes make a targeted set unreliable. A full pass covers every mapped feature and looks for user-facing features missing from the map. Do not turn every ordinary pull request into a full audit.

## Locate the source of truth

Find the project-local verification skill and the map it names. The skill may live in a tool-specific project directory; the map may live under `docs/features/` or inside the skill. If several maps claim to be canonical, stop and resolve ownership before editing. If there is no verification skill, use `create-verification-skill` instead. Read the skill's launch, doctor, drive, evidence, and cleanup instructions before running anything.

Only edit the canonical verification skill, its owned helpers, the canonical map, and tool-specific copies of that skill when needed to keep them in sync. Do not change product code in this maintenance pass. A product regression is a separate issue, not a reason to rewrite the map to hide it.

## Check and correct

1. Compare the selected map entries with the code and with how a user reaches each feature. Check index links and any affected shared entry points. In a full pass, inspect every entry and sweep source for obvious missing features. Use subagents only when their independent, read-only coverage saves time or improves confidence; one subagent per feature is not a requirement.
2. Launch or connect to the instance the verification skill permits. Run its doctor check before driving it. Exercise each selected feature's real user path and capture the action, result, and relevant side effect. If the app cannot run, record the exact prerequisite or failure and do not call the feature verified. After a surprising failure, health-check or reset before another drive.
3. Classify each mismatch. Fix confirmed map drift or a broken harness under this skill's edit scope, then run the affected path again. Report broken product behavior separately; do not silently change the map to match a regression. If a feature is unreachable, record the route attempted and the missing prerequisite.
4. Clean up only resources this run started. Confirm evidence survives cleanup and that any copied tool-specific skill instructions still point to the canonical map.

Report the mode, features covered, evidence, changes made, and anything blocked or out of scope. Follow the project's normal change and pull-request rules if a correction needs to be submitted. Do not claim full-map coverage after a targeted pass.
