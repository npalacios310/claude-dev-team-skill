# claude-dev-team

A portable, custom **software delivery team** for Claude Code — built to start
new projects and bring inherited ones up to standard, across any stack.

## What's inside

### Agents (9)
| Agent | Role |
|-------|------|
| `tech-lead-orchestrator` | Runs the pipeline and routes work between agents |
| `project-planner` | Precise requirement + dependency-ordered work plan |
| `design-engineer` | Crafted, non-generic, fully responsive UI |
| `backend-engineer` | Clean, optimized, modern backend/full-stack code |
| `qa-breaker` | Adversarial QA — tries to break the code, loops with the dev |
| `security-auditor` | OWASP-style review before delivery |
| `doc-writer` | README, codemaps, API docs, ADRs |
| `devops-deploy` | CI/CD, containers, deploy + rollback |
| `project-scout` | Read-only explorer/auditor for inherited repos |

### Skills (2)
| Skill | Purpose |
|-------|---------|
| `project-kickstart` | Bootstrap a new project or standardize an existing one |
| `design-language` | The crafted, anti-"AI-look" design reference the design-engineer studies (grow it with your own references) |

## Install

### As a plugin (recommended — portable across machines/projects)

1. Push this folder to a GitHub repo (e.g. `your-user/claude-dev-team`).
2. In Claude Code:
   ```
   /plugin marketplace add https://github.com/your-user/claude-dev-team
   /plugin install claude-dev-team@claude-dev-team
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

## Notes

- **Stack-agnostic**: every agent detects the project's stack and adapts.
- The QA↔dev fix loop is driven by the tech-lead from the main session, since a
  Claude Code subagent cannot dispatch another subagent.
- Edit `author` / `owner` placeholders in `.claude-plugin/*.json` before publishing.
