# 04 — Tareas: Ruteo de modelos
Leyenda: `[ ]` Pendiente · `[~]` En curso · `[x]` Hecho · `[!]` Bloqueado

«Hecho cuando» común a todo paso que toca un archivo (no se repite abajo): lint de `AGENTS.md` ›
Comandos en el archivo (UTF-8 sin BOM; `name` del frontmatter igual a su carpeta o archivo de rol;
rutas citadas existentes; en `template/`, sin marcadores `<<…>>` salvo los dos admitidos). Evidencia
de datos y verificación manual de UI: No aplica, salvo que el paso diga otra cosa.

Nombres fijos que usan varios pasos (se escriben tal cual):
- Rol nuevo: `auxiliar`.
- Sección de la tabla y la regla de ruteo: `restricciones-de-rol.md` › «Nivel por tarea sin rol».
- Sección de la regla de argumentos: `.agents/README.md` › «Argumentos de un comando».
- Motivo nuevo de lanzamiento: «ruteo».

## Lotes y olas
| Ola | Lote | Pasos | Archivos | Quién |
|---|---|---|---|---|
| 0 | — | F0.1, F0.2 | — | sesión |
| 1 | L1 | F1.1, F2.6 | `template/.agents/README.md` | implementer |
| 1 | L2 | F1.2, F1.3 | 10 comandos de fase | implementer |
| 1 | L3 | F2.1, F2.2, F4.1 | `auxiliar.md`, `restricciones-de-rol.md`, `git-commit/SKILL.md` | implementer |
| 1 | L4 | F1.4, F2.3, F2.4, F2.5 | `validar-estructura.md`, `AGENTS.md` (plantilla y raíz), `configuracion.ejemplo.md`, `sdd-config.md`, `sync-agents.md` | implementer |
| 1 | L5 | F3.1, F3.2, F3.3, F3.4 | `sdd-status.md`, `sdd-estructura.md`, `SKILL.md`, `formato-respuesta/SKILL.md`, `avance-y-tiempos.md` | implementer |
| 2 | — | F5.1, F5.2, F5.3 | réplica, enlaces, chequeos | sesión |
| 3 | — | F6.1, F6.2, F6.3 | medición | sesión |
| 4 | — | F7.1, F7.2, F7.3 | ADR, `README.md` raíz, versión, changelog | sesión |
| 5 | — | F7.4 | `manifiestos/0.15.0.md` | sesión (último cambio a `template/`) |
| aparte | — | F5.4 | OpenCode y Cursor | persona, en su momento (decisión 16) |

## F0 — Línea base y verificación (estimado: 10 min)
- [x] **F0.1 Línea base.** —
  - Hecho cuando:
    - [x] `/sdd-estructura` corrido por la sesión; resultado anotado en `05_avance.md`
    - [x] `git status --short template/` vacío antes de empezar
- [x] **F0.2 Verificación en la documentación de Claude Code.** RF-4, RF-13
  - Hecho cuando:
    - [x] Confirmado con fuente y fecha que un comando de `.claude/commands/` admite `context: fork` y
      `agent: <rol>`, que corre como subagente con el modelo del rol y que `$ARGUMENTS` se sustituye
      antes del fork
    - [x] Confirmado cómo queda `$ARGUMENTS` sin argumentos (vacío o literal)
    - [x] Si algo no se confirma: el paso queda `[!]`, se avisa y no arranca la ola 1

## F1 — Argumentos (estimado: 15 min)
- [x] **F1.1 `README.md`: «Argumentos de un comando» y «Agregar un comando».** RF-13, RF-14
  - Hecho cuando:
    - [x] Sección «Argumentos de un comando», fuente única: los comandos leen solo `$ARGUMENTS`; la
      carpeta es su primera palabra y el resto (alcance, idea, cambio, motivo) es lo que sigue, con
      comillas o signos; nunca `$0`, `$1`, `$2`; si `$ARGUMENTS` llega vacío o literal (sin
      sustituir), se toman como argumentos las palabras que la persona escribió tras el comando y, si
      no hay, el argumento está ausente; una palabra que empieza por `$` nunca es carpeta; un
      argumento ausente aplica el comportamiento definido para su ausencia o se pide; un comando que
      no recibe carpeta (su `argument-hint` no la nombra) toma todo `$ARGUMENTS` como su argumento
    - [x] «Agregar un comando» ya no dice que `$1`, `$2`… son argumentos por posición y remite a la
      sección nueva
