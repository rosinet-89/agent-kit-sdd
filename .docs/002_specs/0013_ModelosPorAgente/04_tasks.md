# 04 — Tareas: Modelos por agente
Leyenda: `[ ]` Pendiente · `[~]` En curso · `[x]` Hecho · `[!]` Bloqueado

Modo completo. Cambio al kit: solo Markdown. Build: No aplica. Consulta de datos: No aplica. No hay `sql/`. Este feature no lleva `07_casos_prueba.md`: el documento nace con esta spec y RF-16 solo aplica a los features que lleguen a `tasks` después de instalarla. La comprobación de `explore` en Claude Code se hace en validate.

Decisiones ya aprobadas (2026-11-21 11:22; no se reabren): las D1 a D8 de `03_plan.md` › §6, y la fuente única de cada regla de `03_plan.md` › §1. Un paso que redacta una regla la escribe en su fuente; los demás pasos solo remiten a ella.

Ejecución: cada paso de F1 a F8 lo hace el `implementer` como subagente en `sonnet` (la tabla de RF-1 ya lo fija). F0 lo hace la sesión. Un paso toca como máximo dos archivos, salvo F1.3 y F2.4, que solo cambian una línea en cada agente. `template/` no se edita después de F8.2.

**L (lint de `AGENTS.md` › Comandos)**, por cada archivo tocado:
- `file -i` da `charset=utf-8` o `us-ascii`.
- `grep -l $'^\xEF\xBB\xBF'` sale vacío.
- Las rutas citadas con comillas inversas existen.
- En los agentes, el `name` coincide con el nombre del archivo.
- En `template/`, no hay marcadores `<<…>>` salvo `<<NOMBRE_PROYECTO>>` e `<<IDIOMA>>`.

Total: 325 min. 42 pasos (F9 del cambio 1: 30 min, 6 pasos).

## F0 — Línea base (estimado: 10 min)
No edita nada. Si un chequeo falla por algo ajeno, el paso queda `[!]` y F1 no empieza.

- [x] **F0.1 Anotar E19.** —
  - Hecho cuando:
    - [x] Se corrió `diff -rq template/.agents .agents`, ignorando `.agents/local/`, y el resultado quedó en el reporte.
    - [x] Solo aparecen `napkin.md`, `skills/caveman` y `agents/claudio.md` (línea base de `MEMORY.md`), más `skills/reporte-sesiones`. Esa última se le informa a la persona, que decide si entra en la línea base. Cualquier otra ruta detiene el paso.
    - [x] Evidencia de datos: No aplica. Verificación manual: No aplica.

- [x] **F0.2 Anotar E20 contra `manifiestos/0.8.0.md`.** —
  - Hecho cuando:
    - [x] La prueba de humo de `ACTUALIZAR.md` › `## Prueba de humo`, con `kit_huella` de `### Linux (probado)` cargada en el shell sin versionarla, da las tres huellas de su tabla.
    - [x] Cada fila de `manifiestos/0.8.0.md` coincide con `template/<ruta>`: no falta ni sobra ningún archivo.
    - [x] Evidencia de datos: No aplica. Verificación manual: No aplica.

- [x] **F0.3 Copia previa y ensayo de instalación.** —
  - Hecho cuando:
    - [x] Hay una copia de `template/` y del `AGENTS.md` de la raíz en el scratchpad de la sesión, para el protocolo P de `MEMORY.md`, que también contrasta con `git diff HEAD`.
    - [x] `tmp=$(mktemp -d); cp -R --update=none template/. "$tmp"/` termina en 0. Un segundo `cp` no pisa un archivo que se alteró a propósito.
    - [x] Evidencia de datos: No aplica. Verificación manual: No aplica.

## F1 — Modelo por rol (estimado: 30 min)
Fuente: `template/.agents/skills/sdd/restricciones-de-rol.md`. Depende de F0.

