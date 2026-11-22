# 04 — Tareas: Arranque ligero de sesión
Leyenda: `[ ]` Pendiente · `[~]` En curso · `[x]` Hecho · `[!]` Bloqueado

Modo completo. Cambio al kit: solo Markdown. Build: No aplica. Consulta de datos: No aplica. No hay
`sql/`. Lleva `07_casos_prueba.md` (ensayos en `mktemp -d` y comparaciones en solo lectura).

Decisiones ya aprobadas (no se reabren): `03_plan.md` › § 6 y ADR 0032. Se mueven reglas, no se
reescriben: un texto que sale de un archivo entra tal cual en su destino, salvo los renglones de
remisión y los pasos 4, 7 y 8, cuyo texto exacto da este documento.

Ejecución: F0 y F4 los hace la sesión. F1 a F3, el `implementer` como subagente en `sonnet`, un paso
por lanzamiento. Un paso toca como máximo dos archivos. La réplica de `template/.agents/` a `.agents/`
la hace la sesión en F2.5, no el implementer.

Commit de referencia para «antes»: `8c9681b` (`git show 8c9681b:<ruta>`).

**L (lint de `AGENTS.md` › Comandos)**, por cada archivo tocado:
- `file -i` da `charset=utf-8` o `us-ascii`.
- `grep -l $'^\xEF\xBB\xBF'` sale vacío.
- Las rutas citadas con comillas inversas existen (salvo `.agents/local/…`, que es de cada dev).
- En `template/`, no hay marcadores `<<…>>` salvo `<<NOMBRE_PROYECTO>>` e `<<IDIOMA>>`.

**P (protocolo de no pisar)**: antes de editar, `git diff HEAD -- <archivo>` sale vacío o solo con
cambios de este feature.

**S (secciones intactas)**: `diff <(git show 8c9681b:<archivo> | sed -n '/^## <Sección>/,/^## /p') <(sed -n '/^## <Sección>/,/^## /p' <archivo>)`
sale vacío para «Guardrails (todos los agentes, siempre)», «Comandos», «Stack y convenciones de
código» y «Reglas de negocio y del sistema».

Total: 195 min. 14 pasos.

## F0 — Línea base (estimado: 20 min)
No edita archivos del kit. Si un chequeo falla por algo ajeno, el paso queda `[!]` y F1 no empieza.

- [x] **F0.1 Registrar la medición de antes.** RF-14
  - Hecho cuando:
    - [x] `05_avance.md` › Decisiones tiene la línea base congelada de la spec y la medición de hoy de
      `02_analisis_impacto.md` § 3, con el comando (`wc -c`) y el commit `8c9681b`
    - [x] `/sdd-estructura` en el clon: resultado E1–E23 anotado (para comparar «sin `FALLA` nueva»)
- [x] **F0.2 Correr los escenarios del chequeo antes del cambio.** RF-8
  - Hecho cuando:
    - [x] CP-04 a CP-07 de `07_casos_prueba.md` corridos con `8c9681b`; el aviso o silencio de cada uno
      anotado en `05_avance.md` › Pruebas como «antes»

## F1 — Chequeo de arranque (estimado: 35 min)
- [x] **F1.1 Reordenar `template/.agents/sync-agents.md`.** RF-7, RF-9, RF-13
  - Orden: encabezado y su párrafo; «Chequeo de arranque»; «Estados de enlace» (nueva); «Estado
    «visto»»; «Qué hace el agente»; «Tabla de enlaces» con sus subsecciones; «Notas para el usuario».
  - «Estados de enlace»: la tabla de estados que hoy está en el paso 1 de «Qué hace el agente», más
    la frase «Para la fila de carpetas por archivo, `OK` = …» (de «Carpeta de agentes por archivo») y
    la frase «Para la fila de adaptadores de OpenCode, `OK` = …» (de «OpenCode»), movidas tal cual.
    Donde estaban queda «(ver «Estados de enlace»)».
  - «Chequeo de arranque» empieza con este renglón: «Qué lee: esta sección. Si el punto 1 lo omite, nada
    más. Si no, también «Estados de enlace», «Estado «visto»» y, de «Tabla de enlaces», solo el
    encabezado y las filas de las herramientas declaradas en «visto». No lee «Qué hace el agente» ni
    «Notas para el usuario».» El resto de la sección no cambia.
  - En «Qué hace el agente», la frase «Los pasos 1 y 2 son la lectura del arranque …» pasa a decir que
    el arranque sigue «Chequeo de arranque». La numeración de los pasos 1 a 8 no cambia (la citan
    `sdd-enlaces.md` y el ADR 0022).
  - Hecho cuando:
    - [x] P y L OK en el archivo
    - [x] `grep -c '^| `OK` |' template/.agents/sync-agents.md` da 1 (una sola tabla de estados)
    - [x] Los encabezados `##` y `###` son los de antes más «Estados de enlace», en el orden pedido
    - [x] «Chequeo de arranque» de hoy y de `8c9681b` solo difieren en el renglón «Qué lee»
    - [x] `diff` de las demás secciones contra `8c9681b`: solo las frases movidas y sus «(ver …)»
    - [x] Verificación manual UI: No aplica
