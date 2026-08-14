---
name: sync-sdd-profiles
description: "Trigger: refrescar los datos de suscripciones, sync data, fetch suscripciones, drift. Fetches the user's subscription catalogs and detects new gentle-ai subagents, then reports drift."
license: MIT
metadata:
  author: "nachosag"
  version: "1.0"
---

## Activation Contract

Se usa cuando el usuario quiere SOLO refrescar los datos (catálogos de suscripciones + detección de subagentes), sin regenerar perfiles. No es la entrada principal (ver `update-sdd-profiles`).

## Hard Rules

- No borrar archivos.
- No regenerar perfiles (eso lo hace `update-sdd-profiles`).
- Subagente nuevo = reportar, NO auto-escribir en `subagents.md`.

## Decision Gates

- Si no hay suscripciones en `considerations.md` → preguntar al usuario antes de continuar.
- Si un provider no tiene fuente definida → marcar como pendiente y reportar, no inventar datos.

## Execution Steps

1. Leer `considerations.md` → obtener las suscripciones del usuario.
2. Por cada suscripción, seguir `../_shared/fetch-suscripcion.md` y escribir `subscriptions/<nombre>/<YYYY-MM>.md`.
3. Verificar gentle-ai (repo/docs) para detectar subagentes nuevos o eliminados.
4. Emitir reporte de drift.
5. Proponer regenerar perfiles (si el usuario acepta, invocar `update-sdd-profiles`).

## Output Contract

- Reporte de drift: suscripciones cambiadas, modelos nuevos/eliminados/repreciados, subagentes nuevos.
- Archivos `subscriptions/` escritos.

## References

- `../_shared/fetch-suscripcion.md`
- `../../considerations.md`
- `../../subagents.md`
