# 03 — Plan: Arranque ligero de sesión
Spec: 01_spec.md (aprobada 2026-11-21 11:00)

## 1. Ubicación y capas
Kit de Markdown. Lo distribuible se edita en `template/` y se replica en el `.agents/` local de la
raíz. El `AGENTS.md` de la raíz (versionado, entorno de mantenimiento) se edita aparte con el mismo
cambio. Docs de la raíz: `ACTUALIZAR.md`, `VersionAgentKitSdd`, `ChangelogAgentKitSdd.md` y el
manifiesto en `manifiestos/`. Fuente única (constitución, punto 3): cada regla movida queda en un solo
lugar y `AGENTS.md` solo deja un renglón de remisión.

Idea central:
- **#6, lectura del arranque:** `sync-agents.md` se reordena para que lo que lee el chequeo de arranque
  quede junto y al inicio, y «Chequeo de arranque» dice qué más leer. En el clon (sin
  `.agents/.kit-version`, con `template/`) su punto 1 lo omite y no se lee nada más. El paso 4
  (`.agents/README.md`) pasa a condicional.
- **#5, `AGENTS.md`:** salen «Qué lee cada rol», «Roles y comandos» y «Formato y numeración», cada una
  con su renglón. Se quedan «Deuda técnica» (su regla 2, proponer deuda al diferir, aplica en
  cualquier turno; su regla 1 es parte de «Antecedentes») y «Skills de proyecto» (E7, filas que agrega
  `/sdd-init` y ruteo de cada turno).

## 2. Archivos que se crean o modifican
| Archivo | Capa | Responsabilidad | RF |
|---|---|---|---|
| `template/.agents/sync-agents.md` (y réplica raíz) | distribuible | Orden nuevo: encabezado, «Chequeo de arranque», «Estados de enlace», «Estado «visto»», «Qué hace el agente», «Tabla de enlaces» y lo demás sin cambio. «Chequeo de arranque» abre con un renglón «Qué lee»: si el punto 1 lo omite, nada más; si no, «Estados de enlace», «Estado «visto»» y, de «Tabla de enlaces», solo las filas de las herramientas declaradas; no «Qué hace el agente» ni «Notas para el usuario». «Estados de enlace» es sección nueva con la tabla de estados (sale del paso 1 de «Qué hace el agente», que remite a ella) y las dos frases de `OK` por archivo y de adaptadores de OpenCode (salen de «Carpeta de agentes por archivo» y «OpenCode», que remiten a ella). Se mueven, no se reescriben | RF-7, RF-8, RF-9, RF-13 |
| `template/.agents/commands/sdd-enlaces.md` (y réplica) | distribuible | Línea «Qué lee»: nombra «Estados de enlace» en vez de «estados» | RF-5 |
| `template/AGENTS.md` y `AGENTS.md` (raíz) | distribuible / mantenimiento | Paso 4: condicional («Antes de crear o modificar un agente, skill o comando, también a media sesión o en `/sdd-init`: `.agents/README.md`»). Paso 7: «lee `.agents/sync-agents.md` › «Chequeo de arranque» y solo lo que esa sección indica». Paso 8: remite a `.docs/002_specs/README.md` › «Numeración única» solo si hay repetidos. Salen «Qué lee cada rol», «Roles y comandos» y «Formato y numeración». Quedan tres renglones de remisión; el de roles conserva en `AGENTS.md` dos reglas: un subagente no repite «Cada sesión», y «Guardrails» se lee siempre sin contar contra el tope de `sdd-implement` ni la lista cerrada de `reviewer`. «Guardrails» no cambia | RF-2, RF-3, RF-4, RF-6, RF-10, RF-12 |
| `template/.agents/README.md` (y réplica) | distribuible | Sección nueva «Qué va en `AGENTS.md`»: el criterio de siempre cargado frente a bajo demanda (fuente única, RF-1). Sección nueva «Roles y comandos» (movida tal cual desde `AGENTS.md`). La línea 78 apunta a ella | RF-1, RF-4, RF-5 |
| `template/.agents/validar-estructura.md` (y réplica) | distribuible | E8: los roles coinciden con `.agents/README.md` › «Roles y comandos». E9: los comandos citados ahí existen y los que existen están citados ahí. Si el `AGENTS.md` del proyecto aún trae su sección vieja, no es `FALLA` (RF-13). E17 cita también el criterio de «Qué va en `AGENTS.md`» | RF-11, RF-13 |
| `ACTUALIZAR.md` | doc raíz | Grupo B: qué incorporar a mano en un `AGENTS.md` existente (pasos 4, 7 y 8 nuevos). Las tres secciones que salen pueden quedarse: no rompen nada, solo dejan de ahorrar | RF-13 |
| `.docs/004_adr/0032_CriterioDeArchivoSiempreCargado.md` | mantenimiento | ADR del criterio (registrado en `plan`, 2026-11-21) | RF-1 |
| `VersionAgentKitSdd`, `ChangelogAgentKitSdd.md`, `manifiestos/0.12.0.md` | doc raíz | Versión 0.12.0 | RNF |
| `05_avance.md` | spec | Medición F0 y final (la escribe la sesión principal) | RF-14 |

## 3. Base de datos
No aplica: sin base de datos ni `sql/`.

## 4. Seguridad
Sin secretos ni credenciales. «Guardrails» queda en `AGENTS.md` de las dos capas con el texto
idéntico (verificación por `diff` de la sección en F4). El chequeo de arranque sigue siendo de solo
lectura.

## 5. Liberación / piloto
Versión 0.12.0. Los proyectos instalados se actualizan con `ACTUALIZAR.md`: `.agents/` se reemplaza
si es KIT-VIEJO (ADR 007) y `AGENTS.md` (grupo B) se ajusta a mano. Mientras no se ajuste, el arranque
viejo sigue funcionando (RF-13).

