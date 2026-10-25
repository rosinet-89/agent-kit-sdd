---
description: SDD · Entrevista y genera 01_spec.md (uso - /sdd-spec 0017_NombreFeature idea inicial)
argument-hint: <NNNN_NombreFeature | NombreFeature> <idea inicial>
---

Fase `spec`. Carpeta: `.docs/002_specs/$1/`. Idea inicial: el resto de `$ARGUMENTS` después de `$1`.

Qué lee: la sesión principal lee una sola vez `.docs/001_constitucion/constitution.md` y `MEMORY.md`, y los pasa en la delegación. El planner no los relee. Si necesita algo que no se le entregó, lo lee de forma acotada. También `.docs/002_specs/README.md` y, si la idea toca un proceso ya trabajado, solo esa carpeta. La forma de `01_spec.md` y de `05_avance.md` está en `.agents/skills/sdd/plantillas.md`. La fila de tiempos, en `.agents/skills/sdd/avance-y-tiempos.md`.

Qué hace:

1. Si la idea toca un proceso ya trabajado, di qué carpeta coincide y léela antes de seguir. Al crear la carpeta, agrega su fila al README con estatus `Análisis`.
2. Si `$1` no trae el prefijo numérico, calcula el siguiente libre con la regla "Numeración única" de `.docs/002_specs/README.md` de esta copia (local y rama remota). En un proyecto el ancho es de 4 dígitos (`0001`). Si esta copia es el clon del kit, manda el ancho de ese README. El mismo ancho vale para deudas y ADR. Confírmalo. Si el prefijo ya existe, avisa y propón el siguiente libre. Al terminar, recomienda subir pronto la carpeta para reservar el número.
3. Toma la hora (`date '+%Y-%m-%d %H:%M'`). Crea la carpeta y `05_avance.md` con la plantilla de `plantillas.md`.
4. Delega en el subagente **planner** `01_spec.md`: plantilla de `plantillas.md`, RF en EARS, `Estado: borrador`, solo el QUÉ y el POR QUÉ. Pásale constitución y memoria ya leídas e indícale que no las relea. Si devuelve preguntas, házmelas de UNA en UNA (máximo 5) y vuelve a llamarle con mis respuestas.
5. La sesión principal registra la fila `spec` según el contrato de tiempos, y el siguiente paso: `/sdd-clarify $1`.

Qué no hace: no escribe código. No reescribe el contrato de avance y tiempos. No hace que el planner relea constitución ni memoria. Si no hay un solo paso, aplica `.agents/skills/sdd/SKILL.md` › «Si no hay un solo paso» y no copies esa regla.
