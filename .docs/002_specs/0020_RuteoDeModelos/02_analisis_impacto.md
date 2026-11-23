# 02 — Análisis de impacto: Ruteo de modelos

## 1. Resumen del requerimiento
Dos cambios en una spec. (a) Ruteo: las tareas sin rol (comandos sin fase, preparación de commits y
tareas sueltas del chat) corren con el nivel de modelo que les toca, delegadas en un subagente con
su modelo, sin que la persona cambie `/model`. (b) Argumentos: los comandos `/sdd-*` dejan de leer
`$1` y `$2`, que en Claude Code son el segundo y tercer argumento (base 0) y en OpenCode el primero y
segundo (base 1).

### 1.1 Antecedentes
Búsqueda en `.docs/002_specs/README.md` (2026-11-22) por modelo, subagente, comando, argumento y
commit:
- `004_ModeloYPermisosAgentes`: modelo por rol en el frontmatter. Se reutiliza el campo `model:`.
- `0013_ModelosPorAgente`: tabla «Modelo por rol», roles Haiku con encargo cerrado y rol en el chat
  salvo cambio de modelo (ADR 0028). Se reutiliza: la tabla nueva va junto a esa y `explore` toma la
  búsqueda de nivel Bajo con su barrera de encargo cerrado.
- `0014_ConfiguracionGuiada`: `## Modelos` de `.agents/local/configuracion.md`, adaptadores de rol y
  tabla de equivalencias por nivel (ADR 0029 y 0030). Se reutiliza para RF-11 sin mecanismo nuevo.
- `0019_FlujoEficiente`: comprobación antes de cada lanzamiento y «Solo roles del kit». Esta spec
  añade una excepción acotada en esa misma sección (RF-8); no la contradice.
- `0016_CierreEnUnaCorrida`: el cierre propone sus commits con una confirmación (ADR 0031). RF-5
  delega solo la preparación; la confirmación y el `git commit` no cambian.
- Sin antecedentes sobre la lectura de argumentos: `grep -rn '\$1\|\$2' .docs/002_specs/*/0*_spec.md`
  no da reglas previas; el `$1` se usa desde la primera versión de los comandos.

### 1.2 Deuda técnica relacionada
`.docs/003_deuda/README.md` (2026-11-22): ninguna deuda `Abierta` ni `En atención`. La deuda 007
(nombres que chocan con otras herramientas, `Resuelta`) se tiene en cuenta al nombrar el rol
`auxiliar`: no coincide con un archivo o carpeta reservada de Claude Code, Cursor, OpenCode, Gemini,
Copilot ni Kiro.

## 2. Módulos, menú, tablas y archivos involucrados
### 2.1 Menú / opciones / rutas de acceso
Comandos `/sdd-*` (las herramientas los leen por enlace desde `.agents/commands/`) y el chat de la
sesión principal. No hay menú de aplicación.

### 2.2 Tablas (existentes y nuevas)
No aplica: el kit no tiene base de datos.

### 2.3 Archivos de código (ruta, capa)
Todo en `template/` salvo lo indicado. Capa = pieza del kit.

