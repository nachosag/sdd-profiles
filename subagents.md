# Subagentes SDD de Gentle-AI

Catálogo canónico de los 19 subagentes del flujo SDD con sus necesidades de modelo.

| Subagente | Categoría | Propósito | Razonamiento | Contexto | Code-focused | Visión | Tools | Frecuencia | Duración | Loop | Impacto de error | Familia distinta a |
|---|---|---|---|---|---|---|---|---|---|---|---|---|
| gentle-orchestrator | coordinación | Coordina el flujo SDD: routing, delegación y validación entre fases. | low | 1M | no | no | sí | muy alta | muy larga | no | alto | — |
| sdd-init | ingestión | Detecta stack, scripts, convenciones y estado mínimo del proyecto. | none | 1M | no | no | sí | 1 vez | corta | no | bajo | — |
| sdd-explore | análisis de código | Investiga flujos, dependencias, archivos clave e impacto. | medium | 1M | sí | no | sí | media | media | no | medio | — |
| sdd-propose | razonamiento puro | Define el QUÉ y POR QUÉ del cambio; decisión de mayor impacto. | high | 1M | no | no | opcional | baja | corta-media | no | crítico | — |
| sdd-spec | escritura técnica | Formaliza requisitos, contratos y criterios de aceptación. | low | alto | sí | no | no | baja | corta | no | medio | — |
| sdd-design | diseño/arquitectura | Define arquitectura técnica, cambios por capas, UI y pruebas. | medium | 1M | medio | sí | no | baja | media | no | medio-alto | — |
| sdd-tasks | formateo | Divide el diseño en tareas atómicas, ordenadas y verificables. | none | alto | no | no | no | alta | corta | no | bajo | — |
| sdd-apply | coding agentic | Escribe el código e implementa tareas en batches. | high | 1M | sí | no | sí | muy alta | muy larga | sí | crítico | ≠ verify |
| sdd-verify | auditoría | Audita diff, pruebas, regresiones, seguridad y cumplimiento de spec. | high | 1M | sí | no | sí | alta | media | no | alto | ≠ apply |
| sdd-archive | compresión | Resume artefactos, deja bitácora y notas de commit. | none | 1M | no | no | no | 1 vez | cortísima | no | bajo | — |
| sdd-onboard | ingestión | Absorbe contexto base, estructura, memoria y reglas del proyecto. | none | 1M | no | sí | sí | 1 vez | media | no | bajo | — |
| review-risk | razonamiento extremo | Revisa seguridad: vulnerabilidades, permisos, exposición de datos. | max | 200K+ | sí | no | no | media | corta | no | crítico | idealmente ≠ apply |
| review-readability | auditoría ligera | Revisa naming, complejidad, intención y mantenibilidad. | medium | 200K+ | sí | no | no | media | corta | no | medio | — |
| review-reliability | auditoría | Revisa tests, determinismo, edge cases y regresiones. | medium | 1M | sí | no | no | media | corta | no | medio-alto | — |
| review-resilience | auditoría | Revisa errores, retry/backoff, degradación y observabilidad. | medium | 1M | sí | no | no | media | corta | no | medio | — |
| review-refuter | razonamiento extremo | Evalúa adversariamente los hallazgos BLOCKER/CRITICAL y da un veredicto por hallazgo. | max | 200K+ | sí | no | no | baja | corta | no | crítico | ≠ lente que generó el hallazgo |
| jd-judge-a | razonamiento extremo | Primer juez ciego del Judgment Day (sin bash). | max | 200K+ | sí | no | no | casi nunca | corta | no | crítico | ≠ judge B y ≠ implementador |
| jd-judge-b | razonamiento extremo | Segundo juez ciego, de laboratorio distinto al Juez A. | max | 200K+ | sí | no | no | casi nunca | corta | no | crítico | ≠ judge A y ≠ implementador |
| jd-fix-agent | coding dirigido | Aplica las correcciones quirúrgicas de los veredictos de los jueces. | low-medium | 200K+ | sí | no | sí | casi nunca | corta | no | medio | — |

## Cómo leer esta tabla

- **Razonamiento**: cuánta capacidad de razonamiento necesita el agente. Escala: `none` (nulo) → `low` (bajo) → `medium` (medio) → `high` (alto) → `max` (extremo).
- **Contexto**: ventana de contexto mínima necesaria. `128K`, `200K`, `256K`, `1M` tokens, o `alto`/`200K+` cuando no hay un número exacto fijo.
- **Code-focused**: si el agente trabaja enfocado en código (`sí`/`no`/`medio`).
- **Visión**: si necesita comprensión visual/multimodal (`sí`/`no`).
- **Tools**: si necesita acceso a tools/MCP (`sí`/`no`/`opcional`).
- **Frecuencia**: `1 vez` → `baja` → `media` → `alta` → `muy alta`; `casi nunca` para roles de emergencia.
- **Duración**: `cortísima` → `corta` → `corta-media` → `media` → `muy larga`.
- **Loop**: si el agente ejecuta un bucle de trabajo sostenido (`sí`/`no`).
- **Impacto de error**: `bajo` → `medio` → `medio-alto` → `alto` → `crítico`.
- **Familia distinta a**: restricción de independencia de laboratorio que debe cumplir la asignación de modelo del agente.
