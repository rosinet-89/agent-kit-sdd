# 05 — Avance: Bitácora por subagente
Leyenda: `[ ]` Pendiente · `[~]` En curso · `[x]` Hecho · `[!]` Bloqueado

## PUNTO DE RETOMO — leer esto primero
| Dato | Contenido |
|---|---|
| Modo | `completo`. Condiciones de [topes-y-modos.md](../../../.agents/skills/sdd/topes-y-modos.md): 1 no se puede comprobar hasta el plan (skill `bitacora-uso` y probables remisiones en `avance-y-tiempos.md` y `sdd-cierre.md`); 2 sí (ninguna otra spec en curso); 3 sí (no toca Guardrails, constitución ni numeración); 4 no (alternativas reales: id por entrada contra ventana filtrada, coordinación aparte contra repartida; ADR candidato); 5 sí (sin `sql/`); 6 no (cambia cómo se registran y completan las entradas y el reporte); 7 probable sí (se juzga al planear); 8 no se puede comprobar (`skills` y quizá `commands`). Flujo: se decide en el plan |
| Última sesión | 2026-11-22 22:03 |
| Fase actual | `Cerrado` (2026-11-22): cumple los cinco criterios |
| Último paso cerrado | `cierre` (2026-11-22 22:03): fila `cierre`, Pasos con tiempos y plan contra real; tokens de la bitácora completados |
| Siguiente paso | Ninguno |
| Bloqueos abiertos | Ninguno |
| Estado BD | No aplica |
| Código | `bitacora-uso/SKILL.md`, `avance-y-tiempos.md`, `VersionAgentKitSdd`, `ChangelogAgentKitSdd.md`, `manifiestos/0.18.0.md` y docs de la spec (sin commit) |
| Tiempo | Implementación: estimado 30 min · real 6 min · delta -24 min · Documentación SDD: 17 min |

