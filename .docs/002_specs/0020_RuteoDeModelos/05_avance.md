# 05 — Avance: Ruteo de modelos
Leyenda: `[ ]` Pendiente · `[~]` En curso · `[x]` Hecho · `[!]` Bloqueado

## PUNTO DE RETOMO — leer esto primero
| Dato | Contenido |
|---|---|
| Modo | `completo` sin flujo (decisión 36). Condiciones de [topes-y-modos.md](../../../.agents/skills/sdd/topes-y-modos.md): 1 no (más de 3 archivos: `restricciones-de-rol.md`, `SKILL.md`, `git-commit/SKILL.md`, comandos delegables, rol nuevo, `configuracion.ejemplo.md`, `sdd-config.md`); 2 sí; 3 no (cambia «Dónde corre cada rol» y «Solo roles del kit»); 4 no (alternativa real: delegación fija frente a `model:` por turno o cambio manual; ADR); 5 sí; 6 no (cambia dónde corren comandos y commits); 7 no (más de 1 h); 8 no (`agents`, `commands` y `skills`) |
| Última sesión | 2026-11-22 19:15 |
| Fase actual | `cierre` |
| Último paso cerrado | cierre (`Cerrado`; commits confirmados por la persona) |
| Siguiente paso | Ninguno en 0020. Siguiente requerimiento: plan contra real en vivo en `05_avance.md` (decisión 19) |
| Bloqueos abiertos | Ninguno. CP-09 en Cursor fuera del alcance (decisiones 35 y 38) |
| Estado BD | No aplica |
| Código | Commits del cierre confirmados: `b1ba9ac` (kit y 0.15.0), ADR 0037 y el de esta spec. `template/`: `auxiliar.md` nuevo y 23 archivos modificados, replicados en `.agents/`; raíz: `README.md`, `ACTUALIZAR.md`, versión, changelog, `manifiestos/0.15.0.md`, ADR 0037; raíz: `AGENTS.md`, índice de specs y la carpeta 0020 |

## Pasos
- [x] F0.1 Línea base
- [x] F0.2 Verificación en la documentación de Claude Code
- [x] F1.1, F2.6 (L1)
- [x] F1.2, F1.3 (L2)
- [x] F2.1, F2.2, F4.1 (L3)
- [x] F1.4, F2.3, F2.4, F2.5 (L4)
- [x] F3.1, F3.2, F3.3, F3.4 (L5)
- [x] F5.1 Réplica local
- [x] F5.2 `/sdd-enlaces` (Claude Code)
- [x] F5.3 Chequeos (CP-05 bloqueado, decisión 27)
- [x] F6.1 `/sdd-status`: chat frente a delegado (decisiones 29 a 31)
- [x] F6.2 Preparación de un commit (decisión 32; CP-07 en el cierre)
- [x] F6.3 Decisiones y ajuste (decisión 33)
- [x] F7.1, F7.2, F7.3 ADR, `README.md`, versión y changelog
- [x] F7.4 Manifiesto y E20 (decisión 34)

## Decisiones
1. [2026-11-22 16:14] Orden acordado con la persona: 0020 ruteo de modelos y después 0021 bitácora de
   uso (`.agents/local/idea-0021-bitacora-de-uso.md`).
2. [2026-11-22 16:14] Sin cambio manual de `/model` (persona). El aviso de cambio de modelo baja a
   respaldo para herramientas sin delegación (RF-9).
3. [2026-11-22 16:14] Modo completo: no se cumplen las condiciones 1, 3, 4, 6, 7 y 8.
4. [2026-11-22] Simulación de costo (pedida por la persona; script local
   `.agents/local/medicion/sim_ruteo.py`, precios de la página oficial, caché de 1 h, datos de 355
   subagentes reales). Con 130k de contexto y 20 turnos siguientes: chat Opus 0.04 a 1.22 USD por
   tarea; comando con solo `model:` 0.53 a 2.03 (siempre el peor); fork Sonnet 0.09 a 0.48; Haiku
   0.08 a 0.10. El chat solo gana en una llamada trivial. El costo fijo del subagente es de unos
   10 s (`medicion/resultados.md`). La persona aprobó incorporar a la spec: se prohíbe declarar solo
   `model:` sin subagente, nivel Bajo para extracción literal y Medio para interpretar (RF-1),
   umbral de una llamada corta (RF-6) y relanzamiento Bajo → Medio → sesión (casos límite).