- [x] **F1.2 Comandos `sdd-spec`, `sdd-clarify`, `sdd-plan`, `sdd-tasks`, `sdd-change`.** RF-13, RF-14
  - Hecho cuando:
    - [x] Cada uno lleva una línea `Argumentos: $ARGUMENTS` con su caso (qué es carpeta y qué es
      resto) y remite a «Argumentos de un comando» sin copiarla
    - [x] Ningún `$1` ni `$2`; el «Siguiente paso» usa `/sdd-x <carpeta>`
    - [x] `sdd-spec`: el cálculo del prefijo se hace sobre la carpeta (primera palabra), no sobre `$1`
- [x] **F1.3 Comandos `sdd-implement`, `sdd-validate`, `sdd-flujo`, `sdd-cierre`, `sdd-pausa`.** RF-13, RF-14
  - Hecho cuando:
    - [x] Igual que F1.2
    - [x] `sdd-implement`: alcance = segunda palabra; sin ella, cascada; `Fn` y `Fn.m` como hoy
    - [x] `sdd-pausa`: no recibe carpeta (`argument-hint: [motivo]`); el motivo es todo `$ARGUMENTS`
      y vacío = sin motivo. Se conserva qué recibe (fuera de alcance: cambiarlo)
- [x] **F1.4 `validar-estructura.md` › E9.** RF-13
  - Hecho cuando:
    - [x] E9 suma: ningún `commands/<n>.md` usa `$0`, `$1`, `$2`… (solo `$ARGUMENTS`)

## F2 — Rol y tabla de ruteo (estimado: 20 min)
- [x] **F2.1 `template/.agents/agents/auxiliar.md`.** RF-7, RF-3, RF-8
  - Hecho cuando:
    - [x] Frontmatter: `name: auxiliar`, `description` (uso general de nivel Medio para tareas sin
      rol delegables; nunca commit), `tools: Read, Glob, Grep, Bash`, `model: sonnet`,
      `omitClaudeMd: true`
    - [x] Cuerpo: lee siempre `AGENTS.md` › Guardrails; trabaja con encargo cerrado; si le falta un
      dato, el encargo pide diseño o decisión, o necesita una confirmación, se detiene y devuelve la
      pregunta o `FUERA DE ALCANCE` sin suponer; nunca `git commit`, `push`, `fetch`, `pull` ni
      `sync`; no escribe `05_avance.md` ni la celda del índice; no usa red ni conectores; dentro de
      una fase solo prepara commits o corre una delegación fija y nunca hace el trabajo del rol de la
      fase (remite a `SKILL.md` › «Dónde corre cada rol»); declara su modelo en la respuesta
- [x] **F2.2 `restricciones-de-rol.md`.** RF-1, RF-3, RF-6, RF-7, RF-9
  - Hecho cuando:
    - [x] «Modelo por rol»: fila `auxiliar` / `sonnet`; el párrafo de fuente sigue valiendo
    - [x] Sección «Nivel por tarea sin rol» debajo de «Modelo por rol» con: tabla Tarea · Nivel ·
      Delegable · Rol (`/sdd-status` Medio sí `auxiliar` fija; `/sdd-estructura` Medio sí `auxiliar`
      fija; preparación de commit Medio sí `auxiliar`; `/sdd-version` no (red); `/sdd-enlaces`,
      `/sdd-config`, `/sdd-pausa` no (confirmación o `05_avance.md`); lectura de conectores no
      (decisión 13); revisión de logs en archivo Bajo sí `explore`; salida larga de un comando Medio
      sí `auxiliar`; búsqueda en el repositorio Bajo sí `explore`; reportes o resúmenes Medio sí
      `auxiliar`); el nivel de cada rol sale de «Modelo por rol»
    - [x] Las cuatro condiciones de delegable (RF-3), la regla de decisión del chat antes de la primera
      llamada (RF-6, sin redelegar a mitad), espera en primer plano salvo pedido, el respaldo sin
      delegación (RF-9, aviso de una línea solo al iniciar un comando cuyo nivel no es el de la sesión,
      sin bloquear, sin aviso si no conoce su modelo), la prohibición de declarar `model:` sin
      subagente y la remisión a `SKILL.md` para el relanzamiento (sin copiarlo)
    - [x] Tabla de restricciones: `auxiliar` en la fila de quienes no registran en Git, en la de
      quienes no lanzan subagentes (sin `Agent`) y en la de red; fila propia: sin `Write` ni `Edit`
      (GARANTIZADA en Claude Code por `tools`), escritura por shell solo la bandera de estructura
      (instrucción)
