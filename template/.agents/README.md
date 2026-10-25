# Layout de agentes

**Raíz canónica:** `.agents/`. Skills, agentes, comandos y napkin viven **una sola vez** aquí.
Las reglas de sesión, la tabla de skills y Git viven en `AGENTS.md` (raíz). Si una copia
diverge, gana este árbol.

---

## Árbol

```
AGENTS.md                         # instrucciones para todos los agentes (fuente de verdad)
CLAUDE.md                         # solo "@AGENTS.md"
MEMORY.md                         # decisiones y trampas vigentes (los agentes solo proponen)
PREFERENCIAS.md                   # directivas personales de cada usuario (gitignored; plantilla: .agents/preferencias.ejemplo.md)
.agents/
  README.md                       # este archivo: layout + cómo agregar skills/agentes/comandos
  napkin.md                       # runbook vivo; todos lo leen cada sesión
  .kit-version                    # marcador de versión del procedimiento; versionable; no es del kit
  .kit-mantener                   # registro de «mantener» del procedimiento; versionable; no es del kit
  sync-agents.md                  # enlaces de cada herramienta hacia .agents/
  validar-estructura.md           # checklist de /sdd-estructura (estructura del kit)
  agents/<rol>.md                 # roles (coordinator, planner, implementer, reviewer)
  commands/<nombre>.md            # comandos /sdd* (flujo de la skill sdd)
  skills/<nombre>/SKILL.md        # skills (cuerpo canónico)
.docs/
  001_constitucion/constitution.md
  002_specs/README.md             # índice de planes
  002_specs/<NNNN>_<Feature>/     # un plan (01_spec … 06_flujo); 4 dígitos en un proyecto
  003_deuda/README.md             # índice de deuda técnica
  003_deuda/<NNNN>_<Nombre>.md    # una deuda; mismo ancho que las specs
  004_adr/README.md               # índice de ADR (skill adr)
  004_adr/<NNNN>_<Titulo>.md      # un ADR; mismo ancho de 4 dígitos en un proyecto
```

## Qué carpeta lee cada herramienta

| Herramienta | Lee | Cómo llega a `.agents/` |
|---|---|---|
| Claude Code | `CLAUDE.md`, `.claude/` (`agents/`, `commands/`, `skills/`) | `CLAUDE.md` importa `AGENTS.md`; enlaces de `.agents/sync-agents.md` |
| Cursor | `AGENTS.md`, `.cursor/` (`rules/`, `agents/`, `commands/`, `skills/`) | enlaces de `.agents/sync-agents.md` |
| GitHub Copilot | `.github/copilot-instructions.md`, `.github/` (`agents/`, `commands/`, `skills/`) | enlaces de `.agents/sync-agents.md` (el archivo apunta a `AGENTS.md`; `.github/` no se versiona, cada quien lo siembra) |
| Otras | su propia carpeta | agregar su fila en `.agents/sync-agents.md` |

Las carpetas de cada herramienta no guardan cuerpos de skills ni de agentes: solo enlaces hacia
`.agents/` y su configuración propia.

---

## Agregar una skill

1. Crear **solo** `.agents/skills/<nombre>/SKILL.md`, con frontmatter:
   - `name`: igual al nombre de la carpeta (minúsculas, guiones, sin acentos).
   - `description`: qué hace **y** cuándo activarla (disparadores concretos). Máximo 1024 caracteres;
     las listas largas de disparadores van en el cuerpo.
2. Si es de dominio del proyecto, agregar su fila en la tabla de `AGENTS.md` (solo la ruta, no un resumen).
3. Napkin: solo si es una directiva recurrente, nunca el texto de la skill.
4. Verificar los enlaces según `.agents/sync-agents.md` y **sugerir replicarla** en las carpetas de
   cada herramienta (paso 8 de ese archivo). Si es una prueba local, proponer excluirla en
   `.git/info/exclude` en vez de versionarla.

## Agregar un agente (rol)

1. Crear `.agents/agents/<rol>.md` con frontmatter `name` y `description` (cuándo delegarle trabajo).
2. Si cambia el proceso o los handoffs, documentarlo en `AGENTS.md` (sección Roles).
3. Verificar los enlaces según `.agents/sync-agents.md` y sugerir replicarlo (paso 8).

## Agregar un comando

1. Crear `.agents/commands/<nombre>.md` con frontmatter `description` y `argument-hint`. En el
   cuerpo, `$ARGUMENTS` es todo el texto y `$1`, `$2`… los argumentos por posición.
2. El comando dice qué skill sigue y en qué agente delega; no copia el cuerpo de la skill.
3. Si es parte del flujo `sdd`, agregarlo a la tabla de fases de `.agents/skills/sdd/SKILL.md` y a
   "Roles y comandos" de `AGENTS.md`.
4. Verificar los enlaces según `.agents/sync-agents.md` y sugerir replicarlo (paso 8).

## Validar la estructura

Pídele al agente `/sdd-estructura` (o "valida la estructura del kit"): sigue
`.agents/validar-estructura.md`, revisa enlaces, réplicas, skills, agentes, comandos, índices y
codificación, y propone las correcciones sin aplicarlas.

## Anti-patrones

- Una skill completa en `.claude/skills/` o `.cursor/` y otra igual en `.agents/skills/`.
- Pegar el cuerpo de una skill dentro de `AGENTS.md` o de `CLAUDE.md`.
- Napkin fuera de `.agents/napkin.md`.
- IDs, rutas o datos del proyecto repetidos en varios README en vez de una sola fuente.
