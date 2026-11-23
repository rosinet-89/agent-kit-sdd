# 04 — Tareas: Plan contra real en vivo
Leyenda: `[ ]` Pendiente · `[~]` En curso · `[x]` Hecho · `[!]` Bloqueado

Plan: `03_plan.md` (ADR 0039). Estimado en minutos y por ola. F1 a F5 tocan archivos disjuntos y van en
una ola de tres lotes, sugeridos así: L1 = F1.1 a F1.3 (un archivo); L2 = F2.1, F3.1, F3.2 y F3.3;
L3 = F3.4, F4.1, F4.2 y F5.1. F6 y F7 van después, en serie. Lint de cada paso: `AGENTS.md` › Comandos
(UTF-8, sin BOM, rutas citadas existentes, `template/` sin marcadores `<<…>>` salvo los dos admitidos).

## F0 — Preparación (estimado: 3 min)
- [x] **F0.1 Releer las secciones citadas y fijar los nombres.** —
  - Hecho cuando:
    - [x] Leídas: `avance-y-tiempos.md` (Estructura, PUNTO DE RETOMO, Tiempos reales, Plan contra real),
      `plantillas.md` › `## 05_avance.md`, `sdd-tasks.md` punto 4, `sdd-implement.md` punto 8,
      `sdd-status.md` › Qué hace, `sdd-cierre.md` punto 6 y la leyenda (criterio 1 y «Un paso que
      cuenta»)
    - [x] Nombres fijados en una decisión de `05_avance.md`: sección «Pasos con tiempos», fila «Tiempo»,
      filas «Total implementación» y «Documentación SDD», marcas «pendiente», «en lote <lote>»,
      «retirado» y «sin estimado»

## F1 — Fuente: `avance-y-tiempos.md` (estimado: 10 min)
- [x] **F1.1 Sección «Pasos con tiempos».** RF-1, RF-2, RF-3, RF-4, RF-6, RF-7, RF-8, RF-9, RF-10, RF-14
  - Hecho cuando:
    - [x] Siembra al aprobar las tareas, con el estimado del plan (completo: `03_plan.md`; ligero:
      `04_tasks.md`, con un solo total como excepción a RF-7)
    - [x] Columnas Estimado, Real y Delta; filas de fase, de paso, «Total implementación» y
      «Documentación SDD»; real en la misma escritura que la fila de Tiempos (y la de la bitácora)
    - [x] Lote en la fila de su primer paso, «en lote» en el resto, lotes paralelos que suman cada uno;
      `pausa por confirmar` fuera hasta resolverse; ids que no existen a Documentación SDD con aviso
    - [x] Delta en la unidad del estimado, rango contra el extremo superior (S2), fase sin estimado
      (S6), «pendiente» sin filas; siembra tardía (RF-9) y nueva siembra tras un cambio (RF-10)
    - [x] Las filas de fase, Total y Documentación SDD no son pasos (RF-14, remite a la leyenda)
    - [x] Un ejemplo corto que cuadra: un paso, un lote, una fase en lote, una fila de documentación y
      una `pausa por confirmar`
- [x] **F1.2 Fila «Tiempo» y estructura.** RF-5
  - Hecho cuando:
    - [x] Fila «Tiempo» en la tabla del PUNTO DE RETOMO: estimado total, real, delta, real de
      documentación y filas en `pausa por confirmar`; aparece al sembrar
    - [x] Estructura: el punto 2 nombra la tabla Pasos con tiempos y el punto 6 queda para la línea de
      ajuste; «Tiempos reales» remite a «Pasos con tiempos» sin copiar la regla
- [x] **F1.3 «Plan contra real (al cierre)».** RF-12, RF-13
  - Hecho cuando:
    - [x] El cierre recalcula y completa la tabla viva desde Tiempos y escribe siempre una línea de
      ajuste o «sin ajuste» (S5); sin tabla viva, la tabla de hoy
    - [x] Dice que la tabla viva es la del criterio 1 (remite a la leyenda) y que manda Tiempos si no
      cuadra
    - [x] Lint del archivo

## F2 — Plantilla (estimado: 3 min)
- [x] **F2.1 Plantilla de `05_avance.md`.** RF-1, RF-5
  - Hecho cuando:
    - [x] `plantillas.md` › `## 05_avance.md`: fila «Tiempo» opcional en el PUNTO DE RETOMO y
      encabezado de la tabla Pasos con Estimado, Real y Delta, que remiten a «Pasos con tiempos»
    - [x] Lint del archivo

