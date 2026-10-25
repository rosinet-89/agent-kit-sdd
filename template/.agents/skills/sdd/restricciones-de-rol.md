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

`model`, `omitClaudeMd`, `mode` y `permissions`: no verificado en Cursor, Gemini y Copilot.
Quitar `mode` y `permissions` puede quitarles una función que aquí no se conoce.

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
| `reviewer` sin `Write` ni `Edit`. Que no modifique archivos por Bash (`sed -i`, redirecciones, `tee`) | `tools`. Instrucción de texto del agente | GARANTIZADA por `tools`. NO garantizada, instrucción de texto del agente |
| `planner` escribe solo los cinco documentos de su carpeta de feature | instrucción (tiene `Write`/`Edit`). Sin deny de ruta | NO garantizada (tiene `Write`/`Edit`); instrucción. Sin deny de ruta |
| `coordinator` escribe solo `05_avance.md` y usa el shell solo para la hora | instrucción. Sin deny de ruta | NO garantizada; instrucción. Sin deny de ruta |
| `implementer` no hace `git commit`, `push`, `fetch`, `pull` ni `sync`, y no ejecuta SQL | instrucción más el `deny` opcional de Git (`disallowedTools` no sirve) | NO garantizada; instrucción más el `deny` opcional de Git (`disallowedTools` no sirve) |
| `planner`, `reviewer` y `coordinator`, que conservan `Bash`, tampoco registran en Git | instrucción más el `deny` opcional de Git | NO garantizada; instrucción más el `deny` opcional de Git |
| Ningún subagente lanza otros subagentes salvo el `coordinator` a los tres roles | `planner`, `implementer` y `reviewer` no listan `Agent`. La lista `Agent(planner, implementer, reviewer)` del `coordinator` solo restringe cuando corre como hilo principal; como subagente se ignora y podría lanzar subagentes hasta 3 capas | GARANTIZADA para `planner`, `implementer` y `reviewer` (no listan `Agent`). Para el `coordinator`, NO garantizada en ese caso |
| Ningún agente usa la red (`webfetch` y búsqueda web denegados) | `tools`, que no lista esas herramientas | GARANTIZADA por `tools`, que no lista esas herramientas |
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
