# Contributing

Thanks for wanting to help. This repository is small and deliberately structured, so there are a few clear ways to contribute.

## Conventions (read first)

- **English** for all committed content (code, tables, docs, comments). Spanish appears only as intentional bilingual *trigger words* quoted in the skill files.
- **Markdown tables** everywhere a table aids scanning — the whole repo is tables-first.
- **`YYYY-MM` versioning** for anything that changes over time (`subscriptions/`, `profiles/`). Never overwrite a month; add a new month file.
- **`.example` / `.md` + gitignore pattern** for personal files. Commit only the `*.example.md` templates; the personal `rules.md` and `considerations.md` are gitignored. Never commit someone's personal configuration.

## Adding a new subscription

Create `subscriptions/<name>/<YYYY-MM>.md` with the header (plan, limits, privacy, source + date), the model table, and a "Changes vs previous month" section. Use the exact column set documented in [File reference](./file-reference.md).

The source of truth depends on the provider — see `skills/_shared/fetch-subscription.md`:

| Subscription | Data source |
|---|---|
| `opencode-go` | `https://opencode.ai/docs/go/` (prices, limits, privacy) + `https://opencode.ai/zen/go/v1/models` (model list/IDs); validate IDs with the TUI's `/models` when available. |
| `opencode-zen-free` | `https://opencode.ai/docs/zen/` + `https://opencode.ai/zen/v1/models`. |
| `openai`, `claude`, `gemini`, `mistral`, others | Not yet defined — use the provider's pricing page and official model list. Mark the source as pending until a canonical one is established. |

Do **not** invent prices or model IDs. If a provider has no defined source, mark it as pending and say so.

## Adding a new profile

Create `profiles/<name>/<YYYY-MM>.md` by applying the generation recipe in `skills/_shared/generate-profile.md` (or just run the `create-sdd-profile` skill and commit the result). The profile must:

- Filter models by cost, context, reasoning, vision, and tools, per `subagents.md`.
- Respect the family restrictions (P2 verify ≠ apply, P3 judge A ≠ judge B, P7 refuter ≠ lens).
- Include the header, the 20-row assignment table, and the cost summary.

## Improving `subagents.md`

`subagents.md` documents the 20 agents and their needs. Changes here are high-impact — every profile is derived from it — so:

- Keep the 13-column schema; don't add columns casually.
- When gentle-ai adds or removes a subagent, update the table and the "How to read this table" scales.
- Update the affected `profiles/` for the current month so the catalog and the derived output stay consistent.

## Improving `rules.example.md`

The universal rules (P1–P7, the decision matrix, the golden rule) are shared knowledge. When you refine them:

- Explain the *reason* for each principle (the "Reason" column) — the rationale is what makes the rules reviewable.
- Keep personal-rule *examples* clearly marked as examples, not as rules that apply to everyone.

## Pull request process

1. **Fork** the repository.
2. **Create a branch** for your change.
3. Make the change, following the conventions above.
4. Open a **pull request** with a description that links the change to its motivation (e.g. "openai catalog for 2026-09", "added sdd-* agent to subagents.md and regenerated balanced/efficient").
5. Make sure the derived layers are consistent: if you change `subagents.md` or a subscription file, regenerate or update the matching `profiles/` in the same PR so the repository never sits in a half-synced state.
