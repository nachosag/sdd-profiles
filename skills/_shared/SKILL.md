---
name: _shared
description: "Shared reference recipes for sdd-profiles skills. Not invokable."
license: MIT
metadata:
  author: "nachosag"
  version: "1.0"
disable-model-invocation: true
user-invocable: false
---

## Purpose

Este directorio contiene las recetas de referencia compartidas por los skills de sdd-profiles:
- `fetch-suscripcion.md` — procedimiento para obtener los datos actuales de una suscripción.
- `generate-perfil.md` — procedimiento para generar un perfil de asignación de modelos.

## Not Invokable

Este paquete es solo de soporte. No se invoca directamente; los skills `sync-sdd-profiles`, `update-sdd-profiles` y `create-sdd-profile` referencian estas recetas.
