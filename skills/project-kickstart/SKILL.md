---
name: project-kickstart
description: Bootstrap a new project or bring an existing one up to standard. Detects whether the repo is empty/new or already in progress, then asks the right questions, proposes structure, and applies a consistent quality baseline (tooling, tests, security, docs, git hygiene). Use when starting a fresh project, taking over a company/client codebase, or whenever the user says "new project", "set up this project", "inherit", "take over", "onboard me", or "bring this up to standard".
origin: custom
---

# Project Kickstart

Get any project to a solid, consistent baseline — whether it's brand new or one
you just inherited. The goal is the same in both cases: a project that is
understandable, testable, secure, and pleasant to work in, set up the same way
every time so quality does not depend on memory.

## When to Activate

- Starting a brand-new personal or work project from scratch
- Taking over a company or client codebase that already exists
- The user says: "new project", "set up this project", "I'm inheriting…",
  "take over this repo", "onboard me", "get this to standard", "kickstart"
- An existing repo feels messy and the user wants a consistent baseline applied

## Step 0: Detect the Mode

Look at the working directory before doing anything else:

- **New Project mode** — directory is empty, or has only a README/license, or
  `git log` shows no real source history.
- **Existing Project mode** — there is already source code, dependencies, and
  commit history.

State which mode you detected in one line, then follow that branch. If it is
ambiguous, ask the user once: *"Is this a fresh start or an existing project I
should bring up to standard?"*

---

## New Project Mode

### Step 1: Understand the Goal (ask, don't assume)

Ask 2-4 short questions, then stop and wait:
- What are we building, in one sentence? (personal, company, or client?)
- Primary language / stack? (or "you choose" → recommend and justify briefly)
- Is this throwaway/prototype or something that will live and grow?
- Any hard constraints? (deadline, deploy target, existing client requirements)

If the user says "just set it up" → proceed with sensible defaults and tell them
what you chose.

### Step 2: Propose Structure Before Creating It

Show a short plan **first** — folder layout, key files, and tooling choices —
and get a yes before writing files. Keep it minimal; do not scaffold features
that were not requested.

### Step 3: Lay the Baseline

Create only what the project actually needs from this checklist:
- **Version control**: `git init`, a real `.gitignore` for the stack, an initial commit
- **Dependency manifest**: `package.json` / `pyproject.toml` / `go.mod` / etc.
- **Entry point + one working "hello path"** that actually runs
- **Test setup**: a test runner wired up + one passing example test
- **Formatter + linter**: configured and runnable with one command
- **README**: what it is, how to run it, how to test it
- **Env handling**: `.env.example` (never commit real secrets)
- **CLAUDE.md** (optional): project-specific conventions so future sessions
  stay consistent

### Step 4: Verify It Actually Works

Run the build, the tests, and the lint command. Do not claim "set up" until a
command has actually succeeded. Report exactly what ran and what passed.

---

## Existing Project Mode

### Step 1: Map Before You Touch

For anything but a tiny repo, delegate this step to the **`project-scout`**
agent — it explores read-only and returns a structured map + quality audit.
Otherwise, do it inline. Either way, build a quick mental model:
- Detect stack, package manager, and how to run / test / build it
- Read the README and any CLAUDE.md / CONTRIBUTING / docs
- Skim the folder structure and entry points
- Check git: recent activity, branch model, who owns what
- Note what's already good vs missing

Deliver a short **onboarding summary**: what the project is, how to run it, the
architecture in 3-5 bullets, and the obvious risks/gaps.

### Step 2: Gap Analysis Against the Baseline

Compare the project to the same baseline as New Project mode and list what's
missing or weak, sorted by impact:
- Tests present and passing? Coverage meaningful?
- Linter/formatter configured and clean?
- Secrets in the repo? `.env` committed? Hardcoded keys?
- Dependencies outdated or vulnerable?
- README accurate enough for a new dev to run it?
- Dead code, broken builds, failing tests?

### Step 3: Propose, Don't Impose

Present the gaps as a prioritized plan and let the user pick what to apply.
**Never reformat or restructure an inherited codebase wholesale without
approval** — match the existing conventions unless asked to change them.

### Step 4: Apply Incrementally + Verify

Apply approved changes in small, reviewable steps. After each meaningful change,
run tests/build to prove nothing broke. Prefer additive fixes (add tests, add
config) over sweeping rewrites.

---

## Quality Baseline (both modes)

This is the standard the project should reach, regardless of how it started:

1. **It runs.** A documented command starts/builds it successfully.
2. **It's tested.** At least one real test, and a command to run the suite.
3. **It's clean.** Formatter + linter configured and passing.
4. **It's safe.** No secrets in the repo; `.env.example` documents needed vars.
5. **It's documented.** README covers what/how-to-run/how-to-test.
6. **It's tracked.** Git initialized, sensible `.gitignore`, clean commits.
7. **It's consistent.** Future work follows the same conventions (CLAUDE.md).

## Rules

1. **Detect the mode first** — never scaffold over an existing project.
2. **Ask before generating** structure or files; keep the footprint minimal.
3. **Match existing conventions** in inherited code unless told otherwise.
4. **Verify with a real command** before claiming anything is "done" or "set up".
5. **Never commit secrets.** Stop and flag if you find any already committed.
6. **Prioritize by impact** — fix what's risky or broken before what's cosmetic.

## Examples

```
"New project: a small CLI in Python to rename files in bulk"
"Set up this empty repo as a Next.js app with tests and linting"
"I just inherited this client repo — onboard me and tell me what's missing"
"Take over this codebase and bring it up to our standard"
"Kickstart a Go microservice, you pick sensible defaults"
```
