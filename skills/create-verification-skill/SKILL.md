---
name: create-verification-skill
description: "Create a project-local skill that runs the real app, drives a user path, and captures proof. Use when a project lacks a repeatable way to verify UI, CLI, or service behavior."
---

# Create a verification skill

Create a project-local skill that lets a fresh agent launch the real app, exercise a user path, and capture evidence. Its instructions must work from a fresh checkout without this generator or a personal skill installation.

## 1. Interview the repo, not the user

Answer these from the codebase and only ask the user what you cannot observe:

- **Surface:** what does a user actually touch? A web UI, a CLI/TUI, a desktop app, an API, a mobile app, a library? A repo can have several; pick the primary one and note the rest.
- **Run:** how does the app start locally? Prefer the repo's own documented dev command (package scripts, Makefile, README quickstart). Note ports, env vars, seed data, auth.
- **Drive:** how can an agent interact with it programmatically? Existing harnesses first — Playwright/Cypress specs, expect scripts, PTY helpers, curl-able endpoints, a debug port. Only then pick a generic recipe: browser/CDP for web and Electron, a tmux/PTY harness for CLI/TUI, plain HTTP for services.
- **Observe:** what evidence can be captured? Screenshots, terminal transcripts, response bodies, logs, exit codes, DB state.
- **Isolate:** can two instances run side by side (ports, data dirs, profiles)? If not, say so in the generated skill: refusing to double-drive a shared instance beats corrupting the user's session.

If the checkout doesn't build or start as-is, fix that first (or report it precisely) before generating; a skill written against a broken base teaches wrong steps. When an irrelevant missing asset blocks startup (a static dir the API never serves, a sample config), the generated skill may create it, clearly marked as verification scaffolding, and remove it in cleanup.

## 2. Choose one project home

Inspect the agent tools the project actually supports and their project-level skill locations. Reuse an existing verification skill if it works. Otherwise, choose one canonical skill source in the repository and make it discoverable from each selected tool's supported project location. Use a link only if that tool follows it; if copies are required, identify the canonical copy and check the others against it. Do not assume `.cursor/skills/` is read by every agent. Verify discovery in a fresh agent session for each selected tool. Do not claim support for a tool you have not checked.

Find the project's current feature map before making another one. Reuse its location, whether it lives in `docs/features/` or inside a verification skill. If no map exists, choose one canonical location and link it from the project instructions and verification skill. Multiple tool-specific skill copies must all point to this same map, not maintain separate maps.

## 3. Generate the skill

Write a `verify-<app>/SKILL.md` with YAML frontmatter (`name: verify-<app>` and a description naming the app, its user-facing surface, and when to use it). Put it in the chosen project-level skill location. Include these sections, grounded in the repo rather than placeholders:

- **Launch:** the exact command that starts the app for verification, and how to tell it's ready (a log line, a port answering, a prompt). Include teardown. For a short-lived CLI or TUI there is no server to keep alive: launch means build the binary (or install deps) once, then start each drive in its own isolated PTY or tmux session.
- **Doctor:** one read-only check that answers "is this instance worth driving?" — process up, right version/build, port owned by us, auth valid. An agent runs this first whenever anything looks off.
- **Drive:** the harness recipe with real selectors/commands from this repo, not examples. Prefer stable handles (ARIA labels, data attributes, prompt strings, route paths) over coordinates and tab order.
- **Evidence:** what to capture for a proof and where it goes. State the proof standards: exercise the real user path, not internal setters or test-only endpoints; capture the action and the resulting state, not just the final screen; verify side effects (files written, rows inserted, messages sent) alongside what's visible; mocks only where a production boundary already isolates the external system. When the safe path is a dry-run or test mode, verify what it actually skips by observing (files, network, git refs) rather than trusting its name: some dry-runs still touch the network or open a browser.
- **Cleanup:** how to tear down instances the run created. Never kill by process name; kill what you started. Cleanup removes instances and scratch state, never the evidence: proof artifacts survive the teardown, in a location the skill names.
- **Helpers:** any script the skill ships is executable and its invocation is shown in the skill body. A helper the reader has to reverse-engineer is not a helper.

## 4. Seed or update the feature map

Update the existing map or seed the chosen canonical map with a README index and files for the user-facing features you can identify. Start with a few real features from routes, commands, menus, or docs; do not invent entries to reach a count. Use [`references/feature-map-example/`](references/feature-map-example/) for the user-point-of-view shape. Each feature should say what it does, how a user reaches it, how the verification skill drives it, and what observable result proves it works. Cover relevant entry points, not only the easiest one. The map describes current behavior, not proposed work or a second issue tracker.

## 5. Prove the generated skill before handing it over

Run its own instructions end to end once: launch, doctor, drive one mapped feature, capture evidence, and clean up. Confirm the result and any side effect, not merely that a command exited successfully. After cleanup, confirm the evidence still exists at the named location. Clean up after failed attempts too. If the app cannot run, report the exact blocker and leave the skill marked unverified; do not present an unexecuted skill as ready. Confirm a fresh agent in each selected tool can find the project-local skill and canonical map without being given this generator.

## 6. Keep it current

Point the project instructions at the canonical map and verification skill. When shipped behavior changes, update the map in the same change. Use `maintain-verification-skill` for a targeted drift check or a full audit when requested; do not invent a schedule.
