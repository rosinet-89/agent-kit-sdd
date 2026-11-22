# 05 — Avance: Pausa, origen del kit y contexto bajo demanda
Leyenda: `[ ]` Pendiente · `[~]` En curso · `[x]` Hecho · `[!]` Bloqueado

## PUNTO DE RETOMO — leer esto primero
| Dato | Contenido |
|---|---|
| Modo | completo sin flujo (decisión 29). Condiciones 1, 3, 4, 6, 7 y 8: no; 2 y 5: sí (lista en `01_spec.md` › «Modo»). |
| Última sesión | 2026-11-22 14:41 |
| Fase actual | `cierre` |
| Último paso cerrado | `cierre`: `Cerrado` (decisión 30). Issues #4, #6, #7, #8, #10 y #11: la persona confirma que quedan atendidos y los cierra ella en GitHub tras el commit (criterio 3) |
| Siguiente paso | Ninguno: requerimiento cerrado |
| Bloqueos abiertos | Ninguno |
| Estado BD | No aplica |
| Código | 18 archivos modificados y 5 nuevos en `template/`, réplica en `.agents/`, `AGENTS.md`, `INSTALAR.md`, `ACTUALIZAR.md`, `README.md`, versión, changelog, `manifiestos/0.13.0.md`, ADR 0033–0035, `.docs/005_contexto/`, leyenda, deudas 0009–0014. Commits locales de esta corrida (decisión 30) |

## Pasos
| Paso | Estado | Nota |
|---|---|---|
| F0.1 | [x] | |
| F0.2 | [x] | |
| F1.1 | [x] | |
| F1.2 | [x] | |
| F1.3 | [x] | |
| F1.4 | [x] | |
| F1.5 | [x] | |
| F2.1 | [x] | |
| F2.2 | [x] | |
| F2.3 | [x] | |
| F2.4 | [x] | |
| F3.1 | [x] | |
| F3.2 | [x] | |
| F3.3 | [x] | |
| F4.1 | [x] | |
| F4.2 | [x] | |
| F4.3 | [x] | |
| F4.4 | [x] | |
| F5.1 | [x] | |
| F5.2 | [x] | |
| F5.3 | [x] | |
| F5.4 | [x] | |
| F6.1 | [x] | |
| F6.2 | [x] | |
| F6.3 | [x] | |
| F7.1 | [x] | |
| F7.2 | [x] | |
| F7.3 | [x] | |
| F7.4 | [x] | |
| F7.5 | [x] | |
| F7.6 | [x] | |
| F8.1 | [x] | |
| F8.2 | [x] | |
| F8.3 | [x] | |
| F8.4 | [x] | |
| F9.1 | [x] | corridas en `07_casos_prueba.md` |

## Decisiones
1. **[2026-11-22 13:39] Una sola spec para los issues #4, #6, #7, #8, #10 y #11 y las deudas 0009, 0010, 0011, 0012, 0013 y 0014.**
   Por qué: lo decidió la persona. Comparten piezas (`AGENTS.md`, `sdd-init`, `sdd-status`, `sdd-cierre`, `validar-estructura`), y una sola versión del kit evita repetir la réplica y el manifiesto. Alternativa descartada: una spec por issue.
2. **[2026-11-22 13:39] El issue #9 queda fuera: se cerró en GitHub como «no se hará».**
   Por qué: lo útil ya está en `0014_ConfiguracionGuiada` (auditor beta). El resto exige un script, y eso choca con el ADR 004. Alternativa descartada: un ADR de excepción.
3. **[2026-11-22 13:45] #6: la consulta de versión usa `git ls-remote` y un clon temporal (`--depth 1` en `mktemp -d`), como excepción explícita y única a la regla Git.**
   Por qué: es la única forma de leer la versión publicada y el changelog sin un clon local; el temporal no toca el proyecto. Respuesta de la persona a la pregunta 1. Alternativas descartadas: solo `ls-remote`; sin red.
4. **[2026-11-22 13:45] #10: `/sdd-init` propone la lista de documentos de contexto, que se crean después uno por uno con visto bueno.**
   Por qué: es el mismo patrón que las skills candidatas, y así no nacen documentos que envejecen. Respuesta a la pregunta 2. Alternativas descartadas: que `/sdd-init` los genere; entregar solo la sección.
