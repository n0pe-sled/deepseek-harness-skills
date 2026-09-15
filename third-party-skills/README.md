# Third-party skills

Skills that came from somewhere else. We do not edit these in place.

| Bundle | Source | License |
|---|---|---|
| [`unslop`](unslop/SKILL.md) | [cursor/plugins: pstack/skills/unslop](https://github.com/cursor/plugins/tree/main/pstack/skills/unslop) | vendored copy |
| [`specterops-skills`](specterops-skills/) | [SpecterOps/skills](https://github.com/SpecterOps/skills), git submodule | Apache-2.0 |

## unslop

A vendored copy. It cuts common AI writing patterns while keeping the meaning and
tone. Source attribution stays in the skill frontmatter.

## specterops-skills

A git submodule pinned to one upstream commit, not a copy of the tree. A `git pull`
in this repo moves the pointer only if someone commits a new pointer. To advance it:

```bash
git submodule update --remote third-party-skills/specterops-skills
git add third-party-skills/specterops-skills
git commit -m "Bump SpecterOps skills submodule"
```

A fresh clone needs `git clone --recurse-submodules`, or `git submodule update
--init --recursive` after the fact. Otherwise the folder arrives empty and every
skill inside it is missing.

What upstream ships: 25 plugin bundles under `plugins/` holding 72 skills, plus 3
more skills at the repo's own `skills/` directory. Two plugin dirs, `ops-adcs` and
`ops-mssql`, are placeholders with no skills yet. Upstream also carries 21 agent
definitions in `agents/*.toml` and Codex/Claude plugin manifests. dsh reads
neither.

Skills sit at `plugins/<plugin>/skills/<name>/SKILL.md`, two levels below the
plugin. dsh discovers one level per root, so the shallow install command in the
root README skips all of them. Use the deeper `find` form there.

No skill name collides with a skill we own.