## F3 — Remisiones en comandos (estimado: 6 min)
- [x] **F3.1 `sdd-tasks.md`.** RF-1, RF-10
  - Hecho cuando:
    - [x] Punto 4: al anotar la aprobación, la sesión siembra la tabla en esa misma escritura, o la
      vuelve a sembrar al reaprobar tras un cambio (remisión)
- [x] **F3.2 `sdd-implement.md`.** RF-2, RF-3
  - Hecho cuando:
    - [x] Punto 8: el real del paso o lote en la tabla, con remisión a «Pasos con tiempos»; sigue sin
      escribir `05_avance.md`
- [x] **F3.3 `sdd-status.md`.** RF-11
  - Hecho cuando:
    - [x] Las horas salen de la tabla y de la fila «Tiempo» si existen, sin leer el plan; si no, como
      hoy. «Qué lee» no crece más allá de lo que ya lee de `05_avance.md`
- [x] **F3.4 `sdd-cierre.md`.** RF-12, RF-13
  - Hecho cuando:
    - [x] Punto 6: completa la tabla viva y escribe siempre la línea de ajuste; sale «si alguna fase se
      desvió fuerte»; remite al formato sin copiarlo
    - [x] Lint de los cuatro comandos

## F4 — Leyenda de estatus (estimado: 4 min)
- [x] **F4.1 Leyenda en `template/.docs/002_specs/README.md`.** RF-13, RF-14
  - Hecho cuando:
    - [x] Criterio 1: la tabla de plan contra real es la tabla Pasos con tiempos si existe; si no, la de
      la sección «Plan contra real»
    - [x] «Un paso que cuenta»: las filas de fase, Total y Documentación SDD no son pasos
- [x] **F4.2 Leyenda en `.docs/002_specs/README.md` (raíz).** RF-13, RF-14
  - Hecho cuando:
    - [x] Mismo texto: `diff` de las dos secciones «Leyenda de estatus» vacío
    - [x] Lint de los dos archivos

## F5 — Publicación (estimado: 3 min)
- [x] **F5.1 Changelog y versión 0.17.0.** —
  - Hecho cuando:
    - [x] `VersionAgentKitSdd` en `0.17.0`; entrada 0.17.0 en `ChangelogAgentKitSdd.md` con la tabla
      viva, la fila «Tiempo» y el ADR 0039

## F6 — Réplica y chequeos (estimado: 6 min)
- [x] **F6.1 Réplica local.** —
  - Hecho cuando:
    - [x] `cp` de lo tocado en `template/.agents/` a `.agents/`
    - [x] `diff -rq template/.agents .agents` solo con las diferencias admitidas en `MEMORY.md`
- [x] **F6.2 Ensayo de instalación.** criterios
  - Hecho cuando:
    - [x] Comando Test de `AGENTS.md` › Comandos en `mktemp -d`: los archivos tocados presentes; nada
      pisado; carpeta borrada al terminar
- [x] **F6.3 Chequeos.** RF-1 a RF-14
  - Hecho cuando:
    - [x] Lint en cada archivo tocado; CP-01 a CP-06 de `07_casos_prueba.md`
    - [x] `/sdd-estructura` (delegación fija con `auxiliar`) sin `FALLA`
- [x] **F6.4 Piloto en esta carpeta.** RF-9, RF-5, RF-2
  - Hecho cuando:
    - [x] CP-07 en el chat: la sesión siembra la tabla Pasos con tiempos de `0022` con la regla nueva y
      cuadra con su tabla Tiempos; anotado en `07_casos_prueba.md`

## F7 — Manifiesto (estimado: 5 min)
- [x] **F7.1 Manifiesto 0.17.0.** —
  - Hecho cuando:
    - [x] `manifiestos/0.17.0.md` con las huellas de `template/` tras el último cambio
- [x] **F7.2 E20.** —
  - Hecho cuando:
    - [x] E20 de `template/.agents/validar-estructura.md` en verde; si falla, se regeneran las filas
      tocadas, se anota la decisión y se vuelve a correr (`AGENTS.md` › Comandos › Chequeos de cierre)
