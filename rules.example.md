# Reglas universales de asignación de modelos

Reglas compartidas para asignar modelos a los 19 subagentes SDD. Son independientes de qué suscripciones tenga el usuario.

## Principios no negociables (P1–P7)

| # | Principio | Razón |
|---|---|---|
| **P1** | **Modelos caros (>$1.00/1M input) SOLO en fases NO repetitivas y de ALTO impacto.** | `costo × volumen` en fases largas/repetitivas es injustificado. El beneficio marginal no compensa. |
| **P2** | **Verify debe usar un modelo de FAMILIA DIFERENTE a apply.** | Principio autor ≠ auditor. Si el mismo modelo revisa lo que escribió, los sesgos se perpetúan. |
| **P3** | **Judgment Day requiere dos modelos DIFERENTES entre sí para judge A y B.** | Revisión adversarial ciega con perspectivas independientes. Si ambos coinciden, el hallazgo es sólido. |
| **P4** | **Fases de formateo/admin (tasks, init, archive, onboard) usan el modelo MÁS BARATO disponible.** | No requieren razonamiento. Pagar más no mejora el output. En Efficient, preferir Zen Free (costo cero). |
| **P5** | **El orchestrator necesita 1M ctx + estabilidad, NO razonamiento premium.** | Su trabajo es routing confiable a lo largo de sesiones largas, no resolver problemas complejos. |
| **P6** | **El costo del modelo debe ser proporcional a `(impacto del error) ÷ (frecuencia de ejecución)`.** | Fases de altísimo impacto pero baja frecuencia (propose, seguridad) justifican precio premium. Fases de alto impacto Y alta frecuencia (apply) necesitan el mejor modelo que se pueda pagar en volumen. |
| **P7** | **`review-refuter` debe ser de familia distinta a la lente de review que generó el hallazgo que está evaluando.** | Un refuter del mismo laboratorio tiende a confirmar los sesgos del hallazgo que está evaluando. |

## Matriz de decisión

Para cada agente, evaluar contra 5 dimensiones. El tipo de modelo que necesita surge de esta matriz:

| Agente | Costo justificable | Contexto | Razonamiento | Frecuencia×Duración | ¿Modelo distinto? |
|---|---|---|---|---|---|
| orchestrator | Bajo ($0.15–0.50) | 1M | Bajo | Muy alta × Muy larga | No |
| init | Mínimo ($0.05–0.15) | 1M | Nulo | 1 vez × Corta | No |
| explore | Medio ($0.15–1.00) | 1M | Medio | Media × Media | No |
| propose | **Alto ($1.50–3.00)** | 1M | **Alto** | Baja × Corta | No |
| spec | Medio ($0.40–1.00) | 272K+ | Bajo | Baja × Corta | No |
| design | Bajo ($0.15–0.50) | 1M | Medio | Baja × Media | No |
| tasks | Mínimo ($0.05–0.40) | 1M | Nulo | Alta × Corta | No |
| apply | **Medio-bajo ($0.15–1.00)** | **1M** | **Alto** | **Muy alta × Muy larga** | No |
| verify | **Medio-bajo ($0.15–1.00)** | 1M | Alto | **Alta × Media** | **✅ Diferente a apply** |
| archive | **Cero si es posible** | 1M | Nulo | 1 vez × Cortísima | No |
| onboard | Mínimo ($0.05–0.30) | 1M | Nulo | 1 vez × Media | No |
| review-risk | **Alto ($1.50–3.00)** | 200K+ | **Extremo** | Media × Corta | Idealmente diferente a apply |
| review-readability | Medio ($0.40–1.00) | 200K+ | Medio | Media × Corta | No |
| review-reliability | Medio ($0.40–1.00) | 1M | Medio | Media × Corta | No |
| review-resilience | Medio ($0.40–1.00) | 1M | Medio | Media × Corta | No |
| review-refuter | **Alto ($0.40–3.00)** | 200K+ | **Extremo** | Casi nunca × Corta | **✅ Diferente a la review lens** |
| jd-judge-a | **Alto ($1.50–3.00)** | 200K+ | **Extremo** | Casi nunca × Corta | **✅ Diferente a judge B** |
| jd-judge-b | **Alto ($1.50–3.00)** | 200K+ | **Extremo** | Casi nunca × Corta | **✅ Diferente a judge A** |
| jd-fix-agent | Medio ($0.40–1.00) | 200K+ | Bajo | Casi nunca × Corta | No |

## Regla de oro

```text
¿El modelo es caro (>$1.00/1M input)?

  SÍ → ¿La fase es NO repetitiva Y de impacto CRÍTICO?
         SÍ → ✅ Se justifica.
         NO → ❌ No se justifica. Buscar modelo <$1.00.

  NO (<$1.00) → ¿La fase es repetitiva/larga?
         SÍ → Priorizar modelos <$0.50 con 1M ctx y Usage $60.
         NO  → Se puede usar cualquier modelo <$1.00 según necesidad de razonamiento.
```

> Reglas universales compartidas. Tus reglas personales van en `rules.md` (gitignored), que extienden o sobreescriben estas.
