# 04 — Tareas: Modelo por rol y permisos del frontmatter de los agentes
Leyenda: `[ ]` Pendiente · `[~]` En curso · `[x]` Hecho · `[!]` Bloqueado
Spec: `01_spec.md` (aprobada, 21 RF, 7 RNF) · Plan: `03_plan.md` §7 · Total estimado: 205 min

Nada de esta lista está hecho. Las fases son las de `03_plan.md` §7 (F0 a F7). Ninguna se llama Cierre. `/sdd-cierre` no es un paso: corre cuando el usuario lo pida, fuera de estas fases. Los ADR 010, 011 y 012 se proponen en ese cierre; este archivo no los crea. La deuda `003_ModeloYPermisosDeAgentes` sigue `En atención`; ninguna fase edita su archivo.

## Reglas comunes a todos los pasos
- **Lint L.** Cuando un paso diga «Lint L en `<archivo>`», sobre cada archivo tocado (`AGENTS.md` › Comandos):
  1. `file -i <archivo>` da `charset=utf-8` (o `us-ascii`).
  2. `grep -l $'^\xEF\xBB\xBF' <archivo>` no devuelve nada (sin BOM).
  3. `name` del frontmatter: en una skill, el de su carpeta (`sdd` en `SKILL.md`); en un agente, el del archivo sin `.md` (`03_plan.md` §7, F7). La carpeta `agents` no es ese `name`. Un comando, `restricciones-de-rol.md`, `AGENTS.md`, `CHANGELOG.md`, `05_avance.md` y el manifiesto no llevan `name`: el punto 3 no aplica.
  4. Cada ruta citada existe. No aplica a `manifiestos/`: pueden listar rutas retiradas.
  5. En `template/`: sin marcadores `<<…>>` salvo `<<NOMBRE_PROYECTO>>` e `<<IDIOMA>>`. Un archivo de la raíz está fuera de `template/`.
- **Convenciones.** UTF-8 sin BOM, español, solo Markdown. Sin scripts. Los únicos nombres de modelo son `sonnet` e `inherit`.
- **Evidencia de datos.** `AGENTS.md` › Comandos: consulta de datos de solo lectura, No aplica. Un `wc -c` no es esa consulta: su cifra se anota en `05_avance.md`.
- **Verificación manual UI.** No aplica. No hay pantallas.
- **Quién edita.** Relee el archivo en ese momento. Toca solo la parte de este feature. Si trae líneas de otra spec, se quedan. Prohibido `git checkout` y `git restore`. No se revierte trabajo ajeno.
- **Qué no se edita.** Carpeta `003_OptimizarArnes`, carpeta `005_ConvivenciaNumeracion`, `sdd-init.md`, `validar-estructura.md`, `ACTUALIZAR.md`, ADR 004, ADR 007, ADR 008, ADR 009, el archivo de la deuda 003, `VERSION`, la copia local `.agents/`, `.claude/` y `sql/`. No se crean `.gemini/agents` ni `.github/agents`. No se edita `02_analisis_impacto.md`.
- **Cifras.** Si un `wc -c` no coincide con `02` §3, vale el nuevo y se anota. El `02` no se reescribe.
- **`05_avance.md`.** Lo escribe solo la sesión principal. Un subagente no lo escribe.
- **Un paso a la vez.** Al cerrarlo se marca su casilla en este archivo y se para.
- **Ensayos** solo en `mktemp -d`. Nunca en un proyecto real. Sin `git commit`, `push`, `pull`, `fetch` ni `sync`.

## Orden
F0 antes de cualquier edición de `template/` o de docs de la raíz. F2 puede seguir a F0 sin esperar a F1. F3 espera el resultado de F1 y el archivo de F2. F4 después de F3. F5 después de F2, F3 y F4. F6 después de F5. F7 al final. Si `template/` cambia después de empezar F7, o después de cerrarla, se repite F7 desde F7.1.

## Fuera de estas fases
Lo hace `/sdd-cierre`, no un `Fn.m`: marcar la deuda 003 `Resuelta` (con «Atendida por» y «Cómo se atendió»); proponer, sin crear, la deuda de que `/sdd-init` sugiera los fragmentos `allow` y `deny`; proponer los ADR 010 (`RestriccionesDeRolSinHooks`), 011 (`OmitClaudeMdEnTresRoles`) y 012 (`ModeloExplicitoPorRol`) con el `model` que F1 haya aceptado; proponer en `MEMORY.md`, sin escribirla, que no hay revisión en la que F1.4 de `001_ActualizarKit` ya esté y F1.5 todavía no. La fila 004 del índice de specs se corrige en ese cierre (decisión 11 del plan). El número de ADR se vuelve a mirar en local y en la remota ya descargada; no se hace `fetch`.

---

## F0 — Línea base en el avance (estimado: 15 min)
La hace la sesión principal. No edita `template/` ni docs de la raíz. RF-13, RF-18.
- [x] **F0.1 Comprobar la precondición y el árbol.** (4 min) RF-18
  - Hacer: sin editar la carpeta `003_OptimizarArnes`, confirmar que su fila en `.docs/002_specs/README.md` sigue en Implementado y que el PUNTO DE RETOMO de su `05_avance.md` sigue en cerrada. En la sesión que va a medir, `CLAUDE_CODE_SUBAGENT_MODEL` y `CLAUDE_CODE_SUBAGENT_MODEL_FORCE` están unset; si no, F1 no arranca y este paso queda `[!]`. `git status --short` de la lista de `03_plan.md` §2. Si un archivo de esa lista tiene cambios ajenos, se anota y no se revierten. Anotar el resultado en `05_avance.md`.
  - Hecho cuando:
    - [x] La `003` sigue cerrada en el índice y en su PUNTO DE RETOMO, y esta carpeta no la editó
    - [x] Las dos variables están unset, o F1 quedó bloqueada con el motivo anotado
    - [x] El `git status` de la lista de §2 está anotado; nada ajeno se revirtió
    - [x] Lint L en `.docs/002_specs/004_ModeloYPermisosAgentes/05_avance.md` (`file -i` utf-8, sin BOM; el punto 3 no aplica; rutas citadas existentes)
    - [x] Convenciones de archivo de `AGENTS.md` (encoding, formato) respetadas
    - [x] Evidencia de datos: No aplica
    - [x] Verificación manual UI: No aplica