5. **[2026-11-22 13:45] #8: las skills se recomiendan desde un catálogo genérico y las candidatas propias. No se nombran fuentes externas ni se instala nada.**
   Por qué: evita recomendar fuentes de confianza dudosa. Respuesta a la pregunta 3. Alternativas descartadas: fuentes externas con criterio de confianza; sin catálogo.
6. **[2026-11-22 13:45] #4: `/sdd-estructura` escribe la bandera local `.agents/local/estructura-ok`, como única excepción a «solo lee», tras comprobarla con `git check-ignore`.**
   Por qué: permite responder «¿está alineada?» sin repetir los chequeos. Respuesta a la pregunta 4. Alternativa descartada: dejarla solo de lectura y sin bandera.
7. **[2026-11-22 13:45] La hora de las decisiones 3 a 6 es la del registro.** Las cuatro respuestas llegaron entre 13:39 y 13:45; no se tomó la hora de cada una por separado.

8. **[2026-11-22 13:49] Clarify, pasada 1: el planner aplica las 18 correcciones (modo automático).** Detalle en `.agents/local/clarify-0017-p1.md`. Supuestos nuevos marcados para revisión: 8 (una carpeta sin fila de índice ya no se detecta en el arranque, solo con E13) y 9 (umbral de 180 días). Opción más acotada en cada caso.
9. **[2026-11-22 13:50] Clarify, pasada 2: el planner aplica las 6 correcciones.** Afectan a RF-4, RF-6, RF-7, RF-14, RF-15 y RF-29. Detalle en `.agents/local/clarify-0017-p2.md`. RF-29: la spec 0012, ya `cerrada`, no se edita; su tensión se resuelve en la leyenda.
10. **[2026-11-22 13:50] Clarify, pasada 3: sin hallazgos nuevos.** Quedan los 3 marcadores [ACLARAR] para la persona.

11. **[2026-11-22 13:54] ACLARAR-1: el cambio pequeño va siempre por el modo ligero, con carpeta (RF-13).** Por qué: lo eligió la persona. Alternativa descartada: no abrir carpeta y correr solo el `grep` de Antecedentes y deuda. Consecuencia: `coordinator.md:16` deja de ofrecer un camino fuera del flujo.
12. **[2026-11-22 13:54] ACLARAR-2: la sesión principal escribe `Planificado`, `En curso` y `En pruebas` en la celda al registrar el cierre de `plan`, del primer paso y del último paso (RF-28).** Alternativas descartadas: que cada comando de fase escriba la celda; dejarlo como está y descartar la deuda 0012.
13. **[2026-11-22 13:54] ACLARAR-3: `/sdd-constitucion` es un comando auxiliar, con ruteo en el `coordinator` y sin fila en la tabla de fases; E10 lo exenta (RF-27).** Alternativa descartada: hacerlo fase del flujo.
14. **[2026-11-22 13:54] Spec aprobada por la persona** (`Estado: aprobada`), con los supuestos 1 a 9, incluidos los marcados 8 y 9. Siguiente: `/sdd-plan`.

15. **[2026-11-22 13:57] Plan redactado con 10 decisiones de diseño (D1 a D10 de `03_plan.md` › 6).** D1: el cuerpo de «Antecedentes» y «Deuda técnica» va a `skills/sdd/antecedentes-y-deuda.md`, porque `.docs/` es Grupo C y una remisión allí quedaría rota en proyectos que actualizan. D10 propone los ADR 0033 a 0035, pendientes del visto bueno de la persona.

