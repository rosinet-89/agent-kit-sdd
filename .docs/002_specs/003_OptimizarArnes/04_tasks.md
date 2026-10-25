# 04 — Tareas: Optimizar el arnés SDD
Leyenda: `[ ]` Pendiente · `[~]` En curso · `[x]` Hecho · `[!]` Bloqueado
Spec: `01_spec.md` (aprobada) · Plan: `03_plan.md` · Total estimado: 4,0 h

Nada de esta lista está hecho. `/sdd-cierre` no es una fase ni un paso: corre cuando el usuario lo pida, sobre el resultado ya medido. El ADR de la fuente única por fase se propone en ese cierre; este archivo no lo crea. La deuda `003_ModeloYPermisosDeAgentes` ya existe: no se crea otra.

## Reglas comunes a todos los pasos
- **Lint (L).** Cuando un paso diga "Lint L en `<archivo>`", sobre cada archivo tocado (`AGENTS.md` › Comandos):
  1. `file -i <archivo>` da `charset=utf-8` (o `us-ascii`).
  2. `grep -l $'^\xEF\xBB\xBF' <archivo>` no devuelve nada (sin BOM).
  3. Si hay `name` en el frontmatter: en una skill es el nombre de su carpeta; en un agente es el nombre del archivo sin `.md`. Un comando no lleva `name`.
  4. Cada ruta citada existe. No aplica a `manifiestos/`: pueden listar rutas retiradas.
  5. En `template/`: sin marcadores `<<…>>` salvo `<<NOMBRE_PROYECTO>>` e `<<IDIOMA>>`.
- **Sin SQL.** Evidencia de datos: No aplica, salvo el paso que copia o mide bytes.
- **Verificación manual UI:** No aplica. No hay pantallas.
- **Quién edita.** Relee el archivo. Toca solo la parte de este feature. Si cambió desde la lectura anterior, para y avisa. Prohibido `git checkout` y `git restore`. No se revierte trabajo ajeno (RNF-4).
- **Qué no se edita.** `plantillas.md`, `ACTUALIZAR.md`, `INSTALAR.md`, ADR 007, ADR 008, la constitución, la carpeta `001_ActualizarKit`, el archivo de la deuda 003, la fila E20 y el qué de cada chequeo E1–E20. No se tocan `mode`, `permissions` ni `model`.
- **Cifras.** Las de `02_analisis_impacto.md` §3 se copian tal cual. No se sustituyen RNF-1 ni la línea base para hacer pasar un tope. Si un `wc -c` de una pieza de `template/` ya no coincide con §3.1, se anota la cifra nueva y se avisa antes de editar; la línea base copiada en F0 sigue siendo la de la comparación.
- **Dos listas de pasos.** `F0.1`…`F8.8` son los pasos de esta carpeta. `F6.2`, `F5.1`, `F1.5` y `F5.9` de `001_ActualizarKit` son el caso medido: en F0, F5.5 y F8.7 se nombran "paso de referencia de `001`". El `F5.1` y el `F6.2` de esta lista son otros: editan `implementer.md` y el `AGENTS.md` de la raíz.
- **Cada pieza editada** declara qué lee, qué hace y qué no hace (RNF-6). El procedimiento de la fase queda en su comando; el resto solo lo nombra y enlaza.
- **Ensayos** solo en `mktemp -d`. Nunca en un proyecto real. Sin `git commit`, `push`, `pull` ni `fetch`.

## Orden
F0 antes de cualquier edición de `template/` o de docs de la raíz. F1 antes de F2: sin F1.1 no hay F2. F2 antes de F3 y de F4. F3 y F4.1–F4.8 después de F2. F5.1–F5.4 después de F3 y de F4.1–F4.8. F4.9 cuando esos comandos y los cuatro agentes ya no repiten la secuencia. F5.5 al terminar F5. F6 después de F5.1–F5.4 y antes de F8. F7 antes de F8. F8 al final. Una edición de `template/` posterior a F8 obliga a repetir F8.

---

## F0 — Línea base en el avance (estimado: 0,25 h)
Lo hace la sesión principal, no un subagente. No edita `template/` ni docs de la raíz.
- [x] **F0.1 Copiar la línea base de `02` §3 en `05_avance.md`.** RF-23
  - Hacer: en `.docs/002_specs/003_OptimizarArnes/05_avance.md`, copiar sin recalcular ni redondear las tablas de §3.1 (piezas), §3.2 (caso congelado `001_ActualizarKit`), las filas de los tres pasos de referencia de §3.3 y su línea base de §3.4, la suma de §3.5 y la tabla de §3.6 (arranque de los otros roles y comandos). Fecha de origen de esas cifras: 2026-10-25, `wc -c`. Deben quedar escritas, entre otras, estas:
    - Pasos de referencia de `001` (no son los de esta lista): `F6.2` mínima 834 y línea base 92 280; `F5.1` mínima 15 181 y línea base 89 004; `F1.5` mínima 21 248 y línea base 89 004. `F5.9` de `001` mínima 1 452, sin RF y sin sección de plan (no es paso de referencia).
    - `/sdd-status`: 122 731. Tope de la mediana: 26 701. Tope de status: 36 819.
    - Caso: `01_spec.md` 22 394, `02_analisis_impacto.md` 13 954, `03_plan.md` 16 330, `04_tasks.md` 29 604, `05_avance.md` 24 928, `06_flujo.md` 13 796, `MEMORY.md` 1 067. Secciones de ese avance: PUNTO DE RETOMO 2 478, Pasos 45, Pruebas 11 541, Tiempos 4 234. Línea `Estado` 21.
    - Arranque §3.6: `coordinator` 34 533, `planner` 35 063, `reviewer` 75 284, `/sdd` 21 498, `/sdd-spec` 21 686, `/sdd-implement` (solo el comando) 64 536, `/sdd-estructura` 12 194, `/sdd-constitucion` 47 364, `/sdd-init` 11 564; comandos que solo aportan su archivo: plan 1 350, tasks 903, clarify 924, validate 1 064, flujo 884, cierre 1 614, change 783.
    - El `AGENTS.md` de la raíz (8 667) no entra en la línea base.
  - Hecho cuando:
    - [x] Esas cifras aparecen en `05_avance.md` iguales a §3 (espacio de millar incluido)
    - [x] `template/` y los docs de la raíz no cambiaron en este paso
    - [x] Lint L (puntos 1 y 2; el 3 no aplica) en `05_avance.md`
    - [x] Evidencia de datos: No aplica
    - [x] Verificación manual UI: No aplica

