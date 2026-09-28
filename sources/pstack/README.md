# Pstack source record

Source: [cursor/plugins, pstack](https://github.com/cursor/plugins/tree/ecc249f1e306fc64ddf83c7bed16cacf7c2239db/pstack)

Copied at commit `ecc249f1e306fc64ddf83c7bed16cacf7c2239db`:

- `skills/show-me-your-work/`, including its log helper and template
- `skills/create-verification-skill/`, including its feature-map examples
- `skills/maintain-verification-skill/`
- `sources/pstack/opening-a-pr.md`, from `pstack/skills/poteto-mode/playbooks/opening-a-pr.md`
- `sources/pstack/poteto-mode/SKILL.md`, the original mode entry point
- `sources/pstack/poteto-mode/playbooks/`: nine unchanged poteto-mode playbooks used as adaptation baselines

The three skills were copied unchanged, then adapted in this repository. Their
current `SKILL.md` files are no longer byte-for-byte upstream. The copied helper
script, templates, examples, PR-opening playbook, and license remain unchanged.
`opening-a-pr.md` is a playbook inside `poteto-mode`, not an independent skill.
Do not install or invoke it as one until a standalone adaptation has been
designed and tested.

`skills/enoch-mode/` now has adapted, active playbooks. The originals under
`sources/pstack/poteto-mode/` remain unchanged for comparison. Structural
validation does not establish real-project behavior; that still needs testing.

The upstream copyright and MIT license are preserved in [LICENSE](LICENSE).
