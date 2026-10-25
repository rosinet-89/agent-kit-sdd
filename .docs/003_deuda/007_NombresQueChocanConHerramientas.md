# 007 — Nombres de archivo que chocan con otras herramientas

| Dato | Valor |
|---|---|
| Estado | Abierta |
| Alta | 2026-10-25 |
| Origen | Hallazgo (revisión pedida por el usuario, 2026-10-25) |
| Módulo | Nombres en la raíz del clon y en `template/` |
| Prioridad | Alta |
| Atendida por | — |

## Descripción
Varios archivos del kit usan un nombre que otras herramientas ya reclaman. En un disco que no distingue mayúsculas (`Changelog.md` y `CHANGELOG.md` son el mismo archivo), un plugin puede pisar el del kit sin aviso. La revisión cubre la raíz del clon y lo que `template/` copia a un proyecto. No se renombró nada.

## Por qué quedó pendiente
El usuario pidió identificar los nombres y dejar el cambio para después. Renombrar toca `INSTALAR.md`, `ACTUALIZAR.md`, los manifiestos y cada cita. No cabe en un parche.

## Impacto si no se atiende
Un release plugin, un generador o la propia herramienta de agentes puede reescribir el archivo. Se pierde el texto del kit o se mezcla con el de otro producto. En macOS y Windows el choque ocurre aunque el otro programa use otra capitalización.

## Alcance (archivos, tablas)
Raíz del clon y raíz de `template/`. Los `README.md` anidados (`.docs/`, `.agents/skills/`) no entran: una herramienta de release mira la raíz.

## Cómo se atendería (propuesta)
1. Abrirlo como spec propia por el flujo `sdd`.
2. Decidir nombre nuevo solo para las filas de riesgo alto y medio. Los de riesgo bajo se quedan.
3. Un nombre del kit no debería ser el que un plugin escribe solo. El marcador de un proyecto ya evita esto: `.agents/.kit-version`, no `VERSION`.
4. Actualizar citas, `INSTALAR.md`, `ACTUALIZAR.md` y el manifiesto si el archivo vive en `template/`.

## Cómo se atendió (al resolver: qué se hizo y dónde)
—

## Hallazgo (2026-10-25)

`VERSION`, `CHANGELOG.md` y `ACTUALIZAR.md` están solo en la raíz del clon. No viajan dentro de `template/`. Un proyecto instalado no los recibe. El riesgo es este repositorio y cualquier herramienta que se ejecute aquí.

| Archivo | Dónde | Quién más lo usa | Riesgo |
|---|---|---|---|
| `VERSION` | Raíz del clon | release-please, semantic-release, standard-version, bumpversion, setuptools, scripts de Make y Docker | Alto. Sin extensión y con un solo número adentro. Lo reescriben entero. `version` y `Version` son el mismo archivo en macOS y Windows. |
| `CHANGELOG.md` | Raíz del clon | Keep a Changelog, GitHub Releases, release-please, semantic-release, changesets, release-it | Alto. Esos programas crean o reescriben ese nombre. `Changelog.md` choca por mayúsculas. |
| `AGENTS.md` | Raíz del clon y `template/` (se copia al proyecto) | Convención agents.md: Cursor, Codex, Copilot, Windsurf, Claude | Alto. El nombre es del ecosistema. La instalación no pisa uno que ya existe; otra herramienta sí puede reescribirlo. |
| `CLAUDE.md` | Raíz del clon y `template/` | Claude Code (`/init` lo regenera) | Medio. Pocas herramientas ajenas lo usan. Claude sí lo reemplaza. |
| `MEMORY.md` | Raíz del clon y `template/` | Plugins de memoria de agentes (`memory.md`) | Medio. Mismo choque de mayúsculas. |
| `.gitignore` | `template/` (se fusiona a mano) | git, npm, cargo, y casi todo generador | Medio. Lo habitual es agregar líneas. Un starter puede reemplazar el archivo. |
| `.agents/` | Copia local, enlazada desde `.claude/`, `.cursor/`, `.github/`, `.gemini/` | Otros kits de agentes que instalan skills en `.agents/skills/` | Alto. Dos kits en el mismo directorio se mezclan. |
| `.claude/`, `.cursor/`, `.github/`, `.gemini/` | Enlaces del kit | La herramienta dueña de esa carpeta | Alto si el enlace se rompe. La herramienta crea la carpeta de verdad y el enlace queda `OCUPADO` (`sync-agents.md`). |
| `README.md` | Raíz del clon. `template/` no trae uno en la raíz | GitHub, npm, cargo, todo generador | Medio en este clon. No se copia a un proyecto. |
| `LICENSE` | Raíz del clon | GitHub, licensee, npm | Medio. Un «agregar licencia» sustituye el archivo. `LICENSE.md` y `LICENCE` chocan por mayúsculas o por el nombre inglés. |
| `ACTUALIZAR.md` | Raíz del clon | Ningún plugin conocido con ese nombre | Bajo. No es un nombre inglés de release. `Actualizar.md` sí choca en macOS y Windows. Un script propio llamado `actualizar` podría reclamarlo. |
| `INSTALAR.md` | Raíz del clon | Los plugins buscan `INSTALL` o `INSTALL.md`, no este nombre | Bajo. Misma reserva de mayúsculas que `ACTUALIZAR.md`. |
| `manifiestos/` | Raíz del clon | `manifest.json`, `MANIFEST.in`, `__manifest__.py` son otros nombres | Bajo. |
| `.docs/` | Clon y `template/` | MkDocs y Docusaurus usan `docs/` sin punto | Bajo. El punto los separa. |
| `template/` | Solo el clon | Cookiecutter, Yeoman y generadores usan `template/` o `templates/` | Medio en este clon. No se copia con ese nombre a un proyecto. |
| `SKILL.md` | `.agents/skills/<nombre>/` | Convención de skills de Claude | Bajo. El nombre es el estándar. El choque sería otro sistema de skills en la misma carpeta. |
| `PREFERENCIAS.md`, `napkin.md` | Local o `.agents/` | Sin herramienta conocida | Bajo. |

`ACTUALIZAR.md` no está en el mismo riesgo que `VERSION` y `CHANGELOG.md`. Esos dos son los que un plugin de release escribe solo.

## Historial
- 2026-10-25 — Alta, con el visto bueno del usuario. Sin renombrar.