- [x] **F1.1 Tabla «Modelo por rol».** RF-1, RF-3
  - Hecho cuando:
    - [x] El encabezado nombra los seis roles: `coordinator`, `planner`, `implementer`, `reviewer`, `tester` y `explore`.
    - [x] Sección `## Modelo por rol` con seis filas: `opus` para planner, coordinator y reviewer, `sonnet` para implementer y `haiku` para `tester` y `explore`.
    - [x] Dice que el `model:` del frontmatter es su aplicación, que la tabla del proyecto registra lo aplicado (S-6) y que el resto del kit remite y no copia.
    - [x] Remite al hecho (c) para el orden de resolución y no lo copia (RF-3).
    - [x] L en el archivo. Evidencia de datos: No aplica. Verificación manual: No aplica.

- [x] **F1.2 Roles Haiku y restricciones nuevas.** RF-10, RF-11, RF-12, RF-13, RF-15, RF-25
  - Hecho cuando:
    - [x] Sección `## Roles Haiku`:
      - Un encargo cerrado trae objetivo, alcance, pasos o casos y formato de salida.
      - Si falta algo, la respuesta es `ENCARGO INCOMPLETO` y dice qué falta.
      - Si un caso no trae destino o datos de partida, ese caso queda `bloqueado` y el resto sigue.
      - `ENCARGO FUERA DE ROL` se usa cuando el encargo pide diseñar, decidir, elegir o juzgar.
    - [x] Filas nuevas en la tabla de restricciones, que distinguen lo que garantiza `tools` de lo que es solo instrucción:
      - `explore` es de solo lectura, sin `Bash` ni red.
      - `tester` no tiene `Write` ni `Edit`: lo garantiza `tools`. Que no escriba por shell es instrucción. Como subagente, la falta de red la garantiza `tools`.
    - [x] La fila «Ningún agente usa la red» suma la excepción: solo `tester`, contra los destinos de QA y por su herramienta de apoyo. Remite a Guardrails, regla 1, y no la copia.
    - [x] La fila de `Agent` nombra a `tester` y `explore` entre los que no lanzan subagentes. El coordinator los lanza.
    - [x] Se suma una fila: el planner escribe además `07_casos_prueba.md`, y quien atiende la fase como sesión escribe las corridas (RF-15).
    - [x] L en el archivo. Evidencia de datos: No aplica. Verificación manual: No aplica.

- [x] **F1.3 `model:` en los cuatro agentes.** RF-1
  - Hecho cuando:
    - [x] `planner.md`, `coordinator.md` y `reviewer.md` de `template/.agents/agents/` dicen `model: opus`, e `implementer.md` dice `model: sonnet`. No cambia ninguna otra línea (`git diff` de una línea por archivo).
    - [x] L en los cuatro archivos. Evidencia de datos: No aplica. Verificación manual: No aplica.

## F2 — Roles Haiku y lista de roles (estimado: 45 min)
Depende de F1.

- [x] **F2.1 Crear `explore.md`.** RF-10, RF-12, RF-13, RF-4
  - Hecho cuando:
    - [x] `template/.agents/agents/explore.md` tiene `name: explore`, una descripción de cuándo lanzarlo, `tools: Read, Glob, Grep` y `model: haiku`, sin `omitClaudeMd` (D7).
    - [x] El cuerpo:
      - Remite a `restricciones-de-rol.md` › «Roles Haiku» para comprobar el encargo, y no copia la regla.
      - Lee y cumple Guardrails.
      - Localiza archivos y ocurrencias y devuelve lo que pide el formato de salida.
      - Declara el modelo con que corrió, o «modelo no confirmado».
    - [x] L en el archivo. Evidencia de datos: No aplica. Verificación manual: No aplica.

- [x] **F2.2 Crear `tester.md`.** RF-11, RF-12, RF-13, RF-14, RF-19, RF-24, RF-25, RF-26, RF-4, S-1, S-5
  - Hecho cuando:
    - [x] `template/.agents/agents/tester.md` tiene `name: tester`, `tools: Read, Glob, Grep, Bash` y `model: haiku`, sin `omitClaudeMd`. Dice que el proyecto suma sus herramientas `mcp__` a `tools` (S-1).
    - [x] El cuerpo, comprobación del encargo y de los casos:
      - Comprueba el encargo y cada caso: remite a «Roles Haiku».
      - Trabaja en modo seguro: solo ejecuta los casos que no escriben y no usan la red. El resto queda `manual` con el motivo.
      - Puede usar la excepción de QA solo si existe `.agents/local/entornos.md` bien formado. Remite a Guardrails, regla 1.
      - Compara el destino antes de cada caso y en cada navegación o llamada: mismo esquema, host y puerto; sin puerto, el del esquema; la ruta no cuenta. Si no coincide, el caso se detiene y queda `manual`.
    - [x] El cuerpo, ejecución y salida:
      - Si una herramienta no responde, un paso no se puede ejecutar o falta la variable de una credencial, el caso queda `bloqueado` y no se inventa resultado.
      - Nunca lee ni imprime el valor de una credencial.
      - Devuelve por caso `obtenido`, `bloqueado` o `manual`, sin marcar `ok` o `falla` y sin veredicto.
      - Declara el modelo con que corrió.
    - [x] L en el archivo. Evidencia de datos: No aplica. Verificación manual: No aplica.

