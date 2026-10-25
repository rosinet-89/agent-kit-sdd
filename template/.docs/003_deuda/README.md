# Índice de deuda técnica

Control de lo que quedó pendiente en el sistema: por una decisión tomada durante un
requerimiento, por un hallazgo, o porque alguien la dio de alta a mano. Cada deuda tiene un
archivo `NNNN_<Nombre>.md` con el detalle completo (4 dígitos en un proyecto, `0001`; 3 en el
clon del kit). Este README es el resumen. El ancho está en `.docs/002_specs/README.md` ›
"Numeración única". Las carpetas de sección de `.docs/` siguen en 3 dígitos.

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
| Atendida por | <NNNN_Requerimiento, commit o ticket> (o "—") |

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
  aplicadas a los archivos `NNNN_<Nombre>.md` de esta carpeta (remota: `git ls-tree --name-only @{u} .docs/003_deuda/`).
  Ancho del prefijo: el de `.docs/002_specs/README.md` › "Numeración única" (4 dígitos en un proyecto, 3 en el clon del kit).
  Si se renumera una deuda, se actualizan también las specs que la citen. Numeración
  **independiente** de la de las specs. No se reutilizan números.
- Una fila por deuda, con su estado y su(s) referencia(s). El detalle vive en el archivo.
- Al pasar a `Resuelta`, llenar "Atendida por" y "Cómo se atendió", y anotar la fecha en el
  historial. La carpeta del plan que la resolvió también debe citar la deuda.
- Una deuda que nace de un plan se cita en el `PUNTO DE RETOMO` o en las decisiones de ese plan.
