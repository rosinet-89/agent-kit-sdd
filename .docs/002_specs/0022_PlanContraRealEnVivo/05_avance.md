# 05 — Avance: Plan contra real en vivo
Leyenda: `[ ]` Pendiente · `[~]` En curso · `[x]` Hecho · `[!]` Bloqueado

## PUNTO DE RETOMO — leer esto primero
| Dato | Contenido |
|---|---|
| Modo | `completo` sin flujo (decisión 21). Condiciones: 1 no (toca más de 3 archivos: contrato de avance, plantillas, tasks, implement, coordinator, status, cierre); 2 sí (ninguna otra spec en curso); 3 no (cambia el contrato de avance y el criterio 1 de la leyenda, S1); 4 no (unificar la tabla del cierre con Pasos es decisión con alternativa); 5 sí (sin `sql/`); 6 no (cambia qué hacen tasks, implement, status y cierre); 7 no (más de 1 h estimada); 8 no (toca `commands`, `agents` y `skills`) |
| Última sesión | 2026-11-22 21:17 |
| Fase actual | `cierre` (Cerrado; commits locales con la confirmación de la persona) |
| Último paso cerrado | cierre: previa OK (sin flujo, decisión 21), E20 56 de 56, criterios 1, 3, 4 y 5 en verde (decisión 23); validate vuelta 2 aprobada (2026-11-22 21:13), sin verificaciones manuales |
| Siguiente paso | Ninguno: spec cerrada |
| Tiempo | Implementación: estimado 40 min · real 23 min · delta -17 min (fases con real) · Documentación SDD: 15 min · pausa por confirmar: 0 |
| Bloqueos abiertos | Ninguno |
| Estado BD | No aplica |
| Código | Spec: `01` a `05`, `07`; ADR 0039; `template/`: `avance-y-tiempos.md`, `plantillas.md`, `sdd-tasks`, `sdd-implement`, `sdd-status`, `sdd-cierre`, leyenda; raíz: leyenda, versión, changelog, `manifiestos/0.17.0.md`; réplica `.agents/` (local) |

## Pasos
| Paso | Estado | Estimado | Real | Delta | Nota |
|---|---|---|---|---|---|
| **F0 — Preparación** | [x] | 3 min | 1 min | -2 min |  |
| F0.1 | [x] | | 1 min | | nombres fijados (decisión 14); en el chat |
| **F1 — Fuente** | [x] | 10 min | 4 min | -6 min | 3 del lote L1 + 1 de la corrección de validate |
| F1.1 | [x] | | 3 min + 1 min | | lote L1, ola 1; corrección de validate v1 (decisión 20) |
| F1.2 | [x] | | en lote L1 | | |
| F1.3 | [x] | | en lote L1 | | |
| **F2 — Plantilla** | [x] | 3 min | 3 min | 0 min |  |
| F2.1 | [x] | | 3 min | | lote L4, ola 1 |
| **F3 — Remisiones** | [x] | 6 min | 3 min | -3 min |  |
| F3.1 | [x] | | 3 min | | lote L2, ola 1 |
| F3.2 | [x] | | en lote L2 | | |
| F3.3 | [x] | | en lote L2 | | |
| F3.4 | [x] | | en lote L2 | | |
| **F4 — Leyenda** | [x] | 4 min | 3 min | -1 min |  |
| F4.1 | [x] | | 3 min | | lote L3, ola 1 |
| F4.2 | [x] | | en lote L3 | | |
| **F5 — Publicación** | [x] | 3 min | 3 min | 0 min |  |
| F5.1 | [x] | | 3 min | | lote L5, ola 1 |
| **F6 — Réplica y chequeos** | [x] | 6 min | 4 min | -2 min |  |
| F6.1 | [x] | | 1 min | | lote L6 |
| F6.2 | [x] | | en lote L6 | | |
| F6.3 | [x] | | en lote L6 + 2 min | | lint y CP-01 a CP-06 ok; `/sdd-estructura` sin FALLA |
| F6.4 | [x] | | 1 min | | piloto CP-07: siembra tardía (RF-9) |
| **F7 — Manifiesto** | [x] | 5 min | 2 min | -3 min |  |
| F7.1 | [x] | | 2 min | | lote L7 |
| F7.2 | [x] | | en lote L7 | | E20 en verde |
| **Total implementación** | | 40 min | 23 min | -17 min | delta contra F0 a F7 (40 min) |
| **Documentación SDD** | | sin estimado | 15 min | — | spec 8, clarify 1, plan 1, tasks 1, validate 2 + 1, cierre 1 |
## Decisiones
1. **[2026-11-22 19:24] Se registran 0021 y 0022 y se empieza por 0021.** La persona: «registra las 2,
   luego iniciamos con la 21». 0022 conserva su número tentativo; la idea viene de la decisión 19 de
   `0020_RuteoDeModelos` y de la nota local `.agents/local/idea-0022-plan-contra-real-en-vivo.md`,
   cuyo contenido pasa a `01_spec.md` para que quede versionado.