- [x] **F2.3 Coordinator lanza a los Haiku.** RF-8, RF-9
  - Hecho cuando:
    - [x] En `template/.agents/agents/coordinator.md`, `tools` dice `Agent(planner, implementer, reviewer, tester, explore)`.
    - [x] «Si te invocan como subagente» dice que no lanza `tester` ni `explore`, que los lanza la sesión principal y que lo devuelve como pendiente.
    - [x] Dice que, si su copia personalizada no lista `tester` o `explore`, avisa por cada rol que falte que no podrá lanzarlo hasta que la persona lo agregue.
    - [x] Dónde corre cada rol: remite a `SKILL.md` › «Dónde corre cada rol» (F3.1) y no copia la regla.
    - [x] L en el archivo. Evidencia de datos: No aplica. Verificación manual: No aplica.

- [x] **F2.4 Los cuatro roles declaran su modelo.** RF-4
  - Hecho cuando:
    - [x] Las secciones «Respuesta» o de veredicto de `planner.md`, `implementer.md`, `reviewer.md` y `coordinator.md` piden declarar el modelo con que corrió (el id, o «modelo no confirmado»), con una misma frase que remite a `avance-y-tiempos.md`.
    - [x] L en los cuatro archivos. Evidencia de datos: No aplica. Verificación manual: No aplica.

- [x] **F2.5 Cuadros de color y layout.** RF-7 (S-4), RF-10, RF-11, RF-27
  - Hecho cuando:
    - [x] `template/.agents/skills/formato-respuesta/SKILL.md` suma `tester` 🟨 y `explore` 🟫 a la tabla de cuadros (D6).
    - [x] `template/.agents/README.md` nombra los seis roles en el layout y lista `entornos.ejemplo.md`.
    - [x] L en los dos archivos. Evidencia de datos: No aplica. Verificación manual: No aplica.

- [x] **F2.6 Roles en `AGENTS.md` (template y raíz).** RF-10, RF-11, D5
  - Hecho cuando:
    - [x] En `template/AGENTS.md`, «Roles y comandos» describe a `tester` (ejecuta casos, Haiku) y a `explore` (localiza, Haiku).
    - [x] «Qué lee cada rol» tiene filas para los dos: «Comandos» y Guardrails. Aclara que no declaran `omitClaudeMd`.
    - [x] Las secciones «Roles y comandos» y «Qué lee cada rol» del `AGENTS.md` de la raíz quedan idénticas a las de `template/` (`diff` vacío).
    - [x] L en los dos archivos. Evidencia de datos: No aplica. Verificación manual: No aplica.

- [x] **F2.7 Lista de agentes en `README.md` y en la skill.** —
  - Hecho cuando:
    - [x] `README.md` de la raíz (línea de `agents/`) nombra los seis roles.
    - [x] La `description` de `template/.agents/skills/sdd/SKILL.md` nombra los seis roles y no pasa de 1024 caracteres (E6).
    - [x] L en los dos archivos. Evidencia de datos: No aplica. Verificación manual: No aplica.

## F3 — Dónde corre cada rol (estimado: 40 min)
Fuente: `SKILL.md` › «Dónde corre cada rol». Depende de F2.

