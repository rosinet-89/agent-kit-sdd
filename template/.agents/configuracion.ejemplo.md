# Configuración guiada (ejemplo)

Este archivo es un ejemplo con los valores sugeridos del kit. Cópialo a
`.agents/local/configuracion.md` y edítalo ahí, o deja que `/sdd-config` lo haga paso a paso. Esa
copia es de cada persona y no se versiona (`.agents/local/` está fuera de Git). Este ejemplo sí viaja
con el kit. Un valor que ya tiene fuente en el kit se cita aquí y no se copia.

## Formato del archivo local

- Primera línea: `version: X.Y.Z`, la versión del kit con que se configuró. La lee el chequeo de
  arranque (`.agents/sync-agents.md` › «Chequeo de arranque»), que solo lee esa línea.
- Luego una sección `##` por paso, con estos nombres fijos: `## Modelos`, `## Subagentes`,
  `## Clarify`, `## Entornos`, `## Presentación`. Esta viñeta es la lista que usan el chequeo de
  arranque y `/sdd-config` para saber qué pasos faltan; un paso nuevo del kit se agrega a ella.
- Un paso cuya sección falta toma su valor sugerido. Un valor inválido también, con un aviso.
- Sin credenciales: ni valores ni nombres de secretos, salvo el nombre de variable de entorno del
  auditor.

## Paso 1. Modelos por rol

Contexto: cada rol corre con un modelo según su nivel (alto: planner, coordinator, reviewer; medio:
implementer; bajo: tester, explore). Los modelos dependen de la herramienta de cada persona.

Sugerido: en Claude Code, los de `.agents/skills/sdd/restricciones-de-rol.md` › «Modelo por rol»
(no se copian aquí). En otra herramienta, su fila de la tabla de abajo; sin fila, `inherit`.

### Tabla de equivalencias

Fuente: equivalencias reunidas por el mantenedor del kit el 2026-11-21 (kit 0.10.0); los nombres de
modelos envejecen, actualízalos en tu copia local si hace falta. Los niveles son los de arriba: la columna
«Alto» corresponde a la sesión principal, «Medio» al implementer y «Bajo» al tester. En una celda con
«o», el primero es el valor sugerido y el segundo, la alternativa. La fila de Claude Code no se
copia. El esfuerzo (effort o reasoning) es solo una sugerencia: el asistente no lo escribe en ningún
archivo.

| Herramienta | Alto | Medio | Bajo | Esfuerzo sugerido |
|---|---|---|---|---|
| Cursor | `Grok 4.7` | `Composer 2.5` o `Grok 4.7 (Fast)` | `Composer 2.5` o `Grok 4.6` | Alto: high (Fast, 512k); medio: standard |
| Antigravity (*) | `Gemini 3.1 Pro` | `Gemini 3.5 Flash` o `Gemini 3.1 Pro` | `Gemini 3.5 Flash` | Alto: High Reasoning; medio: agéntico |
| Codex (*) | `gpt-5.3-codex` | `gpt-5.3-codex` | `gpt-5.3-codex-mini` | Alto: high; medio: medium; bajo: low o medium |
| OpenCode | `opencode/beam-free` o `opencode/minimax-m3.1-flash-free` | `opencode/big-pickle` o `opencode/deepseek-v4.1-flash-free` | `opencode/nemotron-3.5-lightning-free` o `opencode/deepseek-v4.1-flash-free` | Alto: xhigh o max (beam-free), high (minimax) |

(*) Solo sugerencia: sin carpeta de agentes en el kit.

## Paso 2. Roles que corren siempre como subagente

Contexto: por omisión un rol corre en la sesión salvo que la persona pida el subagente. Aquí se
pide una vez.

Sugerido: ninguno. Rige `.agents/skills/sdd/SKILL.md` › «Dónde corre cada rol».

## Paso 3. Clarify

Contexto: el tope de pasadas de `/sdd-clarify` y si las correcciones se aplican solas o con tu
visto bueno.

Sugerido: el tope de `.agents/skills/sdd/topes-y-modos.md` (no se copia el número) y modo de
corrección «automático» (la otra opción es «con aprobación»; ver `topes-y-modos.md` › «Modo de
corrección»).

## Paso 4. Entornos de QA

Contexto: los destinos donde `tester` puede ejecutar casos que escriben datos. Sin destinos rige el
modo seguro.

Sugerido: modo seguro, sin destinos. El valor real vive en `.agents/local/entornos.md`; la sección
`## Entornos` solo remite. Formato y «Advertencia (S-8)» en `.agents/entornos.ejemplo.md`.

## Paso 5. Presentación de las respuestas

Contexto: cómo se ve cada respuesta del asistente: secciones, preguntas y tabla de cascada.

Sugerido: secciones con ícono fijo (✅ ⚠️ ⏳ 📋 ❓), preguntas una por una, y tabla de cascada al
iniciar o retomar una fase.

## Auditor externo (beta)

Apagado por omisión. Solo se ofrece si la persona lo pide o si se detectan dos herramientas de
agente. Combinación probada: Claude Code (sesión principal) con Cursor (auditor), en Linux. Las demás
combinaciones: no probada. La regla de lanzamiento está en `.agents/skills/sdd/SKILL.md` ›
«Auditor externo (beta)»; aquí no se repite.

Formato de `.agents/local/auditor.md` (no se versiona; no existe mientras el auditor esté apagado):

- `herramienta`: la que audita.
- `modelo`: el modelo del auditor.
- `comando`: cómo se lanza.
- `puntos`: de esta lista cerrada, `clarify`, `plan`, `tasks`, `validate`, `cierre`. Sugerido al
  activarlo: `validate`.

Sin credenciales: si el comando necesita una, el archivo lleva solo el nombre de la variable de
entorno, nunca su valor.
