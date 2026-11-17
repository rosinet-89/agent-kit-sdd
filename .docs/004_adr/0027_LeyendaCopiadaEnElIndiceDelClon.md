# ADR 0027: La leyenda se copia en el índice del clon

- **Estado:** Aceptado
- **Fecha:** 2026-11-16
- **Origen:** `0012_EstatusDeSpecYCierre`

## Contexto
Antecedentes abre `.docs/002_specs/README.md`. La numeración de ese índice ya remite a `template/`. La leyenda de estatus ya estaba copiada entre el clon y la plantilla: el análisis la sitúa en las líneas 20-21 de los dos README de specs (`02_analisis_impacto.md`). En un proyecto instalado no hay `template/` como fuente: la definición completa tiene que estar en el README de `template/.docs/`.

## Decisión
Se copia la leyenda en `.docs/002_specs/README.md` y la definición de deuda en `.docs/003_deuda/README.md`, en el mismo cambio que `template/`.

## Alternativas descartadas
| Alternativa | Por qué no |
|---|---|
| Remitir a `template/` como ya hace la numeración | Si el índice del clon solo remite, RF-2 no se responde sin abrir `template/`. |

## Consecuencias
El duplicado clon/plantilla es el que ya existía en esas líneas; no se suma una tercera copia en los comandos (eso lo cierra el ADR 0024, principio 3). En un proyecto instalado la definición completa queda en el README de `template/.docs/`. Las filas ya escritas del índice de specs no se reclasifican. Las filas 0009, 0010 y 0011 de deuda no cambian de estado. Mantener las dos copias al día es el costo: la de `template/` se distribuye y la del clon es la que Antecedentes abre.
