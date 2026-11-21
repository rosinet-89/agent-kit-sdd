# 04 — Tareas: Configuración guiada del kit
Leyenda: `[ ]` Pendiente · `[~]` En curso · `[x]` Hecho · `[!]` Bloqueado

Modo completo. Cambio al kit: solo Markdown. Build: No aplica. Consulta de datos: No aplica. No hay `sql/`. Lleva `07_casos_prueba.md` (ensayos en `mktemp -d`).

Decisiones ya aprobadas (2026-11-21 14:59; no se reabren): `03_plan.md` › §6, ADR 0029 y ADR 0030, y la fuente única de cada regla de `03_plan.md` › §1. Un paso que redacta una regla la escribe en su fuente; los demás pasos solo remiten a ella.

Ejecución: F0 y F5 los hace la sesión. F1 a F4, el `implementer` como subagente en `sonnet`, un paso por lanzamiento. Un paso toca como máximo dos archivos. `template/` no se edita después de F5.2.

**L (lint de `AGENTS.md` › Comandos)**, por cada archivo tocado:
- `file -i` da `charset=utf-8` o `us-ascii`.
- `grep -l $'^\xEF\xBB\xBF'` sale vacío.
- Las rutas citadas con comillas inversas existen (salvo `.agents/local/…`, que es de cada dev, y `/sdd-config` antes de F2.1).
- En comandos y agentes, el `name` o el nombre del archivo coinciden con su carpeta.
- En `template/`, no hay marcadores `<<…>>` salvo `<<NOMBRE_PROYECTO>>` e `<<IDIOMA>>`.

**P (protocolo de no pisar)**: antes de editar, `git diff HEAD -- <archivo>` sale vacío o solo con cambios de este feature.

Total: 155 min. 21 pasos.

## F0 — Línea base (estimado: 10 min)
No edita nada. Si un chequeo falla por algo ajeno, el paso queda `[!]` y F1 no empieza.

- [x] **F0.1 Anotar E19 y el estado de Git.** —
  - Hecho cuando:
    - [x] `diff -rq template/.agents .agents`, ignorando `.agents/local/`, muestra solo la línea base de `MEMORY.md` (`napkin.md`, `skills/caveman`, `agents/claudio.md`, `skills/reporte-sesiones`). Otra ruta detiene el paso.
    - [x] `git status` de `template/`, `ACTUALIZAR.md`, `INSTALAR.md`, `README.md` y `AGENTS.md` sale limpio.
    - [x] Evidencia de datos: No aplica. Verificación manual: No aplica.

- [x] **F0.2 Herramientas que admiten `model` por rol.** RF-13, RF-14
  - Hecho cuando:
    - [x] Se anota en `05_avance.md`, citando `restricciones-de-rol.md` › «Registro de campos» y la tabla de enlaces de `sync-agents.md`: Claude Code, Cursor, Gemini CLI, Copilot y Kiro admiten `model`; OpenCode por adaptador; Codex y Antigravity sin carpeta de agentes (solo sugerencia).
    - [x] Evidencia de datos: No aplica. Verificación manual: No aplica.

## F1 — Sugerencias y fuentes (estimado: 30 min)
Depende de F0.

- [x] **F1.1 `configuracion.ejemplo.md`.** RF-6, RF-10, RF-13, RF-18
  - Hecho cuando:
    - [x] Existe `template/.agents/configuracion.ejemplo.md`, con el formato de `entornos.ejemplo.md`: qué es, que se copia a `.agents/local/configuracion.md` y que no se versiona.
    - [x] Formato del archivo local: la primera línea es `version: X.Y.Z` y hay una sección `##` por paso. Los pasos son 5 (RF-6), cada uno con su contexto de ≤ 2 líneas y su valor sugerido.
    - [x] Los valores que ya tienen fuente se citan y no se copian: modelos de Claude Code, tope de clarify y «Dónde corre cada rol».
    - [x] Tabla de equivalencias: filas Cursor, Antigravity, Codex y OpenCode; columnas por nivel (alto, medio, bajo). Las celdas con «o» llevan el sugerido primero, el esfuerzo va como sugerencia y la tabla lleva fecha y origen (decisión 5 del avance). Antigravity y Codex llevan la nota «solo sugerencia: sin carpeta de agentes».
    - [x] Hay una sección del auditor externo (beta): formato de `auditor.md` (herramienta, modelo, comando, puntos de la lista cerrada de RF-10) y la regla «sin credenciales, solo el nombre de la variable de entorno».
    - [x] L. Evidencia de datos: No aplica. Verificación manual: No aplica.