5. [2026-11-22 16:22] Autochequeo del planner antes de la pasada 1: 4 correcciones (definición de
   «Llamada», nivel de RF-12 según la tabla, relanzamiento solo de Bajo a Medio, ADR en criterios).
6. [2026-11-22 16:23] Clarify, pasada 1 (13 hallazgos), corrección automática del planner: tipos de
   tarea suelta como lista cerrada (RF-1); la delegación fija nombra un rol y el nivel sale de
   «Modelo por rol», con excepción en `SKILL.md` para status y estructura (RF-4); encargo del commit
   con el contexto y parada ante decisiones (RF-5); umbral por llamadas previstas (RF-6); aviso solo
   al iniciar comando (RF-9); constancia dentro de fase (RF-10); ajuste por dev con el mecanismo de
   roles (RF-11); ahorro medido en USD (RF-12); 5 casos límite nuevos. Supuestos marcados para
   revisión: ahorro en USD, umbral por llamadas, espera en primer plano.
7. [2026-11-22 16:24] Clarify, pasada 2 por delta (1 hallazgo: `auxiliar` dentro de fase): RF-7 y
   RF-8 permiten el rol dentro de una fase solo para preparar commits y delegaciones fijas.
8. [2026-11-22 16:24] Clarify, pasada 3 por delta (1 hallazgo, al tope): RF-5 cubre los commits que
   propone una fase; queda como supuesto para revisión al aprobar. Sin marcadores `[ACLARAR]`.
9. [2026-11-22 16:27] La persona aprueba `01_spec.md` con los 4 supuestos (ahorro en USD, umbral por
   llamadas previstas, espera en primer plano, RF-5 cubre los commits de `/sdd-cierre`).
10. [2026-11-22 16:29] Cambio de requisito (`/sdd-change`, decisión de la persona): entra al alcance
    el error de argumentos de los comandos `/sdd-*` (`$1` es el segundo argumento en Claude Code,
    base 0; en OpenCode, base 1). RF-13 y RF-14 nuevos, 4 casos límite, 1 exclusión y 1 criterio.
    La spec vuelve a `borrador` hasta que la persona apruebe el cambio. Sin `02`, `03` ni `04`
    todavía: no hay documentos que corregir; el plan lo incluirá y crece el estimado (unos 15
    comandos de `template/.agents/commands/`). Fuentes: code.claude.com/docs/en/skills y
    opencode.ai/docs/commands (consulta de claude-code-guide, Sonnet, 2026-11-22).
11. [2026-11-22 16:34] La persona aprueba el cambio: `01_spec.md` vuelve a `aprobada` con 14 RF.
12. [2026-11-22 16:52] Plan, RF-7 (persona): un solo rol nuevo `auxiliar` de nivel Medio; lo Bajo de
    buscar o leer archivos va a `explore`; lo Bajo que pide `Bash` sube a `auxiliar` o queda en el
    chat. Descartados: un rol Bajo nuevo y `auxiliar` con modelo por llamada.
13. [2026-11-22 16:53] Plan, conectores (persona): la lectura de conectores queda no delegable y en el
    chat; `auxiliar` no lista red ni MCP y no se toca «Ningún agente usa la red».
14. [2026-11-22 16:55] Plan: el planner marca como ADR la delegación fija con subagente (`context:
    fork` + `agent:`) frente a `model:` sin subagente y al cambio manual (`0037`, criterio 7 de la
    spec). No se crea sin visto bueno; F7 lo redacta.
15. [2026-11-22 17:01] La persona aprueba `03_plan.md` y el ADR 0037 («procede»).
16. [2026-11-22 17:01] Persona: OpenCode y Cursor se prueban en su momento dentro de esta versión;
    se arranca con Claude Code. F0 verifica solo Claude Code; la prueba de las otras dos es F5.4
    (CP-09), manual y necesaria para el cierre.
17. [2026-11-22 17:12] La persona pide parar (punto de retomo), sin motivo. Sin trabajo en segundo
    plano: ningún subagente ni proceso lanzado en la sesión.
