# 05 — Avance: Arranque ligero de sesión
Leyenda: `[ ]` Pendiente · `[~]` En curso · `[x]` Hecho · `[!]` Bloqueado

## PUNTO DE RETOMO — leer esto primero
| Dato | Contenido |
|---|---|
| Modo | `completo`, sin flujo (decisión 9). Validación del conjunto: aprobada (2026-11-21 18:57, decisión 8). Condiciones de [topes-y-modos.md](../../../.agents/skills/sdd/topes-y-modos.md): solo cumple la 5 (sin datos ni esquema); fallan 1, 2, 3, 4, 6, 7 y 8 |
| Última sesión | 2026-11-21 19:16 |
| Fase actual | `cierre`: `Cerrado` (decisiones 10 y 11) |
| Último paso cerrado | Cierre con sus commits (2026-11-21 19:16) |
| Siguiente paso | Ninguno en esta spec. Deuda abierta de origen: 0014 |
| Bloqueos abiertos | Ninguno |
| Estado BD | No aplica |
| Código | Kit 0.12.0 en 3 commits locales (`c1452e1` kit, `29ace0d` ADR 0032 y el de specs), sin push. Ayudas de prueba no versionadas en `.agents/local/0015/`. `MEMORY.md`: cambio ajeno previo, fuera de los commits |

## Pasos
Detalle en `04_tasks.md` (14 pasos). Casos: `07_casos_prueba.md`.

| Fase | Pasos | Estado |
|---|---|---|
| F0 Línea base | F0.1 a F0.2 | `[x]` |
| F1 Chequeo de arranque | F1.1 a F1.2 | `[x]` |
| F2 Archivo siempre cargado | F2.1 a F2.5 | `[x]` |
| F3 Actualización y versión | F3.1 a F3.3 | `[x]` |
| F4 Verificación | F4.1 a F4.2 | `[x]` |

