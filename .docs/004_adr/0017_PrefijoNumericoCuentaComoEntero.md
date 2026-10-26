# ADR 0017: Un prefijo numérico cuenta como entero

- **Estado:** Aceptado
- **Fecha:** 2026-10-25
- **Origen:** `007_DeudasAbiertas`

## Contexto
E12 trataba un ancho distinto de 3 o 4 dígitos como fallo. Un prefijo con ceros de relleno (`00017`) quedaba inválido aunque su valor fuera 17. El nombre no debía reescribirse, y dos escrituras del mismo valor no podían contar como números distintos.

## Decisión
Un prefijo numérico vale su entero: `17`, `017`, `0017` y `00000017` son 17. Los ceros de relleno y el ancho escrito no son fallo y el nombre no se reescribe. Dos ítems de la misma carpeta con el mismo entero son duplicado y es `FALLA`; esa igualdad no es `AVISO`. Un prefijo relleno cuenta para el entero y el duplicado, y no fija el ancho de la carpeta con su cantidad de dígitos. Un nombre que no empieza por dígitos y `_` (`abc_Mal`) no entra en el consecutivo ni en el ancho y no se renombra.

## Alternativas descartadas
| Alternativa | Por qué no |
|---|---|
| Seguir marcando el relleno como ancho inválido | El usuario lo descartó: el relleno no cambia el valor |
| Tratar cada ancho como un número distinto | `17` y `017` serían dos ítems y el duplicado no se vería |
| Renombrar `abc_Mal` o usarlo para el ancho | Solo los prefijos numéricos llevan el consecutivo |

## Consecuencias
No reemplaza al ADR 014. El duplicado por valor sigue siendo fallo. El desbordamiento sin relleno (`1000` en ancho 3) sigue en el ADR 014 y no es este borde. La fila E12 dice el resultado y remite a « Numeración única »; no copia la regla. La deuda `008_LimitacionesDeLaNumeracion` quedó resuelta por `007_DeudasAbiertas`.
