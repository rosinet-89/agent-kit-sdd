# 006 — /sdd-init sugiere los fragmentos allow y deny

| Dato | Valor |
|---|---|
| Estado | Abierta |
| Alta | 2026-10-25 |
| Origen | Decisión de requerimiento. Recorte de alcance en `004_ModeloYPermisosAgentes` (decisión 15 del avance, decisión 10 del plan) |
| Módulo | `sdd-init` |
| Prioridad | Media |
| Atendida por | — |

## Descripción
`template/.agents/skills/sdd/restricciones-de-rol.md` documenta dos fragmentos opcionales: `permissions.deny` (cinco patrones Bash de Git) y `permissions.allow` (los comandos de `AGENTS.md` › Comandos). Ningún agente los aplica. `/sdd-init` no los sugiere ni los copia a la configuración local.

## Por qué quedó pendiente
La spec 004 dejó `/sdd-init` fuera de alcance. Un `deny` de settings vale para toda la sesión y puede bloquear el Git de quien lo copie. Meterlo en la inicialización sin esa advertencia, y sin que el responsable elija los comandos de `allow`, salía del alcance.

## Impacto si no se atiende
Quien instala el kit no ve los fragmentos. Las restricciones que no cubre `tools` siguen solo en la pieza, y el `deny` de Git no llega a la configuración local salvo que alguien lo copie a mano.

## Alcance (archivos, tablas)
`template/.agents/commands/sdd-init.md` y `template/.agents/skills/sdd/restricciones-de-rol.md`. No se copia el fragmento a `.claude/` ni a un settings versionado.

## Cómo se atendería (propuesta)
1. Abrirlo como spec propia por el flujo `sdd`.
2. Hacer que `/sdd-init` sugiera los dos fragmentos, sin aplicarlos.
3. Conservar la advertencia de que el `deny` vale para toda la sesión y puede bloquear el Git de quien lo copie.
4. El `allow` sigue siendo genérico: lo llena el responsable con los comandos de su `AGENTS.md` › Comandos.

## Cómo se atendió (al resolver: qué se hizo y dónde)
—

## Historial
- 2026-10-25 — Alta, al cerrar `004_ModeloYPermisosAgentes`, con el visto bueno del usuario.
