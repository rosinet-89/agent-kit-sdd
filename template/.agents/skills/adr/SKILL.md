---
name: adr
description: Registra decisiones de arquitectura (ADR) en `.docs/004_adr/NNNN_<Titulo>.md` (4 dígitos en un proyecto, `0001`; 3 en el clon del kit) con contexto, alternativas descartadas y consecuencias. Usar cuando se tome una decisión técnica con alternativas reales y efecto duradero (capas, librerías, formato de datos, integración, seguridad), al decidir en `03_plan.md` o al cerrar un feature. No es para trampas o reglas de una línea (eso va en `MEMORY.md`). Disparadores - "ADR", "decisión de arquitectura", "por qué elegimos", "alternativa descartada", "registrar decisión".
---

# Skill: ADR (registro de decisiones de arquitectura)

Un ADR fija **por qué** se eligió algo, para que nadie lo reabra sin saber qué se descartó.
Índice y siguiente número: `.docs/004_adr/README.md`.

## ADR, MEMORY o plan

| Si la decisión… | Va en |
|---|---|
| Tiene alternativas reales y efecto duradero o transversal | ADR |
| Es una trampa, regla o hecho de una línea | `MEMORY.md` (se propone, no se escribe sin aprobación) |
| Solo aplica a un feature | `03_plan.md` › decisiones (con alternativa descartada) |
| Es un principio innegociable de todo el proyecto | `.docs/001_constitucion/constitution.md` (`/sdd-constitucion`) |

Si un ADR de un feature resulta transversal, el plan solo lo enlaza.

## Cuándo proponerlo

- Al redactar `03_plan.md`, si una decisión cumple la primera fila de la tabla.
- Al cerrar un feature (`/sdd-cierre`), para decisiones que se tomaron durante la implementación.
- Cuando el usuario lo pida.

El agente **propone** el ADR (título y una línea de por qué). No lo crea sin el visto bueno del usuario.

## Pasos

1. Busca en el índice `.docs/004_adr/README.md` si ya hay un ADR del tema. Si existe y la decisión
   cambia, no lo edites: crea uno nuevo que lo **reemplaza** y marca el anterior `Reemplazado por` su número.
2. Toma el siguiente número libre, contando la copia local y la rama remota descargada (mismo criterio
   de numeración única que `.docs/002_specs/README.md`, incluido el ancho: 4 dígitos en un proyecto,
   `0001`; 3 en el clon del kit). Los números no se reutilizan. No se limita a specs ni a deuda.
3. Crea `.docs/004_adr/NNNN_<TituloPascalCase>.md` (en el clon del kit, `NNN_`) con la plantilla de abajo.
4. Agrega su fila al índice.
5. Enlázalo desde el `03_plan.md` o la spec de origen.

## Plantilla

```markdown
# ADR NNNN: <título corto de la decisión>

- **Estado:** Propuesto | Aceptado | Reemplazado por NNNN | Descartado
- **Fecha:** AAAA-MM-DD
- **Origen:** <carpeta de .docs/002_specs/ o contexto>

## Contexto
<Qué problema o fuerza obliga a decidir. Restricciones reales, sin soluciones.>

## Decisión
<Qué se decidió, en una o dos frases.>

## Alternativas descartadas
| Alternativa | Por qué no |
|---|---|

## Consecuencias
<Qué se gana, qué se paga y qué queda condicionado. Incluye deuda técnica si la hay.>
```

## Reglas

- Un ADR no se borra ni se reescribe: se reemplaza o se descarta con motivo.
- Una decisión por archivo; sin código del proyecto dentro, solo referencias a rutas.
- Si contradice la constitución, se detiene y se consulta: gana la constitución.
