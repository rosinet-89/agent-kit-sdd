---
description: SDD · Revisa 01_spec.md como un QA (solo detecta, no resuelve)
argument-hint: <prefijo_NombreFeature>
---

Fase `clarify`. Carpeta: `.docs/002_specs/$1/`.

Qué lee: `01_spec.md`, `.docs/001_constitucion/constitution.md` y las skills de dominio que los requisitos citen. No el resto del proyecto.

Qué hace:

1. Toma la hora (`date '+%Y-%m-%d %H:%M'`).
2. Delega en el subagente **reviewer** la revisión de `01_spec.md`. Debe listar, sin proponer soluciones:
   1. Ambigüedades restantes (RF que no se pueden verificar).
   2. Contradicciones entre requisitos.
   3. Casos límite no cubiertos.
   4. Conflictos con la constitución o con las skills de dominio.
   Formato: lista numerada.
3. Muestra la lista. Si se aprueban correcciones, delega en **planner** el ajuste de `01_spec.md` y muestra el contenido que cambió: el párrafo de antes y el de después. Si el archivo no tiene seguimiento en Git, o el proyecto no usa Git, ese contraste es el medio. No dependas de `git diff`.
4. PARA hasta que la spec se apruebe. Entonces cambia `Estado: aprobada` y anota la decisión con su hora.
5. La sesión principal registra la fila `clarify` según `.agents/skills/sdd/avance-y-tiempos.md`. Siguiente paso: `/sdd-plan $1`.

Qué no hace: no resuelve ambigüedades ni propone la solución. No depende de `git diff`. No copia el formato de la fila de tiempos. Si no hay un solo paso, aplica `.agents/skills/sdd/SKILL.md` › «Si no hay un solo paso» y no copies esa regla.