## F1 — Puerta de coordinación (estimado: 0,25 h)
Requisito: F0 cerrada. No está hecha. El punto de retomo actual no es esta confirmación.
- [x] **F1.1 Entrada fechada del usuario antes de editar.** RNF-4
  - Hacer: la sesión principal, no un subagente, deja en `05_avance.md` una entrada con fecha y hora. La escribe el usuario, o la sesión con su confirmación explícita. Tiene que decir (a) con quién del equipo se coordinó la edición en paralelo, (b) quién commitea `001`, `002` y `003`, y (c) el estado del árbol (qué archivos tienen cambios ajenos). Sin esa entrada no se abre F2. La implementación parte del contenido actual y no revierte trabajo ajeno.
  - Hecho cuando:
    - [x] La entrada tiene fecha y hora y cubre (a), (b) y (c); la confirmación es del usuario
    - [x] Sigue en `[ ]` hasta esa confirmación; este archivo no la marca hecha
    - [ ] Lint L (puntos 1 y 2) en `05_avance.md`
    - [ ] Evidencia de datos: No aplica
    - [ ] Verificación manual UI: No aplica

## F2 — Contrato de tiempos y tabla de la skill (estimado: 0,5 h)
Requisito: F0 y F1.1 cerradas.
- [x] **F2.1 Contrato en `avance-y-tiempos.md`.** RF-16, RF-22
  - Hacer: en `template/.agents/skills/sdd/avance-y-tiempos.md`, dejar como dueño único las reglas 1 a 4 de RF-16: (1) pausa de más de 30 minutos y filas `pausa por confirmar`; (2) formato de la fila de tiempo (inicio, fin, minutos); (3) solo la sesión principal escribe `05_avance.md`; (4) leyenda de estados de los pasos. RF-22, para un feature abierto desde esta versión: inicio y fin reales, mínimo 1 minuto por paso real, prohibido el lote de filas con 0 minutos. La única excepción es la frase exacta "sin trabajo real" en la columna Nota. `001_ActualizarKit` sigue válido con sus filas actuales; ninguna comprobación le exige reescribirlas. La regla 5 (arranque de sesión) no vive aquí: su dueño es `AGENTS.md` (F6).
  - Hecho cuando:
    - [x] Las cuatro reglas y la frase "sin trabajo real" están en este archivo y no se copian como segunda definición
    - [x] Un feature ya cerrado no queda rechazado por filas viejas
    - [x] Se releyó el archivo; si cambió, se paró y se avisó; el diff no revierte trabajo ajeno
    - [x] Lint L en `template/.agents/skills/sdd/avance-y-tiempos.md` (`name` = `sdd` no aplica a este archivo: no lleva `name`; el `name` de la skill sigue en `SKILL.md`)
    - [x] Evidencia de datos: No aplica
    - [x] Verificación manual UI: No aplica
- [x] **F2.2 La skill `sdd` queda en tabla de asociación.** RF-14, RF-15, RF-16, RF-18
  - Hacer: en `template/.agents/skills/sdd/SKILL.md`, la tabla de fases solo asocia fase, documento y rol, con enlace al comando `/sdd-<fase>`. No reescribe la secuencia de ninguna fase. La fila de estructura dice que la ejecuta la sesión principal. La lista de comandos del flujo incluye `sdd-estructura`. Donde la skill mencione el cierre, remite al comando y conserva que todo feature revisa deuda y ADR. La numeración que siga citada cumple E12: 4 dígitos para specs, deudas y ADR de un proyecto; las carpetas de sección siguen en 3. No se reescribe el chequeo E12. Las reglas de tiempo se enlazan al contrato (F2.1), no se copian.
  - Hecho cuando:
    - [x] La tabla no contiene la secuencia de pasos de una fase, solo fase, documento, rol y enlace
    - [x] La fila de estructura nombra a la sesión principal
    - [x] `sdd-estructura` aparece en la lista de comandos de la skill
    - [x] Se releyó el archivo; si cambió, se paró y se avisó; el diff no revierte trabajo ajeno
    - [x] Lint L en `template/.agents/skills/sdd/SKILL.md` (`name` = `sdd`)
    - [x] Evidencia de datos: No aplica
    - [x] Verificación manual UI: No aplica

## F3 — Comandos de lectura y de tope (estimado: 0,5 h)
Requisito: F2 cerrada. Cada comando es un paso. Cada uno dice qué lee, qué hace y qué no hace.
- [x] **F3.1 `sdd.md`: elegir el comando con lo mínimo.** RF-8, RF-15
  - Hacer: en `template/.agents/commands/sdd.md`, elegir el comando con lo mínimo para decidirlo. Cargar la skill `sdd` y el contrato de avance solo si la fase elegida los usa. Para `status` o `estructura`, no se cargan. La lista del punto de entrada incluye `sdd-estructura`. No reescribe el procedimiento de las fases.
  - Hecho cuando:
    - [x] El texto dice que `status` y `estructura` no cargan la skill ni el contrato
    - [x] La lista incluye `sdd-estructura` y no es la secuencia de otra fase
    - [x] Declara qué lee, qué hace y qué no hace
    - [x] Se releyó el archivo; si cambió, se paró y se avisó; el diff no revierte trabajo ajeno
    - [x] Lint L en `template/.agents/commands/sdd.md`
    - [x] Evidencia de datos: No aplica
    - [x] Verificación manual UI: No aplica
