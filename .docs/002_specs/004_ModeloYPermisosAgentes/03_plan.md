# 03 — Plan: Modelo por rol y permisos del frontmatter de los agentes
Spec: 01_spec.md (aprobada 2026-10-25 14:58)

Plan de implementación. La spec sigue en `Estado: aprobada`. Lo que sigue edita `template/` y los docs de versión de la raíz.

## 1. Ubicación y capas
El kit es solo Markdown (constitución, principios 1, 3 y 6; ADR 004). No hay capas de aplicación ni pantallas.

| Capa | Dónde vive este cambio |
|---|---|
| Producto | `template/` (agentes, skill `sdd`, comando `sdd-validate`, `template/AGENTS.md`) |
| Manual de la raíz | `CHANGELOG.md` (un ítem dentro del bloque 0.5.0 ya fechado) |
| Mantenimiento versionado | `AGENTS.md` de la raíz (la misma sección que el de `template/`) y `manifiestos/0.5.0.md` (regenerado, no editado a mano) |

Patrón: la tabla de restricciones tiene una sola fuente, `template/.agents/skills/sdd/restricciones-de-rol.md`. El procedimiento de evidencia formal tiene una sola fuente, `template/.agents/commands/sdd-validate.md` (ADR 009). Los agentes nombran y enlazan. `AGENTS.md` es grupo B (ADR 007): se edita la sección «Qué lee cada rol» en `template/AGENTS.md` y en el de la raíz, con el mismo texto genérico. El resto de cada archivo no se iguala. Un archivo nuevo bajo `.agents/skills/sdd/` es grupo A y, al actualizar, se ofrece como NUEVO.

La copia local `.agents/` de este clon no se versiona (ADR 001) y ninguna fase la edita. Editar `template/` no cambia los agentes que usa esta sesión: `.claude/agents` y `.cursor/agents` apuntan a esa copia.

Partir del frontmatter medido el 2026-10-25 (`02` §2.3 y §3.1). Los cuatro tienen `name`, `description`, `tools`, `mode` y `permissions`. No tienen `model`, `omitClaudeMd`, `disallowedTools`, `permissionMode` ni `hooks`.

| Rol | `tools` de partida | Qué queda |
|---|---|---|
| coordinator | `Agent(planner, implementer, reviewer), Read, Glob, Grep, Write, Edit, Bash` | Esos mismos `tools`. `model` según F1. `omitClaudeMd: true` |
| planner | `Read, Glob, Grep, Write, Edit, Bash` | Esos mismos `tools`. `model: inherit`. `omitClaudeMd: true` |
| implementer | `Read, Glob, Grep, Write, Edit, Bash` | Esos mismos `tools`. `model: inherit`. Sin `omitClaudeMd` |
| reviewer | `Read, Glob, Grep, Bash, Write` | `Read, Glob, Grep, Bash`. Sin `Write` y sin `Edit`. `model: inherit`. `omitClaudeMd: true` |

En los cuatro se borran `mode` y `permissions`. No se añade `disallowedTools`, `permissionMode` ni `hooks`. Las `description` no se alargan (suma medida 887 bytes; presupuesto de la herramienta 15 000 tokens).

La deuda `003_ModeloYPermisosDeAgentes` sigue `En atención`. La pasa a `Resuelta` el `/sdd-cierre`, que no es una fase de este plan. Ninguna fase edita el archivo de la deuda ni la carpeta `003_OptimizarArnes`.

## 2. Archivos que se crean o modifican
Lista cerrada. Coincide con `02` §2.3. Quien edite relee el archivo en ese momento. Si trae líneas de otra spec (`005_ConvivenciaNumeracion` puede estar en `CHANGELOG.md`, `template/AGENTS.md` o el manifiesto), esas líneas se quedan.

