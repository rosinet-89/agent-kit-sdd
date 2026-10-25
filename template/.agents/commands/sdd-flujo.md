---
description: SDD · Genera 06_flujo.md, el flujo operativo de lo implementado
argument-hint: <NNNN_NombreFeature>
---
Fase `flujo` de la skill `sdd`. Carpeta: `.docs/002_specs/$1/`.

1. Toma la hora (`date '+%Y-%m-%d %H:%M'`).
2. Delega en el subagente **planner** `06_flujo.md` con la plantilla de la skill, a partir
   de `01_spec.md`, `03_plan.md`, `05_avance.md` y el `git diff` de lo implementado.
   Describe lo que existe (acceso, pantallas o endpoints, roles y permisos, estados,
   notificaciones, errores frecuentes), no lo planeado.
3. Delega en el subagente **reviewer** el contraste de `06_flujo.md` con el código; cada
   diferencia con `archivo:línea`. Si hay diferencias, el planner corrige.
4. Muéstrame el flujo. PARA hasta que lo apruebe; anota la aprobación con su hora.
5. Registra la fila `flujo` en `05_avance.md` y el siguiente paso: `/sdd-cierre $1`.
