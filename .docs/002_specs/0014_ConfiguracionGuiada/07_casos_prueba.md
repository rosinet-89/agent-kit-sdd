# 07 — Casos de prueba: Configuración guiada del kit

Todos los casos corren en un temporal (`tmp=$(mktemp -d)`), nunca en un proyecto real. Herramienta de apoyo: «No aplica» (`AGENTS.md` › Comandos); los casos de shell los puede correr `tester`, los que requieren conversar con el asistente son a mano.

## Casos
### CP-01 Instalación copia los archivos nuevos sin pisar
- RF: criterios de finalización
- Datos de partida: temporal vacío.
- Destino: `local`
- Pasos:
  1. `cp -R --update=none template/. "$tmp"/`
  2. `ls "$tmp/.agents/configuracion.ejemplo.md" "$tmp/.agents/commands/sdd-config.md"`
- Resultado esperado: los dos archivos se listan; `cp` no da error.
- Herramienta: a mano
- Escribe: no

### CP-02 Saltar todo el asistente
- RF: RF-1, RF-4, RF-5; criterio 1
- Datos de partida: temporal de CP-01 con `git init` y un commit inicial; sesión de Claude Code abierta en él.
- Destino: `local`
- Pasos:
  1. Correr `/sdd-config` y saltar todo.
  2. `head -1 "$tmp/.agents/local/configuracion.md"`; `ls -la "$tmp/.claude/"`; `git -C "$tmp" status --porcelain`.
- Resultado esperado: la primera línea es `version: 0.10.0`; `.claude/agents` es enlace de carpeta; `git status` sin cambios atribuibles a la configuración; una línea final dice qué sugeridos se aplicaron.
- Herramienta: a mano
- Escribe: no

### CP-03 Modelo propio en Claude Code y en Cursor
- RF: RF-6, RF-13, RF-14, RF-15, RF-17; criterio 2
- Datos de partida: temporal de CP-02 con `.cursor/` creado.
- Destino: `local`
- Pasos:
  1. `/sdd-config`: en Claude Code, `implementer` en `opus`; en Cursor, aceptar los sugeridos.
  2. `/sdd-enlaces` y confirmar.
  3. `grep -n "^model:" "$tmp/.claude/agents/implementer.md" "$tmp/.cursor/agents/"*.md`; `git -C "$tmp" status --porcelain`.
- Resultado esperado: `.claude/agents/implementer.md` es adaptador con `model: opus`, los demás roles enlaces; `.cursor/agents/` por archivo con los modelos de la fila de Cursor; `git status` sin cambios en `.agents/agents/`.
- Herramienta: a mano
- Escribe: no

### CP-04 Enlaces por archivo resuelven
- RF: RF-14
- Datos de partida: temporal de F3.4.
- Destino: `local`
- Pasos:
  1. `for f in "$tmp/.claude/agents/"*.md; do [ -L "$f" ] && readlink -f "$f"; done`
- Resultado esperado: cada ruta termina en `/.agents/agents/<rol>.md` del mismo temporal.
- Herramienta: a mano
- Escribe: no

### CP-05 `.agents/agents/` no cambia
- RF: RF-14, RF-15
- Datos de partida: temporal de F3.4 o de CP-03.
- Destino: `local`
- Pasos:
  1. `diff -r template/.agents/agents "$tmp/.agents/agents"`
- Resultado esperado: salida vacía.
- Herramienta: a mano
- Escribe: no

### CP-06 Claude Code usa el adaptador
- RF: RF-14
- Datos de partida: temporal de CP-03.
- Destino: `local`
- Pasos:
  1. En el temporal, lanzar el subagente `implementer` con un encargo trivial de solo lectura y pedirle que declare su modelo.
- Resultado esperado: declara un id de la familia `opus`.
- Herramienta: a mano
- Escribe: no

### CP-07 Fuentes únicas sin copias
- RF: RF-5, RF-13, RF-16; RNF fuente única
- Datos de partida: clon tras F5.2.
- Destino: `local`
- Pasos:
  1. `grep -rln "3 pasadas" template`
  2. `grep -rn "model: inherit" template/.agents/agents`
- Resultado esperado: (1) solo `template/.agents/skills/sdd/topes-y-modos.md`; (2) vacío.
- Herramienta: a mano
- Escribe: no

### CP-08 Actualización conserva valores y avisa pasos nuevos
- RF: RF-2, RF-18; criterio 3
- Datos de partida: temporal de CP-03 con `version: 0.9.0` escrito a mano en la primera línea de su configuración y una sección de paso borrada.
- Destino: `local`
- Pasos:
  1. Abrir sesión nueva en el temporal (chequeo de arranque).
  2. Correr `/sdd-config`.
- Resultado esperado: el arranque avisa y sugiere `/sdd-config`; el asistente pregunta solo el paso borrado; `implementer` sigue en `opus`.
- Herramienta: a mano
- Escribe: no

## Corridas
| Caso | Fecha | Estado | Obtenido / nota |
|---|---|---|---|
| CP-01 | 2026-11-21 | ok | Los dos archivos se copian; `cp` sin error; `diff -rq` vacío |
| CP-04 | 2026-11-21 | ok | Cada enlace resuelve a `.agents/agents/<rol>.md` del temporal (F3.4) |
| CP-05 | 2026-11-21 | ok | `diff -r` vacío (F3.4) |
| CP-07 | 2026-11-21 | ok | (1) solo `topes-y-modos.md`; (2) vacío |
| CP-07 | 2026-11-21 | ok | Corrida de `tester` (claude-haiku-5-5) en validate: (1) solo `topes-y-modos.md`; (2) vacío |
| CP-02 | 2026-11-21 | ok | Ejecutor en temporal `/tmp/tmp.SOFSexdNiJ` (claude-sonnet-5-5) con respuestas fijas: `version: 0.10.0`; `.claude/agents` enlace de carpeta; `git status` vacío. Ok por juicio del reviewer (la línea final no tiene texto fijado) |
| CP-03 | 2026-11-21 | ok | `.claude/agents/implementer.md` adaptador `model: opus`, 5 enlaces que resuelven; `.cursor/agents/` 6 adaptadores (`Grok 4.7` alto, `Composer 2.5` medio y bajo); frontmatter igual salvo `model:`; `git status` vacío; `.agents/agents/` sin cambios. Ok por juicio |
| CP-08 | 2026-11-21 | ok | El arranque avisa; el asistente ofrece solo `## Presentación`; `implementer` sigue en `opus`. Ok por juicio; hallazgo: la comparación de títulos del punto 5 del chequeo da falso aviso (corrección en vuelta 1) |
| CP-06 | 2026-11-21 | manual | Requiere abrir Claude Code en el temporal y lanzar `implementer`; lo confirma la persona |
| CP-06 | 2026-11-21 | ok | Confirmado por la persona a las 15:34: en el temporal, `implementer` corrió en Opus con el adaptador `model: opus`; los demás roles, enlaces. La sesión del temporal, sin `.agents/local/configuracion.md`, lo leyó como discrepancia y sugirió `/sdd-enlaces` para volver al enlace (esperable sin configuración local) |

## Evidencia formal (opcional)
No aplica.
