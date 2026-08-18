---
name: sync-sdd-profiles
description: "Trigger: sync subscription data, refresh catalogs, drift, refrescar datos, fetch suscripciones. Syncs model capabilities to models.md and fetches subscription catalogs, then reports drift."
license: MIT
metadata:
  author: "nachosag"
  version: "1.0"
---

## Activation Contract

Used when the user wants to ONLY refresh the data (subscription catalogs + subagent detection), without regenerating profiles. Not the main entry point (see `update-sdd-profiles`).

## Hard Rules

- Do not delete files.
- Do not regenerate profiles (that is done by `update-sdd-profiles`).
- New subagent = report, do NOT auto-write into `subagents.md`.

## Decision Gates

- If there are no subscriptions in `considerations.md` → ask the user before continuing.
- If a provider has no defined source → mark as pending and report, do not invent data.

## Execution Steps

1. Read `considerations.md` → get the user's subscriptions.
2. For each subscription, follow `../_shared/fetch-subscription.md` and write `subscriptions/<name>/<YYYY-MM>.md`.
3. Check gentle-ai (repo/docs) to detect new or removed subagents.
4. Emit the drift report.
5. Propose regenerating profiles (if the user accepts, invoke `update-sdd-profiles`).

## Output Contract

- Drift report: changed subscriptions, new/removed/repriced models, capability changes in models.md, new subagents.
- Written `subscriptions/models.md` (synced model capabilities).
- Written `subscriptions/<name>/` files (provider-controlled data).

## References

- `../_shared/fetch-subscription.md`
- `../../considerations.md`
- `../../subagents.md`
