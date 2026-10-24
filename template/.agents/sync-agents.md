# Enlaces hacia `.agents/` (verificación que hace el agente)

`.agents/` es la fuente canónica de agentes, comandos y skills. Cada herramienta lee su propia
carpeta, que debe **apuntar** a `.agents/` para que exista una sola copia. Este archivo
describe QUÉ debe existir; **CÓMO** crearlo lo decide el entorno (sistema operativo, shell,
permisos, herramienta). No hay script: no se sabe qué shell ni qué dependencias tiene cada
entorno de desarrollo.

## Qué hace el agente

Al inicio de la sesión (paso 7 de "Cada sesión" en `AGENTS.md`), el agente:

1. Revisa cada fila de la tabla y clasifica su estado:

| Estado | Significa |
|---|---|
| `OK` | El enlace existe y apunta al destino esperado |
| `FALTA` | La ruta no existe |
| `DIFERENTE` | Es un enlace, pero apunta a otro destino |
| `OCUPADO` | Existe como carpeta o archivo real (no es un enlace) |

2. Si todo está `OK`, no dice nada.
3. Si hay `FALTA`, `DIFERENTE` u `OCUPADO`, **sugiere** al usuario crear o corregir los enlaces,
   mostrando la tabla de lo pendiente (ruta del enlace y destino).
4. Para crearlos, el agente detecta el entorno (sistema operativo, shell, permisos) y **propone** el
   mecanismo adecuado, por ejemplo enlace simbólico, junction en Windows, o como último recurso una
   copia (avisando que se desactualiza). Muestra el comando exacto y no ejecuta nada sin que el
   usuario lo confirme.
5. `OCUPADO` nunca se borra ni se mueve: se informa y decide el usuario.
6. Pregunta solo por las herramientas que el usuario use. Las filas que no apliquen se omiten
   (por ejemplo, si esa versión de la herramienta ya lee `.claude/`).
7. Revisa si hay archivos **versionados que el `.gitignore` ya ignora** (`git ls-files -ci --exclude-standard`).
   Si hay, los lista y **propone** sacarlos del índice con `git rm --cached <rutas>` (los archivos se
   conservan en disco) y un commit aparte con la skill `git-commit`. No lo ejecuta sin confirmación.
   Avisa que, al hacer `pull`, el resto del equipo verá esos archivos borrados en su copia. Una regla
   de `.gitignore` no afecta a los archivos que Git ya rastrea.
8. Revisa **réplicas de skills, agentes y comandos** (cuando un dev prueba o agrega una nueva):
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
   Se ejecuta al inicio de la sesión (con el paso 7 de `AGENTS.md`) y **cada vez que el agente crea o
   agrega** una skill, agente o comando: al terminar, sugiere replicarlo según este paso.

## Tabla de enlaces

El destino es relativo a la carpeta donde vive el enlace. Las carpetas padre (`.claude/`,
`.cursor/`, `.gemini/`, `.github/`) se crean si no existen.

| Herramienta | Enlace | Destino | Obligatorio |
|---|---|---|---|
| Claude Code | `.claude/agents` | `../.agents/agents` | Si usa Claude Code |
| Claude Code | `.claude/commands` | `../.agents/commands` | Si usa Claude Code |
| Claude Code | `.claude/skills` | `../.agents/skills` | Si usa Claude Code |
| Cursor | `.cursor/agents` | `../.agents/agents` | Si usa Cursor |
| Cursor | `.cursor/commands` | `../.agents/commands` | Si usa Cursor |
| Cursor | `.cursor/skills` | `../.agents/skills` | Si usa Cursor |
| Gemini | `.gemini/agents` | `../.agents/agents` | Si usa Gemini |
| Gemini | `.gemini/commands` | `../.agents/commands` | Si usa Gemini |
| Gemini | `.gemini/skills` | `../.agents/skills` | Si usa Gemini |
| GitHub | `.github/agents` | `../.agents/agents` | Si usa Copilot |
| GitHub | `.github/commands` | `../.agents/commands` | Si usa Copilot |
| GitHub | `.github/skills` | `../.agents/skills` | Si usa Copilot |
| GitHub | `.github/copilot-instructions.md` | `../AGENTS.md` | Si usa Copilot |

## Notas para el usuario

- En Windows, los enlaces simbólicos requieren Modo desarrollador o permisos de administrador; si no,
  el agente propondrá una alternativa (junction o copia).
- Si una de esas rutas ya existe como carpeta real con contenido (estado `OCUPADO`), revisa su
  contenido y muévelo a `.agents/` o bórralo tú mismo antes de crear el enlace.
- Si cambia alguna ruta de esta tabla, actualiza este archivo; es la única fuente de la lista.
- Copilot lee `.github/copilot-instructions.md`: se enlaza a `AGENTS.md` en vez de mantener una
  copia escrita a mano. Si ya existe como archivo real (estado `OCUPADO`), pasa su contenido útil a
  `AGENTS.md` antes de reemplazarlo; lo mismo con `.github/agents/*.agent.md` heredados.
- El `.gitignore` del kit ignora `.claude/`, `.cursor/`, `.github/` y `.gemini/`: los enlaces son
  por entorno y no se versionan; cada desarrollador los siembra en su copia con este archivo. Ojo: eso también ignora la configuración propia de la herramienta
  (por ejemplo `.claude/settings.json`) y, en `.github/`, workflows y plantillas. Si tu proyecto los
  versiona, quita esa línea o acota la regla (por ejemplo `.github/agents`, `.github/commands`,
  `.github/skills`).
