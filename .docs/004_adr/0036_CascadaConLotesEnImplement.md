# ADR 0036: `/sdd-implement` sin paso implementa en cascada, con lotes de hasta 4 pasos

- **Estado:** Aceptado
- **Fecha:** 2026-11-22
- **Origen:** `0019_FlujoEficiente`

## Contexto
`/sdd-implement` hacía un paso y paraba. El reporte de sesiones del 2026-11-21 y 2026-11-22 mostró
dos costos:
- En 0015, cada paso esperó un «procede» de la persona.
- En 0013 hubo 38 lanzamientos del implementer para 42 pasos, y cada arranque en frío escribe de
  15 000 a 20 000 tokens de caché.

En 0017, 36 pasos agrupados en 12 lanzamientos pasaron validate sin perder calidad. La cascada solo
existía como preferencia personal de un dev, no en el kit.

## Decisión
`/sdd-implement <feature>` sin paso implementa en cascada todos los pasos pendientes, y con `Fn`,
los de esa fase. La forma con `Fn.m` sigue haciendo un solo paso.

Funcionamiento de la cascada:
- Agrupa hasta 4 pasos en un lote: de la misma fase, o de fases distintas que tocan el mismo archivo.
- Ningún paso del lote depende de uno pendiente fuera del lote, ni pide verificación manual,
  evidencia de datos o `sql/`.
- Los lotes sin archivos en común van en olas paralelas.
- Antes de arrancar muestra la tabla de lotes.
- Revisa el diff de cada lote.
- Se detiene solo en lo ajeno o en un fallo.

## Alternativas descartadas
| Alternativa | Por qué no |
|---|---|
| Comando nuevo `/sdd-cascada` | Un comando más para aprender y para que la estructura valide; la forma de un paso ya existe en `sdd-implement` |
| Un implementer por paso, en cascada | Conserva el costo de un arranque en frío por paso, que es lo que se quiere bajar |
| Lotes sin tope | Un fallo dentro de un lote grande cuesta más de revisar y de rehacer |
| Dejar la cascada como preferencia personal | Cada dev la reinventa; el kit no la valida ni la documenta |

## Consecuencias
- Menos esperas y menos arranques en frío.
- La revisión del diff por lote y la marca paso a paso conservan la verificación.
- El tope de 4 es provisional y se revisa tras 3 usos reales.
- La cascada nunca hace commits ni ejecuta `sql/`: esos puntos la detienen.
- Las filas de tiempos de un lote siguen `avance-y-tiempos.md` › «Mínimo y lote».
