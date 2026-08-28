# deepseek-harness-skills

A personal collection of reusable **skills** for the
[DeepSeek Harness (dsh)](https://github.com/deepseek-ai/deepseek-harness).
Each skill is a standard skill bundle (`<name>/SKILL.md` with YAML frontmatter)
that any dsh filesystem provider discovers and any agent can load.

| Skill | What it does |
|---|---|
| [`dsh-plugin-authoring`](dsh-plugin-authoring/SKILL.md) | Guidance distilled from a working session on **authoring a third-party dsh plugin** (node half, browser client half, or both): the bundle/profile installation model, the real client-bundle contract, and the deltas between the published plugin docs' sample code and the shipped harness — plus package layout, node/browser half rules, verification, and current version facts. |
| [`trufflehog-pre-push`](trufflehog-pre-push/SKILL.md) | **Scan a git project for secrets with TruffleHog in Docker before pushing** and gate on zero results — filesystem/history scan commands, `--fail` (exit 183) semantics, path exclusions, how to read verified vs unverified hits, the pushed-secrets-stay-in-history caveat, and a supplementary host-info sweep. |
| [`unslop`](unslop/SKILL.md) | **Cut AI tells from any writing** and add human voice. Scans for 31 patterns (puffery, AI vocabulary, overused em dashes and colons, chatbot phrases, filler, jargon), then guides a rewrite that keeps meaning and the intended tone. Must always apply when writing. Original source: [cursor/plugins: pstack/skills/unslop](https://github.com/cursor/plugins/tree/main/pstack/skills/unslop). |

## Security research skills

These run one campaign in the two-agent security-research preset: a root
orchestrator and one foreground worker at a time. They are general, not tied to
any one target.

| Skill | What it does |
|---|---|
| [`security-scope-interview`](security-scope-interview/SKILL.md) | Run a targeted scope conversation to produce a concrete `SCOPE.md`: authorization, attacker access, allowed and forbidden actions, baseline collection, a repeatable success proof, and resource bounds. |
| [`security-campaign-orchestration`](security-campaign-orchestration/SKILL.md) | Root-session routing: one causal gate per task packet, foreground-only delegation, reopen conditions on every closed family, mid-campaign scope amendments, restart handling, and closeout. |
| [`security-vulnerability-discovery`](security-vulnerability-discovery/SKILL.md) | Read-only discovery worker: map attacker-controlled paths to effects, cover the web front end and anonymous surface, and stop at the first unsupported edge. |
| [`security-anonymous-oracle-audit`](security-anonymous-oracle-audit/SKILL.md) | Enumerate and differential-test anonymous web routes (meta, slug, ID lookups) with the known-vs-unknown check before any read surface is certified clean. |
| [`security-attack-simulation`](security-attack-simulation/SKILL.md) | Simulation worker: build a bounded experiment with one semantic mutation, positive and negative controls, and before/after observations on the exact target epoch. |
| [`security-adversarial-validation`](security-adversarial-validation/SKILL.md) | Independent validation worker: falsify or reproduce a promoted candidate from frozen inputs on a clean target, returning supported, refuted, or inconclusive. |
| [`security-findings-triage`](security-findings-triage/SKILL.md) | Classify off-objective findings, decide compose vs park, and keep a parked finding's unpark condition attached so it is not lost. |
| [`security-release-delta-hunt`](security-release-delta-hunt/SKILL.md) | Diff the pinned epoch against prior releases (patch and feature windows) and decide whether new code carries a reachable primitive before live work. |
| [`security-infrastructure-operator`](security-infrastructure-operator/SKILL.md) | Generic infrastructure worker for AWS, Ludus, Docker, and local labs: baseline, readiness, cleanup, and the live-equals-committed-equals-frozen invariant. |
| [`security-ludus-range-operator`](security-ludus-range-operator/SKILL.md) | Ludus-specific guide: range lifecycle, role deploys, testing state as the clean-reset mechanism, snapshots, VM access, and re-freezing after changes. |
| [`security-session-log-export`](security-session-log-export/SKILL.md) | Freeze a campaign's session record into auditable transcripts and metadata at closeout, with the secrets policy stated up front. |

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
cp -R ~/dsh-skills/dsh-plugin-authoring \
      ~/dsh-skills/trufflehog-pre-push \
      ~/dsh-skills/unslop \
      ~/dsh-skills/security-* \
      ~/.agents/skills/

# or symlink instead, so `git pull` updates each in place:
ln -s ~/dsh-skills/dsh-plugin-authoring  ~/.agents/skills/dsh-plugin-authoring
ln -s ~/dsh-skills/trufflehog-pre-push   ~/.agents/skills/trufflehog-pre-push
ln -s ~/dsh-skills/unslop                ~/.agents/skills/unslop
for s in ~/dsh-skills/security-*; do ln -s "$s" ~/.agents/skills/; done
```

The provider watches these roots, so the skill appears in the next catalog
observation — no restart required for newly added skills on a running session's
next load.

### Using `$DSH_HOME/skills` instead

If you would rather keep dsh skills separate from `~/.agents` (which a
third-party GitHub-skill installer also manages via `~/.agents/.skill-lock.json`):

```bash
mkdir -p "$DSH_HOME/skills"     # or "$HOME/.dsh/skills"
cp -R ~/dsh-skills/dsh-plugin-authoring \
      ~/dsh-skills/trufflehog-pre-push \
      ~/dsh-skills/unslop \
      ~/dsh-skills/security-* \
      "$DSH_HOME/skills/"
```

## Verify

```bash
# the skill is discoverable when it appears in the catalog; for the web GUI,
# start a session and reference the skill by name, or list it via:
ls ~/.agents/skills/dsh-plugin-authoring/SKILL.md
ls ~/.agents/skills/trufflehog-pre-push/SKILL.md
ls ~/.agents/skills/unslop/SKILL.md
ls ~/.agents/skills/security-*/SKILL.md  # all security research skills
```

## Updating

```bash
git -C ~/dsh-skills pull
```

Copied skills need the copy refreshed (re-`cp`); symlinked skills pick up the
new content automatically.
