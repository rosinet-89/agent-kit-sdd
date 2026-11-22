# Enlaces hacia `.agents/` (verificación que hace el agente)

`.agents/` es la fuente canónica de agentes, comandos y skills. Cada herramienta lee su propia
carpeta, que debe **apuntar** a `.agents/` para que exista una sola copia. Este archivo
describe QUÉ debe existir; **CÓMO** crearlo lo decide el entorno (sistema operativo, shell,
permisos, herramienta). No hay script: no se sabe qué shell ni qué dependencias tiene cada
entorno de desarrollo.

## Chequeo de arranque

Qué lee: esta sección. Si el punto 1 lo omite, nada más. Si no, también la tabla de «Estados de enlace»,
«Estado «visto»» y, de «Tabla de enlaces», el encabezado y las filas de las herramientas declaradas en
«visto». Nada más del archivo.

Lo ejecuta el paso 7 de «Cada sesión» de `AGENTS.md`. Es de solo lectura: no crea, no corrige y no
escribe «visto». Se hace una vez por sesión, aunque la persona posponga el aviso.

1. Si falta `.agents/.kit-version` y existe `template/` (clon del kit): se omite, sin aviso. Sin
   `template/`, sigue y avisa como «todo nuevo».
2. Lee «visto». Ausente o corrupto = todo nuevo.
3. Calcula las diferencias:
   - Versión distinta de `.agents/.kit-version` (incluye retroceso).
   - Piezas nuevas o retiradas: compara las entradas de primer nivel de `agents/`, `commands/` y
     `skills/` con las líneas `pieza:`.
   - Solo para las herramientas declaradas: filas con estado `FALTA`, `DIFERENTE` u `OCUPADO` sin
     `excepcion:`. Las demás filas se ignoran; una ruta en `copia:` cuenta como `OK (copia)`.
   - Carpeta de agentes por archivo: basta que cada rol tenga su entrada (si no, `FALTA`); la validez
     del adaptador la juzgan `/sdd-enlaces` y E3/E23.
   - Sin «visto» (clon nuevo): todo cuenta como nuevo y no hay herramientas declaradas.
4. Sin diferencias, no digas nada. Con diferencias, muestra un aviso de 3 líneas como máximo,
   sin leer changelog: línea 1, versión y piezas (hasta 5, nuevas primero, luego «y X más»); línea 2,
   enlaces pendientes (siempre aquí, nunca en la 1; con la configuración guiada del punto 5, si aplica)
   y recomendación de `/sdd-enlaces` (no `/sdd-init`); línea 3, pasos mínimos (abre sesión,
   `/sdd-enlaces`, comprueba con `/sdd-estructura`; detalle: `ACTUALIZAR.md` del kit).
   El aviso no menciona réplicas ni ignorados versionados.
5. Configuración guiada: si falta `.agents/local/configuracion.md`, o su primera línea `version: X.Y.Z`
   difiere de `.agents/.kit-version` y al local le falta un paso, lo dice dentro de la línea 2 del aviso
   (junto a los enlaces pendientes, con la sugerencia de `/sdd-config`), sin pasar de 3 líneas. Falta
   un paso si su nombre fijo no está como sección `##` en el local; los nombres fijos son la lista de
   `.agents/configuracion.ejemplo.md` › «Formato del archivo local». Lee solo la primera línea del
   archivo local, y las secciones `##` del local y esa lista solo si la versión difiere. Mientras tanto
   rigen los valores sugeridos. Si es lo único pendiente, el aviso es solo esa línea.

Ejemplo:

```
El kit cambió de 0.5.0 a 0.6.0: 3 piezas nuevas (commands/sdd-enlaces.md, ...) y 1 retirada (skills/x).
Faltan 2 enlaces de claude: corre `/sdd-enlaces` para crearlos con tu confirmación. No corras `/sdd-init`.
Mínimo: abre sesión, `/sdd-enlaces`, comprueba con `/sdd-estructura`; detalle: ACTUALIZAR.md del kit.
```

Variantes: clon nuevo («Clon nuevo: elige tus herramientas con `/sdd-enlaces`»); solo enlaces (sin
línea de versión: el aviso queda en 2 líneas, enlaces pendientes y pasos mínimos).

## Estados de enlace

| Estado | Significa |
|---|---|
| `OK` | El enlace existe y apunta al destino esperado |
| `OK (copia)` | Es una copia real registrada con `copia:` en «visto» (no se sincroniza) |
| `FALTA` | La ruta no existe |
| `DIFERENTE` | Es un enlace, pero apunta a otro destino |
| `OCUPADO` | Existe como carpeta o archivo real (no es un enlace) y no figura en `copia:` ni en `excepcion:` |