- [x] **F3.2 `sdd-status.md`: la lectura de RF-5.** RF-5
  - Hacer: en `template/.agents/commands/sdd-status.md`, leer solo la línea `Estado` de `01_spec.md`, la lista de documentos presentes de la carpeta, las secciones PUNTO DE RETOMO, Pasos, Pruebas y Tiempos de `05_avance.md`, y el conteo de pasos de `04_tasks.md` por búsqueda de identificadores. Con eso entrega fase y Estado; punto de retomo; pasos hechos y pendientes (x de y); verificaciones manuales pendientes; horas acumuladas y filas con `pausa por confirmar`; y el siguiente paso con su comando `/sdd-*`. No lee `01`–`06` completos ni `MEMORY.md`, salvo que el usuario pida detalle de uno. Si no hay `05_avance.md`, deduce la fase por los documentos presentes y el `Estado` de la spec. No entra el archivo de tareas entero.
  - Hecho cuando:
    - [x] El texto nombra esas lecturas y los seis datos de la respuesta, y excluye el resto
    - [x] El conteo es por búsqueda de identificadores, no por leer `04_tasks.md` entero
    - [x] Declara qué lee, qué hace y qué no hace
    - [x] Se releyó el archivo; si cambió, se paró y se avisó; el diff no revierte trabajo ajeno
    - [x] Lint L en `template/.agents/commands/sdd-status.md`
    - [x] Evidencia de datos: No aplica
    - [x] Verificación manual UI: No aplica
- [x] **F3.3 `sdd-implement.md`: lectura mínima y respaldo.** RF-1, RF-2, RF-16
  - Hacer: en `template/.agents/commands/sdd-implement.md`, indicar al `implementer` la lectura mínima de la spec (el paso `Fn.m` con su "Hecho cuando", los RF que cita, la parte del plan que lo cubre, las secciones de `AGENTS.md` que el rol usa, y de la constitución y de la skill `sdd` solo las secciones que el paso o el plan citan). Un documento de menos de 4 096 bytes puede leerse entero. Si el paso no cita ningún RF, lee la fase `Fn` y avisa. Si el plan no tiene sección que cubra el paso, lee el plan completo y avisa. Ampliar solo dentro de la lista cerrada, con tope de 3 secciones; el motivo se devuelve a la sesión principal, que lo anota. Este comando no escribe `05_avance.md`. La pausa y el formato de la fila se enlazan al contrato (F2.1), no se reescriben.
  - Hecho cuando:
    - [x] Están la lectura mínima, el umbral de 4 096 bytes, los dos respaldos con aviso y el tope de 3
    - [x] El comando no escribe `05_avance.md` y no copia las reglas de tiempo
    - [x] Declara qué lee, qué hace y qué no hace
    - [x] Se releyó el archivo; si cambió, se paró y se avisó; el diff no revierte trabajo ajeno
    - [x] Lint L en `template/.agents/commands/sdd-implement.md`
    - [x] Evidencia de datos: No aplica
    - [x] Verificación manual UI: No aplica
- [x] **F3.4 `sdd-estructura.md`: lo corre la sesión principal.** RF-18
  - Hacer: en `template/.agents/commands/sdd-estructura.md`, la sesión principal ejecuta el comando, sin lanzar un subagente. Las comprobaciones E1 a E20 las hace ella. No manda ninguna a un subagente ni al `reviewer`. No cambia qué comprueba cada una ni la definición de E20.
  - Hecho cuando:
    - [x] El comando no lanza subagente y no asigna E1–E20 al `reviewer`
    - [x] E20 no se redefine en este archivo
    - [x] Declara qué lee, qué hace y qué no hace
    - [x] Se releyó el archivo; si cambió, se paró y se avisó; el diff no revierte trabajo ajeno
    - [x] Lint L en `template/.agents/commands/sdd-estructura.md`
    - [x] Evidencia de datos: No aplica
    - [x] Verificación manual UI: No aplica
- [x] **F3.5 `sdd-init.md`: declarar el tope.** RF-10
  - Hacer: en `template/.agents/commands/sdd-init.md`, declarar el tope de inspección: 30 archivos o 150 KB acumulados, lo que ocurra primero, y pedir confirmación para ampliarlo. Este comando no tiene meta de reducción del 70 %.
  - Hecho cuando:
    - [x] El tope queda escrito en este comando con las dos cifras y la confirmación para ampliar
    - [x] No aparece una meta del 70 %
    - [x] Declara qué lee, qué hace y qué no hace
    - [x] Se releyó el archivo; si cambió, se paró y se avisó; el diff no revierte trabajo ajeno
    - [x] Lint L en `template/.agents/commands/sdd-init.md`
    - [x] Evidencia de datos: No aplica
    - [x] Verificación manual UI: No aplica
- [x] **F3.6 `sdd-constitucion.md`: referenciar el tope.** RF-10
  - Hacer: en `template/.agents/commands/sdd-constitucion.md`, referenciar el tope declarado en `sdd-init.md` (30 archivos o 150 KB). No repetir otra cifra. Sin meta del 70 %.
  - Hecho cuando:
    - [x] Remite a `sdd-init.md` y no declara un tope distinto
    - [x] No aparece una meta del 70 %
    - [x] Declara qué lee, qué hace y qué no hace
    - [x] Se releyó el archivo; si cambió, se paró y se avisó; el diff no revierte trabajo ajeno
    - [x] Lint L en `template/.agents/commands/sdd-constitucion.md`
    - [x] Evidencia de datos: No aplica
    - [x] Verificación manual UI: No aplica

## F4 — Resto de comandos, cada uno fuente de su fase (estimado: 0,5 h)
Requisito: F2 cerrada. F4.9 espera además a F3 y a F5.1–F5.4. Cada comando conserva su procedimiento y deja de copiar plantillas, tiempos y arranque.
- [x] **F4.1 `sdd-spec.md`.** RF-9, RF-14, RF-16
  - Hacer: el procedimiento de `spec` queda en `template/.agents/commands/sdd-spec.md`. La sesión principal retiene constitución y memoria y los pasa en la delegación, o indica al `planner` que no los relea. Si el `planner` necesita algo que no se le entregó, lo lee de forma acotada. Los tiempos se enlazan al contrato, no se copian.
  - Hecho cuando:
    - [x] La cadena lee constitución y memoria una vez, y el comando no reescribe el contrato
    - [x] Declara qué lee, qué hace y qué no hace
    - [x] Se releyó el archivo; si cambió, se paró y se avisó; el diff no revierte trabajo ajeno
    - [x] Lint L en `template/.agents/commands/sdd-spec.md`
    - [x] Evidencia de datos: No aplica
    - [x] Verificación manual UI: No aplica
