# Enoch's skills

Reusable Agent Skills for Codex and other coding agents.

## Long-running work

`long-running-work` helps an agent complete substantial, multi-phase work without
losing the original objective. It creates a local, evidence-based plan, works one
verified item at a time, and performs a completion audit before it stops.

### Install

The simplest option works with Codex, Claude Code, and other supported agents:

```bash
npx skills@latest add iamenochchirima/skills
```

The installer lets users choose which skills and agents to install. Use `--global`
if the skill should be available across projects.

### Install as a native Codex plugin

Codex users can also install the managed plugin from this repository's marketplace:

```bash
codex plugin marketplace add iamenochchirima/skills
codex plugin add long-running-work@enoch-skills
```

## License

This repository is released under the MIT License.
