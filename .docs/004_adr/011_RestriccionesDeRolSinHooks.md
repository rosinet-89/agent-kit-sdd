# ADR 011: Restricciones de rol sin hooks

- **Estado:** Aceptado
- **Fecha:** 2026-10-25
- **Origen:** `004_ModeloYPermisosAgentes`

## Contexto
Los cuatro agentes declaraban `mode` y `permissions`, campos de otra herramienta. La documentación de subagentes de Claude Code, leída el 2026-10-25, no los incluye: se ignoran sin aviso. Lo que sí restringe es `tools`. Un patrón de `disallowedTools` sobre Bash quita Bash entero. Un `deny` de settings vale para toda la sesión, no por subagente. Los hooks existen y serían código ejecutable. El ADR 004 fija que el kit es solo Markdown, sin scripts.

## Decisión
Las restricciones de rol viven en `template/.agents/skills/sdd/restricciones-de-rol.md`. `tools` garantiza lo que la lista cubre. Lo que no cubre queda como instrucción, marcada no garantizada, sin un `deny` de ruta. El `deny` de Git es un fragmento de texto en esa pieza, no se copia a `.claude/` y no lo aplica ningún agente. No hay hooks. Se quitan `mode` y `permissions`. El reviewer no tiene `Write` ni `Edit`; la evidencia formal la escribe la sesión.

## Alternativas descartadas
| Alternativa | Por qué no |
|---|---|
| Hooks por subagente | Código ejecutable. Contradice el ADR 004 |
| Dejarlo solo en la instrucción | El reviewer seguiría con `Write` y las restricciones de Git no tendrían ni `tools` ni un fragmento opcional |
| Bloquear Git con `disallowedTools` | Un patrón sobre Bash quita Bash entero |
| Conservar `mode` y `permissions` | Se ignoran y aparentan una protección que no existe |
| Un `deny` de escritura para toda la sesión | Bloquearía también a quien copia el fragmento |

## Consecuencias
La garantía es la lista `tools`. El `deny` de Git es opcional y, si alguien lo copia, puede bloquear su propio Git. No reemplaza al ADR 004: lo aplica. `/sdd-init` no sugiere esos fragmentos; eso queda en la deuda `006_SddInitSugiereAllowDeny`.
