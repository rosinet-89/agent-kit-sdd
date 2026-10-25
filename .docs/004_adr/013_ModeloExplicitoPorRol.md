# ADR 013: model inherit en los cuatro roles

- **Estado:** Aceptado
- **Fecha:** 2026-10-25
- **Origen:** `004_ModeloYPermisosAgentes`

## Contexto
Ningún agente declaraba `model`: todos heredaban el de la sesión. Pasar el coordinator a un alias más barato exigía la comparación de RF-4. En esta sesión el alias `sonnet` no es un modelo que se pueda lanzar y no se sustituyó por otro. El implementer no entró en esa comparación: no hay árbol previo a F1.5 de `001_ActualizarKit` y no se fabricó un fixture.

## Decisión
Los cuatro agentes declaran `model: inherit`. El coordinator no pasa a `sonnet`. El implementer queda en `inherit` sin corrida y sin fixture.

## Alternativas descartadas
| Alternativa | Por qué no |
|---|---|
| `haiku` para el coordinator | No hubo comparación de RF-4 para ese alias |
| `sonnet` para el coordinator | No hubo corrida válida. Un alias ausente no se sustituye |
| Pasar el implementer a otro modelo rehaciendo el paso sobre el árbol de hoy | Cumpliría el criterio sin trabajo del modelo: el paso ya está en el árbol actual |

## Consecuencias
El costo por feature no baja por el modelo del rol. Declarar `inherit` deja explícito que el rol usa el modelo de la sesión. Si más adelante hay una revisión separable, la comparación del implementer se corre antes de aceptar otro valor. Rehacer F1.5 de `001_ActualizarKit` no prueba un modelo; esa trampa está en `MEMORY.md`.