- [x] **F3.1 Sección «Dónde corre cada rol».** RF-7, RF-8, S-4
  - Hecho cuando:
    - [x] `template/.agents/skills/sdd/SKILL.md` tiene una sección `## Dónde corre cada rol`, antes de «Si no hay un solo paso». La sección dice:
      - Un rol con el mismo modelo que la sesión (según la definición de la spec) corre en el chat con la definición de ese rol.
      - Un subagente se lanza solo cuando el rol necesita otro modelo o la persona lo pide.
      - El reviewer también corre en el chat. La separación es por rol: el chat muestra el rol (S-4) y nunca valida con el rol con que produjo.
      - El modelo del rol se toma de `restricciones-de-rol.md` › «Modelo por rol»: remite y no copia.
      - `tester` y `explore` los lanza quien atiende la fase como sesión. Un coordinator subagente no los lanza.
      - En el chat, las restricciones de los Haiku son solo instrucción.
    - [x] «Si no hay un solo paso» remite a la sección nueva y no se contradice con ella.
    - [x] En la tabla de fases, `validate` nombra `tester, reviewer`.
    - [x] L en el archivo. Evidencia de datos: No aplica. Verificación manual: No aplica.

- [x] **F3.2 `sdd-spec`, `sdd-clarify` y `sdd-plan` remiten.** RF-7
  - Hecho cuando:
    - [x] En los tres comandos de `template/.agents/commands/`, cada «Delega en el subagente X» pasa a la misma línea: «Ejecuta el rol **X** (dónde corre: `.agents/skills/sdd/SKILL.md` › «Dónde corre cada rol»)». El resto del procedimiento no cambia.
    - [x] L en los tres archivos (paso de una línea por archivo). Evidencia de datos: No aplica. Verificación manual: No aplica.

- [x] **F3.3 `sdd-tasks`, `sdd-implement` y `sdd-change` remiten.** RF-7
  - Hecho cuando:
    - [x] Misma línea que en F3.2 en los tres comandos.
    - [x] L en los tres archivos. Evidencia de datos: No aplica. Verificación manual: No aplica.

- [x] **F3.4 `sdd-flujo` y `sdd-validate` remiten.** RF-7
  - Hecho cuando:
    - [x] Misma línea que en F3.2 en los dos comandos. Solo cambia la línea de delegación (validate se completa en F4.4).
    - [x] `grep -rn "Delega en el subagente" template/.agents/commands` sale vacío.
    - [x] `grep -c "Dónde corre cada rol" template/.agents/commands/*.md` da al menos 1 en los ocho comandos de fase.
    - [x] L en los dos archivos. Evidencia de datos: No aplica. Verificación manual: No aplica.

- [x] **F3.5 Modelo en la fila de tiempos.** RF-4
  - Hecho cuando:
    - [x] `template/.agents/skills/sdd/avance-y-tiempos.md` › Tiempos reales dice que la Nota de cada tramo lleva el modelo que declaró el rol, o «modelo no confirmado», y que lo anota la sesión principal.
    - [x] L en el archivo. Evidencia de datos: No aplica. Verificación manual: No aplica.

## F4 — Documento de casos de prueba (estimado: 45 min)
Fuente: `plantillas.md` › `## 07_casos_prueba.md`. Depende de F3.

- [x] **F4.1 Plantilla `07_casos_prueba.md`.** RF-16, RF-17, RF-18, RF-19, RF-21, RF-22, RF-23, S-5
  - Hecho cuando:
    - [x] `template/.agents/skills/sdd/plantillas.md` tiene `## 07_casos_prueba.md`, entre `06_flujo.md` y el final, con tres partes:
      - Casos. Cada caso lleva RF, datos de partida, destino (`esquema://host[:puerto]`, `local` o `ninguno`), pasos numerados, resultado esperado, herramienta o a mano, y si escribe (`sí` o `no`; la duda cuenta como `sí`).
      - Corridas, con los estados cerrados `obtenido`, `bloqueado`, `manual`, `ok` y `falla`.
      - Evidencia formal opcional.
    - [x] Nota de reglas:
      - Un caso que usa una aplicación con BD, incluso solo para iniciar sesión o navegar, escribe.
      - De las credenciales solo se escribe el nombre de la variable.
      - Una comparación es literal según S-5.
    - [x] Nota de compatibilidad: un `07_pruebas_qa.md` existente se conserva sin renombrar, y validate lo lee.
    - [x] L en el archivo. Evidencia de datos: No aplica. Verificación manual: No aplica.

