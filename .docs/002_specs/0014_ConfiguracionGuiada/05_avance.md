# 05 — Avance: Configuración guiada del kit
Leyenda: `[ ]` Pendiente · `[~]` En curso · `[x]` Hecho · `[!]` Bloqueado

## PUNTO DE RETOMO — leer esto primero
| Dato | Contenido |
|---|---|
| Modo | `completo`, sin flujo (decisión 25). Condiciones de [topes-y-modos.md](../../../.agents/skills/sdd/topes-y-modos.md): no cumple la 1, 3, 4, 6 ni 8; la 5 sí |
| Última sesión | 2026-11-21 15:44 |
| Fase actual | `cierre`: `Cerrado` (decisión 27) |
| Último paso cerrado | Cierre, segunda corrida: `Cerrado` (decisión 27) |
| Siguiente paso | Ninguno en esta spec. Siguiente spec: `0015_ArranqueLigero` (plan) |
| Bloqueos abiertos | Ninguno. La 0013 está `Cerrado` (2026-11-21) |
| Estado BD | No aplica |
| Código | Kit 0.10.0 en 3 commits locales, sin push |

## Pasos
Detalle en `04_tasks.md` (21 pasos). Casos: `07_casos_prueba.md`.

| Fase | Pasos | Estado |
|---|---|---|
| F0 Línea base | F0.1 a F0.2 | `[x]` |
| F1 Sugerencias y fuentes | F1.1 a F1.3 | `[x]` |
| F2 Asistente y entradas | F2.1 a F2.6 | `[x]` |
| F3 Enlaces y estructura | F3.1 a F3.4 | `[x]` |
| F4 Clarify y auditor | F4.1 a F4.2 | `[x]` |
| F5 Release, réplica y ensayo | F5.1 a F5.4 | `[x]` |

## Decisiones
1. **[2026-11-21 09:51] Origen y alcance:** la persona pide, mientras se especifica la 0013, que la instalación ofrezca configurar todo paso a paso con contexto breve, saltable, con su configuración actual como valor por omisión. Spec aparte de la 0013 (recomendación aceptada); las preguntas de RF-5 y RF-28 de la 0013 se vuelven pasos de este asistente. Antecedentes: `007_DeudasAbiertas` (sugerencia de allow y deny en `/sdd-init`) y `0010_FalsoPositivoInitPendiente` (marcador de instalación); deuda abierta relacionada: ninguna de `sdd-init`. Alternativa descartada: todo dentro de la 0013.
2. **[2026-11-21 09:51] Auditor externo: opcional y beta.** Solo se ofrece si la persona lo pide o si al instalar o actualizar se detectan dos herramientas de agente. Probado: Claude Code como sesión principal y Cursor como auditor, en Linux. Por omisión, apagado. Alternativas descartadas: paso fijo del asistente; deuda hasta reactivarlo.
3. **[2026-11-21 09:52] Convivencia con el kit personal** (pedido de la persona). El kit personal (`~/.claude/personal/README.md` › «Convivencia con agent-kit-sdd») no instala una pieza que el kit SDD ya cubre: solo da seguimiento con `sin`. Para que funcione, esta spec debe fijar rutas estables de la configuración de cada dev y del auditor: el kit personal las usará en `sin` de `preferencias-base` y `auditor-externo`. Se tiene en cuenta al resolver el `[ACLARAR]`.
4. **[2026-11-21 10:49] Criterio de la 0015 para el lugar de la configuración** (decisión de la persona en `0015_ArranqueLigero`): lo que no hace falta en cada turno no va en `AGENTS.md`, que se carga siempre. Se tiene en cuenta al resolver el `[ACLARAR]` del lugar de la configuración.
5. **[2026-11-21 14:31] Insumo: equivalencias de modelos por herramienta** (la persona las trae; decisión 38 de la 0013: van aquí y no en un cambio de la 0013). Tabla tal como la entregó la persona:

   | Plataforma / Agente | Ecosistema / Motor | Rol 1: Sesión Principal | Rol 2: Subagente Implementer | Rol 3: Subagente Tester |
   | ----- | ----- | ----- | ----- | ----- |
   | **Claude Code** *(Referencia)* | Anthropic | `claude-opus-5-5` *(effort: high)* | `claude-sonnet-5-5` | `claude-sonnet-5-5` |
   | **Cursor** | Nativo Cursor / xAI *(Grok & Composer)* | `Grok 4.7` *(effort: high Fast/ 512k)* | `Composer 2.5` *(standard)* o `Grok 4.7 (Fast)` | `Composer 2.5` *(o `Grok 4.6`)* |
   | **Antigravity** | Google DeepMind *(Gemini)* | `Gemini 3.1 Pro` *(High Reasoning)* | `Gemini 3.5 Flash` *(Agéntico)* o `Gemini 3.1 Pro` | `Gemini 3.5 Flash` |
   | **Codex (CLI / IDE)** | OpenAI | `gpt-5.3-codex` *(reasoning: high)* | `gpt-5.3-codex` *(reasoning: medium)* | `gpt-5.3-codex-mini` *(reasoning: low/medium)* |
   | **OpenCode (Free Tier)** | OpenCode Zen *(Modelos gratuitos)* | `opencode/beam-free` *(xhigh/max)* o `opencode/minimax-m3.1-flash-free` *(high)* | `opencode/big-pickle` o `opencode/deepseek-v4.1-flash-free` | `opencode/nemotron-3.5-lightning-free` o `opencode/deepseek-v4.1-flash-free` |

   Ajuste ya decidido: en Claude Code, `tester` va en `haiku`, como define la 0013, y no en `claude-sonnet-5-5` como dice la tabla. Queda por resolver en la spec: la tabla no trae columnas para planner, reviewer, coordinator ni explore; falta decidir qué hacer con las celdas que ofrecen dos modelos («o») y con el esfuerzo (effort o reasoning), y dónde vive la tabla.

