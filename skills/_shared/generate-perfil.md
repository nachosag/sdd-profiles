# generate-perfil

Procedimiento para generar UN perfil de asignación de modelos.

## Entradas

- `subagents.md` — necesidades de los agentes.
- `rules.md` — reglas (si no existe → `rules.example.md`).
- `considerations.md` — parámetros: suscripciones, presupuesto, umbral de "caro".
- `subscriptions/<x>/<YYYY-MM>.md` — datos de modelos.

## Algoritmo

Para cada uno de los 19 agentes, en orden:
1. Filtrar modelos por rango de costo (según frecuencia × impacto).
2. Filtrar por contexto mínimo, nivel de razonamiento, code-focus, visión, tool-calling.
3. Aplicar restricciones de familia:
   - P2: verify ≠ apply.
   - P3: judge A ≠ judge B.
   - P7: refuter ≠ lente.
4. Seleccionar primario + 1-2 fallbacks + thinking level + justificación.

## Checklist final

Verificar P1–P7 + regla de oro antes de escribir.

## Escribir

Crear `profiles/<nombre>/<YYYY-MM>.md` con:
- Header: nombre, propósito, `subscriptions: [...]`, mes.
- Tabla de asignación de los 19 agentes (primario / fallback / thinking / justificación).
- Resumen de costos: promedio, modelos premium, modelos Usage $15, uso de free.
