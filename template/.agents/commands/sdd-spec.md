---
description: SDD · Entrevista y genera 01_spec.md (uso - /sdd-spec 0017_NombreFeature idea inicial)
argument-hint: <NNNN_NombreFeature | NombreFeature> <idea inicial>
---
Fase `spec` de la skill `sdd`. NO escribas código en ningún momento.

Lee `.agents/skills/sdd/SKILL.md`, `.agents/skills/sdd/avance-y-tiempos.md`,
`.docs/001_constitucion/constitution.md`, `MEMORY.md` y `.docs/002_specs/README.md`
(si la idea toca un proceso ya trabajado, dime qué carpeta coincide y léela antes de seguir;
al crear la carpeta, agrega su fila al README con estatus `Análisis`).

- Carpeta: `.docs/002_specs/$1/`. Si `$1` no trae el prefijo numérico, calcula el siguiente
  libre con la regla "Numeración única" de `.docs/002_specs/README.md` (copia local y rama remota):
  4 dígitos en un proyecto (`0001`), 3 en el clon del kit. El mismo ancho vale para deudas y ADR.
  Confírmalo conmigo. Si `$1` trae un prefijo que ya existe, avísame y propón el siguiente libre.
  Al terminar, recomiéndame subir pronto la carpeta para reservar el número.
- Idea inicial: el resto de `$ARGUMENTS` después de `$1`.

1. Toma la hora (`date '+%Y-%m-%d %H:%M'`). Crea la carpeta y `05_avance.md` con la
   plantilla (PUNTO DE RETOMO y tabla de tiempos).
2. Delega en el subagente **planner** la redacción de `01_spec.md`. Si devuelve preguntas,
   házmelas de UNA en UNA (máximo 5) y vuelve a llamarle con mis respuestas.
3. `01_spec.md` con la plantilla, RF en EARS y `Estado: borrador`. Solo el QUÉ y el POR QUÉ.
4. Registra en `05_avance.md` la fila `spec` (inicio, fin, minutos, pausa) y el siguiente
   paso: `/sdd-clarify $1`.
