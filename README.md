# SDD Profiles

Repositorio que documenta qué modelos de IA asignar a cada subagente de Gentle-AI (los 19 agentes del flujo SDD), por perfil (Balanced, Efficient), y que se mantiene sincronizado con los modelos disponibles de cada suscripción.

## Estructura en 3 capas

| Capa | Archivos | Qué contiene |
|---|---|---|
| Conocimiento estable | `subagents.md`, `rules.example.md` | Catálogo de los 19 subagentes y reglas universales de asignación (P1–P7, matriz de decisión, regla de oro). |
| Datos versionados | `subscriptions/`, `profiles/` | Modelos y precios de cada suscripción, y asignaciones por perfil, organizados por mes. |
| Config personal | `rules.md`, `considerations.md` | Reglas y consideraciones propias del usuario, gitignored (plantillas en `*.example.md`). |

## Quick start

1. Clonar el repositorio.
2. Crear `rules.md` y `considerations.md` a partir de las plantillas `*.example.md` y completarlas con tu contexto.
3. Indicarle a tu agente qué perfiles (`balanced`, `efficient`) y suscripciones (`opencode-go`, `opencode-zen-free`) querés usar.
4. Cuando cambien los modelos, pedirle "update my profiles".

## Skills

Los skills (agentes que automatizan la creación, actualización y verificación de perfiles) se crean en una fase posterior, bajo `skills/`.
