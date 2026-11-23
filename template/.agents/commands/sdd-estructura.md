---
description: SDD · Valida la estructura del kit - enlaces, réplicas, skills, agentes, comandos, índices y codificación (lee, reporta y deja la bandera local)
argument-hint: [enlaces|skills|agentes|comandos|docs|codificacion|kit]
context: fork
agent: auxiliar
---

Corre como subagente `auxiliar` (delegación fija, `restricciones-de-rol.md` › «Nivel por tarea sin rol») y valida la estructura. Alcance: la primera palabra de `$ARGUMENTS` (vacío = todos los bloques; regla en `.agents/README.md` › «Argumentos de un comando»: solo se lee `$ARGUMENTS`).

Qué lee: `.agents/validar-estructura.md`. Para el bloque `enlaces`, también `.agents/sync-agents.md`.

Qué hace: ejecuta los chequeos E1 a E26 de ese archivo. Muestra solo lo que falla, en la tabla de "Cómo responder", y las correcciones propuestas sin aplicarlas. Si todo está bien, una línea. Para corregir enlaces y réplicas remite a `/sdd-enlaces`; las demás correcciones las pide la persona como acción aparte.

Paso final: `auxiliar` no tiene Write ni Edit, así que escribe por shell la bandera local `.agents/local/estructura-ok`, solo tras comprobar con `git check-ignore` que está ignorado. Su formato y reglas están en `.agents/validar-estructura.md` › «Bandera de estructura». Es la única escritura de este comando.

Qué no hace: no asigna E1–E26 al `reviewer`. No redefine qué comprueba cada chequeo ni E20. No borra ni mueve nada sin visto bueno. No hace `git add` de la bandera, ni con `-f`. No toca `05_avance.md`: este comando no es una fase del flujo.