- [x] **F0.2 Medir y copiar la línea base.** (8 min) RF-13, RNF-7
  - Hacer: `wc -c` de los 4 agentes, `template/CLAUDE.md`, `template/AGENTS.md`, `template/.agents/skills/sdd/SKILL.md`, `template/.agents/commands/sdd-validate.md`, los cuatro valores de `description`, el conteo de archivos de `template/` y las filas de datos de `manifiestos/0.5.0.md`. Heredado = bytes de `template/CLAUDE.md` + bytes de `template/AGENTS.md`. Arranque de hoy = bytes del agente + heredado. El neto hipotético de `omitClaudeMd` (arranque de hoy menos el heredado) se anota aquí; el campo no se pone. Copiar esas cifras en `05_avance.md` antes de cambiar `template/`. Si no coinciden con `02` §3, vale la medición y se anota la diferencia. No se toca el `02`.
  - Hecho cuando:
    - [x] `05_avance.md` tiene heredado, arranque por rol, bytes de cada `description` y su suma, archivos de `template/` y filas del manifiesto, con fecha
    - [x] El neto hipotético del `implementer` está anotado y `omitClaudeMd` no se declaró
    - [x] `template/` y los docs de la raíz no cambiaron en este paso
    - [x] Lint L en `05_avance.md` (`file -i` utf-8, sin BOM; el punto 3 no aplica)
    - [x] Convenciones de archivo de `AGENTS.md` (encoding, formato) respetadas
    - [x] Evidencia de datos: No aplica
    - [x] Verificación manual UI: No aplica
- [x] **F0.3 Escribir las 5 peticiones antes de lanzarlas.** (3 min) RF-4
  - Hacer: copiar en `05_avance.md`, antes de la primera corrida de F1, esta lista fija. La petición 1 no coincide con ninguna carpeta del índice en el momento de medir (tampoco con `005` ni `006`).
    1. Petición libre de análisis nuevo. Respuesta esperada: elige `sdd-spec` y no implementa.
    2. Spec aprobada que pide el plan. Respuesta esperada: elige `sdd-plan` y no pasa a implementar.
    3. Pide implementar un feature cuya copia no tiene `04_tasks.md` aprobado. Respuesta esperada: frena en la puerta. No escribe código. Se anota el comando que elige.
    4. Pide validar un feature con la implementación terminada (`003_OptimizarArnes`, mientras siga cerrada). Respuesta esperada: elige `sdd-validate` y no abre una implementación.
    5. El `AGENTS.md` de la copia es el de `template/` intacto (lleva el comentario `SDD-INIT-PENDIENTE`). Pide una spec. Respuesta esperada: para, sugiere `/sdd-init` y no ejecuta ningún `/sdd*`.
  - Hecho cuando:
    - [x] Las 5 peticiones y su respuesta esperada están en `05_avance.md`, y la 1 no choca con el índice
    - [x] Aún no hubo corrida
    - [x] Lint L en `05_avance.md` (`file -i` utf-8, sin BOM; el punto 3 no aplica)
    - [x] Convenciones de archivo de `AGENTS.md` (encoding, formato) respetadas
    - [x] Evidencia de datos: No aplica
    - [x] Verificación manual UI: No aplica

## F1 — Comparación del coordinator (estimado: 50 min)
Requisito: F0 cerrada. La sesión principal lanza al coordinator. El implementer no se lanza, no lanza subagentes y no escribe `05_avance.md`. Las dos corridas de un caso usan el mismo texto de agente (el actual, antes de F3): el modelo entra como parámetro de la llamada, que gana al frontmatter. El resultado es del modelo y no de `omitClaudeMd`. RF-4, RF-5, RF-11.
- [x] **F1.1 Armar las copias desechables.** (8 min) RF-4
  - Hacer: dos árboles en `mktemp -d`, fuera del repo, sin versionar. No se usa el `AGENTS.md` de la raíz: el literal `SDD-INIT-PENDIENTE` aparece dentro de la regla y da un falso positivo. Casos 1 a 4: el `AGENTS.md` de esa copia no contiene el literal en ninguna parte (comentario, regla ni tabla); el caso 3 apunta a un feature de esa copia sin `04_tasks.md` aprobado. Caso 5: el `AGENTS.md` es el de `template/` intacto. La copia incluye el índice de specs y el texto actual del coordinator. Nada se escribe en el repo real.
  - Hecho cuando:
    - [x] Los casos 1 a 4 y el caso 5 están en temporales distintos, y el de 1 a 4 no contiene el literal
    - [x] El caso 5 conserva el comentario de `template/AGENTS.md`
    - [x] El repo real no cambió y los temporales no están dentro de él
    - [x] Lint: No aplica (no se edita un archivo del repo)
    - [x] Convenciones de archivo de `AGENTS.md`: No aplica
    - [x] Evidencia de datos: No aplica
    - [x] Verificación manual UI: No aplica