18. [2026-11-22 18:15] La persona aprueba `04_tasks.md` y `07_casos_prueba.md` («Aprobar y
    arrancar»). Arranca la cascada de `/sdd-implement 0020_RuteoDeModelos` desde F0.1. El argumento
    llegó como `spec_20` y se resolvió a la única carpeta con número 20.
19. [2026-11-22 18:16] La persona pide, al terminar lo que está en curso, que `05_avance.md` muestre
    plan contra real mientras se avanza (estimado de cada fase junto a lo real), sin esperar al
    cierre. Queda fuera de esta spec; se propone como spec aparte al terminar.
20. [2026-11-22 18:18] F0.1: `/sdd-estructura` en la sesión: `estructura OK (E1–E26)` con 3 avisos
    (E1/E3: `claudio.md` sin adaptador en `.cursor/agents` y `.opencode/agents`, rol local admitido;
    E19: diferencias admitidas en `MEMORY.md`; E22: la celda 0020 seguía en `Planificado`). E20 sin
    fallas (54 archivos, 54 filas de `manifiestos/0.14.0.md`). Bandera `alineada: si`.
    `git status --short template/` vacío.
21. [2026-11-22 18:18] F0.2: la documentación (code.claude.com/docs/en/skills, leída el 2026-11-22
    con web-fetch) confirma que un archivo de `.claude/commands/` admite el mismo frontmatter que una
    skill (`context: fork`, `agent:`) y que `$N` es de base 0. Lo que la página no decía (modelo del
    fork, sustitución antes del fork, `$ARGUMENTS` vacío) se confirmó con un comando temporal
    `zz-prueba-fork` (`context: fork`, `agent: explore`), ya borrado, en Claude Code 2.1.330: corre
    en `claude-haiku-5-5` (el modelo de `explore`); con `hola mundo`, `$ARGUMENTS` = `hola mundo` y
    `$0` = `hola`; sin argumentos, `$ARGUMENTS` queda vacío y `$0` literal. Los transcripts muestran
    además que con argumentos y sin marcador la herramienta añade `ARGUMENTS: <texto>` al final, y
    que `$1` sin segundo argumento queda literal. Ojo: invocar dos forks a la vez lanzó uno en
    segundo plano.
22. [2026-11-22 18:21] Ola 1 revisada por la sesión (diff y «Hecho cuando» de cada paso). Dos
    correcciones de la sesión dentro del alcance de su paso: F2.3, el renglón de `AGENTS.md` (raíz y
    plantilla) decía «se lee antes de delegarla» y pasa a «nivel y delegación … se lee antes de
    empezarla» (RF-6: la decisión es antes de la primera llamada); F4.1, `git-commit/SKILL.md`: la
    fase 3 escribe `_git_msg.txt` y `auxiliar` no escribe archivos, así que `auxiliar` propone el
    mensaje como texto y el archivo temporal lo escribe la sesión en la fase 4.
23. [2026-11-22 18:21] Hueco del plan: tres frases fuera de `03_plan.md` § 2 contradicen la delegación
    fija de `/sdd-estructura`: `validar-estructura.md` › «Alcance» («La sesión principal corre E1 a
    E26. No se delegan a un subagente»), `coordinator.md` › «Qué hace» («`sdd-estructura.md` y
    `sdd-enlaces.md` los ejecuta la sesión principal, no un subagente») y `sdd-tasks.md` paso 2
    («citan `/sdd-estructura`, que corre la sesión; ningún paso delega esa validación»). Se consulta
    a la persona (cambio fuera del plan).
24. [2026-11-22 18:24] La persona aprueba corregir las tres frases de la decisión 23 («Corregir y
    anotar»). Las corrige la sesión: `validar-estructura.md` › «Alcance» (E1 a E26 por delegación fija
    con `auxiliar`, nunca el `reviewer`), `coordinator.md` › «Qué hace» (`sdd-enlaces` en la sesión;
    `sdd-estructura` y `sdd-status` por delegación fija) y `sdd-tasks.md` paso 2. `03_plan.md` § 2
    suma las tres filas. Lint sin fallas.
