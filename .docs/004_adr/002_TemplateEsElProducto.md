# ADR 002: `template/` es el producto; la raíz no se distribuye

- **Estado:** Aceptado
- **Fecha:** 2026-10-24 (registrada; vigente desde 0.4.0)
- **Origen:** MEMORY.md (2026-10-24)

## Contexto
La instalación copia `template/`. Si el conocimiento propio del kit estuviera dentro, se filtraría a otros proyectos.

## Decisión
Lo distribuible se edita en `template/` y en los docs de la raíz (`README.md`, `INSTALAR.md`, `CHANGELOG.md`, `VERSION`, `LICENSE`). Nada propio de un proyecto (stack, nombres, dominio, ADR, specs del kit) entra a `template/`; allí solo viven los índices vacíos.

## Alternativas descartadas
| Alternativa | Por qué no |
|---|---|
| Un solo árbol compartido por raíz y `template/` | No separa lo que se distribuye de lo que es del kit |

## Consecuencias
El ensayo de instalación (`cp -R --update=none template/. $tmp/`) debe copiar todo sin filas de datos ni referencias propias del kit.
