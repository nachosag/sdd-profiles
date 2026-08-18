# generate-profile

Procedure for generating ONE model assignment profile.

## Inputs

- `subagents.md` — agent needs.
- `rules.md` — rules (if it does not exist → `rules.example.md`).
- `considerations.md` — parameters: subscriptions, budget, "expensive" threshold.
- `subscriptions/models.md` — canonical model capabilities.
- `subscriptions/<x>/<YYYY-MM>.md` — provider-controlled data (pricing, context caps, rate limits).

## Merge logic

Before generating, build a merged view of each model by combining:

1. **models.md row** (matched by model name): family, output cap, reasoning, vision, tools, structured output, knowledge.
2. **Subscription row**: ID, pricing, context, input cap, usage, req/5h.
3. **Provider overrides** (if present in the subscription's "Provider overrides" section): override the corresponding models.md field.

The merged model has all fields needed for assignment: capabilities from models.md + economics from the subscription + any provider-specific overrides.

## Algorithm

For each of the 20 agents, in order:
1. Filter models by cost range (according to frequency × impact). Apply tiered pricing when the agent feeds >200K context.
2. Filter by minimum context, reasoning level, code-focus, vision, tool-calling (from merged view).
3. Apply output and format constraints:
   - Output cap (from merged view) ≥ the agent's generation size (long-generation agents: sdd-apply, review roles).
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
