# AGENTS

Este repositorio mantiene sincronizadas las asignaciones de modelos de los perfiles SDD de gentle-ai.

## Skills disponibles

| Skill | Ruta | Descripción |
|---|---|---|
| `update-sdd-profiles` | `skills/update-sdd-profiles/SKILL.md` | Flujo completo: sync de datos + regenerar perfiles. |
| `sync-sdd-profiles` | `skills/sync-sdd-profiles/SKILL.md` | Solo refrescar datos y detectar drift. |
| `create-sdd-profile` | `skills/create-sdd-profile/SKILL.md` | Crear un perfil nuevo a partir de una suscripción. |

## Routing de intenciones

- "actualizar / sincronizar / refrescar mis perfiles" → `skills/update-sdd-profiles/SKILL.md` (flujo completo: sync + regenerar).
- "solo refrescar los datos / ver drift / fetch suscripciones" → `skills/sync-sdd-profiles/SKILL.md`.
- "crear / agregar un perfil nuevo X con suscripción Y" → `skills/create-sdd-profile/SKILL.md`.

## Nota

Antes de actuar, leer (cargar) el `SKILL.md` correspondiente. `skills/_shared/` contiene recetas de soporte y no es invocable.
