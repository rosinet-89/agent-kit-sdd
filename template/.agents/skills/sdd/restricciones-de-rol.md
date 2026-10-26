# Restricciones de rol

Fuente única de las restricciones de `coordinator`, `planner`, `implementer` y `reviewer`.
Los agentes enlazan este archivo. No copian la tabla.

## Registro de campos

Fecha de la lectura: 2026-10-25.

Fuente de subagentes: https://code.claude.com/docs/en/sub-agents (decisiones 2, 3 y 11 del avance de esta spec).

Los hechos de permisos y de `disallowedTools` se atribuyen a esa misma lectura: documentación de permisos de Claude Code, leída el 2026-10-25. No hay URL de la página de permisos en el avance y no se inventa.

Cada campo queda soportado en Claude Code, ignorado, o no verificado.

| Campo | Claude Code |
|---|---|
| `mode` | Ignorado. Formato de otra herramienta. |
| `permissions` | Ignorado. Formato de otra herramienta. |
| `model` | Soportado. Hecho (c). El respaldo es `inherit`. |
| `omitClaudeMd` | Soportado. Hecho (d). No afecta a los `CLAUDE.md` de política. |
| `tools` | Soportado. Hecho (a). |
| `disallowedTools` | Soportado como lista de herramientas enteras. Hecho (b): no acepta patrones con argumentos. |
| `hooks` | Existen por subagente. Se descartan. |

Fecha de lectura de Cursor, Gemini CLI, GitHub Copilot y Kiro: 2026-10-25. Sin fecha y fuente, esa herramienta no tiene veredicto.

| Herramienta | Fuente |
|---|---|
| Cursor | https://cursor.com/docs/subagents |
| Gemini CLI | https://geminicli.com/docs/core/subagents/ |
| GitHub Copilot | https://docs.github.com/en/copilot/reference/custom-agents-configuration |
| Kiro | https://kiro.dev/docs/custom-agents/configuration-reference/ |

Una celda «Sin veredicto» no clasifica el campo. Los hechos (a), (c) y (d) siguen siendo los de Claude Code.

| Campo | Cursor | Gemini CLI | GitHub Copilot | Kiro |
|---|---|---|---|---|
| `model` | Soportado. La tabla de campos lo lista: `inherit` o un id, con parámetros entre corchetes. No viaja igual que el hecho (c): si el admin bloquea el modelo, el plan no lo trae o hace falta Max Mode, Cursor cae a un modelo compatible. El hecho (c) permanece. | Soportado. El esquema lo lista. El valor por defecto documentado es `inherit` (el modelo de la sesión principal). No viaja igual que el hecho (c): la página no documenta ese orden de resolución. El hecho (c) permanece. | Soportado. La tabla de propiedades lo lista. Si no se pone, hereda el modelo por defecto. No viaja igual que el hecho (c): la página no documenta el valor `inherit` ni ese orden. El hecho (c) permanece. | Soportado. Es el id del modelo. Si no se indica, usa el modelo por defecto. No viaja igual que el hecho (c): si el id no está disponible, el primer prompt puede fallar en lugar de caer solo. El hecho (c) permanece. |
| `omitClaudeMd` | Sin veredicto. La página no nombra la clave. Falta una frase que la clasifique como soportada, ignorada o que provoca error. | Sin veredicto. El esquema no nombra la clave. Falta una frase que la clasifique como soportada, ignorada o que provoca error. | Sin veredicto. La tabla de propiedades no nombra la clave. La nota que ignora `argument-hint` y `handoffs` en el cloud agent no incluye esta clave. Falta una frase que la clasifique. | Sin veredicto. La lista de campos no nombra la clave. Falta una frase que la clasifique como soportada, ignorada o que provoca error. |
| `tools` | Sin veredicto. La tabla de campos lista `name`, `description`, `model`, `readonly` e `is_background`, y no incluye `tools`. La pregunta de MCP dice que el subagente hereda las herramientas del padre; esa frase no clasifica la clave. Falta si `tools` en el frontmatter se soporta, se ignora o provoca error. | Soportado. Lista de nombres, con comodines `*`, `mcp_*` y `mcp_server_*`. Si se omite, hereda las de la sesión padre. No viaja igual que el hecho (a): la página no documenta `Agent(tipo1, tipo2)`. En la misma página, un subagente no puede llamar a otros subagentes aunque tenga `*`. El hecho (a) permanece. | Soportado. Lista o cadena separada por comas. Si no se pone, vale todas; `tools: []` no deja ninguna. Los nombres no reconocidos se ignoran. Alias sin distinción de mayúsculas: `Write` y `Edit` son `edit`; `WebFetch` y `WebSearch` son `web`; `Agent` y `Task` son `agent`. No viaja igual que el hecho (a): `Agent(tipo1, tipo2)` no está en esa tabla. El hecho (a) permanece. | Soportado. Lista de nombres, con etiquetas `read`, `write`, `shell`, `web` y `subagent`, y selectores `@`, `@builtin` y `*`. El formato Markdown usa el mismo campo en el frontmatter. No viaja igual que el hecho (a): la página no documenta `Agent(tipo1, tipo2)`. El hecho (a) permanece. |

Hechos verificados el 2026-10-25:

