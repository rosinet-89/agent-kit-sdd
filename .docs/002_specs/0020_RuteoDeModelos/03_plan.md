# 03 — Plan: Ruteo de modelos
Spec: 01_spec.md (aprobada 2026-11-22 16:34)

## 1. Ubicación y capas
Kit de solo Markdown (constitución, principios 1 y 5). Lo distribuible se edita en `template/` y en
los docs de la raíz; `.agents/` de la raíz se replica con `cp` al final de cada fase que toca
`template/.agents/`. Fuentes únicas:

- **Tabla y regla de ruteo** (RF-1, RF-3, RF-6, RF-9): sección nueva «Nivel por tarea sin rol» de
  `restricciones-de-rol.md`, debajo de «Modelo por rol». Allí viven la tabla, las cuatro condiciones
  de delegable, la regla de decisión del chat y el respaldo. Nadie la copia.
- **Excepción a «Solo roles del kit»** (RF-8) y estado y estructura como delegación fija (RF-4):
  `SKILL.md` › «Dónde corre cada rol», que sigue siendo la única fuente de dónde corre un rol.
- **Lectura de argumentos** (RF-13, RF-14): sección nueva «Argumentos de un comando» de
  `.agents/README.md`. Cada comando lleva una línea con su caso (qué es carpeta y qué es resto) y
  remite ahí para lo demás.
- **Preparación de commits** (RF-5): `git-commit/SKILL.md`.
- **Entrada siempre cargada**: un renglón en `AGENTS.md` (plantilla y raíz) que dice qué es, dónde
  está y cuándo se lee (criterio del ADR 0032), porque fuera de una fase el chat no carga la skill
  `sdd`.

Mecanismo de delegación fija en Claude Code: claves `context: fork` y `agent: auxiliar` en el
frontmatter del comando. El cuerpo del comando, con `$ARGUMENTS` ya sustituido, es el encargo del
subagente; el modelo sale del `model:` de `auxiliar` (y del adaptador si el dev lo cambió). En
OpenCode, `agent:` ya es clave de comando; F0 confirma si admite `context`.

