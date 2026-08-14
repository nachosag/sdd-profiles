# fetch-suscripcion

Procedimiento para obtener los datos actuales de UNA suscripción.

## Fuentes de verdad por suscripción

| Suscripción | Fuente de datos |
|---|---|
| `opencode-go` | Docs `https://opencode.ai/docs/go/` (precios, límites, estimates, privacidad) + endpoint `https://opencode.ai/zen/go/v1/models` (lista/IDs) + validar IDs con `/models` del TUI cuando esté disponible. |
| `opencode-zen-free` | Docs `https://opencode.ai/docs/zen/` + endpoint `https://opencode.ai/zen/v1/models`. |
| `openai`, `claude`, `gemini`, `mistral`, otros | Fuente por definir por provider (página de pricing + lista oficial de modelos). Marcar como pendiente. |

## Campos a extraer por modelo

- nombre, ID exacto, familia/proveedor
- input/1M, output/1M, cached read, cached write
- contexto máx
- razonamiento (`none` / `built-in` / `configurable`)
- visión (sí/no), tool-calling (sí/no)
- usage/subsidio ($15 vs $60 en Go), req/5h
- notas (nuevo / eliminado / deprecado, precios escalonados)

## Escribir

Crear `subscriptions/<nombre>/<YYYY-MM>.md` con:
- Header: nombre, plan/precio, límites, privacidad, fuente + fecha del fetch.
- Tabla de modelos.
- Sección "Cambios vs mes anterior".

## Diff

Comparar contra el archivo del mes anterior para detectar:
- Modelos nuevos.
- Modelos eliminados.
- Modelos repreciados.
- Cambios de contexto / usage.

Devolver el reporte de drift resultante.
