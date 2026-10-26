# 05 — Avance: Deudas abiertas del kit
Leyenda: `[ ]` Pendiente · `[~]` En curso · `[x]` Hecho · `[!]` Bloqueado

## PUNTO DE RETOMO — leer esto primero
| Dato | Contenido |
|---|---|
| Última sesión | 2026-10-25 23:45 |
| Fase actual | `cierre` |
| Último paso cerrado | ADR 0016, 0017 y 0018, y la línea de `MEMORY.md`, 2026-10-25 23:26 |
| Siguiente paso | Ninguno. Commits hechos: `6f9e21a`, `6516e1c`, `6784948`, `63f19b2`, `3770895` |
| Bloqueos abiertos | Ninguno |
| Coordinación | Deudas 005, 006, 007 y 008 resueltas en este cierre. |
| Estado BD | No aplica |
| Código | `template/` editado. Renombres en la raíz. Manifiesto regenerado. Commits `6f9e21a`, `6516e1c`, `6784948`, `63f19b2`, `3770895` |

## Pasos
## Decisiones
1. **[2026-10-25 17:09] Las cuatro deudas abiertas van en una sola spec.** El usuario lo pidió («mete las 4 deudas en una misma spec»). Son la 005 (frontmatter en otras herramientas), la 006 (`/sdd-init` sugiere allow y deny), la 007 (nombres que chocan) y la 008 (limitaciones de la numeración). Alternativa descartada: una spec por deuda, que era lo que cada alta proponía.
2. **[2026-10-25 17:10] Número `007_DeudasAbiertas`.** En specs el mayor es `006`, local y en `@{u}`. El ancho de esta carpeta es 3. El contador de specs es independiente del de deuda y del de ADR. Alternativa descartada: tomar `016` para no repetir el 007 de la deuda y del ADR.
3. **[2026-10-25 17:10] La spec la redacta el planner, lanzado por el chat.** El coordinator sería un segundo paso. Constancia: no hubo fallo de delegación; la ruta es «Si no hay un solo paso». Alternativa descartada: escribir la spec en el chat.
4. **[2026-10-25 20:57] Clarify: CAMBIOS NECESARIOS.** El usuario dijo «yes» a `/sdd-clarify`. El chat lanzó al reviewer (el coordinator sería un segundo paso). Once hallazgos, sin proponer solución. La spec sigue en `borrador`. El hueco 17:16–20:50 (214 min) queda en pausa por confirmar y no entra en los minutos. Alternativa descartada: pasar la spec a `aprobada` con esos hallazgos.
5. **[2026-10-25 21:02] `.agents/` no se renombra.** El usuario lo dijo. Sale del corte de nombre nuevo y entra en «conservan el nombre». El marcador sigue en `.agents/.kit-version`. El resto de nombres altos y medios sigue sin sí ni no. Alternativa descartada: dejar `.agents/` en la lista de renombre.
6. **[2026-10-25 21:07] Tres nombres nuevos, en CamelCase, sin renombrar archivos.** El usuario dijo sí a `VERSION`, `CHANGELOG.md` y `LICENSE`, con la forma `VersionAgentKitSdd`. Quedan `VersionAgentKitSdd`, `ChangelogAgentKitSdd.md` y `LicenseAgentKitSdd`. El punto 6 de la constitución sigue pidiendo `VERSION` y `CHANGELOG.md`: esos dos no se mueven hasta cambiar ese punto. Alternativa descartada: inventar otro patrón, o renombrar los archivos en esta fase.
7. **[2026-10-25 21:08] Todo lo demás no se renombra.** El usuario lo dijo: se sigue manejando como hasta ahora. Entran `AGENTS.md`, las carpetas de herramienta, `CLAUDE.md`, `MEMORY.md`, `.gitignore`, `README.md` de la raíz del clon y `template/`, además de lo ya cerrado. Alternativa descartada: dejarlos en la lista de sí o no.
8. **[2026-10-25 21:09] Las citas del producto sí cambian con los tres nombres.** El usuario lo dijo. Entran la definición de clon en la numeración y el ejemplo de BOM que nombra `CHANGELOG.md`. `LICENSE` no tiene cita ahí. `<VERSION>` del manifiesto es el número de versión y no se toca. No se editó `template/`: los archivos aún no existen con el nombre nuevo y el punto 6 de la constitución sigue pidiendo los viejos. Alternativa descartada: sustituir `<VERSION>` o editar el producto en esta fase.
9. **[2026-10-25 21:13] Un prefijo con ceros de relleno no es fallo.** El usuario lo dijo: `00017` cuenta como 17, sin importar los ceros. El nombre no se reescribe. Si otro ítem de la misma carpeta ya vale ese entero, sigue siendo duplicado. Alternativa descartada: dejar el ancho inválido que E12 marca hoy.
10. **[2026-10-25 21:20] El clon no avisa, y el valor es el entero en cualquier ancho.** El usuario lo dijo: no avisa; `17`, `017`, `0017` y `00000017` cuentan como 17 para el conteo y para el duplicado. La mezcla válida de un proyecto sigue siendo AVISO. Alternativa descartada: que el clon también avise, o tratar cada ancho como un número distinto.
11. **[2026-10-25 21:22] El consecutivo sale solo de los prefijos numéricos.** El usuario lo dijo: `abc_Mal` no cuenta. No se renombra. Alternativa descartada: que un nombre no numérico fije el ancho de la carpeta.
12. **[2026-10-25 21:25] El ancho del clon se declara en el índice del producto.** El usuario eligió «La del template». El índice de numeración que viaja en el producto dice que el clon numera en 3 dígitos. El índice de un proyecto ya instalado no se reescribe. No se editó `template/` en esta fase. Alternativa descartada: dejar la declaración solo en el índice del taller.
13. **[2026-10-25 21:27] Un ítem nuevo del clon se escribe con 4 dígitos.** El usuario lo dijo. Corrige el «3 dígitos» de la decisión 12. Los ítems ya creados no se renumeran. `015` y `0015` son el mismo entero. Un proyecto ya instalado conserva el ancho de su carpeta. No se editó `template/`. Alternativa descartada: renumerar `001` a `0001`, o dejar el clon en 3.
14. **[2026-10-25 21:28] El punto 6 de la constitución nombra los archivos nuevos.** El usuario dijo «correcto actualizalo». El punto 6 pide `VersionAgentKitSdd` y `ChangelogAgentKitSdd.md` en cada versión. Los archivos `VERSION` y `CHANGELOG.md` siguen con el nombre viejo hasta que esta spec los mueva. Alternativa descartada: dejar el punto 6 con los nombres viejos.
15. **[2026-10-25 21:29] Spec aprobada.** El usuario dijo «yes». `Estado: aprobada`. Los archivos no se movieron y `template/` no se editó. Alternativa descartada: seguir en borrador.
16. **[2026-10-25 21:38] Plan escrito, sin aprobar.** El usuario dijo «procede». El chat lanzó al planner (el coordinator sería un segundo paso). `02` y `03` entregados. Tres candidatas a ADR, ninguna creada. `template/` no se editó. Alternativa descartada: escribir el plan en el chat.
17. **[2026-10-25 21:42] Plan aprobado.** El usuario dijo «yes». `02` y `03` quedan aprobados. Las tres candidatas a ADR siguen sin crear. `template/` no se editó. Alternativa descartada: pedir otro ajuste al plan.
18. **[2026-10-25 21:48] Tareas escritas, sin aprobar.** El usuario había aprobado el plan. El chat lanzó al planner (el coordinator sería un segundo paso). `04_tasks.md` entregado: F0 a F6, 26 pasos, 85 min. El chat ajustó el «Hecho cuando» de F4.4: el índice de planes ya tenía en el árbol las filas 004, 005 y 007, así que `git diff HEAD` no puede mostrar solo la frase de numeración. Alternativa descartada: dejar un chequeo que el paso no puede cumplir, o escribir las tareas en el chat.
19. **[2026-10-25 21:49] Tareas aprobadas. Arranca la implementación por F0.1.** El usuario dijo «apruebo y procede con la implementación». El chat lanza al implementer de un solo paso (el coordinator sería un segundo paso). No se empieza F1.1 en este turno. Alternativa descartada: encadenar los 26 pasos.
20. **[2026-10-25 21:51] F0.1 cerrado. `VERSION` es `0.5.0` y su último byte es `30` (el carácter `0`), no `0a`.** `cat -A` no muestra `$`. Los tres archivos siguen con el nombre viejo. El punto 6 ya nombra los nuevos. `git status` de `template/` y de las carpetas `001` a `006` salió vacío. Ampliaciones del implementer: la fila F0 del plan (el paso no cita RF; la ventana también mostró F1–F3 y no se usaron), la tabla del índice de specs para ver si otra spec está en curso, y recortes de decisiones al leer cada punto de retomo. Alternativa descartada: editar producto en F0.
21. **[2026-10-25 21:52] El usuario pidió implementar las fases que faltan.** Son F1 a F6. El chat lanza un implementer por paso y no vuelve a preguntar entre pasos. Cada paso se cierra y se anota antes del siguiente. Alternativa descartada: parar otra vez después de F1.1.
22. **[2026-10-25 22:28] Reintento de F4.3.** El lanzamiento anterior se cortó antes de editar. Las líneas 51 y 67 de `AGENTS.md` siguen con el texto viejo. El usuario dijo «reintenta». El hueco 22:23–22:28 no se suma. Alternativa descartada: dar F4.3 por hecho.
23. **[2026-10-25 22:41] E20 lo recalculó la sesión principal.** `validar-estructura.md` dice que E1–E20 los corre la sesión, no un subagente. Resultado: 42 archivos, 42 filas, 0 faltantes, 0 sobrantes, 0 huellas distintas. El implementer de F5.1 ya había regenerado el manifiesto. Alternativa descartada: dar E20 por verde solo con el reporte del implementer.
24. **[2026-10-25 23:02] F6.5 quedó marcado y el implementer se trabó al responder.** No se relanzó: las casillas están `[x]` y el registro de F1.1 sigue igual. El frontmatter de los cuatro agentes no entra en el diff de este paso. Alternativa descartada: repetir el chequeo con otro implementer.
25. **[2026-10-25 23:13] F0 a F6 cerrados.** Los 26 pasos de `04_tasks.md` están en `[x]`. No se arrancó `validate`. Las tres candidatas a ADR siguen sin crear. Sin commit. Alternativa descartada: seguir a la validación sin que el usuario lo pida.
26. **[2026-10-25 23:22] Validate aprobado.** El usuario dijo «procede con validate». El chat lanzó al reviewer (el coordinator sería un segundo paso). VEREDICTO: APROBADO. Modo de seguridad: solo secretos, sin hallazgos. UI no aplica. Cuatro notas opcionales, ninguna bloquea. No se corrigió nada. Alternativa descartada: abrir una vuelta de cambios por el aviso opcional del changelog.
27. **[2026-10-25 23:24] Sin `06_flujo`. Cierre en esta sesión.** El usuario dijo «omite el flujo y procede al cierre». La tabla de Pasos no tiene renglones: el espejo no se llenó. La búsqueda no encontró un paso en `[ ]`, `[~]` ni `[!]`; solo la leyenda. La frase «pausa por confirmar» sigue en la nota de `clarify` y esos 214 min no entraron al total; no se vuelve a preguntar. Las deudas 005 a 008 pasan a `Resuelta`. Los tres ADR candidatos no se crean en ese momento. Alternativa descartada: exigir `06_flujo.md` aprobado.
28. **[2026-10-25 23:26] El usuario dijo «yes» a los tres ADR y a la línea de MEMORY.** Quedaron `0016`, `0017` y `0018`, en 4 dígitos. El mayor entero local era 15 y el remoto 14. La viñeta vieja del changelog no se tocó. Alternativa descartada: numerarlos `016` o reescribir la línea del tercio.