16. **[2026-11-22 14:05] Plan aprobado por la persona, con D1 a D10.** D10 incluye los ADR 0033 a 0035, que se redactan en F8.1. Por qué: «apruebo plan».
17. **[2026-11-22 14:05] La persona pide correr las tareas en cascada «lo que se pueda».** Se toma como visto bueno anticipado de `04_tasks.md` derivado del plan aprobado y de encadenar los pasos sin parar entre uno y otro. Paradas que se mantienen: commits (siempre con confirmación), dudas de alcance, un paso que no cumpla su «Hecho cuando» y la validación. Alternativa descartada: parar tras `tasks` y tras cada paso.
18. **[2026-11-22 14:09] Implementación en olas paralelas sobre archivos disjuntos, varios pasos por implementer.** Ola 1: 8 implementers (F1.1–F1.3; F1.4, F1.5, F6.2; F2.1–F2.4; F3.1–F3.3; F4.1, F4.2, F6.1; F5.1, F5.3; F7.1, F7.2; F7.3–F7.6). Ola 2: `sdd-init.md` (F4.3, F5.2) y `sdd-cierre.md` (F4.4, F6.3). Ola 3: F5.4 + F8.3. La sesión marca `04_tasks.md` y hace F0, F8.1, F8.2 y F8.4. Por qué: cascada pedida por la persona (decisión 17); ningún archivo lo tocaron dos implementers a la vez. Desvío del plan (§ 7 «las fases van en orden») y de `/sdd-implement` (un paso por lanzamiento). Implementers: subagente `implementer`, claude-sonnet-5-5. Línea base F0.2 sobre una copia congelada del árbol (subagente general, claude-sonnet-5-5).
19. **[2026-11-22 14:11] Ampliación: la leyenda separa el diferimiento (`Diferido`) de la fila «Pausa» de `/sdd-pausa`.** Lo reportó el implementer de F2. La sesión mandó el aviso por error al implementer de F3, ya terminado, que hizo el cambio en las dos capas; el de F6.1 ya no tocaba el archivo y no hubo choque. E22 aclara lo mismo (implementer de F7.1).
20. **[2026-11-22 14:11] Ajustes de la sesión fuera de un implementer:** `reviewer.md` E1–E23 → E1–E26; leyenda «Cómo usarlo» 5 (etapas intermedias) y «Un diferimiento no baja un `Cerrado`»; «Numeración única» suma `005_contexto` en «Carpeta vacía» y «Revisión de duplicados». Por qué: remisiones que quedaban desfasadas con RF-23, RF-27 y RF-28.
21. **[2026-11-22 14:12] Por revisar en validate:** E24 da FALLA si el marcador de la raíz del kit no cuadra (RF-30 solo dice «informa»; severidad elegida por el implementer); la «Advertencia» del `deny` de `restricciones-de-rol.md` no nombra los clientes de BD nuevos.
22. **[2026-11-22 14:13] Celda del índice → `En pruebas`** (RF-28, ya vigente en «Quién escribe»): los 35 pasos de implementación están hechos; F9.1 es prueba y no cuenta.
23. **[2026-11-22 14:15] E10 avisaba porque `sdd-init` no tenía fila de ruteo (D7 la exige).** La sesión la añadió en `coordinator.md`, replicó y recalculó su huella en `manifiestos/0.13.0.md`. La bandera traía una hora inventada por el subagente (14:30); la sesión la reescribió con la hora real.
24. **[2026-11-22 14:24] La persona aprueba bajar E24 a AVISO y correr validate («procede»).** E24 queda «`AVISO` (nunca `FALLA`)»; réplica y huella recalculadas.
25. **[2026-11-22 14:26] F9.1 se dio por cerrado sin medir el arranque del escenario base (error de la sesión).** Al medirlo en validate: 12 515 > 12 110, por «Chequeo de arranque» (3 139 → 3 544, F1.5). La sesión compactó su redacción sin quitar reglas: 3 136 bytes, suma 12 107. Réplica y huella recalculadas. Lo revisa el reviewer.
26. **[2026-11-22 14:29] El reviewer corre en el chat.** La sesión lo lanzó como subagente en contexto limpio (14:27) y la persona lo corrigió: con el mismo modelo que la sesión, el rol corre en el chat, y la separación entre producir y validar es por rol (`SKILL.md` › «Dónde corre cada rol»). Se detuvo el subagente sin usar su salida. Alternativa descartada: subagente por independencia de contexto.
27. **[2026-11-22 14:30] Validate, vuelta 1: CAMBIOS NECESARIOS (3) más 5 opcionales.** RF-7 (`sdd-version.md:13`: clon sin `GIT_TERMINAL_PROMPT=0` y temporal no borrable), RF-19 (Lectura de la bandera sin `VersionAgentKitSdd`) y RNF-2 («pausa anotada» frente a «anotación de diferimiento»). Opcionales: «Advertencia» del `deny`, cita «sección 3, punto 7», criterio de `.agents/README.md` (ADR 0033), nota del Grupo C y líneas de más de 100 columnas. Un implementer (claude-sonnet-5-5) aplicó los 8; la sesión replicó y recalculó 6 huellas.
28. **[2026-11-22 14:33] Validación del conjunto aprobada** (`VEREDICTO: APROBADO`, vuelta 1 de 2). Reviewer en el chat, claude-opus-5-5. Seguridad en modo «solo secretos»: sin hallazgos. Ajeno: `MEMORY.md` (cambios previos a esta spec).
29. **[2026-11-22 14:37] Sin flujo, por decisión de la persona: excepción registrada.** El modo es completo y la comprobación previa del cierre exige `06_flujo.md` aprobado; el kit no prevé «completo sin flujo». La persona elige la excepción («la spec 17 no requiere flujo, pasamos directo al cierre»): la fila de modo queda en «completo sin flujo» y el cierre no exige el flujo. Por qué: la spec cambia el kit y no hay operación de usuario final (mismo criterio que la decisión 4 de la 0016 y la 25 de la 0014). Alternativas descartadas: generar `06_flujo.md`; registrar además una deuda para que el kit admita «flujo no aplica» en modo completo; dejar la spec en `En pruebas`.
30. **[2026-11-22 14:41] Cierre: `Cerrado`, con los commits en la misma corrida.** Previa: 36 de 36 pasos `[x]`, sin verificaciones manuales pendientes, sin flujo (decisión 29), sin pausa y validación del conjunto aprobada (decisión 28). Criterio 1: fila `cierre` y plan contra real escritos. Criterio 3: la persona confirma que los issues #4, #6, #7, #8, #10 y #11 quedan atendidos; los cierra ella en GitHub y el agente no consulta el gestor. Criterio 4: columna de pendiente vacía. Criterio 5: la persona decide pasar las deudas 0009 a 0014 a `Resuelta`, con Atendida por 0017 y «Cómo se atendió»; ninguna queda bloqueando. Criterio 2: se decide con la confirmación de los commits propuestos. Sin confirmación, esta corrida baja a `Implementado` y reescribe esta decisión. Ajeno, fuera de los commits: los cambios previos de `MEMORY.md`.