Para la fila de carpetas por archivo, `OK` = cada rol de `.agents/agents/` tiene en la carpeta un enlace
que resuelve a su archivo o un adaptador que cumple la definición de «Carpeta de agentes por archivo» (en OpenCode, además con el `mode`
indicado); `FALTA` si algún rol no está.

Para la fila de adaptadores de OpenCode, `OK` = el archivo existe con el `mode` indicado; `FALTA` si no.

## Estado «visto»

Archivo local `.agents/local/kit-visto` (ignorado por Git; ver `.gitignore`). Registra qué kit y qué
enlaces vio la persona la última vez que `/sdd-enlaces` terminó bien. **Solo `/sdd-enlaces` lo
escribe**; el arranque y `/sdd-estructura` solo lo leen.

Texto plano UTF-8 sin BOM, una línea por dato (ejemplo):

```
version: 0.6.0
herramientas: claude, cursor
excepcion: .cursor/skills
copia: .gemini/commands
pieza: agents/coordinator.md
pieza: commands/sdd-spec.md
pieza: skills/sdd
```

- Prefijos válidos: `version`, `herramientas`, `excepcion`, `copia`, `pieza`. Orden: `version`,
  `herramientas`, `excepcion*`, `copia*`, `pieza*`.
- `version`: `X.Y.Z`, la de `.agents/.kit-version`. Nunca `ninguna`: sin versión legible no se escribe.
- `herramientas`: lista separada por comas de `claude`, `cursor`, `gemini`, `github`, `opencode`. Nunca
  vacía: sin herramientas no se escribe.
- `excepcion`: ruta de enlace de la tabla que la persona declaró intencional (OCUPADO aceptado).
- `copia`: ruta de enlace de la tabla que `/sdd-enlaces` creó como copia. Se descarta si la ruta pasó a
  ser un enlace.
- `pieza`: ruta relativa a `.agents/` de cada entrada de primer nivel de `agents/`, `commands/` y
  `skills/`.
- Corrupto = inexistente: falta `version` o `herramientas`, `version` inválida, o una línea con un
  prefijo distinto de los cinco. Solo se compara texto; nada de «visto» se ejecuta.
- Se arma el contenido completo y se escribe de una vez, al terminar con éxito.

## Qué hace el agente

El arranque (paso 7 de "Cada sesión" en `AGENTS.md`) sigue «Chequeo de arranque». Los pasos 3 a 8 los ejecuta
`/sdd-enlaces` con confirmación; el arranque nunca los aplica.

1. Revisa cada fila de la tabla y clasifica su estado (ver «Estados de enlace»).
2. Si todo está `OK`, no dice nada.
3. Si hay `FALTA`, `DIFERENTE` u `OCUPADO`, `/sdd-enlaces` muestra la tabla de lo pendiente (ruta del
   enlace y destino) y propone crear o corregir los enlaces.
4. Para crearlos, detecta el entorno (sistema operativo, shell, permisos) y **propone** el
   mecanismo adecuado, por ejemplo enlace simbólico, junction en Windows, o como último recurso una
   copia (avisando que se desactualiza). Muestra el comando exacto y no ejecuta nada sin que el
   usuario lo confirme.
5. `OCUPADO` nunca se borra ni se mueve: se informa y decide el usuario. Si dice que es intencional,
   se registra como `excepcion:` en «visto».
6. Pregunta solo por las herramientas que el usuario use. Las filas que no apliquen se omiten
   (por ejemplo, si esa versión de la herramienta ya lee `.claude/`).
7. Regla que `/sdd-enlaces` ejecuta tras confirmar: archivos **versionados que el `.gitignore` ya
   ignora** (`git ls-files -ci --exclude-standard`). Si hay, los lista y **propone** sacarlos del
   índice con `git rm --cached <rutas>` (los archivos se conservan en disco) y un commit aparte con
   la skill `git-commit`. No lo ejecuta sin confirmación.
   Avisa que, al hacer `pull`, el resto del equipo verá esos archivos borrados en su copia. Una regla
   de `.gitignore` no afecta a los archivos que Git ya rastrea.
