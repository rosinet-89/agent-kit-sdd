# ADR 0032: Qué va en el archivo siempre cargado y qué se lee bajo demanda

- **Estado:** Aceptado
- **Fecha:** 2026-11-21
- **Origen:** `0015_ArranqueLigero`

## Contexto
`CLAUDE.md` inyecta `AGENTS.md` en cada sesión, y el protocolo «Cada sesión» abría archivos enteros
(`.agents/sync-agents.md`, `.agents/README.md`) para usar una sola sección o para un caso que casi
nunca ocurre. Medido el 2026-11-21: el arranque bajo control del kit leía 29 782 bytes en el clon y
`AGENTS.md` pesaba 14 279. No había criterio escrito para decidir si una sección nueva entra en
`AGENTS.md`, así que crecía con cada spec. Restricciones: los roles con `omitClaudeMd` leen secciones
de `AGENTS.md` por nombre. `/sdd-init` escribe ahí los valores del proyecto. Un proyecto ya instalado
nunca recibe un `AGENTS.md` nuevo (grupo B, ADR 007).

## Decisión
`AGENTS.md` lleva solo lo que se usa en cualquier turno o lo que los roles y `/sdd-init` buscan ahí
por nombre: «Guardrails», «Cada sesión», «Antecedentes», «Deuda técnica», «Skills de proyecto», «Git»,
«Stack», «Comandos», «Reglas de negocio» y «Reglas duras». Lo que usa un solo rol, comando o momento
vive en su archivo, y `AGENTS.md` deja un renglón que dice dónde está y cuándo leerlo. Los pasos de
«Cada sesión» piden leer secciones, no archivos enteros, y los que dependen de un caso dicen en una
línea cuándo aplican. El criterio vive en `.agents/README.md` › «Qué va en `AGENTS.md`».

## Alternativas descartadas
| Alternativa | Por qué no |
|---|---|
| Dejar solo «Guardrails» y «Cada sesión» en `AGENTS.md` | Los roles y `/sdd-init` buscan «Comandos», «Stack» y «Reglas de negocio» ahí; en proyectos instalados no se reemplazan |
| Sacar solo «Qué lee cada rol» | Ahorro marginal; no fija criterio y `AGENTS.md` vuelve a crecer |
| Un archivo nuevo para el chequeo de arranque | Un `AGENTS.md` viejo cita `sync-agents.md` › «Chequeo de arranque»; reordenar ese archivo resuelve sin remisiones dobles |
| Dejar el criterio solo en `03_plan.md` de la 0015 | Rige cada sección futura, no un feature |

## Consecuencias
El arranque del clon baja de 29 782 bytes a unos 12 000, y `AGENTS.md` alrededor de un 15 % frente a
su tamaño antes del cambio. Toda sección nueva de `AGENTS.md` se justifica con el criterio. Los roles
dependen de su propio «Qué lee», sin la tabla resumen de `AGENTS.md`. E8 y E9 pasan a mirar
`.agents/README.md` › «Roles y comandos». Los proyectos instalados conservan su `AGENTS.md` y siguen
funcionando: el ahorro llega cuando incorporan a mano los pasos nuevos (`ACTUALIZAR.md`). El ahorro
depende de que la herramienta lea una sección sin abrir el archivo entero.
