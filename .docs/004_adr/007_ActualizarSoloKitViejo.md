# ADR 007: La actualización reemplaza solo archivos KIT-VIEJO, con confirmación

- **Estado:** Aceptado
- **Fecha:** 2026-10-24 (registrada; decidida en `001_ActualizarKit`)
- **Origen:** `.docs/002_specs/001_ActualizarKit/` (clarify, decisión 8)

## Contexto
La regla del kit era "la copia nunca sobrescribe". Eso deja a un proyecto con una versión anterior a medias tras reinstalar: las skills nuevas se copian, pero los archivos que el kit modificó (por ejemplo `reviewer`, `git-commit`) no se reemplazan, y quedan skills sin conectar.

## Decisión
Al actualizar un proyecto, el agente puede reemplazar un archivo solo si es **KIT-VIEJO**: idéntico a la versión base del kit, es decir, que el proyecto no personalizó. Cada reemplazo exige confirmación explícita y se muestra el diff antes. Los archivos PERSONALIZADOS y los de fusión manual nunca se reemplazan. La instalación sigue sin sobrescribir nada.

## Alternativas descartadas
| Alternativa | Por qué no |
|---|---|
| Nunca reemplazar | Deja skills sin conectar y obliga a fusionar a mano archivos que el proyecto no tocó |
| Reemplazar todo salvo lo personalizado sin pedir confirmación | Un error de clasificación pisaría trabajo del proyecto sin aviso |

## Consecuencias
Se enmienda el principio 5 de la constitución, y `AGENTS.md` (raíz y `template/`) e `INSTALAR.md` deben distinguir instalar de actualizar. Depende de los manifiestos por versión para saber qué es KIT-VIEJO.