- (a) `Agent(tipo1, tipo2)` en `tools` solo se aplica al agente que corre como hilo principal con `claude --agent`; en un subagente se ignora la lista y `Agent` permite lanzar subagentes hasta 3 capas.
- (b) `disallowedTools` no acepta patrones con argumentos y una entrada `Bash(git push *)` quita Bash entero; bloquear comandos exige `permissions.deny` en settings, que vale para toda la sesión y cubre comandos compuestos.
- (c) Orden de resolución del modelo: parámetro de la llamada, `model` del frontmatter, `CLAUDE_CODE_SUBAGENT_MODEL`, modelo de la sesión. `CLAUDE_CODE_SUBAGENT_MODEL_FORCE=1` ignora el frontmatter. Un alias bloqueado o ausente cae al modelo heredado con aviso. El respaldo es `inherit`.
- (d) `omitClaudeMd` quita todo el `CLAUDE.md`, no solo `AGENTS.md`. No afecta a los `CLAUDE.md` de política.

Los hooks por subagente existen y se descartan (exigirían un script; no se enmienda el ADR 004).

## Tabla de restricciones (RF-7)

Tres columnas. Ocho filas. Las filas 2 y 3 quedan NO garantizada, solo instrucción; el deny de ruta no se incluye (decisión 12 del plan).

| Restricción | Mecanismo | Garantizada |
|---|---|---|
| `reviewer` sin `Write` ni `Edit`. Que no modifique archivos por Bash (`sed -i`, redirecciones, `tee`) | `tools`. Instrucción de texto del agente | GARANTIZADA en Claude Code por `tools`. En GitHub Copilot también: `Write` y `Edit` son alias de `edit` (sin distinción de mayúsculas) y `tools` es la lista de permiso. No se extiende a Cursor, porque `tools` no tiene veredicto. No se extiende a Gemini CLI ni a Kiro: su fuente no documenta esos nombres. NO garantizada, instrucción de texto del agente |
| `planner` escribe solo los cinco documentos de su carpeta de feature | instrucción (tiene `Write`/`Edit`). Sin deny de ruta | NO garantizada (tiene `Write`/`Edit`); instrucción. Sin deny de ruta |
| `coordinator` escribe solo `05_avance.md` y usa el shell solo para la hora | instrucción. Sin deny de ruta | NO garantizada; instrucción. Sin deny de ruta |
| `implementer` no hace `git commit`, `push`, `fetch`, `pull` ni `sync`, y no ejecuta SQL | instrucción más el `deny` opcional de Git (`disallowedTools` no sirve) | NO garantizada; instrucción más el `deny` opcional de Git (`disallowedTools` no sirve) |
| `planner`, `reviewer` y `coordinator`, que conservan `Bash`, tampoco registran en Git | instrucción más el `deny` opcional de Git | NO garantizada; instrucción más el `deny` opcional de Git |
| Ningún subagente lanza otros subagentes salvo el `coordinator` a los tres roles | `planner`, `implementer` y `reviewer` no listan `Agent`. La lista `Agent(planner, implementer, reviewer)` del `coordinator` solo restringe cuando corre como hilo principal; como subagente se ignora y podría lanzar subagentes hasta 3 capas | GARANTIZADA en Claude Code para `planner`, `implementer` y `reviewer` (no listan `Agent`). En GitHub Copilot, no listar `agent` (alias `Agent` y `Task`, sin distinción de mayúsculas) deja fuera esa herramienta; el hecho (a) no viaja, porque `Agent(tipo1, tipo2)` no está en los alias. Para el `coordinator`, NO garantizada en ese caso, y esa mitad no se extiende. No se extiende a Cursor (`tools` sin veredicto) ni a Gemini CLI ni a Kiro. Gemini CLI dice, en otra frase de su fuente, que un subagente no llama a otros subagentes; eso no es la garantía de esta fila |
| Ningún agente usa la red (`webfetch` y búsqueda web denegados) | `tools`, que no lista esas herramientas | GARANTIZADA en Claude Code por `tools`, que no lista esas herramientas. En GitHub Copilot, `WebFetch` y `WebSearch` son alias de `web` (sin distinción de mayúsculas) y no listarlos deja fuera esa herramienta donde el alias aplica. La fuente dice que `web` no aplica al cloud agent, y ahí esta fila no afirma la garantía por `tools`. No se extiende a Cursor (`tools` sin veredicto) ni a Gemini CLI ni a Kiro |
| `planner` consulta datos solo en modo lectura, con el comando de `AGENTS.md` › Comandos | instrucción (tiene `Bash`) | NO garantizada (tiene `Bash`); instrucción |

## Fragmento opcional `permissions.deny`

Texto de esta pieza. No se copia a `.claude/` ni a settings versionados. Recomendación, no garantía del kit. Solo estos cinco patrones:

```text
permissions:
  deny:
    - Bash(git commit *)
    - Bash(git push *)
    - Bash(git fetch *)
    - Bash(git pull *)
    - Bash(git sync *)
```

Advertencia: vale para toda la sesión, no por subagente, cubre comandos compuestos, y quien lo copie puede bloquear su propio Git. No hay deny de ruta ni de `Write`/`Edit`.

## Fragmento opcional `permissions.allow` (RF-10)

No es una fila de la tabla. No tiene columna «garantizada». Ningún agente lo aplica ni cambia su modo de permisos.

El responsable pone los comandos de su `AGENTS.md` › Comandos para que no pidan confirmación uno por uno. Ejemplo genérico, como texto:

```text
permissions:
  allow:
    - Bash(comando de AGENTS.md › Comandos)
```

## Límite conocido

La herramienta no carga solo secciones de `AGENTS.md`. `omitClaudeMd` quita todo el `CLAUDE.md`. Si un proyecto no importa `AGENTS.md` desde el suyo, el ahorro es cero.