- [x] **F4.2 `sdd-clarify.md`.** RF-14, RF-20
  - Hacer: en `template/.agents/commands/sdd-clarify.md`, el procedimiento de `clarify` queda aquí. Si la spec no tiene seguimiento en Git, o el proyecto no usa Git, mostrar el contenido modificado por otro medio. No depender de `git diff`.
  - Hecho cuando:
    - [x] Hay un medio distinto de `git diff` para el archivo sin seguimiento y para el proyecto sin Git
    - [x] Declara qué lee, qué hace y qué no hace
    - [x] Se releyó el archivo; si cambió, se paró y se avisó; el diff no revierte trabajo ajeno
    - [x] Lint L en `template/.agents/commands/sdd-clarify.md`
    - [x] Evidencia de datos: No aplica
    - [x] Verificación manual UI: No aplica
- [x] **F4.3 `sdd-plan.md`.** RF-14, RF-17
  - Hacer: en `template/.agents/commands/sdd-plan.md`, el procedimiento de `plan` queda aquí. El contenido del documento se referencia en `plantillas.md`; no se copia.
  - Hecho cuando:
    - [x] Apunta a `template/.agents/skills/sdd/plantillas.md` y no repite la plantilla
    - [x] `plantillas.md` no se modificó
    - [x] Declara qué lee, qué hace y qué no hace
    - [x] Se releyó el archivo; si cambió, se paró y se avisó; el diff no revierte trabajo ajeno
    - [x] Lint L en `template/.agents/commands/sdd-plan.md`
    - [x] Evidencia de datos: No aplica
    - [x] Verificación manual UI: No aplica
- [x] **F4.4 `sdd-tasks.md`.** RF-14, RF-17
  - Hacer: en `template/.agents/commands/sdd-tasks.md`, el procedimiento de `tasks` queda aquí. El contenido del documento se referencia en `plantillas.md`; no se copia.
  - Hecho cuando:
    - [x] Apunta a `template/.agents/skills/sdd/plantillas.md` y no repite la plantilla
    - [x] `plantillas.md` no se modificó
    - [x] Declara qué lee, qué hace y qué no hace
    - [x] Se releyó el archivo; si cambió, se paró y se avisó; el diff no revierte trabajo ajeno
    - [x] Lint L en `template/.agents/commands/sdd-tasks.md`
    - [x] Evidencia de datos: No aplica
    - [x] Verificación manual UI: No aplica
- [x] **F4.5 `sdd-validate.md`.** RF-3, RF-11, RF-14, RF-19
  - Hacer: en `template/.agents/commands/sdd-validate.md`, el procedimiento de `validate` queda aquí. El `reviewer` lee el "Hecho cuando" del paso y los criterios de los RF citados, no las tareas ni la spec completas. El alcance es el de RF-19: archivos citados en avance, plan y tareas, más los archivos nuevos sin seguimiento (los lista `git status`, no `git diff`) que estén en la carpeta del feature o citados. Todo otro archivo modificado o sin seguimiento se informa como "ajeno o no atribuible" sin revisarlo como propio. Si un archivo mezcla cambios del feature y ajenos, se avisa. Sin Git, el alcance es solo lo citado. La seguridad referencia el modo de `seguridad-basica` (F6.3); no copia la regla. Hasta que F6.3 exista, el enlace queda señalado y se comprueba al cerrar F6.3.
  - Hecho cuando:
    - [x] El alcance usa citados + nuevos sin seguimiento, y nombra "ajeno o no atribuible"
    - [x] La regla de los dos alcances no está copiada: hay un enlace a la skill
    - [x] Declara qué lee, qué hace y qué no hace
    - [x] Se releyó el archivo; si cambió, se paró y se avisó; el diff no revierte trabajo ajeno
    - [x] Lint L en `template/.agents/commands/sdd-validate.md`
    - [x] Evidencia de datos: No aplica
    - [x] Verificación manual UI: No aplica
- [x] **F4.6 `sdd-flujo.md`.** RF-6, RF-14, RF-19
  - Hacer: en `template/.agents/commands/sdd-flujo.md`, el procedimiento de `flujo` queda aquí. El `planner` lee spec, plan, avance y el alcance acotado de RF-19, no todo el contexto del proyecto. El mismo criterio de ajenos que F4.5.
  - Hecho cuando:
    - [x] La lectura del `planner` para este documento está declarada y no es el contexto entero
    - [x] El alcance de la revisión es el de RF-19
    - [x] Declara qué lee, qué hace y qué no hace
    - [x] Se releyó el archivo; si cambió, se paró y se avisó; el diff no revierte trabajo ajeno
    - [x] Lint L en `template/.agents/commands/sdd-flujo.md`
    - [x] Evidencia de datos: No aplica
    - [x] Verificación manual UI: No aplica
- [x] **F4.7 `sdd-cierre.md`.** RF-4, RF-14, RF-15, RF-16
  - Hacer: en `template/.agents/commands/sdd-cierre.md`, localizar por búsqueda de patrón, en la tabla de Pasos de `05_avance.md`, los pasos cuyo estado no sea `[x]`. Un `[ ]`, `[~]` o `[!]` impide el cierre, salvo que el usuario lo difiera y quede registrado, con propuesta de deuda. Las verificaciones manuales pendientes se detectan en Pruebas y PUNTO DE RETOMO. No leer las tareas completas. Se conservan las comprobaciones de deuda, ADR e índices, para todo feature. Los tiempos se enlazan al contrato.
  - Hecho cuando:
    - [x] El cierre busca estados distintos de `[x]` en `05_avance.md` y no pide las tareas enteras
    - [x] Deuda y ADR siguen en el cierre de todo feature
    - [x] Declara qué lee, qué hace y qué no hace
    - [x] Se releyó el archivo; si cambió, se paró y se avisó; el diff no revierte trabajo ajeno
    - [x] Lint L en `template/.agents/commands/sdd-cierre.md`
    - [x] Evidencia de datos: No aplica
    - [x] Verificación manual UI: No aplica