- [x] **F1.2 Remisiones: preferencias y modelos.** RF-5, RF-16
  - Hecho cuando:
    - [x] `template/.agents/preferencias.ejemplo.md` tiene una línea que remite a `.agents/local/configuracion.md` para dónde corre cada rol, la presentación y lo demás configurable, sin valores.
    - [x] En `template/.agents/skills/sdd/restricciones-de-rol.md` › «Modelo por rol», «la tabla del proyecto» pasa a ser la sección de modelos de `.agents/local/configuracion.md`, y la tabla del kit queda como fuente de los valores sugeridos de Claude Code.
    - [x] P y L en los dos. Evidencia de datos: No aplica. Verificación manual: No aplica.

- [x] **F1.3 Tope y modo de corrección de clarify.** RF-21, RF-22
  - Hecho cuando:
    - [x] `template/.agents/skills/sdd/topes-y-modos.md`:
      - Dice «pasada n de N», con N el tope configurado de quien corre la pasada (por omisión, el de este archivo) y el conteo por spec.
      - Tiene una sección «Modo de corrección» con «automático» (sugerido) y «con aprobación».
      - Lleva la regla de supuestos marcados para alcance o seguridad y dice que aprobar la spec es siempre de la persona.
    - [x] `template/.agents/skills/sdd/avance-y-tiempos.md` dice «pasada n de N» y remite a `topes-y-modos.md`.
    - [x] `grep -rn "pasada n de 3" template` sale vacío.
    - [x] P y L. Evidencia de datos: No aplica. Verificación manual: No aplica.

## F2 — Asistente y entradas (estimado: 35 min)
Depende de F1.

- [x] **F2.1 Comando `/sdd-config`.** RF-2 a RF-5, RF-7 a RF-10, RF-12, RF-20
  - Hecho cuando:
    - [x] Existe `template/.agents/commands/sdd-config.md`, con frontmatter `description` y `argument-hint` como los demás comandos.
    - [x] Lee `configuracion.ejemplo.md` y la configuración local. Muestra los pasos de uno en uno, con el sugerido primero. Sin archivo, ofrece todos; con archivo, muestra el valor actual (RF-12); al invocarlo la actualización, ofrece solo los pasos sin valor (RF-2).
    - [x] Saltar un paso o todo el asistente aplica los sugeridos y lo dice en una línea al final (RF-4). Un valor inválido toma el sugerido y avisa (RF-7).
    - [x] Auditor: solo si la persona lo pide o se detectan dos herramientas con el criterio de `/sdd-init` (cita, no copia). Lleva la marca beta y la combinación probada (RF-8 a RF-10).
    - [x] Entornos: antes, la advertencia S-8 por remisión; después, confirmación por cada destino (RF-20).
    - [x] Escribe solo en `.agents/local/`. Propone mover directivas de `PREFERENCIAS.md` con visto bueno. Si cambió algún modelo, sugiere `/sdd-enlaces`.
    - [x] L. Evidencia de datos: No aplica. Verificación manual: No aplica.

- [x] **F2.2 `/sdd-init` invoca el asistente y deja el `inherit`.** RF-1, RF-15
  - Hecho cuando:
    - [x] En `template/.agents/commands/sdd-init.md`, tras la entrevista y fuera de su tope, se invoca `/sdd-config`.
    - [x] El paso 3 ya no escribe `model:` en `.agents/agents/`: remite a RF-15 y a `sync-agents.md` (adaptadores de rol).
    - [x] `grep -n "inherit" template/.agents/commands/sdd-init.md` no ordena escribir en los agentes.
    - [x] P y L. Evidencia de datos: No aplica. Verificación manual: No aplica.

- [x] **F2.3 `ACTUALIZAR.md`.** RF-2, caso límite `model: inherit`
  - Hecho cuando:
    - [x] Tras actualizar se invoca `/sdd-config` para los pasos sin valor.
    - [x] Si `.agents/agents/` tiene `model: inherit` heredado, se propone volver a los valores de `restricciones-de-rol.md` y pasar `inherit` a los adaptadores. No se cambia sin confirmación.
    - [x] P y L. Evidencia de datos: No aplica. Verificación manual: No aplica.

