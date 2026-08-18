# Quickstart

Get from a fresh clone to generated profiles in about five minutes. Everything is driven by a coding agent that reads the skills in this repository — you mostly type short sentences.

## 1. Clone the repository

```bash
git clone https://github.com/<your-org>/sdd-profiles.git
cd sdd-profiles
```

## 2. Open your coding agent inside the repo

Any agent that reads `AGENTS.md` and the `skills/` directory works — OpenCode, Claude, Pi, or similar. Point it at this folder.

## 3. Say "start"

Type one of the trigger words:

> start

or "get started", "set up", "first time", "comenzar", "empezar", "iniciar", or "configurar".

The `start` skill takes over and guides you interactively. It will ask you for:

- **Subscriptions** — which plans you have, e.g. `opencode-go`, `opencode-zen-free`.
- **Budget** — your monthly cap and per-period limits.
- **Privacy** — whether free models (which may train on your data) are off-limits for client code.
- **Preferences** — optional leanings, e.g. preferring one model family for reasoning.
- **Which profiles** you want — e.g. `balanced`, `efficient`.

It writes your answers to `considerations.md` and `rules.md` (both personal and gitignored), then generates one `profiles/<name>/<YYYY-MM>.md` per profile you asked for.

## 4. Later, say "update my profiles"

When providers change their catalogs — which happens monthly — just tell your agent:

> update my profiles

(or "actualizar mis perfiles"). This runs the full flow: it re-fetches the latest subscription data and regenerates every profile against it.

## What you get

Each profile is a markdown file with a table mapping the 20 SDD subagents to a primary model, fallbacks, a thinking level, and a short justification — for example, `sdd-apply` in the balanced profile maps to `opencode-go/deepseek-v4-pro` with `qwen3.7-plus` as a fallback. See [Usage](./usage.md) for the complete workflow and [File reference](./file-reference.md) for what each file contains.
