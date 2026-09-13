# Claude Certified Architect (CCAR-F) Prep

[![License: MIT](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)
![Progress](https://img.shields.io/badge/progress-7%2F30_modules-brightgreen)
![Domain 1](https://img.shields.io/badge/Domain%201-complete-success)

Hands-on, runnable Jupyter notebooks for every module of the
[**Claude Certified Architect (CCAR-F)**](https://claudecertificationguide.com/learn)
exam — built by actually calling the real Anthropic API, not by summarizing
slides.

## Why this repo is different

Most exam-prep material is notes and flashcards. This is source code:

- 🔌 **Real API calls, no mocks** — every notebook uses the real `anthropic`
  SDK or the real `claude-agent-sdk`, whichever the module actually teaches.
  Nothing here is simulated.
- 🐛 **Anti-patterns you can watch fail** — every documented mistake is
  written as real, working code, then disabled with a comment explaining
  why — and named case-study bugs are reproduced *live*, not just described.
- ✅ **Validated before it ships** — every notebook passes `nbformat`
  validation, per-cell syntax checks, and a full dry run against the real
  SDK's own types before being committed. See a build log's worth of caught
  bugs in the [commit history](https://github.com/yawdor/claude-certified-architect-prep/commits/main) —
  real bugs, caught and fixed before shipping, not after.
- 💸 **Cost and runtime disclosed upfront** — each notebook tells you
  honestly what a full run will cost and roughly how long it takes before
  you spend anything.

## 📓 Domain 1 — Agentic Architecture & Orchestration (complete)

All 7 modules. Click **View** to read on your phone or browser (renders
instantly, no GitHub app needed) — click **Source** to open the notebook
itself.

| # | Module | | |
|---|---|---|---|
| 1.1 | Agentic Loops | [View](https://nbviewer.org/github/yawdor/claude-certified-architect-prep/blob/main/01-agentic-architecture-orchestration/src/1_1_agentic_loops.ipynb) | [Source](01-agentic-architecture-orchestration/src/1_1_agentic_loops.ipynb) |
| 1.2 | Multi-Agent Orchestration | [View](https://nbviewer.org/github/yawdor/claude-certified-architect-prep/blob/main/01-agentic-architecture-orchestration/src/1_2_orchestration_patterns.ipynb) | [Source](01-agentic-architecture-orchestration/src/1_2_orchestration_patterns.ipynb) |
| 1.3 | Subagent Invocation and Context Passing | [View](https://nbviewer.org/github/yawdor/claude-certified-architect-prep/blob/main/01-agentic-architecture-orchestration/src/1_3_subagent_invocation_context.ipynb) | [Source](01-agentic-architecture-orchestration/src/1_3_subagent_invocation_context.ipynb) |
| 1.4 | Workflow Enforcement and Handoff | [View](https://nbviewer.org/github/yawdor/claude-certified-architect-prep/blob/main/01-agentic-architecture-orchestration/src/1_4_workflow_enforcement_handoff.ipynb) | [Source](01-agentic-architecture-orchestration/src/1_4_workflow_enforcement_handoff.ipynb) |
| 1.5 | Agent SDK Hooks | [View](https://nbviewer.org/github/yawdor/claude-certified-architect-prep/blob/main/01-agentic-architecture-orchestration/src/1_5_agent_sdk_hooks.ipynb) | [Source](01-agentic-architecture-orchestration/src/1_5_agent_sdk_hooks.ipynb) |
| 1.6 | Task Decomposition Strategies | [View](https://nbviewer.org/github/yawdor/claude-certified-architect-prep/blob/main/01-agentic-architecture-orchestration/src/1_6_task_decomposition.ipynb) | [Source](01-agentic-architecture-orchestration/src/1_6_task_decomposition.ipynb) |
| 1.7 | Session State and Resumption | [View](https://nbviewer.org/github/yawdor/claude-certified-architect-prep/blob/main/01-agentic-architecture-orchestration/src/1_7_session_state_resumption.ipynb) | [Source](01-agentic-architecture-orchestration/src/1_7_session_state_resumption.ipynb) |

Full write-up and module checklist: [01-agentic-architecture-orchestration/](01-agentic-architecture-orchestration/)

## Exam Details

| | |
|---|---|
| Questions | 60 in 120 minutes |
| Cost | $125 USD |
| Passing score | 720 / 1000 |
| Validity | 12 months |
| Version | v1.0, July 2026 |

## Domains

30 task statements across 5 domains. Each domain has its own folder with a
`src/` directory holding that domain's notebooks.

| # | Domain | Weight | Modules | Status | Folder |
|---|--------|--------|---------|--------|--------|
| 1 | Agentic Architecture & Orchestration | 27% | 7/7 | ✅ Complete | [01-agentic-architecture-orchestration](01-agentic-architecture-orchestration/) |
| 2 | Tool Design & MCP Integration | 18% | 0/5 | ⏳ Not started | [02-tool-design-mcp-integration](02-tool-design-mcp-integration/) |
| 3 | Claude Code Configuration & Workflows | 20% | 0/6 | ⏳ Not started | [03-claude-code-configuration-workflows](03-claude-code-configuration-workflows/) |
| 4 | Prompt Engineering & Structured Output | 20% | 0/6 | ⏳ Not started | [04-prompt-engineering-structured-output](04-prompt-engineering-structured-output/) |
| 5 | Context Management & Reliability | 15% | 0/6 | ⏳ Not started | [05-context-management-reliability](05-context-management-reliability/) |

## Running a notebook yourself

Each notebook is self-contained — install its dependency, set your API key,
open it, run every cell top to bottom.

```bash
pip install anthropic          # most modules
pip install claude-agent-sdk   # modules that use the Claude Agent SDK (noted at the top of the notebook)
```

```bash
# Windows (PowerShell)
$env:ANTHROPIC_API_KEY = "sk-ant-..."

# macOS / Linux (bash/zsh)
export ANTHROPIC_API_KEY="sk-ant-..."
```

Every notebook states its real API cost and runtime up front — most run in
under a minute for a few cents; a few that use tools like web search or
multi-step subagents take longer (up to a couple of minutes) and cost a bit
more, and say so before you run them.

## How this repo gets built

Each notebook is generated by a repeatable process, not written ad hoc:
fetch the module's real content, verify any unfamiliar SDK against the
*installed package* rather than trusting pseudocode, generate the notebook,
then validate it — JSON structure, per-cell syntax, and a full dry run
against real SDK types — before it's ever committed. The process itself is
a reusable [Claude Code](https://claude.com/claude-code) skill, refined with
a real lesson learned from nearly every module so far.

## Structure

```
├── 01-agentic-architecture-orchestration/
│   ├── README.md          # domain description + module checklist
│   └── src/                # notebooks live here
├── 02-tool-design-mcp-integration/
├── 03-claude-code-configuration-workflows/
├── 04-prompt-engineering-structured-output/
└── 05-context-management-reliability/
```

## Following along

This repo is updated module by module as I work through the certification.
⭐ **Star it** to follow along, or open an issue if you spot something
worth fixing — corrections are always welcome, especially since the exam
guide itself is a living document.

## License

[MIT](LICENSE) — use it, fork it, learn from it.
