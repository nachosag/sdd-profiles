# Architecture

This document explains *why* the repository is structured the way it is. If you just want to use it, read the [Quickstart](./quickstart.md); if you want to understand how everything fits together, keep reading.

## The problem

This repository answers one recurring question: **which AI model should each of gentle-ai's SDD subagents use?**

Answering that question once is easy. Keeping the answer correct over time is the hard part, because two things change independently and at different speeds:

1. **Model catalogs change monthly.** Providers add models, remove them, deprecate them, and re-price them. The `opencode-go` subscription went from removing MiniMax M2.5 in August 2026 to adding GPT 5.6 Luna and Qwen3.8 Max. The `opencode-zen-free` subscription swapped North Mini Code Free for Hy3 Free and Nemotron 3.5 Lightning Free in the same month.
2. **gentle-ai adds subagents over time.** The SDD flow already has 19 agents (orchestrator, init, explore, propose, spec, design, tasks, apply, verify, archive, onboard, five reviewers, and three judgment-day roles), and new ones appear as the workflow evolves.

A single handwritten document that mixes "what each agent needs" with "what models your subscription currently offers" goes stale almost immediately: the moment one provider changes a price, every assignment built on top of that price is wrong.

## The solution: separate by rate of change

The repository splits the problem into three layers, ordered by how often each one changes.

| Layer | Changes | Files | What it holds |
|---|---|---|---|
| **Stable knowledge** | Rarely (when gentle-ai adds an agent or the assignment principles change) | `subagents.md`, `rules.example.md` | What the 19 agents are and what they need; the universal rules (P1–P7, decision matrix, golden rule) that never depend on which models you happen to have. |
| **Volatile data** | Monthly (when providers change catalogs) | `subscriptions/<name>/<YYYY-MM>.md` | The model catalog of each subscription for a given month — prices, context, reasoning, vision, tools, usage, and request limits. |
| **Derived output** | Monthly (recomputed from the two layers above) | `profiles/<name>/<YYYY-MM>.md` | The actual assignments: which model each agent gets, plus fallbacks, thinking level, and the justification. |

The key idea is that **the derived output is never edited by hand.** A profile is *generated* by combining the stable knowledge (agent needs + rules) with the volatile data (current model catalog). When a provider changes a price, you re-fetch the data and regenerate the profile — you don't hand-edit 19 rows.

This mirrors a common engineering pattern: keep the *rules* and the *data* separate, and treat *output* as reproducible rather than as source.

## Core concepts

| Concept | Definition |
|---|---|
| **Subagent** | One of the 19 agents in the SDD flow (e.g. `sdd-apply`, `review-risk`, `jd-judge-b`). Each has a defined profile of needs — reasoning level, minimum context, whether it needs vision or tools, how often it runs, and how much an error costs. |
| **Subscription** | A specific plan you have access to (e.g. `opencode-go`, `opencode-zen-free`). Each subscription has a *catalog*: the list of models it exposes, with prices and capabilities. |
| **Profile** | A named assignment strategy (e.g. `balanced`, `efficient`) that maps each of the 19 agents to a primary model and fallbacks, given one or more subscriptions and your budget. |
| **Rule** | A directive that tells the generator *how to choose*. Rules live in `rules.example.md` (universal, committed) and `rules.md` (personal, gitignored). |
| **Consideration** | A fact about *your* context — which subscriptions you have, your budget, your privacy constraints. Considerations live in `considerations.example.md` (template, committed) and `considerations.md` (personal, gitignored). |

See [Rules vs considerations](./rules-vs-considerations.md) for a full treatment of the last two.

## An analogy

Imagine a workshop with 19 different job posts: some need deep reasoning but run rarely, some grind through repetitive code all day, some must be independent of each other. The mechanics who fill those posts come from agencies whose catalogs change every month — a model gets added here, retired there, re-priced somewhere else.

- The **job descriptions** (what each post needs) barely change — that's `subagents.md`.
- The **hiring rules** (don't put the same mechanic on "build" and "audit"; expensive specialists only on high-impact jobs) are stable — that's `rules.example.md`.
- The **agency catalogs** change monthly — those are `subscriptions/`.
- The **actual staffing chart**, re-derived whenever a catalog changes, is the `profiles/` layer.

You never re-write the job descriptions or the hiring rules just because an agency changed its prices; you re-run the staffing.

## How the layers connect

```
subagents.md ─────────────┐
rules.example.md / rules.md ──┤
considerations.example.md / considerations.md ──┤
subscriptions/<name>/<YYYY-MM>.md ──┴──►  generate-profile  ──►  profiles/<name>/<YYYY-MM>.md
```

The generation procedure lives in `skills/_shared/generate-profile.md` and is driven by the [skills](./usage.md): `start` gathers the personal context and produces the first profiles, `sync-sdd-profiles` refreshes the subscription data, and `update-sdd-profiles` re-runs generation over the fresh data.
