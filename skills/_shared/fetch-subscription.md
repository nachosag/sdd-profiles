# fetch-subscription

Procedure for fetching the current data of ONE subscription.

## Sources of truth per subscription

| Subscription | IDs | Characteristics | Plan prices & limits |
|---|---|---|---|
| `opencode-go` | `https://opencode.ai/zen/go/v1/models` | `https://models.dev/api.json` → provider `opencode-go` | Docs `https://opencode.ai/docs/go/` + TUI `/models` |
| `opencode-zen-free` | `https://opencode.ai/zen/v1/models` | `https://models.dev/api.json` → provider `opencode` | Docs `https://opencode.ai/docs/zen/` |
| `openai`, `claude`, `gemini`, `mistral`, others | official model list | `https://models.dev/api.json` (if the provider is listed) | pricing page + docs — mark as pending |

## models.dev — field reference for SDD assignment

`https://models.dev/api.json` is the model registry **OpenCode itself uses** (~3.9 MB JSON, provider-keyed). It is the canonical source for model **capabilities** — use it for capabilities, not for plan prices.

| Field | What it tells you | Value for SDD assignment |
|---|---|---|
| `limit.context` | max context window (input + output) | Core filter: minimum context per agent. |
| `limit.input` | max **usable input** tokens — when present, may be **lower** than `context − output` (e.g. `big-pickle` 200K ctx → 160K input; `gpt-5.3-codex-spark` 128K input = context) | Real constraint for input-heavy agents (sdd-explore, sdd-apply): a 200K-context model can still cap input at 160K. When absent, treat `limit.context` as the input budget. |
| `limit.output` | max output tokens | **Long-generation agents** (sdd-apply, review reports) need large output. |
| `reasoning` + `reasoning_options` | reasoning support + exact options (`effort`, `toggle`, `budget_tokens`) | Core filter: reasoning level per agent + exact config values. |
| `family` | precise model family | **Lab independence P2/P3/P7** — finer than provider name (e.g. `deepseek-thinking` ≠ `deepseek-flash` → both can be used in one profile). |
| `tool_call` | tool/MCP support | Core filter: agents with tools. |
| `modalities.input` | input modalities (text/image/video/audio/pdf) | Core filter: vision (+ pdf for doc-heavy agents). |
| `structured_output` | JSON mode support | Agents returning structured results (verify report, tasks, skill outputs). |
| `knowledge` | knowledge cutoff | Freshness-sensitive agents (current frameworks/libraries). |
| `status` | `deprecated` flag | **Authoritative drift signal**: flag/remove deprecated models. |
| `cost.*` | raw *provider* prices | Tiered-pricing modeling only — NOT the plan price. |
| `cost.tiers` / `cost.context_over_200k` | price above a context tier | Long-context agents (>200K) cost more; use for cost modeling. |
| `temperature` | temperature support (bool) | Determinism for audit/reproducible agents; `false` = locked. |
| `interleaved` | `reasoning_content` field | Output parsing + reasoning-token accounting. |
| `open_weights` | open-weight flag | Licensing/privacy context. |
| `release_date` / `last_updated` | freshness | "New model" + staleness detection. |
| `description` | one-line purpose | Quick role matching. |
| `name` / `id` | identity | Keying and drift comparison. |

Not useful: `provider` (unpopulated in the opencode providers), `experimental` (absent), `attachment` (use `modalities.input` for vision).

⚠️ **Do NOT use `cost.*` from models.dev as the plan price.** The Go plan subsidizes models into `$15` vs `$60` usage tiers and rate-limits them (`req/5h`), so models.dev prices diverge from the plan (e.g. `deepseek-v4-pro` is `0.66/1.98` on models.dev vs `0.435/0.87` on the Go plan). Plan prices, subsidy tiers, limits, privacy, and `req/5h` come ONLY from the docs (`docs/go`, `docs/zen`) and the TUI `/models`.

## Fields to extract per model

- name, exact ID (models.dev `name`, `id`)
- family/provider (models.dev `family` — keep the precise family; provider name in parens if useful)
- input/1M, output/1M, cached read, cached write (docs / TUI — NOT models.dev)
- max context (models.dev `limit.context`)
- max input cap (models.dev `limit.input` — only present for some models; when absent, treat context as the input budget)
- max output (models.dev `limit.output`)
- reasoning (models.dev `reasoning` + `reasoning_options`; exact values, e.g. `effort [low, high, max]`)
- vision (yes/no), tool-calling (yes/no) (models.dev `modalities.input`, `tool_call`)
- structured output (yes/no) (models.dev `structured_output`)
- knowledge cutoff (models.dev `knowledge`)
- status (models.dev `status` — `deprecated` → flag in notes / drift)
- usage/subsidy ($15 vs $60 in Go), req/5h (docs / TUI)
- notes (new / removed / deprecated, tiered prices)

## Write

Create `subscriptions/<name>/<YYYY-MM>.md` with:
- Header: name, plan/price, limits, privacy, source + fetch date.
- Model table.
- "Changes vs previous month" section.

## Diff

Compare against the previous month's file to detect:
- New models (new ID / `release_date`).
- Removed models (missing ID or `status: deprecated` in models.dev).
- Repriced models (docs / TUI).
- Context / usage changes.
- Characteristics drift (re-check models.dev fields: context, output, reasoning, vision, tools, structured output).

Return the resulting drift report.
