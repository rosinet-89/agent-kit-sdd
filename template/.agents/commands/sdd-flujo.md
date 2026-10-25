---
description: SDD · Genera 06_flujo.md, el flujo operativo de lo implementado
argument-hint: <prefijo_NombreFeature>
---

Fase `flujo`. Carpeta: `.docs/002_specs/$1/`.

Qué lee el planner: `01_spec.md`, `03_plan.md`, `05_avance.md` y el alcance de abajo. No el contexto entero del proyecto. La forma de `06_flujo.md` está en `.agents/skills/sdd/plantillas.md`, sección `## 06_flujo.md`.

Alcance, el de RF-19 y el mismo criterio que `.agents/commands/sdd-validate.md`: archivos citados en avance, plan y tareas, más los archivos nuevos sin seguimiento que liste `git status` (no `git diff`) y que estén en la carpeta del feature o citados. Todo otro archivo modificado o sin seguimiento se informa como "ajeno o no atribuible", sin revisarlo como propio. Con seguimiento se usa su diferencia; en los nuevos, su contenido. Si un archivo mezcla cambios del feature y ajenos, se avisa. Sin Git, el alcance es solo lo citado.

Qué hace:

1. Toma la hora (`date '+%Y-%m-%d %H:%M'`).
2. Delega en el subagente **planner** `06_flujo.md` con esa sección de `plantillas.md`. Describe lo que existe (acceso, pantallas o endpoints, roles y permisos, estados, notificaciones, errores frecuentes), no lo planeado.
3. Delega en el subagente **reviewer** el contraste de `06_flujo.md` con el código de ese alcance. Cada diferencia, con `archivo:línea`. Si las hay, el planner corrige.
4. Muestra el flujo. PARA hasta que se apruebe. Anota la aprobación con su hora.
5. La sesión principal registra la fila `flujo` según `.agents/skills/sdd/avance-y-tiempos.md`. Siguiente paso: `/sdd-cierre $1`.

Qué no hace: no describe lo planeado. No revisa como propio lo "ajeno o no atribuible". No copia la plantilla ni el formato de la fila de tiempos. Si no hay un solo paso, aplica `.agents/skills/sdd/SKILL.md` › «Si no hay un solo paso» y no copies esa regla. El orden de esta fase sigue siendo planner y luego reviewer.
