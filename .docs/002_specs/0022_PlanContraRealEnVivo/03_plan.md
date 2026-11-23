# 03 — Plan: Plan contra real en vivo
Spec: 01_spec.md (aprobada 2026-11-22 20:57)

## 1. Ubicación y capas
- Producto: `template/` (contrato de avance, plantillas, cuatro comandos y leyenda de estatus), más
  los docs de publicación de la raíz.
- Conocimiento propio del kit: `.docs/002_specs/README.md` de la raíz (la misma leyenda).
- Réplica local: `.agents/` de la raíz.
- Fuente única: la regla vive en `avance-y-tiempos.md` › «Pasos con tiempos». Plantillas, comandos y
  leyenda remiten y no la copian (constitución, principio 3).

## 2. Archivos que se crean o modifican
| Archivo | Capa | Responsabilidad | RF |
|---|---|---|---|
| `template/.agents/skills/sdd/avance-y-tiempos.md` | skill (fuente) | Sección nueva «Pasos con tiempos»: siembra al aprobar las tareas (fuente del estimado por modo), columnas y filas de fase, paso, Total y Documentación SDD, real y delta (unidad, rango contra el extremo superior S2, fase sin estimado S6, «pendiente», «en lote», «retirado»), actualización en la misma escritura que la fila de Tiempos, lote y lotes paralelos, `pausa por confirmar`, ids que no existen, siembra tardía (RF-9) y nueva siembra tras un cambio (RF-10). Fila «Tiempo» del PUNTO DE RETOMO. «Plan contra real (al cierre)» pasa a completar la tabla viva con una línea de ajuste siempre (S5); sin tabla viva, la de hoy. La estructura (punto 6) cambia en consecuencia | 1–10, 12 |
| `template/.agents/skills/sdd/plantillas.md` | skill | Plantilla de `05_avance.md`: encabezado de la tabla Pasos con Estimado, Real y Delta, y fila «Tiempo» opcional en el PUNTO DE RETOMO (remiten a la fuente) | 1, 5 |
| `template/.agents/commands/sdd-tasks.md` | comando | Punto 4: al anotar la aprobación, la sesión siembra la tabla en esa misma escritura (remisión) | 1, 10 |
| `template/.agents/commands/sdd-implement.md` | comando | Punto 8: el real del lote en la tabla, con remisión a la fuente | 2, 3 |
| `template/.agents/commands/sdd-status.md` | comando | Horas desde la tabla y la fila «Tiempo» si existen; si no, como hoy | 11 |
| `template/.agents/commands/sdd-cierre.md` | comando | Punto 6: completa la tabla viva y escribe siempre la línea de ajuste; quita «si alguna fase se desvió fuerte» | 12 |
| `template/.docs/002_specs/README.md` | leyenda | Criterio 1: la tabla de plan contra real es la tabla Pasos con tiempos si existe (RF-13). «Un paso que cuenta»: las filas de fase, Total y Documentación SDD no son pasos (RF-14) | 13, 14 |
| `.docs/002_specs/README.md` (raíz) | leyenda propia | El mismo texto que `template/` (hoy idénticos) | 13, 14 |
| `ChangelogAgentKitSdd.md`, `VersionAgentKitSdd` | publicación | Entrada 0.17.0 y versión 0.17.0 | — |
| `manifiestos/0.17.0.md` (nuevo) | manifiesto | Huellas de `template/` tras el último cambio (E20) | — |
| `.agents/` de la raíz (local) | réplica | `cp` de lo tocado y `diff -rq template/.agents .agents` | — |

Sin cambios: `agents/coordinator.md`, `commands/sdd-change.md`, `validar-estructura.md` (E13, E22),
`README.md` e `INSTALAR.md` (no citan el plan contra real).

## 3. Base de datos
No aplica (sin `sql/`).

## 4. Seguridad
No aplica: solo Markdown, sin secretos ni datos personales. Se respetan los Guardrails (sin BD).

## 5. Liberación / piloto
Versión 0.17.0. Piloto: la primera spec que pase por `/sdd-tasks` con la regla nueva. Esta misma
carpeta, si se reabre, gana la tabla en su siguiente paso (RF-9).

## 6. Decisiones
| Decisión | Alternativa descartada | Por qué |
|---|---|---|
| La tabla Pasos con tiempos es la tabla de plan contra real (S1) — [ADR 0039](../../004_adr/0039_TablaPasosConTiemposEsElPlanContraReal.md) | Mantener la tabla Pasos y otra tabla por fase en el cierre | Una sola cuenta: sin duplicar ni descuadrar números; el cierre solo completa |
| Columnas nuevas en la tabla Pasos (decisión 5) | Tabla aparte por fase; estimado por paso | Elegido por la persona; el estimado sigue por fase (S3) |
| Actualizar la tabla en la misma escritura que la fila de Tiempos | Recalcular al consultar (`/sdd-status`) | Leer el avance sin sumar es el objetivo; una llamada aparte cuesta caché (`MEMORY.md`) |
| Línea de ajuste siempre en el cierre (S5) | Umbral de «desvío fuerte» | Sin umbral pedido; «sin ajuste» es verificable |
| Leyenda editada en `template/` y en la raíz | Solo en `template/` | Hoy son idénticas y el índice de la raíz la usa |

## 7. Fases y estimado
Estimado en minutos y por ola (`MEMORY.md`, 2026-11-22). F1 a F5 tocan archivos disjuntos: una ola.

| Fase | Contenido | RF | Estimado (min) |
|---|---|---|---|
| F0 | Preparación: releer las secciones citadas y fijar el nombre «Pasos con tiempos» y la fila «Tiempo» | — | 3 |
| F1 | Fuente: `avance-y-tiempos.md` | 1–10, 12 | 10 |
| F2 | Plantilla: `plantillas.md` (`05_avance.md`) | 1, 5 | 3 |
| F3 | Remisiones: `sdd-tasks.md`, `sdd-implement.md`, `sdd-status.md`, `sdd-cierre.md` | 1–3, 10–12 | 6 |
| F4 | Leyenda: `template/.docs/002_specs/README.md` y `.docs/002_specs/README.md` | 13, 14 | 4 |
| F5 | Publicación: changelog y versión 0.17.0 | — | 3 |
| F6 | Réplica (`cp`, `diff -rq`), lint por archivo, ensayo de instalación, coherencia de remisiones | criterios | 6 |
| F7 | Manifiesto 0.17.0 y E20 (después del último cambio a `template/`) | criterios | 5 |
| | **Total** (40 sumado; la ola F1–F5 cuenta por su fase más larga) | | **≈ 24** |

## 8. Criterios de aceptación
- Cada RF de la spec tiene su texto en la fuente o en una remisión (tabla de la sección 2), sin copias
  de la regla fuera de `avance-y-tiempos.md`.
- Un ejemplo de tabla Pasos con tiempos en la fuente cuadra a mano: un paso, un lote, una fase en lote,
  una fila de documentación y una `pausa por confirmar`.
- `/sdd-status` y `/sdd-cierre` remiten a la tabla viva; la leyenda (criterio 1 y «Un paso que cuenta»)
  la reconoce y no cuenta sus filas de fase como pasos.
- Lint de `AGENTS.md` › Comandos en cada archivo tocado; ensayo de instalación sin pisar nada;
  `diff -rq` solo con las excepciones aceptadas; E20 en verde con `manifiestos/0.17.0.md`.
