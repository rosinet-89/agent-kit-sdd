# Restricciones de rol

Fuente única de las restricciones y del modelo de `coordinator`, `planner`, `implementer`, `reviewer`,
`tester` y `explore`. Los agentes enlazan este archivo. No copian las tablas.

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

## Modelo por rol

| Rol | Modelo |
|---|---|
| `planner` | `opus` |
| `coordinator` | `opus` |
| `reviewer` | `opus` |
| `implementer` | `sonnet` |
| `tester` | `haiku` |
| `explore` | `haiku` |

Esta tabla es la fuente de los valores sugeridos de Claude Code. Lo aplicado por cada dev vive en la
sección `## Modelos` de `.agents/local/configuracion.md` (no versionada: `opus`, `sonnet`, `haiku`,
`inherit` o lo que eligió la persona) y se aplica con adaptadores de rol (`.agents/sync-agents.md`),
nunca escribiendo en `.agents/agents/`. El resto del kit remite a esta tabla y no la copia.

El orden de resolución del modelo en Claude Code es el hecho (c) de «Registro de campos»; no se copia aquí.

## Roles Haiku

`tester` y `explore` son los roles Haiku: rápidos más que profundos. No diseñan, no deciden, no juzgan. Esta sección es la fuente única de lo que cada rol Haiku comprueba al recibir un encargo; `tester.md` y `explore.md` remiten aquí y no la copian.

Un **encargo cerrado** trae todo lo que el rol necesita y no le deja decidir nada: objetivo, alcance (rutas o destinos), pasos o casos y formato de salida.

Primera barrera, quien lanza: ANTES de lanzar un rol Haiku, quien lo lanza (la sesión principal, o el coordinator cuando atiende la fase como sesión) comprueba que el encargo es cerrado y que no pide diseñar, decidir, elegir ni juzgar. Si falta algo o lo pide, no lo lanza así: completa el encargo o hace esa parte él.

Segunda barrera, el rol (solo instrucción): aunque quien lanza ya comprobó, ANTES de usar cualquier herramienta el rol repite la comprobación en este orden.

1. Completitud. Si falta alguno de esos cuatro elementos, la respuesta es `ENCARGO INCOMPLETO`: lista qué falta y el rol termina ahí, sin buscar ni ejecutar nada.
2. Fuera de rol (más abajo).

Si se cumplen las dos condiciones a la vez, responde con las dos etiquetas y no trabaja. Lo que falta no se rellena con supuestos: no se toma «todo el repo» como alcance ni se inventa un formato de salida.

- Si un caso concreto no trae destino o datos de partida, ese caso queda `bloqueado` con el motivo y el resto del encargo sigue. `bloqueado` es uno de los estados de un caso (`obtenido`, `bloqueado`, `manual`, `ok`, `falla`); su forma está en `plantillas.md` › `## 07_casos_prueba.md`.
- Si el encargo pide diseñar, decidir, elegir o juzgar, la respuesta es `ENCARGO FUERA DE ROL`: dice qué parte lo es y el rol no la hace.

## Tabla de restricciones (RF-7)

Tres columnas. Doce filas. Las filas 2 y 3 quedan NO garantizada, solo instrucción; el deny de ruta no se incluye (decisión 12 del plan). Las filas 10 a 12 distinguen lo que garantiza `tools` de lo que es solo instrucción.

