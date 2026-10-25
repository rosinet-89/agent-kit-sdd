# 02 — Análisis de impacto: Modelo por rol y permisos del frontmatter de los agentes

## 1. Resumen del requerimiento
La spec está aprobada (2026-10-25 14:58, 21 RF, 7 RNF, sin dudas abiertas). Pide que cada agente
declare `model`, que `planner`, `reviewer` y `coordinator` no carguen `CLAUDE.md`, y que cada
restricción de rol quede garantizada por `tools` o dicha como no garantizada. `mode` y
`permissions` se quitan. El `reviewer` pierde `Write`. La versión sigue siendo 0.5.0 sin publicar.

Medido el 2026-10-25 sobre el árbol de trabajo (sección 3): ninguno de los 4 agentes declara
`model` ni `omitClaudeMd`; los 4 tienen `mode` y `permissions`; el `reviewer` tiene `Write`.
`template/` tiene 41 archivos y el manifiesto de 0.5.0 tiene 41 filas. Esas cifras se vuelven a
tomar en F0, antes de editar, y otra vez al congelar: si `template/` cambió, manda la medición
nueva.

### 1.1 Antecedentes (índice de planes)
Índice leído (`.docs/002_specs/README.md`, 2026-10-25). Hay coincidencia. Manda la carpeta cuando
discrepa con el índice.

| Carpeta | Estatus en el índice | Uso |
|---|---|---|
| `001_ActualizarKit` | Implementado (2026-10-25) | Procedimiento de actualización, grupos A/B, manifiestos y E20. No se cambian ADR 007, ADR 008 ni el texto de E20. Los tres pasos de referencia salen de su `04_tasks.md` y de la medición final de la `003`. |
| `002_Kit050` | Implementado, retroactivo | Fuera de alcance. |
| `003_OptimizarArnes` | Implementado (cierre 2026-10-25 14:23) | Ya reescribió los cuerpos de los 4 agentes. Esta spec parte de ese frontmatter. No se toca su carpeta. |
| `004_ModeloYPermisosAgentes` | Análisis, spec aprobada | Esta carpeta. Se redactan `02` y `03`. |
| `005_ConvivenciaNumeracion` | Análisis (spec en curso, 2026-10-25 15:04) | No edita `template/` mientras documenta. La implementación va en serie con esta. Archivos compartidos: `CHANGELOG.md`, `template/AGENTS.md` (otra sección) y `manifiestos/0.5.0.md`. |

La fila 004 del índice sigue diciendo «falta plan; espera el cierre de la 003». La carpeta ya
registró el cierre de la `003` (decisión 17 de su `05_avance.md`, 2026-10-25 15:05) y el siguiente
paso es el plan. Se corrige la fila en el cierre de esta spec. No se corrige aquí.

### 1.2 Deuda técnica relacionada
Índice `.docs/003_deuda/README.md`:

| Deuda | Estado | Qué hace este feature |
|---|---|---|
| `003_ModeloYPermisosDeAgentes` | En atención, por esta spec | La resuelve en el fondo: `model` explícito, `mode` y `permissions` fuera, `reviewer` sin `Write`, restricciones con mecanismo real o marcadas no garantizadas. `/sdd-cierre` la pasa a `Resuelta`. Ninguna fase de este plan edita el archivo de la deuda. |
| `004_ConvivenciaNumeracion3y4Digitos` | En atención, por la `005` | No es de este módulo. Se deja. |
| 001 y 002 | Resueltas | No se reabren. |

La deuda 003, en «Cómo se atendería», nombra hooks y que `/sdd-init` sugiera reglas `allow`. La
spec cerró las dos: sin hooks (ADR 004 no se enmienda) y `/sdd-init` fuera de alcance. Al cierre
se propone, sin crearla, una deuda nueva: que `/sdd-init` sugiera los fragmentos opcionales
`allow` y `deny` de la pieza de restricciones, sin aplicarlos.

## 2. Módulos, menú, tablas y archivos involucrados
El producto es `template/`. Los docs de versión viven en la raíz. La copia local `.agents/` de
este clon no se versiona (ADR 001) y no se edita en las fases.

### 2.1 Menú / opciones / rutas de acceso
No hay menú ni pantalla. El acceso es el frontmatter que lee la herramienta al lanzar el agente,
como subagente o con `claude --agent`. En este clon, `.claude/agents` y `.cursor/agents` son
enlaces a `../.agents/agents` (directorio real, distinto de `template/.agents/agents`). Editar
`template/` no cambia los agentes que esta sesión está usando.

