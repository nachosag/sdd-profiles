---
name: update-sdd-profiles
description: "Trigger: actualizar mis perfiles SDD, sincronizar mis perfiles, update/refresh my profiles. Runs data sync then regenerates all the user's profiles."
license: MIT
metadata:
  author: "nachosag"
  version: "1.0"
---

## Activation Contract

ENTRADA PRINCIPAL. Se activa cuando el usuario dice "actualizar/sincronizar/refrescar mis perfiles" (indistintamente). No confundir con `sync-sdd-profiles` (solo datos).

## Hard Rules

- Si `rules.md` o `considerations.md` no existen o están vacíos → bootstrap primero (preguntar preferencias, suscripciones y reglas; guardarlas).
- No regenerar un perfil sin datos frescos (sync primero).
- Mantener la invariante de familia (verify ≠ apply, judge A ≠ B).

## Decision Gates

- ¿Configuración presente? Si no → bootstrap.
- ¿Datos frescos? Si no → sync antes de regenerar.

## Execution Steps

1. Bootstrap si falta config.
2. Ejecutar sync (fetch de suscripciones + detección de subagentes).
3. Para cada perfil existente en `profiles/` (o los que el usuario indique), seguir `../_shared/generate-perfil.md` y regenerar `profiles/<nombre>/<YYYY-MM>.md`.

## Output Contract

- Perfiles regenerados (lista).
- Resumen de cambios vs mes anterior.

## References

- `../_shared/fetch-suscripcion.md`
- `../_shared/generate-perfil.md`
- `../../subagents.md`
- `../../rules.example.md`
- `../../considerations.example.md`