8. Regla que `/sdd-enlaces` ejecuta tras confirmar: **réplicas de skills, agentes y comandos**
   (cuando un dev prueba o agrega una nueva):
   - **Huérfana:** existe en una carpeta de herramienta (`.claude/skills/<n>/`, `.cursor/agents/<n>.md`…)
     y no en `.agents/`. Propón **moverla** a `.agents/<tipo>/<n>` (la copia canónica es una sola) y dejar
     en la carpeta de la herramienta un enlace a ella.
   - **Faltante:** existe en `.agents/` y una carpeta de herramienta que es real (no un enlace a toda la
     carpeta) no la tiene. Propón un enlace por entrada (`../../.agents/skills/<n>`); si el SO no permite
     enlaces, una copia avisando que se desactualiza.
   - Si la carpeta de la herramienta ya es un enlace a `.agents/<tipo>`, no hay nada que replicar: lo
     nuevo aparece solo.
   - **En prueba:** si el dev solo la está probando, puede quedarse local. Propón excluirla en
     `.git/info/exclude` (ruta anclada con `/`) y avisa que no llegará al resto del equipo.
   - Al adoptarla de forma definitiva: fila en la tabla de skills de `AGENTS.md` si es de dominio, y
     `.agents/README.md` › "Agregar una skill".
   - Nunca borra ni mueve nada sin confirmación; muestra el comando exacto.
   El arranque ya no detecta réplicas ni ignorados versionados: los pasos 7 y 8 corren desde
   `/sdd-enlaces` (y E2/E3 de `/sdd-estructura` los reportan). **Cada vez que el agente crea o agrega**
   una skill, agente o comando, al terminar sugiere `/sdd-enlaces`.

## Tabla de enlaces

El destino es relativo a la carpeta donde vive el enlace. Las carpetas padre (`.claude/`,
`.cursor/`, `.gemini/`, `.github/`) se crean si no existen.

| Herramienta | Enlace | Destino | Obligatorio |
|---|---|---|---|
| Claude Code | `.claude/agents` | `../.agents/agents` (enlace de carpeta; por archivo solo con modelos propios, ver «Carpeta de agentes por archivo») | Si usa Claude Code |
| Claude Code | `.claude/commands` | `../.agents/commands` | Si usa Claude Code |
| Claude Code | `.claude/skills` | `../.agents/skills` | Si usa Claude Code |
| Cursor | `.cursor/agents/` | carpeta por archivo (ver «Carpeta de agentes por archivo»; no es enlace de carpeta) | Si usa Cursor |
| Cursor | `.cursor/commands` | `../.agents/commands` | Si usa Cursor |
| Cursor | `.cursor/skills` | `../.agents/skills` | Si usa Cursor |
| Gemini | `.gemini/agents/` | carpeta por archivo (ver «Carpeta de agentes por archivo»; no es enlace de carpeta) | Si usa Gemini |
| Gemini | `.gemini/commands` | `../.agents/commands` | Si usa Gemini |
| Gemini | `.gemini/skills` | `../.agents/skills` | Si usa Gemini |
| GitHub | `.github/agents/` | carpeta por archivo (ver «Carpeta de agentes por archivo»; no es enlace de carpeta) | Si usa Copilot |
| GitHub | `.github/commands` | `../.agents/commands` | Si usa Copilot |
| GitHub | `.github/skills` | `../.agents/skills` | Si usa Copilot |
| GitHub | `.github/copilot-instructions.md` | `../AGENTS.md` | Si usa Copilot |
| OpenCode | `.opencode/skills` | `../.agents/skills` | Si usa OpenCode |
| OpenCode | `.opencode/commands` | `../.agents/commands` | Si usa OpenCode |
| OpenCode | `.opencode/agents/` | carpeta por archivo (ver «Carpeta de agentes por archivo»; no es enlace de carpeta) | Si usa OpenCode |

### Carpeta de agentes por archivo

Fuente única de la definición (ADR 0030). Es la carpeta real de agentes de una herramienta, no
versionada, donde cada rol es una de estas dos cosas:

- un enlace relativo `../../.agents/agents/<rol>.md`; o
- un **adaptador de rol**: `<rol>.md` con el mismo frontmatter que `.agents/agents/<rol>.md` salvo
  `model:`, los campos que la herramienta no admite y los que exige (como `mode` en OpenCode), y un
  cuerpo que solo remite a `.agents/agents/<rol>.md` (el rol lo lee al arrancar).

Cuándo se usa:

- En toda herramienta distinta de Claude Code, siempre por archivo. El `model:` del adaptador es el que
  el dev configuró en `.agents/local/configuracion.md` › `## Modelos` o, sin él, el sugerido de su fila
  en `.agents/configuracion.ejemplo.md`; sin fila, `inherit`.