## Pruebas
Resultados en [07_casos_prueba.md](07_casos_prueba.md) › Corridas.

## Tiempos
| Paso | Inicio | Fin | Minutos | Pausa | Nota |
|---|---|---|---|---|---|
| spec | 2026-11-22 13:39 | 2026-11-22 13:45 | 6 | 0 | entrevista (4 preguntas) + `01_spec.md` + fila del índice; planner en el chat, claude-opus-5-5 |
| clarify | 2026-11-22 13:47 | 2026-11-22 13:54 | 7 | 0 | pasada 3 de 3; incluye la resolución de los 3 marcadores y la aprobación (18 + 6 + 0 hallazgos); reviewer y planner en el chat, claude-opus-5-5; modo automático |
| plan | 2026-11-22 13:54 | 2026-11-22 13:57 | 3 | 0 | `02_analisis_impacto.md` + `03_plan.md`; planner en el chat, claude-opus-5-5 |
| tasks | 2026-11-22 14:05 | 2026-11-22 14:07 | 2 | 0 | `04_tasks.md` (36 pasos) + `07_casos_prueba.md` (13 casos); planner en el chat, claude-opus-5-5 |
| F0.1 | 2026-11-22 14:07 | 2026-11-22 14:08 | 1 | 0 | medidas y copias de partida; sesión, claude-opus-5-5 |
| F0.2 | 2026-11-22 14:08 | 2026-11-22 14:12 | 3 | 0 | `/sdd-estructura` de partida sobre copia congelada, en paralelo con la ola 1; subagente general, claude-sonnet-5-5 |
| F1.1–F1.3 | 2026-11-22 14:09 | 2026-11-22 14:10 | 1 | 0 | ola 1, en paralelo; implementer, claude-sonnet-5-5 |
| F1.4, F1.5, F6.2 | 2026-11-22 14:09 | 2026-11-22 14:10 | 1 | 0 | ola 1, en paralelo; implementer, claude-sonnet-5-5 |
| F2.1–F2.4 | 2026-11-22 14:09 | 2026-11-22 14:10 | 1 | 0 | ola 1, en paralelo; implementer, claude-sonnet-5-5 |
| F3.1–F3.3 | 2026-11-22 14:09 | 2026-11-22 14:11 | 2 | 0 | ola 1, en paralelo, más la ampliación de la decisión 19; implementer, claude-sonnet-5-5 |
| F4.1, F4.2, F6.1 | 2026-11-22 14:09 | 2026-11-22 14:10 | 1 | 0 | ola 1, en paralelo; implementer, claude-sonnet-5-5 |
| F5.1, F5.3 | 2026-11-22 14:09 | 2026-11-22 14:10 | 1 | 0 | ola 1, en paralelo; implementer, claude-sonnet-5-5 |
| F7.1, F7.2 | 2026-11-22 14:09 | 2026-11-22 14:11 | 2 | 0 | ola 1, en paralelo, más ampliación de E22; implementer, claude-sonnet-5-5 |
| F7.3–F7.6 | 2026-11-22 14:09 | 2026-11-22 14:10 | 1 | 0 | ola 1, en paralelo; implementer, claude-sonnet-5-5 |
| F8.1 | 2026-11-22 14:10 | 2026-11-22 14:11 | 1 | 0 | ADR 0033–0035 e índice, en paralelo con la ola 1; sesión, claude-opus-5-5 |
| F4.3, F5.2 | 2026-11-22 14:11 | 2026-11-22 14:12 | 1 | 0 | ola 2; implementer, claude-sonnet-5-5 |
| F4.4, F6.3 | 2026-11-22 14:11 | 2026-11-22 14:12 | 1 | 0 | ola 2; implementer, claude-sonnet-5-5 |
| F5.4, F8.3 | 2026-11-22 14:12 | 2026-11-22 14:12 | 1 | 0 | ola 3; implementer, claude-sonnet-5-5 |
| F8.2 | 2026-11-22 14:12 | 2026-11-22 14:12 | 1 | 0 | réplica y `diff -rq`, más los ajustes de la decisión 20; sesión, claude-opus-5-5 |
| F8.4 | 2026-11-22 14:12 | 2026-11-22 14:13 | 1 | 0 | manifiesto 0.13.0; sesión, claude-opus-5-5 |
| F9.1 | 2026-11-22 14:13 | 2026-11-22 14:16 | 3 | 0 | casos medibles (sesión) y `/sdd-estructura` posterior (subagente general, claude-sonnet-5-5, desde 14:12); ruteo de `sdd-init`; sesión, claude-opus-5-5 |
| validate | 2026-11-22 14:24 | 2026-11-22 14:33 | 9 | 0 | E24 a AVISO; `tester` (claude-haiku-5-5); arranque medido y compactado; reviewer en el chat (claude-opus-5-5) tras detener un subagente lanzado por error; 1 vuelta de correcciones (implementer, claude-sonnet-5-5) |
| cierre | 2026-11-22 14:36 | 2026-11-22 14:41 | 5 | 0 | la sesión (claude-opus-5-5, effort high); sin flujo (decisión 29) |

