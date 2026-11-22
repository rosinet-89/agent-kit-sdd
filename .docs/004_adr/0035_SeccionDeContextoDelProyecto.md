# ADR 0035: Sección `.docs/005_contexto/` para el contexto del proyecto bajo demanda

- **Estado:** Aceptado
- **Fecha:** 2026-11-22
- **Origen:** `0017_PausaOrigenYContexto`

## Contexto
El contexto del proyecto (negocio, estructura, procesos, integraciones) no tenía lugar propio:
`/sdd-init` lo volcaba en `AGENTS.md`, que se carga en cada turno (issue #10). Un documento suelto
envejece sin aviso. Restricciones: el ADR 0032 limita lo que entra en `AGENTS.md`; nada propio de un
proyecto entra a `template/`; `.docs/` es Grupo C (ADR 007).

## Decisión
`.docs/005_contexto/` es una sección numerada con índice: cada fila lleva archivo, qué responde,
cuándo leerlo y fecha de última verificación. Cada documento abre con «Objetivo» y «Última
verificación». `/sdd-init` propone la lista y los documentos se crean uno por uno con visto bueno.
`/sdd-cierre` propone actualizar el que se vio afectado. `/sdd-estructura` avisa por un documento sin
fila, una fila sin documento o una verificación de más de 180 días. `AGENTS.md` lleva una línea de
remisión al índice.

## Alternativas descartadas
| Alternativa | Por qué no |
|---|---|
| Seguir en `AGENTS.md` | Engorda el archivo siempre cargado (ADR 0032) |
| Que `/sdd-init` genere los documentos | Nacen sin revisar y envejecen |
| Entregar solo la sección, sin propuesta | La persona no sabe qué documentos le convienen |
| Reglas de framework en esta sección | Son genéricas: van a una skill de dominio |

## Consecuencias
El contexto se lee solo cuando hace falta y tiene fecha de verificación. La numeración es la de
«Numeración única». Un proyecto instalado antes recibe el README como archivo nuevo de Grupo C y, sin
la carpeta, los chequeos no fallan. Lo histórico va a `.docs/legacy/`, fuera del índice.
