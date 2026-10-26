# ADR 0018: Solo tres nombres salen del patrón de un plugin de release

- **Estado:** Aceptado
- **Fecha:** 2026-10-25
- **Origen:** `007_DeudasAbiertas`

## Contexto
`VERSION`, `CHANGELOG.md` y `LICENSE` son nombres que un plugin de release puede escribir solo. En un disco que no distingue mayúsculas, otra capitalización es el mismo archivo. El resto de nombres del corte (raíz, carpetas de herramienta, `template/`, `.agents/`) se seguía manejando como hasta ahora.

## Decisión
Solo esos tres cambian: `VERSION` pasa a `VersionAgentKitSdd`, `CHANGELOG.md` a `ChangelogAgentKitSdd.md` y `LICENSE` a `LicenseAgentKitSdd`. El contenido de versión sigue `0.5.0`. El marcador de un proyecto sigue en `.agents/.kit-version`. El marcador `<VERSION>` del manifiesto y `.agents/skills/napkin/LICENSE` no son estos archivos y no cambian. `.agents/` no se renombra.

## Alternativas descartadas
| Alternativa | Por qué no |
|---|---|
| Renombrar también el riesgo bajo y las carpetas de herramienta | El usuario lo descartó: se siguen manejando como hasta ahora |
| Dejar `VERSION`, `CHANGELOG.md` y `LICENSE` | Un plugin puede pisarlos sin aviso |
| Otra capitalización, o el par `LICENSE`/`LICENCE` | En disco sin distinción de mayúsculas no es un nombre distinto |

## Consecuencias
La raíz del clon y las citas vivas usan los tres nombres nuevos. El punto 6 de la constitución nombra `VersionAgentKitSdd` y `ChangelogAgentKitSdd.md`. No se abre `0.5.1`. Los demás nombres del corte quedan como estaban.