| Archivo | Pieza | Qué toca |
|---|---|---|
| `template/.agents/agents/auxiliar.md` (nuevo) | Rol | RF-7 |
| `template/.agents/skills/sdd/restricciones-de-rol.md` | Fuente de modelos y restricciones | fila en «Modelo por rol», sección nueva con la tabla de RF-1 y la regla de RF-3, RF-6 y RF-9, filas de la tabla de restricciones |
| `template/.agents/skills/sdd/SKILL.md` | Skill `sdd` | tabla de fases (estado y estructura), «Dónde corre cada rol» (excepción de RF-8, motivo «ruteo»), «Si no hay un solo paso» (línea del estado y la estructura) |
| `template/.agents/commands/sdd-status.md`, `sdd-estructura.md` | Comandos | delegación fija (RF-4) y argumentos |
| `template/.agents/commands/sdd-{spec,clarify,plan,tasks,implement,validate,flujo,cierre,change,pausa}.md` | Comandos | argumentos (RF-13, RF-14) y «Siguiente paso» sin `$1` |
| `template/.agents/README.md` | Layout | «Roles y comandos» (rol nuevo), «Agregar un comando» (corrige `$1, $2… por posición`) y regla única de argumentos |
| `template/.agents/skills/git-commit/SKILL.md` | Skill | separa preparación y `git commit` (RF-5) |
| `template/.agents/skills/formato-respuesta/SKILL.md` | Skill | modelo del subagente en Contexto (RF-10) |
| `template/.agents/skills/sdd/avance-y-tiempos.md` | Contrato de tiempos | motivo «ruteo» en la lista de motivos de la viñeta **Modelo** (RF-10), que es donde viven |
| `template/.agents/configuracion.ejemplo.md`, `template/.agents/commands/sdd-config.md` | Configuración | `auxiliar` en el paso de modelos (RF-11) |
| `template/.agents/sync-agents.md` | Enlaces | adaptador de OpenCode para `auxiliar` (`mode: subagent`) |
| `template/.agents/validar-estructura.md` | Validación | E9: ningún comando con `$0`, `$1`, `$2`… |
| `template/AGENTS.md` | Siempre cargado | un renglón que remite a la tabla de ruteo y dice cuándo leerla |
| `README.md`, `ChangelogAgentKitSdd.md`, `VersionAgentKitSdd`, `manifiestos/0.15.0.md` (raíz) | Docs del kit | lista de roles, versión 0.15.0, huella (E20) |
| `.docs/004_adr/0037_…md` (raíz) | ADR | delegación con subagente frente a `model:` solo |
| `AGENTS.md` (raíz) y `.agents/` (raíz, local) | Mantenimiento | réplica con `cp` y el mismo renglón de ruteo |

### 2.4 Código legado o sin uso
- `$ARGUMENTS` en `sdd.md`, `sdd-constitucion.md` y `sdd-init.md` ya es correcto en las dos
  herramientas: no se toca.
- `coordinator.md` no lanza a `auxiliar`: dentro de una fase, la preparación de commits la lanza la
  sesión principal (cierre) y la delegación fija la hace la herramienta. Su lista `Agent(...)` no cambia.

## 3. Datos reales
Sin base de datos. Conteos del repositorio (2026-11-22, `grep` en `template/.agents/commands/`):
- 12 comandos leen `$1` o `$2`: `sdd-spec`, `sdd-clarify`, `sdd-plan`, `sdd-tasks`, `sdd-implement`,
  `sdd-validate`, `sdd-flujo`, `sdd-cierre`, `sdd-change`, `sdd-status`, `sdd-estructura` y
  `sdd-pausa`. Solo `sdd-implement` usa `$2`.
- 3 comandos usan `$ARGUMENTS` y ningún índice: `sdd`, `sdd-constitucion`, `sdd-init`.
- `template/.agents/README.md:85` documenta mal la convención (`$1`, `$2`… por posición).
- Evidencia del error en esta spec (`05_avance.md`, decisión 10): `/sdd-spec 0020_RuteoDeModelos El
  chat …` tomó `El` como carpeta y `/sdd-clarify 0020_RuteoDeModelos` dejó `$1` literal.
- Costo: simulación del 2026-11-22 (decisión 4 de `05_avance.md`). Con 130k de contexto, el chat de
  nivel Alto cuesta 0.04 a 1.22 USD por tarea; un subagente de nivel Medio 0.09 a 0.48; Bajo 0.08 a
  0.10; `model:` sin subagente 0.53 a 2.03 (siempre el peor). El costo fijo de un subagente ronda 10 s.

## 4. Flujo actual
- Comandos sin rol (`/sdd-status`, `/sdd-estructura`, `/sdd-version`, `/sdd-enlaces`): corren en la
  sesión principal con su modelo. `sdd-estructura.md` lo dice («No lanza un subagente») y
  `SKILL.md` › «Si no hay un solo paso» también («El cierre, el estado y la validación de estructura
  siguen en la sesión principal»).
- Commits: la sesión principal sigue `git-commit` de punta a punta (detección, verificación,
  mensaje, stage, confirmación y commit).
- Tareas sueltas: las hace el chat. Solo se lanzan roles del kit en una fase; fuera de una fase no
  hay regla escrita.