25. [2026-11-22 18:28] F5.3: `/sdd-estructura` (fork, corrió como `general-purpose`, `claude-opus-5-5`,
    128k tokens, 204 s) sin `FALLA` en E8, E9 ni E23 (E23 da `auxiliar: sonnet` bien). Dos `FALLA`
    esperadas: E20 (24 huellas distintas y `auxiliar.md` sin fila en `manifiestos/0.14.0.md`; la
    resuelve F7.4) y E3 (falta el adaptador `auxiliar.md` en `.cursor/agents/` y `.opencode/agents/`;
    se crea con `/sdd-enlaces` antes de F5.4). Avisos: filas de Gemini y GitHub sin `kit-visto`, y
    `description` vieja en adaptadores de `.opencode/agents/` y en `.cursor/agents/implementer.md`.
    La línea base de F0.1 (en la sesión) no midió esos avisos: fue menos exhaustiva que esta corrida.
    Bandera `alineada: no`.
26. [2026-11-22 18:28] La persona pide parar y abrir una sesión nueva («Pausa y sesión nueva»): `auxiliar` no
    cargó en esta sesión (decisión 27). Aprueba además anotar en el kit que un rol nuevo exige abrir
    una sesión nueva (no basta `/clear`) tras instalar o actualizar (a revisar: decisión 27 corregida,
    la herramienta recarga con retraso); se hace en F7 (changelog y la
    guía de actualización) y se suma a `03_plan.md` § 2 al retomar. Sin trabajo en segundo plano: los
    subagentes de la sesión terminaron.
27. [2026-11-22 18:29] F5.3, hallazgo (corregido a las 18:29): en esta sesión, `/sdd-status` y
    `/sdd-estructura` (ya con `context: fork` y `agent: auxiliar`) corrieron como `general-purpose` en
    `claude-opus-5-5`, no como `auxiliar` en Sonnet (`ListAgents` y el modelo que declaró el
    subagente). Causa: la herramienta no tenía todavía el agente `auxiliar.md`, creado a las 18:24; lo
    registró a las 18:28, en la misma sesión (aviso de la herramienta: «New agent types are now
    available»). Primero se supuso que solo carga al abrir la sesión; no es así: recarga con retraso.
    Mientras no lo tiene, el fork cae sin avisar a `general-purpose` con el modelo de la sesión, lo que
    choca con «Solo roles del kit» y anula el ahorro. Además, invocados por la sesión, los forks se
    lanzaron en segundo plano, no en primer plano. CP-05 bloqueado hasta repetirlo con `auxiliar`
    cargado.
28. [2026-11-22 18:30] La persona decide seguir en esta sesión («Seguir aquí»): `auxiliar` ya figura entre los
    tipos de agente (decisión 27). Se retira la pausa de las 18:28 sin hueco que medir. La nota del kit
    de la decisión 26 pasa a decir: tras crear o actualizar un rol, esperar a que la herramienta lo
    cargue o abrir una sesión nueva; mientras no lo carga, la delegación cae en `general-purpose`.
29. [2026-11-22 18:32] F6.1, medición de `/sdd-status 0020_RuteoDeModelos` (transcripts de la sesión y de los
    subagentes; precios USD/MTok de platform.claude.com/docs/en/about-claude/pricing del 2026-11-22,
    los de `.agents/local/medicion/sim_ruteo.py`, caché de 1 h; script `medir.py` en el scratchpad).
    Contexto de la sesión: unos 210k a 220k tokens.
    - Chat, nivel Alto (`claude-opus-5-5`): 2 llamadas (lecturas en una sola llamada y respuesta);
      439 569 de caché leída, 4 557 escritos, 2 712 de salida; 0.18 USD (cota alta: la 2.ª llamada
      incluye el script de medición); 14 s.
    - Delegado (`auxiliar`, `claude-sonnet-5-5`): subagente de 4 llamadas, 31 718 leídos, 15 942
      escritos, 2 862 de salida, 0.096 USD, 18 s; más la llamada de lanzamiento de la sesión (211 260
      leídos, 0.057 USD) y la llamada que recibe y transmite el resultado (unos 0.06 USD). Total ≈ 0.21
      USD; unos 25 s.
    - Delegado caído en `general-purpose` (`claude-opus-5-5`, decisión 27): 0.33 USD solo el subagente.
    - Resultado: con esta sesión grande, delegar `/sdd-status` no ahorra (≈ 0.21 contra ≈ 0.15 a 0.18
      USD) y tarda más. El costo fijo de la delegación (lanzar y recibir: dos llamadas de la sesión
      sobre todo su contexto) ya iguala al chat cuando el chat resuelve la tarea en una o dos llamadas.
      Referencia, no medida para RF-12: la línea base de estructura en el chat (F0.1, con F0.2
      intercalado) fueron 18 llamadas y 0.97 USD; ahí cada llamada de la sesión relee unos 0.04 USD de
      contexto y la delegación sí tiene margen.
    - Según F6.3 y RF-12, la fila `/sdd-status` sale de «Nivel por tarea sin rol». Se consulta a la
      persona antes de aplicarlo, porque revierte F3.1 y la muestra es de una sola corrida.
