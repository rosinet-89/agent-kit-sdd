---
description: SDD · Implementa en cascada o UN paso de 04_tasks.md (uso - /sdd-implement 0017_NombreFeature [Fn | Fn.m])
argument-hint: <prefijo_NombreFeature> [Fn | Fn.m]
---

Implementa pasos de `04_tasks.md`. Carpeta: `.docs/002_specs/<carpeta>/`. Alcance: la segunda palabra.
Argumentos: $ARGUMENTS. La carpeta es su primera palabra y el alcance es la segunda. Regla: `.agents/README.md` › «Argumentos de un comando».

Formas:
- Sin alcance: cascada de todos los pasos pendientes.
- Alcance `Fn`: cascada de los pasos pendientes de esa fase.
- Alcance `Fn.m`: un solo paso, y PARA.
- En las tres, si `04_tasks.md` no está aprobado, para sin implementar nada.

Qué lee, y se lo pasa al implementer como lectura mínima: el paso en `04_tasks.md` con su "Hecho cuando"; los RF que ese paso cita; la parte de `03_plan.md` que cubre el paso; las secciones de `AGENTS.md` que el rol usa; de la constitución y de la skill `sdd`, solo las secciones que el paso o el plan citan. Un documento de menos de 4 096 bytes puede leerse entero. En un lote, es la misma regla sumada para sus pasos.

Un paso (`Fn.m`): ejecuta el rol **implementer** (dónde corre y el motivo del lanzamiento: `.agents/skills/sdd/SKILL.md` › «Dónde corre cada rol») solo para ese paso. Si el paso no cita ningún RF, lee la fase `Fn` y avisa. Si el plan no tiene sección que cubra el paso, lee el plan completo y avisa. Ampliar solo dentro de esta lista, con tope de 3 secciones: (a) una sección del plan o de la spec nombrada por un RF o paso citado; (b) una decisión de `05_avance.md` que el paso cite; (c) una sección de `AGENTS.md` distinta de las que el rol ya usa. El motivo de cada ampliación se devuelve a la sesión principal, que lo anota. Muestra archivos, resultado de los comandos, `sql/` que el usuario debe ejecutar y verificaciones manuales de UI pendientes. Después PARA. No empieces el siguiente paso.

Cascada (sin alcance o `Fn`):
1. Antes de arrancar, muestra una tabla con columnas Paso · Qué hago · Lote u ola · Se detiene por. «Se detiene por» lista solo lo ajeno (un `sql/` que ejecuta la persona, verificación manual, decisión o visto bueno, commit) o «—».
2. Lotes: agrupa hasta 4 pasos pendientes.
   - Un lote junta pasos de la misma fase, o de fases distintas que tocan el mismo archivo.
   - Ningún paso del lote depende de un paso pendiente fuera del lote.
   - Ninguno pide en su "Hecho cuando" verificación manual, evidencia de datos ni `sql/` (lint y pruebas sí).
   - El que no cumple va solo.
   - Un paso depende de otro si su texto lo nombra, o si toca un archivo que el otro también toca y va después en `04_tasks.md`.
3. Olas: los lotes sin archivos en común pueden lanzarse en paralelo.
4. Cada lote es un lanzamiento del implementer con la lectura mínima de sus pasos y el tope de 3 ampliaciones de arriba.
5. Al terminar cada lote, la sesión revisa su diff y el "Hecho cuando" de cada paso, y los marca uno por uno.
6. Si un paso falla su "Hecho cuando" o llega a algo ajeno, se detienen ese paso y los que dependen de él. Los independientes siguen. Al final se informa qué falta.
7. La cascada no hace commits ni ejecuta `sql/`.
8. La fila de tiempos de un lote: `.agents/skills/sdd/avance-y-tiempos.md` › «Mínimo y lote».
9. El tope de 4 es un umbral provisional: se revisa tras 3 usos reales.

La celda del índice de `002_specs` no la escribe el implementer: ver `.docs/002_specs/README.md` › «Leyenda de estatus» › «Quién escribe».

Qué no hace: no escribe `05_avance.md`. No copia la pausa ni el formato de la fila de tiempos: están en `.agents/skills/sdd/avance-y-tiempos.md`. No lee la spec, el plan ni las tareas enteros cuando basta la lectura mínima. Si no hay un solo paso, aplica `.agents/skills/sdd/SKILL.md` › «Si no hay un solo paso» y no copies esa regla. Sin excepciones: `AGENTS.md` › Guardrails.
