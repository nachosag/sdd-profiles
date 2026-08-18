# Model Catalog

Canonical model characteristics — capabilities inherent to the model, independent of which provider offers it.

- **Source**: `https://models.dev/api.json` · 18 Aug 2026.
- **Used by**: `subscriptions/<name>/<YYYY-MM>.md` (pricing, context caps, rate limits) + `generate-profile.md` (merge logic).

## Models (18)

| Model | Provider | Family | Output cap | Reasoning | Vision | Tools | Structured | Knowledge |
|---|---|---|---|---|---|---|---|---|
| Big Pickle | unknown (stealth) | big-pickle | 32K | built-in | no | yes | yes | 2025-01 |
| DeepSeek V4 Flash | DeepSeek | deepseek-flash | 384K | toggle + effort [low, high, max] | no | yes | yes | 2025-05 |
| DeepSeek V4 Pro | DeepSeek | deepseek-thinking | 384K | effort [high, max] | no | yes | yes | 2025-05 |
| GLM-5.1 | Zhipu | glm | 32K | built-in | no | yes | no | 2025-04 |
| GLM-5.2 | Zhipu | glm | 128K | effort [high, max] | no | yes | yes | — |
| GPT 5.6 Luna | OpenAI | gpt-luna | 128K | effort [none, low, medium, high, xhigh, max] | yes | yes | yes | 2026-02 |
| Grok 4.5 | xAI | grok | 500K | effort [low, medium, high] | yes | yes | yes | — |
| Hy3 | Hy3 (emerging) | Hy | 64K | effort [none, low, high] | no | yes | no | — |
| Kimi K2.6 | Moonshot | kimi-k2 | 64K | built-in | yes | yes | no | 2024-10 |
| Kimi K2.7 Code | Moonshot | kimi-k2 | 256K | built-in | yes | yes | yes | 2025-01 |
| Kimi K3 | Moonshot | kimi-k3 | 128K | effort [max] | yes | yes | yes | — |
| Laguna S 2.1 | unknown | laguna | 32K | effort [low, medium, high] | no | yes | no | — |
| MiMo V2.5 | Xiaomi | mimo-v2.5 | 128K | built-in | yes | yes | no | 2024-12 |
| MiMo V2.5 Pro | Xiaomi | mimo-v2.5-pro | 128K | built-in | no | yes | no | 2024-12 |
| MiniMax M2.7 | MiniMax | minimax-m2.7 | 128K | built-in | no | yes | no | 2025-01 |
| MiniMax M3 | MiniMax | minimax-m3 | 128K | toggle | yes | yes | no | 2025-01 |
| Nemotron 3 Ultra | NVIDIA | nemotron-free | 128K | built-in | no | yes | no | 2026-02 |
| Nemotron 3.5 Lightning | NVIDIA | nemotron-free | 256K | built-in | no | yes | yes | — |
| Qwen3.6 Plus | Qwen | qwen3.6 | 64K | toggle + budget ≤80K | yes | yes | no | 2025-04 |
| Qwen3.7 Max | Qwen | qwen3.7-max | 64K | toggle + budget ≤256K | no | yes | no | — |
| Qwen3.7 Plus | Qwen | qwen3.7-plus | 64K | toggle + budget ≤256K | yes | yes | no | — |
| Qwen3.8 Max | Qwen | qwen3.8-max | 128K | toggle + budget ≤256K | yes | yes | yes | — |

## Provider overrides

When a provider offers different capabilities than this catalog, the subscription file overrides the specific field. Known overrides:

| Model | Provider | Field | Canonical | Override |
|---|---|---|---|---|
| DeepSeek V4 Flash | opencode-go | Reasoning | toggle + effort [low, high, max] | effort [low, high, max] |
| DeepSeek V4 Flash | opencode-zen-free | Output cap | 384K | 128K |
| MiMo V2.5 | opencode-zen-free | Output cap | 128K | 32K |
| Hy3 | opencode-zen-free | Reasoning | effort [none, low, high] | toggle + effort [low, medium, high] |
| Hy3 | opencode-zen-free | Structured | no | yes |