- [x] **F1.2 Corrida con el modelo actual.** (18 min) RF-4, RF-5
  - Hacer: una corrida por caso con el modelo de la sesión. Anotar su nombre exacto, el comando elegido, si respetó la puerta, los bytes que la instrucción manda leer, los bytes de la respuesta (`wc -c`) y los reintentos. Si el alias de la sesión no es el que se va a comparar, esta corrida es la del actual. No se anota un pase del propuesto.
  - Hecho cuando:
    - [x] Los 5 casos tienen comando, puerta, bytes y reintentos, con el nombre exacto del modelo actual
    - [x] No se escribió en el repo real
    - [x] Lint L en `05_avance.md` (`file -i` utf-8, sin BOM; el punto 3 no aplica)
    - [x] Convenciones de archivo de `AGENTS.md` (encoding, formato) respetadas
    - [x] Evidencia de datos: No aplica
    - [x] Verificación manual UI: No aplica
- [x] **F1.3 Corrida con `sonnet` y umbral.** (18 min) RF-4, RF-5, RNF-1
  - Hacer: una corrida por caso con `sonnet`. Si la sesión ya está en el mismo id que resuelve `sonnet`, esta corrida no se duplica: se anota la igualdad y basta la de F1.2. Si el propuesto falla un caso, se repite ese caso una vez. Si la repetición también falla, el rol no pasa. Se acepta solo si cumple todo lo que cumplió el actual. Por caso, los bytes que la instrucción manda leer más los bytes de la respuesta no superan el 125 % de los del actual, y los reintentos no son más que los del actual. Si el alias cae al heredado, o `CLAUDE_CODE_SUBAGENT_MODEL_FORCE` ignora el frontmatter, esa corrida no cuenta: se anota y se repite, o el coordinator queda en `inherit`. Si no hay una corrida válida, no se anota un pase.
  - Hecho cuando:
    - [x] Quedó anotado el pase o el no pase, con nombres exactos, umbral por caso y si hubo repetición
    - [x] Una corrida caída al heredado, o ignorada por `_FORCE`, no está contada como pase
    - [x] Lint L en `05_avance.md` (`file -i` utf-8, sin BOM; el punto 3 no aplica)
    - [x] Convenciones de archivo de `AGENTS.md` (encoding, formato) respetadas
    - [x] Evidencia de datos: No aplica
    - [x] Verificación manual UI: No aplica
- [x] **F1.4 Anotar el modelo aceptado y borrar las copias.** (6 min) RF-4, RF-5, RF-11, RNF-2
  - Hacer: el `model` del coordinator que F3 va a escribir es el que esta fase aceptó (`sonnet` solo con corrida válida; si no la hay, `inherit`). El implementer no entró en las corridas. Anotar: declara `model: inherit`; no hay árbol previo a F1.5 de `001_ActualizarKit`; no se fabrica un fixture (`02` §3.2 y §6). El campo `omitClaudeMd` no se le pone. Borrar los temporales.
  - Hecho cuando:
    - [x] `05_avance.md` dice el `model` aceptado del coordinator y el `inherit` del implementer, con el motivo de §3.2 y §6
    - [x] Los temporales ya no existen y el repo real no recibió su contenido
    - [x] Lint L en `05_avance.md` (`file -i` utf-8, sin BOM; el punto 3 no aplica)
    - [x] Convenciones de archivo de `AGENTS.md` (encoding, formato) respetadas
    - [x] Evidencia de datos: No aplica
    - [x] Verificación manual UI: No aplica

## F2 — Pieza de restricciones (estimado: 30 min)
Requisito: F0 cerrada. No espera a F1. La hace el implementer.
- [x] **F2.1 Crear `restricciones-de-rol.md`.** (25 min) RF-1, RF-2, RF-5, RF-6, RF-7, RF-9, RF-10, RF-17, RNF-6
  - Hacer: crear `template/.agents/skills/sdd/restricciones-de-rol.md`, en español, sin frontmatter de skill (igual que `avance-y-tiempos.md`) y sin scripts. Lleva:
    - Registro de campos con fecha 2026-10-25. URL de subagentes: la de la decisión 2 del avance (`https://code.claude.com/docs/en/sub-agents`). Los hechos de permisos y de `disallowedTools` se atribuyen a esa lectura (decisiones 2, 3 y 11). No hay URL de la página de permisos en el avance: no se inventa; se escribe «documentación de permisos de Claude Code, leída el 2026-10-25».
    - Por campo: soportado en Claude Code, ignorado, o no verificado. `mode` y `permissions`: formato de otra herramienta, ignorados por Claude Code. `model`, `omitClaudeMd`, `mode` y `permissions`: no verificado en Cursor, Gemini y Copilot. Quitar los dos últimos puede quitarles una función que aquí no se conoce. Hechos (a) a (d) de RF-1. Además: los hooks por subagente existen y se descartan; `omitClaudeMd` no afecta a los `CLAUDE.md` de política; un alias ausente o bloqueado cae al heredado con aviso y el respaldo es `inherit`.
    - La tabla de RF-7, ocho filas, con el texto de la spec (no un resumen que cambie la garantía). Filas 2 y 3: no garantizada, instrucción; el deny de ruta no se incluye (decisión 12).
    - Fragmento opcional `permissions.deny` de `03_plan.md` §4: `Bash(git commit *)`, `Bash(git push *)`, `Bash(git fetch *)`, `Bash(git pull *)` y `Bash(git sync *)`, con la advertencia de que vale para toda la sesión y puede bloquear el Git de quien lo copie. Recomendación, no garantía del kit. No se copia a `.claude/`.
    - Fragmento opcional `permissions.allow` de RF-10, genérico: el responsable pone los comandos de su `AGENTS.md` › Comandos. Sin columna «garantizada». `/sdd-init` no se menciona como quien lo aplica. Ningún agente lo aplica ni cambia su modo de permisos.
    - Límite conocido: la herramienta no carga solo secciones de `AGENTS.md`. `omitClaudeMd` quita todo el `CLAUDE.md`. Si un proyecto no importa `AGENTS.md` desde el suyo, el ahorro es cero.
  - Hecho cuando:
    - [x] Hay fecha, fuente, los hechos (a) a (d) y el rótulo no verificado en Cursor, Gemini y Copilot
    - [x] La tabla tiene las 8 filas de RF-7; el `deny` son solo esos cinco patrones; el `allow` no es una fila
    - [x] No hay frontmatter, ni script, ni copia en `.claude/`
    - [x] Lint L en `template/.agents/skills/sdd/restricciones-de-rol.md` (`file -i` utf-8, sin BOM; sin `name`; rutas citadas existentes; sin marcadores `<<…>>`)
    - [x] Convenciones de archivo de `AGENTS.md` (encoding, formato) respetadas
    - [x] Evidencia de datos: No aplica
    - [x] Verificación manual UI: No aplica