6. **[2026-11-21 14:38] La configuración es de cada dev y no se versiona** (decisión de la persona; resuelve el `[ACLARAR]`). El kit solo sugiere y cada dev reconfigura. Todo valor configurable, incluidos los modelos y el tope de clarify, va en `.agents/local/configuracion.md`; `PREFERENCIAS.md` solo remite. Se propuso primero `.agents/configuracion.md` versionado para modelos y tope, y la persona lo cambió: los modelos dependen de la herramienta de cada dev. Alternativas descartadas: sección de `AGENTS.md` (va contra la decisión 4); editar los archivos del kit (dejan de ser KIT-VIEJO); todo en `PREFERENCIAS.md` (se carga siempre, texto libre).
7. **[2026-11-21 14:35] Rutas estables de cada dev** (cumple la decisión 3, kit personal): `.agents/local/configuracion.md`, `.agents/local/entornos.md` (0013) y `.agents/local/auditor.md` (solo existe si el auditor está activo).
8. **[2026-11-21 14:38] Modelo propio en Claude Code: adaptador local** (decisión de la persona). `.agents/agents/<rol>.md` conserva el `model:` sugerido; si un dev elige otro, `.claude/agents/<rol>.md` pasa de enlace a adaptador con su `model:`, como en OpenCode. Alternativas descartadas: modelos fijos en Claude Code; dejarlo al plan.
9. **[2026-11-21 14:37] Tabla de equivalencias por nivel** (decisión de la persona): las 3 columnas de la decisión 5 son niveles. Alto: planner, coordinator, reviewer. Medio: implementer. Bajo: `tester` y `explore`. Alternativas descartadas: seis columnas; reviewer de otra familia (se cruza con el auditor).
10. **[2026-11-21 14:37] Celdas con dos modelos:** el primero es el sugerido y el segundo se ofrece como alternativa en el paso. Alternativas descartadas: un modelo por celda; preguntar siempre (choca con RF-4).
11. **[2026-11-21 14:40] Las sugerencias viajan en `.agents/configuracion.ejemplo.md`** (decisión de la persona), como `entornos.ejemplo.md`. Lleva la tabla de equivalencias y los valores sugeridos. Alternativa descartada: dentro de `/sdd-init`.
12. **[2026-11-21 14:41] El esfuerzo (effort o reasoning) es solo una sugerencia:** se muestra en la tabla y no se escribe en ningún archivo. Alternativas descartadas: configurarlo por rol; quitarlo.
13. **[2026-11-21 14:50] Ciclo automático de clarify** (decisión de la persona): si clarify detecta algo, el planner corrige sin preguntar y vuelve a clarify, hasta 3 pasadas; si tras la tercera queda algo, se avisa a la persona. La persona pide que sea un valor configurable del paso de clarify (RF-6.3, RF-22), sugerido «automático».
14. **[2026-11-21 14:50] Correcciones de la pasada 1, elegidas por el planner** (ciclo automático, decisión 13):
    - Herramienta de apoyo fuera del asistente: es del proyecto y sigue en `AGENTS.md` › Comandos (hallazgo 1).
    - «Dónde corre cada rol» remite a `SKILL.md` y solo se configura qué roles van siempre como subagente (hallazgo 2).
    - `.agents/agents/` nunca recibe `model:` del asistente; el paso de `inherit` de `/sdd-init` pasa a los adaptadores (hallazgo 3, RF-15).
    - `restricciones-de-rol.md` sigue como fuente única de los valores de Claude Code; «la tabla del proyecto» pasa a la configuración local (hallazgo 4, RF-16).
    - Comando nuevo `/sdd-config` para correr y volver a correr el asistente; «paso sin valor» definido (hallazgo 5).
    - Detección de herramientas con el criterio de `/sdd-init` (hallazgo 6).
    - Carpeta de agentes por archivo, con enlaces por rol y adaptadores, en cualquier herramienta (hallazgos 7 y 10, RF-14, RF-17).
    - Lectura de cada valor solo donde se usa (hallazgo 8, RF-19).
    - Salida del auditor completa en `.agents/local/auditor/` y lista cerrada de puntos del flujo (hallazgo 9).
    - Aviso del chequeo de arranque cuando falta la configuración local (hallazgo 11, RF-18).
    - Tope de clarify por quien corre la pasada, conteo por spec (hallazgo 12, RF-21).
    - Advertencia S-8 y confirmación por destino (hallazgo 13, RF-20).
    - Barra de estado fuera de alcance: requiere scripts; la da el kit personal (hallazgo 14).