30. [2026-11-22 18:41] La persona pide medir otra vez antes de decidir («Medir otra vez»: chat al pie de la
    letra, contexto chico). La muestra ya existe: la corrida de CP-05 que cayó en `general-purpose`
    es Opus con contexto propio de unos 36k tokens que sigue el comando al pie de la letra (4
    llamadas): 145 004 leídos, 28 945 escritos, 3 231 de salida, 0.33 USD, 28 s. Con contexto chico, la
    delegación en `auxiliar` cuesta el subagente (0.096 USD) más dos llamadas de la sesión sobre unos
    36k (≈ 0.05 USD): ≈ 0.15 USD, un 55 % menos. Lectura conjunta: con una sesión chica o con lectura
    al pie de la letra, la delegación fija ahorra; con una sesión grande y el chat que junta las
    lecturas en una llamada, cuesta un 20 % más. Se pide la decisión a la persona con las dos muestras.
31. [2026-11-22 18:42] La persona decide mantener `/sdd-status` con delegación fija en `auxiliar` («Mantener
    delegado»), con las muestras de las decisiones 29 y 30 como evidencia de RF-12: ahorra un 55 % en
    sesión chica y cuesta un 20 % más en una sesión grande cuyo chat junta las lecturas. Excepción de la
    persona a «si no ahorra, sale de la tabla» para el caso de sesión grande. F6.3 lo recoge.
32. [2026-11-22 18:44] F6.2, preparación del commit de `template/` (25 archivos), con los precios de la
    decisión 29. Chat (sesión de unos 239k tokens, claude-opus-5-5): 3 llamadas, 718 k leídos, 3 743
    escritos, 1 240 de salida, 0.20 USD, 11 s; siguió la skill (`seguridad-basica` y paso 7 de
    `sync-agents.md`). Delegado en `auxiliar` (claude-sonnet-5-5, confirmado en su `meta.json`): el
    subagente, 4 llamadas, 0.090 USD, 16 s; más el lanzamiento (0.072 USD) y la recepción (≈ 0.06 USD,
    estimada como en F6.1: la real cayó tras la compactación y no es comparable): ≈ 0.22 USD, un 11 %
    más, y ≈ 0.29 USD (47 % más) si la espera es en segundo plano y la sesión cierra el turno. Además,
    `auxiliar` hizo los secretos con un grep propio, sin leer `seguridad-basica`, y no corrió el paso 7
    de `sync-agents.md`. El commit cae al final del trabajo, con la sesión grande: esta muestra es la
    representativa. Stage sacado tras cada variante (0 archivos). CP-07 queda para el cierre.
33. [2026-11-22 18:45] F6.3: una decisión por resultado. `/sdd-status`: delegación fija por decisión de
    la persona (31). `/sdd-estructura`: sigue en la tabla sin medición propia; corre con la misma
    regla que `/sdd-status`. Preparación de un commit: no ahorra (32), y por RF-12 su fila sale de
    «Nivel por tarea sin rol». Ajuste: `git-commit/SKILL.md` vuelve a su versión de `HEAD` (deshace
    F4.1); `auxiliar.md` y `sdd/SKILL.md` dejan de nombrar la preparación de commits; el renglón de
    `AGENTS.md` (plantilla y raíz) pierde «o commit»; una frase bajo la tabla dice por qué no está.
    `03_plan.md` §2 anota las filas de `git-commit` y `AGENTS.md`. Replicado en `.agents/`. CP-07
    cambia su resultado esperado: el chat prepara y, con confirmación, commitea.
