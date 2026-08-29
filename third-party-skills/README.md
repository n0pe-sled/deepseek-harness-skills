# Third-party skills (ported)

Security review and testing skills vendored from two public sources, kept with
attribution. The selection rationale and the tradeoffs are in
[../SKILLS-REVIEW.md](../SKILLS-REVIEW.md).

| Source | License |
|---|---|
| [dreadnode/capabilities](https://github.com/dreadnode/capabilities) | MIT |
| [trailofbits/skills](https://github.com/trailofbits/skills) | CC BY-SA 4.0 |
| [trailofbits/skills-curated](https://github.com/trailofbits/skills-curated) | CC BY-SA 4.0 |

`unslop` (also vendored here) came from
[cursor/plugins: pstack/skills/unslop](https://github.com/cursor/plugins/tree/main/pstack/skills/unslop).

## Ported bundles

### Core review and auditing (install by default)

| Bundle | Source | License |
|---|---|---|
| `audit-context-building` | trailofbits/skills: plugins/audit-context-building | CC BY-SA 4.0 |
| `fp-check` | trailofbits/skills: plugins/fp-check | CC BY-SA 4.0 |
| `differential-review` | trailofbits/skills: plugins/differential-review | CC BY-SA 4.0 |
| `codeql` | trailofbits/skills: plugins/static-analysis/skills/codeql | CC BY-SA 4.0 |
| `semgrep` | trailofbits/skills: plugins/static-analysis/skills/semgrep | CC BY-SA 4.0 |
| `sarif-parsing` | trailofbits/skills: plugins/static-analysis/skills/sarif-parsing | CC BY-SA 4.0 |
| `supply-chain-risk-auditor` | trailofbits/skills: plugins/supply-chain-risk-auditor | CC BY-SA 4.0 |
| `vuln-assessment-methodology` | dreadnode/capabilities: vuln-assessment-methodology | MIT |
| `security-awareness` | trailofbits/skills-curated: plugins/security-awareness | CC BY-SA 4.0 |

### Web pentesting playbooks (opt in)

| Bundle | Source | License |
|---|---|---|
| `403-bypass` | dreadnode/capabilities: web-security | MIT |
| `csp-bypass` | dreadnode/capabilities: web-security | MIT |
| `ssrf-ip-filter-bypass` | dreadnode/capabilities: web-security | MIT |
| `graphql-pentest` | dreadnode/capabilities: web-security | MIT |
| `http-desync-smuggling` | dreadnode/capabilities: web-security | MIT |

### Fuzzing (opt in)

From trailofbits/skills: plugins/testing-handbook-skills, CC BY-SA 4.0:

`address-sanitizer`, `aflpp`, `atheris`, `cargo-fuzz`, `constant-time-testing`,
`coverage-analysis`, `fuzzing-dictionary`, `fuzzing-obstacles`,
`harness-writing`, `libafl`, `libfuzzer`, `ossfuzz`, `ruzzy`, `wycheproof`

### Smart contracts (opt in)

From trailofbits/skills: plugins/building-secure-contracts, CC BY-SA 4.0:

`algorand-vulnerability-scanner`, `audit-prep-assistant`,
`cairo-vulnerability-scanner`, `code-maturity-assessor`,
`cosmos-vulnerability-scanner`, `guidelines-advisor`, `secure-workflow-guide`,
`solana-vulnerability-scanner`, `substrate-vulnerability-scanner`,
`token-integration-analyzer`, `ton-vulnerability-scanner`

## Adaptation notes

Ports are not verbatim copies. For every bundle I:

- stripped `allowed-tools` from the frontmatter (a Claude-only key);
- rewrote `{baseDir}/...` paths to bundle-relative (`scripts/...`,
  `references/...`), resolved against the skill's own directory;
- removed Claude Code plumbing that does not port: `agents/` (openai.yaml),
  `assets/`, `workflows/`, `evals/`, `tests/`, `.claude-plugin/`;
- appended a `## DSH port notes` section to bundles whose orchestration used
  Claude slash commands, named agents, AskUserQuestion, or Task tools, mapping
  them onto dsh primitives (subagent, todo, chat prompts).

Nothing in this tree requires an MCP server. Skills that do (for example
`zeroize-audit`, which needs the serena MCP server) were left out; the MCP
servers worth adding for the web-playbook set are listed in SKILLS-REVIEW.md.
