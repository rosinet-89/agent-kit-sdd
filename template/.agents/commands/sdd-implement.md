---
description: SDD · Implementa UN paso de 04_tasks.md (uso - /sdd-implement 0017_NombreFeature F1.2)
argument-hint: <NNNN_NombreFeature> <Fn.m>
---

Implementa un solo paso. Carpeta: `.docs/002_specs/$1/`. Paso: `$2`.

Qué lee, y se lo pasa al implementer como lectura mínima: el paso `$2` en `04_tasks.md` con su "Hecho cuando"; los RF que ese paso cita; la parte de `03_plan.md` que cubre el paso; las secciones de `AGENTS.md` que el rol usa; de la constitución y de la skill `sdd`, solo las secciones que el paso o el plan citan. Un documento de menos de 4 096 bytes puede leerse entero.

Qué hace: si `04_tasks.md` no está aprobado, para. Delega en el subagente **implementer** solo ese paso. Si el paso no cita ningún RF, lee la fase `Fn` y avisa. Si el plan no tiene sección que cubra el paso, lee el plan completo y avisa. Ampliar solo dentro de esta lista, con tope de 3 secciones: (a) una sección del plan o de la spec nombrada por un RF o paso citado; (b) una decisión de `05_avance.md` que el paso cite; (c) una sección de `AGENTS.md` distinta de las que el rol ya usa. El motivo de cada ampliación se devuelve a la sesión principal, que lo anota. Muestra archivos, resultado de los comandos, `sql/` que el usuario debe ejecutar y verificaciones manuales de UI pendientes. Después PARA. No empieces el siguiente paso.

Qué no hace: no escribe `05_avance.md`. No copia la pausa ni el formato de la fila de tiempos: están en `.agents/skills/sdd/avance-y-tiempos.md`. No ejecuta SQL. No lee la spec, el plan ni las tareas enteros cuando basta la lectura mínima. Si no hay un solo paso, aplica `.agents/skills/sdd/SKILL.md` › «Si no hay un solo paso» y no copies esa regla.
