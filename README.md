# Long-Running Work Skill

[![skills.sh](https://skills.sh/b/iamenochchirima/skills)](https://skills.sh/iamenochchirima/skills)

Reusable Agent Skills for Codex, Claude Code, and other compatible agents.

## Long-running work

`long-running-work` helps an agent complete substantial, multi-phase work without
losing the original objective. It creates a local, evidence-based plan, works
one verified item at a time, and performs a completion audit before it stops.

## Install with `npx skills`

The primary installation path works with Codex, Claude Code, and other supported
agents:

```bash
npx skills@latest add iamenochchirima/skills
```

To install this skill globally for Codex without the interactive selector:

```bash
npx skills@latest add iamenochchirima/skills \
  --skill long-running-work \
  --agent codex \
  --global
```

Refresh installed skills later with:

```bash
npx skills@latest update
```

## Install as a Claude Code plugin

This repository also contains a Claude Code marketplace that points to the same
canonical `skills/` directory. From inside Claude Code:

```text
/plugin marketplace add iamenochchirima/skills
/plugin install enoch-skills@enoch-skills
```

The skill is then available as `/enoch-skills:long-running-work`. Claude users
can update the marketplace and plugin after a new repository commit with:

```text
/plugin marketplace update enoch-skills
/plugin update enoch-skills@enoch-skills
```

The plugin intentionally omits a fixed version, so Claude uses the repository
commit as the update signal.

## License

This repository is released under the MIT License.