## Pruebas
- **F0.1** (2026-10-25 21:51). Nombres viejos presentes; nombres nuevos ausentes. `VERSION` = `0.5.0`, último byte `30`. Punto 6 ya actualizado. Ninguna spec `001`–`006` toca `template/`. UI: no aplica.
- **F1.1** (2026-10-25 21:59). Registro en `template/.agents/skills/sdd/restricciones-de-rol.md`. Fuentes HTTP 200: Cursor `https://cursor.com/docs/subagents`, Gemini CLI `https://geminicli.com/docs/core/subagents/`, Copilot `https://docs.github.com/en/copilot/reference/custom-agents-configuration`, Kiro `https://kiro.dev/docs/custom-agents/configuration-reference/`. `model` soportado en las cuatro, y no viaja igual que el hecho (c). `omitClaudeMd` sin veredicto en las cuatro. `tools` sin veredicto en Cursor; soportado en Gemini CLI, Copilot y Kiro, y no viaja igual que el hecho (a). `Garantizada` acotada a donde el campo la sostiene. Filas de Claude Code intactas. `file -i` utf-8, sin BOM. Frontmatter de los cuatro agentes intacto. UI: no aplica.
- **F1.2** (2026-10-25 22:04). `template/.agents/sync-agents.md` describe el otro formato de Kiro (`.kiro/agents/`), sin fila de enlace. URLs 200: la referencia y `https://kiro.dev/docs/custom-agents/`. utf-8, sin BOM. UI: no aplica.
- **F2.1** (2026-10-25 22:07). `template/.agents/commands/sdd-init.md` manda mostrar los dos fragmentos en la propuesta y si el proyecto ya está personalizado. El paso 4 no los escribe. Sin YAML duplicado. utf-8, sin BOM. UI: no aplica.
- **F3.1** (2026-10-25 22:09). `git mv VERSION VersionAgentKitSdd`. `cmp` igual. 5 bytes, último `30`. Diff: rename, 0 inserciones. Sin commit. UI: no aplica.
- **F3.2** (2026-10-25 22:11). `git mv CHANGELOG.md ChangelogAgentKitSdd.md`. Línea 21 cita `VersionAgentKitSdd`. Encabezado `## 0.5.0 — 2026-10-24` igual. Viñeta de numeración intacta. utf-8, sin BOM. Sin commit. UI: no aplica.
- **F3.3** (2026-10-25 22:13). `git mv LICENSE LicenseAgentKitSdd`. `cmp` igual, 1077 bytes. napkin/LICENSE y `template/` no se tocaron. Sin commit. UI: no aplica.
- **F3.4** (2026-10-25 22:14). `AGENTS.md` líneas 111 y 120 citan los nombres nuevos. Líneas 51 y 67 intactas. utf-8, sin BOM. UI: no aplica.
- **F3.5** (2026-10-25 22:16). `ACTUALIZAR.md` líneas 51, 57, 184 y 313. `<VERSION>` y napkin/LICENSE intactos. utf-8, sin BOM. UI: no aplica.
- **F3.6** (2026-10-25 22:18). `INSTALAR.md` líneas 16, 54, 56, 73 y 97. utf-8, sin BOM. UI: no aplica.
- **F3.7** (2026-10-25 22:19). `README.md` de la raíz, líneas 68 y 73. El archivo no se renombró. utf-8, sin BOM. UI: no aplica.
- **F4.1** (2026-10-25 22:23). `template/.docs/002_specs/README.md` › « Numeración única »: entero con relleno, `abc_Mal`, ítem nuevo del clon en 4 dígitos, nombres nuevos en la definición de clon. utf-8, sin BOM. UI: no aplica.
- **F4.2** (2026-10-25 22:27). `template/.agents/validar-estructura.md`: E12 remite a « Numeración única » y dice el resultado; el ejemplo de BOM de E15 es `ChangelogAgentKitSdd.md`. E20 y `<VERSION>` intactos. utf-8, sin BOM. UI: no aplica.
- **validate** (2026-10-25 23:22). VEREDICTO: APROBADO. Secretos: sin hallazgos (modo solo secretos). UI: no aplica. ADR 014 y 015 sin diff. Deudas 005 a 008 siguen en atención.
- **F4.3** (2026-10-25 22:32). `AGENTS.md` líneas 51 y 67: ítem nuevo del clon en 4 dígitos, los ya creados no se renumeran, remiten al índice del producto. Línea 52 y líneas 111 y 120 intactas respecto de este paso. utf-8, sin BOM. UI: no aplica.

