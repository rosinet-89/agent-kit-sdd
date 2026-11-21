# ADR 0030: Carpeta de agentes por archivo con adaptadores de rol

- **Estado:** Aceptado
- **Fecha:** 2026-11-21
- **Origen:** `0014_ConfiguracionGuiada`

## Contexto
Claude Code, Cursor, Gemini y Copilot leen los agentes por un enlace de carpeta a `.agents/agents/`,
que se versiona. El `model:` de esos archivos es el de Claude Code (`opus`, `sonnet`, `haiku`, ADR 0028);
otra herramienta no lo entiende, y el paso de `/sdd-init` de la 0013 escribía `inherit` en archivos
versionados cuando solo había otras herramientas. Con la configuración por dev (ADR 0029), un modelo
propio no puede escribirse en `.agents/agents/`. OpenCode ya usa adaptadores por rol.

## Decisión
La carpeta de agentes de una herramienta puede ser «por archivo»: carpeta real, no versionada, con un
enlace por rol a `.agents/agents/<rol>.md` o un adaptador de rol (mismo frontmatter salvo `model:` y
los campos que la herramienta no admite o exige, cuerpo que remite al canónico). En toda herramienta
distinta de Claude Code la carpeta es siempre por archivo; en Claude Code solo cuando un dev elige un
modelo distinto del sugerido. `.agents/agents/` nunca recibe `model:` de la configuración. La definición
vive en `.agents/sync-agents.md`, generaliza la de OpenCode, y la aplica `/sdd-enlaces`.

## Alternativas descartadas
| Alternativa | Por qué no |
|---|---|
| Escribir `model:` en `.agents/agents/` | Cambia archivos versionados por la elección de un dev |
| Modelos fijos en Claude Code, configurables solo fuera | Deja sin opción al dev de Claude Code |
| Enlace de carpeta hasta que el dev configure | La otra herramienta lee `model: opus` mientras tanto |
| Un formato de adaptador por herramienta | Varias definiciones; OpenCode ya prueba el patrón |

## Consecuencias
- Ninguna herramienta lee un `model:` que no entiende; `.agents/agents/` queda estable.
- `/sdd-enlaces` y `/sdd-estructura` (E3, E23) deben reconocer adaptadores en cualquier carpeta por
  archivo y avisar de un adaptador viejo cuando cambia el frontmatter canónico.
- Codex y Antigravity no tienen carpeta de agentes en el kit: su modelo queda solo como sugerencia.
- Reemplaza el paso de `inherit` de `/sdd-init` (0013); complementa el ADR 0028 sin cambiar su decisión.
