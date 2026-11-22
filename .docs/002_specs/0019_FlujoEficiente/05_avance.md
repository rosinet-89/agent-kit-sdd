# 05 — Avance: Flujo eficiente
Leyenda: `[ ]` Pendiente · `[~]` En curso · `[x]` Hecho · `[!]` Bloqueado

## PUNTO DE RETOMO — leer esto primero
| Dato | Contenido |
|---|---|
| Modo | `completo`. Condiciones de [topes-y-modos.md](../../../.agents/skills/sdd/topes-y-modos.md): 1 no (más de 3 archivos: `SKILL.md`, `sdd-implement.md`, `sdd-clarify.md`, `sdd-cierre.md`, `sdd-init.md`, `topes-y-modos.md`, `avance-y-tiempos.md`, `template/AGENTS.md`, `AGENTS.md` raíz); 3 no (cambia contratos entre piezas); 6 no (cambia qué hacen `sdd-implement`, clarify y el cierre); 8 no (`commands` y `skills`) |
| Última sesión | 2026-11-22 15:52 |
| Fase actual | `cierre`: `Cerrado` (decisión 16), con el criterio 2 en los commits |
| Último paso cerrado | Cierre |
| Siguiente paso | Ninguno en esta spec. La persona publica con `git push` |
| Bloqueos abiertos | Ninguno |
| Estado BD | No aplica |
| Código | 10 archivos de `template/`, `AGENTS.md` y `README.md` de la raíz, versión 0.14.0, changelog, manifiesto y ADR 0036; réplica en `.agents/`; en commits locales tras la confirmación del cierre |

## Pasos
Detalle en `04_tasks.md` (lotes y olas en su tabla inicial).

| Fase | Pasos | Estado |
|---|---|---|
| F0 Línea base | F0.1 | `[x]` |
| F1 Lanzamientos | F1.1 a F1.3 | `[x]` |
| F2 Cascada y lotes | F2.1 a F2.6 | `[x]` |
| F3 Clarify | F3.1, F3.2 | `[x]` |
| F4 Chequeos de cierre | F4.1 a F4.3 | `[x]` |
| F5 Réplica, versión, ADR y manifiesto | F5.1 a F5.4 | `[x]` |

## Decisiones
1. **[2026-11-22 15:40] Origen y alcance.** Mejoras 3 a 7 del reporte de sesiones del 2026-11-21 y
   2026-11-22 (sesión 9afbd0c7). La persona respondió «si procede» a la pregunta de agregar la
   entrada a `MEMORY.md` y abrir una spec con las mejoras 3 a 7. Las mejoras 1, 2 y 8 quedan fuera
   de alcance (hábitos de cada dev y herramienta).
2. **[2026-11-22 15:40] Antecedentes.** Coinciden `0013_ModelosPorAgente` («Dónde corre cada rol»,
   modelo por rol), `0014_ConfiguracionGuiada` (`## Subagentes`, tope y modo de clarify),
   `0016_CierreEnUnaCorrida` (comprobación previa del cierre) y `0017_PausaOrigenYContexto` (lotes
   de implementer, E20 y `sdd-estructura`). Esta spec no contradice ninguna: endurece y completa.
   Deuda: ninguna abierta.
3. **[2026-11-22 15:42] Modo completo** (fila Modo): no hace falta confirmarlo.
4. **[2026-11-22 15:43] Clarify, pasada 1 de 3** (rol reviewer en el chat, claude-opus-5-5, modo
   automático): 12 hallazgos. El planner (en el chat) corrigió sin decisiones de alcance:
   - RF-1 y RF-2: «rol configurado» chocaba con la prohibición de agentes fuera del kit. Ahora (a)
     exige rol del kit y `## Subagentes` pasa a ser un motivo de (b).
   - RF-2: exceptúa al auditor externo de `SKILL.md` › «Auditor externo (beta)».
   - RF-3: suma los motivos «relanzamiento» y «auditor».
   - RF-5: para si `04_tasks.md` no está aprobado.
   - RF-7: el criterio de «no pide ejecución» no se podía verificar, porque todo «Hecho cuando» pide
     lint. Se reescribe: sin verificación manual, sin evidencia de datos ni `sql/`; admite fases
     distintas en el mismo archivo.
   - RF-11: aclara que el autochequeo no es validación.
   - RF-12: dice quién arma la lista del delta.
   - RF-15: dice cómo un comando distribuido engancha un chequeo del repo del kit. Se parte en RF-15
     (genérico) y RF-16 (E20 en la raíz).
5. **[2026-11-22 15:44] Clarify, pasada 2 de 3, por delta** (reviewer en el chat): 4 hallazgos,
   corregidos por el planner:
   - RF-7 y RF-9 usaban «depende» sin definirlo: se define.
   - RF-15 no decía dónde se declaran los chequeos: se agrega la línea en `template/AGENTS.md` ›
     Comandos.
   - Un caso límite citaba la RF-15 vieja: se separa en dos casos.
   - RNF-1 no ubicaba los chequeos de cierre: se ubican.
