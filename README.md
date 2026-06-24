# claude-dev-team-skill

A portable, custom **software delivery team** for Claude Code — built to start
new projects and bring inherited ones up to standard, across any stack.

## What's inside

**Lifecycle**: 0 Solve (optional) → 1 Plan → 2 Build (+ design-critic loop) → 3 QA → 4 Security (code audit + skill/plugin/MCP supply-chain vetting via skill-auditor / NVIDIA SkillSpector) → 5 Docs → 6 Deploy → 7 Intelligence (optional, schedulable).

### Agents (13)
| Agent | Role |
|-------|------|
| `tech-lead-orchestrator` | Runs the pipeline and routes work between agents |
| `solve-strategist` | Optional pre-build product strategy + market research — evidence-based PRD that feeds the planner |
| `project-planner` | Precise requirement + dependency-ordered work plan |
| `design-engineer` | Crafted, non-generic, fully responsive UI — picks one Design Kit + Screen Blueprint, builds, then screenshots its work at 360/768/1280 |
| `backend-engineer` | Clean, optimized, modern backend/full-stack code |
| `design-critic` | Adversarial visual reviewer — judges the real screenshots, loops with design-engineer |
| `qa-breaker` | Adversarial QA — tries to break the code, loops with the dev |
| `security-auditor` | OWASP-style review before delivery |
| `skill-auditor` | Vets AI agent skills / MCP servers / third-party plugins for vulnerabilities + malicious patterns via NVIDIA SkillSpector — returns a risk score and an install/ship gate verdict |
| `doc-writer` | README, codemaps, API docs, ADRs |
| `devops-deploy` | CI/CD, containers, deploy + rollback |
| `intel-watcher` | Optional, recurring post-launch competitive-intelligence digest — built to run on a schedule via `/schedule` |
| `project-scout` | Read-only explorer/auditor for inherited repos |

### Skills (2)
| Skill | Purpose |
|-------|---------|
| `project-kickstart` | Bootstrap a new project or standardize an existing one |
| `design-language` | The crafted, anti-"AI-look" design reference the design-engineer studies — 6 curated Design Kits (Editorial SaaS, Utility Dashboard, Premium Commerce, Warm Commerce, Dev Tool, Refined Premium), 10 Screen Blueprints, a 3-layer Design Token Architecture, global craft guardrails, and a deterministic "AI-look" tell checklist (grow it with your own references) |

## Install

### As a plugin (recommended — portable across machines/projects)

1. Push this folder to a GitHub repo (e.g. `npalacios310/claude-dev-team-skill`).
2. In Claude Code:
   ```
   /plugin marketplace add https://github.com/npalacios310/claude-dev-team-skill
   /plugin install claude-dev-team-skill@claude-dev-team-skill
   ```

### As a user-level copy (this machine only)

```powershell
Copy-Item "agents\*.md" "$HOME\.claude\agents\" -Force
Copy-Item -Recurse "skills\*" "$HOME\.claude\skills\" -Force
```

> Don't stack both methods. If you install the plugin, remove the user-level
> copies to avoid duplicates.

## Usage

Let the tech-lead coordinate the whole flow, or call an agent directly:

```
Use the tech-lead-orchestrator to build <feature> end to end.
```

```
Use the design-engineer to design a responsive hero section.
```

```
Use the solve-strategist to research the market for <product idea> and produce an evidence-based PRD before we plan.
```

```
Use the intel-watcher to set up a recurring competitive-intelligence digest for <product> via /schedule.
```

```
Use the skill-auditor to vet https://github.com/<owner>/<skill-repo> before I install it, and give me an install gate verdict.
```

```
Use the skill-auditor to scan our skills/ directory before we publish, and block on any CRITICAL/HIGH findings.
```

## Notes

- **Stack-agnostic**: every agent detects the project's stack and adapts.
- The QA↔dev fix loop is driven by the tech-lead from the main session, since a
  Claude Code subagent cannot dispatch another subagent.
- `design-engineer` must deliver real screenshots (360/768/1280) and
  `design-critic` reviews them — loop driven by the tech-lead, max 3 rounds,
  then it escalates to the user.
- `skill-auditor` wraps **NVIDIA SkillSpector** (Apache-2.0, <https://github.com/NVIDIA/skillspector>) as an external tool — we integrate it, we don't vendor or copy its code. Static scans need no API key (the optional LLM semantic pass does); see `docs/SKILL-SECURITY.md` for setup.
- The design system's token architecture, craft guardrails, and expanded tell checklist draw on two studied open-source references — **impeccable** (Paul Bakaus, Apache-2.0, <https://github.com/pbakaus/impeccable>) and **ui-ux-pro-max-skill** (claudekit, MIT, <https://github.com/nextlevelbuilder/ui-ux-pro-max-skill>) — adapted in our own words, not copied.
- `author` / `owner` in `.claude-plugin/*.json` are set to `npalacios310` — change them if you fork.
