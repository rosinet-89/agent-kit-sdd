# 05 — Avance: Convivencia de la numeración de 3 y 4 dígitos
Leyenda: `[ ]` Pendiente · `[~]` En curso · `[x]` Hecho · `[!]` Bloqueado

## PUNTO DE RETOMO — leer esto primero
| Dato | Contenido |
|---|---|
| Última sesión | 2026-10-25 16:50 |
| Fase actual | Cerrada |
| Último paso cerrado | `cierre` 2026-10-25 16:50; F7.6 aprobada por el usuario; sin `06_flujo` (decisión 29) |
| Siguiente paso | Ninguno en esta carpeta. Siguen propuestas sin crear (decisión 30): ADR de la fuente única en el grupo C y deuda de limitaciones. Commit si el usuario lo pide |
| Bloqueos abiertos | Ninguno |
| Coordinación | Corre en paralelo con `004_ModeloYPermisosAgentes`. Las fases de documentación (`spec`, `clarify`, `plan`, `tasks`) no tocan `template/` y no chocan. **Implementación en serie:** no editar `template/` a la vez que la `004`. Archivos compartidos: `CHANGELOG.md` (mismo bloque de 0.5.0, líneas distintas), `template/AGENTS.md` (secciones distintas: `004` "Qué lee cada rol", esta "Formato y numeración") y `manifiestos/0.5.0.md` + E20: lo regenera quien cierre segundo, siempre al final. Partir del contenido actual de cada archivo y no revertir trabajo ajeno. Corlos puede estar editando otras carpetas; no tocar `003_OptimizarArnes`. |
| Estado BD | No aplica |
| Código | Ninguno |

## Pasos
| Fase | Pasos | Estado |
|---|---|---|
| F0 Preparación | F0.1–F0.2 | [x] |
| F1 Fuente única « Numeración única » | F1.1–F1.5 | [x] (F1.3 aprobada por el usuario) |
| F2 Piezas que remiten a la fuente | F2.1–F2.8 | [x] |
| F3 Chequeo E12 con AVISO | F3.1–F3.2 | [x] |
| F4 Aviso de numeración en `ACTUALIZAR.md` | F4.1–F4.3 | [x] (F4.1 aprobada por el usuario) |
| F5 CHANGELOG y alineación del clon | F5.1–F5.2 | [x] |
| F6 Manifiesto | F6.1–F6.2 | [x] |
| F7 Validación | F7.1–F7.6 | [x] (F7.6 visto final del usuario) |
| F8 Correcciones de la validación (añadida) | F8.1–F8.4 | [x] |