- Única salida para bajar de nivel: `/model` a mano, que la persona descartó (decisión 2).
- Argumentos: `$1` en Claude Code es el segundo argumento. Con una sola palabra queda literal; con
  varias, toma la segunda como carpeta.

## 5. Reglas de negocio: conocidas vs código
| Regla | Dónde vive | Qué cambia |
|---|---|---|
| Modelo de cada rol | `restricciones-de-rol.md` › «Modelo por rol» | se añade `auxiliar`; la tabla de tareas sin rol va al lado y remite a esta para el modelo |
| Rol en el chat salvo cambio de modelo | `SKILL.md` › «Dónde corre cada rol» | sin cambio para las fases (RF-2) |
| Solo roles del kit en una fase | `SKILL.md` › «Dónde corre cada rol» | excepción: `auxiliar` para preparar commits y delegación fija (RF-8) |
| Ningún agente usa la red | `restricciones-de-rol.md`, tabla de restricciones | sin cambio: `auxiliar` no lista red ni conectores; la lectura de conectores queda no delegable (decisión de la persona) |
| `git commit` solo con confirmación | `AGENTS.md` › Git y `git-commit` | sin cambio; solo se separa quién prepara |
| Estado y estructura en la sesión principal | `SKILL.md`, `sdd-estructura.md` | pasan a delegación fija (RF-4), con la excepción escrita en su fuente |
| `05_avance.md` lo escribe la sesión principal | `avance-y-tiempos.md` | sin cambio; `auxiliar` no lo escribe |
| Argumentos por posición | `README.md` › «Agregar un comando» | regla nueva: carpeta = primera palabra de `$ARGUMENTS`; marcador literal = ausente |

## 6. Riesgos y casos borde
- **OpenCode y claves desconocidas.** OpenCode V2 descarta un agente con una clave que no admite
  (`agent file skipped … at ["tools"]`, `sync-agents.md` › «OpenCode»). Si su esquema de comandos es
  igual de estricto, `context: fork` haría que `/sdd-status` desaparezca en OpenCode. Mitigación: F0
  lo verifica en su documentación; si rechaza la clave, se para y se decide (adaptador de comandos por
  herramienta, como dice el caso límite de la spec, o delegación fija solo con `agent:`).
- **Cursor y la sustitución.** Su documentación no confirma `$ARGUMENTS` (2026-11-22). La regla de
  RF-13 prevé que los argumentos lleguen al final del texto, sin sustitución.
- **Delegación en Claude Code.** `context: fork` y `agent:` en un comando de `.claude/commands/`
  (hecho verificado el 2026-11-22 en la idea; F0 lo confirma en la documentación). Si la herramienta
  ignora la clave, el comando corre en el chat (respaldo, RF-9).
- **Costo fijo del subagente.** En un `/sdd-status` corto el chat con la caché cargada puede ganar.
  Lo decide la medición (RF-12, F6); si no ahorra, la fila sale de la tabla.
- **`/sdd-status` pregunta.** Con la marca de pausa pregunta por el hueco largo. Como subagente no
  puede preguntar: devuelve la pregunta al chat (caso límite de la spec).
- **`/sdd-estructura` escribe la bandera local.** Es la única escritura del comando, fuera de
  `05_avance.md` y del índice, así que cumple RF-3. `auxiliar` no tiene `Write`: la escribe por
  shell tras `git check-ignore`. Es instrucción, no garantía.
- **Huella del manifiesto.** Todo cambio en `template/` después de generar `manifiestos/0.15.0.md`
  deja su huella vieja (trampa de `MEMORY.md`). La versión va en la última fase, tras la medición.
- **Réplica local.** `.agents/` de la raíz no está en Git: se replica con `cp` y se comprueba con
  `diff -rq` (trampa de `MEMORY.md`).
- **Bajo que pide Bash.** La revisión de una salida larga de comando no cabe en `explore` (sin
  `Bash`). Con la decisión de la persona sube a `auxiliar` (Medio); si es un archivo de log, va a
  `explore` (Bajo).

## 7. Preguntas abiertas
Ninguna. Las dos de diseño se resolvieron con la persona (decisiones 12 y 13 de `05_avance.md`):
un solo rol nuevo `auxiliar` más `explore`, y la lectura de conectores no delegable.