- [x] **F2.2 Una frase en `SKILL.md`.** (5 min) RF-7, RNF-4
  - Hacer: en `template/.agents/skills/sdd/SKILL.md`, una sola frase junto al enlace del contrato de avance (`avance-y-tiempos.md`, en «Carpeta del feature»): las restricciones de rol viven en `restricciones-de-rol.md`; se enlaza y no se copia. No se repite junto al segundo enlace de tiempos. No se copia la tabla.
  - Hecho cuando:
    - [x] Hay una sola frase, con enlace, y la tabla no está copiada en `SKILL.md`
    - [x] El `name` sigue siendo `sdd`
    - [x] Se releyó el archivo; las líneas ajenas se quedaron
    - [x] Lint L en `template/.agents/skills/sdd/SKILL.md` (`file -i` utf-8, sin BOM; `name` = `sdd`; rutas citadas existentes; sin marcadores `<<…>>` nuevos)
    - [x] Convenciones de archivo de `AGENTS.md` (encoding, formato) respetadas
    - [x] Evidencia de datos: No aplica
    - [x] Verificación manual UI: No aplica

## F3 — Frontmatter y cuerpo de los 4 agentes (estimado: 25 min)
Requisito: F1 cerrada (hay un `model` aceptado) y F2.1 cerrado (existe la pieza). La hace el implementer. Aplicar la tabla de `03_plan.md` §1. En los cuatro se borran `mode` y `permissions`. No se añade `disallowedTools`, `permissionMode` ni `hooks`. Las `description` quedan en los bytes que midió F0. La frase «No supongas que la herramienta te inyecta `AGENTS.md`» se queda en los cuatro. Cada cuerpo enlaza `.agents/skills/sdd/restricciones-de-rol.md` y no copia la tabla.
- [x] **F3.1 `coordinator.md`.** (7 min) RF-3, RF-6, RF-11, RF-12
  - Hacer: `tools` se quedan `Agent(planner, implementer, reviewer), Read, Glob, Grep, Write, Edit, Bash`. `model` es el que F1 anotó como aceptado. `omitClaudeMd: true`. El cuerpo enlaza la pieza y nombra «Git» y «Reglas duras», sin copiar su redacción y sin reescribir el enrutado. La lista `Agent(...)` se queda.
  - Hecho cuando:
    - [x] `model` es el de F1; si F1 no tuvo corrida válida, es `inherit`
    - [x] Siguen los mismos `tools`, con la lista `Agent(...)`; hay `omitClaudeMd: true`; no hay `mode` ni `permissions`
    - [x] El cuerpo enlaza la pieza y nombra «Git» y «Reglas duras»; el enrutado no se reescribió
    - [x] La `description` tiene los mismos bytes que F0
    - [x] Lint L en `template/.agents/agents/coordinator.md` (`file -i` utf-8, sin BOM; `name` = `coordinator`; rutas citadas existentes; sin marcadores `<<…>>`)
    - [x] Convenciones de archivo de `AGENTS.md` (encoding, formato) respetadas
    - [x] Evidencia de datos: No aplica
    - [x] Verificación manual UI: No aplica
- [x] **F3.2 `planner.md`.** (6 min) RF-3, RF-6, RF-11, RF-12
  - Hacer: `tools` se quedan `Read, Glob, Grep, Write, Edit, Bash`. `model: inherit`. `omitClaudeMd: true`. El cuerpo enlaza la pieza y nombra «Git» y «Reglas duras», sin copiar su redacción.
  - Hecho cuando:
    - [x] Siguen `Write` y `Edit`; `model` es `inherit`; hay `omitClaudeMd: true`; no hay `mode` ni `permissions`
    - [x] El cuerpo enlaza la pieza y nombra «Git» y «Reglas duras»
    - [x] La `description` tiene los mismos bytes que F0
    - [x] Lint L en `template/.agents/agents/planner.md` (`file -i` utf-8, sin BOM; `name` = `planner`; rutas citadas existentes; sin marcadores `<<…>>`)
    - [x] Convenciones de archivo de `AGENTS.md` (encoding, formato) respetadas
    - [x] Evidencia de datos: No aplica
    - [x] Verificación manual UI: No aplica
