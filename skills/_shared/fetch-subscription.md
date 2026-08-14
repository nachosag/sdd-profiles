# fetch-subscription

Procedure for fetching the current data of ONE subscription.

## Sources of truth per subscription

| Subscription | Data source |
|---|---|
| `opencode-go` | Docs `https://opencode.ai/docs/go/` (prices, limits, estimates, privacy) + endpoint `https://opencode.ai/zen/go/v1/models` (list/IDs) + validate IDs with the TUI's `/models` when available. |
| `opencode-zen-free` | Docs `https://opencode.ai/docs/zen/` + endpoint `https://opencode.ai/zen/v1/models`. |
| `openai`, `claude`, `gemini`, `mistral`, others | Source to be defined per provider (pricing page + official model list). Mark as pending. |

## Fields to extract per model

- name, exact ID, family/provider
- input/1M, output/1M, cached read, cached write
- max context
- reasoning (`none` / `built-in` / `configurable`)
- vision (yes/no), tool-calling (yes/no)
- usage/subsidy ($15 vs $60 in Go), req/5h
- notes (new / removed / deprecated, tiered prices)

## Write

Create `subscriptions/<name>/<YYYY-MM>.md` with:
- Header: name, plan/price, limits, privacy, source + fetch date.
- Model table.
- "Changes vs previous month" section.

## Diff

Compare against the previous month's file to detect:
- New models.
- Removed models.
- Repriced models.
- Context / usage changes.

Return the resulting drift report.
