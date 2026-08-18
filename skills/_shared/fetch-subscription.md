# fetch-subscription

Procedure for fetching the current data of ONE subscription.

## Sources of truth

| Source | What it provides | File target |
|---|---|---|
| `https://models.dev/api.json` | Model capabilities (provider-keyed) | `subscriptions/models.md` |
| Docs (`docs/go`, `docs/zen`) + TUI `/models` | Plan prices, limits, rate limits, privacy | `subscriptions/<name>/<YYYY-MM>.md` |
| `https://opencode.ai/zen/go/v1/models` | Model IDs for opencode-go | `subscriptions/<name>/<YYYY-MM>.md` |
| `https://opencode.ai/zen/v1/models` | Model IDs for opencode-zen-free | `subscriptions/<name>/<YYYY-MM>.md` |

## models.dev — field reference for SDD assignment

`https://models.dev/api.json` is the model registry **OpenCode itself uses** (~3.9 MB JSON, provider-keyed). It is the canonical source for model **capabilities** — use it for capabilities, not for plan prices.

### Fields → models.md (inherent characteristics)

| Field | What it tells you | Value for SDD assignment |
|---|---|---|
| `name` | canonical model name | Key for matching across files |
| `family` | precise model family | **Lab independence P2/P3/P7** — finer than provider name |
| `limit.output` | max output tokens | **Long-generation agents** (sdd-apply, review reports) need large output |
| `reasoning` + `reasoning_options` | reasoning support + exact options | Core filter: reasoning level per agent + exact config values |
| `modalities.input` | input modalities (text/image/video/audio/pdf) | Core filter: vision (+ pdf for doc-heavy agents) |
| `tool_call` | tool/MCP support | Core filter: agents with tools |
| `structured_output` | JSON mode support | Agents returning structured results (verify report, tasks, skill outputs) |
| `knowledge` | knowledge cutoff | Freshness-sensitive agents (current frameworks/libraries) |
| `status` | `deprecated` flag | **Authoritative drift signal**: flag/remove deprecated models |
| `description` | one-line purpose | Quick role matching |

### Fields → subscription (provider-controlled)

| Field | What it tells you | Value for SDD assignment |
|---|---|---|
| `id` | provider-specific model ID | Model selection in config |
| `limit.context` | max context window | Core filter: minimum context per agent |
| `limit.input` | max usable input tokens (may be lower than context − output) | Real constraint for input-heavy agents |
| `cost.*` | raw provider prices | Tiered-pricing modeling only — NOT the plan price |
| `cost.tiers` / `cost.context_over_200k` | price above a context tier | Long-context agents (>200K) cost more |
| `temperature` | temperature support (bool) | Determinism for audit/reproducible agents; `false` = locked |
| `interleaved` | `reasoning_content` field | Output parsing + reasoning-token accounting |
| `open_weights` | open-weight flag | Licensing/privacy context |
| `release_date` / `last_updated` | freshness | "New model" + staleness detection |

Not useful: `provider` (unpopulated in the opencode providers), `experimental` (absent), `attachment` (use `modalities.input` for vision).

⚠️ **Do NOT use `cost.*` from models.dev as the plan price.** The Go plan subsidizes models into `$15` vs `$60` usage tiers and rate-limits them (`req/5h`), so models.dev prices diverge from the plan. Plan prices, subsidy tiers, limits, privacy, and `req/5h` come ONLY from the docs and the TUI `/models`.

## Procedure

### 1. Fetch subscription IDs

Fetch the model list from the subscription's API endpoint to get the exact model IDs.

### 2. Sync models.md

For each model in the subscription:

1. Match by name against `subscriptions/models.md`.
2. **If missing**: extract inherent characteristics from models.dev and add a row to `models.md`.
3. **If present**: check for drift in inherent fields (reasoning, vision, tools, structured output, output cap, knowledge, status). Update `models.md` if changed.
4. **If the provider offers different capabilities** than models.md (e.g. different reasoning options, different output cap): note the override in the subscription file's "Provider overrides" section.

### 3. Build subscription table

Extract provider-controlled fields (pricing, context, input cap, usage, req/5h) from docs/TUI. Write `subscriptions/<name>/<YYYY-MM>.md` with:

- Header: name, plan/price, limits, privacy, source + fetch date, pointer to `../models.md`.
- Model table: `Model | ID | Input/1M | Output/1M | Cached read | Cached write | Context | Input cap | Usage | req/5h | Notes`.
- Provider overrides section (if any model has different capabilities than models.md).
- "Changes vs previous month" section.

## Diff

Compare against the previous month's file to detect:

**In models.md** (inherent drift):
- New models (new ID / `release_date` in models.dev).
- Removed models (missing ID or `status: deprecated` in models.dev).
- Characteristics drift (re-check: context, output cap, reasoning, vision, tools, structured output, knowledge).

**In subscription file** (provider drift):
- New/removed models in the subscription.
- Repriced models (docs / TUI).
- Context / usage changes.
- New or changed provider overrides vs models.md.

Return the resulting drift report.