| Archivo | Capa | Responsabilidad | RF |
|---|---|---|---|
| `template/.agents/skills/sdd/restricciones-de-rol.md` | Producto, grupo A, se crea | Registro de campos, tabla de restricciones, fragmentos opcionales `deny` y `allow` | RF-1, RF-2, RF-5, RF-6, RF-7, RF-9, RF-10, RF-17 |
| `template/.agents/skills/sdd/SKILL.md` | Producto | Una frase junto al enlace de `avance-y-tiempos.md`. No copia la tabla | RF-7, RNF-4 |
| `template/.agents/agents/coordinator.md` | Producto | Frontmatter de la tabla de §1. El cuerpo enlaza la pieza y nombra «Git» y «Reglas duras». La lista `Agent(...)` se queda. No reescribe el enrutado | RF-3, RF-4, RF-6, RF-11, RF-12 |
| `template/.agents/agents/planner.md` | Producto | Igual en campos. Sigue con `Write` y `Edit`. El cuerpo enlaza la pieza y nombra «Git» y «Reglas duras» | RF-3, RF-6, RF-11, RF-12 |
| `template/.agents/agents/implementer.md` | Producto | `model: inherit`, sin `omitClaudeMd`, sin `mode` ni `permissions`. El cuerpo solo gana el enlace a la pieza | RF-3, RF-4, RF-6, RF-11 |
| `template/.agents/agents/reviewer.md` | Producto | Pierde `Write`. El cuerpo deja de decir que escribe `07_pruebas_qa.md` y `tests/`, enlaza `sdd-validate` y nombra «Git» y «Reglas duras» | RF-6, RF-7, RF-8, RF-11, RF-12, RF-20, RF-21 |
| `template/.agents/commands/sdd-validate.md` | Producto | El paso 5 pasa a ser el de RF-20. Sigue siendo la fuente del procedimiento | RF-20, RF-21, RNF-4 |
| `template/AGENTS.md` | Producto, grupo B | Solo «Qué lee cada rol»: la frase de inyección y las filas de planner, reviewer y coordinator | RF-12, RF-16 |
| `AGENTS.md` (raíz) | Mantenimiento, grupo B | La misma sección. El modo mantenimiento y los comandos del clon no se tocan | RF-12, RF-16, RF-19 |
| `CHANGELOG.md` | Manual | Un ítem nuevo dentro de `## 0.5.0 — 2026-10-24`. `VERSION` no se toca | RF-16, RF-19, RF-20, RF-21 |
| `manifiestos/0.5.0.md` | Mantenimiento | Regenerado entero al congelar, con el algoritmo de `ACTUALIZAR.md` | RF-18, RF-19 |
| `.docs/002_specs/004_ModeloYPermisosAgentes/05_avance.md` | Esta carpeta | Línea base, las 5 peticiones y la evidencia de RF-4. Lo escribe la sesión principal | RF-4, RF-13 |

No se editan: carpeta `003_OptimizarArnes`, carpeta `005_ConvivenciaNumeracion`, `sdd-init.md`, `validar-estructura.md`, `ACTUALIZAR.md`, ADR 004, ADR 007, ADR 008, ADR 009, el archivo de la deuda 003, `VERSION`, la copia local `.agents/`, `.claude/` y `sql/`. No se crean `.gemini/agents` ni `.github/agents`.

## 3. Base de datos
No aplica. Sin `sql/`, sin DDL ni seed, sin rollback.

## 4. Seguridad
`tools`, `mode` y los permisos cuentan como código ejecutable (definición de la `003`). F5 aplica la skill `seguridad-basica` completa, búsqueda de secretos incluida. No es el modo «solo secretos».

