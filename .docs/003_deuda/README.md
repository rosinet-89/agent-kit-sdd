# Índice de deuda técnica

Control de lo que quedó pendiente en el sistema: por una decisión tomada durante un
requerimiento, por un hallazgo, o porque alguien la dio de alta a mano. Cada deuda tiene un
archivo `NNN_<Nombre>.md` con el detalle completo; este README es el resumen.

Sirve para saber **qué está pendiente, de dónde viene, si ya se atendió y qué requerimiento la
resolvió**.

## Cómo usarlo

1. **Al analizar** algo que toca un módulo, pantalla o tabla, busca aquí si hay deuda abierta
   relacionada (columna Módulo) y léela con el resto de antecedentes (ver "Antecedentes" en
   `AGENTS.md`). Si la deuda cae en el alcance del requerimiento, se anota en su
   `02_analisis_impacto.md` (sección 1.2) y se decide si se resuelve ahí o se deja.
2. **Al cerrar** un requerimiento, revisa qué deudas resolvió y cuáles dejó (`/sdd-cierre`).
3. **Si algo se difiere** durante el desarrollo (decisión del usuario o recorte de alcance), el
   agente propone dar de alta la deuda; se crea solo con el visto bueno del usuario.
4. **Alta manual:** copia el formato de la sección "Plantilla", usa el siguiente número y agrega
   su fila al índice.
5. Si el índice y el detalle discrepan, manda el detalle: corrige el índice.

## Estados

| Estado | Significa |
|---|---|
| `Abierta` | Pendiente, sin trabajo iniciado |
| `En atención` | Hay un requerimiento en curso que la resuelve (total o parcialmente) |
| `Resuelta` | Se atendió. Debe indicar el requerimiento o commit que la resolvió |
| `Descartada` | Se decidió no atenderla. Debe indicar el motivo y quién lo decidió |

Las deudas `Resuelta` y `Descartada` no se borran: se quedan como historial.

## Origen (tipo)

`Decisión de requerimiento` (se difirió a propósito) · `Hallazgo` (se encontró al analizar o
revisar) · `Alta manual` (la registró una persona).

## Índice

| NNN | Deuda | Módulo | Origen | Estado | Prioridad | Atendida por | Alta |
|---|---|---|---|---|---|---|---|
| 001 | [Actualizar el clon del kit (modo mantenimiento)](001_ActualizarCloneDelKit.md) | Instalación / actualización | Recorte de alcance en `001_ActualizarKit` | Resuelta | Media | `INSTALAR.md` › Modo mantenimiento (sesión paralela; no es `001_ActualizarKit`) | 2026-10-24 |
| 002 | [Versión 0.5.0 sin spec, plan ni avance](002_FlujoSddNoAplicadoEn050.md) | Proceso de mantenimiento | Hallazgo (flujo `sdd` no aplicado) | Resuelta | Media | `002_Kit050` | 2026-10-24 |
| 003 | [Modelo por rol y permisos del frontmatter de los agentes](003_ModeloYPermisosDeAgentes.md) | Agentes del kit | Recorte de alcance en `003_OptimizarArnes` | Resuelta | Media | `004_ModeloYPermisosAgentes` (cierre 2026-10-25 16:13) | 2026-10-25 |
| 004 | [Convivencia de la numeración de 3 y 4 dígitos en un proyecto que se actualiza](004_ConvivenciaNumeracion3y4Digitos.md) | Numeración de specs, deudas y ADR | Hallazgo al revisar 0.5.0 | Resuelta | Media | `005_ConvivenciaNumeracion` | 2026-10-25 |
| 005 | [Verificar el frontmatter de los agentes en Cursor, Gemini, Copilot y Kiro](005_FrontmatterAgentesPorHerramienta.md) | Agentes del kit (frontmatter) | Alta manual | Resuelta | Media | `007_DeudasAbiertas` | 2026-10-25 |
| 006 | [/sdd-init sugiere los fragmentos allow y deny](006_SddInitSugiereAllowDeny.md) | `sdd-init` | Recorte de alcance en `004_ModeloYPermisosAgentes` | Resuelta | Media | `007_DeudasAbiertas` | 2026-10-25 |
| 007 | [Nombres de archivo que chocan con otras herramientas](007_NombresQueChocanConHerramientas.md) | Nombres en la raíz del clon y en `template/` | Hallazgo (revisión pedida por el usuario) | Resuelta | Alta | `007_DeudasAbiertas` | 2026-10-25 |
| 008 | [Limitaciones de la numeración que dejó la 005](008_LimitacionesDeLaNumeracion.md) | Numeración de specs, deudas y ADR (E12 y aviso) | Recorte de alcance en `005_ConvivenciaNumeracion` | Resuelta | Baja | `007_DeudasAbiertas` | 2026-10-25 |

## Plantilla de un detalle

```markdown
# NNN — <Título>

| Dato | Valor |
|---|---|
| Estado | Abierta / En atención / Resuelta / Descartada |
| Alta | AAAA-MM-DD |
| Origen | <tipo> + referencia (carpeta del plan, decisión o ticket) |
| Módulo | <módulo o pantalla> |
| Prioridad | Alta / Media / Baja |
| Atendida por | <NNN_Requerimiento, commit o ticket> (o "—") |

## Descripción
## Por qué quedó pendiente
## Impacto si no se atiende
## Alcance (archivos, tablas)
## Cómo se atendería (propuesta)
## Cómo se atendió (al resolver: qué se hizo y dónde)
## Historial
- AAAA-MM-DD — Alta.
```

## Reglas de mantenimiento

- Número nuevo y revisión de duplicados: mismas reglas que `.docs/002_specs/README.md` › "Numeración única",
  aplicadas a los archivos `NNN_<Nombre>.md` de esta carpeta (remota: `git ls-tree --name-only @{u} .docs/003_deuda/`).
  Si se renumera una deuda, se actualizan también las specs que la citen. Numeración
  **independiente** de la de las specs. No se reutilizan números.
- Una fila por deuda, con su estado y su(s) referencia(s). El detalle vive en el archivo.
- Al pasar a `Resuelta`, llenar "Atendida por" y "Cómo se atendió", y anotar la fecha en el
  historial. La carpeta del plan que la resolvió también debe citar la deuda.
- Una deuda que nace de un plan se cita en el `PUNTO DE RETOMO` o en las decisiones de ese plan.