- [x] **F4.2 Árbol de carpeta y enlace en Pruebas.** RF-21
  - Hecho cuando:
    - [x] El árbol de `SKILL.md` cambia la línea de `07_pruebas_qa.md` por `07_casos_prueba.md` (casos, corridas y evidencia, si hay pasos verificables por ejecución).
    - [x] `avance-y-tiempos.md` › Pruebas dice que, si existe `07_casos_prueba.md`, los resultados y las confirmaciones manuales van ahí y el avance solo enlaza. Sin el documento, todo sigue como hoy.
    - [x] L en los dos archivos. Evidencia de datos: No aplica. Verificación manual: No aplica.

- [x] **F4.3 `tasks` genera los casos.** RF-15, RF-16, RF-17, RF-22, RF-28
  - Hecho cuando:
    - [x] `template/.agents/commands/sdd-tasks.md` dice:
      - Si hay pasos verificables por ejecución, el planner genera `07_casos_prueba.md` con un caso por comprobación, derivado de los RF.
      - Con la herramienta de apoyo de `AGENTS.md` › Comandos, los pasos se redactan con sus acciones. Sin ella, el caso es manual.
      - Sin pasos verificables por ejecución, no se crea el documento.
      - Un feature con `tasks` ya aprobadas sigue como estaba.
    - [x] `template/.agents/agents/planner.md` suma `07_casos_prueba.md` a lo que escribe.
    - [x] L en los dos archivos. Evidencia de datos: No aplica. Verificación manual: No aplica.

- [x] **F4.4 `validate` con `tester` antes del reviewer.** RF-20, RF-21, RF-22, RF-23, RF-24
  - Hecho cuando:
    - [x] `template/.agents/commands/sdd-validate.md` dice:
      - Quien atiende la fase como sesión lanza `tester` con los casos ejecutables y anota las corridas.
      - La sesión marca `ok` o `falla` solo si la comparación es literal. Si requiere juicio, la decide el reviewer, que da el veredicto con las corridas.
      - `tester` y el reviewer forman una sola vuelta del tope de 2.
    - [x] El punto 5 queda reescrito: la evidencia formal va en `07_casos_prueba.md`, y el harness de `tests/` se conserva. Se mantiene la nota de compatibilidad con `07_pruebas_qa.md`.
    - [x] L en el archivo. Evidencia de datos: No aplica. Verificación manual: No aplica.

- [x] **F4.5 Reviewer con corridas.** RF-20, RF-23, RF-2
  - Hecho cuando:
    - [x] `template/.agents/agents/reviewer.md`:
      - Da el veredicto con las corridas de `tester` y decide los `ok` o `falla` que requieren juicio.
      - Lee un `07_pruebas_qa.md` existente como evidencia.
      - Devuelve el contenido de la evidencia formal y no escribe `07_casos_prueba.md` ni `tests/`.
      - Dice «E1–E23».
    - [x] L en el archivo. Evidencia de datos: No aplica. Verificación manual: No aplica.

- [x] **F4.6 Comprobar las menciones de `07_pruebas_qa.md`.** RNF
  - Hecho cuando:
    - [x] `grep -rn "07_pruebas_qa" template README.md INSTALAR.md ACTUALIZAR.md` solo da la nota de la plantilla, `sdd-validate.md` y `reviewer.md`. No edita archivos.
    - [x] Evidencia de datos: No aplica. Verificación manual: No aplica.

## F5 — Entornos y Guardrails (estimado: 35 min)
Fuente: `AGENTS.md` › Guardrails, regla 1. Puede ir en paralelo con F3 y F4: los archivos son disjuntos.

- [x] **F5.1 Excepción de QA en `template/AGENTS.md`.** RF-24, RF-25, RF-28
  - Hecho cuando:
    - [x] El preámbulo de Guardrails dice que ninguna skill, comando, plan ni preferencia los relaja, salvo las excepciones escritas en sus propias reglas.
    - [x] La regla 1 suma una sub-viñeta de excepción de QA:
      - Solo `tester`, y solo contra los destinos de `.agents/local/entornos.md`.
      - Solo casos de pantalla o de llamada a la aplicación. Nunca sentencias, `sql/` ni shell que escriba.
      - Sin configuración válida, modo seguro.
      - Es la única excepción.
    - [x] Comandos suma la línea «Herramienta de apoyo para casos: <completar o "No aplica">».
    - [x] L en el archivo, con los marcadores permitidos. Evidencia de datos: No aplica. Verificación manual: No aplica.

