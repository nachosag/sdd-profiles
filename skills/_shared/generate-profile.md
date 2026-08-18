# generate-profile

Procedure for generating ONE model assignment profile.

## Inputs

- `subagents.md` — agent needs.
- `rules.md` — rules (if it does not exist → `rules.example.md`).
- `considerations.md` — parameters: subscriptions, budget, "expensive" threshold.
- `subscriptions/<x>/<YYYY-MM>.md` — model data.

## Algorithm

For each of the 20 agents, in order:
1. Filter models by cost range (according to frequency × impact). Apply tiered pricing when the agent feeds >200K context (`cost.context_over_200k` / `cost.tiers`).
2. Filter by minimum context, reasoning level, code-focus, vision, tool-calling.
3. Apply output and format constraints:
   - `limit.output` ≥ the agent's generation size (long-generation agents: sdd-apply, review roles).
   - `structured_output` required for agents returning structured results (verify report, tasks, skill outputs).
   - Prefer a fresh `knowledge` cutoff for agents working with current frameworks/libraries.
   - For reproducible/audit agents, prefer `temperature` support (set 0); `false` means the model is locked.
4. Apply family restrictions (use the precise models.dev `family`, not the provider name):
   - P2: verify ≠ apply.
   - P3: judge A ≠ judge B.
   - P7: refuter ≠ lens.
5. Select primary + 1-2 fallbacks + thinking level + justification.

## Final checklist

Verify P1–P7 + golden rule before writing.

## Write

Create `profiles/<name>/<YYYY-MM>.md` with:
- Header: name, purpose, `subscriptions: [...]`, month.
- Assignment table for the 20 agents (primary / fallback / thinking / justification).
- Cost summary: average, premium models, Usage $15 models, free usage.