34. [2026-11-22 18:47] F7: ADR 0037 aceptado (aprobado con el plan, decisión 15) y su fila en el índice.
    `README.md` de la raíz con `auxiliar` y el ruteo. `VersionAgentKitSdd` en 0.15.0 y entrada en el
    changelog. La nota de las decisiones 26 y 28 va al changelog y a `ACTUALIZAR.md` › «Qué hacer
    después de un pull…» (paso 4 nuevo), y su fila a `03_plan.md` § 2. `manifiestos/0.15.0.md` sale del
    árbol de `template/`: 23 huellas cambian y se agrega `auxiliar.md`, sin retirados. E20 sin `FALLA`
    (55 archivos, 55 filas).
35. [2026-11-22 19:08] La persona deja CP-09 (F5.4, OpenCode y Cursor) pendiente y fuera del alcance de
    la versión 0.15.0: se publica sin esa prueba y la corre en la semana. F5.4 queda sin marcar; en el
    cierre se registra como deuda (o se anota la corrida si ya se hizo).
36. [2026-11-22 19:10] La persona omite el flujo («el flujo se omite»): sin `06_flujo.md`; el cambio es
    del kit (Markdown) y no tiene usuario final de una app. El modo queda «completo sin flujo», como en
    0018, y el cierre no lo exige.
37. [2026-11-22 19:11] La persona corre CP-09 en OpenCode: ok (corrida en `07_casos_prueba.md`). Falta Cursor,
    que sigue diferido (decisión 35). El estado delegado avisó un desfase de la celda («En curso»
    frente a «En pruebas»): con F5.4 fuera del alcance todos los pasos que cuentan están hechos, así que
    la sesión escribe `En pruebas`.
38. [2026-11-22 19:15] Cierre: la persona no da de alta deuda («Ninguna») para CP-09 en Cursor ni para
    el color de `auxiliar` en `formato-respuesta` (adaptador de OpenCode sin `color:`). CP-09 en Cursor
    queda fuera del alcance de esta versión sin deuda (decisión 35); no es verificación pendiente de 0020.
    RF-5 y RF-8 conservan en `01_spec.md` la delegación de la preparación de commits que RF-12 retiró
    (decisión 33): la spec no se reescribe; esta decisión deja la constancia.
## Pruebas
- F0.1 y F0.2: ver decisiones 20 y 21. Casos: `07_casos_prueba.md`.
- validate (2026-11-22 19:03): `VEREDICTO: APROBADO SALVO VERIFICACIÓN MANUAL`. Corridas en [07_casos_prueba.md](07_casos_prueba.md). CP-09: ok en OpenCode (decisión 37); Cursor fuera del alcance (decisiones 35 y 38). CP-07: se completa con los commits del cierre.

