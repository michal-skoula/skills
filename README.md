# Skills

Canonical source for AI skills on this machine. Every harness reads from here instead of keeping its own copy.

Skills live in `skills/`, one directory each. The rest of the repo is free for docs and anything else.

```
Skills/
  README.md
  skills/
    setup-skills/SKILL.md
    unslop/SKILL.md
```

A skill is a folder holding a `SKILL.md`. The file starts with YAML frontmatter naming the skill and describing when it applies, then gives the instructions:

```markdown
---
name: unslop
description: Cut AI tells from any writing. Must always apply.
---

# Unslop

Edit text to remove AI patterns and add human voice.
...
```

The `description` decides when the skill loads, so write it as a trigger condition, not a summary.

## Wiring a harness

Claude Code reads personal skills from `~/.claude/skills`. Point that path at the `skills/` subdirectory:

```
ln -s /Users/michal/Skills/skills ~/.claude/skills
```

Link to `skills/`, not the repo root, or Claude Code reads `README.md` and `.git` as skill candidates and finds nothing.

That is the whole setup. No settings.json entry, no plugin install. Claude Code picks up skills at startup, so restart the session after adding one.

Other harnesses attach the same way. Nothing here is Claude Code specific, though Claude Code is the only one wired up today.

## Adding a skill

The `setup-skills` skill covers this, including how to verify a new skill without restarting your session. Short version: create `skills/<name>/SKILL.md` with `name` and `description` frontmatter, then test it from a fresh process.

## Installed

- `setup-skills` explains this layout and the symlink wiring.
- `unslop` strips AI writing tells. Taken verbatim from [cursor/plugins](https://github.com/cursor/plugins/blob/main/pstack/skills/unslop/SKILL.md).

## On a second machine

Clone the repo, then run the symlink command above with the new path.
