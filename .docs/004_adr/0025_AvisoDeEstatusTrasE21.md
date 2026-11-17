# ADR 0025: Aviso de estatus tras E21

- **Estado:** Aceptado
- **Fecha:** 2026-11-16
- **Origen:** `0012_EstatusDeSpecYCierre`

## Contexto
El chequeo de estructura avisa y no escribe. E13 cruza la fila del índice con el archivo y dice que manda la carpeta. El aviso de la spec no frena. Hacía falta decir el desfase de la celda sin mezclarlo con ese cruce.

## Decisión
Chequeo nuevo E22, después de E21, solo `AVISO`: no es `FALLA` y no escribe. E13 no se edita.

## Alternativas descartadas
| Alternativa | Por qué no |
|---|---|
| Ampliar E13 | E13 cruza fila y archivo y dice que manda la carpeta. Meter ahí el aviso mezcla ese cruce con la forma de la celda. La frase de E13 queda para el cruce fila/archivo; el desfase de la celda lo dice E22, que cita `## Leyenda de estatus` y no la pega. |

## Consecuencias
El alcance del chequeo, el mensaje `estructura OK` y `sdd-estructura.md` pasan a E1–E22. E12, E13 y E21 no cambian de hecho. E22 aplica la definición de chequeo de la spec y cita `.docs/002_specs/README.md` › «Leyenda de estatus» (en el kit, `template/.docs/002_specs/README.md`). Si esa leyenda no trae `Cerrado` y `Descartado`, el aviso lo dice y no reescribe el índice (grupo C).