2. **[2026-11-22 20:44] Se retoma la spec con el argumento `22`.** Coincide con la carpeta ya registrada
   `0022_PlanContraRealEnVivo`; no se abre carpeta nueva ni se renumera. La bitácora de uso ya tenía
   decisión por spec «sí» (20:41), así que no se repregunta. El planner corre en el chat (mismo modelo
   que la sesión, Opus).
3. **[2026-11-22 20:46] Sin aviso de desvío en el momento.** La persona eligió «Sin aviso» entre real
   > 2×, real > estimado, ambos sentidos o sin aviso. El desvío solo se ve en la tabla. Alternativa
   descartada: aviso con real > 2× estimado.
4. **[2026-11-22 20:46] El ajuste de cómo se estima queda fuera de alcance.** Sigue en `MEMORY.md` y, si
   se quiere regla, en deuda o spec aparte. Alternativa descartada: pedir estimar por lote u ola en esta
   spec.
5. **[2026-11-22 20:50] Todos los puntos del plan en la tabla Pasos, con tiempos.** La primera pregunta
   (dónde verlo) se aceptó sin querer; se repreguntó y la persona dijo: «en el mismo archivo de Avance,
   practicamente es poner todos los puntos del plan/task e ir llenando con los tiempos que van
   saliendo». Luego eligió «Pasos con tiempos»: la tabla Pasos suma Estimado, Real y Delta, con fila
   por fase (estimado y subtotal), por paso (real o «en lote»), Total y Documentación SDD, más la fila
   «Tiempo» en el retomo. Alternativas descartadas: estimar también cada paso y tabla aparte por fase.

6. **[2026-11-22 20:53] Autochequeo del planner: 3 correcciones.** RF-4 solo actualiza Documentación SDD
   si la tabla existe; RF-7 separa el estimado del Total (todas las fases) de su delta (fases con real);
   el caso «solo análisis» queda sin tabla ni fila «Tiempo». No cuenta como pasada.
7. **[2026-11-22 20:53] Pasada 1 (10 hallazgos), corrección automática del planner.** RF-2 incluye pasos
   de prueba y fases `Fn`; RF-3 fija el real del lote en la fila de su primer paso y «en lote» para la
   fase cubierta por otra; RF-4 absorbe los ids inexistentes; RF-10 dice que lo retirado sigue sumando;
   RF-12 escribe siempre una línea de ajuste (S5); nuevos RF-13 (la tabla viva es la del criterio 1) y
   RF-14 (filas de fase, Total y Documentación no cuentan como pasos); el RNF admite releer el plan tras
   un cambio; casos límite de prueba y de ligero con un solo total.
8. **[2026-11-22 20:54] Pasada 2 por delta (3 hallazgos), corrección automática.** RF-7 incluye en el
   delta del Total las fases con real «en lote»; la cita del paso de prueba remite a la leyenda; en
   ligero el delta del Total compara contra el total (excepción a RF-7).
9. **[2026-11-22 20:54] Pasada 3 por delta (1 hallazgo) al tope: pasa a supuesto S6.** Una fase «sin
   estimado» con real suma al Total y no aporta estimado. No es de alcance ni de seguridad: sin
   marcador `[ACLARAR]`. La persona decide si aprueba así.