- [x] **F4.8 `sdd-change.md`.** RF-14, RF-16
  - Hacer: en `template/.agents/commands/sdd-change.md`, el procedimiento del cambio queda aquí. El formato de la fila de tiempo se enlaza al contrato; no se copia.
  - Hecho cuando:
    - [x] No repite el formato de tiempos ni la secuencia de otra fase
    - [x] Declara qué lee, qué hace y qué no hace
    - [x] Se releyó el archivo; si cambió, se paró y se avisó; el diff no revierte trabajo ajeno
    - [x] Lint L en `template/.agents/commands/sdd-change.md`
    - [x] Evidencia de datos: No aplica
    - [x] Verificación manual UI: No aplica
- [x] **F4.9 Tabla fase → fuente única → piezas que solo enlazan.** RF-14, RF-15
  - Hacer: la sesión principal, no un subagente, anota en `05_avance.md` la tabla de cada fase: comando que es la fuente, y piezas que solo la nombran y enlazan (skill, coordinator, agente del rol). Comprobar por lectura cruzada que en ninguna otra pieza aparece la secuencia de pasos de esa fase. Incluye las fases de F3 y de F4. La numeración de un proyecto en `template/` no cita 3 dígitos para specs, deudas y ADR, salvo el texto ya existente del chequeo E12, que no se edita. Este paso se ejecuta cuando F3 y F5.1–F5.4 ya cerraron.
  - Hecho cuando:
    - [x] La tabla está en `05_avance.md` y cada fase tiene una sola fuente
    - [x] La lectura cruzada no encuentra otra secuencia, solo nombre y enlace
    - [x] Lint L (puntos 1 y 2) en `05_avance.md`
    - [x] Evidencia de datos: No aplica
    - [x] Verificación manual UI: No aplica

## F5 — Los cuatro agentes (estimado: 0,5 h)
Requisito: F3 y F4.1–F4.8 cerradas. El agente enlaza el comando; no lo reescribe. No repite el arranque de sesión (enlaces, prefijos repetidos, preferencias). No se cambia el frontmatter de permisos ni de modelo.
- [x] **F5.1 `implementer.md`.** RF-1, RF-2, RF-12, RF-13, RF-21
  - Hacer: en `template/.agents/agents/implementer.md`, declarar qué lee (la lectura mínima que define `sdd-implement.md`), qué hace (un paso) y qué no hace: no registra en Git, no escribe `05_avance.md`, devuelve el motivo de cada ampliación y los avisos de RF-1. Nombra las secciones de `AGENTS.md` que usa (Comandos y convenciones de código). No supone que la herramienta inyecta `AGENTS.md`.
  - Hecho cuando:
    - [x] No reescribe el procedimiento de `sdd-implement.md`; lo enlaza
    - [x] Declara las secciones de `AGENTS.md`, y que no hace el arranque de sesión ni el commit
    - [x] `mode`, `permissions` y `model` quedaron como estaban
    - [x] Se releyó el archivo; si cambió, se paró y se avisó; el diff no revierte trabajo ajeno
    - [x] Lint L en `template/.agents/agents/implementer.md` (`name` = `implementer`)
    - [x] Evidencia de datos: No aplica
    - [x] Verificación manual UI: No aplica
- [x] **F5.2 `reviewer.md`.** RF-3, RF-11, RF-12, RF-18, RF-19, RF-21
  - Hacer: en `template/.agents/agents/reviewer.md`, leer el "Hecho cuando" del paso o fase y los criterios de aceptación de los RF que el paso cita, no las tareas ni la spec completas. Las verificaciones de RF-24 se conservan. No ejecuta E1–E20. Aplica el alcance de RF-19. Clasifica el cambio y referencia el modo de `seguridad-basica` (completo o "solo secretos"); ante duda, la skill completa. La búsqueda de secretos sigue siempre. Declara que no corrige código. Nombra las secciones de `AGENTS.md` que usa.
  - Hecho cuando:
    - [x] No hay una instrucción que le mande un chequeo E1–E20
    - [x] La clasificación de seguridad enlaza la skill y no copia la regla de los dos alcances
    - [x] Declara qué lee, qué hace y qué no hace (no corrige código)
    - [x] `mode`, `permissions` y `model` quedaron como estaban
    - [x] Se releyó el archivo; si cambió, se paró y se avisó; el diff no revierte trabajo ajeno
    - [x] Lint L en `template/.agents/agents/reviewer.md` (`name` = `reviewer`)
    - [x] Evidencia de datos: No aplica
    - [x] Verificación manual UI: No aplica
- [x] **F5.3 `planner.md`.** RF-6, RF-9, RF-12, RF-13, RF-17
  - Hacer: en `template/.agents/agents/planner.md`, declarar la lectura por documento: para tareas, spec, plan, plantilla y `AGENTS.md` › Comandos; para flujo, spec, plan, avance y el alcance de RF-19. No leer todo el contexto del proyecto en cada documento. En la cadena de spec, no releer constitución ni memoria si la sesión principal ya los retuvo. Plan y tasks apuntan a `plantillas.md`. Nombra las secciones de `AGENTS.md` que usa. No hace el arranque de sesión.
  - Hecho cuando:
    - [x] Cada documento que redacta tiene su lista de lectura, no el contexto entero
    - [x] La cadena de spec no relee lo ya entregado
    - [x] Declara qué lee, qué hace y qué no hace
    - [x] `mode`, `permissions` y `model` quedaron como estaban
    - [x] Se releyó el archivo; si cambió, se paró y se avisó; el diff no revierte trabajo ajeno
    - [x] Lint L en `template/.agents/agents/planner.md` (`name` = `planner`)
    - [x] Evidencia de datos: No aplica
    - [x] Verificación manual UI: No aplica
- [x] **F5.4 `coordinator.md`.** RF-7, RF-8, RF-14, RF-15, RF-16
  - Hacer: en `template/.agents/agents/coordinator.md`, enrutar: leer solo lo necesario para decidir y delegar. No leer la constitución ni `MEMORY.md` para dirigir el flujo. El índice de specs solo para ubicar una carpeta. No reescribir fases: enlazar el comando. Para `status` o `estructura`, no pedir la skill ni el contrato. La lista incluye `sdd-estructura`. La estructura la ejecuta la sesión principal. Los tiempos se enlazan al contrato.
  - Hecho cuando:
    - [x] No queda una secuencia de fase reescrita en este agente
    - [x] No lee constitución ni `MEMORY.md` para enrutar
    - [x] Declara qué lee, qué hace y qué no hace
    - [x] `mode`, `permissions` y `model` quedaron como estaban
    - [x] Se releyó el archivo; si cambió, se paró y se avisó; el diff no revierte trabajo ajeno
    - [x] Lint L en `template/.agents/agents/coordinator.md` (`name` = `coordinator`)
    - [x] Evidencia de datos: No aplica
    - [x] Verificación manual UI: No aplica
