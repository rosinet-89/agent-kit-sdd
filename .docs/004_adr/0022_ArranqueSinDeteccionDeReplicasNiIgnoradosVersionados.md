# ADR 0022: El arranque ya no detecta réplicas ni ignorados versionados

- **Estado:** Aceptado
- **Fecha:** 2026-11-02
- **Origen:** `0009_EnlacesTrasPull`

## Contexto
Hasta 0.5.0 el paso 7 de «Cada sesión» detectaba, además de los enlaces, las réplicas huérfanas o faltantes y los archivos ignorados que seguían versionados. Con el aviso combinado de 0009 (ADR 0021) el aviso debe caber en 3 líneas, con un máximo de una vez por sesión y solo de lectura. Añadir esas dos detecciones rompería ese tope.

## Decisión
El arranque solo informa versión, piezas y enlaces de las herramientas declaradas. Las réplicas y los ignorados versionados los detecta y corrige `/sdd-enlaces` (con confirmación) y los reporta `/sdd-estructura` (E2 y E3), siempre bajo demanda. El aviso no los menciona. Aprobado por el usuario el 2026-11-01.

## Alternativas descartadas
| Alternativa | Por qué no |
|---|---|
| Mantenerlos como aviso de solo lectura en el arranque | Añade una línea al aviso y rompe el tope de 2 a 3 líneas; aumenta el ruido en cada sesión por casos poco frecuentes |

## Consecuencias
Se pierde la detección automática de réplicas e ignorados versionados al abrir la sesión: solo aparecen si la persona corre `/sdd-enlaces` o `/sdd-estructura`. A cambio el aviso es único, breve y siempre accionable. Si esa pérdida causa problemas reales, un ADR nuevo puede reemplazar a este.
