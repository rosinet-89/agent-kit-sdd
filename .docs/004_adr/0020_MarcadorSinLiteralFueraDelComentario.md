# ADR 0020: El marcador de init pendiente no deja su literal fuera del comentario

- **Estado:** Aceptado
- **Fecha:** 2026-11-01
- **Origen:** `0010_FalsoPositivoInitPendiente`

## Contexto
La regla de detección decía que el proyecto está sin personalizar si `AGENTS.md` «contiene» el marcador. Varias piezas del kit repetían esa condición con el literal del marcador en su propio texto. Se leía de forma literal y daba falso positivo: tras `/sdd-init` el literal seguía apareciendo en el propio `AGENTS.md` (issue #3 de rosinet-89/agent-kit-sdd). La regla necesita una forma de detección que no se dispare con su propia descripción.

## Decisión
El marcador se describe por posición y exactitud, sin el literal en `AGENTS.md`: es el comentario exacto que es la primera línea no vacía tras el título. Regla del cero: tras `/sdd-init` el literal aparece 0 veces en `AGENTS.md`. El paso 1 de «Cada sesión» es la fuente única de la regla y las demás piezas remiten a él sin repetir la condición. La cadena del marcador se copia en `.agents/validar-estructura.md` y la comprobación E21 la usa como control cruzado contra la plantilla template/AGENTS.md. Es una excepción explícita al principio 3 de la constitución (cada regla vive en una sola fuente), porque esa copia es el control que permite detectar una cadena alterada.

## Alternativas descartadas
| Alternativa | Por qué no |
|---|---|
| Archivo de estado que diga si el init está hecho | Añade un archivo que puede desincronizarse de `AGENTS.md`; el marcador ya está donde se lee |
| `grep` laxo, o buscar el comentario completo en 6 o más piezas | El literal seguiría repetido en cada pieza y volvería el falso positivo; la cadena viviría en muchas fuentes |
| Quitar la regla de `AGENTS.md` | Sin ella, un clon nuevo no avisa que falta personalizar y las demás piezas pierden su referencia |

## Consecuencias
Los proyectos existentes incorporan el paso 1 a mano (grupo B), y hasta entonces E21 da FALLA. Sin template/ en el proyecto, una cadena alterada solo se detecta por su forma, no por comparación. La raíz del clon, que no tiene .agents/.kit-version, se vigila con un `grep` manual. Toda pieza nueva que cite la regla debe remitir al paso 1 de «Cada sesión». La copia de la cadena en `.agents/validar-estructura.md` queda como duplicado aceptado y vigilado por E21.