- [x] **F2.3 Renglón de ruteo en `AGENTS.md` (plantilla y raíz).** RF-6
  - Hecho cuando:
    - [x] En «Roles y comandos» de `template/AGENTS.md` y de `AGENTS.md`: ruteo de tareas sin rol en
      `restricciones-de-rol.md` › «Nivel por tarea sin rol»; se lee antes de una tarea suelta que no
      sea diseño o decisión, o de preparar un commit
    - [x] Una sola línea por archivo (criterio del ADR 0032); sin copiar la tabla
- [x] **F2.4 `configuracion.ejemplo.md` y `sdd-config.md`.** RF-11
  - Hecho cuando:
    - [x] Paso 1: `auxiliar` en el nivel medio
    - [x] `sdd-config.md`: el paso de modelos cubre también los roles del ruteo, por remisión a «Modelo
      por rol»; sin valores copiados
- [x] **F2.5 `sync-agents.md` › «OpenCode».** RF-4, RF-7
  - Hecho cuando:
    - [x] `auxiliar`: `mode: subagent`, `edit` denegado y `subagent` denegado
- [x] **F2.6 `README.md` › «Roles y comandos».** RF-7
  - Hecho cuando:
    - [x] Lista de agentes con `auxiliar` (uso general de nivel Medio para tareas sin rol; nunca
      commit) y remisión a «Nivel por tarea sin rol»

## F3 — Delegación fija y fases (estimado: 15 min)
- [x] **F3.1 `sdd-status.md`.** RF-4, RF-13, RF-14
  - Hecho cuando:
    - [x] Frontmatter con `context: fork` y `agent: auxiliar`
    - [x] Argumentos según «Argumentos de un comando»: sin carpeta, informa el índice
    - [x] Con la marca de pausa, la pregunta del hueco largo se devuelve al chat
- [x] **F3.2 `sdd-estructura.md`.** RF-4, RF-13, RF-14
  - Hecho cuando:
    - [x] Frontmatter con `context: fork` y `agent: auxiliar`; fuera «No lanza un subagente» y «no
      asigna … a ningún subagente» (sí sigue sin ir al `reviewer`)
    - [x] Alcance = primera palabra; vacío = todos los bloques
    - [x] La bandera se escribe por shell tras `git check-ignore`; sigue siendo su única escritura
- [x] **F3.3 `SKILL.md`.** RF-2, RF-4, RF-8
  - Hecho cuando:
    - [x] Tabla de fases: Estado y Estructura con `auxiliar` (delegación fija)
    - [x] «Dónde corre cada rol»: la delegación fija es motivo válido de lanzamiento; excepción a
      «Solo roles del kit»: en una fase, `auxiliar` solo para preparar commits y delegación fija;
      fuera de una fase, cualquier delegación de «Nivel por tarea sin rol», a la que remite
    - [x] «Si no hay un solo paso»: el estado y la estructura salen de la lista de la sesión
      principal; el cierre sigue ahí
    - [x] Las fases con rol no cambian (RF-2)
- [x] **F3.4 `formato-respuesta/SKILL.md` y `avance-y-tiempos.md`.** RF-10
  - Hecho cuando:
    - [x] Contexto nombra rol y modelo del subagente (`auxiliar (sonnet) → …`)
    - [x] Viñeta **Modelo**: motivo «ruteo»; dentro de una fase va en la Nota de su fila; fuera,
      basta la línea «quién responde»

