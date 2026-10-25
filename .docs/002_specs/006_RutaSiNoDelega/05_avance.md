# 05 — Avance: Ruta alterna si un agente no puede delegar
Leyenda: `[ ]` Pendiente · `[~]` En curso · `[x]` Hecho · `[!]` Bloqueado

## PUNTO DE RETOMO — leer esto primero
| Dato | Contenido |
|---|---|
| Última sesión | 2026-10-25 17:04 |
| Fase actual | Cerrada |
| Último paso cerrado | `cierre` 2026-10-25 15:50; sin `06_flujo` (decisión 8) |
| Siguiente paso | Ninguno en esta carpeta. Manifiesto y E20 regenerados por la `005` (F6 16:44, F8 16:49; decisión 10). |
| Bloqueos abiertos | Ninguno |
| Coordinación | Caso de origen el 2026-10-25 en `004_ModeloYPermisosAgentes`. No editar las carpetas `003`, `004` ni `005`. El cuerpo de `coordinator.md` y la skill ya llevan la regla de esta spec. La cabecera del coordinator queda para la `004`: no revertirla cuando la escriba. El manifiesto y E20, al cierre, quien termine último. |
| Estado BD | No aplica |
| Código | Skill, contrato, coordinator, nueve comandos y changelog en `template/`; copia local igual. Manifiesto y E20 los regeneró la `005`; ningún archivo de `template/` es más nuevo que `manifiestos/0.5.0.md`. |

## Pasos
- [x] F0.1 Releer lo compartido
- [x] F1.1 Un solo paso y otra ruta
- [x] F2.1 Fila única
- [x] F3.1 Soltar la fase
- [x] F4.1 Una frase, nueve archivos
- [x] F5.1 Viñeta de 0.5.0
- [x] F6.1 Refrescar solo esos archivos

## Decisiones
1. **[2026-10-25 15:27] El caso se documenta como spec nueva `006_RutaSiNoDelega`, no como nota dentro de la `004`.** Por qué: el atasco ocurrió al pedir el plan de la `004`, y el cambio es del kit (el flujo sigue y deja constancia). La `004` ya está en `plan`. Alternativa descartada: anotarlo solo en el avance de la `004` o en `MEMORY.md`.
2. **[2026-10-25 15:34] `clarify` de esta spec lo hace el chat, sin lanzar al coordinator ni al reviewer.** Por qué: el usuario lo pidió en esta sesión («ejecuta todo desde el chat») porque el cambio es que las fases no se tarden. Constancia: no hubo fallo de delegación; la ruta la eligió el usuario. Alternativa descartada: lanzar al reviewer.
3. **[2026-10-25 15:35] El usuario aprobó los cinco hallazgos y el chat ajustó `01_spec.md`.** Quedaron RF-1 a RF-12. Alternativa descartada: dejar la spec como estaba.
4. **[2026-10-25 15:37] Spec aprobada** (`Estado: aprobada`). El usuario confirmó el texto ajustado.
5. **[2026-10-25 15:39] Plan y tareas los redactó el chat.** Por qué: la decisión 2 sigue vigente en esta spec (el usuario pidió hacer las fases en el chat). Alternativa descartada: lanzar al planner. El ADR de la fuente única queda marcado en `03_plan.md` y no se crea hasta el visto bueno.
6. **[2026-10-25 15:40] Plan y tareas aprobados.** El usuario confirmó. F0.1 no cita RF: la fase F0 del plan es la relectura previa. Se parte de este texto, leído a las 15:40, y no se revierte lo que escriban después la `004` o la `005`. (a) `template/.agents/skills/sdd/SKILL.md` líneas 75-79: el chat lanza al coordinator y no ejecuta las fases. (b) `avance-y-tiempos.md` no tiene constancia ni fila del intento que no delegó. (c) `coordinator.md` conserva el frontmatter con `mode`, `permissions` y `tools: Agent(...)`; el cuerpo no suelta la fase en una línea. La `004` sigue en `F0` cerrada y su siguiente paso es `F1.1`, que sí editará ese frontmatter: F3 de esta spec relee el archivo en ese momento y no toca la cabecera. (d) `CHANGELOG.md` `0.5.0`, última viñeta: la skill manda lanzar al coordinator. La `005` aún no tiene tareas y no ha escrito el changelog. `template/` no se editó en este paso. Alternativa descartada: empezar F1 antes de anotar la línea base.
7. **[2026-10-25 15:43] F1 a F6 en este chat, en un turno.** Por qué: el usuario pidió implementar todas las fases. La `004` seguía en F0, así que el texto era el de la decisión 6. El frontmatter de `coordinator.md` no se tocó. `VERSION` sigue en `0.5.0`. El reloj del turno fue 15:42–15:43; cada fila usa el piso de 1 minuto del contrato. Alternativa descartada: un paso por turno.
8. **[2026-10-25 15:50] `06_flujo.md` no es requerido.** Por qué: el usuario lo dijo al pedir el cierre. No se abre deuda. Alternativa descartada: redactar el flujo antes de cerrar.
9. **[2026-10-25 15:51] Quedaron registrados el ADR 010 y la línea de MEMORY.** Por qué: el usuario lo pidió al cerrar («registralo»). El 010 no reemplaza al 009. Alternativa descartada: dejarlos solo propuestos.
10. **[2026-10-25 17:04] El pendiente de manifiesto y E20 ya lo cerró la `005`.** F6 a las 16:44 y F8 a las 16:49 regeneraron `manifiestos/0.5.0.md` (42 de 42). Ningún archivo de `template/` es más nuevo que ese manifiesto. Esta nota solo corrige el retomo; no se regenera otra vez y no se edita `template/`. Alternativa descartada: regenerar el manifiesto sin un cambio de `template/`.

