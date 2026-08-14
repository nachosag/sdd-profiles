# SDD Profiles

A repository that documents which AI model to assign to each of gentle-ai's SDD subagents, per profile, kept in sync with the models your subscriptions actually offer each month.

## 3-layer structure

Separating things by how often they change keeps the repo from going stale:

```text
┌─────────────────────────────────────────────────────────────────┐
│  Stable knowledge      subagents.md · rules.example.md          │  ← changes rarely
├─────────────────────────────────────────────────────────────────┤
│  Volatile data         subscriptions/<name>/<YYYY-MM>.md        │  ← changes monthly
├─────────────────────────────────────────────────────────────────┤
│  Derived output        profiles/<name>/<YYYY-MM>.md             │  ← regenerated, not hand-edited
└─────────────────────────────────────────────────────────────────┘
```

| Layer | Files | What it contains |
|---|---|---|
| Stable knowledge | `subagents.md`, `rules.example.md` | The 19 SDD subagents and their needs, plus the universal assignment rules (P1–P7, decision matrix, golden rule). |
| Volatile data | `subscriptions/` | Each subscription's model catalog — prices, context, reasoning, vision, tools, usage, limits — versioned by month. |
| Derived output | `profiles/` | The model assignment per profile (balanced, efficient), regenerated monthly from the two layers above. |
| Personal config | `rules.md`, `considerations.md` | Your own rules and context, gitignored (templates in `*.example.md`). |

## Quick start

1. **Clone** the repository.
2. Open your coding agent (OpenCode, Claude, Pi, …) inside it.
3. Say **"start"** — the `start` skill asks for your subscriptions, budget, and preferences, then generates your profiles.
4. Later, say **"update my profiles"** to refresh the data and regenerate.

## Skills

| Skill | Trigger | What it does |
|---|---|---|
| `start` | "start", "get started", "comenzar", "empezar" | First-time guided setup: collects rules/considerations, creates profiles. |
| `update-sdd-profiles` | "update/refresh my profiles" / "actualizar mis perfiles" | Full flow: sync data + regenerate all profiles. |
| `sync-sdd-profiles` | "sync data", "drift", "refrescar datos" | Refresh data only and report drift. |
| `create-sdd-profile` | "create a new profile X" / "crear un perfil nuevo" | Add one new profile without touching existing ones. |

## Documentation

| Doc | What it covers |
|---|---|
| [Architecture](./docs/architecture.md) | Why the repo is split into three layers, and the core concepts. |
| [Quickstart](./docs/quickstart.md) | The five-minute path from clone to generated profiles. |
| [Rules vs considerations](./docs/rules-vs-considerations.md) | The distinction between directives (rules) and facts about your context (considerations). |
| [Usage](./docs/usage.md) | The full workflow: the four skills, triggers, and the sync→update chain. |
| [File reference](./docs/file-reference.md) | Every file and directory, its purpose and format. |
| [Contributing](./docs/contributing.md) | How to add subscriptions, profiles, and improve the shared knowledge. |
