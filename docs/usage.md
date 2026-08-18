# Usage

This document describes the full workflow, from first setup to keeping your profiles fresh. Everything is driven by four skills; you invoke them with short trigger phrases in English or Spanish.

## The four skills

| Skill | What it does | Trigger words |
|---|---|---|
| `start` | First-time guided setup: collects your subscriptions, budget, privacy, preferences, and personal rules, then generates your first profiles. | "start", "get started", "set up", "first time", "comenzar", "empezar", "iniciar", "configurar" |
| `update-sdd-profiles` | Full flow: re-fetch the subscription data **and** regenerate all profiles. | "update my profiles", "refresh my profiles", "actualizar mis perfiles", "sincronizar mis perfiles" |
| `sync-sdd-profiles` | Data only: re-fetch subscription catalogs, detect new/removed subagents, and report drift. Does **not** regenerate profiles. | "sync data", "refresh catalogs", "drift", "refrescar datos", "fetch suscripciones" |
| `create-sdd-profile` | Create **one** new profile from a subscription, leaving existing profiles untouched. | "create a new profile X", "add a profile", "crear un perfil nuevo", "agregar un perfil" |

> Note: `update-sdd-profiles` is the main entry point. The word "sync" is ambiguous on purpose — saying "sync my profiles" also triggers the full `update-sdd-profiles` flow, not the data-only `sync-sdd-profiles`.

## The lifecycle

```
start ──► (use) ──► update ──► (use) ──► update ──► ...
                  │                │
                  ▼                ▼
               sync (curiosity)  create (new profile)
```

1. **`start`** — run once. The skill asks for your subscriptions, budget, privacy, preferences, and which profiles you want, then writes `considerations.md` and `rules.md` and generates the first `profiles/`.
2. **`update`** — run regularly (monthly, when catalogs change). It re-syncs the data and regenerates every profile.
3. **`sync`** — run when you're curious whether anything drifted, without wanting to touch your profiles yet.
4. **`create`** — run when you want a brand-new profile (e.g. a "free" profile using a different subscription), without disturbing the ones you have.

## The sync → update chain

`update-sdd-profiles` always runs a sync first. There's no such thing as regenerating against stale data — if the subscription catalogs haven't been re-fetched, the update refreshes them before regenerating. This is enforced by the skill's rules:

- If `rules.md` or `considerations.md` are missing or empty, `update` redirects you to `start` instead of guessing.
- If the data isn't fresh, `update` syncs before regenerating.
- `sync` alone never writes profiles and never deletes files — it only reports what changed.

## What a generated profile contains

Each `profiles/<name>/<YYYY-MM>.md` maps the 20 SDD subagents to models. For each agent the table records:

| Field | Meaning | Example (from `profiles/balanced/2026-08.md`) |
|---|---|---|
| **Primary model** | The default model for that agent. | `sdd-apply` → `opencode-go/deepseek-v4-pro` |
| **Fallback(s)** | One or two backup models, used when the primary isn't suitable or available. | `opencode-go/kimi-k2.7-code`, `opencode-go/qwen3.7-plus` |
| **Thinking level** | How much reasoning capacity the agent needs. | `high` for `sdd-apply` |
| **Justification** | Why this model was chosen, referencing the rules (e.g. P2 family separation) and the model's cost/capabilities. | "DeepSeek V4 Pro = sweet spot: 1M ctx + think modes at $0.435" |

The file also ends with a **summary**: the average primary cost, which premium models were used, how many agents use Usage $15 models, how many use free models, and — importantly — how many expensive models ended up in repetitive phases (the target is zero).

## Reading the assignment table alongside the catalog

A profile's justifications reference both the model catalog and the subscription data. For example, `opencode-go/deepseek-v4-pro` appears as the balanced profile's `sdd-apply` primary because, per `subscriptions/models.md`, it has 384K output, "effort [high, max]" reasoning, and structured output support, while per `subscriptions/opencode-go/2026-08.md`, it costs $0.435/1M input with 1M context and is described as the "workhorse of the stack". Cross-reference the three files when you want to understand *why* an assignment was made. See [File reference](./file-reference.md) for the exact columns of each.
