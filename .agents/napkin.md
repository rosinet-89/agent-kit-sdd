# Napkin Runbook

**Canonical path:** `.agents/napkin.md`
All agents read and curate this file. Do not keep a second copy under `.claude/` or `.cursor/`.

## Curation Rules
- Re-prioritize on every read.
- Keep recurring, high-value notes only.
- Max 10 items per category.
- Each item includes date + "Do instead".

## Execution & Validation (Highest Priority)
1. **[2026-10-24] Ensayo de instalación**
   Do instead: siempre en `mktemp -d`, nunca en un proyecto real.

## Shell & Command Reliability
1. **[2026-10-24] Exclusiones de la instalación en raíz van ancladas con `/` y con negación del napkin**
   Do instead: `/.agents/*` + `!/.agents/napkin.md`, `/.gitignore`, `/PREFERENCIAS.md`, `/.claude/`,
   `/.cursor/` en `.git/info/exclude`. Sin `/` también ignoran `template/AGENTS.md` y `template/.agents/`.
   Lo versionado de la raíz (`AGENTS.md`, `MEMORY.md`, `.docs/`) no se excluye.
2. **[2026-10-24] `cp -n` avisa "non-portable" en GNU coreutils 9.x**
   Do instead: `cp -R --update=none` en Linux.
3. **[2026-10-24] `git clone` falla en la carpeta abierta (la sesión ya creó `.claude/`)**
   Do instead: `git init` + `git remote add origin <url>` + `git fetch` + `git checkout -b main --track origin/main`.

## Domain Behavior Guardrails
1. **[2026-10-24] Los cambios al kit van en `template/`**
   Do instead: editar `template/` y los docs de la raíz; el `.agents/` de la raíz es la herramienta
   (local, ignorada), no el producto.

## User Directives
(Directivas del equipo. Las personales de cada usuario van en `PREFERENCIAS.md`, no aquí.)
(vacío)