## Pasos
Forma y reglas en [avance-y-tiempos.md](../../../.agents/skills/sdd/avance-y-tiempos.md) › «Pasos con tiempos».
| Paso | Estado | Estimado | Real | Delta | Nota |
|---|---|---|---|---|---|
| **F0 — Línea base** | [x] | 2 min | 1 min | -1 min | |
| F0.1 | [x] | | 1 min | | |
| **F1 — Skill `bitacora-uso`** | [x] | 12 min | 1 min | -11 min | lote L1 |
| F1.1 | [x] | | 1 min | | lote F1.1, F1.2, F1.3 |
| F1.2 | [x] | | en lote F1.1, F1.2, F1.3 | | |
| F1.3 | [x] | | en lote F1.1, F1.2, F1.3 | | |
| **F2 — Remisión en `avance-y-tiempos.md`** | [x] | 2 min | 1 min | -1 min | lote L2 |
| F2.1 | [x] | | 1 min | | lote F2.1, F3.1 |
| **F3 — Versión** | [x] | 3 min | en lote | — | |
| F3.1 | [x] | | en lote F2.1, F3.1 | | |
| **F4 — Pruebas** | [x] | 5 min | 1 min | -4 min | |
| F4.1 | [x] | | 1 min | | lote F4.1, F4.2 |
| F4.2 | [x] | | en lote F4.1, F4.2 | | |
| **F5 — Réplica, chequeos y manifiesto** | [x] | 6 min | 2 min | -4 min | |
| F5.1 | [x] | | 2 min | | lote F5.1, F5.2, F5.3 |
| F5.2 | [x] | | en lote F5.1, F5.2, F5.3 | | |
| F5.3 | [x] | | en lote F5.1, F5.2, F5.3 | | |
| **Total implementación** | | 30 min | 6 min | -24 min | |
| **Documentación SDD** | | sin estimado | 17 min | — | spec, clarify, plan, change, tasks, validate, change, cierre |
## Decisiones
1. [2026-11-22 21:34] Antecedente: `0021_BitacoraDeUso` (crea la skill que se cambia) y `0022_PlanContraRealEnVivo` (midió el problema); ambas cerradas. Número 0023 confirmado por la persona; bitácora de esta spec: sí.
2. [2026-11-22 21:34] Modo `completo` sin confirmación (condiciones 4 y 6 en no). Planner en el chat: mismo modelo que la sesión (opus).
3. [2026-11-22 21:36] Autochequeo del planner antes de la pasada 1: 3 correcciones (RF-4 sin coordinación si la escritura ya lleva entrada en el chat con la misma ventana; RF-8 desempate entre dos coordinaciones; criterio de la 0022 contra sesión más transcripts).
4. [2026-11-22 21:37] Clarify pasada 1 (reviewer en el chat): 4 hallazgos (RF-3 contra «Registro por tramo»; actividades; sesión «no disponible»; coordinación en hallazgos). Planner corrige en automático: RF-3, RF-9, RF-10, RF-13 nuevo y 2 supuestos marcados (actividad una entrada por turno; coordinación como tramo en hallazgos).
5. [2026-11-22 21:37] Clarify pasada 2 por delta: sin hallazgos.
6. [2026-11-22 21:37] La persona aprueba la spec con los 5 supuestos explícitos: `Estado: aprobada`.
7. [2026-11-22 21:40] Plan: modo completo sin flujo (D5, mismo criterio que 0021 y 0022). Medición de transcripts de 0022 (solo `usage`): lotes L1–L5 de 45 345 a 140 814 tokens, coordinación 21:03–21:05 1 822 349; no coinciden con las cifras de la idea (criterio 2 de la spec, pregunta 1 de `02`).
8. [2026-11-22 21:42] `/sdd-change`: el criterio de finalización 2 de `01_spec.md` pasa de las cifras de la idea (77–171 mil por lote, ≈ 2,49 M) a las medidas en el plan (L1–L5: 45 345 a 140 814; coordinación 1 822 349; ola ≈ 2,21 M contra 15,9 M). Sin cambio de RF ni de estimado; en `03_plan.md` solo el criterio 2. La persona eligió el texto exacto en la pregunta: queda aprobado. `Estado: aprobada` se mantiene.
9. [2026-11-22 21:42] La persona aprueba el ADR 0040 (D1 a D3), que reemplaza al ADR 0038; el 0038 queda «Reemplazado por 0040».
10. [2026-11-22 21:45] La persona aprueba plan y tareas (`04_tasks.md`, 6 fases, 11 pasos, ≈ 30 min; `07_casos_prueba.md`, 7 casos manuales). Ola 2: L1 (F1) a `implementer` subagente (configurado); L2 (F2.1, F3.1) en el chat por decisión de la persona (`MEMORY.md`: delegar pasos cortos no ahorra).
11. [2026-11-22 21:45] F0.1: copias en `.agents/local/0023-base/`; `git diff HEAD` de los cuatro archivos vacío.
12. [2026-11-22 21:46] L1 (implementer claude-sonnet-5-5, subagente; `agentType` y `model` confirmados en `.meta.json`): F1.1 a F1.3 cumplen «Hecho cuando». Ampliación 1 (plan §1–§6 y CP-01 a CP-04, motivo: saber qué comprobará el grep). La sesión retoca 2 detalles de forma (nota del ejemplo, salto de línea).
13. [2026-11-22 21:47] CP-05: lotes iguales a §3. La coordinación del criterio 2 (1 822 349) usa 21:03–21:05; con la ventana de delegación de la spec (Fin 21:06) y la resta de RF-7 (entradas en el chat F0.1 y F6.4) da 1 252 499. En los dos casos la suma (2,21 M o 1,64 M) es menor que 15,9 M. Queda para `validate` y la persona: el número del criterio 2 no sale de la regla escrita.
14. [2026-11-22 21:47] CP-07: 8 de 12 iguales; 4 difieren por el comando de un solo uso (no guardado en 0021/0022), no por la regla anterior, que sigue igual en la skill (RF-9). Para `validate`.
15. [2026-11-22 21:49] F5: réplica sin diferencias fuera de las admitidas; ensayo de instalación completo sin pisar; huella de humo OK; `manifiestos/0.18.0.md` (56 filas; cambian `bitacora-uso/SKILL.md` y `avance-y-tiempos.md`); E20 en verde. `/sdd-estructura` (auxiliar claude-sonnet-5-5, ruteo; `.meta.json` confirmado): sin FALLA, 4 avisos (E1 enlaces de herramientas no declaradas, E22 celda del índice desfasada, E8 `claudio`, E11 rutas locales); E22 se corrige en esta escritura.
16. [2026-11-22 21:49] Piloto de la regla nueva desde la fila F5 (primera tras la réplica): la bitácora de esta spec sigue con la tabla vieja para las entradas anteriores (RF-9, sin reescribir) y abre una segunda tabla con «Sesión» y «Subagente». F5 da una entrada en el chat y una delegada (`auxiliar`), sin coordinación (misma ventana, RF-4).
17. [2026-11-22 21:54] Validate vuelta 1: `tester` claude-haiku-5-5 subagente (configurado) corre CP-01 a CP-07 con encargo cerrado y un comando de lectura de un solo uso de la sesión (scratchpad, no versionado, sin contenido). Reviewer claude-opus-5-5 en el chat: APROBADO. Lint OK en los 15 archivos del alcance; réplica de los dos archivos sin diferencias; seguridad en modo «solo secretos», sin hallazgos. Ajeno o no atribuible: `MEMORY.md` (línea del 2026-11-22 sobre delegar pasos cortos); la persona decide si es del feature.
18. [2026-11-22 21:54] Decisión 13 resuelta: con la regla escrita (ventana de delegación 21:03–21:06 menos los mensajes que ya cuentan F0.1 y F6.4, RF-7 y RF-8, por `message.id`) la coordinación da 1 033 088; el 1 252 499 de la decisión 13 era 21:03–21:05 menos F0.1. Suma de las 6: 1 418 921, menor que 15 911 085. El 1 822 349 del criterio 2 mide la ventana 21:03–21:05, no la regla: observación para la persona (aceptarlo o corregir el texto con `/sdd-change`); no bloquea, el fondo del criterio se cumple.
19. [2026-11-22 21:54] Decisión 14 resuelta: CP-07 con un solo comando da las 13 ventanas iguales a lo guardado en la bitácora de 0022 (la de cierre está en la sesión `c427763f…`); 21:07 sigue con dos sesiones. Las 4 diferencias de F4 venían de los comandos de entonces. Criterio 4 cumplido.
20. [2026-11-22 21:59] `/sdd-change` (planner claude-opus-5-5 en el chat): el criterio 2 de `01_spec.md` pasa a la regla escrita: coordinación con su ventana de delegación (21:03–21:06) menos F0.1 y F6.4 = 1 033 088; suma de las 6 ≈ 1,42 M < 15,9 M. Sin cambio de RF, casos límite, fuera de alcance, tareas ni estimado. Aplicado en `03_plan.md` (criterio 2), `02_analisis_impacto.md` §3 (fila y nota) y `07_casos_prueba.md` (CP-05, paso 2 y esperado); la corrida de validate ya da ese valor (`ok`, sin repetirla). ADR 0040 sin cambio. La persona aprueba: `Estado: aprobada` se mantiene; observación de la decisión 18 cerrada.
21. [2026-11-22 22:03] Cierre (chat claude-opus-5-5): previa OK (11 pasos `[x]`, sin verificaciones manuales, sin flujo por D5, sin pausa por confirmar, validación del conjunto registrada); E20 en verde (56 de 56 huellas de `manifiestos/0.18.0.md`, humo OK). Criterios 3 (sin aviso ni ticket), 4 (columna de pendiente vacía) y 5 (sin deuda de este requerimiento) cumplidos.
22. [2026-11-22 22:03] Bitácora: tokens completados con un comando de lectura de un solo uso (scratchpad, solo `usage`). Las entradas sin «Sesión» de `change` (21:41) y `tasks` (21:42) caen en dos sesiones (`/clear` entre ellas): «no disponible (sesiones paralelas)». La de L2 (21:45) da 0: su ventana ya la cuentan F0.1 y L1 (`message.id` una sola vez).
23. [2026-11-22 22:03] `MEMORY.md` (línea de delegar pasos cortos, decisión 17): se trata como cambio ajeno salvo que la persona lo incluya al confirmar los commits.
24. [2026-11-22 22:04] La persona confirma los 3 commits del cierre (kit 0.18.0, ADR 0040, spec 0023): criterio 2 cumplido, queda `Cerrado`. `MEMORY.md` queda fuera, como cambio ajeno.
## Pruebas
- CP-01 a CP-07 (2026-11-22 21:47): obtenidos en `07_casos_prueba.md` › «Corridas»; observaciones en las decisiones 13 y 14.
- Validate vuelta 1 (2026-11-22 21:54): CP-01 a CP-07 `ok` en [`07_casos_prueba.md`](07_casos_prueba.md) › «Corridas». Sin verificaciones manuales (sin UI).
## Tiempos
| Paso | Inicio | Fin | Minutos | Pausa | Nota |
|---|---|---|---|---|---|
| spec | 2026-11-22 21:30 | 2026-11-22 21:34 | 4 | 0 | planner claude-opus-5-5 en el chat; 01_spec en borrador, 12 RF |
| clarify | 2026-11-22 21:36 | 2026-11-22 21:37 | 1 | 0 | pasada 2 de 3; reviewer y planner claude-opus-5-5 en el chat; 4 hallazgos en la 1, 0 en la 2 |
| plan | 2026-11-22 21:38 | 2026-11-22 21:40 | 2 | 0 | planner claude-opus-5-5 en el chat; 02 y 03; medición de 0022 |
| change | 2026-11-22 21:41 | 2026-11-22 21:42 | 1 | 0 | planner claude-opus-5-5 en el chat; criterio 2 a lo medido; ADR 0040 |
| tasks | 2026-11-22 21:42 | 2026-11-22 21:45 | 3 | 0 | planner claude-opus-5-5 en el chat; 04_tasks y 07_casos_prueba; aprobadas |
| F0.1 | 2026-11-22 21:45 | 2026-11-22 21:45 | 1 | 0 | implementer claude-opus-5-5 en el chat; línea base |
| F1.1, F1.2, F1.3 | 2026-11-22 21:45 | 2026-11-22 21:46 | 1 | 0 | lote L1, ola 2; implementer claude-sonnet-5-5 subagente (configurado) |
| F2.1, F3.1 | 2026-11-22 21:45 | 2026-11-22 21:45 | 1 | 0 | lote L2, ola 2; implementer claude-opus-5-5 en el chat (decisión 10) |
| F4.1, F4.2 | 2026-11-22 21:46 | 2026-11-22 21:47 | 1 | 0 | lote; implementer claude-opus-5-5 en el chat; CP-01 a CP-07 |
| F5.1, F5.2, F5.3 | 2026-11-22 21:47 | 2026-11-22 21:49 | 2 | 0 | lote; implementer claude-opus-5-5 en el chat; /sdd-estructura con auxiliar claude-sonnet-5-5 (ruteo) |
| validate | 2026-11-22 21:52 | 2026-11-22 21:54 | 2 | 0 | vuelta 1 de 2: APROBADO; reviewer claude-opus-5-5 en el chat; tester claude-haiku-5-5 subagente (configurado) |
| change | 2026-11-22 21:58 | 2026-11-22 21:59 | 2 | 0 | planner claude-opus-5-5 en el chat; criterio 2 a la regla escrita (decisión 20) |
| cierre | 2026-11-22 22:01 | 2026-11-22 22:03 | 2 | 0 | chat claude-opus-5-5; previa, E20, tokens de la bitácora, plan contra real |
## Plan contra real

Tabla de plan contra real: la tabla Pasos con tiempos (arriba). Implementación 6 min contra 30 estimados (20 %).

Ajuste del siguiente plan: en una spec de solo Markdown con lotes en el chat, estimar ≈ 1 min por paso y no por fase (aquí 11 pasos, 6 min).
