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
  local/kit-visto                 # estado local de /sdd-enlaces (gitignored; no es del kit)
  agents/<rol>.md                 # roles (coordinator, planner, implementer, reviewer, tester, explore)
  entornos.ejemplo.md             # plantilla de entornos de ejecución (junto a preferencias.ejemplo.md)
  configuracion.ejemplo.md        # sugerencias del kit; se copian a .agents/local/configuracion.md (no versionado) con /sdd-config
  commands/<nombre>.md            # comandos /sdd* (flujo de la skill sdd; incluye sdd-config.md, asistente de configuración de cada dev)
  skills/<nombre>/SKILL.md        # skills (cuerpo canónico)
.docs/
  001_constitucion/constitution.md
  002_specs/README.md             # índice de planes
  002_specs/<prefijo>_<Feature>/   # un plan (01_spec … 06_flujo)
  003_deuda/README.md             # índice de deuda técnica
  003_deuda/<prefijo>_<Nombre>.md  # una deuda
  004_adr/README.md               # índice de ADR (skill adr)
  004_adr/<prefijo>_<Titulo>.md    # un ADR
```

## Qué carpeta lee cada herramienta

| Herramienta | Lee | Cómo llega a `.agents/` |
|---|---|---|
| Claude Code | `CLAUDE.md`, `.claude/` (`agents/`, `commands/`, `skills/`) | `CLAUDE.md` importa `AGENTS.md`; enlaces de `.agents/sync-agents.md` |
| Cursor | `AGENTS.md`, `.cursor/` (`rules/`, `agents/`, `commands/`, `skills/`) | enlaces de `.agents/sync-agents.md` |
| GitHub Copilot | `.github/copilot-instructions.md`, `.github/` (`agents/`, `commands/`, `skills/`) | enlaces de `.agents/sync-agents.md` (el archivo apunta a `AGENTS.md`; `.github/` no se versiona, cada quien lo siembra) |
| OpenCode | `AGENTS.md`, `.opencode/` (`agents/`, `commands/`, `skills/`) | `.opencode/skills` y `.opencode/commands` son enlaces; `.opencode/agents/` son adaptadores con `mode` (ver `.agents/sync-agents.md` › «OpenCode») |
| Otras | su propia carpeta | agregar su fila en `.agents/sync-agents.md` |

Las carpetas de cada herramienta no guardan cuerpos de skills ni de agentes: solo enlaces hacia
`.agents/` y su configuración propia.

---

## Roles y comandos
Agentes en `.agents/agents/`: coordinator (dirige el flujo y lleva `05_avance.md`), planner (documentos), implementer (código), reviewer (QA y validación), tester (ejecuta casos; Haiku) y explore (localiza; Haiku).
Comandos en `.agents/commands/`: `/sdd-init` (una sola vez al instalar el kit), `/sdd <petición libre>` (entrada única: el coordinator decide la fase) y, para fases sueltas, `/sdd-spec`, `/sdd-clarify`, `/sdd-plan`, `/sdd-tasks`, `/sdd-implement`, `/sdd-validate`, `/sdd-flujo`, `/sdd-cierre`, `/sdd-change`, `/sdd-status`, `/sdd-constitucion`, `/sdd-estructura` (valida la estructura del kit: réplicas, enlaces, índices; solo lectura), `/sdd-enlaces` (crea o corrige los enlaces y réplicas tras un pull, con tu confirmación) y `/sdd-config` (asistente de configuración de cada dev; se puede repetir).
Claude Code: sesión dedicada con `claude --agent coordinator`.
OpenCode: `/sdd <petición>` en la sesión principal; los roles viven en `.opencode/agents/` como adaptadores con `mode` y remiten a `.agents/agents/` (ver `.agents/sync-agents.md` › «OpenCode»).

## Agregar una skill

1. Crear **solo** `.agents/skills/<nombre>/SKILL.md`, con frontmatter:
   - `name`: igual al nombre de la carpeta (minúsculas, guiones, sin acentos).
   - `description`: qué hace **y** cuándo activarla (disparadores concretos). Máximo 1024 caracteres;
     las listas largas de disparadores van en el cuerpo.
2. Si es de dominio del proyecto, agregar su fila en la tabla de `AGENTS.md` (solo la ruta, no un resumen).
3. Napkin: solo si es una directiva recurrente, nunca el texto de la skill.
4. Verificar los enlaces según `.agents/sync-agents.md` y **sugerir `/sdd-enlaces`** para replicarla
   en las carpetas de cada herramienta. Si es una prueba local, proponer excluirla en
   `.git/info/exclude` en vez de versionarla.

## Agregar un agente (rol)

1. Crear `.agents/agents/<rol>.md` con frontmatter `name` y `description` (cuándo delegarle trabajo).
2. Si cambia el proceso o los handoffs, documentarlo en `AGENTS.md` (sección Roles).
3. Verificar los enlaces según `.agents/sync-agents.md` y sugerir `/sdd-enlaces`.

## Agregar un comando

1. Crear `.agents/commands/<nombre>.md` con frontmatter `description` y `argument-hint`. En el
   cuerpo, `$ARGUMENTS` es todo el texto y `$1`, `$2`… los argumentos por posición.
2. El comando dice qué skill sigue y en qué agente delega; no copia el cuerpo de la skill.
3. Si es parte del flujo `sdd`, agregarlo a la tabla de fases de `.agents/skills/sdd/SKILL.md` y a
   «Roles y comandos» de este archivo.
4. Verificar los enlaces según `.agents/sync-agents.md` y sugerir `/sdd-enlaces`.

## Validar la estructura

Pídele al agente `/sdd-estructura` (o "valida la estructura del kit"): sigue
`.agents/validar-estructura.md`, revisa enlaces, réplicas, skills, agentes, comandos, índices y
codificación, y propone las correcciones sin aplicarlas. Los enlaces y las réplicas se corrigen
con `/sdd-enlaces`, con tu confirmación.

## Qué va en `AGENTS.md`

- Siempre cargado: lo que se usa en cualquier turno o lo que los roles y `/sdd-init` buscan ahí por
  nombre: «Guardrails», «Cada sesión», «Antecedentes», «Deuda técnica», «Skills de proyecto», «Git»,
  «Stack y convenciones de código», «Comandos», «Reglas de negocio y del sistema» y «Reglas duras».
- Bajo demanda: lo que usa un solo rol, comando o momento vive en su archivo, y `AGENTS.md` deja un
  renglón que dice qué es, dónde está y cuándo leerlo.
- Los pasos de «Cada sesión» piden secciones, no archivos enteros, y los condicionales dicen en una
  línea cuándo aplican.

## Anti-patrones

- Una skill completa en `.claude/skills/` o `.cursor/` y otra igual en `.agents/skills/`.
- Pegar el cuerpo de una skill dentro de `AGENTS.md` o de `CLAUDE.md`.
- Napkin fuera de `.agents/napkin.md`.
- IDs, rutas o datos del proyecto repetidos en varios README en vez de una sola fuente.
