---
description: SDD · Revisa 01_spec.md como un QA (solo detecta, no resuelve)
argument-hint: <prefijo_NombreFeature>
---

Fase `clarify`. Carpeta: `.docs/002_specs/<carpeta>/`.
Argumentos: $ARGUMENTS. La carpeta es su primera palabra. Regla: `.agents/README.md` › «Argumentos de un comando».

Qué lee: `01_spec.md`, `.docs/001_constitucion/constitution.md` y las skills de dominio que los requisitos citen. No el resto del proyecto.

Qué hace:

1. Toma la hora (`date '+%Y-%m-%d %H:%M'`).
2. Antes de la pasada 1, el planner hace el autochequeo; desde la pasada 2, la revisión es por delta (`.agents/skills/sdd/topes-y-modos.md` › «Pasadas de clarify»). Ejecuta el rol **reviewer** (dónde corre: `.agents/skills/sdd/SKILL.md` › «Dónde corre cada rol») para la revisión de `01_spec.md`. Debe listar, sin proponer soluciones:
   1. Ambigüedades restantes (RF que no se pueden verificar).
   2. Contradicciones entre requisitos.
   3. Casos límite no cubiertos.
   4. Conflictos con la constitución o con las skills de dominio.
   Formato: lista numerada.
3. Muestra la lista. Las correcciones siguen `.agents/skills/sdd/topes-y-modos.md` › «Modo de corrección»: en «automático», el **planner** ajusta `01_spec.md` y la spec vuelve a clarify hasta el tope; en «con aprobación», se pregunta tras cada pasada y, si se aprueban, el planner las aplica. En ambos modos muestra el contenido que cambió: el párrafo de antes y el de después. Si el archivo no tiene seguimiento en Git, o el proyecto no usa Git, ese contraste es el medio. No dependas de `git diff`.
4. PARA hasta que la spec se apruebe. Aprobarla es siempre de la persona, en ambos modos. Entonces cambia `Estado: aprobada` y anota la decisión con su hora.
5. La sesión principal registra la fila `clarify` según `.agents/skills/sdd/avance-y-tiempos.md`: cuenta la pasada, anota «pasada n de N» (N según `.agents/skills/sdd/topes-y-modos.md`) y aplica el cierre de tope y la pasada única en ligero de `.agents/skills/sdd/topes-y-modos.md`. Siguiente paso: `/sdd-plan <carpeta>`; en ligero, `/sdd-tasks <carpeta>`.

Qué no hace: no resuelve ambigüedades ni propone la solución. No depende de `git diff`. No copia el formato de la fila de tiempos. Si no hay un solo paso, aplica `.agents/skills/sdd/SKILL.md` › «Si no hay un solo paso» y no copies esa regla.
