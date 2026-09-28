# Enoch's Agent Skills

[![skills.sh](https://skills.sh/b/iamenochchirima/skills)](https://skills.sh/iamenochchirima/skills)

Reusable agent skills for Codex, Claude Code, and other compatible agents.

## Skills

| Skill | Purpose |
| --- | --- |
| [enoch-mode](skills/enoch-mode/SKILL.md) | Cross-project playbooks for investigation, implementation, pull requests, merge follow-up, and handoffs. |
| [show-me-your-work](skills/show-me-your-work/SKILL.md) | An evidence-backed decision log for reviewable long-running work. |
| [create-verification-skill](skills/create-verification-skill/SKILL.md) | Build a project-local way to run the app and verify real user paths. |
| [maintain-verification-skill](skills/maintain-verification-skill/SKILL.md) | Keep that verification skill and its feature map aligned with the product. |
| [long-running-work](skills/long-running-work/SKILL.md) | Plan and execute substantial work in verified slices with a completion audit. |

The first four are adapted from pstack for this workflow. `long-running-work` is
a separate skill. The original upstream material and attribution are kept under
[`sources/pstack/`](sources/pstack/README.md); that directory is not an
additional set of installable skills. These skills provide workflows, not a
substitute for testing them against the project where they are used.

Matt Pocock's `code-review`, `to-spec`, and `to-tickets` skills are not
published from this repository.

## Install with `npx skills`

List the skills available from the repository:

```bash
npx skills@latest add iamenochchirima/skills --list
```

Install a selected skill for the current project, for example:

```bash
npx skills@latest add iamenochchirima/skills \
  --skill enoch-mode \
  --agent codex \
  --copy
```

Replace `enoch-mode` with any skill name in the table. Add `--global` if you
want the skill available across projects. Refresh installed skills later with:

```bash
npx skills@latest update
```

## Install as a Claude Code plugin

This repository also contains a Claude Code marketplace that points to the same
`skills/` directory. From inside Claude Code:

```text
/plugin marketplace add iamenochchirima/skills
/plugin install enoch-skills@enoch-skills
```

The skills are then available under the `enoch-skills` namespace, for example
`/enoch-skills:enoch-mode`. After publishing a release with both manifest
versions bumped, Claude users can run:

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
