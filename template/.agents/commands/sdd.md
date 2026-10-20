---
description: SDD · Punto de entrada único - el coordinator interpreta tu petición y decide la fase (uso - /sdd <lo que necesitas>)
argument-hint: <petición en lenguaje libre>
---
Actúa como el coordinator de este proyecto: lee y sigue `.agents/agents/coordinator.md`
(sus fases, reglas, la tabla "Cómo decides qué hacer" y lo único que puede escribir), con la
skill `.agents/skills/sdd/SKILL.md` y `.agents/skills/sdd/avance-y-tiempos.md`.

Si `AGENTS.md` contiene `SDD-INIT-PENDIENTE`, para y sugiere `/sdd-init`.

Petición del usuario:

$ARGUMENTS

1. Con la tabla "Cómo decides qué hacer" de `coordinator.md`, elige UN comando de
   `.agents/commands/` (`sdd-spec`, `sdd-clarify`, `sdd-plan`, `sdd-tasks`,
   `sdd-implement`, `sdd-validate`, `sdd-flujo`, `sdd-cierre`, `sdd-change` o
   `sdd-status`).
2. Dime en UNA línea qué comando elegiste y por qué (carpeta nueva o existente, y hasta
   dónde llegas). Si la petición es ambigua, pregúntame antes de crear o escribir nada.
3. Lee ese comando y síguelo. Él dice a quién delegar (planner, implementer o reviewer).
   No reescribas su procedimiento ni escribas código ni los documentos `01`–`04` y `06`.
4. Respeta el alcance que yo indique (por ejemplo "solo análisis", "hasta las tareas").
   Para en cada puerta de aprobación. El comando registra la fase en `05_avance.md`.
