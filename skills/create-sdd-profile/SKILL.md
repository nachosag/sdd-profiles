---
name: create-sdd-profile
description: "Trigger: create a new profile, add a profile, crear un perfil nuevo, agregar un perfil. Creates ONE new profile from a subscription without touching existing profiles."
license: MIT
metadata:
  author: "nachosag"
  version: "1.0"
---

## Activation Contract

Used when the user wants a NEW profile (e.g. "create a Free profile with Mistral's free plan"). Does not touch existing profiles.

## Hard Rules

- Isolation: never modify existing profiles.
- If the subscription is not in `subscriptions/`, fetch it first.
- Register the new subscription in `considerations.md`.

## Decision Gates

- Is the subscription catalog missing? → fetch before generating.

## Execution Steps

1. Identify the profile name + subscription(s).
2. If the catalog is missing, follow `../_shared/fetch-subscription.md`.
3. Add the subscription to `considerations.md` (`subscriptions` list).
4. Follow `../_shared/generate-profile.md` for that single profile.
5. Report.

## Output Contract

- The created profile (`profiles/<name>/<YYYY-MM>.md`).
- Confirmation that the others were not touched.

## References

- `../_shared/fetch-subscription.md`
- `../_shared/generate-profile.md`
- `../../considerations.md`
