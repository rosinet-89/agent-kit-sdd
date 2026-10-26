# ADR 0016: Un ítem nuevo del clon se escribe con 4 dígitos

- **Estado:** Aceptado
- **Fecha:** 2026-10-25
- **Origen:** `007_DeudasAbiertas`

## Contexto
El índice del producto declara el ancho del clon. Los ítems ya creados de este repo están en 3 dígitos (`001` sigue `001`). Hacía falta decir con cuántos dígitos se escribe el siguiente, sin renumerar los viejos y sin tocar la regla de un proyecto.

## Decisión
Un ítem nuevo del clon se escribe con 4 dígitos. Los ya creados no se renumeran. `015` y `0015` son el mismo entero: si coexisten, son duplicado. En un proyecto, un ítem nuevo conserva el ancho de su carpeta. En el clon, una mezcla válida no es `AVISO` y el ancho ya escrito no es `FALLA`.

## Alternativas descartadas
| Alternativa | Por qué no |
|---|---|
| Dejar el clon en 3 dígitos | El usuario pidió 4 para el ítem nuevo |
| Renumerar `001` a `0001` | Dejaría en rojo los ítems ya creados y reescribiría el historial |
| Avisar también en el clon | El `AVISO` de mezcla válida sigue siendo solo de proyecto (ADR 014) |

## Consecuencias
No reemplaza al ADR 014: la convivencia de un proyecto, el `AVISO` de proyecto y el desbordamiento (`1000` en ancho 3) siguen ahí. El siguiente ADR de este clon es `0016`, no `016`. La declaración vive en `template/.docs/002_specs/README.md` › « Numeración única ».