- [x] **F5.2 El mismo texto en el `AGENTS.md` de la raíz.** RF-25, D5
  - Hecho cuando:
    - [x] `diff` de la sección Guardrails entre la raíz y `template/` sale vacío.
    - [x] Comandos de la raíz dice «Herramienta de apoyo para casos: No aplica».
    - [x] L en el archivo. Evidencia de datos: No aplica. Verificación manual: No aplica.

- [x] **F5.3 Crear `entornos.ejemplo.md`.** RF-19, RF-26, RF-27, S-8
  - Hecho cuando:
    - [x] `template/.agents/entornos.ejemplo.md` explica que se copia a `.agents/local/entornos.md`, que es de cada persona y no se versiona.
    - [x] El formato:
      - Sección `## QA` con una línea `- esquema://host[:puerto]` por destino.
      - Una línea distinta o un destino repetido se toma como formato roto, y entonces rige el modo seguro.
      - Credenciales solo por nombre de variable de entorno.
    - [x] Advierte que declarar un destino afirma que su BD, y todo lo que su página llama en segundo plano, es de QA (S-8).
    - [x] No tiene valores de credenciales. Los ejemplos de host son genéricos (`https://qa.ejemplo.test`).
    - [x] L en el archivo. Evidencia de datos: No aplica. Verificación manual: No aplica.

## F6 — Instalación, actualización y estructura (estimado: 40 min)
Depende de F1 y F2.

- [x] **F6.1 Chequeo E23.** RF-2
  - Hecho cuando:
    - [x] `template/.agents/validar-estructura.md` tiene E23 (agentes), que compara el `model:` de cada agente con la tabla «Modelo por rol» del proyecto. Solo `AVISO`. Un agente personalizado distinto avisa y no falla.
    - [x] Las líneas «E1 a E22» y «E1–E22» pasan a E23.
    - [x] `template/.agents/commands/sdd-estructura.md` dice E1 a E23.
    - [x] L en los dos archivos. Evidencia de datos: No aplica. Verificación manual: No aplica.

- [x] **F6.2 `sdd-init` aplica los modelos por omisión.** RF-5, S-2, S-3
  - Hecho cuando:
    - [x] `template/.agents/commands/sdd-init.md` elige el valor así:
      - Lee las herramientas de `.agents/local/kit-visto` o, si no existe, las carpetas `.claude/`, `.cursor/`, `.gemini/`, `.github/` (solo con `copilot-instructions.md` o `agents/`) y `.kiro/`.
      - Solo Claude Code: la tabla.
      - Solo otras herramientas: `inherit` en los agentes y en la tabla del proyecto.
      - Las dos: la tabla.
    - [x] Dice que no hace preguntas nuevas (las hará la 0014) y remite a la tabla sin copiarla.
    - [x] L en el archivo. Evidencia de datos: No aplica. Verificación manual: No aplica.

- [x] **F6.3 `ACTUALIZAR.md` para agentes.** RF-6, RF-28, S-1, S-7
  - Hecho cuando:
    - [x] `ACTUALIZAR.md` dice:
      - Al reemplazar un agente KIT-VIEJO se aplica el modelo por omisión de RF-5 (remite a `sdd-init`). Si la tabla del proyecto está personalizada, se aplica la de esa tabla (S-7).
      - Un agente personalizado conserva su modelo.
      - Un `tester.md` que solo difiere en entradas `mcp__` de `tools` no cuenta como personalizado, y esas entradas se conservan (S-1).
    - [x] Recuerda que la excepción de QA y los roles en `AGENTS.md` son grupo B y se incorporan a mano.
    - [x] L en el archivo. Evidencia de datos: No aplica. Verificación manual: No aplica.

## F7 — ADR (estimado: 20 min)
No toca `template/`. Plantilla de la skill `adr`. Fecha: `date '+%Y-%m-%d'` del día del paso.