15. **[2026-11-21 14:50] Supuesto explícito:** qué herramientas admiten modelo por rol en su formato de agente se comprueba en el plan; la que no lo admita queda en `inherit`.

16. **[2026-11-21 14:51] Correcciones de la pasada 2** (ciclo automático): `/sdd-enlaces` crea por archivo la carpeta de agentes de toda herramienta distinta de Claude Code, con el modelo sugerido de su fila (sin ventana con `model: opus`); el adaptador admite campos exigidos por la herramienta (`mode` de OpenCode); en modo automático, una corrección de alcance o seguridad se escribe como supuesto explícito marcado; el arranque lee solo la sección «Presentación»; `configuracion.md` anota la versión del kit y el chequeo de arranque avisa si faltan pasos nuevos.
17. **[2026-11-21 14:51] Tope de clarify alcanzado** (3 de 3). Los 2 hallazgos de la pasada 3 se corrigieron (RF-14 restaura el enlace solo en Claude Code; RF-19 admite la lectura de la primera línea por el chequeo de arranque) sin pasada que los verifique. Queda a la persona aprobar así (`topes-y-modos.md`).

18. **[2026-11-21 14:53] Spec aprobada** (la persona: «procede») con las correcciones del ciclo automático, incluidos los supuestos de la decisión 14 y las 2 correcciones sin verificar de la decisión 17.

19. **[2026-11-21 14:58] Plan:** implementar la 0014 antes que la 0015 (choque en `template/AGENTS.md` y en «Chequeo de arranque»). Codex y Antigravity solo reciben la sugerencia de modelo; no tienen carpeta de agentes. Candidatos a ADR, pendientes del visto bueno de la persona: configuración por dev no versionada; carpeta de agentes por archivo con adaptadores.

20. **[2026-11-21 14:59] Plan aprobado** (la persona: «procede») y ADR 0029 y 0030 creados con su visto bueno.

21. **[2026-11-21 15:16] Tareas aprobadas** (la persona: «procede»). Implementación en cascada; el commit de lo documental se propone aparte y espera confirmación explícita.
22. **[2026-11-21 15:16] F0.2:** según `restricciones-de-rol.md` › «Registro de campos» (lectura 2026-10-25) y la tabla de enlaces de `sync-agents.md`: Claude Code, Cursor, Gemini CLI, GitHub Copilot y Kiro admiten `model` por rol; OpenCode por adaptador; Codex y Antigravity no tienen carpeta de agentes en el kit (solo sugerencia).