- [x] **F2.4 `AGENTS.md`: comando y paso 10.** RF-19
  - Hecho cuando:
    - [x] `template/AGENTS.md` › «Roles y comandos» lista `/sdd-config`.
    - [x] El paso 10 de «Cada sesión» lee, tras `PREFERENCIAS.md`, solo la sección «Presentación» de `.agents/local/configuracion.md`, si existe. Agrega una sola frase.
    - [x] `AGENTS.md` de la raíz lista `/sdd-config` en «Roles y comandos».
    - [x] P y L en los dos. Evidencia de datos: No aplica. Verificación manual: No aplica.

- [x] **F2.5 Listas de comandos.** —
  - Hecho cuando:
    - [x] `template/.agents/README.md` lista `commands/sdd-config.md` y `configuracion.ejemplo.md`.
    - [x] `template/.agents/agents/coordinator.md` lista `/sdd-config` donde lista los comandos; si no los lista, no se toca.
    - [x] P y L. Evidencia de datos: No aplica. Verificación manual: No aplica.

- [x] **F2.6 Docs de la raíz.** —
  - Hecho cuando:
    - [x] `README.md` e `INSTALAR.md` mencionan `/sdd-config` donde mencionan `/sdd-init` o la lista de comandos. Una línea por archivo.
    - [x] P y L. Evidencia de datos: No aplica. Verificación manual: No aplica.

## F3 — Enlaces y estructura (estimado: 35 min)
Depende de F1. Puede correr en paralelo con F2 (archivos disjuntos).

- [x] **F3.1 `sync-agents.md`: carpeta por archivo y adaptador.** RF-14, RF-17, RF-18
  - Hecho cuando:
    - [x] Hay una sección con la definición de carpeta de agentes por archivo y de adaptador de rol (ADR 0030). Generaliza la de OpenCode, que pasa a remitir a ella. Incluye el enlace relativo `../../.agents/agents/<rol>.md`.
    - [x] La tabla de enlaces marca las carpetas de agentes de Cursor, Gemini y GitHub como «por archivo». La de Claude Code queda como enlace de carpeta, salvo modelos propios.
    - [x] «Chequeo de arranque» suma al aviso combinado la falta de `.agents/local/configuracion.md`, o una `version:` distinta de `.agents/.kit-version` con pasos nuevos, y sugiere `/sdd-config`. Lee solo la primera línea y no escribe.
    - [x] P y L. Evidencia de datos: No aplica. Verificación manual: No aplica.

- [x] **F3.2 `/sdd-enlaces`.** RF-14, RF-17
  - Hecho cuando:
    - [x] `template/.agents/commands/sdd-enlaces.md` remite a la definición de F3.1 y no la copia. Debe:
      - crear por archivo la carpeta de agentes de toda herramienta distinta de Claude Code, con el `model:` configurado o el sugerido de su fila (o `inherit`);
      - en Claude Code, crear por archivo solo con modelos propios y volver al enlace de carpeta cuando ya no hay ninguno;
      - regenerar el adaptador viejo conservando su `model:`.
    - [x] Todo con confirmación. Nunca escribe en `.agents/agents/`.
    - [x] P y L. Evidencia de datos: No aplica. Verificación manual: No aplica.

- [x] **F3.3 `validar-estructura.md`: E3 y E23.** RF-17
  - Hecho cuando:
    - [x] E3 toma como válido, en toda carpeta por archivo, un enlace por rol o un adaptador que cumple la definición. La excepción propia de OpenCode pasa a remitir a esa definición.
    - [x] E23 compara también el `model:` de los adaptadores con la sección de modelos de `.agents/local/configuracion.md`. Si no existe, lo compara con los sugeridos. Sigue siendo solo `AVISO`.
    - [x] Un adaptador cuyo frontmatter difiere del canónico (salvo `model:` y los campos de la definición) da `AVISO` de adaptador viejo.
    - [x] P y L. Evidencia de datos: No aplica. Verificación manual: No aplica.

- [x] **F3.4 Ensayo de carpeta por archivo.** RF-14, RF-17; casos CP-04 y CP-05
  - Hecho cuando:
    - [x] En `mktemp -d`, con el `template/` copiado, se arma a mano `.claude/agents/` por archivo: un adaptador de `implementer` con otro `model:` y los demás roles como enlaces.
    - [x] `readlink -f` resuelve cada enlace a `.agents/agents/<rol>.md`.
    - [x] `.agents/agents/` no cambió (`diff -r` contra `template/.agents/agents`).
    - [x] No edita `template/`. Evidencia de datos: No aplica. Verificación manual: que Claude Code tome el adaptador queda para `/sdd-validate` (CP-06).

