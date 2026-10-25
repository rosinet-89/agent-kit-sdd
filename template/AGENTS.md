# <<NOMBRE_PROYECTO>> – Instrucciones para agentes

<!-- SDD-INIT-PENDIENTE: este marcador lo quita /sdd-init al terminar la personalización. -->

Conocimiento canónico en **`.agents/`** (agentes, skills, notas) y **`.docs/`**
(constitución, planes, deuda). No duplicar.

## Cada sesión (obligatorio)
1. Si este archivo contiene `SDD-INIT-PENDIENTE`: el proyecto aún no está personalizado. Dilo en
   una línea y sugiere correr `/sdd-init`. No ejecutes otros comandos `/sdd*` hasta entonces.
2. Leer `.agents/napkin.md` y aplicarlo en silencio.
3. Leer `MEMORY.md` (decisiones y trampas vigentes).
4. Estructura de agentes y skills nuevas: `.agents/README.md`.
5. Leer `.docs/001_constitucion/constitution.md` antes de cambiar código.
6. Si la petición es análisis o desarrollo nuevo, o toca un módulo, pantalla o tabla: seguir
   "Antecedentes" (abajo) ANTES de buscar o diseñar.
7. Verificar los enlaces hacia `.agents/` según `.agents/sync-agents.md`. Si falta alguno,
   sugerir crearlo (sin ejecutar nada sin confirmación). Revisa también si hay skills, agentes o
   comandos en una carpeta de herramienta que no estén en `.agents/` (o al revés) y sugiere
   replicarlos (`sync-agents.md` paso 8). Si todo está bien, no decir nada.
8. Revisar que no haya prefijos repetidos en `.docs/002_specs/`, `.docs/003_deuda/` ni
   `.docs/004_adr/` (ver "Formato y numeración"). Si hay, avisar y proponer el cambio; si no, no decir nada.
9. Cerrar cada respuesta con la línea "quién responde" de `.agents/skills/formato-respuesta/SKILL.md`
   (al final o justo antes de pedir una decisión). Solo se quita si el usuario lo pide.
10. Leer `PREFERENCIAS.md` (raíz, no versionado: directivas personales de cada usuario según su
    entorno) y aplicarlo en silencio. Si no existe, créalo copiando `.agents/preferencias.ejemplo.md`
    sin las entradas de ejemplo y sigue; no lo pidas ni lo menciones. Las preferencias nuevas que el
    usuario exprese sobre su entorno ("yo uso Zed", "no me limpies el chat") se proponen aquí, no en
    el napkin ni en `MEMORY.md`.

## Antecedentes (obligatorio en análisis y desarrollo nuevo)
Índice de planes: `.docs/002_specs/README.md`.
1. Ábrelo y busca el módulo, pantalla, tabla o palabra clave de la petición.
2. Si hay coincidencia: dile al usuario qué carpeta es y lee SOLO esa (primero su `PUNTO DE RETOMO`).
3. Si no hay: di "sin antecedentes en el índice" y confírmalo con un `grep` antes de afirmarlo.
4. No recorras las carpetas de plan ni asumas que un proceso es nuevo, o que ya está resuelto, sin este paso.
5. Si el índice y la carpeta discrepan, manda la carpeta: avisa para corregir el índice.
6. Carpeta nueva o cambio de estatus = actualizar su fila. Lo hacen `/sdd-spec` y `/sdd-cierre`.

## Deuda técnica
Índice: `.docs/003_deuda/README.md` (un `NNNN_<Nombre>.md` por deuda, con estado, origen y qué la resolvió).
1. Al hacer "Antecedentes", busca también ahí deuda abierta del módulo, pantalla o tabla y dila al usuario.
2. Si durante un requerimiento algo se difiere (por decisión del usuario o por recorte de alcance), o aparece un hallazgo que no se atiende ahora, PROPÓN darlo de alta; no la crees sin el visto bueno del usuario.
3. Al cerrar un requerimiento, indica qué deudas resolvió (estado `Resuelta` + "Atendida por" + "Cómo se atendió") y cuáles dejó. Lo hace `/sdd-cierre`.
4. Una deuda nunca se borra: pasa a `Resuelta` o `Descartada` (con motivo).

## Formato y numeración
Reglas completas en `.docs/002_specs/README.md`.
- Toda carpeta de plan nace en formato SDD (skill `sdd`): lleva `01_spec.md`.
- Las carpetas de sección de `.docs/` (`001_constitucion`, `002_specs`, `003_deuda`, `004_adr`) se quedan en 3 dígitos.
- En un proyecto, cada spec, deuda y ADR lleva un prefijo de 4 dígitos (`0001`). No se repite dentro de su carpeta. Si aparece repetido (por ejemplo tras un `pull`), conserva el número el que ya está en la rama remota; propón para el otro el siguiente libre y no renombres sin confirmación. El ancho completo está en `.docs/002_specs/README.md` › "Numeración única".

