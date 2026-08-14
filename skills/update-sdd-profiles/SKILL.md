---
name: update-sdd-profiles
description: "Trigger: update my profiles, refresh my profiles, actualizar mis perfiles, sincronizar mis perfiles. Runs data sync then regenerates all the user's profiles."
license: MIT
metadata:
  author: "nachosag"
  version: "1.0"
---

## Activation Contract

MAIN ENTRY POINT. Activates when the user says "update/sync/refresh my profiles" (interchangeably). Do not confuse with `sync-sdd-profiles` (data only).

## Hard Rules

- If `rules.md` or `considerations.md` do not exist or are empty → bootstrap first (ask for preferences, subscriptions, and rules; save them).
- Do not regenerate a profile without fresh data (sync first).
- Maintain the family invariant (verify ≠ apply, judge A ≠ B).

## Decision Gates

- Is the config present? If not → bootstrap.
- Is the data fresh? If not → sync before regenerating.

## Execution Steps

1. Bootstrap if config is missing.
2. Run sync (subscription fetch + subagent detection).
3. For each existing profile in `profiles/` (or those the user specifies), follow `../_shared/generate-profile.md` and regenerate `profiles/<name>/<YYYY-MM>.md`.

## Output Contract

- Regenerated profiles (list).
- Summary of changes vs previous month.

## References

- `../_shared/fetch-subscription.md`
- `../_shared/generate-profile.md`
- `../../subagents.md`
- `../../rules.example.md`
- `../../considerations.example.md`
