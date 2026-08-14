---
name: create-sdd-profile
description: "Trigger: crear un perfil nuevo, agregar un perfil, create a new profile. Creates ONE new profile from a subscription without touching existing profiles."
license: MIT
metadata:
  author: "nachosag"
  version: "1.0"
---

## Activation Contract

Se usa cuando el usuario quiere un perfil NUEVO (ej. "creá un perfil Free con el plan free de Mistral"). No toca perfiles existentes.

## Hard Rules

- Aislamiento: nunca modificar perfiles existentes.
- Si la suscripción no está en `subscriptions/`, fetchearla primero.
- Registrar la suscripción nueva en `considerations.md`.

## Decision Gates

- ¿Falta el catálogo de la suscripción? → fetch antes de generar.

## Execution Steps

1. Identificar nombre del perfil + suscripción(es).
2. Si falta el catálogo, seguir `../_shared/fetch-suscripcion.md`.
3. Agregar la suscripción a `considerations.md` (lista `suscripciones`).
4. Seguir `../_shared/generate-perfil.md` para ese único perfil.
5. Reportar.

## Output Contract

- El perfil creado (`profiles/<nombre>/<YYYY-MM>.md`).
- Confirmación de que los demás no se tocaron.

## References

- `../_shared/fetch-suscripcion.md`
- `../_shared/generate-perfil.md`
- `../../considerations.md`
