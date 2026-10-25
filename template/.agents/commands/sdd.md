---
description: SDD · Punto de entrada único - el coordinator interpreta tu petición y decide la fase (uso - /sdd <lo que necesitas>)
argument-hint: <petición en lenguaje libre>
---

Punto de entrada. Elige un comando y lo sigue. No reescribe el procedimiento de ninguna fase.

Qué lee: la tabla "Cómo decides qué hacer" de `.agents/agents/coordinator.md`, y de `AGENTS.md` solo si contiene `SDD-INIT-PENDIENTE`. La skill `.agents/skills/sdd/SKILL.md` y el contrato `.agents/skills/sdd/avance-y-tiempos.md` se cargan solo si la fase elegida los usa. Para `status` o `estructura`, no se cargan.

Qué hace: con esa tabla elige UN comando y lo dice en una línea (carpeta nueva o existente, y hasta dónde llega). Si la petición es ambigua, pregunta antes de crear o escribir nada. Luego lee ese comando y lo sigue. Para en cada puerta de aprobación.

Qué no hace: no reescribe el procedimiento de la fase, no escribe código ni los documentos `01`–`04` y `06`, y no carga la skill ni el contrato para `status` o `estructura`.

Si `AGENTS.md` contiene `SDD-INIT-PENDIENTE`, para y sugiere `/sdd-init`.

Petición del usuario:

$ARGUMENTS

Comandos: `sdd-spec`, `sdd-clarify`, `sdd-plan`, `sdd-tasks`, `sdd-implement`, `sdd-validate`, `sdd-flujo`, `sdd-cierre`, `sdd-change`, `sdd-status`, `sdd-estructura`, `sdd-init`, `sdd-constitucion`.
