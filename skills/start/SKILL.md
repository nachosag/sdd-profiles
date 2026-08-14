---
name: start
description: "Trigger: start, get started, set up, first time, comenzar, empezar, iniciar, configurar. Guides first-time setup: collects rules and considerations, then creates profiles."
license: MIT
metadata:
  author: "nachosag"
  version: "1.0"
---

## Activation Contract

Load when the user wants to START or set up the repo for the first time (or reconfigure from scratch). Trigger words: "start", "get started", "set up", "first time", "comenzar", "empezar", "iniciar", "configurar". This is the entry point BEFORE profiles exist.

## Hard Rules

- Never assume the user's subscriptions/budget — ask.
- Save answers to `considerations.md` and `rules.md` (gitignored, personal).
- Ask interactively (question tool or conversational) and never invent answers.
- If config already exists, confirm before overwriting.

## Decision Gates

| State | Action |
|---|---|
| `considerations.md` missing/empty | ask subscriptions, budget, privacy, preferences → save |
| `rules.md` missing/empty | offer universal rules (rules.example.md) + ask personal rules → save |
| `profiles/` empty | ask which profiles to create |
| config + profiles exist | confirm: reconfigure, or redirect to update-sdd-profiles |

## Execution Steps

1. Detect config state (`rules.md`, `considerations.md`, `profiles/`).
2. Collect subscriptions + budget + privacy + preferences → write `considerations.md` following `considerations.example.md`.
3. Collect personal rules (offer the examples from `rules.example.md`) → write `rules.md`.
4. Ask which profiles to create (name, purpose, subscriptions each uses).
5. For each profile, follow `../_shared/generate-profile.md`.
6. Report done + tell the user "next time, just say 'update my profiles'".

## Output Contract

- `considerations.md` + `rules.md` written.
- `profiles/<name>/<YYYY-MM>.md` generated.
- Summary of what was configured.

## References

- `../_shared/generate-profile.md`
- `../../rules.example.md`
- `../../considerations.example.md`
- `../../subagents.md`
