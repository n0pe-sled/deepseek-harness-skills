# deepseek-harness-skills

A personal collection of reusable **skills** for the
[DeepSeek Harness (dsh)](https://github.com/deepseek-ai/deepseek-harness).
Each skill is a standard skill bundle (`<name>/SKILL.md` with YAML frontmatter)
that any dsh filesystem provider discovers and any agent can load.

| Skill | What it does |
|---|---|
| [`dsh-plugin-authoring`](dsh-plugin-authoring/SKILL.md) | Guidance distilled from a working session on **authoring a third-party dsh plugin** (node half, browser client half, or both): the bundle/profile installation model, the real client-bundle contract, and the deltas between the published plugin docs' sample code and the shipped harness — plus package layout, node/browser half rules, verification, and current version facts. |

## Skill format

Skills follow `@deepseek-ai/dsh-skill-filesystem` conventions:

- **Bundle form**: `<name>/SKILL.md` with `---` frontmatter carrying `name`
  (kebab-case) and `description`, plus optional `whenToUse`,
  `disable-model-invocation`, and `user-invocable`.
- **Flat form**: `<name>.md` at a root's top level (same frontmatter).

Discovery is one level deep per root, and the standard roots are:

| Root | Location |
|---|---|
| project | `<project>/.dsh/skills`, `<project>/.agents/skills` |
| user (dsh) | `$DSH_HOME/skills` |
| user (agents) | `$DSH_AGENTS_HOME/skills` (default `~/.agents/skills`) |

## Install

Clone the repo somewhere stable, then copy (or symlink) each skill bundle into
a user root the filesystem provider watches:

```bash
git clone https://github.com/n0pe-sled/deepseek-harness-skills.git ~/dsh-skills

# into the shared agent user root (~/.agents/skills also hosts installed skills)
mkdir -p ~/.agents/skills
cp -R ~/dsh-skills/dsh-plugin-authoring ~/.agents/skills/

# or symlink instead, so `git pull` updates it in place:
ln -s ~/dsh-skills/dsh-plugin-authoring ~/.agents/skills/dsh-plugin-authoring
```

The provider watches these roots, so the skill appears in the next catalog
observation — no restart required for newly added skills on a running session's
next load.

### Using `$DSH_HOME/skills` instead

If you would rather keep dsh skills separate from `~/.agents` (which a
third-party GitHub-skill installer also manages via `~/.agents/.skill-lock.json`):

```bash
mkdir -p "$DSH_HOME/skills"     # or "$HOME/.dsh/skills"
cp -R ~/dsh-skills/dsh-plugin-authoring "$DSH_HOME/skills/"
```

## Verify

```bash
# the skill is discoverable when it appears in the catalog; for the web GUI,
# start a session and reference the skill by name, or list it via:
ls ~/.agents/skills/dsh-plugin-authoring/SKILL.md
```

## Updating

```bash
git -C ~/dsh-skills pull
```

Copied skills need the copy refreshed (re-`cp`); symlinked skills pick up the
new content automatically.
