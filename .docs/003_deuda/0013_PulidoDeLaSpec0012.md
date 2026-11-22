# 0013 — Pulido de lo entregado en la spec 0012: duplicación, una tensión de la spec, una frase y E13

| Dato | Valor |
|---|---|
| Estado | Resuelta |
| Alta | 2026-11-16 |
| Origen | Hallazgo al validar `0012_EstatusDeSpecYCierre` (observaciones «puede esperar» aceptadas por la persona el 2026-11-16 17:57; alta aprobada en el cierre) |
| Módulo | `sdd-cierre.md`, leyenda de estatus, `validar-estructura.md` (E13) |
| Prioridad | Baja |
| Atendida por | `0017_PausaOrigenYContexto` (cierre 2026-11-22) |

## Descripción
Cuatro puntos menores que la validación independiente dejó como «puede esperar»:
1. `template/.agents/commands/sdd-cierre.md` repite parte de lo que ya dicen la leyenda y el README de deuda: las palabras que el cierre no escribe, los destinos de la reapertura y las transiciones de `En atención`. Es el único punto donde el RNF-4 (una sola fuente) queda con reservas.
2. Tensión interna de la spec: «Transiciones» dice que el cierre escribe `Implementado`, `Cerrado` o `Descartado`, pero su supuesto 12 hace que reabrir un `Descartado` lo devuelva a `Análisis` y `borrador`, escrito por el cierre.
3. El punto 3 de `sdd-cierre.md` tiene una frase enrevesada («y no antes de la comprobación previa cuando el camino la exige»): no es incorrecta, pero se lee mal.
4. E13 conserva «si discrepan, manda la carpeta» para el cruce de la fila con la carpeta o el archivo, que convive con la regla de que los conteos siguen la celda. E22 lo aclara; E13 no lo dice.

## Por qué quedó pendiente
No bloquean ningún RF y la persona aceptó las tres primeras observaciones al validar; la cuarta salió de la revisión final. Corregir cualquiera obliga a repetir la réplica y el manifiesto.

## Impacto si no se atiende
Riesgo bajo: una lectura menos clara de `sdd-cierre.md` y la posibilidad de que dos fuentes (E13 y la leyenda) se lean como contradictorias.

## Alcance (archivos, tablas)
`template/.agents/commands/sdd-cierre.md`, `template/.agents/validar-estructura.md` (E13), `.docs/002_specs/0012_EstatusDeSpecYCierre/01_spec.md` (texto de «Transiciones» o del supuesto 12) y las réplicas en `.agents/`; versión, changelog y manifiesto si cambia `template/`.

## Cómo se atendería (propuesta)
1. Aligerar `sdd-cierre.md` remitiendo a la leyenda y al README de deuda en los tramos duplicados, sin perder lo que exigen RF-5 y RF-12.
2. Decidir si «Transiciones» incluye la reapertura de `Descartado` entre lo que escribe el cierre, o cambiar el supuesto 12.
3. Reescribir la frase del punto 3.
4. Añadir a E13 una remisión a la leyenda para el desfase.

## Cómo se atendió (al resolver: qué se hizo y dónde)
RF-29 de `0017_PausaOrigenYContexto`: `template/.agents/commands/sdd-cierre.md` remite a la leyenda y a `## Estados` de la deuda en los tramos duplicados; «Quién escribe» de la leyenda incluye la reapertura de un `Descartado` por el cierre (la spec 0012, `cerrada`, no se editó); se reescribió la frase del punto 3; E13 remite a la leyenda para el desfase.

## Historial
- 2026-11-16 — Alta, en el cierre de `0012_EstatusDeSpecYCierre`, con el visto bueno de la persona.
- 2026-11-22 — Pasa a En atención y a Resuelta en el cierre de `0017_PausaOrigenYContexto`, con el visto bueno de la persona.