| Restricción | Mecanismo | Garantizada |
|---|---|---|
| `reviewer` sin `Write` ni `Edit`. Que no modifique archivos por Bash (`sed -i`, redirecciones, `tee`) | `tools`. Instrucción de texto del agente | GARANTIZADA en Claude Code por `tools`. En GitHub Copilot también: `Write` y `Edit` son alias de `edit` (sin distinción de mayúsculas) y `tools` es la lista de permiso. No se extiende a Cursor, porque `tools` no tiene veredicto. No se extiende a Gemini CLI ni a Kiro: su fuente no documenta esos nombres. NO garantizada, instrucción de texto del agente |
| `planner` escribe solo los cinco documentos de su carpeta de feature | instrucción (tiene `Write`/`Edit`). Sin deny de ruta | NO garantizada (tiene `Write`/`Edit`); instrucción. Sin deny de ruta |
| `coordinator` escribe solo `05_avance.md` y usa el shell solo para la hora | instrucción. Sin deny de ruta | NO garantizada; instrucción. Sin deny de ruta |
| `implementer` no hace `git commit`, `push`, `fetch`, `pull` ni `sync` | instrucción más el `deny` opcional de Git (`disallowedTools` no sirve) | NO garantizada; instrucción más el `deny` opcional de Git (`disallowedTools` no sirve) |
| `planner`, `reviewer` y `coordinator`, que conservan `Bash`, tampoco registran en Git | instrucción más el `deny` opcional de Git | NO garantizada; instrucción más el `deny` opcional de Git |
| Ningún subagente lanza otros subagentes salvo el `coordinator` a los cinco roles (`planner`, `implementer`, `reviewer`, `tester` y `explore`) | `planner`, `implementer`, `reviewer`, `tester` y `explore` no listan `Agent`. La lista `Agent(planner, implementer, reviewer, tester, explore)` del `coordinator` solo restringe cuando corre como hilo principal; como subagente se ignora y podría lanzar subagentes hasta 3 capas | GARANTIZADA en Claude Code para `planner`, `implementer`, `reviewer`, `tester` y `explore` (no listan `Agent`). Quien atiende la fase como sesión lanza a `tester` y `explore`; el `coordinator` como subagente no los lanza. En GitHub Copilot, no listar `agent` (alias `Agent` y `Task`, sin distinción de mayúsculas) deja fuera esa herramienta; el hecho (a) no viaja, porque `Agent(tipo1, tipo2)` no está en los alias. Para el `coordinator`, NO garantizada en ese caso, y esa mitad no se extiende. No se extiende a Cursor (`tools` sin veredicto) ni a Gemini CLI ni a Kiro. Gemini CLI dice, en otra frase de su fuente, que un subagente no llama a otros subagentes; eso no es la garantía de esta fila |
| Ningún agente usa la red (`webfetch` y búsqueda web denegados). Única excepción: `tester`, solo contra los destinos de QA de la configuración de entornos y por su herramienta de apoyo; la excepción y su alcance están en `AGENTS.md` › Guardrails, regla 1, y no se copian aquí | `tools`, que no lista esas herramientas. La excepción de `tester` no añade `webfetch` ni búsqueda web: pasa por su herramienta de apoyo, y el destino lo comprueba `tester` (instrucción) | GARANTIZADA en Claude Code por `tools`, que no lista esas herramientas, salvo la excepción de `tester` (su herramienta de apoyo, regla 1), cuyo límite de destinos es instrucción. En GitHub Copilot, `WebFetch` y `WebSearch` son alias de `web` (sin distinción de mayúsculas) y no listarlos deja fuera esa herramienta donde el alias aplica. La fuente dice que `web` no aplica al cloud agent, y ahí esta fila no afirma la garantía por `tools`. No se extiende a Cursor (`tools` sin veredicto) ni a Gemini CLI ni a Kiro |
| `planner` consulta datos con el comando de `AGENTS.md` › Comandos, según `AGENTS.md` › Guardrails, regla 1 | instrucción (tiene `Bash`) | NO garantizada (tiene `Bash`); instrucción |
| Ningún agente escribe en la base de datos (`AGENTS.md` › Guardrails, regla 1) | instrucción | NO garantizada; instrucción. Sin permisos de BD que la impongan |
| `explore` es de solo lectura: sin escribir archivos, sin `Bash` y sin red | `tools`: solo herramientas de lectura y búsqueda (`Read`, `Glob`, `Grep`) | GARANTIZADA en Claude Code por `tools` cuando corre como subagente. En el chat (sesión con su mismo modelo) es solo instrucción: NO garantizada. No se extiende a otras herramientas |
| `tester` no tiene `Write` ni `Edit`, no escribe archivos por shell (`sed -i`, redirecciones, `tee`) y, como subagente, no tiene red propia | `tools`: no lista `Write`, `Edit`, `webfetch` ni búsqueda web. Lista `Bash` y las herramientas de apoyo del proyecto. Instrucción de texto del agente para no escribir por shell | GARANTIZADA en Claude Code, como subagente, por `tools`: sin `Write` ni `Edit` y sin red propia. NO garantizada: que no escriba por shell (tiene `Bash`); instrucción. En el chat todo es instrucción, NO garantizada. No se extiende a otras herramientas |
| `planner` escribe además `07_casos_prueba.md`; quien atiende la fase como sesión (incluido el `coordinator`) escribe las corridas de ese documento | instrucción (el `planner` tiene `Write`/`Edit`). Sin deny de ruta | NO garantizada; instrucción. Sin deny de ruta |

## Recomendación: usuario de BD de solo lectura

Sugerencia, no garantía del kit. La regla 1 de «Guardrails» es una instrucción. Como refuerzo
externo, las consultas de los agentes conviene que usen un usuario de BD con permisos solo de lectura.

## Fragmento opcional `permissions.deny`

Texto de esta pieza. No se copia a `.claude/` ni a settings versionados. Recomendación, no garantía del kit. Estos patrones, los cinco de Git y los clientes de BD (sugerencia, texto que se muestra):

```text
permissions:
  deny:
    - Bash(git commit *)
    - Bash(git push *)
    - Bash(git fetch *)
    - Bash(git pull *)
    - Bash(git sync *)
    - Bash(mysql *)
    - Bash(psql *)
    - Bash(sqlcmd *)
    - Bash(sqlplus *)
    - Bash(sqlite3 *)
    - Bash(mongosh *)
    - Bash(redis-cli *)
```

Advertencia: vale para toda la sesión, no por subagente, cubre comandos compuestos, y quien lo copie puede bloquear su propio Git. Los clientes de BD también bloquean las consultas de lectura por línea de comandos: si hace falta consultar, usa el mecanismo de `AGENTS.md` › Comandos. No hay deny de ruta ni de `Write`/`Edit`.

## Fragmento opcional `permissions.allow` (RF-10)

No es una fila de la tabla. No tiene columna «garantizada». Ningún agente lo aplica ni cambia su modo de permisos.

El responsable pone los comandos de su `AGENTS.md` › Comandos para que no pidan confirmación uno por uno. Qué comandos pone, y cuáles no, lo define `.agents/commands/sdd-init.md` (sección 3, punto 7); no se copia aquí. Ejemplo genérico, como texto:

```text
permissions:
  allow:
    - Bash(comando de AGENTS.md › Comandos)
```

## Límite conocido

La herramienta no carga solo secciones de `AGENTS.md`. `omitClaudeMd` quita todo el `CLAUDE.md`. Si un proyecto no importa `AGENTS.md` desde el suyo, el ahorro es cero.
