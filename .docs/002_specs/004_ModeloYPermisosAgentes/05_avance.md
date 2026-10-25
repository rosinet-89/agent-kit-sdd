# 05 — Avance: Modelo por rol y permisos del frontmatter de los agentes
Leyenda: `[ ]` Pendiente · `[~]` En curso · `[x]` Hecho · `[!]` Bloqueado

## PUNTO DE RETOMO — leer esto primero
| Dato | Contenido |
|---|---|
| Última sesión | 2026-10-25 16:13 |
| Fase actual | Cerrada |
| Último paso cerrado | `cierre` 2026-10-25 16:13; sin `06_flujo` (decisión 36) |
| Siguiente paso | Ninguno en esta carpeta. Commit si el usuario lo pide. |
| Bloqueos abiertos | Ninguno |
| Coordinación | Corlos cerró `003_OptimizarArnes` (verificado 2026-10-25 15:05: Fase actual Cerrada, cierre 2026-10-25 14:23, archivos `01`–`06`). No tocar esa carpeta. Esta spec cambia el frontmatter de los 4 agentes de `template/.agents/agents/`; partir de su contenido actual (la `003` ya reescribió sus cuerpos). Cualquier cambio a `template/` obliga a regenerar `manifiestos/0.5.0.md` y repetir E20 al cerrar. |
| Estado BD | No aplica |
| Código | F2 a F7 en `template/`, los dos `AGENTS.md`, `CHANGELOG.md` y `manifiestos/0.5.0.md` (42 filas, E20 en 0). Deuda 003 resuelta. Deuda 006 abierta. ADR 011, 012 y 013 aceptados. Refresco de `.agents/` propuesto, no hecho. |

## Pasos
- [x] F0.1 Comprobar la precondición y el árbol
- [x] F0.2 Medir y copiar la línea base
- [x] F0.3 Escribir las 5 peticiones antes de lanzarlas
- [x] F1.1 Armar las copias desechables
- [x] F1.2 Corrida con el modelo actual
- [x] F1.3 Corrida con `sonnet` y umbral
- [x] F1.4 Anotar el modelo aceptado y borrar las copias
- [x] F2.1 Crear `restricciones-de-rol.md`
- [x] F2.2 Una frase en `SKILL.md`
- [x] F3.1 `coordinator.md`
- [x] F3.2 `planner.md`
- [x] F3.3 `implementer.md`
- [x] F3.4 `reviewer.md`
- [x] F4.1 Paso 5 de `sdd-validate.md`
- [x] F4.2 «Qué lee cada rol» en `template/AGENTS.md`
- [x] F4.3 La misma sección en el `AGENTS.md` de la raíz
- [x] F5.1 Aplicar la skill y anotar el reporte
- [x] F5.2 Corregir una vez, o cerrar si no hubo hallazgo
- [x] F6.1 Un ítem en `## 0.5.0 — 2026-10-24`
- [x] F7.1 Regenerar `manifiestos/0.5.0.md`
- [x] F7.2 E20 pasa
- [x] F7.3 Contraste contra `manifiestos/0.4.0.md`
- [x] F7.4 Chequeos de Comandos en cada archivo tocado
- [x] F7.5 Ensayo de instalación
- [x] F7.6 Medir RF-13 y RF-14 sobre el contenido final
- [x] F7.7 Proponer el refresco de la copia local

