# ADR 0034: Única excepción de red de la regla Git: consultar la versión publicada del kit

- **Estado:** Aceptado
- **Fecha:** 2026-11-22
- **Origen:** `0017_PausaOrigenYContexto`

## Contexto
La regla Git de `AGENTS.md` prohíbe `push`, `pull`, `fetch` y `sync`. Un proyecto sabe qué versión
del kit tiene (`.agents/.kit-version`), pero no puede responder «¿tengo la última versión?» sin red
(issue #6). Restricciones: no se pasan credenciales (Guardrails), no se toca el repositorio del proyecto
ni el clon local de la persona, y no hay scripts (ADR 004).

## Decisión
`/sdd-version` es la única excepción de red: `git ls-remote` contra el origen de `.agents/.kit-origen`
y, si hace falta leer la versión y el changelog, un `git clone --depth 1` en un directorio de
`mktemp -d` fuera del proyecto, que se borra al terminar. Corre sin interacción
(`GIT_TERMINAL_PROMPT=0`) y para si el remoto pide credenciales. `push`, `pull`, `fetch` y `sync`
siguen prohibidos. La excepción se declara en una línea de `AGENTS.md` › Git.

## Alternativas descartadas
| Alternativa | Por qué no |
|---|---|
| Solo `git ls-remote` | Da la punta, pero no la versión ni el changelog |
| Sin red: pedir siempre el clon local | La persona puede no tener clon; no responde la pregunta |
| `git fetch` en el clon local | Toca el clon de la persona y abre la regla Git más de lo necesario |

## Consecuencias
La persona pregunta sin dar la URL. El clon temporal es descartable y no deja rastro en el proyecto.
Sin red o con credenciales, la consulta para y lo dice. Cualquier otro uso de red sigue prohibido y
exige un ADR nuevo.
