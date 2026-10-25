# Índice de decisiones de arquitectura (ADR)

Un archivo `NNN_<Titulo>.md` por decisión (en este clon, 3 dígitos; no subirlos a 4), creado con la skill `adr`
(`.agents/skills/adr/SKILL.md`). Este índice es el resumen: búscalo antes de decidir algo que
ya pudo decidirse.

## Cómo usarlo

1. Antes de proponer una decisión técnica, busca aquí el tema.
2. Un ADR no se borra ni se reescribe: se reemplaza por otro o se descarta con motivo.
3. El prefijo `NNN_` no se repite (mismo criterio que `.docs/002_specs/README.md` › "Numeración única").
   En un proyecto el archivo es `<prefijo>_<Titulo>.md`, con el ancho de `template/.docs/002_specs/README.md` › « Numeración única ». La carpeta `004_adr` sigue en 3.
4. Si el índice y el archivo discrepan, manda el archivo: corrige el índice.

Estados: `Propuesto` · `Aceptado` · `Reemplazado por NNN` · `Descartado`.

## Índice

| NNN | Decisión | Estado | Fecha | Origen |
|---|---|---|---|---|
| 001 | [Entorno de mantenimiento versionado en la raíz del kit](001_ModoMantenimientoDelKit.md) | Aceptado | 2026-10-24 | Conversación de mantenimiento, 0.5.0 |
| 002 | [`template/` es el producto; la raíz no se distribuye](002_TemplateEsElProducto.md) | Aceptado | 2026-10-24 | MEMORY.md (2026-10-24) |
| 003 | [`AGENTS.md` es la fuente de verdad; `CLAUDE.md` solo lo importa](003_AgentsMdFuenteDeVerdad.md) | Aceptado | 2026-10-24 | Commit 861bb99 |
| 004 | [El kit es solo Markdown, sin scripts de instalación](004_SoloMarkdownSinScripts.md) | Aceptado | 2026-10-24 | CHANGELOG 0.4.0 |
| 005 | [Skills de equipo de 0.5.0 y las que se descartan o difieren](005_SkillsDeEquipo050.md) | Aceptado | 2026-10-24 | SKILLS-SUGERIDAS.md (análisis de 11 skills) |
| 006 | [Graphify no entra al kit](006_GraphifyNoEntra.md) | Aceptado | 2026-10-24 | Evaluación de https://github.com/Graphify-Labs/graphify |
| 007 | [La actualización reemplaza solo archivos KIT-VIEJO, con confirmación](007_ActualizarSoloKitViejo.md) | Aceptado | 2026-10-24 | `001_ActualizarKit` (clarify) |
| 008 | [Formato de manifiestos, huella y marcador](008_FormatoManifiestosHuellaMarcador.md) | Aceptado | 2026-10-25 | `001_ActualizarKit` |
| 009 | [Una fuente por fase del flujo SDD](009_FuenteUnicaPorFase.md) | Aceptado | 2026-10-25 | `003_OptimizarArnes` |
| 010 | [La ruta alterna vive en la skill sdd](010_RutaSiNoHayUnSoloPaso.md) | Aceptado | 2026-10-25 | `006_RutaSiNoDelega` |
| 011 | [Restricciones de rol sin hooks](011_RestriccionesDeRolSinHooks.md) | Aceptado | 2026-10-25 | `004_ModeloYPermisosAgentes` |
| 012 | [omitClaudeMd en planner, reviewer y coordinator](012_OmitClaudeMdEnTresRoles.md) | Aceptado | 2026-10-25 | `004_ModeloYPermisosAgentes` |
| 013 | [model inherit en los cuatro roles](013_ModeloExplicitoPorRol.md) | Aceptado | 2026-10-25 | `004_ModeloYPermisosAgentes` |
| 014 | [Un ítem nuevo conserva el ancho de su carpeta](014_ConvivenciaDeAnchosDeNumeracion.md) | Aceptado | 2026-10-25 | `005_ConvivenciaNumeracion` |
| 015 | [La fuente única de la numeración vive en el grupo C](015_FuenteUnicaDeNumeracionEnGrupoC.md) | Aceptado | 2026-10-25 | `005_ConvivenciaNumeracion` |