- [x] **F3.3 `implementer.md`.** (5 min) RF-3, RF-4, RF-6, RF-11
  - Hacer: `tools` se quedan `Read, Glob, Grep, Write, Edit, Bash`. `model: inherit`. Sin `omitClaudeMd`. Sin `mode` ni `permissions`. El cuerpo solo gana el enlace a la pieza. Conserva «no registras en Git», «No escribes `05_avance.md`» y «No ejecutas SQL».
  - Hecho cuando:
    - [x] `model` es `inherit`; no existe la clave `omitClaudeMd`; no hay `mode` ni `permissions`
    - [x] Esas tres frases siguen, y el cuerpo enlaza la pieza sin copiar la tabla
    - [x] La `description` tiene los mismos bytes que F0
    - [x] Lint L en `template/.agents/agents/implementer.md` (`file -i` utf-8, sin BOM; `name` = `implementer`; rutas citadas existentes; sin marcadores `<<…>>`)
    - [x] Convenciones de archivo de `AGENTS.md` (encoding, formato) respetadas
    - [x] Evidencia de datos: No aplica
    - [x] Verificación manual UI: No aplica
- [x] **F3.4 `reviewer.md`.** (7 min) RF-6, RF-7, RF-8, RF-11, RF-12, RF-20, RF-21
  - Hacer: `tools` quedan `Read, Glob, Grep, Bash`. Sin `Write` y sin `Edit`. `model: inherit`. `omitClaudeMd: true`. El cuerpo deja de decir que escribe `07_pruebas_qa.md` y `tests/`. Enlaza `sdd-validate` (ahí vive el procedimiento) y la pieza, y nombra «Git» y «Reglas duras». Aunque el comando que tenga delante sea el anterior, devuelve el contenido y no escribe esos archivos (RF-21 a).
  - Hecho cuando:
    - [x] `tools` no incluye `Write` ni `Edit`; `model` es `inherit`; hay `omitClaudeMd: true`; no hay `mode` ni `permissions`
    - [x] El cuerpo ya no le manda escribir `07_pruebas_qa.md` ni `tests/`; enlaza el comando y la pieza
    - [x] Nombra «Git» y «Reglas duras»; la `description` tiene los mismos bytes que F0
    - [x] Lint L en `template/.agents/agents/reviewer.md` (`file -i` utf-8, sin BOM; `name` = `reviewer`; rutas citadas existentes; sin marcadores `<<…>>`)
    - [x] Convenciones de archivo de `AGENTS.md` (encoding, formato) respetadas
    - [x] Evidencia de datos: No aplica
    - [x] Verificación manual UI: No aplica

## F4 — Validación y «Qué lee cada rol» (estimado: 20 min)
Requisito: F3 cerrada. La hace el implementer. El comando y el agente dicen lo mismo: el procedimiento queda solo en el comando (F4.1); el agente ya lo enlazó en F3.4.
- [x] **F4.1 Paso 5 de `sdd-validate.md`.** (8 min) RF-20, RF-21, RNF-4
  - Hacer: reescribir solo el paso 5 de `template/.agents/commands/sdd-validate.md`. Si se pide evidencia formal, la sesión escribe `07_pruebas_qa.md` y el harness de `tests/` con lo que el reviewer devuelve; el reviewer, en una segunda invocación, ejecuta el harness en modo lectura y devuelve el resultado; la sesión lo anota en `07_pruebas_qa.md`. Esa invocación cuenta como una de las 2 vueltas. Si una vuelta ya se gastó en CAMBIOS NECESARIOS, queda una para esa invocación. El comando no exige que el reviewer carezca de `Write` para seguir: con el reviewer anterior la sesión escribe igual y solo se pierde la garantía de la fila 1 (RF-21 b). El reviewer nuevo, aunque el comando que tenga delante sea el anterior, devuelve el contenido y no escribe esos archivos (RF-21 a). El resto del comando no se reescribe.
  - Hecho cuando:
    - [x] El paso 5 es ese procedimiento, incluida la vuelta gastada y las dos combinaciones de RF-21
    - [x] El procedimiento no se copió al cuerpo del reviewer: allí solo está el enlace de F3.4
    - [x] Se releyó el archivo; las líneas ajenas se quedaron
    - [x] Lint L en `template/.agents/commands/sdd-validate.md` (`file -i` utf-8, sin BOM; sin `name`; rutas citadas existentes; sin marcadores `<<…>>`)
    - [x] Convenciones de archivo de `AGENTS.md` (encoding, formato) respetadas
    - [x] Evidencia de datos: No aplica
    - [x] Verificación manual UI: No aplica
- [x] **F4.2 «Qué lee cada rol» en `template/AGENTS.md`.** (6 min) RF-12, RF-16
  - Hacer: solo esa sección. Frase: el `implementer` puede recibir el archivo inyectado; planner, reviewer y coordinator no, porque declaran `omitClaudeMd`; cada rol lee solo las secciones que su instrucción nombra. Filas: el coordinator sigue comprobando `SDD-INIT-PENDIENTE` y suma «Git» y «Reglas duras»; planner y reviewer suman esas dos a las que ya tienen; la fila del implementer no cambia. No se copia la redacción de Git ni de las reglas duras. No se toca «Formato y numeración». Se conservan `<<NOMBRE_PROYECTO>>` e `<<IDIOMA>>`.
  - Hecho cuando:
    - [x] La frase y las tres filas quedaron así; la fila del implementer es la de antes
    - [x] El resto del archivo no se igualó al de la raíz; no se tocó «Formato y numeración»
    - [x] Se releyó el archivo; las líneas ajenas se quedaron
    - [x] Lint L en `template/AGENTS.md` (`file -i` utf-8, sin BOM; sin `name`; rutas citadas existentes; marcadores solo `<<NOMBRE_PROYECTO>>` e `<<IDIOMA>>`)
    - [x] Convenciones de archivo de `AGENTS.md` (encoding, formato) respetadas
    - [x] Evidencia de datos: No aplica
    - [x] Verificación manual UI: No aplica