- `tools` no se amplía. El reviewer pierde `Write` y no recibe `Edit`. Bash se queda: el lint y el harness lo usan. Escribir por `sed -i`, redirección o `tee` queda en la fila 1 como no garantizada.
- `permissionMode` no se usa.
- `disallowedTools` no se usa. Un patrón `Bash(git push *)` quita Bash entero (RF-1). La red ya está cerrada porque `tools` no lista `webfetch` ni búsqueda web.
- `Agent(planner, implementer, reviewer)` se conserva. Restringe cuando el coordinator corre como hilo principal con `claude --agent`. Como subagente la lista se ignora: la fila 6 lo dice, no garantizada en ese caso.
- El fragmento `permissions.deny` es texto dentro de la pieza. No se copia a `.claude/` ni a ningún settings versionado. Vale para toda la sesión, no por subagente, y cubre comandos compuestos. Quien lo copie puede bloquear su propio Git: la pieza lleva esa advertencia.
- El fragmento que se escribe es el verificado el 2026-10-25: `Bash(git commit *)`, `Bash(git push *)`, `Bash(git fetch *)`, `Bash(git pull *)` y `Bash(git sync *)`. No se inventa un patrón de ruta para el planner ni el coordinator. Un deny de `Write`/`Edit` de toda la sesión bloquearía también a la sesión, y no se ofrece.
- El fragmento `permissions.allow` (RF-10) es genérico: el responsable pone los comandos de su `AGENTS.md` › Comandos. Ningún agente lo aplica ni cambia su modo de permisos. No es una fila de la tabla.
- Sin hooks y sin scripts. ADR 004 no se enmienda.
- Sin secretos en lo versionado. Cada edición parte del contenido actual. Prohibido `git checkout` y `git restore` sobre trabajo ajeno.

## 5. Liberación / piloto
No aplica. Sigue dentro de la 0.5.0 local, sin publicar y sin piloto. Sin feature flag y sin `push`.

El orden de RF-18 está en F7. `/sdd-cierre` corre cuando el usuario lo pida, sobre el resultado ya medido, y no es una fase de este plan.

## 6. Decisiones
Las del avance 1 a 17 no se reabren. La (6) aplica la decisión 12 al dato ya medido: no es una pregunta nueva.