## Tiempos
| Paso | Inicio | Fin | Minutos | Pausa | Nota |
|---|---|---|---|---|---|
| spec | 2026-10-25 17:09 | 2026-10-25 17:16 | 7 | 0 | planner redactó `01_spec` en borrador; el chat lanzó al planner |
| plan | 2026-10-25 21:30 | 2026-10-25 21:38 | 8 | 0 | planner escribió `02` y `03`; el chat lo lanzó |
| tasks | 2026-10-25 21:42 | 2026-10-25 21:48 | 6 | 0 | planner escribió `04_tasks.md`; el chat lo lanzó y ajustó F4.4 |
| F0.1 | 2026-10-25 21:49 | 2026-10-25 21:51 | 2 | 0 | implementer confirmó el punto de partida; el chat lo lanzó |
| F1.1 | 2026-10-25 21:52 | 2026-10-25 21:59 | 7 | 0 | implementer registró el veredicto; el chat lo lanzó |
| F1.2 | 2026-10-25 21:59 | 2026-10-25 22:04 | 5 | 0 | implementer describió el formato de Kiro; el chat lo lanzó |
| F2.1 | 2026-10-25 22:04 | 2026-10-25 22:07 | 3 | 0 | implementer hizo que sdd-init muestre deny y allow; el chat lo lanzó |
| F3.1 | 2026-10-25 22:07 | 2026-10-25 22:09 | 2 | 0 | implementer renombró VERSION; el chat lo lanzó |
| F3.2 | 2026-10-25 22:09 | 2026-10-25 22:11 | 2 | 0 | implementer renombró el changelog y la cita de la línea 21; el chat lo lanzó |
| F3.3 | 2026-10-25 22:11 | 2026-10-25 22:13 | 2 | 0 | implementer renombró LICENSE; el chat lo lanzó |
| F3.4 | 2026-10-25 22:13 | 2026-10-25 22:14 | 1 | 0 | implementer citó los nombres nuevos en AGENTS.md; el chat lo lanzó |
| F3.5 | 2026-10-25 22:14 | 2026-10-25 22:16 | 2 | 0 | implementer citó los nombres nuevos en ACTUALIZAR.md; el chat lo lanzó |
| F3.6 | 2026-10-25 22:16 | 2026-10-25 22:18 | 2 | 0 | implementer citó los nombres nuevos en INSTALAR.md; el chat lo lanzó |
| F3.7 | 2026-10-25 22:18 | 2026-10-25 22:19 | 1 | 0 | implementer citó los nombres nuevos en README.md; el chat lo lanzó |
| F4.1 | 2026-10-25 22:19 | 2026-10-25 22:23 | 4 | 0 | implementer escribió los cuatro bordes en el índice del producto; el chat lo lanzó |
| F4.2 | 2026-10-25 22:23 | 2026-10-25 22:27 | 4 | 0 | implementer actualizó E12 y el ejemplo de E15; el chat lo lanzó |
| F4.3 | 2026-10-25 22:28 | 2026-10-25 22:32 | 4 | 0 | reintento; implementer remitió la numeración del clon en AGENTS.md |
| F4.4 | 2026-10-25 22:32 | 2026-10-25 22:35 | 3 | 0 | implementer remitió la numeración en el índice de planes; el chat lo lanzó |
| F4.5 | 2026-10-25 22:35 | 2026-10-25 22:35 | 1 | 0 | implementer remitió la numeración en el índice de ADR; el chat lo lanzó. Fin tomado al lanzar F4.6 |
| F4.6 | 2026-10-25 22:35 | 2026-10-25 22:41 | 6 | 0 | implementer corrigió la viñeta del clon y anotó el feature; el chat lo lanzó |
| F5.1 | 2026-10-25 22:41 | 2026-10-25 22:45 | 4 | 0 | manifiesto 42 filas, 5 huellas nuevas; la sesión recalculó E20 42/42. El par F5+F6.1 cabe en 22:41–22:48; no hubo hora intermedia |
| F6.1 | 2026-10-25 22:45 | 2026-10-25 22:48 | 3 | 0 | refresco de .agents/; napkin y caveman locales. Misma nota de reloj que F5.1 |
| F6.2 | 2026-10-25 22:48 | 2026-10-25 22:48 | 1 | 0 | ensayo en mktemp, 42 archivos, segunda pasada no pisó. Fin mínimo: el reloj no separó inicio y fin |
| F6.3 | 2026-10-25 22:48 | 2026-10-25 22:53 | 5 | 0 | grep del conjunto vivo; solo quedan `<VERSION>` y napkin/LICENSE. Reparto del hueco 22:48–23:02; no hubo reloj intermedio |
| F6.4 | 2026-10-25 22:53 | 2026-10-25 22:57 | 4 | 0 | versión `0.5.0`, último byte `30`, sin `0.5.1`. Mismo hueco que F6.3 |
| F6.5 | 2026-10-25 22:57 | 2026-10-25 23:02 | 5 | 0 | registro y Kiro; el implementer se trabó al responder. Mismo hueco que F6.3 |
| F6.6 | 2026-10-25 23:02 | 2026-10-25 23:05 | 3 | 0 | texto de sdd-init. Reparto del hueco 23:02–23:13; no hubo reloj intermedio |
| F6.7 | 2026-10-25 23:05 | 2026-10-25 23:10 | 5 | 0 | siete casos de numeración en mktemp. Mismo hueco que F6.6 |
| F6.8 | 2026-10-25 23:10 | 2026-10-25 23:13 | 3 | 0 | encoding, E19 y 42 huellas. Mismo hueco que F6.6 |
| validate | 2026-10-25 23:17 | 2026-10-25 23:22 | 5 | 0 | reviewer, VEREDICTO: APROBADO; el chat lo lanzó |
| cierre | 2026-10-25 23:24 | 2026-10-25 23:25 | 1 | 0 | sin `06_flujo`; deudas 005 a 008 resueltas; la sesión lo escribió |
| clarify | 2026-10-25 20:50 | 2026-10-25 20:57 | 7 | 0 | reviewer, CAMBIOS NECESARIOS, 11 hallazgos. Hueco 17:16–20:50 (214 min): pausa por confirmar; no entra en este renglón |

## Plan contra real
| Fase | Estimado | Real | Delta |
|---|---|---|---|
| F0 | 5 min | 2 min | −3 min |
| F1 | 20 min | 12 min | −8 min |
| F2 | 8 min | 3 min | −5 min |
| F3 | 12 min | 12 min | 0 |
| F4 | 16 min | 22 min | +6 min |
| F5 | 6 min | 4 min | −2 min |
| F6 | 18 min | 29 min | +11 min |
| Documentación SDD | sin estimado por fase | 34 min | — |

El real de F0 a F6 suma 84 min, contra 85 estimados. F6 se desvió: ocho chequeos, cada uno con su lanzamiento, y F6.5 se trabó al responder. F4 incluye el reintento cortado. En el siguiente plan, si el usuario pide correr las fases que faltan, los chequeos de una misma validación van en un solo paso. El tercio habitual no se dio: un implementer por paso ocupó casi el estimado entero.

Documentación SDD: spec 7, clarify 7, plan 8, tasks 6, validate 5, cierre 1.
