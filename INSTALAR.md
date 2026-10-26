# Instalar el kit en un proyecto

Este documento es para el **agente** (y para quien prefiera hacerlo a mano). El kit no trae scripts
de instalación: dependen del SO y del shell, y el kit es genérico. Instalar = **copiar el contenido
de `template/` a la raíz del proyecto destino, sin sobrescribir nada**.

Si el destino ya tiene el kit, no instales de nuevo: sigue `ACTUALIZAR.md`.

## Primero: elegir el modo

Antes de copiar nada, el agente decide dónde está parado:

| Situación | Qué hace |
|---|---|
| Destino = **proyecto existente** (ruta distinta del kit, con código o historial) | **Modo proyecto**: instala solo `template/`. No pregunta nada más. |
| Se abrió el **clon del kit** (hay `template/`, `VersionAgentKitSdd` y `ChangelogAgentKitSdd.md` en la raíz) o el destino es un proyecto **nuevo** | **Pregunta** al usuario cuál de las dos opciones de abajo. |

Pregunta (una sola, con estas dos opciones):

1. **Usar el kit en un proyecto nuevo**: se instala solo `template/` en la ruta que indique el usuario
   (**Modo proyecto**, pasos de "Qué hace el agente").
2. **Trabajar el propio kit** (actualizarlo, versionarlo): se instala en el clon, con las specs, ADR,
   memoria y reglas con las que se ha llevado el kit a su versión actual (**Modo mantenimiento**).

Si la respuesta no es clara, no asumas: pregunta de nuevo. En ningún modo se sobrescribe nada.

## Modo proyecto

Antes de copiar, comprueba si el destino ya tiene el kit: existe `.agents/.kit-version` o
`.agents/skills/sdd/SKILL.md`. Si existe cualquiera de los dos, no instales: remite a
`ACTUALIZAR.md` y detente.

Los pasos de la sección siguiente. El marcador de versión se escribe solo en este modo. El modo
mantenimiento no lo escribe.

### Qué hace el agente

1. Pregunta (o deduce) la ruta del proyecto destino. Debe existir.
2. Detecta el SO y el shell disponibles (Windows: PowerShell o Git Bash; macOS/Linux: sh/zsh/bash).
   No asumas ninguno.
3. Lista los archivos de `template/` (incluidos los ocultos: `.agents/`, `.docs/`, `.gitignore`)
   y marca cuáles ya existen en el destino.
4. Ofrece **una** de estas dos salidas y espera la elección del usuario:
   - **Script:** genera un script corto para su SO/shell, lo muestra y lo ejecuta solo con
     confirmación. Guárdalo fuera del kit y del destino (carpeta temporal); no lo dejes en el repo.
   - **Paso a paso:** da los comandos de copia (uno por carpeta/archivo) para que el usuario los corra.
5. Reglas de la copia, sea script o manual:
   - **Nunca sobrescribir** un archivo existente: se omite y se reporta.
   - Si el destino ya tiene `.gitignore`, `AGENTS.md`, `CLAUDE.md` o `MEMORY.md`, **no los
     reemplaces**: muestra qué líneas o secciones del kit faltan y propón fusionarlas a mano
     (en `.gitignore`: las reglas de `template/.gitignore`, incluida `PREFERENCIAS.md`).
   - No crear enlaces hacia `.claude/`, `.cursor/`, etc.: eso lo propone el agente dentro del
     proyecto con `.agents/sync-agents.md`.
6. Escribe `.agents/.kit-version` en el destino copiando la única línea de `VersionAgentKitSdd` del kit
   usado (no la escribas a mano). Al terminar, reporta: copiados, omitidos y pendientes de
   fusionar, más la versión del kit (archivo `VersionAgentKitSdd`).
7. Siguiente paso para el usuario: abrir el **proyecto** (no el kit) con su agente y correr `/sdd-init`.

### A mano (sin agente)

Copia el contenido de `template/` (con archivos ocultos) a la raíz del proyecto, sin pisar lo que
exista. Ejemplos de la idea, adáptalos a tu shell:

```sh
# macOS / Linux (cp -n no sobrescribe)
cp -Rn template/. /ruta/al/proyecto/
```
```powershell
# Windows: copia omitiendo los existentes
robocopy template C:\ruta\al\proyecto /E /XC /XN /XO
```

Después de copiar, deja el marcador de versión: una línea, la del archivo `VersionAgentKitSdd` del kit.

```sh
printf '%s\n' <versión> > .agents/.kit-version
```
```powershell
Set-Content -Path .agents\.kit-version -Value '<versión>'
```