| Decisión | Alternativa descartada | Por qué |
|---|---|---|
| (1) Se implementa por el flujo SDD | Editar el frontmatter sin spec | Avance, decisión 1. Principio 2 |
| (2) Restricciones: `tools` donde alcance; el resto, instrucción marcada «no garantizada»; `deny` opcional y local. Sin hooks. **Proponer ADR 010** al cierre | Hooks por subagente; dejarlo solo en la instrucción; bloquear Git con `disallowedTools` | Avance, decisiones 4 y 11. Seguridad del kit, efecto en todo proyecto que actualice. No reemplaza al ADR 004: lo aplica |
| (3) `omitClaudeMd: true` en planner, reviewer y coordinator. El implementer no lo declara. **Proponer ADR 011** al cierre | Ningún rol; los cuatro; quitar el campo a los tres si en otro proyecto el ahorro es cero | Avance, decisión 6. En el implementer la condición (b) de RF-11 pide los tres pasos sobre un árbol que no existe (`02` §6). El neto en bytes se anota igual. No reemplaza al ADR 003: `AGENTS.md` sigue siendo la fuente; estos tres roles nombran las secciones |
| (4) Se borran `mode` y `permissions` de los 4. El registro dice que son formato de otra herramienta, ignorado por Claude Code, y no verificado en Cursor, Gemini y Copilot | Conservarlos documentados; mantener dos versiones de cada agente | Avance, decisión 7. Entra en el ADR 010 |
| (5) El reviewer pierde `Write`. La sesión escribe `07_pruebas_qa.md` y `tests/` con lo que él devuelve. La segunda invocación, en modo lectura, cuenta como una de las 2 vueltas | Conservar `Write` como no garantizado; sacar el harness del flujo | Avance, decisiones 8 y 14. El procedimiento queda solo en `sdd-validate` (ADR 009). Entra en el ADR 010 |
| (6) `model` explícito en los 4. Planner y reviewer: `inherit`, sin pasar por RF-4. Implementer: `inherit`, sin corrida y sin fixture. Coordinator: `sonnet` solo si F1 pasa; si no pasa, `inherit`. **Proponer ADR 012** al cierre, con el resultado que F1 haya aceptado | `haiku` para el coordinator; declarar solo `inherit` en los cuatro; pasar el implementer a `sonnet` rehaciendo el paso sobre el árbol de hoy | Avance, decisiones 5 y 12, condicionadas por `02` §3.2 y §6. Un solo commit de `ACTUALIZAR.md` (`63f586d`); el padre no tiene el archivo. Rehacer F1.5, F5.1 o F6.2 cumpliría el «Hecho cuando» sin trabajo del modelo. Si más adelante hay una revisión separable, la comparación del implementer se corre antes de aceptar el cambio |
| (7) Umbral de costo: 125 % de los bytes del modelo actual, por caso. Una repetición si el propuesto falla un caso. Los tres pasos de referencia de la `003` son el techo del implementer, y no se corren | Sin umbral; repetir hasta que pase; re-ejecutar un feature completo | Avance, decisión 15. RF-4 |
| (8) Dentro de 0.5.0 sin publicar. Un ítem en el changelog ya fechado. Manifiesto regenerado en F7, con E20. Ninguna fase se llama Cierre | Versión 0.5.1; una fase llamada Cierre | Avance, decisión 9. Una fase con ese nombre no ejecuta `/sdd-cierre` |
| (9) Una sola pieza, `restricciones-de-rol.md`, junto a `avance-y-tiempos.md`, grupo A. Los agentes enlazan | Copiar la tabla en cada agente; ponerla en `AGENTS.md` | Avance, decisión 15. Aplica el ADR 009. No es un ADR nuevo |
| (10) `/sdd-init` no sugiere `allow` ni `deny`. Al cierre se propone una deuda, sin crearla: que `/sdd-init` sugiera esos fragmentos | Meter los fragmentos en `sdd-init` ahora | Avance, decisión 15. `02` §1.2. Fuera de alcance de la spec |
| (11) La carpeta `003_OptimizarArnes` no se toca. F0 comprueba que sigue cerrada. Si otra spec editó un archivo compartido, se relee y no se revierten sus líneas. Quien congele último regenera el manifiesto y repite E20 | Implementar antes del cierre de la `003`; corregir la fila del índice aquí; tocar «Formato y numeración» | Avance, decisiones 13 y 17. `02` §1.1 y §6. La fila 004 del índice se corrige en `/sdd-cierre` |
| (12) El `deny` de la pieza es solo los cinco patrones Bash de Git de §4. La ruta del planner y del coordinator queda en instrucción, no garantizada | `disallowedTools` con patrón; un deny de escritura para toda la sesión; inventar `Edit(ruta)` | RF-1: un campo o un patrón no verificado no se usa. `02` §6 |
| (13) La lista `Agent(...)` del coordinator se queda. La fila 6 dice que no restringe cuando es subagente | Quitar `Agent`; tratar la lista como garantía también en subagente | Avance, decisión 11. RF-7 fila 6 |
| (14) Si `template/` cambió desde el `02`, F0 y F7 vuelven a medir y esa cifra es la que vale. El `02` no se reescribe | Seguir con 41 archivos, 8 403 bytes o el arranque de §3.1 aunque el árbol ya no coincida | `02` §1 y §6 |

ADR que este plan propone y no crea:

| Propuesta | Título | Qué fija |
|---|---|---|
| 010 | `RestriccionesDeRolSinHooks` | Decisión (2), con (4) y (5) como consecuencias. Alternativas: hooks, `disallowedTools` con patrón, conservar `mode`/`permissions` |
| 011 | `OmitClaudeMdEnTresRoles` | Decisión (3). No reemplaza al ADR 003 |
| 012 | `ModeloExplicitoPorRol` | Decisión (6), redactada al cierre con lo que F1 haya aceptado. Si el coordinator no pasa, el ADR dice `inherit`, no `sonnet` |

Hoy el índice local de `.docs/004_adr/` acaba en 009. `02` §3.2: `git ls-tree` de la remota ya descargada no listó ADR. El número es el siguiente libre en el momento de crearlos; se vuelve a mirar local y remota ya descargada. Ninguna fase de este plan crea el archivo ni la fila del índice.

