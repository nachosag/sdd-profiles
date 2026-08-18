# Gentle-AI SDD subagents

Canonical catalog of the 19 subagents of the SDD flow with their model needs.

| Agent | Category | Purpose | Reasoning | Context | Code-focused | Vision | Tools | Frequency | Duration | Loop | Error impact | Different family from |
|---|---|---|---|---|---|---|---|---|---|---|---|---|
| gentle-orchestrator | coordination | Coordinates the SDD flow: routing, delegation, and validation between phases. | low | 1M | no | no | yes | very high | very long | no | high | — |
| sdd-init | ingestion | Detects stack, scripts, conventions, and minimum project state. | none | 1M | no | no | yes | 1x | short | no | low | — |
| sdd-explore | code analysis | Investigates flows, dependencies, key files, and impact. | medium | 1M | yes | no | yes | medium | medium | no | medium | — |
| sdd-propose | pure reasoning | Defines the WHAT and WHY of the change; highest-impact decision. | high | 1M | no | no | optional | low | short-medium | no | critical | — |
| sdd-spec | technical writing | Formalizes requirements, contracts, and acceptance criteria. | low | high | yes | no | no | low | short | no | medium | — |
| sdd-design | design/architecture | Defines technical architecture, changes per layer, UI, and tests. | high | 1M | medium | yes | no | low | medium | no | high | — |
| sdd-tasks | formatting | Divides the design into atomic, ordered, verifiable tasks. | none | high | no | no | no | high | short | no | low | — |
| sdd-apply | coding agentic | Writes the code and implements tasks in batches. | high | 1M | yes | no | yes | very high | very long | yes | critical | ≠ verify |
| sdd-verify | audit | Audits diff, tests, regressions, security, and spec compliance. | high | 1M | yes | no | yes | high | medium | no | high | ≠ apply |
| sdd-archive | compression | Summarizes artifacts, leaves a log and commit notes. | none | 1M | no | no | no | 1x | very short | no | low | — |
| sdd-onboard | ingestion | Absorbs base context, structure, memory, and project rules. | none | 1M | no | yes | yes | 1x | medium | no | low | — |
| review-risk | extreme reasoning | Reviews security: vulnerabilities, permissions, data exposure. | max | 200K+ | yes | no | no | medium | short | no | critical | ideally ≠ apply |
| review-readability | light audit | Reviews naming, complexity, intent, and maintainability. | medium | 200K+ | yes | no | no | medium | short | no | medium | — |
| review-reliability | audit | Reviews tests, determinism, edge cases, and regressions. | medium | 1M | yes | no | no | medium | short | no | medium-high | — |
| review-resilience | audit | Reviews errors, retry/backoff, degradation, and observability. | medium | 1M | yes | no | no | medium | short | no | medium | — |
| review-refuter | extreme reasoning | Adversarially evaluates BLOCKER/CRITICAL findings and gives a verdict per finding. | max | 200K+ | yes | no | no | low | short | no | critical | ≠ lens that produced the finding |
| jd-judge-a | extreme reasoning | First blind judge of the Judgment Day (without bash). | max | 200K+ | yes | no | no | almost never | short | no | critical | ≠ judge B and ≠ implementer |
| jd-judge-b | extreme reasoning | Second blind judge, from a lab different from Judge A. | max | 200K+ | yes | no | no | almost never | short | no | critical | ≠ judge A and ≠ implementer |
| jd-fix-agent | directed coding | Applies the surgical corrections from the judges' verdicts. | low-medium | 200K+ | yes | no | yes | almost never | short | no | medium | — |

## How to read this table

- **Reasoning**: how much reasoning capacity the agent needs. Scale: `none` (null) → `low` → `medium` → `high` → `max` (extreme).
- **Context**: minimum context window required. `128K`, `200K`, `256K`, `1M` tokens, or `high`/`200K+` when there is no exact fixed number.
- **Code-focused**: whether the agent works focused on code (`yes`/`no`/`medium`).
- **Vision**: whether it needs visual/multimodal comprehension (`yes`/`no`).
- **Tools**: whether it needs tools/MCP access (`yes`/`no`/`optional`).
- **Frequency**: `1x` → `low` → `medium` → `high` → `very high`; `almost never` for emergency roles.
- **Duration**: `very short` → `short` → `short-medium` → `medium` → `very long`.
- **Loop**: whether the agent runs a sustained work loop (`yes`/`no`).
- **Error impact**: `low` → `medium` → `medium-high` → `high` → `critical`.
- **Different family from**: lab-independence restriction that the agent's model assignment must satisfy.
