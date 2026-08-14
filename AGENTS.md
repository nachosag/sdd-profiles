# AGENTS

This repository keeps the model assignments of gentle-ai's SDD profiles in sync.

## Available skills

| Skill | Path | Description |
|---|---|---|
| `update-sdd-profiles` | `skills/update-sdd-profiles/SKILL.md` | Full flow: data sync + regenerate profiles. |
| `sync-sdd-profiles` | `skills/sync-sdd-profiles/SKILL.md` | Only refresh data and detect drift. |
| `create-sdd-profile` | `skills/create-sdd-profile/SKILL.md` | Create a new profile from a subscription. |

## Intent routing

- "update / sync / refresh my profiles" → `skills/update-sdd-profiles/SKILL.md` (full flow: sync + regenerate).
- "only refresh the data / see drift / fetch subscriptions" → `skills/sync-sdd-profiles/SKILL.md`.
- "create / add a new profile X with subscription Y" → `skills/create-sdd-profile/SKILL.md`.

## Note

Before acting, read (load) the corresponding `SKILL.md`. `skills/_shared/` contains support recipes and is not invokable.
