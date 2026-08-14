# Rules vs considerations

The generator combines two kinds of personal input that are easy to confuse. Keeping them separate makes the whole system predictable: change a rule when you want to change *how* decisions are made, change a consideration when *your situation* changes.

## The distinction

| | Rule | Consideration |
|---|---|---|
| **What it is** | A directive — an instruction about *how to choose*. | A fact — a parameter about *what your situation is*. |
| **Answers** | "What should the generator prefer or avoid?" | "What do I have, and what are my limits?" |
| **Grammar** | Imperative / verb-first ("Prefer…", "Never…", "Avoid…", "Use…"). | State declaration ("I have…", "My budget is…", "My subscriptions are…"). |
| **Scope** | Applies to *any* assignment, independent of your subscriptions. | Applies to *your* specific context. |
| **Changes when** | You change your mind about policy. | Your subscriptions, budget, or privacy constraints change. |

**Mnemonic:** verb/imperative → **rule**; state declaration → **consideration**.

## Examples from the repository

| Input | Example | Kind | Why |
|---|---|---|---|
| Rule | "Prefer DeepSeek V4 Pro for `sdd-apply` because it fits my budget cap." | Rule | It's an imperative preference ("Prefer…") about *how* to assign. |
| Rule | "Never assign free models that train on my data to client-facing code." | Rule | It's a directive ("Never…") enforcing a policy. |
| Rule | "Use Qwen3.7 Max for `sdd-propose` when the change touches auth/payments." | Rule | Imperative ("Use…") with a condition. |
| Consideration | "subscriptions: `[opencode-go, opencode-zen-free]`" | Consideration | Declares what you *have*. |
| Consideration | "budget: $60/month Go cap" | Consideration | Declares a *limit*. |
| Consideration | "expensive threshold: > $1.00 per 1M input tokens" | Consideration | Declares a *parameter* the rules reference. |
| Consideration | "privacy: free models may train on submitted data — never use them for sensitive/client code" | Consideration | Declares a *constraint on your situation*. |

Notice the subtlety in the last consideration: it says "free models may train on your data" (a fact about your context), whereas the matching *rule* turns it into an imperative ("Never assign free models that train on my data to client-facing code"). The consideration states the situation; the rule states what to do about it.

## The `.example` vs `.md` pattern

Two files, two roles:

| File | Committed? | Purpose |
|---|---|---|
| `rules.example.md` | Yes | Universal rules (P1–P7, the decision matrix, the golden rule) plus *examples* of personal rules. Shared with everyone. |
| `rules.md` | No (gitignored) | Your actual personal rules. You copy the universal part and add or override rules for yourself. |
| `considerations.example.md` | Yes | A filled-in example showing the shape of the file. |
| `considerations.md` | No (gitignored) | Your real subscriptions, budget, privacy, and preferences. |

The `.example.md` files are committed because they document *structure* and *shared knowledge* — everyone benefits from seeing the P1–P7 principles and the fields a consideration file should contain. The `.md` files are gitignored because they hold *your* private context: which plans you pay for, how much you can spend, and what privacy tradeoffs you accept. Nobody else should inherit those when they clone the repo.

## Summary

- Write **rules** when you want to change *how the generator decides*.
- Write **considerations** when *your facts* (subscriptions, budget, privacy) change.
- If it starts with a verb and tells the generator what to do, it's a rule. If it states what you have or what your limits are, it's a consideration.