## Decisiones
1. **[2026-10-25 15:04] Se atiende la deuda `004_ConvivenciaNumeracion3y4Digitos` por el flujo SDD, como esta spec,** solo si no choca con la `004`: el cruce de archivos mostró que las fases de documentación no tocan `template/` y que la implementación debe ir en serie, con el manifiesto al final. Alternativa descartada: esperar a que la `004` cierre por completo antes de abrir la spec.
2. **[2026-10-25 15:29] Un ítem nuevo conserva el ancho de su carpeta** y se compara por valor numérico (`017` = `0017`). Alternativa descartada: pasar a 4 dígitos desde el primero (rompe el orden alfabético).
3. **[2026-10-25 15:29] Fuente única en « Numeración única »,** con aviso al actualizar (`ACTUALIZAR.md` avisa si el README del proyecto trae la regla vieja y ofrece el texto sin escribir sin confirmación); las demás piezas remiten. Alternativa descartada: archivo nuevo del grupo A.
4. **[2026-10-25 15:29] Con anchos mezclados, el ítem nuevo usa el ancho del de mayor valor numérico.** Alternativas descartadas: la mayoría y preguntar cada vez.
5. **[2026-10-25 15:29] E12 con anchos mezclados y sin duplicados informa sin fallar;** el duplicado por valor numérico sigue siendo fallo. Alternativa descartada: contarlo como fallo.
6. **[2026-10-25 15:29] En una carpeta de ítems vacía se usa el ancho de las otras carpetas con ítems,** y 4 solo si ninguna los tiene. Alternativa descartada: siempre 4.
7. **[2026-10-25 15:31] Clarify: 18 hallazgos del reviewer (3 ALTA); veredicto CAMBIOS NECESARIOS.**
8. **[2026-10-25 15:31] Detección en la actualización por nombres de carpeta, sin leer el README** (grupo C): si hay ítems de 3 dígitos en specs, deuda o ADR, `ACTUALIZAR.md` ofrece el texto de la regla sin escribir sin confirmación; "mantener" acredita que el usuario ya lo vio. No cambia la definición del grupo C. Alternativa descartada: lectura de solo lectura de los tres README (obliga a enmendar el grupo C y a definir qué texto cuenta como vigente).
9. **[2026-10-25 15:31] Las piezas llevan la regla mínima de una frase** (« un ítem nuevo conserva el ancho de su carpeta ») y remiten a « Numeración única » para el detalle, para funcionar aunque el README del proyecto sea el de 0.4.0. Excepción acotada a la fuente única, declarada como limitación conocida. Alternativa descartada: solo remitir (la pieza no sabría qué ancho usar).
10. **[2026-10-25 15:34] Spec aprobada por el usuario** (`Estado: aprobada`; 20 RF, 8 RNF, sin dudas abiertas), con las decisiones del planner: carpeta vacía toma el ancho del ítem de mayor valor entre las otras (empate: specs, deuda, ADR); ancho válido 3 o 4 dígitos o los que el valor necesite si pasa de 9999; en el clon un ancho distinto del declarado es `FALLA`; E12 gana el estado `AVISO` para anchos mezclados; el aviso de numeración se repite hasta "mantener", no cuenta como pendiente ni bloquea el marcador; mismo nombre con distinto ancho se trata como probable mismo ítem.
11. **[2026-10-25 15:34] Se abre `/sdd-plan`:** el plan es solo documentación y no choca con la `004`; la implementación queda en serie con el manifiesto al final.
12. **[2026-10-25 15:39] Orden de implementación: la `005` va DESPUÉS de que la `004` termine sus ediciones de `template/`;** quien cierre segundo regenera el manifiesto una sola vez. La `006_RutaSiNoDelega` (otra sesión) también tocará `template/`: se coordina aparte. Alternativas descartadas: antes de la `004` y en paralelo.
13. **[2026-10-25 15:39] Placeholders `<prefijo>` y ejemplos sin número** (cambia los 9 `argument-hint` y los ejemplos). Alternativa descartada: conservar `NNNN` (seguiría afirmando 4 dígitos).
14. **[2026-10-25 15:39] Se alinean con un cambio mínimo (remitir a la fuente única) las frases « en un proyecto el ancho es de 4 » del clon** (`AGENTS.md` de la raíz y los README de specs y ADR); el clon sigue en 3 dígitos. Alternativa descartada: no tocar el clon (quedaría contradiciendo la regla nueva).
15. **[2026-10-25 15:42] Plan y tareas aprobados por el usuario** (`03_plan.md`, 89 min; `04_tasks.md`, 30 pasos), sin implementar. La implementación queda para después de la `004` (F0.1 lo exige). Alternativa descartada: implementar ahora.
16. **[2026-10-25 16:19] F0.1 hecho:** la `004` cerró (16:13) y regeneró manifiesto y E20; `git status --short template manifiestos CHANGELOG.md` vacío; sin cambios ajenos pendientes. Línea base de commits: `8eb1b7e`, `cdefc4c`, `565e964`, `30cd458`, `2fc7125`, `09ab080`.
17. **[2026-10-25 16:20] F0.2 hecho; fase F0 cerrada.** `template/` tiene 42 archivos, no los 41 del plan: la `004` añadió `.agents/skills/sdd/restricciones-de-rol.md` y regeneró el manifiesto (42 filas). La línea base pasa a 42. `$bak` = `/tmp/tmp.WZy8FH7TII` (copia previa del protocolo P; está en `/tmp`, se pierde si se reinicia la máquina antes de F2.8); lista de antes en `$bak/antes.txt`: 50 líneas en 21 archivos. Ritmo acordado con el usuario: un implementer por fase, con parada al cerrar cada una.
18. **[2026-10-25 16:38] F1 cerrada; F1.3 (verificación manual) aprobada por el usuario.** Regla de « Numeración única » aprobada tal como quedó. Dos decisiones del implementer fuera del plan, aceptadas: la frase mínima aparece sin paréntesis de remisión dentro de la propia fuente (sería autorreferente) y se añadió la línea « Esta es la única fuente de la regla… » al inicio de la sección. Cambios ajenos vistos y no tocados: `.agents/skills/` sin seguimiento y la deuda `007_NombresQueChocanConHerramientas` (otra sesión). El usuario eligió parar tras F1; F2 pendiente de su visto bueno.
19. **[2026-10-25 16:40] F2 cerrada,** verificada por la sesión principal contra `HEAD`: 19 archivos de `template/` cambiados (F1 + F2), los 9 comandos con una sola línea, lint limpio; restos de `NNN`: `COM-NNN` (no es prefijo) y la fila E12 (F3). Decisión menor del implementer fuera del plan, aceptada: el ejemplo `0017_NombreFeature` de la descripción de `sdd-spec` pasa a `<prefijo>_NombreFeature`. Limitación anotada: el protocolo P compara con copias hechas justo antes de editar, así que no detecta ediciones ajenas anteriores; por eso la sesión principal contrasta con `git diff` contra `HEAD`. Ritmo: el usuario autorizó correr F2 a F7 seguidas, con parada ante algo ajeno, un chequeo roto o una verificación manual suya (F4.1, F7.6).
20. **[2026-10-25 16:40] F3 cerrada,** verificada contra `HEAD`: `validar-estructura.md` +6 −4, todo propio; fila E20 idéntica a `HEAD`; lint limpio. Decisión menor del implementer, aceptada: pasos 2 y 3 repartidos en dos líneas por ancho. La copia previa de `/tmp/tmp.WZy8FH7TII/template/.agents/` no existía y la creó justo antes de editar, tras comprobar que el archivo coincidía con `HEAD`.
21. **[2026-10-25 16:42] F4 cerrada; F4.1 (verificación manual del aviso) aprobada por el usuario,** que además autorizó seguir con F5 a F7. `ACTUALIZAR.md` +38 −1, verificado contra `HEAD`: todo propio; sin ancho absoluto fuera de la fuente (el "3 dígitos" es el criterio de detección de RF-16); no se tocó `template/` ni el grupo C. El aviso se calcula por nombres, no cuenta como pendiente ni bloquea el marcador, y «mantener» lo oculta mientras no cambie el texto ofrecido.
22. **[2026-10-25 16:43] F5 cerrada,** verificada contra `HEAD`: `CHANGELOG.md` +8 −4, `AGENTS.md` +2 −2, README de specs +3 −2 (la línea de más es la fila 005, previa), README de ADR +1 −1; sin "ancho es de 4" en el clon; `VERSION` 0.5.0. La fila 005 del índice sigue diciendo "implementa después de la 004": se actualiza al cierre.
23. **[2026-10-25 16:44] F6 cerrada: `template/` congelado.** Manifiesto de 0.5.0 regenerado una sola vez: 20 filas con huella nueva (los 20 archivos de `template/` modificados por esta spec), 42 filas en total. E20 OK con el cálculo del implementer y con uno independiente de la sesión principal. Toda edición de `template/` a partir de ahora obliga a repetir F6.
24. **[2026-10-25 16:47] F7.1 a F7.5 validadas por dos reviewers en paralelo: ambos APROBADO.** Evidencia en `/tmp/tmp.nSawgeOMPI` (numeración y duplicados) y la ruta de `/tmp/simpath` (aviso, E12, E13, E20, contraste). 15 casos de numeración y duplicados y 12 de aviso, E12 y chequeos finales: todos dan lo del paso. Contraste 0.4.0→0.5.0: grupo A = 22 KIT-VIEJO, 6 NUEVO, 5 IGUAL, 0 PERSONALIZADO, 0 RETIRADO; el diff de `ACTUALIZAR.md` no toca `## Clasificación` ni `## Grupos por ruta`.
25. **[2026-10-25 16:47] Cinco ambigüedades de redacción halladas en F7;** el usuario aprueba corregir las 1 a 4 ahora y repetir F6: (1) empate de valor con anchos distintos, (2) `1000_` en el clon da FALLA pese al desbordamiento, (3) límites del fragmento cuya huella identifica el aviso de «mantener», (4) E12 con mezcla válida más ítem inválido. La 5 (E12 en el clon se compara con el ancho que declara "su índice" y el de `template/` no declara ninguno; solo el README local del kit) queda como limitación del clon. Se añade la fase F8 a `04_tasks.md` (4 pasos, 10 min). Alternativa descartada: aprobar tal cual y registrar las 5 como deuda.
26. **[2026-10-25 16:47] Desempate de ancho:** en empate de valor con anchos distintos, el ancho de la carpeta es el del ítem publicado en la rama remota y, si ninguno lo está, el mayor ancho (decisión de la sesión principal, dentro de la autorización de corregir). Nota E13: la fila de la `005` en el índice de specs sigue en "Planificado"; la corrige `/sdd-cierre`.
27. **[2026-10-25 16:49] F8 cerrada y F8.4 APROBADA por el reviewer** (9 casos de E12 y desempate: FALLA/AVISO/clon/desbordamiento/ancho del publicado, todos como pide la fila; huella del aviso `9903c8f8d27e93b49744947fbfd1206592e4037e2cbdda3c909fed67f0f0d213` con dos extracciones independientes; E20 42 de 42; fila E20 idéntica a `HEAD`). Verificado por la sesión principal con cálculo propio. Decisión de la sesión principal: el "3 dígitos" de `ACTUALIZAR.md` › `## Aviso de numeración` se conserva porque es el criterio de detección que exige RF-16, no una afirmación del ancho de la regla.
28. **[2026-10-25 16:49] Limitaciones conocidas que dejó la revalidación (no bloquean):** (a) un prefijo de 5 o más dígitos con ceros de relleno (`00017_`) se trata como inválido por la regla « sin ceros de relleno »; (b) en el clon, una carpeta con mezcla válida (p. ej. `001_` y `1000_` por desbordamiento) no emite AVISO porque el AVISO es solo de proyecto; la fila E12 no lo dice con esas palabras; (c) el ancho de carpeta con un ítem inválido no está definido (el script lo ignora). Más la limitación 5 de F7: el índice de `template/` no declara el ancho del clon; solo el README local del kit.
29. **[2026-10-25 16:50] `06_flujo.md` omitido por decisión del usuario** (como las `004` y `006`): es una regla de numeración y un aviso de actualización, sin pantallas ni flujo de usuario final. F7.6 aprobada. Cierre ejecutado.
30. **[2026-10-25 16:50] Propuestas del cierre, SIN crear (esperan visto bueno del usuario).** MEMORY.md: (a) el protocolo P compara con copias hechas justo antes de editar y no delata ediciones ajenas previas: contrastar con `git diff` contra `HEAD`; (b) reanudar un subagente con `SendMessage` a veces no lo arranca: confirmar con `ListAgents` que figura como `running`; (c) en features que solo editan Markdown estimar en minutos. ADR candidatos: (1) regla de convivencia de anchos (conservar el ancho de la carpeta, comparar por valor numérico, no renumerar nunca; desempate por ítem publicado); (2) fuente única de la regla en el grupo C con aviso por nombres de carpeta y regla mínima de una frase repetida en las piezas como excepción acotada. Deuda candidata (lo diferido): las limitaciones de la decisión 28 (ceros de relleno, AVISO en el clon, ancho con ítem inválido, el índice de `template/` no declara el ancho del clon).
31. **[2026-10-25 16:52] Propuestas del cierre aprobadas por el usuario y creadas:** 3 entradas en `MEMORY.md` (protocolo P frente a `git diff` contra `HEAD`; reanudar subagente y confirmar con `ListAgents`; estimar en minutos los features solo-Markdown) y el ADR 014 « Un ítem nuevo conserva el ancho de su carpeta » (`.docs/004_adr/014_ConvivenciaDeAnchosDeNumeracion.md`). No se creó el ADR de la fuente única en el grupo C ni la deuda de limitaciones: el usuario no las marcó; siguen como propuestas de la decisión 30.