Creados el 2026-10-25, con el visto bueno del usuario. El 010 ya era `RutaSiNoHayUnSoloPaso`, así que estos tomaron el siguiente libre: [011](../../004_adr/011_RestriccionesDeRolSinHooks.md), [012](../../004_adr/012_OmitClaudeMdEnTresRoles.md) y [013](../../004_adr/013_ModeloExplicitoPorRol.md). La línea de `MEMORY.md` quedó escrita el mismo día. La deuda nueva es `006_SddInitSugiereAllowDeny`.

## 7. Fases y estimado
La unidad es el minuto. F0 no edita `template/`. F1 no está hecha: la decisión 12 no se da por pasada. Ninguna fase se llama Cierre ni ejecuta `/sdd-cierre`.

F0 antes de cualquier edición. F2 puede seguir a F0 sin esperar a F1. F3 espera el resultado de F1 y el archivo de F2. F4 después de F3 (el comando y el agente dicen lo mismo). F5 después de F2, F3 y F4. F6 después de F5. F7 al final. Una edición de `template/` posterior a F7 obliga a repetir F7.

| Fase | Contenido | RF | Estimado |
|---|---|---|---|
| F0 | La sesión principal vuelve a medir y copia la línea base en `05_avance.md`. No edita `template/` ni docs de la raíz. Escribe ahí las 5 peticiones de F1, antes de lanzarlas | RF-13, RF-18 | 15 min |
| F1 | Comparación RF-4 solo del coordinator, en copia desechable. El implementer no se lanza: la sesión anota `inherit` y el motivo de `02` §3.2 y §6 | RF-4, RF-5, RF-11 | 50 min |
| F2 | Crear la pieza de restricciones y la frase en `SKILL.md` | RF-1, RF-2, RF-5, RF-6, RF-7, RF-9, RF-10, RF-17, RNF-4 | 30 min |
| F3 | Frontmatter y el ajuste de cuerpo de los 4 agentes, con el `model` que F1 haya aceptado | RF-3, RF-6, RF-8, RF-11, RF-12, RF-20, RF-21 | 25 min |
| F4 | Paso 5 de `sdd-validate.md`. Sección «Qué lee cada rol» en los dos `AGENTS.md` | RF-12, RF-16, RF-20, RF-21 | 20 min |
| F5 | `seguridad-basica` completa sobre lo tocado. Si falla, se corrige el archivo de su fase y F5 se repite una vez | RF-15 | 20 min |
| F6 | Un ítem en el bloque 0.5.0 de `CHANGELOG.md` | RF-16, RF-19, RF-20, RF-21 | 10 min |
| F7 | Congelar: manifiesto, E20, contraste, chequeos, ensayo de instalación y medición sobre el contenido final | RF-13, RF-14, RF-18, RF-19, RNF-2, RNF-7 | 35 min |
| | **Total** | | **205 min** |

**F0.** Releer, sin editar la carpeta `003`: la fila del índice sigue en Implementado y el PUNTO DE RETOMO sigue en cerrada. En la sesión que va a medir, `CLAUDE_CODE_SUBAGENT_MODEL` y `CLAUDE_CODE_SUBAGENT_MODEL_FORCE` están unset; si no, F1 no arranca. `wc -c` de los 4 agentes, `template/CLAUDE.md`, `template/AGENTS.md`, `SKILL.md`, `sdd-validate.md`, las cuatro `description`, archivos de `template/` y filas de datos del manifiesto. `git status` de la lista de §2. Si un archivo de esa lista tiene cambios ajenos, se anota y no se revierten. Si el conteo no coincide con `02` §3, vale el nuevo y se anota; el `02` no se toca. Heredado = bytes de `template/CLAUDE.md` + bytes de `template/AGENTS.md` (en el `02`, 11 + 8 403 = 8 414). Arranque de hoy = bytes del agente + heredado.

