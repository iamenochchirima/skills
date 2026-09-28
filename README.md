# Long-Running Work Skill

[![skills.sh](https://skills.sh/b/iamenochchirima/skills)](https://skills.sh/iamenochchirima/skills)

Reusable Agent Skills for Codex, Claude Code, and other compatible agents.

## Long-running work

`long-running-work` helps an agent complete substantial, multi-phase work without
losing the original objective. It creates a local, evidence-based plan, works
one verified item at a time, and performs a completion audit before it stops.

## Pstack adaptation baselines

`skills/create-verification-skill`, `skills/maintain-verification-skill`, and
`skills/show-me-your-work` started as pstack copies and have been adapted for
project-local setup and tool-dependent verification. The PR-opening playbook is
source material under `sources/pstack/`, not a standalone skill. See
[the upstream record](sources/pstack/README.md) for provenance. Test the adapted
skills on a real project before adding them to the one-time project playbook.

`skills/enoch-mode` has nine adapted playbooks for investigation, changes,
PRs, and handoffs. The unchanged poteto-mode source files live under
`sources/pstack/poteto-mode/` for comparison. The skill passes structural
validation, but still needs a real-project forward test before adding it to
the one-time project playbook.

## Install with `npx skills`

The primary installation path works with Codex, Claude Code, and other supported
agents:

```bash
npx skills@latest add iamenochchirima/skills --skill long-running-work
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

The skill is then available as `/enoch-skills:long-running-work`. After
publishing a release with both manifest versions bumped, Claude users can run:

```text
/plugin marketplace update enoch-skills
/plugin update enoch-skills@enoch-skills
```

The Claude plugin declares its version in both `.claude-plugin/plugin.json` and
`.claude-plugin/marketplace.json`. Bump both values for each plugin release so
Claude Code detects the update. The `npx skills` installer continues to use the
repository commit as its update signal.

## License

This repository is released under the MIT License.
