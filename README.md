# claude-skills

A growing collection of [Claude Code](https://claude.com/claude-code) skills.

## Skills

| Skill | What it does |
|-------|--------------|
| [`guided-planning`](skills/guided-planning/SKILL.md) | Turns a raw idea into a project overview and a dependency-ordered, phased implementation plan through a one-question-at-a-time interview. Writes markdown to `plan/<feature>/`. Planning only, no code. |
| [`code-audit`](skills/code-audit/SKILL.md) | Runs six parallel audit agents (standards, duplication, logging, security, tests, dependencies) and produces a severity-ranked report plus an effort-bucketed fix plan. Accepts an optional path to scope the audit. |

## Install

### Option 1: as a plugin (all skills)

From inside Claude Code:

```
/plugin marketplace add tmac1973/claude-skills
/plugin install tmac-skills@tmac-skills
```

### Option 2: individual skills

Clone the repo and symlink (or copy) the skills you want into `~/.claude/skills/`:

```bash
git clone https://github.com/tmac1973/claude-skills.git
ln -s "$PWD/claude-skills/skills/guided-planning" ~/.claude/skills/guided-planning
ln -s "$PWD/claude-skills/skills/code-audit" ~/.claude/skills/code-audit
```

For a single project, use `.claude/skills/` in that project instead of `~/.claude/skills/`.

## Repository layout

```
.claude-plugin/
  plugin.json         # plugin manifest
  marketplace.json    # lets this repo be added as a plugin marketplace
skills/
  <skill-name>/
    SKILL.md          # required: frontmatter (name, description) + instructions
    references/       # optional: supporting files the skill reads on demand
```

## Adding a skill

1. Create `skills/<skill-name>/SKILL.md` with `name` and `description` frontmatter.
2. Put any supporting material in subfolders next to it (e.g. `references/`, `scripts/`).
3. Add a row to the table above.
4. Bump `version` in `.claude-plugin/plugin.json`.

## Credits

`code-audit` is adapted from a [gist by nud3l](https://gist.github.com/nud3l/15468abc5c4ca7e4e0e38e5b120a7997).

## License

[MIT](LICENSE)
