# 05 — Avance: Enlaces de cada herramienta tras un pull
Leyenda: `[ ]` Pendiente · `[~]` En curso · `[x]` Hecho · `[!]` Bloqueado

## PUNTO DE RETOMO — leer esto primero
| Dato | Contenido |
|---|---|
| Última sesión | 2026-11-21 10:04 |
| Fase actual | `cierre`: `Cerrado` (2026-11-21 10:04) |
| Último paso cerrado | F0 a F7, `validate` del delta y `cierre` el 2026-11-03 21:37 |
| Siguiente paso | Ninguno |
| Bloqueos abiertos | Ninguno (ver riesgo aceptado y `06_flujo.md` en Decisiones 23 y 28) |
| Validación del conjunto | Aprobada: validate del 2026-11-02 17:03, APROBADO (decisión 21), y validate del delta F7 el 2026-11-03 21:37, APROBADO (decisión 28); la prueba manual de `/sdd-enlaces` la sustituyó el riesgo aceptado por la persona (decisión 23). Registrada en el punto de retomo el 2026-11-21 con el visto bueno de la persona |
| Aviso o ticket | Issue #2 cerrado en GitHub; lo confirmó la persona el 2026-11-21 |
| Coordinación | Otras sesiones trabajan en `0008` (issue #1) y `0010` (issue #3). No tocar sus carpetas. Si se edita un archivo de `template/` que otra pueda tocar, usar Edit mínimo y releer antes. |
| Estado BD | No aplica |
| Código | Hecho en el árbol, sin commitear: `sdd-enlaces.md`, `sync-agents.md`, `validar-estructura.md`, `sdd-estructura.md`, ambos `AGENTS.md`, README, SKILL, coordinator, `sdd.md`, `reviewer.md`, `sdd-init.md` (template y clon), `ACTUALIZAR.md`, `INSTALAR.md`, `VersionAgentKitSdd` 0.6.0, `ChangelogAgentKitSdd.md`, `manifiestos/0.6.0.md`, ADR 0021 y 0022 |

## Pasos
Espejo de `04_tasks.md` (53 pasos): todos `[x]` al cierre.
- [x] F0 (3 pasos) · [x] F1 (6) · [x] F2 (8) · [x] F3 (8) · [x] F4 (3) · [x] F5 (8) · [x] F6 (9) · [x] F7 (8, delta E1/E7)

## Decisiones
1. **[2026-10-31 19:26] Carpeta `0009_EnlacesTrasPull` asignada por el usuario** (4 dígitos). Origen: issue #2 de rosinet-89/agent-kit-sdd. Límite pedido: llegar hasta la puerta de aprobación de la spec; sin implementar, commit, push ni cerrar el issue.
2. **[2026-10-31 19:30] Antecedentes.** Coinciden `001_ActualizarKit` (actualización y enlaces tras actualizar, ACTUALIZAR.md › Enlaces) y `002_Kit050` (`/sdd-estructura`, E1–E3). Deuda abierta del módulo: ninguna (001–008 resueltas). ADR relevante: 004 (solo Markdown, sin scripts). `.agents/local/` ya está en `.gitignore`. Hay que coordinar con 0008 y 0010 en `AGENTS.md`/`template/AGENTS.md` (paso 7) y `sdd-estructura`.

3. **[2026-10-31 19:28] Puerta de aprobación pendiente.** No se aplicó ninguna corrección de clarify a la spec: espera decisión del usuario. No hay commit, push ni cierre del issue.

4. **[2026-10-31 20:39] Respuestas del usuario aplicadas a la spec.** Versión única 0.6.0 para 0008, 0009 y 0010. Orden de fusión: 0008, 0010, 0009. `/sdd-enlaces` como comando aparte; `/sdd-estructura` solo lectura. Sección «Qué hacer después de un pull» en `ACTUALIZAR.md`. Sin changelog, el aviso lista piezas nuevas y retiradas. Chequeo dentro del paso 7; si se pospone no se marca «visto». `.agents/local/` la crea el agente con confirmación.
5. **[2026-10-31 20:39] Clarify 2.ª pasada: CAMBIOS NECESARIOS.** Hallazgos nuevos sin cubrir: quién escribe «visto» (RF-5 frente a RF-11), «aceptar» no implica enlaces OK, confirmar la carpeta con todo OK repite la pregunta cada sesión, uno o dos avisos (RF-1 y oferta de enlaces), qué comando recomienda el aviso, `/sdd-init` y «visto», cambio de rama, enlaces colgantes, `ACTUALIZAR.md` ausente en el proyecto, `/sdd-enlaces` como pieza nueva. Sin aplicar: espera al usuario. La spec NO está aprobada.

6. **[2026-10-31 23:08] Segunda ronda del usuario aplicada.** Solo `/sdd-enlaces` escribe «visto» al terminar bien; el paso 7 solo lee y avisa; un solo mensaje combinado por sesión. Derivadas pendientes de confirmar: (a) éxito = re-verificación con todo OK; (b) `/sdd-enlaces` crea `.agents/local/` con confirmación; (c) 0.6.0 compartida, manifiesto y changelog una sola vez al final. El planner añadió RF-14 a RF-16, CF-8 a CF-10 y propuestas pendientes de confirmar.
7. **[2026-10-31 23:08] Clarify 3.ª pasada: 8 hallazgos bloqueantes y 6 menores.** Bloqueantes: sin RF para el disparador con `.kit-version` ausente o «visto» corrupto ni para enlaces faltantes con misma versión; «éxito» imposible con RF-7 (herramientas no declaradas quedan FALTA); RF-16 frente a repetición entre sesiones y OCUPADO permanente; `/sdd-enlaces` sin nada que corregir, ¿escribe «visto»?; una confirmación de lote frente a reemplazar y retirar enlaces; sin RF que exija `.agents/local/` ignorada por Git; pasos 7 y 8 de `sync-agents.md` frente a RF-11; riesgo de que el agente ejecute comandos. Sin aplicar. La spec NO está aprobada.

8. **[2026-10-31 23:26] Tercera ronda del usuario aplicada** por el planner (14 RF, 8 CF, sin dudas abiertas, sin marcas «pendiente de confirmar»; RF-15 eliminado porque `sync-agents.md` ya trae Copilot y Kiro fuera). Decisiones del usuario: todo OK sobre herramientas declaradas; OCUPADO intencional como excepción en «visto»; `/sdd-enlaces` sin nada que corregir escribe «visto»; una confirmación por tipo de acción; pasos 7 y 8 de `sync-agents.md` pasan a `/sdd-enlaces`; `.agents/local/` ignorada (`git check-ignore`); 0.6.0 compartida.
9. **[2026-10-31 23:26] Clarify final: sin bloqueantes, 10 menores** para el plan (RF-3 frente a la edición de `.gitignore`; copia frente a OCUPADO; OCUPADO nuevo sin disparador; pasos 1 a 6 de `sync-agents.md` sin tratar; regla de `.gitignore` de serie; herramientas declaradas vacías; trazabilidad de CF-8; repo sin Git; RF-7 y confirmaciones extra). La spec sigue en borrador: la aprueba el usuario.

10. **[2026-10-31 23:33] El usuario aprobó la spec.** `01_spec.md` pasa a `Estado: aprobada`. Se inicia la fase `plan` hasta su puerta de aprobación (sin `04_tasks`, sin implementar, sin commit ni push).

11. **[2026-10-31 23:40] Plan redactado y revisado.** Reviewer: 1 bloqueante (decisión «sin `.kit-version` => `version: ninguna`» contradice RF-1b) y 8 menores; el planner corrigió los menores. El bloqueante queda como decisión del usuario: (A, recomendada) cumplir RF-1b literal y omitir el chequeo si falta `.agents/.kit-version` y existe `template/`; (B) aprobar la excepción y registrarla con `/sdd-change`. Otras decisiones por aprobar: cero herramientas declaradas, perder la detección de réplicas al arrancar, editar `sdd-init.md` e `INSTALAR.md`, ADR-A y ADR-B (propuestos, no creados). Sin `04_tasks`, sin implementar, sin commit ni push.

12. **[2026-11-01 13:49] Respuestas del usuario al plan incorporadas** (opción A para `.kit-version` ausente; cero herramientas no es éxito; el arranque deja de detectar ignorados y réplicas; ediciones en `sdd-init.md` e `INSTALAR.md`; ADR-A y ADR-B al cierre con la skill `adr`; RF-1d ampliada se anota en la spec al cierre). Total 12,5 h. Revisión final del reviewer: APROBABLE, sin bloqueantes. Menores: sin `.kit-version` ni `template/`, `/sdd-enlaces` crea enlaces pero no escribe «visto» (decisión del planner, en tensión literal con RF-4, falta escenario E-11 y el paso 8 del comando); el usuario debe confirmarla. Sin `04_tasks`, sin implementar, sin commit ni push.

13. **[2026-11-01 13:56] El usuario aprobó el plan** (dijo «Procede»). Acepta el caso sin `.kit-version` ni `template/`: aviso «todo nuevo»; `/sdd-enlaces` no escribe «visto» e informa que falta el marcador. Se añaden al plan el paso 8 de `/sdd-enlaces`, el escenario E-11 y la nota en F6 junto a RF-1d. La hora de inicio de la fila «plan ajuste + revisión final» se corrigió a aprox. 13:40 (antes 00:00, que era un dato aproximado erróneo).

14. **[2026-11-01 13:59] Tareas redactadas y revisadas.** `04_tasks.md`: F0 a F6, 45 pasos, 12,5 h. Revisión: 2 bloqueantes (sección «Marcador» inexistente; grep de E20 que no podía dar vacío por `reviewer.md`) y 4 menores, corregidos por el planner. Añadidos al plan: paso 8 de `/sdd-enlaces` sin marcador ni `template/`, E-11 y la nota de RF-4 en F6. Sin implementar, sin commit ni push.

15. **[2026-11-01 14:51] El usuario aprobó las tareas** (45 pasos, 12,5 h), incluida la asignación de RF/CF en F5.5 y F5.6. Pidió NO implementar todavía: esperar a que 0010 tenga plan y tareas para respetar el orden 0008, 0010, 0009. Sin commit ni push.

16. **[2026-11-02 16:51] El usuario autorizó implementar F0 a F6** (0008 y 0010 ya en el árbol sin commitear; sin commit ni push). Cada fase se delega en el implementer con sus pasos en orden; los «Hecho cuando» se anotan por paso. Desviación consciente: delegación por fase y no por paso, para no multiplicar el costo; se informa en el reporte.
17. **[2026-11-02 16:51] F0 hecha (lectura).** F0.1: E21 existe en `template/.agents/validar-estructura.md` (línea 38; 0010 presente); 0008 y 0010 sin commitear, en el árbol. F0.2: ocurrencias «E1 a E20/E1–E20» a cambiar: `template/.agents/validar-estructura.md`/`sdd-estructura.md`/`sdd-init.md`/`SKILL.md`/`sdd.md`/`coordinator.md` ya dicen E21 (0010) salvo `reviewer.md:21` (template y réplica); paso 7 vigente en `AGENTS.md:54` y `template/AGENTS.md:53`. F0.3: `git check-ignore -v .agents/local/kit-visto` imprime `.gitignore:4:.agents/local/`; `template/.gitignore:4` la trae; `diff -rq template/.agents .agents`: solo `napkin.md` y `skills/caveman`. Último ADR: 0020 (siguientes libres: 0021 y 0022, a verificar en F6). Manifiestos: 0.4.0 y 0.5.0; versión actual 0.5.0.

18. **[2026-11-02 17:00] F1 a F5 hechas** (implementer por fase, no por paso). F1: `sdd-enlaces.md` (template y clon). Ajuste decidido durante F1: la rama sin marcador del paso 8 se activa con `.agents/.kit-version` ausente o ilegible, con o sin `template/` (`template/` solo omite el chequeo de arranque), porque sin versión no hay qué registrar. F2: `sync-agents.md` (Estado «visto», Chequeo de arranque, pasos 1-8 reorientados, disparador), `validar-estructura.md` (E1 sobre herramientas declaradas, E19 ignora `local/`, «Cómo responder» 4), `sdd-estructura.md` (solo propone y remite). F3: ambos `AGENTS.md` (paso 7 y «Roles y comandos»), README, SKILL, coordinator, `sdd.md`, `reviewer.md`, `sdd-init.md`. F4: `ACTUALIZAR.md` (sección nueva, `## Enlaces`, `## Huérfanos`, `## Marcador de versión`) e `INSTALAR.md`. F5: lint, `diff -rq` solo `napkin.md` y `caveman`, sin scripts, ensayo de instalación OK, escenarios E-1 a E-11 OK como recorrido de las reglas sobre estados reales en `mktemp -d` (no una sesión con otro agente). F5.2 con observaciones: E1 FALLA en el clon (7 filas FALTA de `.gemini/` y `.github/`, preexistente: clon solo con claude y cursor) y E20 FALLA hasta F6.4 (el manifiesto 0.5.0 ya no coincide con `template/`); E21 no aplica sin `.kit-version`.
19. **[2026-11-02 17:00] Observaciones menores de F5 (sin defecto de archivo).** (1) `sync-agents.md` pone los enlaces pendientes en la «línea 2» del aviso y el ejemplo en la línea 1; en la variante «solo enlaces» el aviso queda en 2 líneas. (2) E1 sin `kit-visto` mide todas las filas y da FALLA en un clon que usa pocas herramientas; el texto de E1 no dice si es FALLA o AVISO en ese caso. (3) Las tareas F3.4, F3.5, F3.6 y F3.8 pedían cambiar «E1 a E20» que 0010 ya había cambiado: cambio vacío, el grep final cumple. (4) En E-11 el aviso queda sin versión legible y vuelve en cada sesión (consecuencia aceptada de la decisión 11).

20. **[2026-11-02 17:03] F6 hecha** (F6.1 a F6.8; F6.9 sin marcar). Bump 0.6.0 único para las tres specs; changelog con las viñetas de 0008 y 0010 pegadas y las de 0009 añadidas; `manifiestos/0.6.0.md` con 43 filas (el recuento real de `template/` es 43, no 44) y E20 43 de 43; ADR 0021 (estado local «visto» y aviso al arranque) y 0022 (el arranque ya no detecta réplicas ni ignorados) aceptados, con sus filas en el índice de ADR; nota de RF-1d y RF-4 en la spec; fila 0009 en «En pruebas». F6.9: E1 FALLA en el clon (`.gemini/` y `.github/` ausentes) y E7 FALLA (filas de `adr`, `migracion-bd`, `seguridad-basica` y `caveman` ausentes en el `AGENTS.md` raíz); ambos preexistentes en HEAD y fuera del alcance de 0009; el usuario decide si se aceptan y se marca F6.9 o se atienden aparte (propuesta: dar de alta como deuda, con su visto bueno). Aviso E11: rutas de tiempo de ejecución (`.agents/.kit-version`, `.agents/local/`, `.agents/local/kit-visto`, `.claude/settings.json`) no existen en el clon.
21. **[2026-11-02 17:03] `/sdd-validate`: APROBADO salvo verificación manual.** RF-1 a RF-14 y CF-1 a CF-8 CUMPLEN por lectura y comandos de solo lectura. Sin defectos bloqueantes. Menores: enlaces pendientes en «línea 2» del aviso frente a línea 1 del ejemplo; E1 sin «visto» no dice si es FALLA o AVISO. Opcional: `skills/git-commit/SKILL.md:43-45` repite `git ls-files -ci` (anterior a 0009). Pendiente del usuario: probar `/sdd-enlaces` en una sesión real (crear un enlace, escribir «visto», comprobar que el aviso del siguiente arranque no se repite) y anotarlo en Pruebas. Desviación registrada: el implementer trabajó por fase y no por paso; los tiempos son el total del reloj, no por paso. Sin commit ni push.

22. **[2026-11-03 21:21] Retoma tras un corte de API: el registro estaba completo** (decisiones 18 a 21, tiempos y PUNTO DE RETOMO). Comprobado con comandos: `diff -rq template/.agents .agents` solo `napkin.md` y `caveman`; `VersionAgentKitSdd` = 0.6.0; E20 con 43 de 43 huellas y sin filas sobrantes; E21 no aplica en el clon (sin `.agents/.kit-version`) y la cadena del marcador de `template/AGENTS.md` aparece una vez en `validar-estructura.md`. E1 (FALTA de `.gemini/` y `.github/`, 7 filas) y E7 (faltan en el `AGENTS.md` raíz las filas de `adr`, `caveman`, `migracion-bd` y `seguridad-basica`) ya fallaban en HEAD (mismas 13 filas de la tabla; mismo E7 con `git show HEAD:AGENTS.md`). Marcas de `04_tasks.md`: 40 pasos `[x]`; sin pasos `[!]`; quedan `[ ]` F0.1, F0.2, F0.3 (hechos por la sesión principal, nunca marcados), F5.2 (E1 pendiente) y F6.9 (E1 y E7); las casillas «Hecho cuando» no se marcaron una a una. Sin commit ni push.

23. **[2026-11-03 21:22] Decisiones del usuario tras el reporte.** (1) E1 y E7 se arreglan dentro de 0009, no como deuda: se amplía el alcance aprobado, así que sigue el flujo `/sdd-change` (spec primero, luego plan y tareas solo del delta; sin implementar ni commit hasta aprobar). (2) El usuario aprueba 0009 sin la prueba manual de `/sdd-enlaces`: **riesgo aceptado** (los escenarios E-1 a E-11 fueron un recorrido de las reglas en `mktemp -d`, no una sesión real). La aprobación final de la implementación queda pendiente hasta cerrar el delta de E1 y E7. (3) F0.1 a F0.3 de `04_tasks.md` marcados `[x]` con la evidencia de la decisión 17, por autorización del usuario al coordinator.

24. **[2026-11-03 21:23] `/sdd-change` (delta E1/E7): spec propuesta.** `01_spec.md` ganó la sección «Cambio 1 (2026-11-03): E1 y E7» con RF-15 (E7) y RF-16 (E1), casos límite, CF-9 y CF-10; el cambio sigue «propuesto» y la spec original conserva `Estado: aprobada`. Recomendaciones del planner: E7 (a) añadir a la raíz las filas de `adr`, `migracion-bd` y `seguridad-basica` y exentar de E7 las skills presentes en `.agents/skills/` y ausentes en `template/` (como `caveman`) solo en el repo del kit; E1 (a) sin «visto» legible, `FALTA` de herramienta no declarada es AVISO y `DIFERENTE` u `OCUPADO` sin `excepcion:` son FALLA. Impacto: `validar-estructura.md` (template y réplica), `AGENTS.md` raíz, huella en `manifiestos/0.6.0.md`, una línea en el changelog; nueva fase F7 en las tareas. Estimado del delta: unas 2,5 h (2,2 h pendientes). Opcionales: aclarar la «línea 2» del aviso en `sync-agents.md` y que `git-commit/SKILL.md:43-45` remita a `sync-agents.md`. Sin implementar, sin commit ni push.

25. **[2026-11-03 21:27] Decisiones del usuario sobre el Cambio 1 incorporadas a la spec** (cambio aún «propuesto», sin aprobar). E7 (a): 3 filas del kit (`adr`, `migracion-bd`, `seguridad-basica`) en `AGENTS.md` raíz y exención de toda skill presente en `.agents/skills/` y ausente de `template/.agents/skills/`, solo en el repo del kit (RF-15). E1 (a): sin «visto» legible `DIFERENTE` y `OCUPADO` sin `excepcion:` son FALLA y `FALTA` de herramienta no declarada es AVISO con el texto «enlace FALTA de herramienta no declarada: correr `/sdd-enlaces`»; con «visto» legible E1 no cambia (RF-16). No se crean `.gemini/` ni `.github/` en esta máquina (7 filas FALTA como AVISO). Los dos opcionales entran: RF-17 (enlaces pendientes en la línea 2 del aviso; ejemplo en `sync-agents.md:116-118` incoherente) y RF-18 (`git-commit/SKILL.md:43-45` remite al paso 7 de `sync-agents.md`). CF-9 a CF-12. Archivos del delta: `validar-estructura.md`, `sync-agents.md` y `git-commit/SKILL.md` (template y réplica), `AGENTS.md` raíz, `manifiestos/0.6.0.md` (3 huellas) y `ChangelogAgentKitSdd.md`. Estimado pendiente: 2,6 h. Avisos del planner: «Fuera de alcance» original excluye cambios a E1-E20 salvo lo necesario (RF-17 y RF-18 son coherencias de texto: el usuario debe verlo al aprobar); el texto vigente de E1 (línea 18) ya trata como AVISO las filas no declaradas, por lo que RF-16 cambia sobre todo la severidad de `DIFERENTE` y `OCUPADO`; la sección final «Dudas abiertas» puede quedar desactualizada. Sin commit ni push.

26. **[2026-11-03 21:28] El usuario aprobó el Cambio 1 (RF-15 a RF-18).** Quedó marcado como aprobado en `01_spec.md` y se cerraron sus dudas abiertas. Sigue plan y tareas solo del delta (F7), con revisión; sin implementar ni commit.

27. **[2026-11-03 21:31] Plan y tareas del delta redactados y revisados.** `02` y `03` con «Delta Cambio 1» (D-1 a D-7); `04_tasks.md` con F7 (F7.1 a F7.8, 1,6 h); total 53 pasos y 14,1 h. Revisión del reviewer: APROBABLE, sin bloqueantes y 5 menores opcionales (orden de F7.6 frente al `diff -rq`, texto de E7 sin relajar filas a skills inexistentes, viñetas de F7.7 para CF-10 y CF-11, redacción de F7.8, aclaración en D-6). Sin implementar, sin commit ni push.

28. **[2026-11-03 21:37] Delta F7 implementado y validado; cierre.** F7.1 a F7.8 hechas (un implementer por paso; la sesión principal marcó las casillas). El validate del delta dio APROBADO con un defecto menor (el paso 3 de `git-commit/SKILL.md` repetía `git rm --cached`), corregido y con la huella del manifiesto recalculada. E1 en el clon: 6 filas OK y 7 AVISO (`.gemini/` y `.github/`), 0 FALLA. E7: `caveman` exenta, 0 FALLA. E20: 43 de 43. F5.2 y F6.9 marcados. El usuario aprobó 0009 (con riesgo aceptado: sin prueba manual de `/sdd-enlaces`). **Aviso para el usuario: no existe `06_flujo.md`**; el cierre exige uno aprobado o una decisión expresa de omitirlo (las specs 004 a 007 se cerraron «sin `06_flujo` por decisión del usuario»). Se cierra con esa salvedad, pendiente de su confirmación. Sin commit ni push.

## Pruebas
| RF | Verificación (F5) | Resultado |
|---|---|---|
| 1 | E-1 a E-4, E-10, E-11 | OK |
| 2 | Aviso ≤3 líneas, N=5, una vez, sin changelog | OK |
| 3 | `check-ignore`; escenarios sin Git y sin la regla | OK |
| 4 | Éxito escribe «visto»; rechazo, cero herramientas y sin marcador no | OK |
| 5 | E-8 y E-9 | OK |
| 6 | Comando por enlace y criterio (revisión de `sdd-enlaces.md`) | OK |
| 7 | Una confirmación por tipo, orden de la decisión 10 | OK |
| 8 | E-5 (excepción) | OK |
| 9 | Colgante retirado; OCUPADO intacto | OK |
| 10 | `grep` de pasos: una sola fuente | OK |
| 11 | `aplícala` ausente; E1 y E19 ajustados | OK |
| 12 | Reporte `OK (copia)` | OK |
| 13 | Sección en `ACTUALIZAR.md` (líneas ~498-506) y aviso con pasos mínimos | OK (revisión de texto) |
| 14 | `diff -rq`, `find` de scripts, `sdd-enlaces` en AGENTS/SKILL/coordinator/sdd/README/sdd-init/INSTALAR | OK; versión y changelog en F6 |

## Tiempos
| Paso | Inicio | Fin | Minutos | Pausa | Nota |
|---|---|---|---|---|---|
| spec + clarify | 2026-10-31 19:26 | 2026-10-31 19:28 | — | — | Incluye antecedentes; subagentes planner y reviewer |
| spec ajuste + clarify 2 | 2026-10-31 20:38 | 2026-10-31 20:39 | — | — | Respuestas del usuario aplicadas por el planner |
| ronda 3 + clarify final | 2026-10-31 20:50 | 2026-10-31 23:26 | — | — | Planner y reviewer |
| plan + revisión | 2026-10-31 23:33 | 2026-10-31 23:40 | — | — | Planner (02 y 03), reviewer y corrección |
| plan ajuste + revisión final | 2026-11-01 13:40 (aprox.) | 2026-11-01 13:49 | — | — | Planner y reviewer |
| plan aprobado + tasks + revisión | 2026-11-01 13:56 | 2026-11-01 13:59 | — | — | Planner (04_tasks, ajustes al plan) y reviewer |
| tareas aprobadas | 2026-11-01 14:51 | 2026-11-01 14:51 | 0 | — | Aprobación; implementación en pausa hasta la autorización |
| F0 | 2026-11-02 16:51 | 2026-11-02 16:52 | 1 | — | Lectura y verificación (coordinator) |
| F1 a F6 + validate | 2026-11-02 16:52 | 2026-11-02 17:03 | 11 | — | Implementer por fase y reviewer. Las horas por fase no se tomaron una a una; se registra el total del reloj real |
| change + plan/tareas del delta | 2026-11-03 21:22 | 2026-11-03 21:31 | — | — | planner (spec del Cambio 1, 02, 03, F7) y reviewer |
| F7.1 a F7.8 + validate del delta | 2026-11-03 21:31 | 2026-11-03 21:37 | 6 | — | Un implementer por paso; reviewer; corrección de git-commit |
| cierre | 2026-11-03 21:37 | 2026-11-03 21:37 | 0 | — | Plan contra real, índice, propuestas; re-cierre 2026-11-21 a `Cerrado` (corrida conjunta de 8 carpetas con la leyenda de la 0012): validación registrada en el retomo y columna del índice vaciada |

## Plan contra real
| Fase | Estimado (h) | Real | Nota |
|---|---|---|---|
| Spec, clarify (3 rondas), plan, tareas | — | ~30 min de agentes más esperas del usuario (la ronda 3 abarca 2,5 h de reloj con esperas, sin medir la pausa) | Tiempos de planificación sin estimado propio |
| F0 | 0,5 | 1 min | Lectura y verificación |
| F1 a F6 + validate | 12,5 | 11 min | Implementer por fase; sin tiempos por fase |
| F7 (delta E1/E7) | 2,6 (incl. 02/03/04) | ~6 min la implementación (21:31 a 21:37) | Un implementer por paso |
| Total implementación | 14,1 | ~18 min de reloj de agentes | Las horas estimadas eran de trabajo humano; el real mide ejecución de agentes |

Ajuste para el siguiente plan: las estimaciones en horas no corresponden a la ejecución por agentes (minutos); estimar aparte el tiempo de decisión del usuario, que fue el costo dominante, y tomar la hora al abrir y cerrar cada fase.