## F4 — Commits (estimado: 10 min)
- [x] **F4.1 `git-commit/SKILL.md`.** RF-5
  - Hecho cuando:
    - [x] La preparación (fases 1 a 3, stage explícito y propuesta de mensaje) la hace `auxiliar` con
      un encargo que lleva requerimiento, carpeta y resumen de lo hecho; remite a «Nivel por tarea sin
      rol»
    - [x] Paradas: prefijo repetido, archivos ajenos ya en el stage o agrupación dudosa devuelven la
      pregunta al chat
    - [x] Fase 4 (confirmación y `git commit`) en la sesión principal; sin confirmación, informa el
      stage y propone sacarlo
    - [x] Reglas absolutas, deny list y checklist sin cambio de fondo; vale también para los commits
      de `/sdd-cierre`

## F5 — Réplica y chequeos (estimado: 10 min)
- [x] **F5.1 Réplica local.** —
  - Hecho cuando:
    - [x] `cp` de los archivos tocados de `template/.agents/` a `.agents/`
    - [x] `diff -rq template/.agents .agents` solo con las diferencias admitidas en `MEMORY.md`
- [x] **F5.2 `/sdd-enlaces`.** —
  - Hecho cuando:
    - [x] `.claude/agents` ve `auxiliar.md` (enlace de carpeta o adaptador); confirmación de la persona
      si hay que crear o corregir algo
- [x] **F5.3 Chequeos.** RF-13
  - Hecho cuando:
    - [x] Lint en cada archivo tocado; casos CP-01 a CP-06, CP-08 y CP-10 de `07_casos_prueba.md`
    - [x] `/sdd-estructura` sin `FALLA` en E8, E9 y E23 (primera corrida delegada si la herramienta
      ya cargó el comando; si no, en el chat y se anota)
- [ ] **F5.4 Prueba en OpenCode y Cursor.** RF-4, RF-9, RF-13
  - Hecho cuando:
    - [ ] CP-09 corrido por la persona, en su momento (decisión 16); sin esto no hay cierre

## F6 — Medición (estimado: 30 min)
- [x] **F6.1 `/sdd-status`: chat de nivel Alto frente a delegado.** RF-12
  - Hecho cuando:
    - [x] Tokens (entrada, caché, salida), costo en USD con precios de la página oficial y fecha, y
      tiempo de cada variante, de los transcripts de la sesión y del subagente
- [x] **F6.2 Preparación de un commit: chat frente a delegado.** RF-12
  - Hecho cuando:
    - [x] Igual que F6.1, sobre un commit real de esta spec (CP-07)
- [x] **F6.3 Decisiones y ajuste.** RF-12
  - Hecho cuando:
    - [x] Una decisión por resultado en `05_avance.md`; si no ahorra, la fila sale de «Nivel por tarea
      sin rol»; si ahorra y tarda más del doble, decide la persona (se detiene)

## F7 — ADR y versión (estimado: 15 min)
- [x] **F7.1 ADR 0037 e índice de ADR.** criterio 7
  - Hecho cuando:
    - [x] `.docs/004_adr/0037_DelegacionConSubagenteYNoSoloModelo.md` con contexto, alternativas
      (`model:` sin subagente, `/model` a mano) y consecuencias; fila en el índice
- [x] **F7.2 `README.md` de la raíz.** RF-7
  - Hecho cuando:
    - [x] Lista de roles con `auxiliar` y una línea del ruteo
- [x] **F7.3 Versión y changelog.** criterio 9
  - Hecho cuando:
    - [x] `VersionAgentKitSdd` en `0.15.0`; entrada en `ChangelogAgentKitSdd.md`
- [x] **F7.4 Manifiesto y E20.** criterio 9
  - Hecho cuando:
    - [x] `manifiestos/0.15.0.md` generado tras el último cambio a `template/`
    - [x] E20 de `template/.agents/validar-estructura.md` sin `FALLA`
