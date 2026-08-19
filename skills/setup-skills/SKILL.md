---
name: setup-skills
description: Add a skill to the global library on the user's machine, or connect that library to a harness on a new machine.
disable-model-invocation: true
---

# Setup skills

The global skill library lives at `$HOME/Skills`. Skills go in the `skills/` directory, one subfolder each. Everything else in the repo is docs.

```
$HOME/Skills/
  README.md
  skills/
    unslop/SKILL.md
    setup-skills/SKILL.md
```

## Connecting to harnesses
### Claude Code
Claude Code reads personal skills from `~/.claude/skills`. Point that path at the `skills/` subdirectory:

```
ln -s /Users/michal/Skills/skills ~/.claude/skills
```

The link targets `skills/`, not the repo root. Pointing it at the root makes Claude Code read `README.md` and `.git` as skill candidates and find no skills at all.

That is the entire setup. There is no `settings.json` entry and no plugin install. Skill discovery at that path is built in.

If `~/.claude/skills` already exists, check what it is and ask the user how to proceed before replacing it:

```
test -L ~/.claude/skills && readlink ~/.claude/skills
```

A symlink is safe to `rm` and recreate, because `rm` deletes the link and leaves the target. A real directory holds skills that live nowhere else, so move its contents into the library first.

## Add a skill

1. Create `skills/<name>/SKILL.md`. Directory name matches the frontmatter `name`.
2. Write the frontmatter:

```yaml
---
name: <name>
description: <when to use this skill>
---
```

3. Write the instructions below the frontmatter as plain markdown.
4. Commit.

The `description` is the only part Claude reads when deciding whether to load the skill. Write it as a trigger condition, naming the situations and the words a request would use. A summary of the contents will not fire reliably.

Skills are read at startup, so a new one is invisible to sessions already running.

## Verify

Do not ask the user to restart to find out whether it worked. Start a fresh process instead:

```
cd /tmp && claude -p "List your available skill names, one per line. Then stop."
```

The new name should appear. That proves the file is readable and parsed.

Then prove the description actually triggers, which is the part that usually fails:

```
cd /tmp && claude -p "<a request the skill should catch, with no mention of the skill>"
```

If the skill loads but never fires, the `description` is too vague. Rewrite it around trigger words before touching anything else.

## Other harnesses

Nothing in the library is Claude Code specific. Another harness attaches the same way, by symlinking its own expected skills path to `/Users/michal/Skills/skills`. Only Claude Code is wired up today.