## Pruebas
Validación de F7 y F8 por reviewers, en `mktemp -d` (nunca en el kit ni en un proyecto real); la sesión principal contrastó con `git diff` contra `HEAD` y con un cálculo propio de E20.
- F7.1: instalación 42 de 42, solo marcadores permitidos. F7.2: 9 casos de numeración. F7.3: 5 casos de duplicados con remoto simulado `git init --bare`. Reviewer A: APROBADO (15 de 15). Evidencia en `/tmp/tmp.nSawgeOMPI`.
- F7.4: aviso por nombres, «mantener», E12 en FALLA/AVISO/clon. F7.5: E13, E20 42/42, contraste 0.4.0→0.5.0 (grupo A: 22 KIT-VIEJO, 6 NUEVO, 5 IGUAL, 0 PERSONALIZADO, 0 RETIRADO), lint, secretos. Reviewer B: APROBADO. Ruta en `/tmp/simpath`.
- F8.4: 9 casos de E12 y desempate; huella del aviso `9903c8f8d27e93b49744947fbfd1206592e4037e2cbdda3c909fed67f0f0d213` con dos extracciones independientes; E20 42/42; fila E20 sin cambios. Reviewer: APROBADO. Evidencia en `/tmp/tmp.yqrhLQR2Lv`.
- Verificaciones manuales del usuario: F1.3 (regla de « Numeración única »), F4.1 (aviso) y F7.6 (visto final): todas aprobadas.