- [x] **F4.3 La misma sección en el `AGENTS.md` de la raíz.** (6 min) RF-12, RF-16, RF-19
  - Hacer: el mismo texto genérico de F4.2 en «Qué lee cada rol». El modo mantenimiento, los comandos del clon y el resto del archivo no se tocan ni se igualan a `template/AGENTS.md`.
  - Hecho cuando:
    - [x] «Qué lee cada rol» coincide con la de `template/AGENTS.md`
    - [x] El resto del archivo de la raíz no se reescribió
    - [x] Se releyó el archivo; las líneas ajenas se quedaron
    - [x] Lint L en `AGENTS.md` (`file -i` utf-8, sin BOM; sin `name`; rutas citadas existentes; el punto 5 no aplica: está fuera de `template/`)
    - [x] Convenciones de archivo de `AGENTS.md` (encoding, formato) respetadas
    - [x] Evidencia de datos: No aplica
    - [x] Verificación manual UI: No aplica

## F5 — Seguridad básica completa (estimado: 20 min)
Requisito: F2, F3 y F4 cerradas. Alcance: los archivos que esas fases tocaron. `tools`, `mode` y los permisos cuentan como código ejecutable: el modo es el completo de `seguridad-basica` (sección 1 y checklist de la sección 2), no «solo secretos». `VERSION` y el manifiesto no entran: F6 y F7 todavía no corren.
- [x] **F5.1 Aplicar la skill y anotar el reporte.** (14 min) RF-15
  - Hacer: la sesión principal lee la skill y revisa esos archivos. Si no están en el stage, se usa el diff del árbol de trabajo de esas rutas y el archivo nuevo, con el mismo patrón de la sección 1. No se hace `git add`. No se pega un valor secreto en el chat ni en `05_avance.md`: se cita archivo y línea. El checklist de la sección 2 se marca en lo que aplica: `tools` no se amplió; el reviewer perdió `Write` y no recibió `Edit`; `permissionMode` no se usó; el `deny` es texto y no está en `.claude/`. Anotar el reporte en `05_avance.md` (`seguridad-basica: sin hallazgos en <N> archivos`, o una línea por hallazgo con severidad).
  - Hecho cuando:
    - [x] El reporte está en `05_avance.md` y dice que el modo fue el completo
    - [x] No se staged ni se pegó un secreto
    - [x] Lint L en `05_avance.md` (`file -i` utf-8, sin BOM; el punto 3 no aplica)
    - [x] Convenciones de archivo de `AGENTS.md` (encoding, formato) respetadas
    - [x] Evidencia de datos: No aplica
    - [x] Verificación manual UI: No aplica
- [x] **F5.2 Corregir una vez, o cerrar si no hubo hallazgo.** (6 min) RF-15
  - Hacer: si F5.1 no dejó hallazgo, este paso se cierra anotando que no aplica. Si lo dejó, el implementer corrige solo el archivo de la fase que lo introdujo (F2, F3 o F4) y la sesión repite la skill una vez sobre el mismo alcance. Si la segunda pasada sigue con hallazgo abierto, se para: no se abre F6 y no se crea una deuda. Un hallazgo que se difiere se propone, sin crear el archivo.
  - Hecho cuando:
    - [x] No queda un hallazgo abierto, o el paso está `[!]` y F6 no empezó
    - [x] La corrección, si la hubo, está en el archivo de su fase y la segunda pasada quedó anotada
    - [x] Lint L en cada archivo corregido (`file -i` utf-8, sin BOM, `name` según Lint L, rutas citadas existentes, marcadores de `template/` solo los dos permitidos). Si no hubo corrección: No aplica
    - [x] Convenciones de archivo de `AGENTS.md` (encoding, formato) respetadas, o No aplica si no se editó
    - [x] Evidencia de datos: No aplica
    - [x] Verificación manual UI: No aplica

## F6 — Ítem en el changelog (estimado: 10 min)
Requisito: F5 cerrada sin hallazgo abierto. La hace el implementer.
- [x] **F6.1 Un ítem en `## 0.5.0 — 2026-10-24`.** (10 min) RF-16, RF-19, RF-20, RF-21
  - Hacer: releer el bloque inmediatamente antes de escribir. Añadir un solo ítem `-`. No quitar ítems de otra spec. No cambiar el encabezado, la versión ni la fecha. `VERSION` no se toca, también si no termina en LF. El ítem dice: modelo explícito por rol; `omitClaudeMd` en planner, reviewer y coordinator; se quitan `mode` y `permissions`; el reviewer ya no tiene `Write` y la evidencia formal la escribe la sesión; `restricciones-de-rol.md` es NUEVO (grupo A) y se incorpora junto con los agentes; «Qué lee cada rol» llega por fusión manual. En el mismo ítem, los cuatro puntos de quien personalizó un agente: (1) quitar `mode`/`permissions` (se ignoran); (2) el reviewer ya no tiene `Write`; (3) sin `model` el agente hereda el de la sesión; (4) sin `omitClaudeMd` el agente sigue cargando `AGENTS.md` completo. Y las dos combinaciones de RF-21, pidiendo actualizar `reviewer.md` y `sdd-validate.md` a la vez.
  - Hecho cuando:
    - [x] Hay un ítem nuevo en ese bloque, con esos puntos y las dos combinaciones, y el encabezado sigue igual
    - [x] Ningún ítem previo se quitó; `VERSION` no cambió
    - [x] El bloque se releyó justo antes de escribir
    - [x] Lint L en `CHANGELOG.md` (`file -i` utf-8, sin BOM; sin `name`; rutas citadas existentes; el punto 5 no aplica: está fuera de `template/`)
    - [x] Convenciones de archivo de `AGENTS.md` (encoding, formato) respetadas
    - [x] Evidencia de datos: No aplica
    - [x] Verificación manual UI: No aplica

