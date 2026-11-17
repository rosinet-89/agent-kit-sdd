# Índice de deuda técnica

Control de lo que quedó pendiente en el sistema: por una decisión tomada durante un
requerimiento, por un hallazgo, o porque alguien la dio de alta a mano. Cada deuda tiene un
archivo `<prefijo>_<Nombre>.md` con el detalle completo. Este README es el resumen. El ancho del
prefijo y la regla del siguiente libre están en `.docs/002_specs/README.md` › "Numeración única".
Las carpetas de sección de `.docs/` siguen en 3 dígitos.

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

Deuda cerrada = `Resuelta` o `Descartada`, palabra exacta, salvo `Resuelta` sin «Cómo se atendió», que es deuda pendiente sin clasificar. Deuda pendiente = `Abierta`, `En atención` u otra palabra. Esa otra es deuda pendiente sin clasificar: el chequeo avisa y, si la columna Atendida por es este requerimiento, bloquea igual que `En atención`.

Atendida por es la columna del índice de deuda. La referencia de origen es el Origen del detalle (tipo + referencia). La columna Origen del índice es solo el tipo. Si el índice y el detalle discrepan, manda el detalle. Hay un solo Atendida por: si dos carpetas reclaman la misma deuda, manda el detalle, bloquea solo a ese y el otro no la modifica.

Bloquea `Cerrado` una `En atención` o una deuda pendiente sin clasificar cuya columna Atendida por es este requerimiento. Si el origen es este y la atiende otro, no bloquea. Una `Abierta` no bloquea: va enlazada en la columna de pendiente de la fila del índice. Se juzgan una por una. El criterio de `Cerrado` está en `.docs/002_specs/README.md` › «Leyenda de estatus»; esta sección no pega esa leyenda.

Transiciones de deuda. El agente propone y no escribe. Las escribe el cierre del requerimiento de Atendida por, con la decisión de la persona: `Abierta` → `En atención` al empezar, con visto bueno; `En atención` → `Resuelta` al atenderla, con cómo se atendió, o → `Abierta` si el trabajo se detiene; `Abierta` o `En atención` → `Descartada` con motivo y quién. `Resuelta` y `Descartada` permanecen. Al descartar la spec, ese cierre elige `Abierta` o `Descartada` para cada `En atención` de este requerimiento. Una `Abierta` cuyo origen es este sigue `Abierta`, salvo descarte en esa decisión. La que atiende otro no se toca. Al diferir no corre el cierre: la persona deja esa elección en el detalle al anotar la pausa.

## Origen (tipo)

`Decisión de requerimiento` (se difirió a propósito) · `Hallazgo` (se encontró al analizar o
revisar) · `Alta manual` (la registró una persona).

## Índice

| Prefijo | Deuda | Módulo | Origen | Estado | Prioridad | Atendida por | Alta |
|---|---|---|---|---|---|---|---|

## Plantilla de un detalle

```markdown
# <prefijo> — <Título>

| Dato | Valor |
|---|---|
| Estado | Abierta / En atención / Resuelta / Descartada |
| Alta | AAAA-MM-DD |
| Origen | <tipo> + referencia (carpeta del plan, decisión o ticket) |
| Módulo | <módulo o pantalla> |
| Prioridad | Alta / Media / Baja |
| Atendida por | <prefijo_Requerimiento, commit o ticket> (o "—") |

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

- Número nuevo y revisión de duplicados: las reglas de `.docs/002_specs/README.md` › "Numeración única",
  aplicadas a los archivos `<prefijo>_<Nombre>.md` de esta carpeta (remota: `git ls-tree --name-only @{u} .docs/003_deuda/`).
  Si se renumera una deuda, se actualizan también las specs que la citen. Numeración
  **independiente** de la de las specs. No se reutilizan números.
- Una fila por deuda, con su estado y su(s) referencia(s). El detalle vive en el archivo.
- Al pasar a `Resuelta`, llenar "Atendida por" y "Cómo se atendió", y anotar la fecha en el
  historial. La carpeta del plan que la resolvió también debe citar la deuda.
- Una deuda que nace de un plan se cita en el `PUNTO DE RETOMO` o en las decisiones de ese plan.
