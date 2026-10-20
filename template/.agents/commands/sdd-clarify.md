---
description: SDD · Revisa 01_spec.md como un QA (solo detecta, no resuelve)
argument-hint: <NNN_NombreFeature>
---
Fase `clarify` de la skill `sdd`. Carpeta: `.docs/002_specs/$1/`.

1. Toma la hora (`date '+%Y-%m-%d %H:%M'`).
2. Delega en el subagente **reviewer** la revisión de `01_spec.md`. Debe listar:
   1. Ambigüedades restantes (RF que no se pueden verificar).
   2. Contradicciones entre requisitos.
   3. Casos límite no cubiertos.
   4. Conflictos con `.docs/001_constitucion/constitution.md` o con las skills de dominio.
   Solo detecta, no propone soluciones. Formato: lista numerada.
3. Muéstrame la lista. Si apruebo correcciones, delega en **planner** el ajuste de
   `01_spec.md` y enséñame el diff.
4. PARA hasta que yo apruebe la spec; entonces cambia `Estado: aprobada` y anota la
   decisión con su hora.
5. Registra la fila `clarify` en `05_avance.md` y el siguiente paso: `/sdd-plan $1`.