- [x] **F5.5 Comprobar el tope de RNF-1.** RF-1, RF-5, RF-23, RNF-1
  - Hacer: medir, sin editar `template/` ni la línea base. Si la mediana no cabe, parar y volver al usuario. No se cambia RNF-1, no se cambian 15 181, 26 701, 89 004, 92 280 ni 36 819.
    - Mediana, bloqueante: bytes de `template/.agents/agents/implementer.md` + bytes de `template/.agents/commands/sdd-implement.md` + 15 181 ≤ 26 701.
    - `/sdd-status`: bytes del comando nuevo + 21 (línea `Estado` congelada) + 2 478 + 45 + 11 541 + 4 234 (cuatro secciones congeladas) + los bytes de las líneas que devuelva la búsqueda de identificadores en `04_tasks.md` de `001`. Ese total ≤ 36 819. No entra el archivo de tareas entero ni `MEMORY.md`. Si ya no cabe, parar.
    - Paso de referencia `F1.5` de `001`: los dos archivos anteriores + 21 248 ≤ 89 004. Si no cabe, parar.
    - Paso de referencia `F6.2` de `001`: no usar la mínima 834. Sumar el respaldo de RF-1: plan congelado 16 330 + fase F6 de las tareas de `001` congelada 1 635 + constitución congelada 1 111 + el tamaño actual de las secciones de `template/AGENTS.md` que el rol usa + el tamaño actual de `seguridad-basica` + los dos archivos del agente y del comando. Ese total ≤ 92 280. Si no cabe, parar.
    - Paso `F5.9` de `001`: el mismo respaldo (fase F5 de esas tareas en lugar de F6), se informa y no es paso de referencia.
  - Hecho cuando:
    - [x] La mediana cumple ≤ 26 701 con la fórmula de arriba, o el paso quedó bloqueado y se volvió al usuario
    - [x] Status y los pasos de referencia `F1.5` y `F6.2` de `001` quedaron anotados con su suma; si alguno supera su tope, se paró
    - [x] RNF-1 y la línea base de F0 no se editaron
    - [x] Lint L: No aplica (no se escribe un archivo del kit)
    - [x] Evidencia de datos: No aplica
    - [x] Verificación manual UI: No aplica

## F6 — Los dos `AGENTS.md`, secretos y Alcance (estimado: 0,5 h)
Requisito: F5.1–F5.4 cerradas. Puede medirse F5.5 antes o después de esta fase; la medición que vale es la de F8.
- [x] **F6.1 `template/AGENTS.md`: secciones por rol.** RF-12, RF-13, RF-15, RF-16
  - Hacer: en `template/AGENTS.md`, cada rol declara qué secciones lee. Un subagente no ejecuta el arranque de sesión que la sesión principal ya hizo (regla 5 de RF-16; el dueño es este archivo). Lo genérico de este cambio es el mismo texto que en el `AGENTS.md` de la raíz (F6.2). Se conserva la diferencia de E12: 4 dígitos en un proyecto. No se copia a `template/` texto que solo existe por el modo mantenimiento del clon. Siguen `<<NOMBRE_PROYECTO>>` e `<<IDIOMA>>`. El grupo B llega a un proyecto por fusión manual; este paso no cambia el procedimiento de actualización.
  - Hecho cuando:
    - [x] Cada rol tiene nombradas sus secciones, y el arranque de sesión no se asigna al subagente
    - [x] E12 no se reescribió; los marcadores permitidos siguen
    - [x] Se releyó el archivo; si cambió, se paró y se avisó; el diff no revierte trabajo ajeno
    - [x] Lint L en `template/AGENTS.md` (sin `name`)
    - [x] Evidencia de datos: No aplica
    - [x] Verificación manual UI: No aplica
- [x] **F6.2 `AGENTS.md` de la raíz, alineado en lo genérico.** RF-12, RF-13, RF-15, RF-16
  - Hacer: en `AGENTS.md` de la raíz, el mismo texto genérico que F6.1 (secciones por rol y arranque que no repite el subagente). La diferencia que se conserva es la de E12: 3 dígitos en este clon, 4 en un proyecto, carpetas de sección en 3. No se borra lo que es solo del modo mantenimiento, ni se copia eso a `template/`. Antes de editar, contrastar los dos archivos y limitarse a los párrafos de este feature.
  - Hecho cuando:
    - [x] Los párrafos de este feature coinciden con `template/AGENTS.md` en lo genérico
    - [x] La frase de ancho de este clon sigue en 3, y el modo mantenimiento de la raíz sigue
    - [x] Se releyó el archivo; si cambió, se paró y se avisó; el diff no revierte trabajo ajeno
    - [x] Lint L (puntos 1, 2 y 4) en `AGENTS.md` (no está en `template/`: el punto 5 no aplica)
    - [x] Evidencia de datos: No aplica
    - [x] Verificación manual UI: No aplica
- [x] **F6.3 Modo "solo secretos" en `seguridad-basica`.** RF-11
  - Hacer: en `template/.agents/skills/seguridad-basica/SKILL.md`, declarar una sola vez el modo "solo secretos" y cuándo aplica la skill completa. Si el cambio es solo documentación, solo la verificación de secretos, y se dice. Si toca código ejecutable (definición de la spec), datos, accesos o secretos, la skill completa. Ante duda, la skill completa. La búsqueda de secretos aplica siempre. `sdd-validate.md` y `reviewer.md` la referencian y no la repiten; si F4.5 o F5.2 aún copian la regla, se quita de ahí en este paso, sin reabrir el resto del archivo. RF-16 no se escribe en esta skill.
  - Hecho cuando:
    - [x] El modo "solo secretos" y la skill completa están definidos una sola vez, en esta skill
    - [x] `sdd-validate.md` y `reviewer.md` solo enlazan
    - [x] `name` sigue siendo `seguridad-basica`
    - [x] Se releyó el archivo; si cambió, se paró y se avisó; el diff no revierte trabajo ajeno
    - [x] Lint L en `template/.agents/skills/seguridad-basica/SKILL.md`
    - [x] Evidencia de datos: No aplica
    - [x] Verificación manual UI: No aplica
