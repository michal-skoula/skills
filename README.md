# Skills

Canonical source for AI skills on this machine. Every harness reads from here instead of keeping its own copy.

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

## Layout

```
Skills/
  README.md
  unslop/
    SKILL.md
```

One directory per skill. Directory name should match the frontmatter `name`.

## Wiring a harness

Claude Code reads personal skills from `~/.claude/skills`. Point that path here:

```
ln -s /Users/michal/Skills ~/.claude/skills
```

That is the whole setup. No settings.json entry, no plugin install. Claude Code picks up skills at startup, so restart the session after adding one.

Other harnesses attach the same way: symlink their expected skills path to this folder. Nothing here is Claude Code specific.

## Adding a skill

1. `mkdir <name>` and write `<name>/SKILL.md` with `name` and `description` frontmatter.
2. Restart the harness.
3. Confirm it appears in the skills list, then give it a task it should catch.

Getting it listed only proves the file is readable. Run a real task through it to prove the description triggers.

## Installed

- `unslop` strips AI writing tells. Taken verbatim from [cursor/plugins](https://github.com/cursor/plugins/blob/main/pstack/skills/unslop/SKILL.md).

## On a second machine

Clone the repo, then run the symlink command above with the new path.