## F4 — Clarify y auditor (estimado: 25 min)
Depende de F1.3.

- [x] **F4.1 `/sdd-clarify` según el modo de corrección.** RF-22
  - Hecho cuando:
    - [x] Los pasos 3 y 4 de `template/.agents/commands/sdd-clarify.md` remiten a `topes-y-modos.md` › «Modo de corrección»:
      - en «automático», el planner corrige y vuelve a clarify hasta el tope;
      - en «con aprobación», se pregunta tras cada pasada.
    - [x] El paso 5 dice «pasada n de N».
    - [x] Aprobar la spec sigue siendo de la persona.
    - [x] P y L. Evidencia de datos: No aplica. Verificación manual: No aplica.

- [x] **F4.2 `SKILL.md`: dónde corre y auditor.** RF-6.2, RF-8 a RF-11
  - Hecho cuando:
    - [x] «Dónde corre cada rol» suma los roles configurados como subagente en `.agents/local/configuracion.md`, leídos al iniciar la fase, y no copia sus valores.
    - [x] Hay una sección nueva «Auditor externo (beta)», que es fuente única:
      - Se lanza solo si `.agents/local/auditor.md` existe y la fase está en sus puntos.
      - Trabaja en solo lectura y cumple `AGENTS.md` › Guardrails.
      - Su salida completa va a `.agents/local/auditor/` y `05_avance.md` anota quién auditó, en qué punto, la ruta y una línea por observación.
      - Nunca hace commit.
    - [x] P y L. Evidencia de datos: No aplica. Verificación manual: No aplica.

## F5 — Release, réplica y ensayo (estimado: 20 min)
Depende de F1 a F4. Después de F5.2, `template/` no se edita.

- [x] **F5.1 Versión `0.10.0` y changelog.** Criterios de finalización
  - Hecho cuando:
    - [x] `VersionAgentKitSdd` tiene una sola línea: `0.10.0`.
    - [x] `ChangelogAgentKitSdd.md` abre `## 0.10.0 — <fecha>` encima de `0.9.0`, con:
      - asistente `/sdd-config`;
      - configuración por dev no versionada (ADR 0029);
      - carpeta por archivo y adaptadores (ADR 0030);
      - fin del `inherit` en `.agents/agents/`;
      - modo de corrección de clarify;
      - auditor externo beta;
      - nota para proyectos actualizados: `/sdd-config` y `/sdd-enlaces`.
    - [x] L en los dos. Evidencia de datos: No aplica. Verificación manual: No aplica.

- [x] **F5.2 Manifiesto `0.10.0`.** E20, criterios
  - Hecho cuando:
    - [x] La prueba de humo de `ACTUALIZAR.md` da las tres huellas.
    - [x] `manifiestos/0.10.0.md` tiene el formato de `0.9.0.md`: una fila por archivo regular de `template/`, ordenadas con `LC_ALL=C sort`. Incluye `configuracion.ejemplo.md` y `sdd-config.md`.
    - [x] Los manifiestos anteriores no cambiaron.
    - [x] L, sin exigir que existan las rutas. Evidencia de datos: No aplica. Verificación manual: No aplica.

- [x] **F5.3 Réplica a `.agents/` y E19.** —
  - Hecho cuando:
    - [x] Cada archivo cambiado o nuevo de `template/.agents/` se copió a `.agents/` de la raíz.
    - [x] `diff -rq template/.agents .agents`, ignorando `.agents/local/`, muestra solo la línea base de F0.1.
    - [x] `git status` no muestra `.agents/`.
    - [x] Evidencia de datos: No aplica. Verificación manual: No aplica.

- [x] **F5.4 Ensayo de instalación y lint final.** Criterios de finalización; casos CP-01, CP-07, CP-08
  - Hecho cuando:
    - [x] El ensayo de `AGENTS.md` › Comandos en `mktemp -d` copia todo, incluidos los archivos nuevos, sin pisar nada.
    - [x] L en cada archivo tocado por el feature.
    - [x] Los valores sugeridos solo están en su fuente: `grep -rn "opus" template/.agents --include=*.md` fuera de `agents/` y de `restricciones-de-rol.md` solo da remisiones, y «3 pasadas» solo aparece en `topes-y-modos.md`.
    - [x] `grep -rn "model: inherit" template/.agents/agents` sale vacío.
    - [x] Evidencia de datos: No aplica. Verificación manual: correr `/sdd-config` en el temporal queda para `/sdd-validate` (CP-02, CP-03).