Revisa después el `.gitignore` del proyecto y fusiona a mano las reglas de `template/.gitignore`.

## Modo mantenimiento

Para quien trabaja el propio kit. La raíz del clon es el destino. Lo propio del kit **ya viene
versionado en el clon** y no se sobrescribe:

| Viene en el clon (versionado) | Se instala desde `template/` (local, excluido) |
|---|---|
| `AGENTS.md`, `CLAUDE.md`, `MEMORY.md` | `.agents/` (agentes, comandos, skills, README, sync) salvo `napkin.md` |
| `.docs/` (constitución, specs, deuda, ADR) | `.gitignore`, `PREFERENCIAS.md` |
| `.agents/napkin.md` | enlaces `.claude/`, `.cursor/`… (`.agents/sync-agents.md`) |

Pasos del agente:

1. Comprueba que está en la raíz del clon (existen `template/`, `VersionAgentKitSdd` y `AGENTS.md`).
2. Copia `template/` a la raíz **sin sobrescribir** (como en "Modo proyecto"). `AGENTS.md`, `MEMORY.md`,
   `.docs/` y `napkin.md` quedan como están en el clon; los archivos de `template/` que falten en
   `.docs/` (por ejemplo un índice nuevo) se copian y aparecerán como nuevos para revisar.
3. Escribe en `.git/info/exclude` (local, no en un `.gitignore`), **ancladas con `/`**:
   ```
   /.agents/*
   !/.agents/napkin.md
   /.gitignore
   /PREFERENCIAS.md
   /.claude/
   /.cursor/
   ```
   Agrega `/.gemini/` y `/.github/` si usa esas herramientas. Sin la barra inicial se ignorarían
   también `template/AGENTS.md` y `template/.agents/`, y el kit se rompería.
4. Propone los enlaces de `.agents/sync-agents.md` y crea `PREFERENCIAS.md` (paso 10 de `AGENTS.md`).
5. No corre `/sdd-init`: el `AGENTS.md` del clon ya está personalizado.
6. Comprueba con `git status --short`: solo deben aparecer cambios en `template/`, en los docs de la
   raíz o en el conocimiento versionado del kit. Si aparece `.agents/skills/` u otro archivo copiado,
   falta una exclusión.
7. Si `template/.agents/` cambió desde la última copia, refresca la copia local con la subsección
   siguiente. Aquí sí se sobrescribe: no uses `--update=none` (eso es solo para instalar).

### Refrescar la copia local

La copia del paso 2 no sobrescribe, así que al editar el producto (`template/.agents/`) la copia
local `.agents/` se queda vieja. `/sdd-estructura` (chequeo E19) lo detecta y no lo corrige solo:
propone este refresco y espera confirmación.

Esto es solo el clon en modo mantenimiento. No actualiza un proyecto externo y no usa el
procedimiento de actualización de proyectos.

- Sobrescribe en `.agents/` los archivos que también están en `template/.agents/`.
- Al terminar restaura `napkin.md` del clon: es conocimiento versionado, no del producto.
- No borra archivos que solo existen en `.agents/`.
- No toca `AGENTS.md`, `CLAUDE.md`, `MEMORY.md`, `.docs/`, `PREFERENCIAS.md` ni `.git/info/exclude`
  (las exclusiones de arriba se quedan como están).
- El agente muestra los comandos y los ejecuta solo con confirmación.

```sh
# macOS / Linux — sí sobrescribe; al final devuelve el napkin del clon
napkin_tmp=$(mktemp)
cp -a .agents/napkin.md "$napkin_tmp"
cp -R template/.agents/. .agents/
cp -a "$napkin_tmp" .agents/napkin.md
rm -f "$napkin_tmp"
```

```powershell
# Windows — sí sobrescribe; al final devuelve el napkin del clon
Copy-Item .agents\napkin.md $env:TEMP\napkin.md.kit -Force
Get-ChildItem -Force template\.agents | ForEach-Object {
  Copy-Item $_.FullName -Destination .agents\ -Recurse -Force
}
Copy-Item $env:TEMP\napkin.md.kit .agents\napkin.md -Force
Remove-Item $env:TEMP\napkin.md.kit
```

Después, `diff -rq template/.agents .agents` no debe mostrar archivos de `template/.agents/`
distintos de su copia, salvo `napkin.md`. Lo que solo está en `.agents/` sigue en el `diff`: no se
borra en este paso.

Lo que cambie en `AGENTS.md`, `MEMORY.md`, `.docs/` o `napkin.md` se commitea como cualquier otro
cambio del kit (skill `git-commit`, con confirmación). Las decisiones se registran como ADR en
`.docs/004_adr/` con la skill `adr`.
