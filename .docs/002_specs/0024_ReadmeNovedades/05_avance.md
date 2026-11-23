# 05 — Avance: README con la versión actual y sus novedades
Leyenda: `[ ]` Pendiente · `[~]` En curso · `[x]` Hecho · `[!]` Bloqueado

## PUNTO DE RETOMO — leer esto primero
| Dato | Contenido |
|---|---|
| Modo | `ligero`, confirmado por la persona (decisión 5). Las 8 condiciones se cumplen; la 8 con lectura. Lista en `01_spec.md` › «Modo» |
| Última sesión | 2026-11-22 22:22 |
| Fase actual | `cierre`: `Cerrado` (decisión 8), con el criterio 2 en los commits |
| Último paso cerrado | Cierre (decisión 8). Validación del conjunto aprobada (decisiones 6 y 7) |
| Siguiente paso | Ninguno en esta spec. La persona publica con `git push` |
| Bloqueos abiertos | Ninguno |
| Estado BD | No aplica |
| Código | `README.md` y `.docs/002_specs/README.md` (sin commit) |
| Tiempo | Implementación: estimado 15 min · real 1 min · delta -14 min (fases con real) · Documentación SDD: 5 min · pausa por confirmar: 0 |

## Pasos
Forma y reglas en [avance-y-tiempos.md](../../../.agents/skills/sdd/avance-y-tiempos.md) › «Pasos con tiempos».
| Paso | Estado | Estimado | Real | Delta | Nota |
|---|---|---|---|---|---|
| **F1 — README** | [x] | 15 min | 1 min | -14 min | |
| F1.1 | [x] | | 1 min | | implementer subagente, con una vuelta de corrección |
| **Total implementación** | | 15 min | 1 min | -14 min | |
| **Documentación SDD** | | sin estimado | 5 min | — | spec, clarify, tasks, validate, cierre |

## Decisiones
1. **[2026-11-22 22:17] Origen y alcance.** La persona publicó 0.18.0 y pide actualizar el README
   «no solo la versión sino lo que cambió». README sin cambios desde 0.16.0 (`9117418`). Se abre spec
   (memoria «flujo SDD obligatorio en el kit»), igual que `0018_ReadmeYFlujoGeneral`.
2. **[2026-11-22 22:17] Antecedentes:** `0018_ReadmeYFlujoGeneral` (crea el README actual; cerrada) y,
   como fuente de lo que cambió, `0022_PlanContraRealEnVivo` y `0023_BitacoraPorSubagente` (cerradas).
   Deuda: ninguna abierta (14 de 14 `Resuelta`). Bitácora de esta spec: sí (elegida por la persona).
3. **[2026-11-22 22:18] Clarify, pasada 1 de 1** (reviewer en el chat, modo automático): 2 hallazgos,
   corregidos por el planner sin decisiones de alcance. (a) RF-2 «3 líneas» era ambiguo: pasa a líneas
   de fuente de hasta 100 caracteres. (b) RF-2 no decía dónde viven el ADR y la skill: ADR en
   `.docs/004_adr/` y skill en `template/`.
4. **[2026-11-22 22:18] Spec y tareas en una sola puerta.** Por ser un cambio de un archivo, las
   tareas se redactan antes de aprobar la spec y se presentan juntas; si la persona cambia la spec,
   las tareas se rehacen.
5. **[2026-11-22 22:19] Spec, modo ligero y tareas aprobados** por la persona («Aprobar tal cual»):
   supuestos 1 a 3 aceptados. F1.1 va a `implementer` como subagente (Sonnet) y la sesión valida en
   el chat como reviewer, para que no produzca y valide el mismo rol.
6. **[2026-11-22 22:21] Validate aprobado** (reviewer en el chat, claude-opus-5-5; produjo el
   implementer en claude-sonnet-5-5). Una vuelta de corrección: las viñetas de «Novedades» tenían 4
   líneas con renglones de hasta 104 caracteres (RF-2); el ADR pasa a nombrarse sin enlace y cada
   versión queda en 3 líneas. Evidencia en «Pruebas».
7. **[2026-11-22 22:21] Verificación manual hecha.** La persona revisa la versión y «Novedades» y
   responde «ok», que también pide el cierre.
8. **[2026-11-22 22:22] Cierre: `Cerrado`.** La previa pasa: 1 paso `[x]`, sin verificaciones
   manuales pendientes (decisión 7), sin flujo (modo ligero), sin `pausa por confirmar` y validación
   del conjunto aprobada (decisiones 6 y 7). Chequeos de cierre: E20 no aplica (`VersionAgentKitSdd`
   sin cambio). Criterio 1: fila `cierre`, tabla Pasos y línea de ajuste. Criterio 3: sin ticket ni
   aviso. Criterio 4: columna de pendiente vacía. Criterio 5: ninguna deuda con Atendida por `0024`.
   El criterio 2 se decide con los commits. Tokens de la bitácora completados. Sin deuda nueva, sin
   ADR, sin propuesta de memoria y sin cambio de contexto.

## Pruebas
- F1.1 (RF-1 a RF-4), validate 2026-11-22 22:21: versión = `VersionAgentKitSdd` (0.18.0); «Novedades» con 0.18.0 y 0.17.0, 3 líneas de fuente cada una (máx. 98 caracteres), spec, ADR y skill de `template/`, cierre con el changelog; filas de `05_avance.md` y bitácora y atajo de `/sdd-status` al día; enlaces nuevos existen; utf-8 sin BOM. Verificación manual: la persona confirma la lectura en su visor («ok», 2026-11-22 22:21).

## Tiempos
| Paso | Inicio | Fin | Minutos | Pausa | Nota |
|---|---|---|---|---|---|
| spec | 2026-11-22 22:17 | 2026-11-22 22:18 | 1 | 0 | rol planner en el chat (claude-opus-5-5); antecedentes y deuda |
| clarify | 2026-11-22 22:18 | 2026-11-22 22:18 | 1 | 0 | pasada 1 de 1 (reviewer en el chat, claude-opus-5-5): 2 hallazgos, corregidos por el planner (modo automático) |
| tasks | 2026-11-22 22:18 | 2026-11-22 22:19 | 1 | 0 | rol planner en el chat (claude-opus-5-5); plan dentro, 1 paso, estimado 15 min |
| F1.1 | 2026-11-22 22:19 | 2026-11-22 22:20 | 1 | 0 | implementer subagente (claude-sonnet-5-5), motivo «otro modelo»; una vuelta de corrección del reviewer |
| validate | 2026-11-22 22:20 | 2026-11-22 22:21 | 1 | 0 | reviewer en el chat (claude-opus-5-5); RF-1 a RF-4 OK; queda la verificación manual |
| cierre | 2026-11-22 22:21 | 2026-11-22 22:22 | 1 | 0 | sesión principal (claude-opus-5-5); `Cerrado`, el criterio 2 con los commits |

## Plan contra real
La tabla viva es «Pasos». Qué ajustar: en una edición de un solo archivo de documentación con
implementer en subagente, estimar 5 min por paso, no 15.