- [x] **F6.4 Una frase de Alcance en `validar-estructura.md`.** RF-18
  - Hacer: en `template/.agents/validar-estructura.md`, una frase en la sección Alcance: la sesión principal corre E1–E20. No se edita la fila E20, ni el qué de E1–E19, ni se reescribe el checklist fila por fila. El checklist, `sdd-estructura.md` y `reviewer.md` coinciden en quién ejecuta.
  - Hecho cuando:
    - [x] Alcance tiene esa frase y la fila E20 está igual que antes de este paso (`diff` limitado a Alcance)
    - [x] Ninguna instrucción manda E1–E20 a un subagente
    - [x] Se releyó el archivo; si cambió, se paró y se avisó; el diff no revierte trabajo ajeno
    - [x] Lint L en `template/.agents/validar-estructura.md`
    - [x] Evidencia de datos: No aplica
    - [x] Verificación manual UI: No aplica

## F7 — Una línea en el changelog (estimado: 0,25 h)
Requisito: F2 a F6 cerradas, para que la línea describa el cambio ya hecho.
- [x] **F7.1 Una línea en la entrada 0.5.0 de `CHANGELOG.md`.** Criterio de finalización
  - Hacer: agregar una sola viñeta bajo `## 0.5.0 — 2026-10-24` en `CHANGELOG.md`. Describe este feature (menos lectura por rol y comando, una fuente por fase) y que el cambio de redacción en `AGENTS.md` llega por fusión manual. No abre una versión nueva, no cambia la fecha de la entrada y no publica.
  - Hecho cuando:
    - [x] Hay una viñeta nueva en 0.5.0 y no hay entrada 0.5.1
    - [x] `VERSION` no se modificó
    - [x] Se releyó el archivo; si cambió, se paró y se avisó; el diff no revierte trabajo ajeno
    - [x] Lint L (puntos 1 y 2) en `CHANGELOG.md`
    - [x] Evidencia de datos: No aplica
    - [x] Verificación manual UI: No aplica

## F8 — Congelar, manifiesto y medición (estimado: 0,75 h)
Requisito: F0 a F7 cerradas. Si `template/` cambia después de F8.1, se repite F8 desde F8.1. No se edita el algoritmo de `ACTUALIZAR.md` ni ADR 007 ni ADR 008.
- [x] **F8.1 Congelar `template/`.** RF-23
  - Hacer: anotar `find template -type f | wc -l` y el estado de `template/`. A partir de aquí no se edita `template/` salvo para repetir esta fase.
  - Hecho cuando:
    - [x] El conteo y el estado quedaron anotados en la respuesta
    - [x] Lint L: No aplica
    - [x] Evidencia de datos: No aplica
    - [x] Verificación manual UI: No aplica
- [x] **F8.2 Regenerar `manifiestos/0.5.0.md`.** Criterio de finalización
  - Hacer: regenerar `manifiestos/0.5.0.md` sobre el árbol de trabajo de `template/` (no sobre `HEAD`), con el mismo formato (título, una frase de uso, tabla `Ruta | Huella`). Algoritmo de `ACTUALIZAR.md`, sin cambiarlo: quitar el BOM `EF BB BF`, CRLF a LF, quitar espacios y tabuladores al final de cada línea, quitar líneas vacías finales y terminar con un solo LF, SHA-256 en hexadecimal minúscula. Rutas relativas a la raíz del proyecto, sin el prefijo `template/`, ordenadas por ruta, incluidos los ocultos. No se edita a mano una huella.
  - Hecho cuando:
    - [x] El número de filas de datos es igual a `find template -type f | wc -l` de F8.1
    - [x] Cada huella tiene 64 hex minúsculas; ninguna ruta empieza por `template/`
    - [x] `ACTUALIZAR.md`, ADR 007 y ADR 008 no se modificaron
    - [x] Lint L (puntos 1 y 2; el 4 no aplica a manifiestos) en `manifiestos/0.5.0.md`
    - [x] Evidencia de datos: No aplica
    - [x] Verificación manual UI: No aplica
- [x] **F8.3 E20 pasa.** RNF-5
  - Hacer: recalcular la huella de cada archivo de `template/` con el algoritmo de `ACTUALIZAR.md` y compararla con `manifiestos/0.5.0.md`. La definición de E20 en `validar-estructura.md` no se toca.
  - Hecho cuando:
    - [x] No hay diferencia, ni archivo sin fila, ni fila sin archivo
    - [x] El texto de la fila E20 es el mismo que antes de F6.4, más la frase de Alcance ya aceptada
    - [x] Lint L: No aplica
    - [x] Evidencia de datos: No aplica
    - [x] Verificación manual UI: No aplica
- [x] **F8.4 Contraste contra `manifiestos/0.4.0.md`.** Criterio de finalización
  - Hacer: contrastar de nuevo 0.4.0 contra el 0.5.0 regenerado. Anotar lo medido (altas, bajas, huellas distintas). No se exigen las cifras de un contraste anterior.
  - Hecho cuando:
    - [x] El contraste quedó anotado en la respuesta, con el conteo medido en esta corrida
    - [x] Lint L: No aplica
    - [x] Evidencia de datos: No aplica
    - [x] Verificación manual UI: No aplica
- [x] **F8.5 Chequeos de `AGENTS.md` › Comandos en cada archivo tocado.** RNF-3
  - Hacer: Lint L en cada archivo que este feature modificó. Sin scripts nuevos versionados. Español. UTF-8 sin BOM.
  - Hecho cuando:
    - [x] Lint L pasa en cada archivo de F2 a F7 y en `manifiestos/0.5.0.md` (con las excepciones de cada paso)
    - [x] No hay un script de instalación nuevo en el repo
    - [x] Evidencia de datos: No aplica
    - [x] Verificación manual UI: No aplica