## Pruebas
- **validate (2026-10-25 15:47).** VEREDICTO: APROBADO. Lint utf-8 sin BOM en 26 archivos. Seguridad modo «solo secretos»: sin hallazgos en 13 archivos. Frontmatter de `coordinator.md` igual a `HEAD`. Copia local igual en 12 pares. Verificación manual: no aplica. `README.md` mezcla la fila 006 con la fila 005; el resto no se revisó como propio.

## Tiempos
| Paso | Inicio | Fin | Minutos | Pausa | Nota |
|---|---|---|---|---|---|
| spec | 2026-10-25 15:27 | 2026-10-25 15:31 | 4 | 0 | carpeta, índice y `01_spec` (9 RF, 4 RNF) |
| clarify | 2026-10-25 15:33 | 2026-10-25 15:37 | 4 | 0 | revisión en el chat, cinco hallazgos y texto aprobado |
| plan | 2026-10-25 15:37 | 2026-10-25 15:38 | 1 | 0 | `02` y `03` en el chat; ADR marcado, no creado |
| tasks | 2026-10-25 15:38 | 2026-10-25 15:39 | 1 | 0 | `04_tasks.md`, F0 a F6, 2,25 h |
| F0.1 | 2026-10-25 15:39 | 2026-10-25 15:40 | 1 | 0 | línea base de skill, contrato, coordinator y changelog; sin editar `template/` |
| F1.1 | 2026-10-25 15:42 | 2026-10-25 15:43 | 1 | 0 | sección «Si no hay un solo paso» en la skill; piso de 1 min, turno continuo |
| F2.1 | 2026-10-25 15:42 | 2026-10-25 15:43 | 1 | 0 | constancia en el contrato; piso de 1 min, turno continuo |
| F3.1 | 2026-10-25 15:42 | 2026-10-25 15:43 | 1 | 0 | coordinator suelta la fase; frontmatter intacto; piso de 1 min |
| F4.1 | 2026-10-25 15:42 | 2026-10-25 15:43 | 1 | 0 | frase en nueve comandos; piso de 1 min, turno continuo |
| F5.1 | 2026-10-25 15:42 | 2026-10-25 15:43 | 1 | 0 | viñeta nueva en 0.5.0; VERSION sigue 0.5.0 |
| F6.1 | 2026-10-25 15:42 | 2026-10-25 15:43 | 1 | 0 | 12 archivos copiados; napkin sin tocar |
| validate | 2026-10-25 15:44 | 2026-10-25 15:47 | 3 | 0 | reviewer, un solo paso; APROBADO |
| cierre | 2026-10-25 15:49 | 2026-10-25 15:50 | 1 | 0 | sin flujo, por decisión del usuario; sin manifiesto |

## Plan contra real
| Fase | Estimado | Real | Delta |
|---|---|---|---|
| F0 | 0,25 h | 1 min | −14 min |
| F1 | 0,5 h | 1 min | −29 min |
| F2 | 0,25 h | 1 min | −14 min |
| F3 | 0,25 h | 1 min | −14 min |
| F4 | 0,5 h | 1 min | −29 min |
| F5 | 0,25 h | 1 min | −14 min |
| F6 | 0,25 h | 1 min | −14 min |
| Documentación SDD | sin estimado | 14 min | — |

El estimado de F1 a F6 (2 h) era de una pasada en serie. El trabajo fue un turno de un minuto. En el siguiente plan de este tipo, estimar en minutos. El piso de 1 minuto, repetido seis veces sobre el mismo minuto de reloj, no se lea como seis minutos de trabajo: el real de ese turno es el reloj.