## 2. Archivos que se crean o modifican
| Archivo | Capa | Responsabilidad | RF |
|---|---|---|---|
| `template/.agents/agents/auxiliar.md` (nuevo) | Rol | Rol de uso general de nivel Medio: `model: sonnet`, `tools: Read, Glob, Grep, Bash` (sin `Write`, `Edit`, `Agent` ni red). Encargo cerrado, devuelve resultado o pregunta, declara su modelo, nunca commit, no escribe `05_avance.md` ni la celda del índice, cumple Guardrails, rechaza lo que sea diseño (`FUERA DE ALCANCE`) | RF-7, RF-3 |
| `template/.agents/skills/sdd/restricciones-de-rol.md` | Fuente | Fila `auxiliar` / `sonnet` en «Modelo por rol». Sección «Nivel por tarea sin rol»: tabla (tarea, nivel, delegable, rol), condiciones de RF-3, regla de RF-6, respaldo de RF-9 y prohibición de `model:` sin subagente. Filas de la tabla de restricciones: `auxiliar` sin `Write`/`Edit`/red (por `tools`), sin `Agent`, sin commit (instrucción) | RF-1, RF-3, RF-6, RF-7, RF-9 |
| `template/.agents/skills/sdd/SKILL.md` | Skill | Tabla de fases: Estado y Estructura con «`auxiliar` (delegación fija)». «Dónde corre cada rol»: excepción de RF-8, delegación fija como motivo válido de lanzamiento, fuera de fase remite a «Nivel por tarea sin rol». «Si no hay un solo paso»: el estado y la estructura salen de la lista de la sesión principal | RF-2, RF-4, RF-8 |
| `template/.agents/commands/sdd-status.md` | Comando | Frontmatter `context: fork`, `agent: auxiliar`. Cuerpo: argumentos nuevos; si necesita preguntar (hueco largo), devuelve la pregunta | RF-4, RF-13, RF-14 |
| `template/.agents/commands/sdd-estructura.md` | Comando | Igual que status. Quita «No lanza un subagente» y «no asigna … a ningún subagente»; la bandera se escribe por shell tras `git check-ignore` | RF-4, RF-13, RF-14 |
| `template/.agents/commands/sdd-{spec,clarify,plan,tasks,implement,validate,flujo,cierre,change,pausa}.md` | Comandos | Línea de argumentos según «Argumentos de un comando»; `$1`, `$2` fuera; «Siguiente paso: `/sdd-x <carpeta>`» | RF-13, RF-14 |
| `template/.agents/README.md` | Layout | «Roles y comandos»: `auxiliar`. «Agregar un comando»: corrige la frase de `$1`, `$2`. Sección «Argumentos de un comando» (fuente única) | RF-7, RF-13, RF-14 |
| `template/.agents/skills/git-commit/SKILL.md` | Skill | Preparación (fases 1 a 3, stage y propuesta) delegada en `auxiliar` con encargo (requerimiento, carpeta, resumen); se detiene y devuelve la pregunta ante prefijo repetido, archivos ajenos en el stage o agrupación dudosa. Fase 4 (confirmación y `git commit`) en la sesión principal. Sin confirmación: informa el stage y propone sacarlo | RF-5 (retirado en F6.3, decisión 33: el archivo queda sin cambios) |
| `template/.agents/skills/formato-respuesta/SKILL.md` | Skill | Contexto nombra rol y modelo del subagente (`auxiliar (sonnet) → …`) | RF-10 |
| `template/.agents/skills/sdd/avance-y-tiempos.md` | Contrato | Motivo «ruteo» en la viñeta **Modelo** | RF-10 |
| `template/.agents/configuracion.ejemplo.md` | Configuración | Paso 1: `auxiliar` en el nivel medio | RF-11 |
| `template/.agents/commands/sdd-config.md` | Comando | El paso de modelos incluye los roles del ruteo (por remisión, sin copiar) | RF-11 |
| `template/.agents/sync-agents.md` | Enlaces | OpenCode: `auxiliar` con `mode: subagent`, `edit` denegado y `subagent` denegado | RF-4, RF-7 |
| `template/.agents/validar-estructura.md` | Validación | E9: ningún comando usa `$0`, `$1`, `$2`…; solo `$ARGUMENTS` | RF-13 |
| `template/.agents/validar-estructura.md` | Validación | «Alcance»: E1 a E26 los corre `/sdd-estructura` como delegación fija con `auxiliar`, nunca el `reviewer` (decisión 24) | RF-4 |
| `template/.agents/agents/coordinator.md` | Agente | «Qué hace»: `sdd-estructura.md` y `sdd-status.md` corren como delegación fija; `sdd-enlaces.md` sigue en la sesión (decisión 24) | RF-4 |
| `template/.agents/commands/sdd-tasks.md` | Comando | Paso 2: `/sdd-estructura` corre como delegación fija con `auxiliar` (decisión 24) | RF-4 |
| `template/AGENTS.md`, `AGENTS.md` (raíz) | Siempre cargado | Renglón en «Roles y comandos»: ruteo de tareas sin rol en `restricciones-de-rol.md` › «Nivel por tarea sin rol»; se lee antes de una tarea suelta que no sea diseño o decisión (la mención del commit se retiró en F6.3, decisión 33) | RF-6 |
| `README.md` (raíz) | Docs | Lista de roles y nota del ruteo | RF-7 |
| `ACTUALIZAR.md` (raíz) | Docs | «Qué hacer después de un pull…»: tras crear o cambiar un rol, esperar a que la herramienta lo cargue o abrir una sesión nueva; mientras tanto la delegación cae en `general-purpose` (decisiones 26 y 28) | criterio 9 |
| `.docs/004_adr/0037_DelegacionConSubagenteYNoSoloModelo.md`, índice de ADR | ADR | Delegación fija con subagente frente a `model:` por turno y cambio manual | criterio 7 |
| `VersionAgentKitSdd`, `ChangelogAgentKitSdd.md`, `manifiestos/0.15.0.md` | Versión | 0.15.0 y huella (E20) | criterio 9 |
| `.agents/` (raíz, local) | Réplica | `cp` desde `template/.agents/` y `diff -rq` | — |

## 3. Base de datos
No aplica: sin `sql/`.

## 4. Seguridad
- `auxiliar` no tiene `Write`, `Edit`, `Agent` ni herramientas de red: garantizado por `tools` en
  Claude Code; en el resto, instrucción. No lee conectores (decisión 13).
- Nunca `git commit`, `push`, `fetch`, `pull` ni `sync`: instrucción más el `deny` opcional de Git
  (como `implementer`).
- Guardrails de `AGENTS.md` sin cambio; `auxiliar` los lee siempre.
- La preparación del commit corre la búsqueda de secretos de `seguridad-basica` igual que hoy.

## 5. Liberación / piloto
Versión 0.15.0 en commits locales. Piloto en el propio clon del kit: `/sdd-enlaces` y la medición de
F6 con `/sdd-status` y la preparación de un commit reales.

