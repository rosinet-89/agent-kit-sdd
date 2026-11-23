# 07 — Casos de prueba: Plan contra real en vivo

Destino: `local`. CP-01 a CP-06 son de solo lectura con `grep` o `diff` (`AGENTS.md` › Comandos:
«Herramienta de apoyo para casos: No aplica»). CP-07 es a mano, en el chat. Escribe: no, salvo CP-07
(`05_avance.md` de esta carpeta, que escribe la sesión principal). Ningún caso toca una BD.

## Casos
### CP-01 Fuente con su sección y la fila «Tiempo»
- RF: RF-1 a RF-10, RF-14
- Pasos: `grep -n -E '^#+ Pasos con tiempos' template/.agents/skills/sdd/avance-y-tiempos.md`;
  `grep -n '| Tiempo |' template/.agents/skills/sdd/avance-y-tiempos.md`
- Resultado esperado: una sección «Pasos con tiempos»; la fila «Tiempo» en la tabla del PUNTO DE RETOMO
- Herramienta: a mano (`grep`)
- Escribe: no

### CP-02 Una sola fuente de la regla
- RF: RNF «Fuente única»
- Pasos: `grep -rln -E 'Total implementación|Documentación SDD|en lote' template/.agents template/.docs`
- Resultado esperado: la regla solo está en `avance-y-tiempos.md`; los demás archivos que aparezcan solo
  remiten a «Pasos con tiempos» (plantilla y leyenda nombran las filas sin redefinirlas)
- Herramienta: a mano (`grep`)
- Escribe: no

### CP-03 Plantilla con columnas y fila «Tiempo»
- RF: RF-1, RF-5
- Pasos: `awk '/^## 05_avance.md/,/^## 06_flujo.md/' template/.agents/skills/sdd/plantillas.md | grep -n -E 'Estimado|Real|Delta|Tiempo'`
- Resultado esperado: el encabezado de la tabla Pasos con Estimado, Real y Delta y la fila «Tiempo»
- Herramienta: a mano (`awk`, `grep`)
- Escribe: no

### CP-04 Remisiones en los cuatro comandos
- RF: RF-1, RF-2, RF-11, RF-12
- Pasos: `grep -c 'Pasos con tiempos' template/.agents/commands/sdd-tasks.md template/.agents/commands/sdd-implement.md template/.agents/commands/sdd-status.md template/.agents/commands/sdd-cierre.md`;
  `grep -c 'desvió fuerte' template/.agents/commands/sdd-cierre.md`
- Resultado esperado: al menos `1` en cada comando; `0` en el segundo
- Herramienta: a mano (`grep`)
- Escribe: no

### CP-05 Leyenda igual en `template/` y en la raíz
- RF: RF-13, RF-14
- Pasos: `diff <(awk '/^## Leyenda de estatus/,/^## Índice/' .docs/002_specs/README.md) <(awk '/^## Leyenda de estatus/,/^## Índice/' template/.docs/002_specs/README.md)`;
  `grep -c 'Pasos con tiempos' template/.docs/002_specs/README.md`
- Resultado esperado: `diff` vacío; al menos `1` (criterio 1 y «Un paso que cuenta»)
- Herramienta: a mano (`diff`, `grep`)
- Escribe: no

### CP-06 Versión y changelog
- RF: —
- Pasos: `cat VersionAgentKitSdd`; `grep -n '0.17.0' ChangelogAgentKitSdd.md`
- Resultado esperado: `0.17.0`; una entrada 0.17.0
- Herramienta: a mano
- Escribe: no

### CP-07 Piloto: sembrar y cuadrar en esta carpeta
- RF: RF-9, RF-2, RF-4, RF-5, RF-7
- Datos de partida: `05_avance.md` de `0022` con su tabla Tiempos y sin la tabla Pasos con tiempos
- Pasos:
  1. La sesión siembra la tabla Pasos con tiempos con la regla de `avance-y-tiempos.md` (RF-9).
  2. Comparar a mano cada subtotal de fase, el «Total implementación», la «Documentación SDD» y la fila
     «Tiempo» contra la suma de la tabla Tiempos.
- Resultado esperado: todos cuadran; las fases sin filas dicen «pendiente»; los lotes, «en lote»
- Herramienta: a mano (en el chat)
- Escribe: sí (`05_avance.md` de esta carpeta; lo escribe la sesión principal)

## Corridas
| Caso | Fecha | Estado | Obtenido / nota |
|---|---|---|---|
| CP-01 | 2026-11-22 | obtenido | `## Pasos con tiempos` en la línea 123; fila «Tiempo» en la tabla del PUNTO DE RETOMO (línea 38) |
| CP-02 | 2026-11-22 | obtenido | 3 archivos: la fuente, `plantillas.md` y la leyenda de `template/`; los dos últimos solo remiten o nombran las filas |
| CP-03 | 2026-11-22 | obtenido | encabezado con Estimado, Real y Delta (línea 25) y fila «Tiempo» (línea 20) |
| CP-04 | 2026-11-22 | obtenido | `1` en cada uno de los cuatro comandos; `0` de «desvió fuerte» |
| CP-05 | 2026-11-22 | obtenido | `diff` vacío; `2` en la leyenda de `template/` |
| CP-06 | 2026-11-22 | obtenido | `0.17.0`; entrada `## 0.17.0 — 2026-11-22` |
| CP-07 | 2026-11-22 | obtenido | tabla sembrada por la sesión; Total 17 min = suma de las filas `Fn` de Tiempos; Documentación SDD 11 min = spec 8 + clarify 1 + plan 1 + tasks 1; F7 «pendiente»; lotes «en lote» |
| CP-01 | 2026-11-22 21:11 | ok | validate v1, tester (claude-haiku-5-5): línea 123 y línea 38 |
| CP-02 | 2026-11-22 21:11 | ok | validate v1, tester: 3 archivos (fuente, `plantillas.md`, leyenda de `template/`); reviewer: los dos últimos solo nombran las filas y remiten |
| CP-03 | 2026-11-22 21:11 | ok | validate v1, tester: líneas 20 y 25 |
| CP-04 | 2026-11-22 21:11 | ok | validate v1, tester: `1` en los cuatro; `0` de «desvió fuerte» |
| CP-05 | 2026-11-22 21:11 | ok | validate v1, tester: `diff` vacío y `0`; re-corrida del reviewer: `2` (líneas 42 y 48); se toma la re-corrida |
| CP-06 | 2026-11-22 21:11 | ok | validate v1, tester: `0.17.0`; línea 3 |
| CP-07 | 2026-11-22 21:12 | ok | validate v1, reviewer: Total 22 min = suma de filas `Fn` de Tiempos; Documentación SDD 11 min antes de la fila `validate`; deltas cuadran |

## Evidencia formal (opcional)
No aplica.