10. **[2026-11-22 20:57] Spec aprobada por la persona con los supuestos S1 a S6.** La persona: «apruebo la spec,
    procede con el plan». `01_spec.md` pasa a `Estado: aprobada`.
11. **[2026-11-22 20:58] S1 se marca como candidata a ADR 0039.** Alternativa real (dos tablas) y efecto duradero en
    el contrato de avance y la leyenda. No se crea sin visto bueno de la persona.
12. **[2026-11-22 20:59] ADR 0039 creado y aceptado.** La persona: «crea el ADR 0039 y sigue con tasks». Enlazado
    desde `03_plan.md` › 6; fila en `.docs/004_adr/README.md`.
13. **[2026-11-22 21:02] Plan y tareas aprobados.** La persona: «apruebo plan y tareas, procede con implement».
    Siguiente: `/sdd-implement` en cascada (ola 1: L1, L2 y L3 con implementers Sonnet).

14. **[2026-11-22 21:03] F0.1: nombres y formas fijados; F0.1 en el chat.** Sección «Pasos con tiempos» (`##` en
    `avance-y-tiempos.md`); tabla Pasos `| Paso | Estado | Estimado | Real | Delta | Nota |`; fila de fase
    con `**Fn — nombre**` y su estimado; fila «Total implementación» y fila «Documentación SDD»; marcas
    «pendiente», «en lote <lote>», «retirado», «sin estimado» y delta «—»; fila «Tiempo» del PUNTO DE
    RETOMO. Desvío: F0.1 lo hizo el chat (Opus) y no un implementer Sonnet, porque no edita el producto
    y su entrega es esta decisión, que solo escribe la sesión.
15. **[2026-11-22 21:03] Lotes de la ola 1 según `sdd-implement.md` › 2.** Un lote solo junta pasos de una fase
    o del mismo archivo, así que el L2 y el L3 sugeridos en `04_tasks.md` se parten: L1 = F1.1 a F1.3;
    L2 = F3.1 a F3.4; L3 = F4.1 y F4.2; L4 = F2.1; L5 = F5.1. Cinco implementers Sonnet en paralelo
    (archivos disjuntos). La sesión marca `04_tasks.md` para que no compitan por el mismo archivo.

16. **[2026-11-22 21:06] Ola 1 revisada y aceptada.** La sesión revisó el diff de los cinco lotes contra su «Hecho
    cuando»: la regla vive solo en `avance-y-tiempos.md` › «Pasos con tiempos»; plantilla, cuatro
    comandos y leyenda remiten; la leyenda de la raíz y la de `template/` siguen idénticas; versión
    0.17.0 y changelog. El ejemplo de la fuente cuadra (F1 25 min, Total -25 contra 50). Duda de L1
    resuelta: una fase cuya única fila está en `pausa por confirmar` queda con real vacío y delta
    «pendiente», por analogía con RF-7. Los cinco subagentes figuran como `implementer`,
    `claude-sonnet-5-5` en sus `meta.json`.

17. **[2026-11-22 21:06] F6.4: tabla Pasos con tiempos sembrada en esta carpeta con la regla nueva (siembra tardía,
    RF-9).** Desde ahora la sesión actualiza el real de cada fila en la misma escritura. La hizo la
    sesión y no un implementer: solo la sesión escribe `05_avance.md`.

18. **[2026-11-22 21:09] Cascada terminada: 18 de 18 pasos.** `/sdd-estructura` sin FALLA; sus 6 avisos (E1, E3, E8,
    E11, E19 y la nota de E22) son de piezas locales ya admitidas (`claudio`, `caveman`,
    `reporte-sesiones`, enlaces de herramientas no declaradas) o del detalle de la celda, que se
    actualiza ahora. Sin commit: espera `validate` y la confirmación de la persona.
