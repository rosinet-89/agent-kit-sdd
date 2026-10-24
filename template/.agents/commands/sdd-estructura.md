---
description: SDD · Valida la estructura del kit - enlaces, réplicas, skills, agentes, comandos, índices y codificación (solo lee y reporta)
argument-hint: [enlaces|skills|agentes|comandos|docs|codificacion|kit]
---
Lee `.agents/validar-estructura.md` y sigue sus chequeos. Alcance: `$1` (vacío = todos los bloques).

1. Delega en el subagente **reviewer** la revisión; no modifiques ningún archivo.
2. Aplica `.agents/sync-agents.md` (estados de enlaces y paso 8, réplicas) para los bloques `enlaces`.
3. Muéstrame solo lo que falla, en la tabla de "Cómo responder" de `validar-estructura.md`, y las
   **Correcciones propuestas** sin aplicar. Si todo está bien, una línea.
4. Cuando confirme una corrección, aplícala una por una. Nunca borres ni muevas nada sin mi visto bueno.
5. Si existe `05_avance.md` del feature en curso, no lo toques: este comando no es una fase del flujo.
