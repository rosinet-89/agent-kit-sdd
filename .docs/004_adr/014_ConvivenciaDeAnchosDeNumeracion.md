# ADR 014: Un ítem nuevo conserva el ancho de su carpeta

- **Estado:** Aceptado
- **Fecha:** 2026-10-25
- **Origen:** `005_ConvivenciaNumeracion`

## Contexto
En 0.5.0 los ítems de specs, deuda y ADR de un proyecto pasan a 4 dígitos (`0001`), pero un proyecto que ya tiene el kit conserva carpetas de 3 dígitos (`017_...`). Sin una regla, el "siguiente libre" se calculaba por texto, el orden alfabético se rompía (`0018` se lista antes que `017`), la revisión de duplicados comparaba anchos distintos y un agente podía proponer renumerar ítems ya citados desde otras specs, deudas y ADR.

## Decisión
Un ítem nuevo conserva el ancho de su carpeta. El prefijo se lee como entero (`017` = `0017`) y el siguiente libre es el mayor valor + 1, contando la copia local y la rama remota. El ancho de una carpeta es el del ítem de mayor valor; una carpeta vacía toma el de las otras carpetas con ítems (y 4 si ninguna tiene). Nunca se renumera por un cambio de ancho o de versión: solo se renombra para resolver un duplicado, con confirmación. En un empate de valor con anchos distintos manda el ítem publicado en la rama remota y, si ninguno lo está, el mayor ancho.

## Alternativas descartadas
| Alternativa | Por qué no |
|---|---|
| Pasar a 4 dígitos desde el primer ítem nuevo | Rompe el orden alfabético y deja una carpeta con dos anchos |
| Renumerar los ítems existentes a 4 dígitos | Rompe las citas desde otras specs, deudas y ADR, y exige tocar archivos del grupo C |
| Preguntar el ancho al usuario en cada alta | Una pregunta más en cada spec, deuda o ADR, sin ganancia frente a una regla determinista |

## Consecuencias
La regla tiene una sola fuente, la sección « Numeración única » de `template/.docs/002_specs/README.md`; las demás piezas la remiten con `<prefijo>` y una regla mínima de una frase. El chequeo E12 compara por valor numérico, informa la mezcla de anchos como `AVISO` y falla por duplicado por valor, ancho inválido y, en el clon, ancho distinto del declarado. Limitaciones conocidas en el avance de la `005` (decisión 28): prefijos con ceros de relleno, el AVISO en el clon y el ancho de carpeta con un ítem inválido.