**F1.** La sesión principal lanza al coordinator. El implementer no lanza subagentes y no escribe `05_avance.md`. Las dos corridas de un caso usan el mismo texto de agente (el actual): el modelo entra como parámetro de la llamada, que gana al frontmatter. Así el resultado es del modelo y no de `omitClaudeMd`.

Lista fija, copiada a `05_avance.md` antes de la primera corrida:

1. Petición libre de análisis nuevo, que no coincida con una carpeta del índice. Respuesta esperada: elige `sdd-spec` y no implementa.
2. Spec aprobada que pide el plan. Respuesta esperada: elige `sdd-plan` y no pasa a implementar.
3. Pide implementar un feature cuya copia no tiene `04_tasks.md` aprobado. Respuesta esperada: frena en la puerta. No escribe código. Se anota el comando que elige.
4. Pide validar un feature con la implementación terminada (`003_OptimizarArnes`, mientras siga cerrada). Respuesta esperada: elige `sdd-validate` y no abre una implementación.
5. El `AGENTS.md` de la copia es el de `template/` intacto (lleva el comentario `SDD-INIT-PENDIENTE`). Pide una spec. Respuesta esperada: para, sugiere `/sdd-init` y no ejecuta ningún `/sdd*`.

Los casos 1 a 4 usan otra copia, cuyo `AGENTS.md` no contiene el literal `SDD-INIT-PENDIENTE`. El de la raíz no sirve: el literal aparece dentro de la regla y da un falso positivo (`02` §3.3). La copia sale de `mktemp -d`, no se versiona y se borra al terminar. Nada se escribe en el repo real.

Una corrida por caso y por modelo (el de la sesión, con su nombre exacto, y `sonnet`). Si el propuesto falla un caso, se repite ese caso una vez. Si la repetición también falla, el rol no pasa. El propuesto se acepta solo si cumple todo lo que cumplió el actual. Por caso, los bytes que la instrucción manda leer más los bytes de la respuesta (`wc -c`) no superan el 125 % de los del actual, y los reintentos no son más que los del actual. Si la sesión ya está en el mismo id que resuelve `sonnet`, una corrida por caso basta y se anota la igualdad. Si el alias cae al heredado, o la variable `_FORCE` ignora el frontmatter, esa corrida no cuenta: se anota y se repite, o el coordinator queda en `inherit`. Si no hay una corrida válida, F3 escribe `inherit`. No se anota un pase.

El implementer no entra en esas corridas. La sesión anota en `05_avance.md`: declara `model: inherit`; no hay árbol previo a F1.5; no se fabrica un fixture. El neto hipotético de `omitClaudeMd` (arranque de hoy menos el heredado) se anota en F0 o aquí, y el campo no se pone.

**F2.** La pieza, en español, sin frontmatter de skill (igual que `avance-y-tiempos.md`) y sin scripts, lleva:

- Registro de campos con fecha 2026-10-25 y fuente. URL de subagentes: la de la decisión 2 del avance. Los hechos de permisos y de `disallowedTools` se atribuyen a esa lectura (decisiones 2, 3 y 11). Si no hay URL de la página de permisos en el avance, no se inventa: se escribe «documentación de permisos de Claude Code, leída el 2026-10-25».
- Por campo: soportado en Claude Code, ignorado, o no verificado. `mode` y `permissions`: formato de otra herramienta, ignorados por Claude Code. `model`, `omitClaudeMd`, `mode` y `permissions`: no verificado en Cursor, Gemini y Copilot. Quitar los dos últimos puede quitarles una función que aquí no se conoce. Hechos (a) a (d) de RF-1, más: los hooks por subagente existen y se descartan; `omitClaudeMd` no afecta a los `CLAUDE.md` de política; un alias ausente o bloqueado cae al heredado con aviso y el respaldo es `inherit`.
- La tabla de RF-7, ocho filas, con mecanismo y si está garantizada. Texto de cada fila como en la spec. Filas 2 y 3: no garantizada, instrucción; el deny de ruta no se incluye (decisión 12).
- Fragmento opcional `permissions.deny` de §4, con la advertencia de sesión entera. Recomendación, no garantía del kit.
- Fragmento opcional `permissions.allow` de RF-10, sin columna «garantizada». `/sdd-init` no se menciona como quien lo aplica.
- Límite conocido: la herramienta no carga solo secciones de `AGENTS.md`. `omitClaudeMd` quita todo el `CLAUDE.md`. Si un proyecto no importa `AGENTS.md` desde el suyo, el ahorro es cero.

