# SDD Profiles

Repository documenting which AI models to assign to each Gentle-AI subagent (the 19 agents of the SDD flow), per profile (Balanced, Efficient), kept in sync with the available models of each subscription.

## 3-layer structure

| Layer | Files | What it contains |
|---|---|---|
| Stable knowledge | `subagents.md`, `rules.example.md` | Catalog of the 19 subagents and universal assignment rules (P1–P7, decision matrix, golden rule). |
| Versioned data | `subscriptions/`, `profiles/` | Models and prices of each subscription, and assignments per profile, organized by month. |
| Personal config | `rules.md`, `considerations.md` | The user's own rules and considerations, gitignored (templates in `*.example.md`). |

## Quick start

1. Clone the repository.
2. Create `rules.md` and `considerations.md` from the `*.example.md` templates and fill them in with your context.
3. Tell your agent which profiles (`balanced`, `efficient`) and subscriptions (`opencode-go`, `opencode-zen-free`) you want to use.
4. When the models change, ask it to "update my profiles".

## Skills

The skills (agents that automate creating, updating, and verifying profiles) are created in a later phase, under `skills/`.