- [x] **F1.2 Ajustar `template/.agents/commands/sdd-enlaces.md`.** RF-5
  - En «Qué lee», «estados» pasa a «Estados de enlace». Nada más.
  - Hecho cuando:
    - [x] P y L OK
    - [x] `git diff` del archivo: una sola línea cambiada

## F2 — Archivo siempre cargado (estimado: 80 min)
- [x] **F2.1 Criterio y «Roles y comandos» en `template/.agents/README.md`.** RF-1, RF-4, RF-5
  - Sección nueva «Qué va en `AGENTS.md`», antes de «Anti-patrones», con el criterio del ADR 0032
    (sin citar el ADR: `template/` no lleva ADR). Siempre cargado: lo que se usa en cualquier turno o
    lo que los roles y `/sdd-init` buscan ahí por nombre, con la lista de secciones. Bajo demanda: lo
    que usa un solo rol, comando o momento, en su archivo, con un renglón de remisión en `AGENTS.md`.
    Los pasos de «Cada sesión» piden secciones, no archivos enteros, y los condicionales dicen en una
    línea cuándo aplican.
  - Sección nueva «Roles y comandos», con el cuerpo de `template/AGENTS.md` › «Roles y comandos» tal
    cual.
  - La línea de «Agregar un comando» que dice «"Roles y comandos" de `AGENTS.md`» pasa a ««Roles y
    comandos» de este archivo».
  - Hecho cuando:
    - [x] P y L OK
    - [x] `diff` del cuerpo de «Roles y comandos» contra `git show 8c9681b:template/AGENTS.md`: vacío
    - [x] `grep -n 'Roles y comandos" de `AGENTS.md`' template/.agents/README.md` sale vacío
- [x] **F2.2 `template/AGENTS.md`.** RF-2, RF-3, RF-4, RF-6, RF-10, RF-12
  - Paso 4: «4. Antes de crear o modificar un agente, skill o comando (también a media sesión o en
    `/sdd-init`): leer `.agents/README.md`. Si la petición no los toca, no se lee.»
  - Paso 7: «7. Chequeo de enlaces y kit (solo lectura, un único aviso por sesión): lee
    `.agents/sync-agents.md` › «Chequeo de arranque» y solo lo que esa sección indica, no el archivo
    entero. Si hay diferencias, muestra el aviso combinado y recomienda `/sdd-enlaces` (no
    `/sdd-init`); si no, no digas nada. No escribas nada.»
  - Paso 8: «8. Revisar que no haya prefijos repetidos (mismo entero) en `.docs/002_specs/`,
    `.docs/003_deuda/` ni `.docs/004_adr/`. Si hay, lee `.docs/002_specs/README.md` › «Numeración
    única», avisa y propone el cambio; si no, no digas nada.»
  - «Roles y comandos», cuerpo nuevo: «Roles de `.agents/agents/` y comandos de `.agents/commands/`:
    `.agents/README.md` › «Roles y comandos». Se lee al elegir o crear un rol o comando.»
  - «Qué lee cada rol», cuerpo nuevo: «Cada rol lee de este archivo solo las secciones que nombra su
    `.agents/agents/<rol>.md`, y siempre «Guardrails», que no cuenta contra el tope de lecturas de
    `sdd-implement` ni contra la lista cerrada de `reviewer`. «Cada sesión» lo ejecuta la sesión
    principal; un subagente no lo repite.»
  - «Formato y numeración», cuerpo nuevo: «Numeración, ancho y prefijos repetidos:
    `.docs/002_specs/README.md` › «Numeración única». Se lee al crear un ítem o si el paso 8 halla
    repetidos.»
  - Los títulos `##` se conservan (referencias por nombre). Nada más cambia.
  - Hecho cuando:
    - [x] P, L y S OK
    - [x] El marcador de instalación sigue siendo la primera línea no vacía tras el título (E21)
    - [x] `wc -c template/AGENTS.md` ≤ 11 266 (meta de `03_plan.md` § 8); el valor, anotado en la respuesta