- [x] **F7.1 ADR 0028 y su fila.** RF-29
  - Hecho cuando:
    - [x] Existe `.docs/004_adr/0028_ModeloPorRolYRolEnElChat.md`, `Aceptado`, origen `0013_ModelosPorAgente`. Decide:
      - Modelos por rol.
      - Rol en el chat salvo cambio de modelo, también el reviewer, con la separación por rol.
      - Sonnet para el implementer sin comparación previa: se mide el tiempo por paso y las vueltas de validate, y se revisa tras 3 features.
      - Excepción de QA a la regla 1.
    - [x] Alternativas descartadas: `inherit` (ADR 013), comparación previa y `tester` siempre como subagente.
    - [x] `.docs/004_adr/README.md` tiene la fila `0028`.
    - [x] L en los dos archivos. Evidencia de datos: No aplica. Verificación manual: No aplica.

- [x] **F7.2 ADR 013 reemplazado.** RF-29
  - Hecho cuando:
    - [x] `013_ModeloExplicitoPorRol.md` dice `Estado: Reemplazado por 0028` con enlace. No se reescribe el resto.
    - [x] La fila `013` del índice dice `Reemplazado`.
    - [x] L en los dos archivos. Evidencia de datos: No aplica. Verificación manual: No aplica.

## F8 — Release y réplica (estimado: 30 min)
Depende de F1 a F7. Después de F8.2, `template/` no se edita.

- [x] **F8.1 Versión `0.9.0` y changelog.** Criterio de finalización
  - Hecho cuando:
    - [x] `VersionAgentKitSdd` tiene una sola línea: `0.9.0`.
    - [x] `ChangelogAgentKitSdd.md` abre `## 0.9.0 — <fecha>` encima de `0.8.0`, con:
      - Modelos por rol y roles `tester` y `explore`.
      - Rol en el chat.
      - `07_casos_prueba.md`.
      - Excepción de QA.
      - E23.
      - Nota de grupo B: `AGENTS.md` (Guardrails, roles y Comandos) se incorpora a mano, y hasta entonces `tester` queda en modo seguro.
      - Proyectos solo con otras herramientas: `inherit`.
    - [x] L en los dos archivos. Evidencia de datos: No aplica. Verificación manual: No aplica.

- [x] **F8.2 Manifiesto `0.9.0`.** E20, criterio de finalización
  - Hecho cuando:
    - [x] La prueba de humo da otra vez las tres huellas.
    - [x] `manifiestos/0.9.0.md` tiene el formato de `0.8.0.md`: una fila por archivo regular de `template/`, ordenadas con `LC_ALL=C sort`, sin filas de más ni de menos. Incluye `tester.md`, `explore.md` y `entornos.ejemplo.md`.
    - [x] Los manifiestos `0.4.0` a `0.8.0` no cambiaron y las huellas no se editaron a mano.
    - [x] L en el archivo, sin exigir que existan las rutas. Evidencia de datos: No aplica. Verificación manual: No aplica.

- [x] **F8.3 Réplica a `.agents/` y E19.** —
  - Hecho cuando:
    - [x] Se copió a `.agents/` de la raíz cada archivo cambiado o nuevo de `template/.agents/`.
    - [x] `diff -rq template/.agents .agents`, ignorando `.agents/local/`, muestra solo la línea base de F0.1.
    - [x] `.claude/agents` lista `tester.md` y `explore.md` (por el enlace).
    - [x] `git status` no muestra `.agents/`, salvo `napkin.md`, que no se tocó.
    - [x] Evidencia de datos: No aplica. Verificación manual: No aplica.

- [x] **F8.4 Ensayo de instalación y lint final.** Criterios de finalización
  - Hecho cuando:
    - [x] El ensayo de `AGENTS.md` › Comandos en `mktemp -d` copia todo, incluidos los archivos nuevos, sin pisar nada.
    - [x] L en cada archivo tocado por el feature (lista de `git status` y de F8.3).
    - [x] `grep -rn "model: inherit" template/.agents/agents` sale vacío.
    - [x] La comprobación de F4.6 sigue igual.
    - [x] Evidencia de datos: No aplica. Verificación manual: lanzar `explore` en Claude Code se comprueba en `/sdd-validate`.