### 2.2 Tablas (existentes y nuevas)
No aplica. No hay base de datos.

### 2.3 Archivos de código (ruta, capa)
Capa según la constitución: producto (`template/`), manual de la raíz, mantenimiento versionado.

| Ruta | Capa | Situación medida el 2026-10-25 | Qué le pasa |
|---|---|---|---|
| `template/.agents/agents/coordinator.md` | Producto | 4 859 bytes. `tools` con `Agent(planner, implementer, reviewer)`, `Write`, `Edit`, `Bash`. `mode: primary`. Bloque `permissions` de edición, shell, red y subagente. Sin `model` ni `omitClaudeMd`. | Frontmatter nuevo. El cuerpo solo gana el enlace a la pieza de restricciones y el nombre de las secciones «Git» y «Reglas duras». |
| `template/.agents/agents/planner.md` | Producto | 2 428 bytes. `tools: Read, Glob, Grep, Write, Edit, Bash`. `mode: subagent`. `permissions` de edición a `.docs/002_specs/**`, shell en `ask`, red y subagente denegados. | Igual que el coordinator en campos. Sigue con `Write` y `Edit`. |
| `template/.agents/agents/implementer.md` | Producto | 1 560 bytes. `tools: Read, Glob, Grep, Write, Edit, Bash`. `permissions` con shell abierto. | `model: inherit`. Sin `omitClaudeMd` (sección 3, historial de F1.5). Se quitan `mode` y `permissions`. |
| `template/.agents/agents/reviewer.md` | Producto | 2 484 bytes. `tools: Read, Glob, Grep, Bash, Write`. No tiene `Edit`. El cuerpo dice que escribe `07_pruebas_qa.md` y `tests/`. | Pierde `Write`. El cuerpo enlaza el comando y deja de decir que escribe esos archivos. |
| `template/.agents/skills/sdd/restricciones-de-rol.md` | Producto | No existe. | Se crea. Grupo A: en una actualización es NUEVO. |
| `template/.agents/skills/sdd/SKILL.md` | Producto | 8 251 bytes. Enlaza `avance-y-tiempos.md`. | Una frase que enlaza la pieza nueva. No copia la tabla. |
| `template/.agents/commands/sdd-validate.md` | Producto | 2 305 bytes. El paso 5 dice que el reviewer genera `07_pruebas_qa.md`. | El paso 5 pasa a ser el de RF-20. |
| `template/AGENTS.md` | Producto, grupo B | 8 403 bytes. Sección «Qué lee cada rol» idéntica a la de la raíz en sus cuatro filas. El comentario de la línea 3 contiene el marcador `SDD-INIT-PENDIENTE`. | Solo esa sección: la frase de inyección y las filas de `planner`, `reviewer` y `coordinator`. |
| `AGENTS.md` (raíz) | Mantenimiento versionado, grupo B | 9 272 bytes. La misma sección. El literal `SDD-INIT-PENDIENTE` aparece en la instrucción y en la tabla, no como marcador de init. | El mismo ajuste de sección. El resto (modo mantenimiento, comandos del clon) no se toca. |
| `CHANGELOG.md` | Manual | Bloque `## 0.5.0 — 2026-10-24` ya existe. `VERSION` es `0.5.0` (5 bytes, sin LF final). | Líneas nuevas dentro de ese bloque. `VERSION` no se toca. |
| `manifiestos/0.5.0.md` | Mantenimiento | 41 filas de datos. Huellas de los 4 agentes y de `sdd-validate.md` ya no valdrán tras el cambio. | Se regenera entero al congelar, con el algoritmo de `ACTUALIZAR.md`. No se edita a mano. |
| `.docs/002_specs/004_ModeloYPermisosAgentes/05_avance.md` | Mantenimiento | Modificado en el árbol (`git diff --stat`: 7 líneas) por la sesión que llevó la spec. | Lo escribe la sesión (línea base y evidencia). Este análisis no lo edita. |

No se editan: carpeta `003_OptimizarArnes`, carpeta `005_ConvivenciaNumeracion`, `template/` fuera
de la tabla, `sdd-init.md`, `validar-estructura.md`, ADR 004, ADR 007, ADR 008, ADR 009.