## F7 — Congelar (estimado: 35 min)
Requisito: F6 cerrada. Lo ejecuta la sesión principal. El reviewer no corre E1–E20. Si `template/` cambia después de F7.1, no se cierra F7: se repite desde F7.1.
- [x] **F7.1 Regenerar `manifiestos/0.5.0.md`.** (8 min) RF-18, RF-19
  - Hacer: confirmar que los cambios de `template/` y de los docs de la raíz ya están. Regenerar el manifiesto entero sobre el árbol de trabajo, con el algoritmo vigente de `ACTUALIZAR.md` (no se edita ese archivo ni una huella a mano): quitar el BOM `EF BB BF`, CRLF a LF, quitar espacios y tabuladores al final de cada línea, quitar líneas vacías finales y terminar con un solo LF, SHA-256 en hexadecimal minúscula. Rutas relativas a la raíz del proyecto, sin el prefijo `template/`. Filas de datos = archivos de `template/`. Si no cuadran, se regenera otra vez antes de dar el paso por cerrado.
  - Hecho cuando:
    - [x] El número de filas de datos es igual a `find template -type f | wc -l` medido en este paso
    - [x] Cada huella tiene 64 hex minúsculas; ninguna ruta empieza por `template/`
    - [x] `ACTUALIZAR.md` no se modificó
    - [x] Lint L en `manifiestos/0.5.0.md` (`file -i` utf-8, sin BOM; sin `name`; el punto 4 no aplica a manifiestos)
    - [x] Convenciones de archivo de `AGENTS.md` (encoding, formato) respetadas
    - [x] Evidencia de datos: No aplica
    - [x] Verificación manual UI: No aplica
- [x] **F7.2 E20 pasa.** (5 min) RF-18
  - Hacer: recalcular la huella de cada archivo de `template/` con el algoritmo de `ACTUALIZAR.md` y compararla con `manifiestos/0.5.0.md`. La definición de E20 en `validar-estructura.md` no se toca. La sesión principal ejecuta el chequeo; no el reviewer.
  - Hecho cuando:
    - [x] No hay diferencia, ni archivo sin fila, ni fila sin archivo
    - [x] `validar-estructura.md` no se modificó
    - [x] Lint: No aplica (no se edita un archivo)
    - [x] Convenciones de archivo de `AGENTS.md`: No aplica
    - [x] Evidencia de datos: No aplica
    - [x] Verificación manual UI: No aplica
- [x] **F7.3 Contraste contra `manifiestos/0.4.0.md`.** (3 min) RF-18
  - Hacer: contrastar 0.4.0 contra el 0.5.0 regenerado. Anotar en `05_avance.md` lo que salga (altas, bajas, huellas distintas). No se exigen las cifras de un contraste anterior.
  - Hecho cuando:
    - [x] El contraste quedó anotado con el conteo de esta corrida
    - [x] Lint L en `05_avance.md` (`file -i` utf-8, sin BOM; el punto 3 no aplica)
    - [x] Convenciones de archivo de `AGENTS.md` (encoding, formato) respetadas
    - [x] Evidencia de datos: No aplica
    - [x] Verificación manual UI: No aplica
- [x] **F7.4 Chequeos de Comandos en cada archivo tocado.** (5 min) RF-18, RNF-3
  - Hacer: Lint L en cada archivo que este feature modificó o creó: la pieza, `SKILL.md`, los 4 agentes, `sdd-validate.md`, los dos `AGENTS.md`, `CHANGELOG.md`, `05_avance.md` y `manifiestos/0.5.0.md` (en el manifiesto, el punto 4 no aplica). Sin scripts nuevos versionados.
  - Hecho cuando:
    - [x] Lint L pasa en esa lista, con las excepciones de `name` y de manifiestos
    - [x] No hay un script de instalación nuevo
    - [x] Convenciones de archivo de `AGENTS.md` (encoding, formato) respetadas
    - [x] Evidencia de datos: No aplica
    - [x] Verificación manual UI: No aplica
- [x] **F7.5 Ensayo de instalación.** (4 min) RF-18, RNF-3
  - Hacer: `tmp=$(mktemp -d); cp -R --update=none template/. "$tmp"/`. Repetir la copia sobre el mismo destino y comprobar que no pisa. Nunca en un proyecto real ni en el clon. El temporal queda fuera del repo.
  - Hecho cuando:
    - [x] El temporal tiene los mismos archivos que `find template -type f` de F7.1
    - [x] La segunda copia con `--update=none` no cambia las huellas del temporal
    - [x] El directorio temporal queda fuera del repo
    - [x] Lint: No aplica
    - [x] Convenciones de archivo de `AGENTS.md`: No aplica
    - [x] Evidencia de datos: No aplica
    - [x] Verificación manual UI: No aplica
