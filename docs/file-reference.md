# File reference

A reference of every file and directory in the repository, what it's for, and — where relevant — its format.

## Top-level files

### `subagents.md`

The canonical catalog of the 19 SDD subagents and their model needs. It contains a single table with 13 columns:

| Column | Meaning |
|---|---|
| `Agent` | The subagent's name (e.g. `sdd-apply`, `jd-judge-b`). |
| `Category` | Its role family (coordination, ingestion, code analysis, pure reasoning, technical writing, design/architecture, formatting, coding agentic, audit, compression, extreme reasoning, light audit, directed coding). |
| `Purpose` | One-line description of what it does. |
| `Reasoning` | How much reasoning capacity it needs (`none` → `low` → `medium` → `high` → `max`). |
| `Context` | Minimum context window (`128K`, `200K`, `256K`, `1M`, or `high`/`200K+`). |
| `Code-focused` | Whether it works focused on code (`yes`/`no`/`medium`). |
| `Vision` | Whether it needs visual/multimodal comprehension (`yes`/`no`). |
| `Tools` | Whether it needs tool/MCP access (`yes`/`no`/`optional`). |
| `Frequency` | How often it runs (`1x` → `very high`; `almost never` for emergency roles). |
| `Duration` | How long it runs (`very short` → `very long`). |
| `Loop` | Whether it runs a sustained work loop (`yes`/`no`). |
| `Error impact` | The cost of a mistake (`low` → `critical`). |
| `Different family from` | Lab-independence restriction (e.g. `verify` must differ from `apply`). |

A "How to read this table" section below the table explains the scales.

### `rules.example.md`

The universal assignment rules, shared and committed. Contains:

- **P1–P7**: seven non-negotiable principles (expensive models only in high-impact non-repetitive phases, verify ≠ apply family, judge A ≠ judge B, cheapest model for formatting/admin phases, etc.).
- **Decision matrix**: a per-agent table of justifiable cost, context, reasoning, frequency×duration, and the "different model?" restriction.
- **Golden rule**: a decision flowchart for whether an expensive model is justified.
- **Personal rules (examples)**: sample personal rules to copy into your own `rules.md`.

### `rules.md`

Your personal rules. Gitignored. Starts from a copy of `rules.example.md`; your additions *extend or override* the universal rules.

### `considerations.example.md`

A filled-in example of the considerations file, showing the shape. Fields: `subscriptions`, `budget`, `expensive threshold`, `privacy`, `preferences`.

### `considerations.md`

Your real context. Gitignored. Structured sections: `subscriptions`, `budget`, `expensive_threshold`, `privacy`, `preferences`.

### `AGENTS.md`

Agent-facing registration file. Lists the four skills in a table (name → path → description) and maps user intent to the right skill (the "intent routing" section). Human users don't need to read it — their coding agent does.

### `.gitignore`

Excludes the personal files so they never get committed:

```
rules.md
considerations.md
.atl/
```

## Directories

### `subscriptions/<name>/<YYYY-MM>.md`

One file per subscription per month. For example `subscriptions/opencode-go/2026-08.md`.

Format:

- **Header**: plan and price, usage limits, privacy, source URL + fetch date.
- **Model table**, one row per model, with columns: `Model`, `ID`, `Family/provider`, `Input/1M`, `Output/1M`, `Cached read`, `Cached write`, `Context`, `Reasoning`, `Vision`, `Tools`, `Usage/subsidy`, `req/5h`, `Notes`.
- **Changes vs previous month**: a bullet list of removed models, added models, and corrected estimates.

### `profiles/<name>/<YYYY-MM>.md`

One file per profile per month, e.g. `profiles/balanced/2026-08.md` and `profiles/efficient/2026-08.md`.

Format:

- **Header**: purpose, the subscriptions it uses, the month, and what it was derived from.
- **Model assignment table**, one row per agent, with columns: `Agent`, `Primary model`, `Fallback(s)`, `Thinking level`, `Justification`.
- **Summary**: average primary cost, premium models used, Usage $15 models used, free models used, and how many expensive models ended up in repetitive phases.

### `skills/`

The four invokable skills plus a shared support directory.

| Path | Purpose |
|---|---|
| `skills/start/SKILL.md` | First-time setup. |
| `skills/update-sdd-profiles/SKILL.md` | Full flow: sync + regenerate. |
| `skills/sync-sdd-profiles/SKILL.md` | Data-only refresh + drift report. |
| `skills/create-sdd-profile/SKILL.md` | Create one new profile. |
| `skills/_shared/fetch-subscription.md` | Recipe: fetch one subscription's catalog, including the source-of-truth table per provider, the fields to extract, and the diff procedure. |
| `skills/_shared/generate-profile.md` | Recipe: generate one profile (inputs, algorithm, family restrictions, final checklist, write format). |
| `skills/_shared/SKILL.md` | Marks `_shared` as support-only (not invokable). |

### `docs/`

The human-facing documentation you're reading (this file and its siblings).

## The `YYYY-MM` versioning convention

Both `subscriptions/` and `profiles/` are versioned by month. Each month gets its own file — `2026-08.md`, then `2026-09.md`, and so on — rather than overwriting a single "current" file. This makes it trivial to diff one month against the previous one, which is exactly what the `Changes vs previous month` section and the drift report rely on. To see how a subscription or profile evolved, compare two adjacent month files.