6. **[2026-11-22 15:44] Clarify, pasada 3 de 3, por delta** (reviewer en el chat): sin hallazgos.
   Supuestos S-1 a S-3 en la spec, para la aprobación de la persona.
7. **[2026-11-22 15:45] Spec aprobada** por la persona («procede» a la pregunta de aprobar con S-1 a
   S-3). `Estado: aprobada`.
8. **[2026-11-22 15:48] Plan y tareas.** Hay 6 fases, 19 pasos y 38 min estimados; 7 lotes en una ola
   paralela, aplicando RF-7 a esta misma spec. Hay 10 casos de prueba de solo lectura. El planner
   marca como candidata a ADR 0036 la cascada por omisión con lotes (S-1 y S-2). Sin visto bueno, el
   ADR no se crea.
9. **[2026-11-22 15:49] Plan y tareas aprobados, con el ADR 0036** (respuesta de la persona: «Aprobar
   con ADR»). Arranca la cascada hasta validate.
10. **[2026-11-22 15:50] F0.1, línea base** (sesión): `git status --short template/` vacío. La
    réplica solo difiere en las excepciones aceptadas. E20 contra `manifiestos/0.13.0.md`: OK, 54
    archivos. Se corrió con un script de la sesión en el scratchpad (no versionado), con el
    algoritmo de `ACTUALIZAR.md` › «Huella». La línea base de estructura se acotó a los chequeos que
    esta spec toca (E19 y E20), sin subagente (RF-4).
11. **[2026-11-22 15:50] Ola 1, lotes L1 a L7** (7 `implementer` en paralelo, claude-sonnet-5-5;
    motivo: otro modelo). Lotes de 9 a 25 s, de 15 000 a 31 000 tokens cada uno. La sesión revisó cada
    diff contra su «Hecho cuando». Un ajuste de la sesión: quitó las referencias «(RF-n)» que los
    lotes L1 y L5 dejaron en `SKILL.md` y `topes-y-modos.md`, porque en un proyecto no significan
    nada. `template/AGENTS.md` pasa de 9 603 a 9 635 bytes.
12. **[2026-11-22 15:51] Ola 2 y 3** (sesión):
    - Réplica de 9 archivos a `.agents/`; `diff -rq` solo con las excepciones aceptadas.
    - Lint OK en todo lo tocado.
    - `VersionAgentKitSdd` 0.14.0 y entrada del changelog.
    - ADR 0036 con su fila del índice y el enlace en `03_plan.md`.
    - `manifiestos/0.14.0.md` generado después del último cambio a `template/`: cambian solo las 10
      huellas de los archivos tocados.
    - E20 OK (54 archivos).
    - Ensayo de instalación en `mktemp -d` sin diferencias.
13. **[2026-11-22 15:52] Validate, vuelta 1** (`tester` claude-haiku-5-5, motivo: otro modelo; reviewer
    en el chat, claude-opus-5-5). `tester`: CP-01 a CP-09 obtenidos, todos `ok` por comparación
    literal; CP-10 lo corrió la sesión. Reviewer:
    - RF-1 a RF-16 con evidencia en los diffs.
    - RF-14: `topes-y-modos.md` solo tiene adiciones (10 y 0).
    - Seguridad básica (solo Markdown): sin secretos; las coincidencias de «token» son texto.
    - Veredicto: CAMBIOS NECESARIOS por una sola línea: `SKILL.md` › «Implementación (reglas
      duras)» decía «Un paso a la vez […] y se para», en contra de RF-5.
    - Corrección por un `implementer` (claude-sonnet-5-5, motivo: otro modelo).
14. **[2026-11-22 15:52] Validate, vuelta 2:** APROBADO.
    - Línea corregida y replicada.
    - E20 falló con la huella vieja de `SKILL.md`, el caso que RF-16 quiere atrapar. Se regeneró
      `manifiestos/0.14.0.md` y E20 pasa (54 archivos).
    - Ensayo de instalación OK; réplica solo con las excepciones aceptadas.
    - Sin verificaciones manuales.
15. **[2026-11-22 15:54] Sin flujo:** la persona elige «Sin flujo, al cierre». Solo cambia Markdown
    del kit, como en 0015 y 0017.
16. **[2026-11-22 15:56] Cierre: `Cerrado`, con los commits en la misma corrida.**
    - Previa: 19 de 19 pasos `[x]`, sin verificaciones manuales pendientes, sin flujo (decisión 15),
      sin pausa y validate aprobado (decisión 14).
    - Chequeo de cierre de `AGENTS.md` › Comandos: E20 OK (54 archivos), primer uso de RF-15 y RF-16.
    - Criterio 1: fila `cierre` y plan contra real escritas. Criterio 3: sin ticket ni aviso.
      Criterio 4: la columna de pendiente queda vacía. Criterio 5: ninguna deuda con Atendida por
      `0019`.
    - El criterio 2 se decide con los commits propuestos.
    - Sin deuda nueva. ADR 0036 ya registrado. Memoria: la entrada del 2026-11-22 ya está en
      `MEMORY.md`. Sin `.docs/005_contexto/` con documentos.

