# ADR 0031: El cierre decide el criterio 2 con sus commits en una corrida

- **Estado:** Aceptado
- **Fecha:** 2026-11-21
- **Origen:** `0016_CierreEnUnaCorrida`

## Contexto
El criterio 2 de la leyenda de estatus exigía que no quedara commit pendiente del requerimiento antes
de escribir `Cerrado`. En el orden habitual, el cierre corre antes de los commits, así que llegar a
`Cerrado` costaba dos corridas del cierre y un commit solo para el estatus (`0013_ModelosPorAgente`,
`0014_ConfiguracionGuiada`). El criterio protege algo real: `Cerrado` es terminal y el índice se
versiona, y no debe quedar un `Cerrado` cuyo trabajo no está en Git.

## Decisión
Si pasan la previa y los criterios 1, 3, 4 y 5, el cierre escribe `Cerrado` y `cerrada` y propone en la
misma corrida los commits del requerimiento y, al final, el de sus propios archivos, con una sola
confirmación. Confirmados, `Cerrado` queda. Sin confirmación, o si un commit falla, el cierre baja en
esa corrida a `Implementado` e `implementada`.

## Alternativas descartadas
| Alternativa | Por qué no |
|---|---|
| Commits primero y segunda corrida (regla anterior) | Doble ciclo y un commit extra solo para el estatus |
| Quitar el criterio 2 o dejarlo como aviso | Permite un `Cerrado` con trabajo sin registrar |
| Dejar `Cerrado` sin commits con un aviso | Es justo lo que el criterio evita; el índice mentiría al versionarse |

## Consecuencias
Un cierre con trabajo sin commit llega a `Cerrado` con una confirmación. La confirmación del commit
sigue siendo explícita (`AGENTS.md` › Git). Queda un estado transitorio local: si la sesión se corta
antes de confirmar, el árbol tiene `Cerrado` sin commits hasta la siguiente corrida, que lo retoma.
