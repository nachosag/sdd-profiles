# generate-profile

Procedure for generating ONE model assignment profile.

## Inputs

- `subagents.md` — agent needs.
- `rules.md` — rules (if it does not exist → `rules.example.md`).
- `considerations.md` — parameters: subscriptions, budget, "expensive" threshold.
- `subscriptions/<x>/<YYYY-MM>.md` — model data.

## Algorithm

For each of the 19 agents, in order:
1. Filter models by cost range (according to frequency × impact).
2. Filter by minimum context, reasoning level, code-focus, vision, tool-calling.
3. Apply family restrictions:
   - P2: verify ≠ apply.
   - P3: judge A ≠ judge B.
   - P7: refuter ≠ lens.
4. Select primary + 1-2 fallbacks + thinking level + justification.

## Final checklist

Verify P1–P7 + golden rule before writing.

## Write

Create `profiles/<name>/<YYYY-MM>.md` with:
- Header: name, purpose, `subscriptions: [...]`, month.
- Assignment table for the 19 agents (primary / fallback / thinking / justification).
- Cost summary: average, premium models, Usage $15 models, free usage.