- [x] **F2.3 E8, E9 y E17 en `template/.agents/validar-estructura.md`.** RF-11, RF-13
  - E8: los roles coinciden con `.agents/README.md` › «Roles y comandos».
  - E9: los comandos citados en `.agents/README.md` › «Roles y comandos» existen y los que existen están
    citados ahí. Que `AGENTS.md` cite comandos o no ya no cuenta (un `AGENTS.md` viejo puede tener su
    sección y no es `FALLA`).
  - E17: en la columna de cómo, «Revisar anti-patrones y «Qué va en `AGENTS.md`» de `.agents/README.md`».
  - Hecho cuando:
    - [x] P y L OK
    - [x] `git diff` del archivo: solo las filas E8, E9 y E17
- [x] **F2.4 `AGENTS.md` de la raíz.** RF-2 a RF-4, RF-6, RF-10
  - Mismo cambio que F2.2 sobre los pasos 4, 7 y 8 y las tres secciones, con el mismo texto. El paso 0
    y lo propio del clon fuera de esas tres secciones no cambian.
  - Hecho cuando:
    - [x] P, L y S OK
    - [x] `diff` de «Qué lee cada rol», «Roles y comandos» y «Formato y numeración» entre la raíz y
      `template/AGENTS.md`: vacío
    - [x] `wc -c AGENTS.md` ≤ 12 648; el valor, anotado en la respuesta
- [x] **F2.5 Réplica en la raíz (sesión).** RNF de réplicas
  - `cp` de `sync-agents.md`, `README.md`, `validar-estructura.md` y `commands/sdd-enlaces.md` de
    `template/.agents/` a `.agents/`.
  - Hecho cuando:
    - [x] `diff -rq template/.agents .agents` solo muestra las diferencias de la línea base
      (`napkin.md`, `agents/claudio.md`, `local/`, `skills/caveman`, `skills/reporte-sesiones`) más las
      que ya existían en `8c9681b` (anotarlas en F0.1)

## F3 — Actualización y versión (estimado: 30 min)
- [x] **F3.1 `ACTUALIZAR.md`.** RF-13
  - En «Grupo B», junto a la nota del paso 7: desde 0.12.0, los pasos 4, 7 y 8 de «Cada sesión» se
    incorporan a mano con el texto de `template/AGENTS.md`. «Qué lee cada rol», «Roles y comandos» y
    «Formato y numeración» pueden quedarse como estaban o pasar a su renglón; quedarse no rompe nada,
    solo no ahorra.
  - Hecho cuando:
    - [x] P y L OK
    - [x] El texto nombra las secciones y los pasos sin copiarlos
- [x] **F3.2 Versión 0.12.0 y changelog.** RNF
  - `VersionAgentKitSdd` = `0.12.0`. Entrada `## 0.12.0 — <fecha>` en `ChangelogAgentKitSdd.md` con el
    estilo de 0.11.0: arranque ligero (ADR 0032), qué salió de `AGENTS.md` y adónde, `sync-agents.md`
    reordenado, paso 4 condicional, E8, E9 y E17, y la nota para proyectos actualizados.
  - Hecho cuando:
    - [x] P y L OK en los dos
- [x] **F3.3 Manifiesto 0.12.0 (sesión).** RNF
  - `manifiestos/0.12.0.md` con el algoritmo de huella de `ACTUALIZAR.md`, desde el árbol de
    `template/` tras F3.2, con el formato de `manifiestos/0.11.0.md`.
  - Hecho cuando:
    - [x] Una fila por archivo de `template/`; las huellas de los archivos no tocados coinciden con 0.11.0

## F4 — Verificación (estimado: 30 min)
La hace la sesión. No edita archivos del kit; si algo falla, se vuelve al paso que lo produjo.

- [x] **F4.1 Casos de prueba.** RF-2, RF-6, RF-8, RF-11, RF-13
  - Hecho cuando:
    - [x] CP-01 a CP-10 de `07_casos_prueba.md` corridos; resultados en `05_avance.md` › Pruebas
    - [x] `/sdd-estructura` en el clon y en el ensayo de instalación: sin `FALLA` nueva frente a F0.1
- [x] **F4.2 Medición final.** RF-14
  - Hecho cuando:
    - [x] `05_avance.md` tiene la medición final con el método de la spec, la del escenario base (clon)
      y la de referencia (proyecto con herramientas declaradas), y el porcentaje contra la línea base
      congelada: lectura del arranque ≤ 15 721 bytes; `AGENTS.md` raíz ≤ 12 648 y `template/` ≤ 11 266