## Skills de proyecto
Antes de responder, lee y aplica la skill del tema. Si el tema cruza varias, lee todas.

| Tema | Skill |
|------|-------|
| Saber si un proceso ya se trabajó en un plan anterior | `.docs/002_specs/README.md` (ver "Antecedentes") |
| Análisis o desarrollo nuevo (spec, impacto, plan, tareas, avance con tiempos, flujo) | `.agents/skills/sdd/SKILL.md` |
| Runbook vivo del repo (se aplica cada sesión) | `.agents/skills/napkin/SKILL.md` |
| Registrar cambios en Git | `.agents/skills/git-commit/SKILL.md` |
| Línea "quién responde" al cierre de cada respuesta (rol con color, skill, contexto, desvíos) | `.agents/skills/formato-respuesta/SKILL.md` |
| Decisión de arquitectura con alternativas (índice en `.docs/004_adr/README.md`) | `.agents/skills/adr/SKILL.md` |
| Scripts SQL de un feature (`sql/`, rollback, idempotencia) | `.agents/skills/migracion-bd/SKILL.md` |
| Secretos antes del commit y checklist de seguridad básica | `.agents/skills/seguridad-basica/SKILL.md` |
<!-- Las skills de dominio del proyecto se agregan aquí, una fila por skill (solo la ruta). -->

Notas:
- `sdd` exige `.docs/002_specs/<NNNN>_<Feature>/` (NNNN = consecutivo de 4 dígitos, `0001`) con `01_spec.md`, `02_analisis_impacto.md`, `03_plan.md`, `04_tasks.md`, `05_avance.md` (tiempos reales), `06_flujo.md` y `sql/` si hay cambios de datos. Contrato de avance y tiempos: `.agents/skills/sdd/avance-y-tiempos.md`.

## Roles y comandos
Agentes en `.agents/agents/`: coordinator (dirige el flujo y lleva `05_avance.md`), planner (documentos), implementer (código), reviewer (QA y validación).
Comandos en `.agents/commands/`: `/sdd-init` (una sola vez al instalar el kit), `/sdd <petición libre>` (entrada única: el coordinator decide la fase) y, para fases sueltas, `/sdd-spec`, `/sdd-clarify`, `/sdd-plan`, `/sdd-tasks`, `/sdd-implement`, `/sdd-validate`, `/sdd-flujo`, `/sdd-cierre`, `/sdd-change`, `/sdd-status`, `/sdd-constitucion` y `/sdd-estructura` (valida la estructura del kit: réplicas, enlaces, índices).
Claude Code: sesión dedicada con `claude --agent coordinator`.

## Qué lee cada rol
La sección "Cada sesión" es el arranque. Lo ejecuta la sesión principal. Un subagente no lo repite
(enlaces, prefijos, preferencias) y no se supone que la herramienta le inyecta este archivo. Lee
solo las secciones que su instrucción nombra:

| Rol | Secciones |
|---|---|
| coordinator | Ninguna para enrutar. Solo comprueba si este archivo contiene `SDD-INIT-PENDIENTE`. |
| planner | "Comandos" y, al planear, "Stack y convenciones de código". |
| implementer | "Comandos" y "Stack y convenciones de código". |
| reviewer | "Comandos" y "Stack y convenciones de código". |

## Git (regla obligatoria)
Al registrar cambios, lee y aplica `.agents/skills/git-commit/SKILL.md`.
- NUNCA `git commit` sin confirmación explícita del usuario.
- NUNCA `push`/`pull`/`fetch`/`sync`.
- Un commit por módulo; `git add <archivos>` explícito (nunca `.` ni `-A`).
- Mensajes en <<IDIOMA>>, imperativo, máx. 72 caracteres: `<tipo>(<módulo>): <descripción>`.

## Stack y convenciones de código
<!-- COMPLETAR (/sdd-init): lenguaje y versión, framework, base de datos, arquitectura en capas,
     convención de nombres, encoding de archivos, idioma de código/UI/docs. -->

## Comandos
<!-- COMPLETAR (/sdd-init). Los agentes leen esta sección para el checklist "Hecho cuando". -->
- Build: <completar>
- Test: <completar>
- Lint / sintaxis (por archivo tocado): <completar>
- Consulta de datos de solo lectura (cómo y desde dónde): <completar o "No aplica">

## Reglas de negocio y del sistema
<!-- COMPLETAR (/sdd-init): reglas que un agente no puede deducir del código y que no se cambian sin plan
     (flujos críticos, integraciones con layout fijo, sesión y seguridad, etc.). -->

## Reglas duras
- Nunca secretos ni credenciales en archivos versionados.
- Cambios fuera del plan aprobado se consultan, no se improvisan.
- Propón entradas para MEMORY.md; no las agregues sin aprobación.
- No reinstales el kit encima de un proyecto que ya lo tiene. El paso a una versión nueva usa el procedimiento de actualización del kit (documento ACTUALIZAR.md, junto al de instalación). La versión instalada está en el marcador .agents/.kit-version.