### 2.4 Código legado o sin uso (no tocar, sí documentar)
`mode` y `permissions` están en los 4 frontmatters y Claude Code los ignora (decisiones 2 y 7,
documentación de subagentes leída el 2026-10-25). Se eliminan. No se conservan «por si otra
herramienta». El registro de la pieza nueva dice que son formato de otra herramienta y que en
Cursor, Gemini y Copilot su efecto queda no verificado.

La copia local `.agents/agents/*.md` está hoy igual que `template/` (`diff -q` sin salida,
2026-10-25). Es una copia, no un enlace. Refrescarla no es parte de las fases; si al cerrar sigue
distinta, queda pendiente de refrescar (RF-19).

## 3. Datos reales (SELECT de solo lectura, con fecha y BD)
No aplica una base de datos. `AGENTS.md` › Comandos dice «Consulta de datos de solo lectura: No
aplica». Lo de abajo es lectura del árbol de trabajo el 2026-10-25, sin `fetch`. Origen al lado
de cada cifra. Bytes ÷ 4 es referencia (cociente entero).

### 3.1 Frontmatter y arranque en frío
`wc -c` y lectura del frontmatter. Los 4 archivos: UTF-8 (`file -i`), 0 CR, 0 BOM, terminan en LF.
Claves presentes: `name`, `description`, `tools`, `mode`, `permissions`. Ausentes: `model`,
`omitClaudeMd`, `disallowedTools`, `permissionMode`, `hooks`.

Hoy no hay `omitClaudeMd`, así que cada arranque suma el agente, `template/CLAUDE.md` (11 bytes,
`@AGENTS.md`) y `template/AGENTS.md` (8 403 bytes). Heredado = 8 414 bytes (~2 103 tokens).

| Rol | Agente (bytes) | Arranque en frío hoy | Tokens del agente + heredado |
|---|---:|---:|---:|
| coordinator | 4 859 | 13 273 | 3 318 |
| planner | 2 428 | 10 842 | 2 710 |
| implementer | 1 560 | 9 974 | 2 493 |
| reviewer | 2 484 | 10 898 | 2 724 |

Esas cifras coinciden con el cierre de la `003` en lo que esta spec puede mover:
`implementer.md` 1 560 y `template/.agents/commands/sdd-implement.md` 1 590 (`wc -c`, mismo día).
La `003` dejó la lectura posterior así (su `05_avance.md`, F8, 2026-10-25 14:06): mediana `F5.1`
18 331, mayor `F1.5` 24 398, respaldo de `F6.2` 26 828. Si un `wc -c` posterior no coincide, manda
el posterior.

`description` (bytes del valor): coordinator 267, planner 191, implementer 201, reviewer 228.
Suma 887 bytes (~221 tokens). El presupuesto conjunto de la herramienta es 15 000 tokens. No se
alargan las `description`.

### 3.2 Historial del paso mayor
`git log --oneline -- ACTUALIZAR.md` (2026-10-25, sin fetch): un solo commit, `63f586d`
(`feat(actualizar): agrega ACTUALIZAR.md y manifiestos de huellas`). No hay revisión en la que
F1.4 de `001_ActualizarKit` ya esté y F1.5 todavía no. El padre de ese commit no tiene el
archivo, así que tampoco sirve como «estado previo» del paso: el «Hecho cuando» de F1.5 supone
el documento de F1.1 a F1.4.

`git ls-tree --name-only origin/main .docs/004_adr/` (2026-10-25, sin fetch) no listó archivos.
El índice local llega a ADR 009. El siguiente número local es 010; se vuelve a mirar local y
remota ya descargada antes de crear un ADR.

En el shell de esta medición, `CLAUDE_CODE_SUBAGENT_MODEL` y
`CLAUDE_CODE_SUBAGENT_MODEL_FORCE` están unset. F0 lo comprueba en la sesión que compare, porque
este shell no es esa sesión.

### 3.3 Árbol y copias
| Dato | Valor | Origen |
|---|---|---|
| Archivos en `template/` | 41 | `find template -type f \| wc -l` |
| Filas de datos en `manifiestos/0.5.0.md` | 41 | conteo de filas `| … |` que no son encabezado |
| `git status --short` de los archivos que este plan edita | limpio, salvo `05_avance.md` de esta carpeta | 2026-10-25 |
| Copia local de los 4 agentes vs `template/` | iguales | `diff -q` |
| `.claude/agents`, `.cursor/agents` | enlace a `../.agents/agents` | `readlink` |
| `.gemini/agents`, `.github/agents` | no existen | no se crean en este feature |