`SKILL.md` gana una frase al lado del enlace a `avance-y-tiempos.md`: las restricciones de rol viven en `restricciones-de-rol.md`; se enlaza y no se copia.

**F3.** Aplicar la tabla de §1. El `model` del coordinator es el que F1 anotó como aceptado. El cuerpo del implementer conserva «No registras en Git», «No escribes `05_avance.md`» y «No ejecutas SQL». La frase «No supongas que la herramienta te inyecta `AGENTS.md`» se queda en los cuatro: en los tres con `omitClaudeMd` es cierta, y en el implementer no autoriza a saltarse las secciones que ya nombra. Las `description` quedan en los bytes que midió F0.

**F4.** El paso 5 de `sdd-validate.md` queda así: si se pide evidencia formal, la sesión escribe `07_pruebas_qa.md` y el harness de `tests/` con lo que el reviewer devuelve; el reviewer, en una segunda invocación, ejecuta el harness en modo lectura y devuelve el resultado; la sesión lo anota en `07_pruebas_qa.md`. Esa invocación cuenta como una de las 2 vueltas. Si una vuelta ya se gastó en CAMBIOS NECESARIOS, queda una para esa invocación. El comando no exige que el reviewer carezca de `Write` para seguir: con el reviewer anterior la sesión escribe igual y solo se pierde la garantía de la fila 1 (RF-21 b). El reviewer nuevo, aunque el comando que tenga delante sea el anterior, devuelve el contenido y no escribe esos archivos (RF-21 a).

En los dos `AGENTS.md`, solo «Qué lee cada rol». Frase: el `implementer` puede recibir el archivo inyectado; planner, reviewer y coordinator no, porque declaran `omitClaudeMd`; cada rol lee solo las secciones que su instrucción nombra. Filas: el coordinator sigue comprobando `SDD-INIT-PENDIENTE` y suma «Git» y «Reglas duras»; planner y reviewer suman esas dos a las que ya tienen; la fila del implementer no cambia. No se copia la redacción de Git ni de las reglas duras. No se toca «Formato y numeración».

**F6.** Un solo ítem `-` dentro de `## 0.5.0 — 2026-10-24`, sin cambiar versión ni fecha. Dice: modelo explícito por rol; `omitClaudeMd` en planner, reviewer y coordinator; se quitan `mode` y `permissions`; el reviewer ya no tiene `Write` y la evidencia formal la escribe la sesión; `restricciones-de-rol.md` es NUEVO (grupo A) y se incorpora junto con los agentes; «Qué lee cada rol» llega por fusión manual. Los cuatro puntos de quien personalizó un agente (RF-16) y las dos combinaciones de RF-21 van en ese mismo ítem. Releer el bloque antes de escribir. No quitar ítems que haya agregado otra spec. `VERSION` sigue como está, también si no termina en LF.

**F7.** Lo ejecuta la sesión principal. El reviewer no corre E1–E20. Orden: confirmar que los cambios de `template/` y de los docs de la raíz ya están; regenerar `manifiestos/0.5.0.md` con el algoritmo vigente (BOM, CRLF, espacios finales, líneas vacías finales, un LF, SHA-256 en minúsculas); E20; contraste contra `manifiestos/0.4.0.md`, anotado con lo que salga; chequeos de `AGENTS.md` › Comandos en cada archivo tocado (`file -i` utf-8, sin BOM, `name` del agente igual al del archivo, rutas citadas existentes, en `template/` sin marcadores `<<…>>` salvo `<<NOMBRE_PROYECTO>>` e `<<IDIOMA>>`); ensayo `tmp=$(mktemp -d); cp -R --update=none template/. "$tmp"/`, nunca en un proyecto real. Medición de RF-13 sobre ese contenido: arranque por rol = bytes del agente + heredado, y heredado = 0 en quien tenga `omitClaudeMd: true`. Lista de RF-14: cada verificación, con la pieza donde sigue. El neto del implementer se anota y el campo sigue ausente. En los otros tres el campo no se revierte aunque el neto de otro proyecto sea cero; en este `template/` se anota el neto real. Las `description` siguen sumando lo que midió F0. Si la copia local de `.agents/` quedó distinta, se propone refrescarla y no se hace. Filas del manifiesto = archivos de `template/`. Si no cuadran, se regenera otra vez antes de dar F7 por cerrada.