## Decisiones
1. **[2026-11-21 10:36] Origen y alcance:** la persona pidió revisar el contexto que se carga al inicio de cada sesión. La medición de la primera llamada de la sesión (transcript `.jsonl`) dio ~56k tokens de entrada, de ellos ~26k de sistema y herramientas no controlables. Se aprobaron dos frentes para esta spec: (#5) `AGENTS.md` siempre cargado (13 314 bytes, ~3.8k tokens) y (#6) el protocolo «Cada sesión» (~9k tokens si se cumple completo: `.agents/sync-agents.md` 10 689 bytes, `PREFERENCIAS.md` 7 194, `.agents/README.md` 4 927, `MEMORY.md` 3 415, `formato-respuesta` 2 443, `napkin.md` 1 579, constitución 1 133). Fuera de esta spec, ya resueltos en la sesión: plugin caveman duplicado (desinstalado) y conectores MCP (la persona no los quiere quitar). Antecedente: `003_OptimizarArnes` (cerrada; redujo tokens de agentes y comandos, no el arranque de la sesión principal). Deuda abierta relacionada: ninguna.
2. **[2026-11-21 10:48] Respuestas a las 5 preguntas del planner:** (1) salen de `AGENTS.md` solo las secciones genéricas: «Qué lee cada rol»; «Formato y numeración», «Deuda técnica» y «Roles y comandos» se evalúan en el plan; «Comandos», «Stack» y «Reglas de negocio» se quedan. Descartadas: sacar todo salvo Guardrails y «Cada sesión», o solo «Qué lee cada rol». (2) S-1 fija en ≥35 %; el porcentaje de S-2 se fija en el plan. (3) Se implementa después de la 0013; clarify y plan pueden ir antes. (4) Proyectos ya instalados: el nuevo «Cada sesión» se incorpora a mano según `ACTUALIZAR.md` (ADR 007). Descartado: que `/sdd-enlaces` lo proponga. (5) La 0014 sigue el criterio de RF-1; anotado en su avance.
3. **[2026-11-21 11:00] Spec aprobada** por la persona tras la pasada 1 de clarify (rol reviewer en el chat): 10 hallazgos corregidos por el rol planner en el chat (método y escenario de medición, RF-6, RF-7, RF-9 con renglón de remisión, línea base de réplicas, tres casos límite y ensayo de actualización).
4. **[2026-11-21 16:08] Plan:** el bloqueo «después de la 0013» queda cumplido (0013 y 0014 cerradas). Medición de hoy (`02_analisis_impacto.md` § 3): lectura del arranque 29 782 bytes y `AGENTS.md` 14 279 / 12 990; las metas se miden contra la línea base congelada. Salen de `AGENTS.md` «Qué lee cada rol», «Roles y comandos» (a `.agents/README.md`) y «Formato y numeración»; se quedan «Deuda técnica» y «Skills de proyecto». Meta de `AGENTS.md`: ≥ 5 % frente a la línea base congelada. `sync-agents.md` se reordena y «Chequeo de arranque» dice qué leer.

5. **[2026-11-21 16:12] ADR 0032 aprobado** por la persona («Procede»): criterio de archivo siempre cargado; registrado en `.docs/004_adr/` y enlazado desde `03_plan.md` § 6.
6. **[2026-11-21 16:17] Plan y tareas aprobados** por la persona («Yes»).
7. **[2026-11-21 16:18] F0.1 Línea base (`wc -c`, commit `8c9681b`):** congelada de la spec: lectura del arranque 24 186; `AGENTS.md` 13 314 / 11 859. Hoy: lectura del arranque 29 782 (napkin 1 579, `MEMORY.md` 3 779, `.agents/README.md` 5 475, constitución 1 133, `sync-agents.md` 15 336, `formato-respuesta` 2 480); `AGENTS.md` 14 279 / 12 990. `/sdd-estructura` en el clon (antes): E4–E9, E12–E16 y E20 (49 de 49 huellas de 0.11.0) OK; E21 no aplica (sin `.kit-version`); E1 sin «visto»: `.gemini/*` y `.github/*` FALTA (AVISO), `.cursor/agents` es enlace de carpeta y `.opencode/agents` tiene 4 de 6 roles; E7: `caveman` y `reporte-sesiones` exentas; E10: `sdd-config` y `sdd-constitucion` fuera de la tabla de la skill (deuda 0011) y el ruteo del coordinator cita por archivo; E11: 51 rutas citadas sin destino, todas genéricas (`sql/`, `tests/`, `.gemini/`, modelos de OpenCode…), lista guardada para comparar; E15: `.agents/agents/claudio.md` es enlace local; E19: `napkin.md`, `agents/claudio.md`, `local/`, `skills/caveman`, `skills/reporte-sesiones` (igual a la línea base de la spec).

8. **[2026-11-21 18:57] Validate aprobado** (rol reviewer en el chat, claude-opus-5-5; vuelta 1 de 2). `tester` (claude-haiku-5-5) corrió CP-01, CP-02, CP-03, CP-09 y CP-10: todo `obtenido` e igual a lo esperado. CP-04 a CP-08 los corrió la sesión y los juzgó el reviewer. Lint y secretos (modo «solo secretos»: solo prosa, sin `tools`, `mode` ni scripts) sin hallazgos en los 17 archivos del alcance. Ajeno o no atribuible: `MEMORY.md` (venía de antes). Opcional, no bloquea: (a) en un proyecto con Cursor u OpenCode declarados, el chequeo de arranque puede necesitar la definición de adaptador de «Carpeta de agentes por archivo», que su «Qué lee» no nombra; (b) el paso 10 de la raíz no tiene la lectura de `## Presentación` que sí tiene `template/` (viene de la 0014); (c) dos `implementer` en paralelo pisaron las casillas de `04_tasks.md` (F2.2 desmarcó F2.3 y la sesión lo corrigió).
9. **[2026-11-21 18:57] Sin flujo:** `06_flujo.md` no aplica (decisión de la persona: «no requiere flujo por lo que sigues con el cierre»). El feature cambia el kit y no hay una operación que describir.

10. **[2026-11-21 18:58] Cierre: `Cerrado`, con los commits en la misma corrida.** Previa: 14 de 14 pasos `[x]`, sin verificaciones manuales pendientes, sin flujo (decisión 9), sin pausa, validación del conjunto aprobada (decisión 8). Criterio 1: fila `cierre` y plan contra real escritos. Criterio 3: sin ticket ni aviso (`Ticket: sin ticket`). Criterio 4: la columna de pendiente solo enlaza la deuda 0014, con visto bueno. Criterio 5: ninguna deuda `En atención` con Atendida por de esta spec. Criterio 2: se decide con la confirmación de los commits. Relacionado fuera del kit: issue #7 de GitHub («AGENTS.md pesa 13 KB»); el agente no lo cierra.

11. **[2026-11-21 18:59] Deuda y memoria:** la persona aprobó dar de alta el opcional (a) como deuda 0014 (`Abierta`, origen esta spec; «solo regístrala») y no aprobó la entrada propuesta para `MEMORY.md`. Commits: confirmados los 3 por la persona; la deuda entra al commit de specs.

## Pruebas
Corridas de CP-01 a CP-10 (antes y después), del `tester` y `/sdd-estructura`: [`07_casos_prueba.md` › Corridas](07_casos_prueba.md#corridas). Verificaciones manuales de UI: No aplica (kit de Markdown).

**Medición final (RF-14, `wc -c`, método de la spec):**

| Pieza | Línea base congelada | Antes (8c9681b) | Después | Frente a la congelada |
|---|---|---|---|---|
| Lectura del arranque, raíz, escenario base | 24 186 | 29 782 | 12 110 | −49,9 % (meta ≥ 35 %, ≤ 15 721) |
| `AGENTS.md` raíz | 13 314 | 14 279 | 12 267 | −7,9 % (meta ≥ 5 %, ≤ 12 648) |
| `template/AGENTS.md` | 11 859 | 12 990 | 11 061 | −6,7 % (meta ≥ 5 %, ≤ 11 266) |

Desglose del escenario base: napkin 1 579 + `MEMORY.md` 3 779 + constitución 1 133 + `formato-respuesta` 2 480 + «Chequeo de arranque» 3 139. Referencia fuera de la meta (proyecto con `claude` declarado): + «Estados de enlace» y «Estado «visto»» 2 322 + encabezado y filas de claude 385 = 14 817 (antes 29 782 con el mismo `MEMORY.md`). Frente a la medición de antes, `AGENTS.md` baja −14,1 % (raíz) y −14,8 % (`template/`). Tokens de la primera llamada: referencia pendiente para una sesión nueva.

## Tiempos
| Paso | Inicio | Fin | Minutos | Pausa | Nota |
|---|---|---|---|---|---|
| spec | 2026-11-21 10:36 | 2026-11-21 10:40 | 4 | 0 | Borrador de `01_spec.md` por el planner; 5 preguntas abiertas |
| clarify | 2026-11-21 10:51 | 2026-11-21 11:00 | 9 | 0 | pasada 1 de 3; rol reviewer y ajuste rol planner en el chat; 10 hallazgos corregidos; spec aprobada |
| plan | 2026-11-21 16:05 | 2026-11-21 16:08 | 3 | 0 | rol planner en el chat; claude-opus-5-5; `02_analisis_impacto.md` y `03_plan.md`; 1 ADR propuesto sin visto bueno |
| tasks | 2026-11-21 16:12 | 2026-11-21 16:14 | 2 | 0 | rol planner en el chat; claude-opus-5-5; ADR 0032 registrado; `04_tasks.md` y `07_casos_prueba.md`; aprobado 16:17 |
| F0.1 | 2026-11-21 16:17 | 2026-11-21 16:18 | 1 | 0 | sesión; medición y `/sdd-estructura` base |
| F0.2 | 2026-11-21 16:18 | 2026-11-21 16:19 | 1 | 0 | sesión; CP-04 a CP-07 «antes»; fixtures en el scratchpad (`montar.sh`, `chequeo.py`) |
| F1.1 | 2026-11-21 16:20 | 2026-11-21 16:21 | 1 | 0 | implementer en subagente; claude-sonnet-5-5; `sync-agents.md` reordenado (15 779 bytes; «Chequeo de arranque» 3 139); la sesión corrigió «esta definición» → «la definición de «Carpeta de agentes por archivo»» en la frase movida |
| F1.2 | 2026-11-21 16:25 | 2026-11-21 16:25 | 1 | 0 | implementer en subagente; claude-sonnet-5-5; `sdd-enlaces.md`: «estados» → «Estados de enlace» (1 línea) |
| F2.1 | 2026-11-21 16:26 | 2026-11-21 16:26 | 1 | 0 | implementer en subagente; claude-sonnet-5-5; `.agents/README.md`: «Qué va en `AGENTS.md`» (659 bytes) y «Roles y comandos»; 5 475 → 7 231 bytes |
| F2.2 | 2026-11-21 18:05 | 2026-11-21 18:06 | 1 | 0 | implementer en subagente, en paralelo con F2.3; claude-sonnet-5-5; `template/AGENTS.md` 12 990 → 11 061 bytes (meta ≤ 11 266); el subagente desmarcó F2.3 en `04_tasks.md` por error y la sesión lo volvió a marcar |
| F2.3 | 2026-11-21 18:05 | 2026-11-21 18:06 | 1 | 0 | implementer en subagente, en paralelo con F2.2; claude-sonnet-5-5; E8, E9 y E17 apuntan a `.agents/README.md` |
| F2.4 | 2026-11-21 18:06 | 2026-11-21 18:07 | 1 | 0 | implementer en subagente; claude-sonnet-5-5; `AGENTS.md` raíz 14 279 → 12 267 bytes (meta ≤ 12 648); S y pasos 4–8 iguales a `template/` verificados por la sesión |
| F2.5 | 2026-11-21 18:07 | 2026-11-21 18:07 | 1 | 0 | sesión; `cp` de `sync-agents.md`, `README.md`, `validar-estructura.md` y `commands/sdd-enlaces.md`; `diff -rq` igual a la línea base |
| F3.1 | 2026-11-21 18:10 | 2026-11-21 18:11 | 1 | 0 | implementer en subagente, en paralelo con F3.2; claude-sonnet-5-5; `ACTUALIZAR.md` › «Grupo B»: pasos 4, 7 y 8 a mano desde 0.12.0 |
| F3.2 | 2026-11-21 18:10 | 2026-11-21 18:11 | 1 | 0 | implementer en subagente; claude-sonnet-5-5; `VersionAgentKitSdd` 0.12.0 y entrada del changelog |
| F3.3 | 2026-11-21 18:10 | 2026-11-21 18:11 | 1 | 0 | sesión; `manifiestos/0.12.0.md`: 49 filas; cambian solo las 5 huellas de los archivos tocados |
| F4.1 | 2026-11-21 18:10 | 2026-11-21 18:11 | 1 | 0 | sesión; CP-01 a CP-10 y `/sdd-estructura` en el clon y en el ensayo de instalación; CP-01 a CP-03 y CP-09 corrieron mientras los subagentes de F3 trabajaban (archivos disjuntos) |
| F4.2 | 2026-11-21 18:11 | 2026-11-21 18:12 | 1 | 0 | sesión; medición final registrada |
| validate | 2026-11-21 18:56 | 2026-11-21 18:57 | 1 | 0 | `tester` en subagente (claude-haiku-5-5) y rol reviewer en el chat (claude-opus-5-5); vuelta 1: APROBADO; sin flujo (decisión 9) |
| cierre | 2026-11-21 18:57 | 2026-11-21 18:58 | 1 | 0 | la sesión (claude-opus-5-5); previa OK; criterios 1, 3, 4 y 5 cumplen; `Cerrado` y `cerrada`; criterio 2 con los commits de esta corrida confirmados por la persona; deuda 0014 dada de alta |

## Plan contra real

El plan estimó en minutos; el real es la suma de la columna Minutos (pasos en paralelo cuentan cada uno). La fila `cierre` no entra.

| Fase | Estimado (min) | Real (min) | Delta |
|---|---|---|---|
| Documentación SDD (spec, clarify, plan, tasks) | sin estimado | 18 | — |
| F0 Línea base | 20 | 2 | -18 |
| F1 Chequeo de arranque | 35 | 2 | -33 |
| F2 Archivo siempre cargado | 80 | 5 | -75 |
| F3 Actualización y versión | 30 | 3 | -27 |
| F4 Verificación | 30 | 2 | -28 |
| validate | sin estimado | 1 | — |
| **Implementación (F0–F4)** | **195** | **14** | **-181** |

Qué ajustar en el siguiente plan: F1 a F4 se desviaron fuerte por debajo. Un paso de 1 a 2 archivos con texto exacto en las tareas tarda 1 a 2 min con `implementer` en subagente; estimar por paso y no por fase, y sumar aparte la revisión de la sesión.