`template/AGENTS.md` línea 3 es el comentario `<!-- SDD-INIT-PENDIENTE: … -->`. El `AGENTS.md` de
la raíz contiene el literal dentro de la frase de la regla y de la tabla del coordinator. Usar la
raíz como proyecto «sin inicializar» daría un falso positivo. El caso 5 de RF-4 usa una copia
desechable cuyo `AGENTS.md` es el de `template/`.

## 4. Flujo actual
1. La sesión lanza un subagente. Sin `model` en el frontmatter, el modelo es el de la sesión,
   salvo parámetro de la llamada o `CLAUDE_CODE_SUBAGENT_MODEL`. Con `_FORCE=1` se ignora el
   frontmatter.
2. El subagente carga la jerarquía de `CLAUDE.md`. En `template/` eso importa `AGENTS.md` entero
   (8 403 bytes), aunque la `003` fijó que cada rol lea solo las secciones que nombra.
3. `tools` es la lista permitida y sí se aplica. `planner`, `implementer` y `reviewer` no listan
   `Agent`, así que no lanzan subagentes. El `coordinator` lista `Agent(planner, implementer, reviewer)`:
   la lista restringe cuando corre como hilo principal con `claude --agent`; como subagente se ignora.
4. `mode` y `permissions` no se aplican. Lo que aparentan (el planner solo escribe en su carpeta,
   el coordinator solo en `05_avance.md` y solo `date`, el implementer sin un límite de Git, el
   reviewer pidiendo confirmación en el lint) queda en el texto del cuerpo.
5. El `reviewer` tiene `Write`. Si piden evidencia formal, `sdd-validate.md` paso 5 le manda
   generar `07_pruebas_qa.md`, y su cuerpo también le manda escribir `tests/`.
6. Al publicar, los agentes y el comando son grupo A (se reemplazan si son KIT-VIEJO). `AGENTS.md`
   es grupo B (fusión manual; un cambio de redacción de una sección que ya existe no se detecta
   como faltante). Un archivo nuevo bajo `.agents/skills/sdd/` es NUEVO.

## 5. Reglas de negocio: conocidas vs código
La regla está en el cuerpo o en `permissions`. La herramienta, hoy, solo aplica `tools`.

| Restricción que el rol dice cumplir | Qué hay en el archivo | ¿La herramienta la garantiza hoy? |
|---|---|---|
| `reviewer` no modifica código ni documentos de diseño | `permissions` deniega edición salvo `07_pruebas_qa.md` y `tests/`. `tools` incluye `Write`. El cuerpo dice que él escribe esos dos. | No. `Write` se lo permite. Bash (`sed -i`, redirección, `tee`) tampoco se puede cerrar con `tools`. |
| `planner` escribe solo los cinco documentos de su carpeta | `permissions` allow a `.docs/002_specs/**`. `tools` incluye `Write` y `Edit` sin ruta. | No. |
| `coordinator` escribe solo `05_avance.md` y el shell solo para la hora | `permissions` lo dice. `tools` incluye `Write`, `Edit` y `Bash` sin límite. | No. |
| `implementer` no hace `git commit`, `push`, `fetch`, `pull` ni `sync`, y no ejecuta SQL | El cuerpo dice «No registras en Git» y «No ejecutas SQL». `permissions` deja el shell en `allow`. | No. `disallowedTools` con `Bash(git push *)` quitaría Bash entero. |
| `planner`, `reviewer` y `coordinator` tampoco registran en Git | No está en `tools`. El cuerpo del coordinator dice «No haces git commit». | No. |
| Solo el `coordinator` lanza subagentes, y solo a los tres roles | Los otros tres no listan `Agent`. El coordinator lista los tres. | Garantizada para los tres que no listan `Agent`. Para el coordinator, solo cuando es hilo principal. |
| Ningún agente usa la red | `permissions` deniega `webfetch` y, en el coordinator, `websearch`. `tools` no lista esas herramientas. | Garantizada por `tools`, no por `permissions`. |
| `planner` consulta datos solo en lectura | El cuerpo lo dice. Tiene `Bash`. | No. |
| Lint del `reviewer` sin confirmación por comando | `permissions` mezcla `ask` y `allow` para `git diff` y `git status`. | No se aplica. RF-10 lo saca de las restricciones de rol y lo deja como fragmento `allow` opcional. |

