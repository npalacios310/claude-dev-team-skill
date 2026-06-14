# Skill Security — Vetting AI Agent Skills with SkillSpector

This plugin vets the AI agent skills, MCP servers, and third-party plugins you are about to **install** — and the ones your team **ships** — using [**NVIDIA SkillSpector**](https://github.com/NVIDIA/skillspector) (Apache-2.0), an open-source security scanner that answers one question: *is this skill safe to install?* It runs a two-stage pipeline — a fast, key-free static stage (regex + AST behavioral analysis + live OSV.dev CVE lookups) and an optional LLM semantic stage that filters false positives — across 64 patterns in 16 categories, from prompt injection and data exfiltration to supply-chain CVEs and MCP tool poisoning. This matters: in *Agent Skills in the Wild* (Liu et al., 2026), **26.1% of skills contained at least one vulnerability and 5.2% showed likely malicious intent** — and skills bundling executable scripts were 2.12× more likely to be vulnerable. SkillSpector is an external tool we wrap, not vendor; the `skill-auditor` agent operationalizes it. A clean scan **reduces** risk — it does not prove safety.

---

## Install

SkillSpector needs **Python 3.12+**. Pick one path.

### Docker (no local Python)

```bash
git clone https://github.com/NVIDIA/skillspector.git
cd skillspector
docker build -t skillspector .
docker run --rm -v "$PWD:/scan" skillspector scan ./my-skill/ --no-llm
```

The `-v "$PWD:/scan"` mount exposes the current directory to the container so the scanner can read your target.

### Python virtualenv (uv — recommended)

```bash
git clone https://github.com/NVIDIA/skillspector.git
cd skillspector
uv venv .venv && source .venv/bin/activate
uv pip install -e .
```

### Python virtualenv (stdlib pip)

```bash
git clone https://github.com/NVIDIA/skillspector.git
cd skillspector
python3 -m venv .venv && source .venv/bin/activate
pip install -e .
```

Verify the install:

```bash
skillspector --help
```

---

## Configure (optional LLM stage)

The static stage needs **no API key** — pass `--no-llm` and you are done. The optional LLM semantic stage (~87% precision, filters static false positives) needs a provider, configured entirely through environment variables. Its prompt ships with anti-jailbreak protections.

| `SKILLSPECTOR_PROVIDER` | What it talks to | Key / env to set |
|---|---|---|
| `openai` | OpenAI API (or any OpenAI-compatible server) | `OPENAI_API_KEY`; set `OPENAI_BASE_URL` for local Ollama/vLLM |
| `anthropic` | Anthropic API (bundled default model `claude-opus-4-6`) | `ANTHROPIC_API_KEY` |
| `nv_build` | NVIDIA build endpoint | per NVIDIA's provider docs |

Override the model for any provider with `SKILLSPECTOR_MODEL`. Example (Anthropic):

```bash
export SKILLSPECTOR_PROVIDER=anthropic
export ANTHROPIC_API_KEY=sk-ant-...
# optional: export SKILLSPECTOR_MODEL=claude-opus-4-6
skillspector scan ./my-skill/
```

For a local OpenAI-compatible server (Ollama/vLLM):

```bash
export SKILLSPECTOR_PROVIDER=openai
export OPENAI_BASE_URL=http://localhost:11434/v1
export OPENAI_API_KEY=ollama   # placeholder; many local servers ignore it
skillspector scan ./my-skill/
```

> Skipping the LLM stage (`--no-llm`) keeps scans fast, offline, and free — at the cost of more false positives to triage.

---

## Usage

`skillspector scan <target>` accepts four input types — a directory, a single `SKILL.md`, a git repo URL, or a zip:

```bash
skillspector scan ./my-skill/                                   # directory
skillspector scan ./my-skill/SKILL.md                           # single file
skillspector scan https://github.com/some/skill-repo            # git URL
skillspector scan ./downloaded-skill.zip                        # zip archive
```

### Flags (the complete set)

| Flag | Purpose |
|---|---|
| `-f`, `--format [terminal\|json\|markdown\|sarif]` | output format (default `terminal`) |
| `-o`, `--output PATH` | write the report to a file |
| `--no-llm` | static stage only — no API key required |
| `-V`, `--verbose` | detailed output |
| `--help` | usage |

There are **no other flags** — do not invent any.

### Reading the result

Every scan returns:

| Field | Meaning |
|---|---|
| `risk_score` | 0–100; higher is worse |
| `risk_severity` | the headline severity bucket |
| `risk_recommendation` | the tool's plain-language verdict |
| `filtered_findings[]` | each with `severity`, `rule_id`, `message` |

### Install / ship gate

The `skill-auditor` agent turns the score into a gate decision. Driven by the `tech-lead-orchestrator`:

| Verdict | Action |
|---|---|
| **CRITICAL or HIGH** | **Block** install/ship → route to remediation or reject. The user must explicitly sign off to override. |
| **MEDIUM** | Proceed with documented caution; fix before shipping client/production work. |
| **LOW / clean** | Proceed. |

Use it two ways: **pre-install** vetting before adding any third-party skill/plugin/MCP, and as a **ship gate** on the skills, agents, and MCP configs your own team produces.

### Python API

```python
from skillspector import graph

result = graph.invoke({
    "input_path": "./my-skill/",
    "output_format": "json",
    "use_llm": True,
})
```

---

## CI

Emit **SARIF** so findings surface in CI dashboards, PR annotations, and IDEs. Run static-only in pipelines to avoid needing an API key on runners:

```bash
skillspector scan ./my-skill/ --no-llm --format sarif --output report.sarif
```

Upload `report.sarif` with your platform's code-scanning step (e.g. GitHub's `upload-sarif` action) and fail the job on CRITICAL/HIGH per the gate above.

---

## Self-scan (dogfood)

> **Status: not yet run.** Running SkillSpector requires installing an external
> package (`pip install -e .`), which executes third-party code — so this plugin
> ships without a self-scan on record rather than running it unvetted in an
> automated context. To certify this plugin yourself, run a static scan (no API
> key, nothing sent off-machine):
>
> ```bash
> git clone https://github.com/NVIDIA/skillspector.git && cd skillspector
> uv venv .venv && source .venv/bin/activate && uv pip install -e .
> # then, from anywhere:
> skillspector scan /path/to/claude-dev-team-skill --no-llm --format markdown
> ```
>
> Paste the resulting risk score and findings here to keep a dated record. The
> gate we hold ourselves to: ship only on a `LOW`/none verdict with no
> `CRITICAL`/`HIGH` findings.

---

## Limitations

- **English-centric** — may miss patterns expressed in other languages.
- **Cannot read text in images** — instructions hidden in screenshots are invisible to it.
- **Static, not dynamic** — no runtime/sandbox execution; it reasons about code, it does not run it.
- **Offline SC4** — without network access, live OSV.dev CVE lookups fall back to a small static list.

A clean scan reduces risk — it does not prove safety.