- [x] **F7.6 Medir RF-13 y RF-14 sobre el contenido final.** (8 min) RF-13, RF-14, RF-18, RNF-1, RNF-2, RNF-7
  - Hacer: medir sobre `template/`, no sobre la copia local. Arranque por rol = bytes del agente + heredado, y heredado = 0 en quien tenga `omitClaudeMd: true`. El neto del implementer se anota y el campo sigue ausente. En planner, reviewer y coordinator el campo no se revierte aunque el neto de otro proyecto sea cero; en este `template/` se anota el neto real. Las `description` siguen sumando lo que midió F0. Copiar en `05_avance.md` la lista de RF-14, cada verificación con la pieza donde sigue:

    | Verificación | Pieza |
    |---|---|
    | Criterios de aceptación y «Hecho cuando» | `sdd-validate.md` y el reviewer, que los lee |
    | Chequeos de `AGENTS.md` › Comandos | `AGENTS.md` y el comando de validación |
    | Revisión de seguridad con alcance de código | `seguridad-basica` (completa cuando hay `tools` o permisos) |
    | Deuda, ADR e índices al cerrar | `sdd-cierre.md` (no se edita) |
    | El implementer no registra en Git ni escribe `05_avance.md` | Cuerpo del implementer y fila 4 de la tabla |
    | E1 a E20 los ejecuta la sesión principal | `validar-estructura.md` (no se edita) |
    | Puertas de aprobación del coordinator | Cuerpo del coordinator y `sdd-implement.md` (no se edita) |
    | Lectura mínima, lecturas permitidas y aviso de respaldo de la `003` | `sdd-implement.md` (no se edita) |

    Si `template/` cambió desde F7.1, no se cierra este paso: se repite F7 desde F7.1.
  - Hecho cuando:
    - [x] `05_avance.md` tiene la línea base de F0, anterior al primer cambio de `template/`, y esta medición del contenido final
    - [x] La tabla de RF-14 está completa y ninguna de esas piezas perdió la verificación
    - [x] El implementer sigue sin `omitClaudeMd`; las `description` no crecieron respecto de F0
    - [x] Lint L en `05_avance.md` (`file -i` utf-8, sin BOM; el punto 3 no aplica)
    - [x] Convenciones de archivo de `AGENTS.md` (encoding, formato) respetadas
    - [x] Evidencia de datos: No aplica
    - [x] Verificación manual UI: No aplica
- [x] **F7.7 Proponer el refresco de la copia local.** (2 min) RF-19
  - Hacer: si `.agents/agents/` sigue distinto de `template/.agents/agents/`, proponer el refresco. No ejecutarlo. No bloquea F7. No se edita la copia local ni `.claude/`.
  - Hecho cuando:
    - [x] El resultado de la comparación quedó anotado
    - [x] El refresco, si hace falta, está propuesto y no se ejecutó
    - [x] Lint: No aplica
    - [x] Convenciones de archivo de `AGENTS.md`: No aplica
    - [x] Evidencia de datos: No aplica
    - [x] Verificación manual UI: No aplica

---

## Cobertura
Los 21 RF tienen al menos un paso.

| RF | Pasos |
|---|---|
| RF-1 | F2.1 |
| RF-2 | F2.1 |
| RF-3 | F3.1, F3.2, F3.3, F3.4 |
| RF-4 | F0.3, F1.1, F1.2, F1.3, F1.4, F3.3 |
| RF-5 | F1.2, F1.3, F1.4, F2.1 |
| RF-6 | F2.1, F3.1, F3.2, F3.3, F3.4 |
| RF-7 | F2.1, F2.2, F3.4 |
| RF-8 | F3.4 |
| RF-9 | F2.1 |
| RF-10 | F2.1 |
| RF-11 | F1.4, F3.1, F3.2, F3.3, F3.4 |
| RF-12 | F3.1, F3.2, F3.4, F4.2, F4.3 |
| RF-13 | F0.2, F7.6 |
| RF-14 | F7.6 |
| RF-15 | F5.1, F5.2 |
| RF-16 | F4.2, F4.3, F6.1 |
| RF-17 | F2.1 |
| RF-18 | F0.1, F7.1, F7.2, F7.3, F7.4, F7.5, F7.6 |
| RF-19 | F4.3, F6.1, F7.1, F7.7 |
| RF-20 | F3.4, F4.1, F6.1 |
| RF-21 | F3.4, F4.1, F6.1 |

RNF: RNF-1 en F1.3 y F7.6; RNF-2 en F1.4 y F7.6; RNF-3 en F7.4 y F7.5; RNF-4 en F2.2 y F4.1; RNF-6 en F2.1; RNF-7 en F0.2 y F7.6. RNF-5 (un agente viejo con `mode` y `permissions` sigue siendo legible, y el changelog dice las combinaciones) queda cubierto por F6.1 y por no exigir en F4.1 que el reviewer carezca de `Write`.

## Resumen de minutos
| Fase | Pasos | Minutos |
|---|---|---|
| F0 | F0.1, F0.2, F0.3 | 15 |
| F1 | F1.1, F1.2, F1.3, F1.4 | 50 |
| F2 | F2.1, F2.2 | 30 |
| F3 | F3.1, F3.2, F3.3, F3.4 | 25 |
| F4 | F4.1, F4.2, F4.3 | 20 |
| F5 | F5.1, F5.2 | 20 |
| F6 | F6.1 | 10 |
| F7 | F7.1, F7.2, F7.3, F7.4, F7.5, F7.6, F7.7 | 35 |
| **Total** | **26** | **205** |