## Decisiones
1. **[2026-10-25 14:36] Se atiende la deuda `003_ModeloYPermisosDeAgentes` por el flujo SDD, como esta spec.** Origen: petición del usuario (\"procede con la deuda 3\"), una vez implementada la `003`. Alternativa descartada: editar el frontmatter directo sin spec.
2. **[2026-10-25 14:36] Verificación previa (paso 1 de la deuda), contra la documentación de subagentes de Claude Code (https://code.claude.com/docs/en/sub-agents), leída el 2026-10-25 14:36.** Hallazgos: (a) `mode` y `permissions` NO están entre los campos soportados y se ignoran sin aviso; (b) sí existen `tools` (lista permitida), `disallowedTools`, `model` (`sonnet`, `opus`, `haiku`, `fable`, ID completo o `inherit`), `permissionMode`, `maxTurns`, `skills`, `hooks`, `memory`, `omitClaudeMd`, `effort`, `color`; (c) por defecto el subagente carga la jerarquía de CLAUDE.md y los AGENTS.md cargados como instrucciones del proyecto, salvo con `omitClaudeMd: true`; (d) las reglas de permisos por patrón viven en `permissions.allow/deny` de los archivos de settings y valen para toda la sesión, no por subagente. Limitación: el resumen de la página cubrió los primeros 100.000 de 116.349 caracteres.
3. **[2026-10-25 14:42] Lectura completa de la documentación de subagentes** (los 16.349 caracteres restantes). Hechos nuevos: existen `hooks` por subagente (`PreToolUse` sobre `Bash`, exit code 2 bloquea) pero el ejemplo usa un script externo; el ejemplo de revisor de solo lectura de la propia documentación usa `tools` sin `Edit`/`Write`; las `description` de subagentes comparten un presupuesto de 15.000 tokens.
4. **[2026-10-25 14:42] Mecanismo de restricciones:** `tools`/`disallowedTools` donde alcancen, instrucción marcada "no garantizada" para el resto y una regla `deny` opcional en la configuración local. Sin hooks: no se enmienda el ADR 004. Alternativas descartadas: hooks por subagente (código ejecutable) y solo instrucción.
5. **[2026-10-25 14:42] Modelo por rol:** `implementer` y `coordinator` en un modelo más barato; `planner` y `reviewer` en `inherit`; cada cambio se acepta solo con la comparación en los 3 pasos de referencia de la `003`. Alternativas descartadas: solo el coordinator y ningún rol.
6. **[2026-10-25 14:42] `omitClaudeMd: true` en `planner`, `reviewer` y `coordinator`;** no en `implementer` (depende de las reglas de Git y las reglas duras). Alternativas descartadas: ningún rol y los cuatro.
7. **[2026-10-25 14:42] `mode` y `permissions` se eliminan de los 4 agentes** y se registra que son formato de otra herramienta. Alternativas descartadas: conservarlos documentados y mantener dos versiones de cada agente.
8. **[2026-10-25 14:42] El reviewer pierde `Write`;** la sesión principal escribe `07_pruebas_qa.md` y `tests/` con lo que él devuelve. Alternativa descartada: conservarlo como no garantizado.
9. **[2026-10-25 14:42] Versión:** dentro de 0.5.0 sin publicar, con una línea más en su changelog y manifiesto regenerado al cerrar. Alternativa descartada: 0.5.1.
10. **[2026-10-25 14:48] Clarify: 18 hallazgos del reviewer (5 ALTA); veredicto CAMBIOS NECESARIOS.**
11. **[2026-10-25 14:48] Hechos verificados en la documentación:** `Agent(lista)` en `tools` solo restringe a un agente que corre como hilo principal con `claude --agent`; en un subagente se ignora. `disallowedTools` no acepta patrones (una entrada `Bash(git push *)` quita Bash entera); bloquear `git commit/push/fetch` exige `permissions.deny` en settings, que vale para toda la sesión. Orden del modelo: parámetro de la llamada, frontmatter, variable de entorno, modelo de la sesión; un alias bloqueado cae al heredado con aviso.
12. **[2026-10-25 14:48] Modelo: `sonnet` para `implementer` y `coordinator`;** `planner` y `reviewer` en `inherit`. Alternativas descartadas: `haiku` para el coordinator (más riesgo de enrutar mal y saltarse puertas) y solo declarar `inherit`.
13. **[2026-10-25 14:48] Orden de cierre: la `003` cierra primero** (Corlos) y esta spec parte de ese estado y regenera el manifiesto al final. Alternativa descartada: cerrar esta primero.
14. **[2026-10-25 14:48] Evidencia formal sin `Write` en el reviewer:** la sesión escribe `07_pruebas_qa.md` y el harness; el reviewer lo ejecuta en una segunda invocación (cuenta como una vuelta de las 2). Alternativa descartada: retirar el harness del flujo.
15. **[2026-10-25 14:58] Spec aprobada por el usuario** (`Estado: aprobada`; 21 RF, 7 RNF, sin dudas abiertas), con las decisiones del planner: umbral de costo del 125 % de los bytes del modelo actual con una repetición por caso fallido; `/sdd-init` fuera del alcance; fila "sin red" garantizada por `tools`; pieza única de restricciones en la skill `sdd` (grupo A); RF-21 para la actualización parcial reviewer/`sdd-validate`. Corrección menor: RF-19 llamaba "no versionada" a toda la raíz; el `AGENTS.md` de la raíz sí está versionado.
16. **[2026-10-25 14:58] No se abre `/sdd-plan` todavía:** la `003` debe cerrar primero (decisión 13) y el usuario no pidió el plan.
17. **[2026-10-25 15:05] Se levanta la espera de la decisión 16.** La carpeta `003_OptimizarArnes` tiene Fase actual Cerrada (cierre 2026-10-25 14:23) y archivos `01`–`06`; el índice la marca Implementado. El párrafo Traspaso de ese avance sigue diciendo que `02`–`06` no existen; mandan la fila de PUNTO DE RETOMO y los archivos. No se editó la `003`. El usuario pidió cargar esta spec y el siguiente comando es `/sdd-plan`. Alternativa descartada: seguir esperando el cierre.
18. **[2026-10-25 15:25] El plan quedó escrito** (`02_analisis_impacto.md` y `03_plan.md`). Los ADR 010 (`RestriccionesDeRolSinHooks`), 011 (`OmitClaudeMdEnTresRoles`) y 012 (`ModeloExplicitoPorRol`) se proponen al cierre y no se crean. Alternativa descartada: crearlos en esta fase.
19. **[2026-10-25 15:33] Tareas entregadas** (`04_tasks.md`: F0–F7, 26 pasos, 205 min, 21 RF). Pendiente la aprobación del usuario de `03_plan.md` y `04_tasks.md` juntos. La fila `tasks` de tiempos se registra después de esa aprobación. Alternativa descartada: empezar `/sdd-implement` sin aprobación.
20. **[2026-10-25 15:34] El usuario aprobó `03_plan.md` y `04_tasks.md`.** Sus palabras: "yes", en respuesta a «¿Apruebas el plan y las tareas?». Alternativa descartada: seguir sin aprobación.
21. **[2026-10-25 15:36] F0.1: precondición y árbol.** (1) Índice `.docs/002_specs/README.md` fila 003: Implementado (2026-10-25). (2) `.docs/002_specs/003_OptimizarArnes/05_avance.md`: Fase actual Cerrada; `git status` de esa carpeta no mostró archivos de la 003; esta sesión no la editó. (3) `CLAUDE_CODE_SUBAGENT_MODEL` y `CLAUDE_CODE_SUBAGENT_MODEL_FORCE` están unset; F1 no queda bloqueada. (4) `git status --short` de `03_plan.md` §2: el único modificado es `.docs/002_specs/004_ModeloYPermisosAgentes/05_avance.md` (esta sesión, en plan y tareas); no es un cambio ajeno; no se revirtió nada; el resto de la lista está limpio; `template/.agents/skills/sdd/restricciones-de-rol.md` aún no existe. Fuera de la lista, `.docs/002_specs/README.md` está modificado y no se toca en este paso. Alternativa descartada: revertir el avance o el índice.
22. **[2026-10-25 15:38] F0.2: línea base copiada en este avance, con fecha 2026-10-25.** Coincide con `02` §3, `SKILL.md` y `sdd-validate.md` incluidos. `git status` de `template/`, `CHANGELOG.md`, `manifiestos/0.5.0.md` y el `AGENTS.md` de la raíz salió vacío. El neto hipotético del implementer es 1 560 bytes y `omitClaudeMd` no se declara. Alternativa descartada: reescribir el `02` o declarar el campo.
23. **[2026-10-25 15:38] F0.3: las 5 peticiones quedaron escritas antes de cualquier corrida.** La petición 1 no coincide con las carpetas del índice 001 a 006 (`ActualizarKit`, `Kit050`, `OptimizarArnes`, `ModeloYPermisosAgentes`, `ConvivenciaNumeracion`, `RutaSiNoDelega`). No hubo corrida de F1. Alternativa descartada: lanzar F1 antes de anotar la lista.
24. **[2026-10-25 15:42] F1.1: dos temporales fuera del repo, ya borrados.** `/tmp/tmp.SYaecUFpm2` (casos 1 a 4; `grep` de `SDD-INIT-PENDIENTE` dio 0) y `/tmp/tmp.75qLf9AVG8` (caso 5, `template/AGENTS.md` intacto, comentario de la línea 3). El caso 3 apuntaba a `007_FeatureSinTareas` (solo `01_spec.md`, sin `04_tasks.md`). Nada se copió al repo. `git status` de `template/` no mostró esta fase. Alternativa descartada: usar el `AGENTS.md` de la raíz.
25. **[2026-10-25 15:42] F1.2: modelo actual `Grok 4.7`.** Las cinco corridas cumplieron la respuesta esperada de F0.3. Reintentos: 0. No se escribió en el repo. `CLAUDE_CODE_SUBAGENT_MODEL` y `CLAUDE_CODE_SUBAGENT_MODEL_FORCE` seguían unset. Alternativa descartada: anotar un pase de `sonnet` en esta corrida.
26. **[2026-10-25 15:42] F1.3: no hay corrida válida de `sonnet`.** El alias no está entre los modelos que esta sesión puede lanzar y no se sustituyó por otro slug. No se anotó un pase. No hubo repetición. Una corrida inexistente no cuenta como caída al heredado ni como pase. El coordinator no pasa a `sonnet`. Alternativa descartada: contar la corrida de `Grok 4.7` como si fuera `sonnet`.
27. **[2026-10-25 15:42] F1.4: F3 escribirá `model: inherit` en el coordinator.** El implementer no entró en las corridas: declara `model: inherit`; no hay árbol previo a F1.5 de `001_ActualizarKit`; no se fabrica un fixture (`02` §3.2 y §6). `omitClaudeMd` no se le pone. Los temporales ya no existen. Alternativa descartada: declarar `sonnet` sin corrida válida.
28. **[2026-10-25 15:48] F2.1: creada `template/.agents/skills/sdd/restricciones-de-rol.md`.** Sin frontmatter. Ocho filas de RF-7. Filas 2 y 3 sin deny de ruta. Deny de Git con los cinco patrones. El allow de RF-10 no es fila de la tabla. Lint utf-8, sin BOM. Alternativa descartada: copiar la tabla en cada agente.
29. **[2026-10-25 15:48] F2.2: una frase en `template/.agents/skills/sdd/SKILL.md`,** junto al enlace de `avance-y-tiempos.md` en «Carpeta del feature». `name` sigue `sdd`. El `model` del coordinator que F3 escribirá sigue siendo `inherit` (F1 no pasó a `sonnet`). Alternativa descartada: copiar la tabla dentro de la skill.
30. **[2026-10-25 15:54] F3.1: `coordinator.md` con `model: inherit` y `omitClaudeMd: true`.** Se quitaron `mode` y `permissions`. `description`: 267 bytes. Alternativa descartada: `sonnet` (F1 no pasó).
31. **[2026-10-25 15:54] F3.2: `planner.md` con `model: inherit` y `omitClaudeMd: true`.** Se quitaron `mode` y `permissions`. `description`: 191 bytes. Alternativa descartada: dejar `mode` y `permissions`.
32. **[2026-10-25 15:54] F3.3: `implementer.md` con `model: inherit`.** No tiene `omitClaudeMd`. Se quitaron `mode` y `permissions`. `description`: 201 bytes. Alternativa descartada: ponerle `omitClaudeMd`.
33. **[2026-10-25 15:54] F3.4: `reviewer.md` con `model: inherit` y `omitClaudeMd: true`.** Sin `Write` ni `Edit`. Se quitaron `mode` y `permissions`. `description`: 228 bytes. Alternativa descartada: conservar `Write`.
34. **[2026-10-25 16:01] F4 a F7 cerradas.** F4: el paso 5 de `sdd-validate.md` queda en que la sesión escribe la evidencia, el reviewer la ejecuta en lectura y esa invocación cuenta como vuelta (RF-21 a y b); «Qué lee cada rol» quedó igual en `template/AGENTS.md` y en el `AGENTS.md` de la raíz; la fila del implementer no cambió; no se tocó «Formato y numeración». F5: modo completo de `seguridad-basica` (secciones 1 y 2) sobre 9 archivos de F2, F3 y F4; `git diff` sin stage y el archivo nuevo sin coincidencias del patrón de secretos; `tools` no se amplió; el reviewer perdió `Write` y no recibió `Edit`; no se usó `permissionMode`; el `deny` es texto en la pieza y no está en `.claude/`; reporte `seguridad-basica: sin hallazgos en 9 archivos`; F5.2 no aplica; no se hizo `git add`. F6: un ítem nuevo al final de `## 0.5.0 — 2026-10-24`; encabezado, fecha y `VERSION` iguales; los ítems previos se quedaron. F7: `template/` tiene 42 archivos; manifiesto regenerado (42 filas, huellas de 64 hex minúsculas, ninguna ruta con prefijo `template/`); E20 con 0 faltantes, 0 sobrantes y 0 distintas; `ACTUALIZAR.md` y `validar-estructura.md` no se modificaron; contraste 0.4.0 → 0.5.0: altas 7, bajas 0, huellas distintas 25; lint utf-8 sin BOM; ensayo en `mktemp` con 42 archivos y la segunda copia sin cambiar huellas; el temporal se borró. Medición y RF-14, más abajo. F7.7 propone refrescar `.agents/` desde `template/.agents/` conservando `napkin.md` y no se ejecutó. Alternativa descartada: refrescar la copia local en este paso.
35. **[2026-10-25 16:10] Validate: APROBADO.** El reviewer contrastó el «Hecho cuando» y los RF citados contra `template/`. RF-1 a RF-21 pasan. Lint utf-8 sin BOM. `seguridad-basica: sin hallazgos en 15 archivos` (modo completo). No hay interfaz. No se escribió `07_pruebas_qa.md`. El manifiesto regenerado también actualiza huellas de archivos que esta spec no editó (`sdd-change.md`, `sdd-clarify.md`, `sdd-flujo.md`, `sdd-implement.md`, `sdd-plan.md`, `sdd-spec.md`, `sdd-tasks.md`, `sdd.md`, `avance-y-tiempos.md`); E20 sigue en 0 contra el árbol actual. Ajenos, sin revisar: índice de specs, carpeta `005`, deudas `004` y `005` y su README. Alternativa descartada: pedir cambios por la copia local desfasada; el refresco sigue propuesto y no bloquea.
36. **[2026-10-25 16:13] Cierre sin `06_flujo`.** El usuario dijo que no se requiere flujo y que se proceda con el cierre. No hay `06_flujo.md`. Deuda `003_ModeloYPermisosDeAgentes` marcada `Resuelta` (Atendida por y Cómo se atendió). El índice local de ADR acaba en 010 (`RutaSiNoHayUnSoloPaso`, origen `006`); `git ls-tree` de `origin/main` no listó `.docs/004_adr/`. Los ADR que el plan numeraba 010, 011 y 012 se proponen como 011 `RestriccionesDeRolSinHooks`, 012 `OmitClaudeMdEnTresRoles` y 013 `ModeloExplicitoPorRol` (`inherit`, no `sonnet`) y no se crean. Se proponen, sin escribirlos, la deuda de que `/sdd-init` sugiera los fragmentos `allow` y `deny`, y la línea de `MEMORY.md` de que no hay revisión en la que F1.4 de `001_ActualizarKit` ya esté y F1.5 todavía no. Alternativa descartada: crear ADR, deuda o memoria sin visto bueno.
37. **[2026-10-25 16:13] El usuario acepta crear los tres registros.** ADR [011](../../004_adr/011_RestriccionesDeRolSinHooks.md), [012](../../004_adr/012_OmitClaudeMdEnTresRoles.md) y [013](../../004_adr/013_ModeloExplicitoPorRol.md), estado Aceptado. Deuda `006_SddInitSugiereAllowDeny` abierta. Línea escrita en `MEMORY.md`. El 010 no se reutilizó. Alternativa descartada: conservar los números 010–012 del plan.

## Línea base (F0.2, 2026-10-25)
Medición con `wc -c` sobre el árbol de trabajo. Coincide con `02` §3. El `02` no se reescribió. Heredado = `template/CLAUDE.md` 11 + `template/AGENTS.md` 8 403 = 8 414 bytes. Arranque = bytes del agente + heredado.

| Rol | Agente | Arranque | description |
|---|---:|---:|---:|
| coordinator | 4 859 | 13 273 | 267 |
| planner | 2 428 | 10 842 | 191 |
| implementer | 1 560 | 9 974 | 201 |
| reviewer | 2 484 | 10 898 | 228 |

Suma de `description`: 887. `template/.agents/skills/sdd/SKILL.md`: 8 251. `template/.agents/commands/sdd-validate.md`: 2 305. Archivos en `template/`: 41. Filas de datos de `manifiestos/0.5.0.md`: 41. Neto hipotético de `omitClaudeMd` en el implementer = 9 974 − 8 414 = 1 560. El campo no se pone. `git status` de `template/`, `CHANGELOG.md`, `manifiestos/0.5.0.md` y el `AGENTS.md` de la raíz: vacío. Este paso no los cambió.

## Peticiones de F1 (F0.3, 2026-10-25 15:38)
Escritas antes de cualquier corrida. No hubo corrida de F1. La petición 1 no coincide con las carpetas del índice 001 a 006: `001_ActualizarKit`, `002_Kit050`, `003_OptimizarArnes`, `004_ModeloYPermisosAgentes`, `005_ConvivenciaNumeracion`, `006_RutaSiNoDelega`.

1. Petición libre de análisis nuevo. Respuesta esperada: elige `sdd-spec` y no implementa.
2. Spec aprobada que pide el plan. Respuesta esperada: elige `sdd-plan` y no pasa a implementar.
3. Pide implementar un feature cuya copia no tiene `04_tasks.md` aprobado. Respuesta esperada: frena en la puerta. No escribe código. Se anota el comando que elige.
4. Pide validar un feature con la implementación terminada (`003_OptimizarArnes`, mientras siga cerrada). Respuesta esperada: elige `sdd-validate` y no abre una implementación.
5. El `AGENTS.md` de la copia es el de `template/` intacto (lleva el comentario `SDD-INIT-PENDIENTE`). Pide una spec. Respuesta esperada: para, sugiere `/sdd-init` y no ejecuta ningún `/sdd*`.

## Corridas de F1 (2026-10-25 15:42)
`model` que F3 escribirá en el coordinator: `inherit`. El implementer declara `model: inherit` y no lleva `omitClaudeMd`.

F1.1. Temporales fuera del repo, ya borrados: `/tmp/tmp.SYaecUFpm2` (casos 1 a 4) y `/tmp/tmp.75qLf9AVG8` (caso 5). El `AGENTS.md` de 1 a 4 no contenía `SDD-INIT-PENDIENTE` (grep 0). El del caso 5 es `template/AGENTS.md` intacto y conserva el comentario de la línea 3. El caso 3 apuntaba a `007_FeatureSinTareas` (solo `01_spec.md`, sin `04_tasks.md`). Nada se copió al repo. `git status` de `template/` no mostró esta fase.

F1.2. Modelo actual, nombre exacto: `Grok 4.7`. Reintentos: 0 en los cinco. No se escribió en el repo. `CLAUDE_CODE_SUBAGENT_MODEL` y `CLAUDE_CODE_SUBAGENT_MODEL_FORCE` seguían unset. Los cinco cumplieron la respuesta esperada de F0.3.

| Caso | Comando | Puerta | Bytes que la instrucción mandó leer | Bytes de la respuesta | Reintentos |
|---|---|---|---:|---:|---:|
| 1 | `sdd-spec` | no implementa | 12 966 (AGENTS 8 107 + coordinator 4 859) | 279 | 0 |
| 2 | `sdd-plan` | no implementa | 18 763 (los anteriores + índice 5 797) | 270 | 0 |
| 3 | `sdd-implement` | frena, no escribe código | 18 859 (+ `01_spec` de 007, 96) | 247 | 0 |
| 4 | `sdd-validate` | no abre implementación | 18 877 (+ avance de 003 en la copia, 114) | 208 | 0 |
| 5 | ninguno | para, sugiere `/sdd-init`, no ejecuta `/sdd*` | 13 262 (AGENTS de template 8 403 + coordinator 4 859) | 83 | 0 |

F1.3. No hay corrida válida de `sonnet`. El alias no está entre los modelos que esta sesión puede lanzar (no se sustituyó por otro slug). No se anotó un pase. No hubo repetición. Una corrida inexistente no cuenta como caída al heredado ni como pase. El coordinator no pasa a `sonnet`.

F1.4. El implementer no entró en las corridas. Motivo (`02` §3.2 y §6): no hay árbol previo a F1.5 de `001_ActualizarKit` y no se fabrica un fixture. `omitClaudeMd` no se le pone. Los temporales ya no existen.

## Medición final (F7.6, 2026-10-25 16:01)
Sobre el contenido de `template/`, no la copia local. `template/AGENTS.md` mide 8 549. Heredado si se carga = `template/CLAUDE.md` 11 + 8 549 = 8 560. Con `omitClaudeMd: true` el heredado de ese rol es 0. La línea base de F0 sigue arriba (heredado de entonces 8 414, antes de editar `template/AGENTS.md`).

| Rol | Agente | omitClaudeMd | Arranque |
|---|---:|---|---:|
| coordinator | 4 656 | sí | 4 656 |
| planner | 2 331 | sí | 2 331 |
| implementer | 1 495 | no | 10 055 |
| reviewer | 2 268 | sí | 2 268 |

Suma de `description`: 887, igual que F0 (267, 191, 201, 228). Neto del implementer = 10 055 − 8 560 = 1 495. El campo sigue ausente. En los otros tres el campo no se revierte.

Contraste 0.4.0 → 0.5.0 de esta corrida: altas 7 (`.agents/commands/sdd-estructura.md`, `.agents/skills/adr/SKILL.md`, `.agents/skills/migracion-bd/SKILL.md`, `.agents/skills/sdd/restricciones-de-rol.md`, `.agents/skills/seguridad-basica/SKILL.md`, `.agents/validar-estructura.md`, `.docs/004_adr/README.md`), bajas 0, huellas distintas 25.

| Verificación (RF-14) | Pieza |
|---|---|
| Criterios de aceptación y «Hecho cuando» | `sdd-validate.md` y el reviewer |
| Chequeos de `AGENTS.md` › Comandos | `AGENTS.md` y el comando de validación |
| Revisión de seguridad con alcance de código | `seguridad-basica`, modo completo |
| Deuda, ADR e índices al cerrar | `sdd-cierre.md` (no se editó) |
| El implementer no registra en Git ni escribe `05_avance.md` | Cuerpo del implementer y fila 4 de la tabla |
| E1 a E20 los ejecuta la sesión principal | `validar-estructura.md` (no se editó) |
| Puertas de aprobación del coordinator | Cuerpo del coordinator y `sdd-implement.md` (no se editó) |
| Lectura mínima, lecturas permitidas y aviso de respaldo de la `003` | `sdd-implement.md` (no se editó) |

F7.7. `diff -rq template/.agents/agents .agents/agents` muestra los 4 agentes distintos. `SKILL.md` local difiere y `restricciones-de-rol.md` solo está en `template/`. Se propone refrescar `.agents/` desde `template/.agents/` conservando `napkin.md`. No se ejecutó. No bloquea el cierre de F7.

## Pruebas
Validate 2026-10-25 16:10. Veredicto: APROBADO. Lint utf-8 sin BOM en el alcance propio. Sin interfaz. `seguridad-basica: sin hallazgos en 15 archivos` (modo completo).

| RF | Resultado | Evidencia breve |
|---|---|---|
| RF-1 | pasa | Pieza con fecha, fuente y hechos (a)–(d) |
| RF-2 | pasa | `model`, `omitClaudeMd`, `mode` y `permissions` quedan no verificados en Cursor, Gemini y Copilot |
| RF-3 | pasa | Los cuatro declaran `model`; planner y reviewer en `inherit` |
| RF-4 | pasa | Cinco peticiones, corrida `Grok 4.7` 5/5; `sonnet` sin corrida válida; implementer en `inherit` |
| RF-5 | pasa | Alias ausente cae al heredado con aviso; respaldo `inherit` |
| RF-6 | pasa | Sin `mode` ni `permissions` |
| RF-7 | pasa | Ocho filas; filas 2 y 3 sin deny de ruta |
| RF-8 | pasa | Reviewer: `Read, Glob, Grep, Bash`; sin `permissionMode` |
| RF-9 | pasa | Deny de Git solo como texto; no está en `.claude/` |
| RF-10 | pasa | Allow genérico, no es fila y ningún agente lo aplica |
| RF-11 | pasa | `omitClaudeMd` en planner, reviewer y coordinator; implementer sin la clave; neto 1 495 |
| RF-12 | pasa | «Qué lee cada rol» igual en los dos `AGENTS.md` |
| RF-13 | pasa | Línea base y medición final coinciden con los bytes actuales |
| RF-14 | pasa | Cada verificación sigue en su pieza |
| RF-15 | pasa | Modo completo, sin hallazgos (F5 y esta pasada) |
| RF-16 | pasa | Ítem nuevo en `## 0.5.0 — 2026-10-24` |
| RF-17 | pasa | Quitar `mode` y `permissions` queda no verificado en otras herramientas |
| RF-18 | pasa | Manifiesto 42 filas; `VERSION` fuera del diff; E20 anotado en F7.2 |
| RF-19 | pasa | Mismo bloque 0.5.0; refresco propuesto, no ejecutado |
| RF-20 | pasa | Paso 5: la sesión escribe la evidencia; el reviewer no |
| RF-21 | pasa | Las dos combinaciones de reviewer y comando están cubiertas |

## Tiempos
| Paso | Inicio | Fin | Minutos | Pausa | Nota |
|---|---|---|---|---|---|
| spec | 2026-10-25 14:36 | 2026-10-25 14:43 | 7 | 0 | verificación de documentación, planner (20 RF, 7 RNF), 5 dudas respondidas y ajustes |
| clarify | 2026-10-25 14:43 | 2026-10-25 14:58 | 15 | 0 | reviewer (18 hallazgos), verificación de docs, 3 decisiones, ajustes del planner y aprobación |
| plan | 2026-10-25 15:02 | 2026-10-25 15:25 | 23 | 0 | 02 y 03 (F0–F7, 205 min); el primer planner se cortó con 02; el usuario confirmó a las 15:16 y el segundo cerró 03 |
| tasks | 2026-10-25 15:27 | 2026-10-25 15:34 | 7 | 0 | se entregaron 26 pasos (F0–F7, 205 min) y el usuario aprobó plan y tareas |
| F0.1 | 2026-10-25 15:36 | 2026-10-25 15:36 | 1 | 0 | precondición 003, variables unset y git status de §2; mismo minuto, mínimo 1 del contrato |
| F0.2 | 2026-10-25 15:38 | 2026-10-25 15:38 | 1 | 0 | línea base copiada; mismo minuto que F0.3, mínimo 1 del contrato avance-y-tiempos.md |
| F0.3 | 2026-10-25 15:38 | 2026-10-25 15:38 | 1 | 0 | 5 peticiones antes de corrida; mismo minuto que F0.2, mínimo 1 del contrato avance-y-tiempos.md |
| F1.1 | 2026-10-25 15:42 | 2026-10-25 15:42 | 1 | 0 | copias desechables ya borradas; mismo minuto que F1.2–F1.4, mínimo 1 del contrato avance-y-tiempos.md |
| F1.2 | 2026-10-25 15:42 | 2026-10-25 15:42 | 1 | 0 | cinco corridas con Grok 4.7; mismo minuto que F1.1, F1.3 y F1.4, mínimo 1 del contrato avance-y-tiempos.md |
| F1.3 | 2026-10-25 15:42 | 2026-10-25 15:42 | 1 | 0 | sonnet sin corrida válida; mismo minuto que F1.1, F1.2 y F1.4, mínimo 1 del contrato avance-y-tiempos.md |
| F1.4 | 2026-10-25 15:42 | 2026-10-25 15:42 | 1 | 0 | coordinator e implementer en inherit; mismo minuto que F1.1–F1.3, mínimo 1 del contrato avance-y-tiempos.md |
| F2.1 | 2026-10-25 15:48 | 2026-10-25 15:48 | 1 | 0 | pieza de restricciones; mismo minuto que F2.2, mínimo 1 del contrato avance-y-tiempos.md |
| F2.2 | 2026-10-25 15:48 | 2026-10-25 15:48 | 1 | 0 | frase en SKILL.md; mismo minuto que F2.1, mínimo 1 del contrato avance-y-tiempos.md |
| F3.1 | 2026-10-25 15:54 | 2026-10-25 15:54 | 1 | 0 | coordinator inherit; mismo minuto que F3.2–F3.4, mínimo 1 del contrato avance-y-tiempos.md |
| F3.2 | 2026-10-25 15:54 | 2026-10-25 15:54 | 1 | 0 | planner inherit; mismo minuto que F3.1, F3.3 y F3.4, mínimo 1 del contrato avance-y-tiempos.md |
| F3.3 | 2026-10-25 15:54 | 2026-10-25 15:54 | 1 | 0 | implementer inherit, sin omitClaudeMd; mismo minuto que F3.1, F3.2 y F3.4, mínimo 1 del contrato avance-y-tiempos.md |
| F3.4 | 2026-10-25 15:54 | 2026-10-25 15:54 | 1 | 0 | reviewer sin Write ni Edit; mismo minuto que F3.1–F3.3, mínimo 1 del contrato avance-y-tiempos.md |
| F4.1 | 2026-10-25 16:01 | 2026-10-25 16:01 | 1 | 0 | paso 5 de sdd-validate; mismo minuto que F4–F7, mínimo 1 del contrato avance-y-tiempos.md |
| F4.2 | 2026-10-25 16:01 | 2026-10-25 16:01 | 1 | 0 | Qué lee cada rol en template/AGENTS.md; mismo minuto que F4–F7, mínimo 1 del contrato avance-y-tiempos.md |
| F4.3 | 2026-10-25 16:01 | 2026-10-25 16:01 | 1 | 0 | la misma sección en AGENTS.md de la raíz; mismo minuto que F4–F7, mínimo 1 del contrato avance-y-tiempos.md |
| F5.1 | 2026-10-25 16:01 | 2026-10-25 16:01 | 1 | 0 | seguridad-basica sin hallazgos en 9 archivos; mismo minuto que F4–F7, mínimo 1 del contrato avance-y-tiempos.md |
| F5.2 | 2026-10-25 16:01 | 2026-10-25 16:01 | 1 | 0 | sin hallazgo, no hubo corrección; mismo minuto que F4–F7, mínimo 1 del contrato avance-y-tiempos.md |
| F6.1 | 2026-10-25 16:01 | 2026-10-25 16:01 | 1 | 0 | ítem en el bloque 0.5.0; mismo minuto que F4–F7, mínimo 1 del contrato avance-y-tiempos.md |
| F7.1 | 2026-10-25 16:01 | 2026-10-25 16:01 | 1 | 0 | manifiesto 0.5.0 regenerado, 42 filas; mismo minuto que F4–F7, mínimo 1 del contrato avance-y-tiempos.md |
| F7.2 | 2026-10-25 16:01 | 2026-10-25 16:01 | 1 | 0 | E20 en 0; mismo minuto que F4–F7, mínimo 1 del contrato avance-y-tiempos.md |
| F7.3 | 2026-10-25 16:01 | 2026-10-25 16:01 | 1 | 0 | contraste 0.4.0 a 0.5.0, altas 7; mismo minuto que F4–F7, mínimo 1 del contrato avance-y-tiempos.md |
| F7.4 | 2026-10-25 16:01 | 2026-10-25 16:01 | 1 | 0 | lint utf-8 sin BOM; mismo minuto que F4–F7, mínimo 1 del contrato avance-y-tiempos.md |
| F7.5 | 2026-10-25 16:01 | 2026-10-25 16:01 | 1 | 0 | ensayo mktemp, 42 archivos; mismo minuto que F4–F7, mínimo 1 del contrato avance-y-tiempos.md |
| F7.6 | 2026-10-25 16:01 | 2026-10-25 16:01 | 1 | 0 | medición final y RF-14; mismo minuto que F4–F7, mínimo 1 del contrato avance-y-tiempos.md |
| F7.7 | 2026-10-25 16:01 | 2026-10-25 16:01 | 1 | 0 | refresco propuesto, no ejecutado; mismo minuto que F4–F7, mínimo 1 del contrato avance-y-tiempos.md |
| validate | 2026-10-25 16:02 | 2026-10-25 16:10 | 8 | 0 | reviewer, RF-1 a RF-21 pasan; seguridad-basica sin hallazgos en 15 archivos |
| cierre | 2026-10-25 16:11 | 2026-10-25 16:13 | 2 | 0 | sin flujo, por decisión del usuario; deuda 003 resuelta |

## Plan contra real
| Fase | Estimado | Real | Delta |
|---|---|---|---|
| F0 | 15 min | 3 min | −12 min |
| F1 | 50 min | 4 min | −46 min |
| F2 | 30 min | 2 min | −28 min |
| F3 | 25 min | 4 min | −21 min |
| F4 | 20 min | 3 min | −17 min |
| F5 | 20 min | 2 min | −18 min |
| F6 | 10 min | 1 min | −9 min |
| F7 | 35 min | 7 min | −28 min |
| Documentación SDD | sin estimado | 62 min | — |

Documentación SDD = spec 7 + clarify 15 + plan 23 + tasks 7 + validate 8 + cierre 2 = 62 min. El plan estimó las fases en minutos. F1 son 4 filas y un minuto de reloj; F4 a F7 son 13 filas y un minuto de reloj. En el siguiente plan de este tipo, estimar cada fase en minutos de un dígito: el piso de 1 minuto repetido en el mismo minuto no es tiempo de reloj.
