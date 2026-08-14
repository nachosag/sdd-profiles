# Universal model assignment rules

Shared rules for assigning models to the 19 SDD subagents. They are independent of which subscriptions the user has.

## Non-negotiable principles (P1–P7)

| # | Principle | Reason |
|---|---|---|
| **P1** | **Expensive models (>$1.00/1M input) ONLY in NON-repetitive and HIGH-impact phases.** | `cost × volume` in long/repetitive phases is unjustified. The marginal benefit does not pay off. |
| **P2** | **Verify must use a model from a DIFFERENT FAMILY than apply.** | Author ≠ auditor principle. If the same model reviews what it wrote, the biases are perpetuated. |
| **P3** | **Judgment Day requires two DIFFERENT models for judge A and B.** | Blind adversarial review with independent perspectives. If both agree, the finding is solid. |
| **P4** | **Formatting/admin phases (tasks, init, archive, onboard) use the CHEAPEST available model.** | They do not require reasoning. Paying more does not improve the output. In Efficient, prefer Zen Free (zero cost). |
| **P5** | **The orchestrator needs 1M ctx + stability, NOT premium reasoning.** | Its job is reliable routing across long sessions, not solving complex problems. |
| **P6** | **Model cost must be proportional to `(error impact) ÷ (execution frequency)`.** | Very high-impact but low-frequency phases (propose, security) justify a premium price. High-impact AND high-frequency phases (apply) need the best model you can afford at volume. |
| **P7** | **`review-refuter` must be from a family different from the review lens that produced the finding it is evaluating.** | A refuter from the same lab tends to confirm the biases of the finding it is evaluating. |

## Decision matrix

For each agent, evaluate against 5 dimensions. The type of model it needs emerges from this matrix:

| Agent | Justifiable cost | Context | Reasoning | Frequency×Duration | Different model? |
|---|---|---|---|---|---|
| orchestrator | Low ($0.15–0.50) | 1M | Low | Very high × Very long | No |
| init | Minimum ($0.05–0.15) | 1M | Null | 1x × Short | No |
| explore | Medium ($0.15–1.00) | 1M | Medium | Medium × Medium | No |
| propose | **High ($1.50–3.00)** | 1M | **High** | Low × Short | No |
| spec | Medium ($0.40–1.00) | 272K+ | Low | Low × Short | No |
| design | Low ($0.15–0.50) | 1M | Medium | Low × Medium | No |
| tasks | Minimum ($0.05–0.40) | 1M | Null | High × Short | No |
| apply | **Medium-low ($0.15–1.00)** | **1M** | **High** | **Very high × Very long** | No |
| verify | **Medium-low ($0.15–1.00)** | 1M | High | **High × Medium** | **✅ Different from apply** |
| archive | **Zero if possible** | 1M | Null | 1x × Very short | No |
| onboard | Minimum ($0.05–0.30) | 1M | Null | 1x × Medium | No |
| review-risk | **High ($1.50–3.00)** | 200K+ | **Extreme** | Medium × Short | Ideally different from apply |
| review-readability | Medium ($0.40–1.00) | 200K+ | Medium | Medium × Short | No |
| review-reliability | Medium ($0.40–1.00) | 1M | Medium | Medium × Short | No |
| review-resilience | Medium ($0.40–1.00) | 1M | Medium | Medium × Short | No |
| review-refuter | **High ($0.40–3.00)** | 200K+ | **Extreme** | Almost never × Short | **✅ Different from the review lens** |
| jd-judge-a | **High ($1.50–3.00)** | 200K+ | **Extreme** | Almost never × Short | **✅ Different from judge B** |
| jd-judge-b | **High ($1.50–3.00)** | 200K+ | **Extreme** | Almost never × Short | **✅ Different from judge A** |
| jd-fix-agent | Medium ($0.40–1.00) | 200K+ | Low | Almost never × Short | No |

## Golden rule

```text
Is the model expensive (>$1.00/1M input)?

  YES → Is the phase NON-repetitive AND CRITICAL impact?
         YES → ✅ Justified.
         NO  → ❌ Not justified. Look for a model <$1.00.

  NO (<$1.00) → Is the phase repetitive/long?
         YES → Prioritize models <$0.50 with 1M ctx and Usage $60.
         NO  → Any model <$1.00 can be used according to reasoning need.
```

> Shared universal rules. Your personal rules go in `rules.md` (gitignored), which extend or override these.

## Personal rules (examples)

These are examples of personal rules you can add to your own `rules.md` (gitignored).
Copy `rules.example.md` to `rules.md` and replace these examples with your own:

- Prefer DeepSeek V4 Pro for `sdd-apply` because it fits my budget cap.
- Never assign free models that train on my data to client-facing code.
- Use Qwen3.7 Max for `sdd-propose` when the change touches auth/payments.
- Avoid Usage $15 models in long-running phases (apply, orchestrator) to preserve the monthly cap.
- For `sdd-verify`, prefer a model from a different family than apply, even if it costs slightly more.
