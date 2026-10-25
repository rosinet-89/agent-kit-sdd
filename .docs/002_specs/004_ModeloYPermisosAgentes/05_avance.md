# 05 — Avance: Modelo por rol y permisos del frontmatter de los agentes
Leyenda: `[ ]` Pendiente · `[~]` En curso · `[x]` Hecho · `[!]` Bloqueado

## PUNTO DE RETOMO — leer esto primero
| Dato | Contenido |
|---|---|
| Última sesión | 2026-10-25 14:58 |
| Fase actual | `clarify` cerrada; falta `plan` |
| Último paso cerrado | `clarify`: spec aprobada (21 RF, 7 RNF, sin dudas abiertas) |
| Siguiente paso | Esperar el cierre de la `003` (Corlos); luego `/sdd-plan 004_ModeloYPermisosAgentes` |
| Bloqueos abiertos | Ninguno |
| Coordinación | Corlos se encarga del `06_flujo.md` y del cierre de `003_OptimizarArnes`. No tocar esa carpeta. Esta spec cambia el frontmatter de los 4 agentes de `template/.agents/agents/`; partir de su contenido actual (la `003` ya reescribió sus cuerpos). Cualquier cambio a `template/` obliga a regenerar `manifiestos/0.5.0.md` y repetir E20 al cerrar. |
| Estado BD | No aplica |
| Código | Ninguno |

## Pasos
(Se llena al tener `04_tasks.md`.)

## Decisiones
1. **[2026-10-25 14:36] Se atiende la deuda `003_ModeloYPermisosDeAgentes` por el flujo SDD, como esta spec.** Origen: petición del usuario (\"procede con la deuda 3\"), una vez implementada la `003`. Alternativa descartada: editar el frontmatter directo sin spec.
2. **[2026-10-25 14:36] Verificación previa (paso 1 de la deuda), contra la documentación de subagentes de Claude Code (https://code.claude.com/docs/en/sub-agents), leída el 2026-10-25 14:36.** Hallazgos: (a) `mode` y `permissions` NO están entre los campos soportados y se ignoran sin aviso; (b) sí existen `tools` (lista permitida), `disallowedTools`, `model` (`sonnet`, `opus`, `haiku`, `fable`, ID completo o `inherit`), `permissionMode`, `maxTurns`, `skills`, `hooks`, `memory`, `omitClaudeMd`, `effort`, `color`; (c) por defecto el subagente carga la jerarquía de CLAUDE.md y los AGENTS.md cargados como instrucciones del proyecto, salvo con `omitClaudeMd: true`; (d) las reglas de permisos por patrón viven en `permissions.allow/deny` de los archivos de settings y valen para toda la sesión, no por subagente. Limitación: el resumen de la página cubrió los primeros 100.000 de 116.349 caracteres.
3. **[2026-10-25 14:42] Lectura completa de la documentación de subagentes** (los 16.349 caracteres restantes). Hechos nuevos: existen `hooks` por subagente (`PreToolUse` sobre `Bash`, exit code 2 bloquea) pero el ejemplo usa un script externo; el ejemplo de revisor de solo lectura de la propia documentación usa `tools` sin `Edit`/`Write`; las `description` de subagentes comparten un presupuesto de 15.000 tokens.
4. **[2026-10-25 14:42] Mecanismo de restricciones:** `tools`/`disallowedTools` donde alcancen, instrucción marcada "no garantizada" para el resto y una regla `deny` opcional en la configuración local. Sin hooks: no se enmienda el ADR 004. Alternativas descartadas: hooks por subagente (código ejecutable) y solo instrucción.
5. **[2026-10-25 14:42] Modelo por rol:** `implementer` y `coordinator` en un modelo más barato; `planner` y `reviewer` en `inherit`; cada cambio se acepta solo con la comparación en los 3 pasos de referencia de la `003`. Alternativas descartadas: solo el coordinator y ningún rol.
6. **[2026-10-25 14:42] `omitClaudeMd: true` en `planner`, `reviewer` y `coordinator`;** no en `implementer` (depende de las reglas de Git y las reglas duras). Alternativas descartadas: ningún rol y los cuatro.
7. **[2026-10-25 14:42] `mode` y `permissions` se eliminan de los 4 agentes** y se registra que son formato de otra herramienta. Alternativas descartadas: conservarlos documentados y mantener dos versiones de cada agente.
8. **[2026-10-25 14:42] El reviewer pierde `Write`;** la sesión principal escribe `07_pruebas_qa.md` y `tests/` con lo que él devuelve. Alternativa descartada: conservarlo como no garantizado.
9. **[2026-10-25 14:42] Versión:** dentro de 0.5.0 sin publicar, con una línea más en su changelog y manifiesto regenerado al cerrar. Alternativa descartada: 0.5.1.
10. **[2026-10-25 14:48] Clarify: 18 hallazgos del reviewer (5 ALTA); veredicto CAMBIOS NECESARIOS.**
11. **[2026-10-25 14:48] Hechos verificados en la documentación:** `Agent(lista)` en `tools` solo restringe a un agente que corre como hilo principal con `claude --agent`; en un subagente se ignora. `disallowedTools` no acepta patrones (una entrada `Bash(git push *)` quita Bash entera); bloquear `git commit/push/fetch` exige `permissions.deny` en settings, que vale para toda la sesión. Orden del modelo: parámetro de la llamada, frontmatter, variable de entorno, modelo de la sesión; un alias bloqueado cae al heredado con aviso.
12. **[2026-10-25 14:48] Modelo: `sonnet` para `implementer` y `coordinator`;** `planner` y `reviewer` en `inherit`. Alternativas descartadas: `haiku` para el coordinator (más riesgo de enrutar mal y saltarse puertas) y solo declarar `inherit`.
13. **[2026-10-25 14:48] Orden de cierre: la `003` cierra primero** (Corlos) y esta spec parte de ese estado y regenera el manifiesto al final. Alternativa descartada: cerrar esta primero.
14. **[2026-10-25 14:48] Evidencia formal sin `Write` en el reviewer:** la sesión escribe `07_pruebas_qa.md` y el harness; el reviewer lo ejecuta en una segunda invocación (cuenta como una vuelta de las 2). Alternativa descartada: retirar el harness del flujo.
15. **[2026-10-25 14:58] Spec aprobada por el usuario** (`Estado: aprobada`; 21 RF, 7 RNF, sin dudas abiertas), con las decisiones del planner: umbral de costo del 125 % de los bytes del modelo actual con una repetición por caso fallido; `/sdd-init` fuera del alcance; fila "sin red" garantizada por `tools`; pieza única de restricciones en la skill `sdd` (grupo A); RF-21 para la actualización parcial reviewer/`sdd-validate`. Corrección menor: RF-19 llamaba "no versionada" a toda la raíz; el `AGENTS.md` de la raíz sí está versionado.
16. **[2026-10-25 14:58] No se abre `/sdd-plan` todavía:** la `003` debe cerrar primero (decisión 13) y el usuario no pidió el plan.

## Pruebas
(Se llena en `validate`.)

## Tiempos
| Paso | Inicio | Fin | Minutos | Pausa | Nota |
|---|---|---|---|---|---|
| spec | 2026-10-25 14:36 | 2026-10-25 14:43 | 7 | 0 | verificación de documentación, planner (20 RF, 7 RNF), 5 dudas respondidas y ajustes |
| clarify | 2026-10-25 14:43 | 2026-10-25 14:58 | 15 | 0 | reviewer (18 hallazgos), verificación de docs, 3 decisiones, ajustes del planner y aprobación |

## Plan contra real
(Al cierre.)
