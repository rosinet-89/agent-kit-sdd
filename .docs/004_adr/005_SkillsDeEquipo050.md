# ADR 005: Skills de equipo de 0.5.0 y las que se descartan o difieren

- **Estado:** Aceptado
- **Fecha:** 2026-10-24 (registrada; decidida en 0.5.0)
- **Origen:** SKILLS-SUGERIDAS.md (análisis de 11 skills)

## Contexto
Se sugirieron 11 skills (6 de equipo, 5 de gestión personal). Había que evitar choques con `sdd`, `git-commit`, `napkin`, `formato-respuesta`, los 4 roles y los comandos `/sdd*`.

## Decisión
Entran a `template/`: `adr`, `migracion-bd` y `seguridad-basica`, integradas en `git-commit`, reviewer, `/sdd-plan`, `/sdd-cierre` y `sdd`. No entra `code-review`. Se difieren `pruebas`, `release-notes`, `gestion-sprint-review`. No entran `gestion-riesgos` ni `gestion-onboarding`. Quedan para la capa de usuario `gestion-spec-desde-ticket` y `gestion-minuta` (propuesta aparte, no aplicada).

## Alternativas descartadas
| Alternativa | Por qué no |
|---|---|
| `code-review` | Mismo nombre que la skill integrada de Claude Code (la personal gana a la de proyecto); el reviewer y `/sdd-validate` ya lo cubren |
| `pruebas` | Depende del stack y se solapa con `07_pruebas_qa.md`; mejor por proyecto vía `/sdd-init` |
| `release-notes` | Se solapa con `/sdd-cierre` y `git-commit` |

## Consecuencias
`seguridad-basica` no sustituye `/security-review`. `migracion-bd` no fija un motor de base de datos.