## F9 — Cambio 1: comprueba quien lanza y respaldo (estimado: 30 min)
Decisiones 32 y 33 de `05_avance.md`. Fuentes únicas en `03_plan.md` › §2 › «Cambio 1». Depende de F1 a F8. F9.1 y F9.2 van antes que F9.3 y F9.4. F9.6 va al final, después del último cambio en `template/`.

- [x] **F9.1 Quien lanza comprueba el encargo.** RF-12, RF-13
  - Hecho cuando:
    - [x] `template/.agents/skills/sdd/restricciones-de-rol.md` › «Roles Haiku» dice:
      - Antes de lanzar un rol Haiku, quien lo lanza comprueba que el encargo trae objetivo, alcance, pasos o casos y formato de salida, y que no pide diseñar, decidir, elegir ni juzgar. Si falta algo o lo pide, no lo lanza así: completa el encargo o hace esa parte él.
      - El orden de comprobación de la vuelta 1 de validate se conserva como segunda barrera del rol, que es solo instrucción.
    - [x] L en el archivo. Evidencia de datos: No aplica. Verificación manual: No aplica.

- [x] **F9.2 Relanzamiento y paso a la sesión.** RF-30, RF-4
  - Hecho cuando:
    - [x] `template/.agents/skills/sdd/SKILL.md` › «Dónde corre cada rol» suma:
      - Qué cuenta como no entregar: no responde; devuelve fuera del formato; se sale del alcance; rellena con supuestos; o rechaza un encargo ya comprobado. Ese resultado se descarta.
      - Un solo relanzamiento del mismo rol con el mismo encargo y el modelo del implementer. Remite a «Modelo por rol», no nombra el alias.
      - Si vuelve a fallar, la sesión principal hace la tarea en el chat con la definición del rol, y sus restricciones pasan a ser instrucción.
      - Si la herramienta no deja elegir el modelo al lanzar, tras el primer fallo la tarea pasa a la sesión.
      - Cada intento queda en la Nota de la fila con su modelo.
    - [x] L en el archivo. Evidencia de datos: No aplica. Verificación manual: No aplica.

- [x] **F9.3 Coordinator y validate remiten.** RF-12, RF-30
  - Hecho cuando:
    - [x] `template/.agents/agents/coordinator.md` y `template/.agents/commands/sdd-validate.md` dicen que, antes de lanzar `tester` o `explore`, se comprueba el encargo según «Roles Haiku», y que si no entrega se aplica «Dónde corre cada rol». Remiten y no copian.
    - [x] L en los dos archivos. Evidencia de datos: No aplica. Verificación manual: No aplica.

- [x] **F9.4 Segunda barrera en los Haiku.** RF-12
  - Hecho cuando:
    - [x] `template/.agents/agents/explore.md` y `tester.md` dicen que quien los lanza ya comprobó el encargo y que su comprobación es la segunda barrera. Conservan lo de la vuelta 1: primero comprobar y sin supuestos.
    - [x] L en los dos archivos. Evidencia de datos: No aplica. Verificación manual: No aplica.

- [x] **F9.5 ADR 0028 y changelog.** RF-29
  - Hecho cuando:
    - [x] `.docs/004_adr/0028_ModeloPorRolYRolEnElChat.md` suma la consecuencia: el encargo lo comprueba quien lanza, con respaldo en el modelo del implementer y después en la sesión. Suma como alternativa descartada aceptar RF-12 con deuda.
    - [x] `ChangelogAgentKitSdd.md` › 0.9.0 suma una línea con eso.
    - [x] L en los dos archivos. Evidencia de datos: No aplica. Verificación manual: No aplica.

- [x] **F9.6 Manifiesto, réplica y ensayo.** Criterios
  - Hecho cuando:
    - [x] `manifiestos/0.9.0.md` se regenera con el mismo procedimiento de F8.2: humo con tres huellas, una fila por archivo de `template/` y huellas con `kit_huella`.
    - [x] Se copiaron a `.agents/` los archivos de F9.1 a F9.4. `diff -rq template/.agents .agents` muestra solo la línea base.
    - [x] El ensayo de instalación en `mktemp -d` no pisa nada. L en cada archivo de F9.
    - [x] Evidencia de datos: No aplica. Verificación manual: en validate (vuelta 2), un encargo incompleto no se lanza, y un fallo se relanza con el modelo del implementer.
