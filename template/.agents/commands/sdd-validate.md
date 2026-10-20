---
description: SDD · Valida la spec RF por RF (consultas de solo lectura + comandos de verificación + verificación manual)
argument-hint: <NNN_NombreFeature>
---
Fase `validate` de la skill `sdd`. Carpeta: `.docs/002_specs/$1/`.

1. Toma la hora (`date '+%Y-%m-%d %H:%M'`).
2. Delega en el subagente **reviewer** la validación RF por RF de `01_spec.md`:
   - Comandos de lint, sintaxis y prueba de `AGENTS.md` › Comandos en cada archivo tocado (`git diff`).
   - Evidencia por RF con consultas de solo lectura o salida de pruebas.
   - Lista de verificaciones manuales de UI para mí, con qué abrir, qué hacer y qué debe verse.
3. NO arregles nada todavía. Muéstrame el VEREDICTO y la lista de verificaciones manuales.
4. Cuando confirme cada verificación manual, anótala con su hora en la sección Pruebas
   de `05_avance.md`. Sin eso, el RF queda pendiente.
5. Si pido evidencia formal, el reviewer genera `07_pruebas_qa.md`.
6. Registra la fila `validate` en `05_avance.md` y el siguiente paso: `/sdd-flujo $1`
   (o la corrección que toque).