19. **[2026-11-22 21:12] Validate, vuelta 1: CAMBIOS NECESARIOS.** Reviewer en el chat (claude-opus-5-5);
    tester subagente (claude-haiku-5-5) corrió CP-01 a CP-06. Falla: `avance-y-tiempos.md` › «Pasos con
    tiempos» › «Actualización del real» no dice que, en la misma escritura, se rehacen el delta de la
    fase, el «Total implementación» y la fila «Tiempo» (RF-2, RF-5, RF-6: hoy la fila «Tiempo» solo
    consta al sembrar y `/sdd-status` la leería vieja). Corrección: un renglón en esa subsección; luego
    réplica, fila del manifiesto y E20. Resto en verde: lint, réplica, E20 (56 de 56), secretos (modo
    «solo secretos») y CP-07 cuadra (22 min, 11 de documentación antes de esta fila). El tester dio `0`
    en la segunda parte de CP-05; la re-corrida del reviewer da `2` (líneas 42 y 48): se toma la re-corrida.
20. **[2026-11-22 21:13] Validate, vuelta 2: APROBADO.** Implementer subagente (claude-sonnet-5-5)
    rehízo el primer renglón de «Actualización del real»: en la misma escritura se anota el real y se
    rehacen subtotal y delta de la fase, «Total implementación» y fila «Tiempo». Réplica idéntica,
    huella nueva de `.agents/skills/sdd/avance-y-tiempos.md` en `manifiestos/0.17.0.md`; E20 56 de 56;
    lint en verde. Reviewer en el chat (claude-opus-5-5) lo confirmó. Validación del conjunto aprobada;
    sin verificaciones manuales (solo Markdown). Opcionales sin bloqueo: el «≈ 24» de `03_plan.md` § 7
    frente al Total 40 de RF-7, y F6.3 en lote L6 con fila propia.

21. **[2026-11-22 21:16] Sin flujo** (persona: «no se requiere el flujo en la spec 22 pasa al cierre»). Sin
    `06_flujo.md`: el cambio es Markdown del kit, sin usuario final de una app; el modo queda «completo sin
    flujo», como en `0021_BitacoraDeUso` (decisión 24).
22. **[2026-11-22 21:17] Tokens completados en el cierre** (punto 1 de `sdd-cierre.md`). Faltaban en la bitácora las
    entradas de validate v1, F1.1 (corrección) y validate v2: se agregan ahora con sus filas de Tiempos.
    Hallazgos: los cinco lotes de la ola 1 comparten la ventana 21:03–21:06, así que cada entrada repite
    los mismos tokens (los de toda la ola); F6.3 (`/sdd-estructura`) y el lote L7 (21:07–21:09) quedan «no
    disponible (sesiones paralelas)»: hay mensajes de dos sesiones en esa ventana.
23. **[2026-11-22 21:17] Cierre hacia `Cerrado`.** Previa OK: 18 de 18 pasos hechos, sin verificaciones manuales, sin
    flujo (decisión 21), sin `pausa por confirmar`, validación del conjunto aprobada (decisión 20) y chequeo
    de cierre E20 en verde (56 archivos, 56 filas de `manifiestos/0.17.0.md`). Criterio 1: tabla Pasos con
    tiempos (es la de plan contra real, ADR 0039) y fila `cierre`. Criterio 3: sin ticket ni aviso.
    Criterio 4: columna de pendiente vacía. Criterio 5: ninguna deuda con Atendida por este requerimiento.
    El criterio 2 se decide con los commits.
24. **[2026-11-22 21:19] Deuda, memoria y commits** (persona). Deuda: ninguna (sin pasos diferidos). Memoria:
    línea en `MEMORY.md` › Trampas sobre tokens de lotes en paralelo (decisión 22). Commits confirmados: kit
    0.17.0, ADR 0039 y spec 0022 con el índice y la memoria; criterio 2 en verde.