## 6. Decisiones
| Decisión | Alternativa descartada | Por qué |
|---|---|---|
| Un solo rol nuevo `auxiliar` (Medio); lo Bajo de buscar o leer archivos va a `explore` | Rol Bajo nuevo con `Bash`; `auxiliar` con modelo por llamada | Persona, decisión 12: menos listas y adaptadores; la medición dice si hace falta un Bajo propio |
| Lectura de conectores no delegable, en el chat | `auxiliar` con conectores MCP | Persona, decisión 13: no se amplía «Ningún agente usa la red» |
| Salida larga de un comando: `auxiliar` (Medio) | Bajo, como dice RF-1 para logs | `explore` no tiene `Bash`; un log en archivo sí va a `explore` (Bajo). Se anota en la tabla |
| Delegación fija con `context: fork` y `agent:` (ADR 0037) | `model:` sin subagente; `/model` a mano | Spec (casos límite) y simulación: `model:` solo cuesta de 1.5 a 17 veces el chat; la persona descartó el cambio manual |
| Tabla y regla de ruteo en `restricciones-de-rol.md` | En `SKILL.md` | Va junto a «Modelo por rol» (RF-1) y la regla de RF-6 la usa con la tabla: un solo archivo que leer |
| Renglón en `AGENTS.md` | Solo en la skill `sdd` | Fuera de una fase el chat no carga la skill; ADR 0032 admite un renglón con qué, dónde y cuándo |
| Argumentos con `$ARGUMENTS` y «primera palabra = carpeta» | `$0`/`$1` según herramienta; `$ARGUMENTS[0]` | Única forma igual en Claude Code y OpenCode y legible si Cursor no sustituye |
| Delegación fija solo para `/sdd-status` y `/sdd-estructura` | También `/sdd-version` y `/sdd-enlaces` | `version` usa red (excepción del comando, no del rol); `enlaces` pide confirmación y el plan no lo separa (caso límite) |
| La medición antes de la versión | Versión y después medir | La tabla refleja la medición y el manifiesto se genera al final (trampa de `MEMORY.md`) |

## 7. Fases y estimado
Estimado en minutos, por ola de agentes (decisiones de `MEMORY.md` de 2026-11-22).

| Fase | Contenido | RF | Estimado (min) |
|---|---|---|---|
| F0 | Verificación en documentación de Claude Code (`context: fork` y `agent:` en `.claude/commands/`, sustitución de `$ARGUMENTS`). OpenCode y Cursor: prueba manual de la persona en esta versión (decisión 16, F5.4) | RF-4, RF-13 | 10 |
| F1 | Argumentos: «Argumentos de un comando» en `README.md` y los 12 comandos; E9 | RF-13, RF-14 | 15 |
| F2 | Rol y tabla: `auxiliar.md`, `restricciones-de-rol.md`, `AGENTS.md` (plantilla y raíz), `configuracion.ejemplo.md`, `sdd-config.md`, `sync-agents.md`, «Roles y comandos» | RF-1, RF-3, RF-6, RF-7, RF-9, RF-11 | 20 |
| F3 | Delegación fija y fases: `sdd-status.md`, `sdd-estructura.md`, `SKILL.md`, `formato-respuesta`, `avance-y-tiempos.md` | RF-2, RF-4, RF-8, RF-10 | 15 |
| F4 | Commits: `git-commit/SKILL.md` | RF-5 | 10 |
| F5 | Réplica y chequeos: `cp` a `.agents/` de la raíz, `diff -rq`, `/sdd-enlaces` (con confirmación), lint de `AGENTS.md` › Comandos, E8, E9 y E23 | — | 10 |
| F6 | Medición: `/sdd-status` y preparación de un commit, chat de nivel Alto frente a delegado; tokens, USD (precios oficiales con fecha) y tiempo; decisiones y ajuste de la tabla | RF-12 | 30 |
| F7 | ADR 0037, `README.md` de la raíz, versión 0.15.0, changelog, manifiesto y E20 | criterios 7 y 9 | 15 |
| **Total** | | | **125** |

F1 y F2 tocan archivos disjuntos y pueden ir en la misma ola; F3 depende de F2 (rol y tabla); F4
de F2; F5 de F1 a F4; F6 de F5; F7 de F6.

## 8. Criterios de aceptación
Los de `01_spec.md` › «Criterios de finalización», más:
- `grep -rn '\$[0-9]' template/.agents/commands/` vacío.
- `auxiliar` figura en «Modelo por rol», en «Roles y comandos» y en el adaptador de OpenCode; E8 y
  E23 sin `FALLA`.
- La tabla de ruteo aparece una sola vez (`grep` de su encabezado en `template/` da un archivo).
- `diff -rq template/.agents .agents` solo con las diferencias admitidas en `MEMORY.md`.