- [x] **F8.6 Ensayo de instalación en un temporal.** RNF-3
  - Hacer: `tmp=$(mktemp -d); cp -R --update=none template/. "$tmp"/`. Repetir la copia sobre el mismo destino y comprobar que no pisa. Nunca en un proyecto real ni en el clon.
  - Hecho cuando:
    - [x] El temporal tiene los mismos archivos que `find template -type f` de F8.1
    - [x] La segunda copia con `--update=none` no cambia las huellas del temporal
    - [x] El directorio temporal queda fuera del repo
    - [x] Lint L: No aplica
    - [x] Evidencia de datos: No aplica
    - [x] Verificación manual UI: No aplica
- [x] **F8.7 Medir RF-23 y RF-24 sobre el contenido final.** RF-23, RF-24, RNF-1, RNF-2
  - Hacer: medir sobre `template/`, no sobre la copia local `.agents/`. Mismo método y mismo caso que F0. Tamaños congelados de F0 para los documentos del caso; tamaños actuales para las piezas del kit. Tabla antes/después de los 4 roles (`coordinator`, `planner`, `implementer`, `reviewer`) y de los 14 comandos (`sdd`, `sdd-spec`, `sdd-clarify`, `sdd-plan`, `sdd-tasks`, `sdd-implement`, `sdd-validate`, `sdd-flujo`, `sdd-cierre`, `sdd-change`, `sdd-status`, `sdd-init`, `sdd-constitucion`, `sdd-estructura`). `sdd-init` y `sdd-constitucion` entran con su lectura declarada, sin meta del 70 %. Repetir las fórmulas de F5.5: mediana ≤ 26 701, status ≤ 36 819, paso de referencia `F1.5` de `001` ≤ 89 004, paso de referencia `F6.2` de `001` con respaldo ≤ 92 280. Ninguna fila aumenta sin justificación anotada. RF-24: lista de verificaciones que siguen, cada una con su pieza: criterios de aceptación, "Hecho cuando", chequeos de `AGENTS.md` › Comandos, revisión de seguridad con alcance de código, cierre con deuda y ADR, y el contenido de `/sdd-status` (fase y Estado, punto de retomo, pasos hechos y pendientes, verificaciones manuales pendientes, horas y `pausa por confirmar`, siguiente comando). Si `template/` cambió desde F8.1, no se cierra este paso: se repite F8.
  - Hecho cuando:
    - [x] La tabla antes/después usa las cifras de F0 en la columna antes, y el contenido final en la después
    - [x] Mediana ≤ 26 701 y status ≤ 36 819; el `F6.2` de `001` ≤ 92 280 y el `F1.5` de `001` ≤ 89 004
    - [x] Cada verificación de RF-24 tiene una pieza localizable
    - [x] Si algún tope no cabe, se paró y se volvió al usuario, sin cambiar RNF-1 ni la línea base
    - [x] Lint L: No aplica
    - [x] Evidencia de datos: No aplica
    - [x] Verificación manual UI: No aplica
- [x] **F8.8 Proponer el refresco de la copia local.** Criterio de finalización
  - Hacer: si `diff -rq template/.agents .agents` muestra algo más que `napkin.md`, proponer el refresco de `INSTALAR.md` › Modo mantenimiento. No ejecutarlo sin confirmación del usuario. Los demás chequeos de réplicas de `/sdd-estructura` tienen que seguir pasando. Este paso no es `/sdd-cierre`.
  - Hecho cuando:
    - [x] El resultado del `diff` quedó anotado
    - [x] El refresco, si hace falta, está propuesto y no se ejecutó
    - [x] Lint L: No aplica
    - [x] Evidencia de datos: No aplica
    - [x] Verificación manual UI: No aplica

---

## Cobertura RF → pasos
| RF | Pasos |
|---|---|
| RF-1 | F3.3, F5.1, F5.5 |
| RF-2 | F3.3, F5.1 |
| RF-3 | F4.5, F5.2 |
| RF-4 | F4.7 |
| RF-5 | F3.2, F5.5, F8.7 |
| RF-6 | F4.6, F5.3 |
| RF-7 | F5.4 |
| RF-8 | F3.1, F5.4 |
| RF-9 | F4.1, F5.3 |
| RF-10 | F3.5, F3.6 |
| RF-11 | F4.5, F5.2, F6.3 |
| RF-12 | F5.1, F5.2, F5.3, F6.1, F6.2 |
| RF-13 | F5.1, F5.2, F5.3, F6.1, F6.2 |
| RF-14 | F2.2, F4.1, F4.2, F4.3, F4.4, F4.5, F4.6, F4.7, F4.8, F4.9, F5.4 |
| RF-15 | F2.2, F3.1, F4.7, F4.9, F5.4, F6.1, F6.2 |
| RF-16 | F2.1, F2.2, F3.3, F4.1, F4.7, F4.8, F5.4, F6.1, F6.2 |
| RF-17 | F4.3, F4.4, F5.3 |
| RF-18 | F2.2, F3.4, F5.2, F6.4 |
| RF-19 | F4.5, F4.6, F5.2 |
| RF-20 | F4.2 |
| RF-21 | F5.1, F5.2 |
| RF-22 | F2.1 |
| RF-23 | F0.1, F5.5, F8.1, F8.7 |
| RF-24 | F5.2, F8.7 |

RNF: RNF-1 en F5.5 y F8.7; RNF-2 en F8.7; RNF-3 en F8.5 y F8.6; RNF-4 en F1.1; RNF-5 en F2.1 y F8.3; RNF-6 en cada paso que edita una pieza (F2.1 a F7.1).

## Resumen de horas
| Fase | Pasos | Horas |
|---|---|---|
| F0 | F0.1 | 0,25 |
| F1 | F1.1 | 0,25 |
| F2 | F2.1, F2.2 | 0,5 |
| F3 | F3.1 a F3.6 | 0,5 |
| F4 | F4.1 a F4.9 | 0,5 |
| F5 | F5.1 a F5.5 | 0,5 |
| F6 | F6.1 a F6.4 | 0,5 |
| F7 | F7.1 | 0,25 |
| F8 | F8.1 a F8.8 | 0,75 |
| **Total** | **37** | **4,0** |
