# ADR 006: Graphify no entra al kit

- **Estado:** Aceptado
- **Fecha:** 2026-10-24 (registrada; evaluada, no probada)
- **Origen:** Evaluación de https://github.com/Graphify-Labs/graphify

## Contexto
Se evaluó Graphify (grafo de conocimiento del código para asistentes). Los datos salen de la página del repo; no se instaló ni probó.

## Decisión
No se incluye en el kit. Si se usa, es una herramienta personal fuera del repo, o una mención opcional en `README.md`.

## Alternativas descartadas
| Alternativa | Por qué no |
|---|---|
| Integrarlo en `template/` | Exige Python y hooks de Git, escribe sus propios `AGENTS.md` y skills, y sus hooks pisan el orden de lectura que dicta el kit; contradice "solo Markdown" (ADR 004) |

## Consecuencias
Si se retoma, hay que verificar la licencia (la página dice Apache-2.0 y MIT) y probarlo en un proyecto con mucho código.