## Tiempos
| Paso | Inicio | Fin | Minutos | Pausa | Nota |
|---|---|---|---|---|---|
| spec | 2026-10-25 15:04 | 2026-10-25 15:29 | 25 | 0 | planner (16 RF, 8 RNF), 5 dudas respondidas; incluye ≈14 min de espera por el límite de sesión de la API (15:06 a 15:20), contados por ser menos de 30 |
| clarify | 2026-10-25 15:29 | 2026-10-25 15:34 | 5 | 0 | reviewer (18 hallazgos), 2 decisiones, ajustes del planner y aprobación |
| plan | 2026-10-25 15:34 | 2026-10-25 15:39 | 5 | 0 | planner (02 y 03), 3 preguntas respondidas y plan final sin condicionales |
| tasks | 2026-10-25 15:41 | 2026-10-25 15:42 | 1 | 0 | planner (30 pasos, 8 fases, 89 min), revisión y aprobación |
| F0.1 | 2026-10-25 16:19 | 2026-10-25 16:19 | 1 | 0 | precondición comprobada (004 cerrada, manifiesto e E20 regenerados); `git status` vacío en template, manifiestos y CHANGELOG |
| F0.2 | 2026-10-25 16:20 | 2026-10-25 16:20 | 1 | 0 | línea base: 42 archivos en `template/`, `$bak` en `/tmp/tmp.WZy8FH7TII`, lista de antes con 50 líneas en 21 archivos |
| F1.1 | 2026-10-25 16:23 | 2026-10-25 16:24 | 1 | 0 | « Numeración única » reescrita; índice con encabezado `Prefijo` (implementer, fase F1 en una invocación) |
| F1.2 | 2026-10-25 16:23 | 2026-10-25 16:24 | 1 | 0 | README de deuda: remite a la fuente, `<prefijo>` |
| F1.3 | 2026-10-25 16:23 | 2026-10-25 16:38 | 1 | 14 | README de ADR; casilla manual aprobada por el usuario a las 16:38 (la espera hasta la aprobación, 14 min, va como pausa) |
| F1.4 | 2026-10-25 16:23 | 2026-10-25 16:24 | 1 | 0 | lint L y diffs limpios (mismo tramo que F1.1; mínimo 1 minuto) |
| F1.5 | 2026-10-25 16:23 | 2026-10-25 16:24 | 1 | 0 | grep de verificación de la fase (mismo tramo; mínimo 1 minuto) |
| F2.1 | 2026-10-25 16:39 | 2026-10-25 16:40 | 1 | 0 | `template/AGENTS.md`: `<prefijo>` y frase mínima (implementer, fase F2 en una invocación; mínimo 1 min por paso) |
| F2.2 | 2026-10-25 16:39 | 2026-10-25 16:40 | 1 | 0 | `.agents/README.md`: árbol con `<prefijo>` (implementer, fase F2 en una invocación; mínimo 1 min por paso) |
| F2.3 | 2026-10-25 16:39 | 2026-10-25 16:40 | 1 | 0 | skill `sdd`: frase mínima una vez, ejemplo sin número (implementer, fase F2 en una invocación; mínimo 1 min por paso) |
| F2.4 | 2026-10-25 16:39 | 2026-10-25 16:40 | 1 | 0 | skill `adr`: paso 2 y plantilla con `<prefijo>` (implementer, fase F2 en una invocación; mínimo 1 min por paso) |
| F2.5 | 2026-10-25 16:39 | 2026-10-25 16:40 | 1 | 0 | skill `git-commit`: revisión por valor numérico (implementer, fase F2 en una invocación; mínimo 1 min por paso) |
| F2.6 | 2026-10-25 16:39 | 2026-10-25 16:40 | 1 | 0 | skill `migracion-bd`: encabezado con `<prefijo_Feature>` (implementer, fase F2 en una invocación; mínimo 1 min por paso) |
| F2.7 | 2026-10-25 16:39 | 2026-10-25 16:40 | 1 | 0 | `sdd-spec` y 9 `argument-hint` (una línea cada uno) (implementer, fase F2 en una invocación; mínimo 1 min por paso) |
| F2.8 | 2026-10-25 16:39 | 2026-10-25 16:40 | 1 | 0 | `grep` de menciones de ancho: solo quedan las legítimas y la fila E12 (F3) (implementer, fase F2 en una invocación; mínimo 1 min por paso) |
| F3.1 | 2026-10-25 16:40 | 2026-10-25 16:40 | 1 | 0 | fila E12: duplicado por valor, ancho inválido y clon = FALLA; mezcla válida en proyecto = AVISO; E20 intacta (implementer, fase F3) |
| F3.2 | 2026-10-25 16:40 | 2026-10-25 16:40 | 1 | 0 | pasos 1 a 3 de « Cómo responder » con el tercer estado AVISO y el cierre con el número de avisos |
| F4.1 | 2026-10-25 16:41 | 2026-10-25 16:42 | 1 | 0 | `## Aviso de numeración` en `ACTUALIZAR.md`; casilla manual aprobada por el usuario; la espera de su revisión va como pausa (implementer, fase F4) |
| F4.2 | 2026-10-25 16:41 | 2026-10-25 16:42 | 1 | 0 | reporte y marcador al cerrar: el aviso va aparte, no es pendiente ni bloquea |
| F4.3 | 2026-10-25 16:41 | 2026-10-25 16:42 | 1 | 0 | registro «mantener» (clase `aviso`, huella de « Numeración única ») y caso límite 22 |
| F5.1 | 2026-10-25 16:42 | 2026-10-25 16:43 | 1 | 0 | `CHANGELOG.md` de 0.5.0: dos viñetas nuevas (numeración y aviso de `ACTUALIZAR.md`); `VERSION` y fecha sin cambio (implementer, fase F5) |
| F5.2 | 2026-10-25 16:42 | 2026-10-25 16:43 | 1 | 0 | clon: `AGENTS.md`, README de specs y de ADR de la raíz remiten a la fuente; sigue en 3 dígitos |
| F6.1 | 2026-10-25 16:44 | 2026-10-25 16:44 | 1 | 0 | `manifiestos/0.5.0.md` regenerado: 20 de 42 filas cambiadas, cabecera y orden intactos (implementer, fase F6; script temporal fuera del repo) |
| F6.2 | 2026-10-25 16:44 | 2026-10-25 16:44 | 1 | 0 | E20: 42 de 42 iguales, 0 diferencias; verificado por la sesión principal con un cálculo propio |
| F7.1 | 2026-10-25 16:46 | 2026-10-25 16:47 | 1 | 0 | ensayo de instalación: 42 de 42, solo marcadores permitidos (reviewer, fase F7; mínimo 1 min por paso) |
| F7.2 | 2026-10-25 16:46 | 2026-10-25 16:47 | 1 | 0 | 9 casos de numeración: todos dan lo del paso (reviewer, fase F7; mínimo 1 min por paso) |
| F7.3 | 2026-10-25 16:46 | 2026-10-25 16:47 | 1 | 0 | 5 casos de duplicados con remoto simulado `git init --bare`: todos OK (reviewer, fase F7; mínimo 1 min por paso) |
| F7.4 | 2026-10-25 16:46 | 2026-10-25 16:47 | 1 | 0 | aviso por nombres, «mantener», E12 en FALLA/AVISO/clon: todos OK (reviewer, fase F7; mínimo 1 min por paso) |
| F7.5 | 2026-10-25 16:46 | 2026-10-25 16:47 | 1 | 0 | E13, E20 42/42, contraste 0.4.0→0.5.0, lint, secretos: OK (reviewer, fase F7; mínimo 1 min por paso) |
| F8.1 | 2026-10-25 16:46 | 2026-10-25 16:49 | 1 | 0 | fuente: desempate de ancho (publicado; si ninguno, el mayor) y desbordamiento en el clon; fila E12: excepción de desbordamiento y mezcla más inválido = FALLA sin AVISO (implementer, fase F8) |
| F8.2 | 2026-10-25 16:46 | 2026-10-25 16:49 | 1 | 0 | `ACTUALIZAR.md` › «mantener»: límites del fragmento cuya huella identifica el aviso |
| F8.3 | 2026-10-25 16:46 | 2026-10-25 16:49 | 1 | 0 | F6 repetido: manifiesto regenerado una vez (solo cambiaron las filas de `template/.docs/002_specs/README.md` y `validar-estructura.md`); E20 42/42 |
| F8.4 | 2026-10-25 16:46 | 2026-10-25 16:49 | 1 | 0 | revalidación por reviewer: 9 casos de E12 y desempate OK; huella del aviso `9903c8f8…d213` con dos extracciones independientes; E20 42/42; fila E20 sin cambios |
| F7.6 | 2026-10-25 16:49 | 2026-10-25 16:50 | 1 | 0 | visto final del usuario (F7.6) |
| cierre | 2026-10-25 16:50 | 2026-10-25 16:50 | 1 | 0 | plan contra real, deuda 004 resuelta, propuestas de MEMORY y ADR |