- En Claude Code, solo cuando un dev elige un modelo distinto del de `.agents/agents/`: ese rol lleva
  adaptador y los demás, enlace. Si ya no hay ningún modelo propio, la carpeta vuelve a ser el enlace
  de carpeta de la tabla.
- Nunca se escribe en `.agents/agents/`.

Adaptador viejo: su frontmatter difiere del canónico fuera de lo permitido (`model:`, campos no
admitidos o exigidos por la herramienta). Se regenera conservando su `model:`.

Para la fila de carpetas por archivo: (ver «Estados de enlace»).

### OpenCode

OpenCode lee `AGENTS.md` y descubre `.agents/skills` por sí solo: las skills funcionan sin enlace. Aun así se
crean `.opencode/skills` y `.opencode/commands` para que use su carpeta propia y no dependa de la
compatibilidad con `.agents/` y `.claude/`.

`.opencode/agents` es una carpeta por archivo (ver «Carpeta de agentes por archivo»), no un enlace a
`.agents/agents`: el frontmatter de Claude (`tools: Agent(...)`, `omitClaudeMd`) no es válido en OpenCode
V2 y el archivo se descarta con `agent file skipped … at ["tools"]`. Su adaptador es un caso de esa
definición: remite al archivo canónico y conserva lo propio de OpenCode.

```md
---
description: <la misma description del archivo canónico de .agents/agents/>
model: <el de la definición>
mode: all          # coordinator
# mode: subagent   # planner, implementer, reviewer
color: <color del rol en la skill formato-respuesta>
permissions:
  - action: skill
    resource: "*"
    effect: allow
---

Actúas como <rol>. Lee el archivo canónico de `.agents/agents/` y sigue su «Qué lee», «Qué hace» y «Qué no hace».
Cumple `AGENTS.md` › Guardrails sin excepción.
```

- `coordinator`: `mode: all` y `subagent` permitido solo para `planner`, `implementer` y `reviewer`.
- `planner` e `implementer`: `mode: subagent`.
- `reviewer`: `mode: subagent`, `edit` denegado (solo lectura) y `subagent` denegado.
- El permiso `skill` es lo que deja al subagente cargar las skills; sin él no las ve.
- `claudio` no se adapta: envuelve el CLI `claude`, y para E3 su ausencia en `.opencode/agents/` no es réplica
  faltante.
- Para la fila de adaptadores de OpenCode: (ver «Estados de enlace»).

### Kiro

Lectura del 2026-10-25: https://kiro.dev/docs/custom-agents/configuration-reference/
y https://kiro.dev/docs/custom-agents/. Kiro usa otro formato. Los agentes son
archivos de configuración, JSON o Markdown, en `.kiro/agents/[name].json` o
`.kiro/agents/[name].md` del workspace y en `~/.kiro/agents/` global. En Markdown
el frontmatter es la configuración y el cuerpo es el prompt. El del workspace se
versiona y gana si el nombre coincide. El comando de actualización recorre
`.kiro/agents/` y `~/.kiro/agents/`. Las skills se citan con
`skill://.kiro/skills/**/SKILL.md`. Por defecto el agente hereda steering, skills
y `AGENTS.md`. Ese almacén no es un enlace de `agents`, `commands` y `skills`
hacia `.agents/`.

## Notas para el usuario

- En Windows, los enlaces simbólicos requieren Modo desarrollador o permisos de administrador; si no,
  el agente propondrá una alternativa (junction o copia).
- Si una de esas rutas ya existe como carpeta real con contenido (estado `OCUPADO`), revisa su
  contenido y muévelo a `.agents/` o bórralo tú mismo antes de crear el enlace.
- Si cambia alguna ruta de esta tabla, actualiza este archivo; es la única fuente de la lista.
- Copilot lee `.github/copilot-instructions.md`: se enlaza a `AGENTS.md` en vez de mantener una
  copia escrita a mano. Si ya existe como archivo real (estado `OCUPADO`), pasa su contenido útil a
  `AGENTS.md` antes de reemplazarlo; lo mismo con `.github/agents/*.agent.md` heredados.
- El `.gitignore` del kit ignora `.claude/`, `.cursor/`, `.github/`, `.gemini/` y `.opencode/`: los enlaces son
  por entorno y no se versionan; cada desarrollador los siembra en su copia con este archivo. Ojo: eso también ignora la configuración propia de la herramienta
  (por ejemplo `.claude/settings.json`) y, en `.github/`, workflows y plantillas. Si tu proyecto los
  versiona, quita esa línea o acota la regla (por ejemplo `.github/agents`, `.github/commands`,
  `.github/skills`).