## 6. Decisiones
| Decisión | Alternativa descartada | Por qué |
|---|---|---|
| Reordenar `sync-agents.md` y que «Chequeo de arranque» diga qué leer | Partirlo en un archivo nuevo de chequeo | Un `AGENTS.md` viejo cita `sync-agents.md` › «Chequeo de arranque»: con el mismo archivo y la misma sección resuelve sin tocar nada (RF-13). Un archivo nuevo obliga a dejar remisiones en los dos |
| La omisión del clon sigue en el punto 1 del chequeo | Ponerla en el paso 7 de `AGENTS.md` | Sería una segunda fuente (RF-9) y un `AGENTS.md` viejo no la tendría |
| Sale «Qué lee cada rol» sin moverse a otro archivo; solo quedan sus dos reglas | Moverla a `restricciones-de-rol.md` | Cada `.agents/agents/<rol>.md` ya declara su «Qué lee»; la tabla es una copia y ya diverge (`explore`). Las dos reglas generales sí se quedan en `AGENTS.md` |
| «Roles y comandos» va a `.agents/README.md` | Dejarla en `AGENTS.md` | Solo se usa al elegir o crear un rol o comando; Claude Code y OpenCode ya listan los comandos |
| Sale «Formato y numeración» | Moverla a otro archivo del kit | Duplica `.docs/002_specs/README.md` › «Numeración única» y la skill `sdd` |
| Se quedan «Deuda técnica» y «Skills de proyecto» | Sacarlas | Se usan en cualquier turno (proponer deuda al diferir, ruteo a skills) y E7 y `/sdd-init` las leen ahí |
| Meta de `AGENTS.md` contra la línea base congelada | Contra la medición de hoy | Lo pide la spec. La medición de F0 queda como referencia, porque la 0013 y la 0014 sumaron ~1 kB que se queda |
| ADR del criterio de archivo siempre cargado: [ADR 0032](../../004_adr/0032_CriterioDeArchivoSiempreCargado.md) | Solo dejarlo en el plan | Tiene alternativas reales y efecto duradero en cada sección futura. Visto bueno 2026-11-21 16:12 |

## 7. Fases y estimado
| Fase | Contenido | RF | Estimado (h) |
|---|---|---|---|
| F0 | Diagnóstico / preparación: registrar en `05_avance.md` la línea base congelada y la medición de hoy (`02_analisis_impacto.md` § 3); guardar en el scratchpad el `template/AGENTS.md` de `HEAD` para el ensayo de actualización | RF-14 | 0.25 |
| F1 | `sync-agents.md`: orden nuevo, «Estados de enlace», renglón «Qué lee» del chequeo; `sdd-enlaces.md`; réplica en la raíz | RF-7, RF-8, RF-9 | 0.75 |
| F2 | `AGENTS.md` (las dos capas): pasos 4, 7 y 8, salida de las tres secciones con sus renglones. `.agents/README.md`: criterio y «Roles y comandos». `validar-estructura.md`: E8, E9 y E17. Réplicas | RF-1 a RF-6, RF-10 a RF-12 | 1.0 |
| F3 | `ACTUALIZAR.md`, versión 0.12.0, changelog y manifiesto | RF-13 | 0.5 |
| F4 | Verificación: lint de `AGENTS.md` › Comandos en cada archivo tocado; `diff` de «Guardrails» contra `HEAD`; E1–E23 en el clon y en el ensayo de instalación; ensayo de actualización (`AGENTS.md` viejo con `.agents/` nuevo); los cuatro escenarios de RF-8 en `mktemp -d`; `diff -rq template/.agents .agents` frente a la línea base; medición final en `05_avance.md` | RF-8, RF-11, RF-13, RF-14 | 0.75 |
| | **Total** | | **3.25** |

## 8. Criterios de aceptación
- **Lectura del arranque bajo control del kit (raíz, escenario base):** baja al menos un 35 % frente a
  24 186 bytes, es decir, ≤ 15 721. Estimado: napkin 1 579 + `MEMORY.md` 3 779 + constitución 1 133 +
  `formato-respuesta` 2 480 + «Chequeo de arranque» ~3 150 ≈ 12 100 (−50 %). Sale `.agents/README.md` y
  el resto de `sync-agents.md`. Referencia, fuera de la meta: un proyecto con herramientas declaradas
  suma «Estados de enlace», «Estado «visto»» y sus filas de la tabla (≈ 15 kB frente a los 29 782 de
  hoy).
- **`AGENTS.md`:** baja al menos un 5 % frente a la línea base congelada, es decir, raíz ≤ 12 648 bytes
  y `template/` ≤ 11 266. Estimado: raíz ~12 200 (−8 %; −15 % frente a los 14 279 de hoy) y `template/`
  ~11 000 (−7 %; −15 % frente a 12 990).
- «Guardrails», «Comandos», «Stack y convenciones de código» y «Reglas de negocio y del sistema» siguen
  en `AGENTS.md` de las dos capas con su texto de antes (RF-2, RF-6, RF-12).
- Los cuatro escenarios de RF-8 dan el mismo aviso o el mismo silencio que antes.
- E1–E23 sin `FALLA` nueva en el clon y en el ensayo de instalación. E11 resuelve toda referencia movida.
- Ensayo de actualización: con el `AGENTS.md` de 0.11.0 y el `.agents/` nuevo, el chequeo da el mismo
  resultado y E8 y E9 no dan `FALLA`.
- `diff -rq template/.agents .agents` sin diferencias nuevas frente a la línea base. Lint de
  `AGENTS.md` › Comandos en cada archivo tocado.
- `ACTUALIZAR.md`, versión 0.12.0, changelog y manifiesto al día.