Nota: las filas de las olas se solapan en el reloj. De 14:07 a 14:13 pasaron 6 minutos de reloj; la suma de filas los cuenta varias veces.

## Plan contra real

El real sale de la columna Minutos de Tiempos. Las filas que juntan pasos de dos fases cuentan en la fase del primer paso: F6 no tiene fila propia (F6.1 a F6.3 van en filas de F1 y F4), F5.2 cuenta en F4 y F8.3 en F5. Las olas se solapan: de 14:07 a 14:16 pasaron 9 minutos de reloj. Documentación SDD = `spec`, `clarify`, `plan`, `tasks`, `validate` y `cierre`.

| Fase | Estimado | Real | Delta |
|---|---|---|---|
| Documentación SDD | sin estimado | 32 min | — |
| F0 | 0.25 h | 4 min | −11 min |
| F1 | 1.5 h | 2 min | −88 min |
| F2 | 1 h | 1 min | −59 min |
| F3 | 1 h | 2 min | −58 min |
| F4 | 0.75 h | 3 min | −42 min |
| F5 | 1 h | 2 min | −58 min |
| F6 | 0.75 h | 0 min (en filas de F1 y F4) | −45 min |
| F7 | 1 h | 3 min | −57 min |
| F8 | 1 h | 3 min | −57 min |
| F9 | 1 h | 3 min | −57 min |
| **Total implementación** | **9.25 h** | **23 min** | **−532 min** |

Qué ajustar en el siguiente plan: en una spec de solo Markdown que ejecutan implementers en olas paralelas, estimar en minutos y por ola, no en horas por fase. Aquí el real fue un 4 % del estimado, lejos del tercio de `MEMORY.md`.