## Tiempos
| Paso | Inicio | Fin | Minutos | Pausa | Nota |
|---|---|---|---|---|---|
| spec | 2026-11-22 16:12 | 2026-11-22 16:14 | 2 | 0 | planner en el chat (mismo modelo), claude-opus-5-5; 12 RF; sin preguntas, 3 supuestos |
| change | 2026-11-22 16:28 | 2026-11-22 16:29 | 1 | 0 | planner en el chat (mismo modelo), claude-opus-5-5; RF-13 y RF-14 (argumentos de comandos); consulta de documentación con claude-code-guide (Sonnet, pedido implícito de verificar) |
| clarify | 2026-11-22 16:22 | 2026-11-22 16:24 | 2 | 0 | pasada 3 de 3; reviewer y planner en el chat (mismo modelo), claude-opus-5-5; autochequeo con 4 correcciones; hallazgos 13, 1 y 1; al tope, 1 supuesto |
| plan | 2026-11-22 16:50 | 2026-11-22 16:55 | 5 | 0 | planner en el chat (mismo modelo), claude-opus-5-5; 2 preguntas de diseño a la persona (decisiones 12 y 13); ADR 0037 marcado |
| tasks | 2026-11-22 17:01 | 2026-11-22 17:03 | 2 | 0 | planner en el chat (mismo modelo), claude-opus-5-5; 28 pasos en 8 fases y 5 olas; `07_casos_prueba.md` con 10 casos |
| F0.1, F0.2 | 2026-11-22 18:15 | 2026-11-22 18:18 | 3 | 0 | lote; sesión, claude-opus-5-5; F0.2 con web-fetch y un fork de prueba en `explore` (claude-haiku-5-5), motivo «otro modelo» |
| F1.1, F2.6 | 2026-11-22 18:19 | 2026-11-22 18:20 | 1 | 0 | lote L1; implementer (subagente), claude-sonnet-5-5, motivo «otro modelo»; ola 1 en paralelo |
| F1.2, F1.3 | 2026-11-22 18:19 | 2026-11-22 18:21 | 2 | 0 | lote L2; implementer (subagente), claude-sonnet-5-5, motivo «otro modelo»; 10 comandos sin `$N` |
| F2.1, F2.2, F4.1 | 2026-11-22 18:19 | 2026-11-22 18:21 | 2 | 0 | lote L3; implementer (subagente), claude-sonnet-5-5, motivo «otro modelo»; corrección de la sesión en F4.1 (decisión 22) |
| F1.4, F2.3, F2.4, F2.5 | 2026-11-22 18:19 | 2026-11-22 18:21 | 2 | 0 | lote L4; implementer (subagente), claude-sonnet-5-5, motivo «otro modelo»; corrección de la sesión en F2.3 (decisión 22) |
| F3.1, F3.2, F3.3, F3.4 | 2026-11-22 18:19 | 2026-11-22 18:21 | 2 | 0 | lote L5; implementer (subagente), claude-sonnet-5-5, motivo «otro modelo» |
| F5.1, F5.2, F5.3 | 2026-11-22 18:24 | 2026-11-22 18:28 | 4 | 0 | lote; sesión, claude-opus-5-5; `/sdd-estructura` y `/sdd-status` delegados cayeron en `general-purpose` (claude-opus-5-5), motivo «ruteo» fallido (decisión 27) |
| F6.1 | 2026-11-22 18:30 | 2026-11-22 18:32 | 2 | 0 | sesión, claude-opus-5-5; variante delegada con `auxiliar` (claude-sonnet-5-5), motivo «ruteo»; medición en la decisión 29 |
| F6.2 | 2026-11-22 18:42 | 2026-11-22 18:44 | 2 | 0 | sesión, claude-opus-5-5; variante delegada con `auxiliar` (claude-sonnet-5-5), motivo «ruteo»; medición en la decisión 32 |
| F6.3 | 2026-11-22 18:44 | 2026-11-22 18:45 | 1 | 0 | sesión, claude-opus-5-5; sale la fila de commit (decisión 33) |
| F7.1, F7.2, F7.3 | 2026-11-22 18:45 | 2026-11-22 18:47 | 2 | 0 | lote; sesión, claude-opus-5-5 |
| F7.4 | 2026-11-22 18:47 | 2026-11-22 18:47 | 0 | 0 | sesión, claude-opus-5-5; E20 sin `FALLA` |
| validate | 2026-11-22 19:02 | 2026-11-22 19:03 | 1 | 0 | vuelta 1 de 2; tester (subagente), claude-haiku-5-5, motivo «otro modelo», 5 casos; reviewer en el chat (mismo modelo), claude-opus-5-5; aprobado salvo CP-09 y CP-07 |
| cierre | 2026-11-22 19:12 | 2026-11-22 19:15 | 3 | 0 | sesión principal (claude-opus-5-5); `Cerrado`, el criterio 2 con los commits; chequeo de cierre E20 OK (55 archivos, 55 filas) |

## Plan contra real
| Fase | Estimado | Real | Delta |
|---|---|---|---|
| F0 | 10 min | 3 min | -7 min |
| F1 (lotes L1, L2 y L4) | 15 min | 5 min | -10 min |
| F2 (lote L3) | 20 min | 2 min | -18 min |
| F3 (lote L5) | 15 min | 2 min | -13 min |
| F4 (dentro de L3) | 10 min | 0 min | -10 min |
| F5 | 10 min | 4 min | -6 min |
| F6 | 30 min | 5 min | -25 min |
| F7 | 15 min | 2 min | -13 min |
| **Total implementación** | **125 min** | **23 min** | **-102 min** |
| Documentación SDD (spec, change, clarify, plan, tasks, validate, cierre) | sin estimado | 16 min | — |

Las filas de lote cuentan en la fase de su primer paso, y los lotes paralelos suman cada uno su minuto.
Ajuste para el siguiente plan: los estimados del kit siguen de 4 a 5 veces altos; estimar unos 2 min
por lote del implementer y 5 min por medición, en vez de minutos por paso.