23. **[2026-11-21 15:19] Ajustes de la sesión a lo entregado:** en `configuracion.ejemplo.md`, la fuente de la tabla citaba `.docs/002_specs/0014_…`, que no existe en un proyecto (constitución 1); ahora dice «reunidas por el mantenedor del kit el 2026-11-21 (kit 0.10.0)». En `sdd-config.md`, «Fase `config`» pasó a «Asistente de configuración de cada dev (no es una fase del flujo)». F2.3 quedó en 8 líneas (pedía 2–6); se acepta.
24. **[2026-11-21 15:20] Implementación F0–F5 terminada.** Pendiente para validate: los casos manuales CP-02, CP-03, CP-06 y CP-08. Observación de F3.1: el punto 5 nuevo del chequeo de arranque queda fuera del tope de 3 líneas del punto 4; revisar en validate.
25. **[2026-11-21 15:36] Sin flujo: `06_flujo.md` no aplica; se aplica el ajuste del paso 7** (decisión de la persona: «no aplica flujo para este req, aplica el paso 7 y luego procede con el cierre»). El feature cambia el kit y no hay una operación de usuario final que documentar; la fila de modo queda en «`completo`, sin flujo», como en la 0012 y la 0013, y `/sdd-flujo` no se corre. Alternativa descartada: generar `06_flujo.md`. El paso 7 de `sdd-config.md` remite ahora a los nombres fijos de `configuracion.ejemplo.md` › «Formato del archivo local»; réplica en `.agents/` igual y huella de `sdd-config.md` actualizada en `manifiestos/0.10.0.md` (49 de 49 coinciden con `template/`). De paso se corrigió la leyenda de `04_tasks.md` (decía `[x]` Pendiente).
26. **[2026-11-21 15:37] Cierre, primera corrida: `Implementado`.** La comprobación previa pasa: 21 pasos `[x]`, sin verificaciones manuales pendientes (CP-06 confirmado), sin flujo por el modo, sin pausa y con validate aprobado. Criterios: 1 cumple (fila `cierre` y plan contra real); 2 falla, porque todo el requerimiento sigue sin commit (la persona pidió cerrar antes de los commits); 3 cumple (sin ticket ni aviso); 4 cumple (columna de pendiente vacía); 5 cumple (ninguna deuda tiene Atendida por 0014). Sin deuda nueva propuesta: lo de los adaptadores de Cursor ya es la deuda 005.
27. **[2026-11-21 15:44] Cierre, segunda corrida: `Cerrado`, con los commits en la misma corrida** (decisión de la persona: «ejecuta como te digo: Cierre, commit»; antes, «no me hace sentido […] hacemos 2 commits seguidos»). El criterio 2 se da por cumplido con los commits que la persona confirma en esta misma corrida, justo después de escribir el estatus: van todos los cambios del requerimiento, incluidos los de este cierre, y no queda un commit aparte solo para el cierre. Los criterios 1, 3, 4 y 5 siguen como en la decisión 26. Alternativa descartada: commits primero y una segunda corrida con su propio commit (la regla vigente de «Leyenda de estatus», criterio 2). La regla se analiza aparte y no se cambia aquí: el kit pasa por su propio flujo SDD.

## Pruebas
Corridas y resultados: [07_casos_prueba.md](07_casos_prueba.md) › «Corridas». CP-06 confirmado por la persona [2026-11-21 15:34]: `implementer` corre en Opus con el adaptador.
Opcionales del reviewer (no bloquean): el paso 7 de `sdd-config.md` ya remite a los nombres fijos (decisión 25); los adaptadores de Cursor conservan `tools` y `omitClaudeMd` sin veredicto (deuda 005, sin regresión).

