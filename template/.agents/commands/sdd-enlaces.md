---
description: SDD · Crea o corrige los enlaces de tus herramientas hacia .agents/ y marca el estado «visto» (con tu confirmación)
argument-hint:
---

La sesión principal ejecuta este comando. No lanza un subagente, no pasa por planner, implementer ni reviewer, y no toca `05_avance.md`: no es una fase del flujo. Pregunta al usuario varias veces y ejecuta en su máquina, por eso no se delega.

## Qué lee
- `.agents/sync-agents.md`: «Estados de enlace», tabla de enlaces, mecanismo, reglas de los pasos 3 a 8, «Carpeta de agentes por archivo» (definición y adaptador de rol), «Estado «visto»» (formato) y «Chequeo de arranque». Es la única fuente de esas reglas; este comando remite a ellas y no las copia.
- `.agents/local/kit-visto`, si existe.
- `.agents/local/configuracion.md` › `## Modelos`, si existe, y `.agents/configuracion.ejemplo.md` (modelo sugerido por herramienta). Solo para el `model:` de los adaptadores.

## Procedimiento

### 1. Leer
Lee los dos archivos de «Qué lee». Un «visto» ausente o corrupto (según `sync-agents.md` › «Estado «visto»») se trata como inexistente.

### 2. Preparar
- Detecta el sistema operativo y el shell (decide el comando de cada enlace, paso 5).
- Detecta si es un repositorio Git con `git rev-parse --is-inside-work-tree`. Si falla: omite `check-ignore` y la edición de `.gitignore`, avisa que «visto» no queda protegido por Git y pide confirmación antes de crear `.agents/local/` y `kit-visto`. El `git rm --cached` del paso 7 de `sync-agents.md` tampoco aplica.

### 3. Preguntar las herramientas
Pregunta qué herramientas usa la persona entre las de la tabla de `sync-agents.md` (Claude Code, Cursor, Gemini, Copilot, OpenCode). Solo esas se tratan. Si no declara ninguna: no crea enlaces, no escribe «visto», lo informa y termina; el aviso de arranque volverá en la siguiente sesión.

### 4. Clasificar
- Clasifica solo las filas de las herramientas declaradas, con los estados de `sync-agents.md`; las demás filas se omiten.
- Detecta, remitiendo a las reglas de `sync-agents.md` (pasos 7 y 8) sin repetirlas: enlaces colgantes (apuntan a una pieza retirada), réplicas huérfanas o faltantes y archivos versionados que `.gitignore` ya ignora.

### 5. Mostrar el plan y pedir confirmaciones
Muestra un plan con un comando exacto por enlace, según el sistema detectado y el criterio de `sync-agents.md` (enlace simbólico, junction en Windows sin administrador, copia solo si fallan los anteriores, avisando que la copia no se sincroniza). Antes de ejecutar, valida cada ruta contra la tabla de `sync-agents.md`: no se usan rutas libres. La carpeta de agentes de cada herramienta se trata según `sync-agents.md` › «Carpeta de agentes por archivo» (no se repite aquí); en OpenCode, además, con el `mode` de `sync-agents.md` › «OpenCode». En el plan:
- En toda herramienta distinta de Claude Code, crea la carpeta de agentes por archivo, con un adaptador por rol cuyo `model:` es el de `.agents/local/configuracion.md` › `## Modelos` o, sin él, el sugerido de su fila en `.agents/configuracion.ejemplo.md` (o `inherit`). Esto mantiene el cuerpo del agente canónico sin copiar.
- En Claude Code, convierte `.claude/agents` a carpeta por archivo solo si hay modelos propios; cuando ya no queda ninguno, propone volver al enlace de carpeta de la tabla.
- Un adaptador viejo se regenera conservando su `model:`.
- Nunca escribe en `.agents/agents/`.

Pide una confirmación por tipo de acción, nunca por enlace, con los comandos visibles, en este orden fijo (de menos a más riesgo; el estado propio al final):
1. Crear enlaces, carpetas de agentes por archivo (con sus adaptadores) y réplicas faltantes; incluye convertir o restaurar `.claude/agents` y regenerar adaptadores viejos.
2. Reemplazar DIFERENTE (mostrando destino actual y esperado).
3. Retirar enlaces colgantes (solo el enlace).
4. Pregunta por cada OCUPADO: «¿es intencional?». No ejecuta nada.
5. Mover réplicas huérfanas a `.agents/`.
6. `git rm --cached` de ignorados versionados.
7. Crear `.agents/local/` si no existe.
8. Editar `.gitignore` si `git check-ignore` falla para `.agents/local/kit-visto`.
9. Escribir «visto».

Un «no» omite solo ese tipo, salvo en (7) a (9): sin ellos no hay «visto» y el aviso vuelve.

### 6. Ejecutar
Ejecuta solo lo confirmado, una acción a la vez y con el comando mostrado. Si una acción falla, la anota y sigue con las demás herramientas (fallo parcial).
- OCUPADO nunca se borra ni se mueve. Si la persona dice que es intencional, lo registra para «visto» como `excepcion: <ruta>`; si no, queda pendiente.
- Si crea una copia, la anota para «visto» como `copia: <ruta>`.

### 7. Re-verificar y reportar
Vuelve a clasificar las filas de las herramientas declaradas y reporta el estado por herramienta: `OK`, `OK (copia)` (sin sincronización), FALLA o excepción.

### 8. Marcar «visto»
- Si la re-verificación queda OK sobre las herramientas declaradas (una excepción cuenta como OK): crea `.agents/local/` si falta (confirmado en 7), comprueba `git check-ignore`, edita `.gitignore` si falta la regla (confirmado en 8) y escribe «visto» completo de una vez, con el formato de `sync-agents.md` › «Estado «visto»» (confirmado en 9). Esto incluye el caso sin nada que corregir.
- Si no queda OK, hubo rechazo o falló la escritura: no escribe «visto» y avisa que el aviso de arranque volverá en la siguiente sesión.
- Rama sin `.agents/.kit-version` legible (con o sin `template/`; `template/` solo omite el chequeo de arranque, no este comando): crea los enlaces, NO escribe «visto» (no hay versión legible que registrar) e informa que falta el marcador (ver `ACTUALIZAR.md` › Marcador de versión).

## Qué no hace
No escribe scripts (`.sh`, `.ps1`, `.py`). No corre `/sdd-init`. No hace commit. No borra ni mueve contenido real (solo reemplaza o retira enlaces, con confirmación). No modifica `AGENTS.md`. No copia las reglas de `sync-agents.md`: las remite.