## Plan contra real

Estimado y real en **minutos** (el plan estimó en minutos, no en horas). El real sale de la columna Minutos; con el mínimo de 1 minuto por paso la suma de la tabla supera el reloj.

| Fase | Estimado | Real | Delta |
|---|---|---|---|
| Documentación SDD | sin estimado | 37 min | — |
| F0 | 5 min | 2 min | -3 min |
| F1 | 15 min | 5 min | -10 min |
| F2 | 15 min | 8 min | -7 min |
| F3 | 6 min | 2 min | -4 min |
| F4 | 12 min | 3 min | -9 min |
| F5 | 6 min | 2 min | -4 min |
| F6 | 5 min | 2 min | -3 min |
| F7 | 25 min | 6 min | -19 min |
| F8 (añadida) | 10 min | 4 min | -6 min |

Documentación SDD = spec 25 + clarify 5 + plan 5 + tasks 1 + cierre 1 = 37 min (la spec incluye unos 14 min de espera por el límite de sesión de la API). Implementación F0 a F8 = 34 min reales contra 99 estimados.

Ajuste para el siguiente plan: en un feature que solo edita Markdown y se ejecuta con agentes, el real ronda un tercio del estimado; conviene estimar por fase en pocos minutos y reservar la validación (F7) para los casos que pueden fallar, no por tiempo. Las fases pequeñas (F0, F5, F6) se pueden unir en una sola invocación.