## Tiempos
| Paso | Inicio | Fin | Minutos | Pausa | Nota |
|---|---|---|---|---|---|
| spec | 2026-11-21 09:47 | 2026-11-21 09:51 | 4 | 0 | Borrador en el chat, rol planner (Opus 5.5) |
| spec (aclarar) | 2026-11-21 14:30 | 2026-11-21 14:41 | 11 | 0 | 7 preguntas una por una; decisiones 6 a 12; rol planner en el chat (Opus 5.5) |
| clarify | 2026-11-21 14:45 | 2026-11-21 14:48 | 3 | 0 | pasada 1 de 3: 14 hallazgos (4 contradicciones, 5 ambigüedades, 4 casos límite, 1 conflicto con la constitución); reviewer en el chat (Opus 5.5) |
| spec (corrige p1) | 2026-11-21 14:49 | 2026-11-21 14:50 | 1 | 0 | planner en el chat corrige los 14 hallazgos (decisiones 13 a 15); RF-16 a RF-22 nuevos |
| clarify | 2026-11-21 14:50 | 2026-11-21 14:51 | 1 | 0 | pasada 2 de 3: 5 hallazgos (2 contradicciones, 2 ambigüedades, 1 caso límite); reviewer en el chat (Opus 5.5) |
| spec (corrige p2) | 2026-11-21 14:51 | 2026-11-21 14:51 | 0 | 0 | planner corrige los 5 (decisión 16) |
| clarify | 2026-11-21 14:51 | 2026-11-21 14:51 | 0 | 0 | pasada 3 de 3: 2 hallazgos menores (contradicciones RF-14/RF-17 y RF-19/RF-18); tope alcanzado |
| spec (corrige p3) | 2026-11-21 14:51 | 2026-11-21 14:51 | 0 | 0 | planner corrige los 2 sin pasada de verificación (tope); decisión 17 |
| plan | 2026-11-21 14:56 | 2026-11-21 14:58 | 2 | 0 | `02_analisis_impacto.md` y `03_plan.md`; 6 fases, estimado 155 min; planner en el chat (Opus 5.5) |
| tasks | 2026-11-21 14:59 | 2026-11-21 15:01 | 2 | 0 | ADR 0029 y 0030; `04_tasks.md` (21 pasos, 155 min) y `07_casos_prueba.md` (8 casos); planner en el chat (Opus 5.5) |
| F0 | 2026-11-21 15:16 | 2026-11-21 15:17 | 1 | 0 | E19 = línea base; Git limpio en `template/` y docs de raíz; F0.2 en decisión 22; sesión |
| F1 | 2026-11-21 15:17 | 2026-11-21 15:18 | 1 | 0 | F1.1–F1.3 en paralelo, implementer (claude-sonnet-5-5); la sesión corrigió la fuente de la tabla en el ejemplo (decisión 23) |
| F2–F4 | 2026-11-21 15:18 | 2026-11-21 15:19 | 1 | 0 | F2.1–F2.6, F3.1–F3.3, F4.1–F4.2 en paralelo, implementer (claude-sonnet-5-5); F3.4 ensayo por la sesión (CP-04, CP-05) |
| F5 | 2026-11-21 15:19 | 2026-11-21 15:20 | 1 | 0 | Versión 0.10.0, changelog, manifiesto (humo con las tres huellas), réplica (E19 = línea base), ensayo de instalación y lint; sesión |
| validate | 2026-11-21 15:23 | 2026-11-21 15:26 | 3 | 0 | Vuelta 1: CAMBIOS NECESARIOS (chequeo de arranque, punto 5: comparación de títulos y tope de 3 líneas); corrección por implementer (claude-sonnet-5-5); vuelta 2: APROBADO SALVO VERIFICACIÓN MANUAL (CP-06). Reviewer en el chat (claude-opus-5-5); tester (claude-haiku-5-5) CP-07; ejecutor en temporal (claude-sonnet-5-5) CP-02, CP-03, CP-08. Manifiesto 0.10.0 regenerado |
| cierre | 2026-11-21 15:36 | 2026-11-21 15:44 | 8 | 0 | la sesión (claude-opus-5-5). Ajuste del paso 7 y huella del manifiesto (decisión 25). Primera corrida: previa OK, criterio 2 falla, resultado `Implementado` (decisión 26). Segunda corrida: `Cerrado`, con los commits en la misma corrida (decisión 27) |

## Plan contra real

El plan estimó en minutos; el real es la suma de la columna Minutos. La fila `F2–F4` es un lote en paralelo y no se parte. Documentación SDD = `spec`, aclarar, tres pasadas de `clarify` con sus correcciones, `plan`, `tasks`, `validate` y `cierre` (35 min).

| Fase | Estimado | Real | Delta |
|---|---|---|---|
| Documentación SDD | sin estimado | 35 min | — |
| F0 | 10 min | 1 min | −9 min |
| F1 | 30 min | 1 min | −29 min |
| F2 a F4 (lote en paralelo) | 95 min | 1 min | −94 min |
| F5 | 20 min | 1 min | −19 min |

Qué ajustar en el siguiente plan: la lección de la 0013 no se aplicó al estimar. Con `implementer` en paralelo por lotes de archivos disjuntos, estimar ≈ 1 a 2 min por lote más la verificación de la sesión. El costo real está en las decisiones de la persona y en validate (los casos que piden conversar con el asistente, como CP-06).
