---
description: SDD · Valida la estructura del kit - enlaces, réplicas, skills, agentes, comandos, índices y codificación (solo lee y reporta)
argument-hint: [enlaces|skills|agentes|comandos|docs|codificacion|kit]
---

La sesión principal valida la estructura. No lanza un subagente. Alcance: `$1` (vacío = todos los bloques).

Qué lee: `.agents/validar-estructura.md`. Para el bloque `enlaces`, también `.agents/sync-agents.md`.

Qué hace: ejecuta los chequeos E1 a E23 de ese archivo. Muestra solo lo que falla, en la tabla de "Cómo responder", y las correcciones propuestas sin aplicarlas. Si todo está bien, una línea. Para corregir enlaces y réplicas remite a `/sdd-enlaces`; las demás correcciones las pide la persona como acción aparte.

Qué no hace: no asigna E1–E23 al `reviewer` ni a ningún subagente. No redefine qué comprueba cada chequeo ni E20. No borra ni mueve nada sin visto bueno. No toca `05_avance.md`: este comando no es una fase del flujo.