Verificaciones que RF-14 tiene que poder señalar, y la pieza que las conserva:

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

## 8. Criterios de aceptación
| Criterio | Cómo se acepta |
|---|---|
| RF-1, RF-2, RF-17, RNF-6 | La pieza trae fecha, fuente y el rótulo no verificado en Cursor, Gemini y Copilot. Un campo sin esa lectura no aparece en el frontmatter |
| RF-3, RF-4, RF-5 | Los 4 agentes tienen `model`. Planner y reviewer dicen `inherit`. El implementer dice `inherit`, con el motivo de §3.2 en `05_avance.md` y sin corrida. El coordinator dice `sonnet` solo con la evidencia de F1 (casos, umbral, nombres exactos); si no hay corrida válida, dice `inherit` |
| RF-6, RF-8 | Ningún agente tiene `mode` ni `permissions`. El reviewer no tiene `Write` ni `Edit`. `tools` del resto no gana entradas |
| RF-7, RF-9, RF-10, RNF-4 | La tabla tiene las 8 filas. Los 4 agentes y `SKILL.md` enlazan la pieza y no la copian. El `deny` y el `allow` están en la pieza y no en `.claude/`. `sdd-init.md` no cambió |
| RF-11, RF-12, RNF-2 | `omitClaudeMd: true` en los tres roles de la decisión (3) y ausente en el implementer. Cada regla de Git y de reglas duras que esos tres dejan de recibir inyectada tiene la sección nombrada en su cuerpo y en «Qué lee cada rol». El neto queda anotado. Los tres no se revierten por un ahorro cero en otro proyecto |
| RF-13 | `05_avance.md` tiene la línea base de F0, anterior al primer cambio de `template/`, y la medición de F7 sobre el contenido final |
| RF-14, RNF-1 | La tabla de F7 está completa en `05_avance.md`. Ninguna de esas piezas perdió la verificación |
| RF-15 | F5 aplicó la skill completa y no dejó un hallazgo abierto |
| RF-16, RF-19, RF-20, RF-21 | Los dos `AGENTS.md` coinciden en «Qué lee cada rol». El changelog tiene el ítem en 0.5.0, con los cuatro puntos de quien personalizó y las dos combinaciones. El paso 5 de `sdd-validate.md` es el de F4. `VERSION` sigue en 0.5.0 |
| RF-18 | F7 en el orden de arriba. E20 pasa. El contraste contra 0.4.0 está anotado. Chequeos de Comandos pasan. El ensayo en `mktemp -d` no pisa. Si `template/` cambió después, F7 se repitió |
| RNF-3, RNF-5, RNF-7 | UTF-8 sin BOM, español, sin scripts. Los únicos nombres de modelo son `sonnet` e `inherit`. Un agente viejo que aún tenga `mode` y `permissions` sigue siendo legible. Las `description` no crecieron |
| Deuda 003 | Ninguna fase la edita. `/sdd-cierre`, fuera de estas fases, la marca `Resuelta` con «Atendida por» y «Cómo se atendió», propone la deuda de `/sdd-init` sin crearla, y propone los ADR 010, 011 y 012 sin haberlos creado antes |
| Copia local | Si `.agents/agents/` sigue distinto de `template/`, queda pendiente de refrescar y no bloquea F7 |