## Pruebas
Resultados en [07_casos_prueba.md](07_casos_prueba.md) › Corridas (CP-01 a CP-10 `ok`). Sin
verificaciones manuales.

## Tiempos
| Paso | Inicio | Fin | Minutos | Pausa | Nota |
|---|---|---|---|---|---|
| spec | 2026-11-22 15:40 | 2026-11-22 15:42 | 2 | 0 | rol planner en el chat (claude-opus-5-5); origen: reporte de sesiones |
| clarify | 2026-11-22 15:43 | 2026-11-22 15:44 | 1 | 0 | pasada 3 de 3; 12 + 4 + 0 hallazgos; reviewer y planner en el chat (claude-opus-5-5); modo automático |
| plan | 2026-11-22 15:45 | 2026-11-22 15:46 | 1 | 0 | `02_analisis_impacto.md` y `03_plan.md`; 6 fases, 38 min estimados; rol planner en el chat (claude-opus-5-5) |
| tasks | 2026-11-22 15:47 | 2026-11-22 15:48 | 1 | 0 | `04_tasks.md` (19 pasos, 7 lotes) y `07_casos_prueba.md` (10 casos); rol planner en el chat (claude-opus-5-5) |
| F0.1 | 2026-11-22 15:48 | 2026-11-22 15:49 | 1 | 0 | sesión (claude-opus-5-5); E19 y E20, línea base |
| F1.1, F2.4 | 2026-11-22 15:50 | 2026-11-22 15:51 | 1 | 0 | lote L1; implementer (claude-sonnet-5-5); motivo: otro modelo |
| F1.2, F2.5 | 2026-11-22 15:50 | 2026-11-22 15:51 | 1 | 0 | lote L2; implementer (claude-sonnet-5-5); motivo: otro modelo |
| F1.3, F2.6 | 2026-11-22 15:50 | 2026-11-22 15:51 | 1 | 0 | lote L3; implementer (claude-sonnet-5-5); motivo: otro modelo |
| F2.1, F2.2 | 2026-11-22 15:50 | 2026-11-22 15:51 | 1 | 0 | lote L4; implementer (claude-sonnet-5-5); motivo: otro modelo |
| F3.1, F3.2 | 2026-11-22 15:50 | 2026-11-22 15:51 | 1 | 0 | lote L5; implementer (claude-sonnet-5-5); motivo: otro modelo |
| F4.1, F4.2, F4.3 | 2026-11-22 15:50 | 2026-11-22 15:51 | 1 | 0 | lote L6; implementer (claude-sonnet-5-5); motivo: otro modelo |
| F2.3 | 2026-11-22 15:50 | 2026-11-22 15:51 | 1 | 0 | lote L7; implementer (claude-sonnet-5-5); motivo: otro modelo |
| F5.1, F5.2, F5.3, F5.4 | 2026-11-22 15:50 | 2026-11-22 15:52 | 2 | 0 | lote; sesión (claude-opus-5-5): réplica, versión, ADR, manifiesto y E20 |
| validate | 2026-11-22 15:51 | 2026-11-22 15:53 | 2 | 0 | vuelta 1 CAMBIOS NECESARIOS (1 línea), vuelta 2 APROBADO; tester (claude-haiku-5-5, motivo: otro modelo), implementer de corrección (claude-sonnet-5-5, motivo: otro modelo), reviewer en el chat (claude-opus-5-5) |
| cierre | 2026-11-22 15:55 | 2026-11-22 15:56 | 1 | 0 | sesión principal (claude-opus-5-5); `Cerrado`, el criterio 2 con los commits; chequeo de cierre E20 OK |

## Plan contra real
| Fase | Estimado | Real | Delta |
|---|---|---|---|
| F0 | 3 min | 1 min | -2 min |
| F1 (lotes L1 a L3) | 6 min | 3 min | -3 min |
| F2 (lotes L4 y L7) | 10 min | 2 min | -8 min |
| F3 (lote L5) | 5 min | 1 min | -4 min |
| F4 (lote L6) | 6 min | 1 min | -5 min |
| F5 | 8 min | 2 min | -6 min |
| **Total implementación** | **38 min** | **10 min** | **-28 min** |
| Documentación SDD (spec, clarify, plan, tasks, validate) | sin estimado | 7 min | — |

Las filas de lote cuentan en la fase de su primer paso, y los lotes paralelos suman cada uno su minuto.
En reloj, la ola 1 tardó unos 2 min. Qué ajustar: con lotes en una ola, estimar por ola, de 2 a 3 min
por ola y no por paso (`MEMORY.md`, 2026-11-22).
