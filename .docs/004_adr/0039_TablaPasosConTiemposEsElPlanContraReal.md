# ADR 0039: La tabla Pasos con tiempos es el plan contra real, viva desde las tareas
- **Estado:** Aceptado
- **Fecha:** 2026-11-22
- **Origen:** `.docs/002_specs/0022_PlanContraRealEnVivo/` (supuesto S1 de `01_spec.md`; decisión 1 de
  `03_plan.md` › 6; decisión 12 de `05_avance.md`, donde la persona aprobó el ADR)

## Contexto
El estimado de cada fase vive en el plan y lo real, fila por fila, en la tabla Tiempos de
`05_avance.md`. La tabla «Plan contra real» solo se escribía en el cierre, así que a media
implementación había que abrir dos archivos y sumar. La persona pidió ver en el avance todos los puntos
del plan con sus tiempos, conforme salen. La leyenda de estatus exige una sola tabla de plan contra real
en el cierre (criterio 1) y cuenta como paso cada fila de implementación de la tabla Pasos. Cada llamada
extra de la sesión relee todo el contexto (`MEMORY.md`, 2026-11-22).

## Decisión
La tabla Pasos de `05_avance.md` lleva Estimado, Real y Delta, con filas de fase, de paso, «Total
implementación» y «Documentación SDD». Se siembra al aprobar las tareas y se actualiza en la misma
escritura que cada fila de Tiempos. Una fila «Tiempo» del PUNTO DE RETOMO lleva los totales. Esa tabla es
la tabla de plan contra real del criterio 1: el cierre la recalcula desde Tiempos y la completa, y la
sección «Plan contra real» del avance queda para la línea de ajuste. Las filas de fase, Total y
Documentación SDD no cuentan como pasos. Fuente de las reglas:
`template/.agents/skills/sdd/avance-y-tiempos.md` › «Pasos con tiempos».

## Alternativas descartadas
| Alternativa | Por qué no |
|---|---|
| Dos tablas: Pasos viva y otra por fase en el cierre | Dos cuentas de los mismos números que pueden descuadrar; contradice «una regla, una fuente» |
| Tabla aparte por fase, sembrada en tareas | La persona pidió ver todos los puntos del plan, no solo las fases |
| Estimado también por paso | Requisito nuevo para el planner; en lote el real sigue siendo del lote, no del paso |
| Solo una fila de totales en el PUNTO DE RETOMO | No muestra qué fase se desvía |
| Recalcular al consultar (`/sdd-status`) | El avance no se leería solo; cada consulta volvería a leer plan y Tiempos |

## Consecuencias
- Se gana: el avance se lee solo y a cualquier hora; estado y cierre usan los mismos números.
- Se paga: cada fila de Tiempos edita también la tabla Pasos y la fila «Tiempo» (en la misma escritura,
  sin llamada extra). La leyenda (criterio 1 y «Un paso que cuenta») cambia de texto, en `template/` y en
  la raíz.
- Queda condicionado: si la tabla y Tiempos discrepan, manda Tiempos y el cierre recalcula. Las specs ya
  cerradas conservan su tabla vieja; una en curso gana la viva en su siguiente paso. El ajuste de cómo
  se estima (estimados de 3 a 5 veces lo real) queda fuera y sigue en `MEMORY.md`.