## Pruebas
Resultados y corridas en [07_casos_prueba.md](07_casos_prueba.md): CP-01 a CP-07 obtenidos como se esperaba (2026-11-22 21:07).
## Tiempos
| Paso | Inicio | Fin | Minutos | Pausa | Nota |
|---|---|---|---|---|---|
| spec | 2026-11-22 20:44 | 2026-11-22 20:52 | 8 | 0 | retomo de carpeta registrada; entrevista de 4 preguntas (una repreguntada) + `01_spec.md`; planner en el chat, claude-opus-5-5 |
| clarify | 2026-11-22 20:53 | 2026-11-22 20:54 | 1 | 0 | pasada 3 de 3; autochequeo + 3 pasadas por delta en modo automático; reviewer y planner en el chat, claude-opus-5-5 |
| plan | 2026-11-22 20:57 | 2026-11-22 20:58 | 1 | 0 | `02` y `03`; planner en el chat, claude-opus-5-5 |
| tasks | 2026-11-22 20:59 | 2026-11-22 21:00 | 1 | 0 | ADR 0039 + `04_tasks` + `07_casos_prueba`; planner en el chat, claude-opus-5-5 |
| F0.1 | 2026-11-22 21:02 | 2026-11-22 21:03 | 1 | 0 | nombres fijados; chat, claude-opus-5-5 (desvío, decisión 14) |
| F1.1, F1.2, F1.3 | 2026-11-22 21:03 | 2026-11-22 21:06 | 3 | 0 | lote L1, ola 1; implementer subagente (otro modelo), claude-sonnet-5-5 |
| F3.1, F3.2, F3.3, F3.4 | 2026-11-22 21:03 | 2026-11-22 21:06 | 3 | 0 | lote L2, ola 1; implementer subagente (otro modelo), claude-sonnet-5-5 |
| F4.1, F4.2 | 2026-11-22 21:03 | 2026-11-22 21:06 | 3 | 0 | lote L3, ola 1; implementer subagente (otro modelo), claude-sonnet-5-5 |
| F2.1 | 2026-11-22 21:03 | 2026-11-22 21:06 | 3 | 0 | lote L4, ola 1; implementer subagente (otro modelo), claude-sonnet-5-5 |
| F5.1 | 2026-11-22 21:03 | 2026-11-22 21:06 | 3 | 0 | lote L5, ola 1; implementer subagente (otro modelo), claude-sonnet-5-5 |
| F6.4 | 2026-11-22 21:06 | 2026-11-22 21:06 | 1 | 0 | piloto CP-07: siembra y cuadre; en el chat, claude-opus-5-5 |
| F6.1, F6.2, F6.3 | 2026-11-22 21:06 | 2026-11-22 21:07 | 1 | 0 | lote L6 (sin `/sdd-estructura`); implementer subagente (otro modelo), claude-sonnet-5-5 |
| F6.3 | 2026-11-22 21:07 | 2026-11-22 21:09 | 2 | 0 | `/sdd-estructura`: sin FALLA, 6 avisos locales ya admitidos; auxiliar subagente (ruteo), claude-sonnet-5-5 |
| F7.1, F7.2 | 2026-11-22 21:07 | 2026-11-22 21:09 | 2 | 0 | lote L7: manifiesto 0.17.0 (56 filas, 7 huellas nuevas) y E20 en verde; implementer subagente (otro modelo), claude-sonnet-5-5 |
| validate | 2026-11-22 21:10 | 2026-11-22 21:12 | 2 | 0 | vuelta 1 de 2: CAMBIOS NECESARIOS (decisión 19); reviewer en el chat, claude-opus-5-5; tester subagente (otro modelo), claude-haiku-5-5 |
| F1.1 | 2026-11-22 21:12 | 2026-11-22 21:13 | 1 | 0 | corrección de validate v1 (decisión 20); implementer subagente (otro modelo), claude-sonnet-5-5 |
| validate | 2026-11-22 21:13 | 2026-11-22 21:13 | 1 | 0 | vuelta 2 de 2: APROBADO (decisión 20); reviewer en el chat, claude-opus-5-5 |
| cierre | 2026-11-22 21:16 | 2026-11-22 21:17 | 1 | 0 | sesión principal (claude-opus-5-5); previa OK sin flujo; E20 OK (56 de 56); tokens completados (decisión 22) |
## Plan contra real
La tabla de plan contra real es la tabla Pasos con tiempos (ADR 0039); no se escribe otra.

Ajuste para el siguiente plan: el real fue un 58 % de la suma de estimados (23 de 40 min). La ola 1 (5
lotes en paralelo) tardó 3 min de reloj; estimar unos 3 min por lote y 2 min por corrección de validate.
