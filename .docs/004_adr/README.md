# Índice de decisiones de arquitectura (ADR)

Un archivo `NNN_<Titulo>.md` por decisión (en este clon, la numeración remite a la declaración de `template/.docs/002_specs/README.md` › « Numeración única »), creado con la skill `adr`
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
| 013 | [model inherit en los cuatro roles](013_ModeloExplicitoPorRol.md) | Reemplazado por 0028 | 2026-10-25 | `004_ModeloYPermisosAgentes` |
| 014 | [Un ítem nuevo conserva el ancho de su carpeta](014_ConvivenciaDeAnchosDeNumeracion.md) | Aceptado | 2026-10-25 | `005_ConvivenciaNumeracion` |
| 015 | [La fuente única de la numeración vive en el grupo C](015_FuenteUnicaDeNumeracionEnGrupoC.md) | Aceptado | 2026-10-25 | `005_ConvivenciaNumeracion` |
| 0016 | [Un ítem nuevo del clon se escribe con 4 dígitos](0016_ItemNuevoDelClonEnCuatroDigitos.md) | Aceptado | 2026-10-25 | `007_DeudasAbiertas` |
| 0017 | [Un prefijo numérico cuenta como entero](0017_PrefijoNumericoCuentaComoEntero.md) | Aceptado | 2026-10-25 | `007_DeudasAbiertas` |
| 0018 | [Solo tres nombres salen del patrón de un plugin de release](0018_TresNombresFueraDelPatronDeRelease.md) | Aceptado | 2026-10-25 | `007_DeudasAbiertas` |
| 0019 | [Regla global de la base de datos solo lectura en AGENTS.md › Guardrails](0019_ReglaGlobalBdSoloLectura.md) | Aceptado | 2026-11-01 | `0008_GuardrailsBdSoloLectura` |
| 0020 | [El marcador de instalación pendiente no lleva el literal fuera de su comentario](0020_MarcadorSinLiteralFueraDelComentario.md) | Aceptado | 2026-11-01 | `0010_FalsoPositivoInitPendiente` |
| 0021 | [Estado local `kit-visto` y aviso al arranque; solo `/sdd-enlaces` escribe](0021_EstadoLocalKitVistoYAvisoAlArranque.md) | Aceptado | 2026-11-02 | `0009_EnlacesTrasPull` |
| 0022 | [El arranque ya no detecta réplicas ni ignorados versionados](0022_ArranqueSinDeteccionDeReplicasNiIgnoradosVersionados.md) | Aceptado | 2026-11-02 | `0009_EnlacesTrasPull` |
| 0023 | [Ceremonia proporcional: modo ligero, tope de pasadas y marcador único](0023_CeremoniaProporcional.md) | Aceptado | 2026-11-03 | `0011_ReducirCeremoniaSdd` |
| 0024 | [La leyenda de estatus vive en el índice](0024_LeyendaDeEstatusEnElIndice.md) | Aceptado | 2026-11-16 | `0012_EstatusDeSpecYCierre` |
| 0025 | [Aviso de estatus tras E21](0025_AvisoDeEstatusTrasE21.md) | Aceptado | 2026-11-16 | `0012_EstatusDeSpecYCierre` |
| 0026 | [Antecedentes remite a la leyenda](0026_AntecedentesRemitenALaLeyenda.md) | Aceptado | 2026-11-16 | `0012_EstatusDeSpecYCierre` |
| 0027 | [La leyenda se copia en el índice del clon](0027_LeyendaCopiadaEnElIndiceDelClon.md) | Aceptado | 2026-11-16 | `0012_EstatusDeSpecYCierre` |
| 0028 | [Modelo por rol, rol en el chat salvo cambio de modelo y excepción de QA](0028_ModeloPorRolYRolEnElChat.md) | Aceptado | 2026-11-21 | `0013_ModelosPorAgente` |
| 0029 | [Configuración del flujo por dev, no versionada, con sugerencias en un ejemplo](0029_ConfiguracionPorDevNoVersionada.md) | Aceptado | 2026-11-21 | `0014_ConfiguracionGuiada` |
| 0030 | [Carpeta de agentes por archivo con adaptadores de rol](0030_CarpetaDeAgentesPorArchivo.md) | Aceptado | 2026-11-21 | `0014_ConfiguracionGuiada` |
| 0031 | [El cierre decide el criterio 2 con sus commits en una corrida](0031_CierreConSusCommitsEnUnaCorrida.md) | Aceptado | 2026-11-21 | `0016_CierreEnUnaCorrida` |
| 0032 | [Qué va en el archivo siempre cargado y qué se lee bajo demanda](0032_CriterioDeArchivoSiempreCargado.md) | Aceptado | 2026-11-21 | `0015_ArranqueLigero` |
| 0033 | [«Antecedentes» y «Deuda técnica» se leen bajo demanda desde la skill `sdd`](0033_AntecedentesYDeudaBajoDemanda.md) | Aceptado | 2026-11-22 | `0017_PausaOrigenYContexto` |
| 0034 | [Única excepción de red de la regla Git: consultar la versión publicada del kit](0034_ExcepcionDeRedParaConsultarVersion.md) | Aceptado | 2026-11-22 | `0017_PausaOrigenYContexto` |
| 0035 | [Sección `.docs/005_contexto/` para el contexto del proyecto bajo demanda](0035_SeccionDeContextoDelProyecto.md) | Aceptado | 2026-11-22 | `0017_PausaOrigenYContexto` |
| 0036 | [`/sdd-implement` sin paso implementa en cascada, con lotes de hasta 4 pasos](0036_CascadaConLotesEnImplement.md) | Aceptado | 2026-11-22 | `0019_FlujoEficiente` |
| 0037 | [Las tareas sin rol cambian de modelo con un subagente, no solo con `model:`](0037_DelegacionConSubagenteYNoSoloModelo.md) | Aceptado | 2026-11-22 | `0020_RuteoDeModelos` |