Reglas que siguen en su fuente y este feature no reescribe: Git y reglas duras en `AGENTS.md`;
el procedimiento de cada fase en su comando (ADR 009); el contrato de tiempos en
`avance-y-tiempos.md`; E20 en `validar-estructura.md`.

Con `omitClaudeMd: true`, `planner`, `reviewer` y `coordinator` dejan de recibir solas estas
reglas de `AGENTS.md`: de «Git», no commitear sin confirmación, no `push`/`pull`/`fetch`/`sync`,
un commit por módulo con `git add` explícito y el formato del mensaje; de «Reglas duras», no
secretos, consultar lo que salga del plan, proponer y no escribir `MEMORY.md`, y no reinstalar
el kit. El `coordinator` sigue teniendo que detectar el marcador `SDD-INIT-PENDIENTE` porque su
instrucción se lo pide, no porque el archivo llegue inyectado. El `implementer` hoy sí puede
recibir el archivo, porque no llevará `omitClaudeMd`.

## 6. Riesgos y casos borde
| Riesgo | Qué se hace |
|---|---|
| No existe árbol «previo a F1.5» (sección 3.2). Rehacer el paso sobre el árbol de hoy cumpliría el «Hecho cuando» sin trabajo del modelo. | El `implementer` no pasa a `sonnet`. Declara `model: inherit`. No se fabrica un fixture. Si más adelante aparece una revisión separable, la comparación se corre antes de aceptar el cambio; hasta entonces no se acepta. |
| La condición (b) de RF-11 (el paso no intenta registrar en Git ni se sale del plan) pide los mismos tres pasos sobre ese árbol. | El `implementer` no adopta `omitClaudeMd`. El neto en bytes se anota igual. |
| Un alias `sonnet` cae al modelo heredado, o `CLAUDE_CODE_SUBAGENT_MODEL_FORCE=1` ignora el frontmatter. | Esa corrida no cuenta. Se anota y se repite, o el rol conserva `inherit`. |
| `omitClaudeMd` quita Git y reglas duras a tres roles. | La instrucción nombra esas secciones y la tabla «Qué lee cada rol» también. No se copia la redacción. |
| El `reviewer` conserva `Bash` y puede escribir por shell. | Fila 1 de la tabla: no garantizada. |
| `Agent(lista)` no restringe al `coordinator` cuando es subagente. | Fila 6: no garantizada en ese caso. La lista se queda para cuando es hilo principal. |
| El fragmento `deny` vale para toda la sesión. Quien lo copie puede bloquearse su propio Git. | Texto de advertencia en la pieza. No se versiona `.claude/`. |
| Cursor, Gemini y Copilot no están verificados. Quitar `mode` y `permissions` puede quitarles una función que aquí no se conoce. | El registro lo dice. El cuerpo sigue siendo legible sin esos campos. |
| Un `CLAUDE.md` de proyecto que no importa `AGENTS.md`: el ahorro de `omitClaudeMd` es cero y además quita el `CLAUDE.md` propio. | Se registra así (RNF-2). En los tres roles el campo no se revierte por eso. |
| Actualizar solo `reviewer.md` o solo `sdd-validate.md`. | RF-21. El changelog dice las dos combinaciones. |
| La `005` puede estar editando `CHANGELOG.md`, `template/AGENTS.md` o el manifiesto. | Releer el archivo antes de tocarlo. No revertir sus líneas. Quien cierre segundo regenera el manifiesto y repite E20. Esta spec no toca «Formato y numeración». |
| Un conteo de esta sección envejece si `template/` cambia antes de congelar. | F0 y F7 vuelven a medir. Manda la medición. |
| La copia local queda vieja y los enlaces `.claude/` y `.cursor/` siguen apuntando a ella. | No bloquea el cierre. Queda pendiente de refrescar. |
| El caso 5 de RF-4, si lee el `AGENTS.md` de la raíz, ve el literal del marcador dentro de la propia regla. | El caso usa el `AGENTS.md` de `template/` en una copia desechable. |
| `VERSION` no termina en LF. | No se «arregla» en este feature. |
| Una fase llamada Cierre no ejecuta `/sdd-cierre`. | Ninguna fase del plan lleva ese nombre. |

## 7. Preguntas abiertas
Ninguna. La spec no tiene dudas abiertas y las decisiones 1 a 17 del avance no se reabren. El
resultado `inherit` del `implementer` sale de RF-4 aplicado al dato de la sección 3.2, no de una
pregunta nueva.
