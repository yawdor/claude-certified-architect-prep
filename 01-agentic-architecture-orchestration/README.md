# Domain 1: Agentic Architecture & Orchestration (27%)

Design and implement agentic systems using Claude's API, including loop management, orchestration patterns, guardrails, and the Claude Agent SDK.

**7 modules — all complete ✅**

## Modules

| # | Module | Notebook | View on mobile/browser |
|---|---|---|---|
| 1.1 | Agentic Loops | [1_1_agentic_loops.ipynb](src/1_1_agentic_loops.ipynb) | [nbviewer](https://nbviewer.org/github/yawdor/claude-certified-architect-prep/blob/main/01-agentic-architecture-orchestration/src/1_1_agentic_loops.ipynb) |
| 1.2 | Multi-Agent Orchestration | [1_2_orchestration_patterns.ipynb](src/1_2_orchestration_patterns.ipynb) | [nbviewer](https://nbviewer.org/github/yawdor/claude-certified-architect-prep/blob/main/01-agentic-architecture-orchestration/src/1_2_orchestration_patterns.ipynb) |
| 1.3 | Subagent Invocation and Context Passing | [1_3_subagent_invocation_context.ipynb](src/1_3_subagent_invocation_context.ipynb) | [nbviewer](https://nbviewer.org/github/yawdor/claude-certified-architect-prep/blob/main/01-agentic-architecture-orchestration/src/1_3_subagent_invocation_context.ipynb) |
| 1.4 | Workflow Enforcement and Handoff | [1_4_workflow_enforcement_handoff.ipynb](src/1_4_workflow_enforcement_handoff.ipynb) | [nbviewer](https://nbviewer.org/github/yawdor/claude-certified-architect-prep/blob/main/01-agentic-architecture-orchestration/src/1_4_workflow_enforcement_handoff.ipynb) |
| 1.5 | Agent SDK Hooks | [1_5_agent_sdk_hooks.ipynb](src/1_5_agent_sdk_hooks.ipynb) | [nbviewer](https://nbviewer.org/github/yawdor/claude-certified-architect-prep/blob/main/01-agentic-architecture-orchestration/src/1_5_agent_sdk_hooks.ipynb) |
| 1.6 | Task Decomposition Strategies | [1_6_task_decomposition.ipynb](src/1_6_task_decomposition.ipynb) | [nbviewer](https://nbviewer.org/github/yawdor/claude-certified-architect-prep/blob/main/01-agentic-architecture-orchestration/src/1_6_task_decomposition.ipynb) |
| 1.7 | Session State and Resumption | [1_7_session_state_resumption.ipynb](src/1_7_session_state_resumption.ipynb) | [nbviewer](https://nbviewer.org/github/yawdor/claude-certified-architect-prep/blob/main/01-agentic-architecture-orchestration/src/1_7_session_state_resumption.ipynb) |

## What each notebook covers

- **1.1 Agentic Loops** — the `stop_reason`-driven control loop behind every
  Claude agent; the classic content-type-checking bug that breaks it, reproduced live.
- **1.2 Multi-Agent Orchestration** — hub-and-spoke coordination, the
  isolation principle, and the narrow-decomposition failure that silently
  drops entire categories of work.
- **1.3 Subagent Invocation and Context Passing** — the real Claude Agent
  SDK's `Task`/`Agent` tool, verified against the installed package; an
  attribution-failure bug reproduced against a real captured API response.
- **1.4 Workflow Enforcement and Handoff** — deterministic prerequisite
  gates vs. probabilistic prompting, proven by adversarially telling the
  model to skip verification and watching the gate hold anyway.
- **1.5 Agent SDK Hooks** — `PreToolUse`/`PostToolUse` on real in-process
  MCP tools: data normalization and policy enforcement, hook dispatch and
  all.
- **1.6 Task Decomposition Strategies** — fixed pipelines vs. dynamic
  decomposition, and attention dilution demonstrated with a real, shared vs.
  dedicated token-budget experiment (not just asserted).
- **1.7 Session State and Resumption** — `--resume`, `fork_session`, and the
  stale-context bug reproduced on real files, real sessions, real fixes.

## Contents

Source code and exercises for this domain live in [src/](src/).
