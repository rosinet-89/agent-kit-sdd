# 05 — Avance: Optimizar el arnés SDD (agentes y comandos)
Leyenda: `[ ]` Pendiente · `[~]` En curso · `[x]` Hecho · `[!]` Bloqueado

## PUNTO DE RETOMO — leer esto primero
| Dato | Contenido |
|---|---|
| Última sesión | 2026-10-25 14:23 |
| Fase actual | Cerrada |
| Último paso cerrado | `cierre` el 2026-10-25 14:23 |
| Siguiente paso | Ninguno. ADR 009 aceptado. Commit no pedido. |
| Bloqueos abiertos | Ninguno |
| Coordinación | Corlos edita `AGENTS.md`, `template/.agents/commands/` y otros archivos que este feature también tocaría (26 archivos modificados sin commit al abrir la carpeta). Antes de implementar, acordar con el usuario quién commitea 001 y 002 y partir del contenido actual de esos archivos, sin revertirlos. |
| Traspaso (Claudio → siguiente agente) | La spec está aprobada; `02` a `06` no existen. **Antes de `/sdd-plan`:** (1) acordar con el usuario quién commitea las carpetas `001`, `002` y `003`, las deudas, los ADR 007/008 y los cambios de `template/` (decisión 5; hay decenas de archivos sin commit). (2) La spec exige fijar la línea base de lectura en este `05_avance.md` ANTES de cambiar nada (definición de línea base y pasos de referencia: mínimo, mediana y máximo de `001_ActualizarKit/04_tasks.md`). (3) La deuda del modelo por rol y los permisos del frontmatter ya está dada de alta (`.docs/003_deuda/003_ModeloYPermisosDeAgentes.md`). (4) Partir del contenido actual de `AGENTS.md`, `template/.agents/commands/` y `validar-estructura.md` (otro agente los editó); no revertirlos. (5) Orden de cierre: cambios en `template/`, manifiesto de 0.5.0, E20, contraste 0.4.0 vs 0.5.0, chequeos de Comandos y medición. Decisiones 1 a 13 vigentes. |
| Estado BD | No aplica |
| Código | Ninguno |

## Pasos
- [x] F0.1 línea base en este avance
- [x] F1.1 confirmación de coordinación
- [x] F2.1 contrato de tiempos
- [x] F2.2 la skill `sdd` queda en tabla de asociación
- [x] F3.1 a F3.6 comandos de lectura y de tope
- [x] F4.1 a F4.8 cada comando es la fuente de su fase
- [x] F5.1 a F5.4 los cuatro agentes enlazan su comando
- [x] F5.5 tope de RNF-1: mediana 18 331
- [x] F4.9 tabla de fuente única
- [x] F6.1 a F6.4 secciones por rol, secretos y Alcance
- [x] F7.1 viñeta en el changelog 0.5.0
- [x] F8.1 a F8.8 congelar, manifiesto, medición y refresco hecho
- [x] flujo `06_flujo.md` aprobado
- [x] cierre

## Decisiones
1. **[2026-10-25 12:53] Se abre esta spec a partir de la revisión de los 4 agentes y 14 comandos del `template/`.** Origen: petición del usuario de reducir consumo de tokens sin bajar la calidad (lecturas innecesarias, pasos repetidos o asumidos, trabajo fuera de su rol). Alternativa descartada: aplicar los cambios directo sin spec (contradice el principio 2 de la constitución).
2. **[2026-10-25 12:58] Alcance: modelo por rol y permisos del frontmatter quedan fuera**; se darán de alta como deuda tras verificar cómo trata Claude Code `mode`/`permissions`. Alternativas descartadas: incluirlos (más riesgo de regresión y de choque con Corlos) y solo el permiso del implementer.
3. **[2026-10-25 12:58] Medición de calidad:** tabla de bytes antes/después sobre `001_ActualizarKit` (al menos 70 % menos de lectura por paso del implementer y por `/sdd-status`) más lista de verificaciones existentes con la pieza donde sigue cada una. Alternativas descartadas: re-ejecutar un feature completo (no reproducible) y sin objetivo numérico.
4. **[2026-10-25 12:58] Versión: este feature sale dentro de 0.5.0** (sin publicar); al cerrar se regenera el manifiesto de 0.5.0. Alternativa descartada: 0.5.1.
5. **[2026-10-25 12:58] Antes de implementar se acuerda con Corlos quién commitea 001 y 002**; se parte del contenido actual sin revertir trabajo ajeno. Alternativas descartadas: que lo haga Claudio o implementar sobre el árbol sucio.
6. **[2026-10-25 12:58] Tiempos: se prohíbe el lote con "0 minutos"**, salvo un paso marcado "sin trabajo real". Alternativas descartadas: sin excepción y permitir el lote.
7. **[2026-10-25 13:04] Clarify: 25 hallazgos del reviewer (11 ALTA); veredicto CAMBIOS NECESARIOS.** Corrección propia: la spec tiene 24 RF distintos, no 26 (conté líneas, no ids).
8. **[2026-10-25 13:04] Tiempos:** la prohibición del lote con "0 minutos" aplica solo a features abiertos desde esta versión (los existentes siguen válidos); mínimo 1 minuto por paso real; "sin trabajo real" se marca con esa frase en la columna Nota. Alternativas descartadas: solo advertir y prohibir también en los existentes.
9. **[2026-10-25 13:04] Herencia de `AGENTS.md`:** no se supone que la herramienta la inyecte; cada subagente lee solo las secciones que usa. Alternativas descartadas: suponer herencia y verificar después, y no tocar la lectura.
10. **[2026-10-25 13:04] Los cambios en `AGENTS.md` llegan a los proyectos por fusión manual** (grupo B); se actualizan `template/AGENTS.md` y el de la raíz sin divergir. Alternativa descartada: sacar esos cambios de `AGENTS.md`.
11. **[2026-10-25 13:04] `/sdd-estructura` lo ejecuta la sesión principal, sin subagente;** el reviewer conserva `validate`. Alternativa descartada: mantener el reviewer.
12. **[2026-10-25 13:07] Spec aprobada por el usuario** (`Estado: aprobada`; 24 RF, 6 RNF), incluidas las 6 decisiones del planner (documentos de menos de 4 KB se leen enteros; tope de `/sdd-init` y `/sdd-constitucion` de 30 archivos o 150 KB; modo "solo secretos" en `seguridad-basica`; sin meta del 70 % para esos dos comandos; 70 % en el paso de mediana del implementer; la lectura cuenta también el texto del agente y del comando).
13. **[2026-10-25 13:07] No se invoca `/sdd-plan` en esta sesión:** el usuario cambia de agente para la siguiente fase. Alternativa descartada: seguir con el plan ahora.
14. **[2026-10-25 13:09] Se da de alta la deuda `003_ModeloYPermisosDeAgentes`** (modelo por rol y permisos del frontmatter), con el visto bueno del usuario. Resuelve el pendiente de la decisión 2.
15. **[2026-10-25 13:28] Plan redactado.** Pasos de referencia: menor `F6.2` (mínima 834, línea base 92 280), mediana `F5.1` (15 181 / 89 004), mayor `F1.5` (21 248 / 89 004). `/sdd-status` 122 731 bytes. La deuda 003 ya estaba abierta; este plan no la crea. La fuente única por fase queda marcada para ADR al cierre, sin archivo. F1 sigue esperando la confirmación de coordinación.
16. **[2026-10-25 13:37] `04_tasks.md` entregado: 37 pasos, 4,0 h, RF-1 a RF-24 cubiertos, casillas abiertas.** Plan y tareas siguen sin el visto bueno. No se implementa.
17. **[2026-10-25 13:38] El usuario aprobó el plan y las tareas.** Siguiente paso: `/sdd-implement 003_OptimizarArnes F0.1`. F1 sigue sin la confirmación de coordinación.
18. **[2026-10-25 13:39] F0.1 cerrado.** La línea base de `02` §3 quedó copiada en Pruebas, sin recalcular. `template/` y los docs de la raíz no se tocaron en este paso.
19. **[2026-10-25 13:42] El usuario pausa F1.1.** Otro agente commitea los cambios que ya están. Esta sesión no commitea, no abre F2 y no edita `template/` ni los docs de ese commit. Al volver se cierra F1.1 con quién commiteó y qué quedó fuera. (a) y (b) siguen sin confirmar.
20. **[2026-10-25 13:46] F1.1 cerrado.** El usuario confirmó que el commit ya estaba hecho. (a) Lo coordinó él, mandando a otro agente a commitear; la edición en paralelo previa sigue siendo la de Corlos. (b) Esos commits quedaron a nombre de ChristianRS: `f5f083b`, `63f586d`, `e7c2187`, `bb4b185`, `fc98212`. Entran `001`, `002` y `003` hasta la pausa. (c) `git status` vacío: no quedan cambios ajenos sin commit. No se revirtió nada.
21. **[2026-10-25 13:46] F2.1 cerrado.** `template/.agents/skills/sdd/avance-y-tiempos.md` define el mínimo de 1 minuto, prohíbe el lote de filas en 0 y la excepción "sin trabajo real". Las reglas 1 a 4 siguen en sus secciones. `001_ActualizarKit` no se reescribió.
22. **[2026-10-25 13:49] F2.2 cerrado.** La tabla de `template/.agents/skills/sdd/SKILL.md` quedó en fase, enlace, rol y documento. `/sdd-estructura` lo ejecuta la sesión principal. El cierre remite a `/sdd-cierre` y sigue revisando deuda y ADR. Los tiempos no se copian: enlazan el contrato. La numeración citada sigue en 4 dígitos en un proyecto y 3 en el clon y en las carpetas de sección.
23. **[2026-10-25 13:50] F3 cerrado.** `sdd.md` no carga la skill ni el contrato para `status` ni `estructura`, y lista `sdd-estructura`. `sdd-status.md` lee secciones y cuenta identificadores. `sdd-implement.md` pasa la lectura mínima, el umbral de 4 096 bytes, los dos respaldos y el tope de 3, y no escribe `05_avance.md`. `sdd-estructura.md` lo corre la sesión principal y no redefine E20. `sdd-init.md` declara el tope de 30 archivos o 150 KB. `sdd-constitucion.md` usa ese tope y no otro.
24. **[2026-10-25 13:53] F4.1 a F4.8 cerrados. F4.9 no.** Cada comando conserva su procedimiento y declara qué lee, qué hace y qué no hace. `plantillas.md` no se tocó. El modo "solo secretos" todavía no está en la skill: `sdd-validate.md` solo la enlaza y esa comprobación queda para F6.3. F4.9 espera a que cierren F5.1–F5.4. Siguiente paso: `/sdd-implement 003_OptimizarArnes F5.1`.
25. **[2026-10-25 13:57] F5 cerrado.** Los cuatro agentes enlazan su comando y nombran las secciones de `AGENTS.md` que usan. `mode` y `permissions` no cambiaron. La vuelta de correcciones (máximo 2) salió del coordinator y quedó en `sdd-validate.md`, que es la fuente de esa fase. Mediana 1 560 + 1 590 + 15 181 = 18 331. Status 21 944. `F1.5` de `001` 24 398. Respaldo de `F6.2` 26 007. `F5.9` informado en 31 369 y no es paso de referencia. Ningún tope se pasó. RNF-1 y las cifras de F0 no se editaron. Siguiente paso: `/sdd-implement 003_OptimizarArnes F4.9`.
26. **[2026-10-25 14:01] F4.9 cerrado.** Cada fase de F3 y F4 tiene un solo comando como fuente. La skill, el coordinator y el agente del rol nombran y enlazan; no repiten la lista numerada del comando. E12 no se editó. Las demás piezas de `template/` que mandaban 3 dígitos para specs, deudas o ADR ahora remiten al índice de la copia. Siguiente paso: `/sdd-implement 003_OptimizarArnes F6.1`.
27. **[2026-10-25 14:03] F6 cerrado.** Los dos `AGENTS.md` comparten la sección "Qué lee cada rol". El clon sigue en 3 dígitos y conserva el modo mantenimiento. `template/AGENTS.md` sigue en 4 dígitos y con `<<NOMBRE_PROYECTO>>` e `<<IDIOMA>>`. El modo "solo secretos" y el modo completo viven en `seguridad-basica`. `sdd-validate.md` y `reviewer.md` solo enlazan esa skill. Alcance de `validar-estructura.md` dice que la sesión principal corre E1 a E20. La fila E20 no cambió. Siguiente paso: `/sdd-implement 003_OptimizarArnes F7.1`.
28. **[2026-10-25 14:04] F7.1 cerrado.** Una viñeta en `## 0.5.0 — 2026-10-24` de `CHANGELOG.md`. No hay entrada 0.5.1. `VERSION` sigue en 0.5.0. La fecha de la entrada no cambió. Siguiente paso: `/sdd-implement 003_OptimizarArnes F8.1`.
29. **[2026-10-25 14:06] F8 cerrado.** `template/` tiene 41 archivos; 28 modificados respecto de `HEAD`. Manifiesto `0.5.0` regenerado con 41 filas. E20 sin diferencia. Contraste contra 0.4.0: 6 altas, 0 bajas, 25 huellas distintas. Mediana 18 331, status 21 944, `F1.5` de `001` 24 398, respaldo de `F6.2` 26 828. El refresco de la copia local queda propuesto y no se ejecutó. `ACTUALIZAR.md`, ADR 007 y ADR 008 no se modificaron. Siguiente paso: `/sdd-flujo 003_OptimizarArnes`.
30. **[2026-10-25 14:08] El usuario confirmó el refresco.** Se copió `template/.agents/` sobre `.agents/` y se restauró `napkin.md`. `diff -rq` solo muestra `napkin.md`. No se tocó `AGENTS.md`, `MEMORY.md` ni `.docs/`.
31. **[2026-10-25] Borrador de `06_flujo.md`.** El planner lo redactó y el reviewer pidió 15 ajustes. El planner los aplicó. Falta la aprobación del usuario. La fila `flujo` se anota cuando esa aprobación exista.
32. **[2026-10-25 14:23] El usuario aprobó `06_flujo.md`.** Siguiente paso: `/sdd-cierre 003_OptimizarArnes`.
33. **[2026-10-25 14:23] Cierre.** Los pasos de este avance están en `[x]`. No hay verificación manual pendiente ni fila `pausa por confirmar`. `06_flujo.md` estaba aprobado. La spec pasa a `implementada`. La deuda 003 sigue abierta: este feature no la resolvió. Se propone un ADR de fuente única por fase; no se crea sin visto bueno. No se pidió commit.
34. **[2026-10-25 14:25] ADR 009 aceptado.** El usuario pidió crearlo. Archivo `.docs/004_adr/009_FuenteUnicaPorFase.md`, enlazado desde la decisión 11 del plan. La deuda 003 sigue abierta. No se pidió commit.

## Pruebas
- **F0.1 (2026-10-25 13:39).** Línea base copiada de `02_analisis_impacto.md` §3, sin recalcular. Origen: `wc -c` del 2026-10-25. El `AGENTS.md` de la raíz (8 667) no entra.

- **F1.1 (2026-10-25 13:46).** El usuario confirmó el commit. `git status` vacío. Specs `001`, `002` y `003` están en HEAD.

- **F2.1 (2026-10-25 13:46).** `template/.agents/skills/sdd/avance-y-tiempos.md` ganó el mínimo de 1 minuto, la prohibición del lote en 0 y la frase "sin trabajo real". `001_ActualizarKit` no se tocó. UTF-8, sin BOM. El diff solo agrega.

- **F2.2 (2026-10-25 13:49).** `SKILL.md`: tabla de 12 filas con enlace, rol y documento. Estructura = sesión principal. `name` = `sdd`. UTF-8, sin BOM. Diff 18/18, sin quitar la regla de 4 y 3 dígitos.

- **F3 (2026-10-25 13:50).** Seis comandos en `template/.agents/commands/`: utf-8, sin BOM, sin `name`. `sdd-init.md` conserva `<<NOMBRE_PROYECTO>>` y `<<IDIOMA>>`. `sdd-init.md` y `sdd-constitucion.md` no traen una meta del 70 %. E20 no se reescribe.

- **F4.1–F4.8 (2026-10-25 13:53).** Ocho comandos en `template/.agents/commands/`: utf-8, sin BOM, sin `name`, sin marcadores `<< >>`. `plantillas.md` sin diff. `sdd-clarify.md` muestra el párrafo de antes y el de después, sin `git diff`. `sdd-validate.md` y `sdd-flujo.md` nombran "ajeno o no atribuible" y enlazan `seguridad-basica` sin copiar los dos alcances. El modo de esa skill se comprueba en F6.3. `sdd-cierre.md` busca estados en la tabla de Pasos de `05_avance.md` y no pide `04_tasks.md`. F4.9 sigue abierto: espera F5.1–F5.4.

- **F5 (2026-10-25 13:57).** Agentes en `template/.agents/agents/`: utf-8, sin BOM, `name` igual al archivo. `mode` y `permissions` sin diff. Bytes: implementer 1 560, reviewer 2 591, planner 2 428, coordinator 4 859, `sdd-implement.md` 1 590, `sdd-status.md` 917. Secciones actuales de `template/AGENTS.md`: Stack 216 y Comandos 289 (505). `seguridad-basica` 3 276. `04_tasks.md` de `001` sigue en 29 604. La fase F6 congelada usada en la fórmula es 1 635 (líneas 320 a 341, hasta el separador). La fase F5 de ese archivo mide 6 997.
  - Mediana: 1 560 + 1 590 + 15 181 = **18 331** ≤ 26 701.
  - Status: 917 + 21 + 2 478 + 45 + 11 541 + 4 234 + 2 708 (32 líneas `**Fn.m`) = **21 944** ≤ 36 819.
  - `F1.5` de `001`: 1 560 + 1 590 + 21 248 = **24 398** ≤ 89 004.
  - Respaldo `F6.2` de `001`: 16 330 + 1 635 + 1 111 + 505 + 3 276 + 3 150 = **26 007** ≤ 92 280.
  - `F5.9` de `001` (no es referencia): 16 330 + 6 997 + 1 111 + 505 + 3 276 + 3 150 = **31 369**.
  - La línea base de F0 y las cifras de RNF-1 no se editaron.

- **F4.9 (2026-10-25 14:01).** Lectura cruzada de `template/.agents/`: la lista numerada de cada fase está solo en su comando. La tabla de la skill `sdd` tiene fase, enlace, rol y documento. El coordinator elige el comando y no reescribe los pasos. Cada agente enlaza el comando y declara qué lee y qué no hace. E12 de `validar-estructura.md` quedó igual. `avance-y-tiempos.md` sigue citando el feature ya cerrado `001_ActualizarKit`; no es una regla de ancho.

| Fase | Fuente | Solo nombran y enlazan |
|---|---|---|
| `spec` | `sdd-spec.md` | skill `sdd`, coordinator, planner |
| `clarify` | `sdd-clarify.md` | skill `sdd`, coordinator, reviewer |
| `plan` | `sdd-plan.md` | skill `sdd`, coordinator, planner |
| `tasks` | `sdd-tasks.md` | skill `sdd`, coordinator, planner |
| Implementación | `sdd-implement.md` | skill `sdd`, coordinator, implementer |
| `validate` | `sdd-validate.md` | skill `sdd`, coordinator, reviewer |
| `flujo` | `sdd-flujo.md` | skill `sdd`, coordinator, planner y reviewer |
| `cierre` | `sdd-cierre.md` | skill `sdd`, coordinator |
| `change` | `sdd-change.md` | skill `sdd`, coordinator, planner |
| Estado | `sdd-status.md` | skill `sdd` (no se carga), coordinator, `sdd.md` |
| Estructura | `sdd-estructura.md` | skill `sdd` (no se carga), coordinator, `sdd.md` |
| Entrada | `sdd.md` | coordinator |
| Inicio | `sdd-init.md` | `sdd.md`, marcador en `AGENTS.md` |
| Constitución | `sdd-constitucion.md` | `sdd.md`, `sdd-init.md` (el tope) |

- **F6 (2026-10-25 14:03).** Sección "Qué lee cada rol" idéntica en `template/AGENTS.md` y en `AGENTS.md` de la raíz. Diff de `validar-estructura.md` limitado a Alcance (dos líneas). E20 sin cambio. `seguridad-basica`: `name` sigue igual; utf-8, sin BOM. Modo "solo secretos" y modo completo definidos ahí. `sdd-validate.md` y `reviewer.md` enlazan la skill y no definen los dos modos.

- **F7.1 (2026-10-25 14:04).** `CHANGELOG.md` utf-8, sin BOM. Viñeta nueva al cierre de `## 0.5.0 — 2026-10-24`. No hay `## 0.5.1`. `VERSION` sigue `0.5.0`.

- **F8 (2026-10-25 14:06).** `find template -type f` = 41. `git status -- template` = 28 modificados, sin archivos nuevos. Prueba de humo de la huella: las tres cifras de `ACTUALIZAR.md` coinciden. `manifiestos/0.5.0.md`: 41 filas, huellas de 64 hex minúsculas, ninguna ruta con prefijo `template/`. E20: 0 faltantes, 0 sobrantes, 0 distintas. La fila E20 de `validar-estructura.md` no entra en el diff; la frase de Alcance ya estaba. Contraste 0.4.0 → 0.5.0: altas 6 (`.agents/commands/sdd-estructura.md`, `.agents/skills/adr/SKILL.md`, `.agents/skills/migracion-bd/SKILL.md`, `.agents/skills/seguridad-basica/SKILL.md`, `.agents/validar-estructura.md`, `.docs/004_adr/README.md`), bajas 0, huellas distintas 25. `ACTUALIZAR.md`, ADR 007 y ADR 008 sin cambios. Lint de los `.md` tocados: utf-8, sin BOM. Sin script de instalación nuevo. Ensayo en temporal: 41 archivos, bytes iguales al origen; la segunda copia con `--update=none` no cambió ninguna huella. El temporal se borró.

Topes: mediana 1 560 + 1 590 + 15 181 = **18 331** ≤ 26 701. Status 917 + 21 + 2 478 + 45 + 11 541 + 4 234 + 2 708 = **21 944** ≤ 36 819. `F1.5` de `001`: 1 560 + 1 590 + 21 248 = **24 398** ≤ 89 004. Respaldo de `F6.2` de `001`: 16 330 + 1 635 + 1 111 + 505 + 4 097 + 3 150 = **26 828** ≤ 92 280. Secciones Stack y Comandos siguen en 505. Constitución 1 111 y `MEMORY.md` 1 067, iguales al congelado. `04_tasks.md` de `001` sigue en 29 604.

| Pieza | Antes | Después |
|---|---:|---:|
| coordinator | 34 533 | 9 265 |
| planner | 35 063 | 6 903 |
| implementer | 89 004 | 18 331 |
| reviewer | 75 284 | 7 086 |
| `/sdd` | 21 498 | 6 209 |
| `/sdd-spec` | 21 686 | 18 187 |
| `/sdd-clarify` | 924 | 1 378 |
| `/sdd-plan` | 1 350 | 1 348 |
| `/sdd-tasks` | 903 | 995 |
| `/sdd-implement` | 64 536 | 16 771 |
| `/sdd-validate` | 1 064 | 2 305 |
| `/sdd-flujo` | 884 | 1 743 |
| `/sdd-cierre` | 1 614 | 2 186 |
| `/sdd-change` | 783 | 1 202 |
| `/sdd-status` | 122 731 | 21 944 |
| `/sdd-init` | 11 564 | 11 368 |
| `/sdd-constitucion` | 47 364 | 49 085 |
| `/sdd-estructura` | 12 194 | 12 288 |

El después es lo que el texto actual manda leer. Los documentos de `001` y `MEMORY.md` usan el tamaño congelado. Las piezas del kit, el tamaño actual. Suben `/sdd-clarify`, `/sdd-tasks`, `/sdd-validate`, `/sdd-flujo`, `/sdd-cierre` y `/sdd-change` porque el comando quedó como única fuente del procedimiento. Sube `/sdd-estructura` en 94 bytes por la frase de Alcance. Sube `/sdd-constitucion` a 49 085, dentro del tope de 150 KB y sin meta del 70 %: crecieron las skills y `AGENTS.md` que esa lectura sigue nombrando. `/sdd-init` no tiene meta del 70 % y baja a 11 368.

RF-24, pieza donde sigue cada verificación:

| Verificación | Pieza |
|---|---|
| Criterios de aceptación | `template/.agents/agents/reviewer.md` y `plantillas.md` |
| "Hecho cuando" | `plantillas.md`, `sdd-implement.md`, `implementer.md` |
| Chequeos de `AGENTS.md` › Comandos | `template/AGENTS.md`, `implementer.md`, `reviewer.md` |
| Revisión de seguridad con alcance de código | `seguridad-basica` › modo completo |
| Cierre con deuda y ADR | `sdd-cierre.md` |
| Contenido de `/sdd-status` | `sdd-status.md` |

`diff -rq template/.agents .agents` difiere en los cuatro agentes, los comandos `sdd*`, `napkin.md`, `adr`, `git-commit`, `sdd`, `avance-y-tiempos`, `seguridad-basica` y `validar-estructura.md`. El refresco de `INSTALAR.md` › Modo mantenimiento queda propuesto y no se ejecutó. Enlaces de Claude Code y Cursor: OK. Gemini y GitHub no están en esta copia; la tabla los pide solo si se usan.

- **Refresco (2026-10-25 14:08).** Confirmado por el usuario. Después del refresco, `diff -rq template/.agents .agents` solo muestra `napkin.md`.

### 3.1 Piezas

| Archivo | Bytes |
|---|---:|
| `template/.agents/agents/coordinator.md` | 6 679 |
| `template/.agents/agents/planner.md` | 3 510 |
| `template/.agents/agents/implementer.md` | 2 074 |
| `template/.agents/agents/reviewer.md` | 3 175 |
| `template/.agents/commands/sdd.md` | 1 361 |
| `template/.agents/commands/sdd-spec.md` | 1 630 |
| `template/.agents/commands/sdd-clarify.md` | 924 |
| `template/.agents/commands/sdd-plan.md` | 1 350 |
| `template/.agents/commands/sdd-tasks.md` | 903 |
| `template/.agents/commands/sdd-implement.md` | 1 142 |
| `template/.agents/commands/sdd-validate.md` | 1 064 |
| `template/.agents/commands/sdd-flujo.md` | 884 |
| `template/.agents/commands/sdd-cierre.md` | 1 614 |
| `template/.agents/commands/sdd-change.md` | 783 |
| `template/.agents/commands/sdd-status.md` | 658 |
| `template/.agents/commands/sdd-constitucion.md` | 1 254 |
| `template/.agents/commands/sdd-init.md` | 3 013 |
| `template/.agents/commands/sdd-estructura.md` | 900 |
| `template/AGENTS.md` | 7 798 |
| `template/.agents/skills/sdd/SKILL.md` | 8 551 |
| `template/.agents/skills/sdd/avance-y-tiempos.md` | 4 907 |
| `template/.agents/skills/sdd/plantillas.md` | 3 970 |
| `template/.agents/skills/seguridad-basica/SKILL.md` | 3 276 |
| `.docs/001_constitucion/constitution.md` | 1 111 |

Secciones de `template/AGENTS.md` que el implementer usa hoy: **505**.

### 3.2 Caso congelado `001_ActualizarKit`

| Archivo | Bytes |
|---|---:|
| `.docs/002_specs/001_ActualizarKit/01_spec.md` | 22 394 |
| `.docs/002_specs/001_ActualizarKit/02_analisis_impacto.md` | 13 954 |
| `.docs/002_specs/001_ActualizarKit/03_plan.md` | 16 330 |
| `.docs/002_specs/001_ActualizarKit/04_tasks.md` | 29 604 |
| `.docs/002_specs/001_ActualizarKit/05_avance.md` | 24 928 |
| `.docs/002_specs/001_ActualizarKit/06_flujo.md` | 13 796 |
| `MEMORY.md` | 1 067 |

Secciones de ese avance: PUNTO DE RETOMO 2 478, Pasos 45, Pruebas 11 541, Tiempos 4 234. Línea `Estado` 21.

### 3.3 Tres pasos de referencia de `001` y `F5.9`

| Paso de `001` | Mínima | Línea base |
|---|---:|---:|
| `F6.2` (menor) | 834 | 92 280 |
| `F5.1` (mediana, posición 16 de 32) | 15 181 | 89 004 |
| `F1.5` (mayor) | 21 248 | 89 004 |
| `F5.9` (no es paso de referencia; sin RF y sin sección de plan) | 1 452 | — |

### 3.4 Línea base del implementer

| Paso de `001` | Skill de dominio | Línea base |
|---|---|---:|
| `F6.2` | `seguridad-basica` (3 276) | 92 280 |
| `F5.1` | ninguna | 89 004 |
| `F1.5` | ninguna | 89 004 |

Tope de la mediana: 26 701. Quedan 11 520 para el texto final de `implementer.md` más `sdd-implement.md`.

### 3.5 `/sdd-status`

658 + 22 394 + 13 954 + 16 330 + 29 604 + 24 928 + 13 796 + 1 067 = **122 731**. Tope: 36 819.

### 3.6 Arranque

| Pieza | Bytes |
|---|---:|
| `coordinator` | 34 533 |
| `planner` | 35 063 |
| `reviewer` | 75 284 |
| `/sdd` | 21 498 |
| `/sdd-spec` | 21 686 |
| `/sdd-implement` (solo el comando) | 64 536 |
| `/sdd-estructura` | 12 194 |
| `/sdd-constitucion` | 47 364 |
| `/sdd-init` | 11 564 |
| `/sdd-plan` | 1 350 |
| `/sdd-tasks` | 903 |
| `/sdd-clarify` | 924 |
| `/sdd-validate` | 1 064 |
| `/sdd-flujo` | 884 |
| `/sdd-cierre` | 1 614 |
| `/sdd-change` | 783 |

## Tiempos
| Paso | Inicio | Fin | Minutos | Pausa | Nota |
|---|---|---|---|---|---|
| spec | 2026-10-25 12:53 | 2026-10-25 12:58 | 5 | 0 | planner (24 RF, 6 RNF), 5 dudas respondidas y ajustes |
| clarify | 2026-10-25 12:58 | 2026-10-25 13:07 | 9 | 0 | reviewer (25 hallazgos), 4 decisiones, planner ajusta la spec y aprobación |
| plan | 2026-10-25 13:09 | 2026-10-25 13:28 | 19 | 0 | planner midió con wc -c y escribió 02 y 03 |
| tasks | 2026-10-25 13:30 | 2026-10-25 13:38 | 8 | 0 | 04_tasks.md entregado; el usuario aprobó plan y tareas a las 13:38 |
| F0.1 | 2026-10-25 13:39 | 2026-10-25 13:39 | 1 | 0 | línea base copiada; el reloj no cambió de minuto; 1 por el mínimo de RF-22 |
| F1.1 | 2026-10-25 13:46 | 2026-10-25 13:46 | 1 | 0 | confirmación del commit; el hueco 13:42–13:46 fue el otro agente, no se suma |
| F2.1 | 2026-10-25 13:46 | 2026-10-25 13:46 | 1 | 0 | mismo minuto de reloj que F1.1; 1 por el mínimo de RF-22 |
| F2.2 | 2026-10-25 13:48 | 2026-10-25 13:49 | 1 | 0 | tabla de la skill sin secuencia de fase |
| F3.1 | 2026-10-25 13:50 | 2026-10-25 13:50 | 1 | 0 | mismo minuto de F3; 1 por el mínimo de RF-22 |
| F3.2 | 2026-10-25 13:50 | 2026-10-25 13:50 | 1 | 0 | mismo minuto de F3; 1 por el mínimo de RF-22 |
| F3.3 | 2026-10-25 13:50 | 2026-10-25 13:50 | 1 | 0 | mismo minuto de F3; 1 por el mínimo de RF-22 |
| F3.4 | 2026-10-25 13:50 | 2026-10-25 13:50 | 1 | 0 | mismo minuto de F3; 1 por el mínimo de RF-22 |
| F3.5 | 2026-10-25 13:50 | 2026-10-25 13:50 | 1 | 0 | mismo minuto de F3; 1 por el mínimo de RF-22 |
| F3.6 | 2026-10-25 13:50 | 2026-10-25 13:50 | 1 | 0 | mismo minuto de F3; 1 por el mínimo de RF-22 |
| F4.1 | 2026-10-25 13:52 | 2026-10-25 13:53 | 1 | 0 | mismo tramo de F4; 1 por el mínimo de RF-22 |
| F4.2 | 2026-10-25 13:52 | 2026-10-25 13:53 | 1 | 0 | mismo tramo de F4; 1 por el mínimo de RF-22 |
| F4.3 | 2026-10-25 13:52 | 2026-10-25 13:53 | 1 | 0 | mismo tramo de F4; 1 por el mínimo de RF-22 |
| F4.4 | 2026-10-25 13:52 | 2026-10-25 13:53 | 1 | 0 | mismo tramo de F4; 1 por el mínimo de RF-22 |
| F4.5 | 2026-10-25 13:52 | 2026-10-25 13:53 | 1 | 0 | mismo tramo de F4; 1 por el mínimo de RF-22 |
| F4.6 | 2026-10-25 13:52 | 2026-10-25 13:53 | 1 | 0 | mismo tramo de F4; 1 por el mínimo de RF-22 |
| F4.7 | 2026-10-25 13:52 | 2026-10-25 13:53 | 1 | 0 | mismo tramo de F4; 1 por el mínimo de RF-22 |
| F4.8 | 2026-10-25 13:52 | 2026-10-25 13:53 | 1 | 0 | mismo tramo de F4; 1 por el mínimo de RF-22 |
| F5.1 | 2026-10-25 13:55 | 2026-10-25 13:57 | 1 | 0 | mismo tramo de F5; 1 por el mínimo de RF-22 |
| F5.2 | 2026-10-25 13:55 | 2026-10-25 13:57 | 1 | 0 | mismo tramo de F5; 1 por el mínimo de RF-22 |
| F5.3 | 2026-10-25 13:55 | 2026-10-25 13:57 | 1 | 0 | mismo tramo de F5; 1 por el mínimo de RF-22 |
| F5.4 | 2026-10-25 13:55 | 2026-10-25 13:57 | 1 | 0 | mismo tramo de F5; 1 por el mínimo de RF-22 |
| F5.5 | 2026-10-25 13:55 | 2026-10-25 13:57 | 1 | 0 | medición; mismo tramo; 1 por el mínimo de RF-22 |
| F4.9 | 2026-10-25 14:00 | 2026-10-25 14:01 | 1 | 0 | tabla de fuente única y lectura cruzada |
| F6.1 | 2026-10-25 14:02 | 2026-10-25 14:03 | 1 | 0 | mismo tramo de F6; 1 por el mínimo de RF-22 |
| F6.2 | 2026-10-25 14:02 | 2026-10-25 14:03 | 1 | 0 | mismo tramo de F6; 1 por el mínimo de RF-22 |
| F6.3 | 2026-10-25 14:02 | 2026-10-25 14:03 | 1 | 0 | mismo tramo de F6; 1 por el mínimo de RF-22 |
| F6.4 | 2026-10-25 14:02 | 2026-10-25 14:03 | 1 | 0 | mismo tramo de F6; 1 por el mínimo de RF-22 |
| F7.1 | 2026-10-25 14:04 | 2026-10-25 14:04 | 1 | 0 | mismo minuto; 1 por el mínimo de RF-22 |
| F8.1 | 2026-10-25 14:04 | 2026-10-25 14:06 | 1 | 0 | mismo tramo de F8; 1 por el mínimo de RF-22 |
| F8.2 | 2026-10-25 14:04 | 2026-10-25 14:06 | 1 | 0 | mismo tramo de F8; 1 por el mínimo de RF-22 |
| F8.3 | 2026-10-25 14:04 | 2026-10-25 14:06 | 1 | 0 | mismo tramo de F8; 1 por el mínimo de RF-22 |
| F8.4 | 2026-10-25 14:04 | 2026-10-25 14:06 | 1 | 0 | mismo tramo de F8; 1 por el mínimo de RF-22 |
| F8.5 | 2026-10-25 14:04 | 2026-10-25 14:06 | 1 | 0 | mismo tramo de F8; 1 por el mínimo de RF-22 |
| F8.6 | 2026-10-25 14:04 | 2026-10-25 14:06 | 1 | 0 | mismo tramo de F8; 1 por el mínimo de RF-22 |
| F8.7 | 2026-10-25 14:04 | 2026-10-25 14:06 | 1 | 0 | mismo tramo de F8; 1 por el mínimo de RF-22 |
| F8.8 | 2026-10-25 14:04 | 2026-10-25 14:06 | 1 | 0 | mismo tramo de F8; 1 por el mínimo de RF-22 |
| flujo | 2026-10-25 14:08 | 2026-10-25 14:23 | 15 | 0 | planner redacta, reviewer pide 15 ajustes, planner corrige, usuario aprueba |
| cierre | 2026-10-25 14:23 | 2026-10-25 14:23 | 1 | 0 | mismo minuto que la aprobación del flujo; 1 por el mínimo de RF-22 |

## Plan contra real

El real sale de la columna Minutos. 60 minutos = 1 h.

| Fase | Estimado | Real | Delta |
|---|---|---|---|
| Documentación SDD | sin estimado | 0.95 h | — |
| F0 | 0.25 h | 0.02 h | -0.23 h |
| F1 | 0.25 h | 0.02 h | -0.23 h |
| F2 | 0.50 h | 0.03 h | -0.47 h |
| F3 | 0.50 h | 0.10 h | -0.40 h |
| F4 | 0.50 h | 0.15 h | -0.35 h |
| F5 | 0.50 h | 0.08 h | -0.42 h |
| F6 | 0.50 h | 0.07 h | -0.43 h |
| F7 | 0.25 h | 0.02 h | -0.23 h |
| F8 | 0.75 h | 0.13 h | -0.62 h |

Documentación SDD = spec 5 + clarify 9 + plan 19 + tasks 8 + flujo 15 + cierre 1 = 57 minutos.

En un feature que solo edita Markdown del kit, el siguiente plan estima cada fase en minutos, no en bloques de 0,25 h. El mínimo de 1 minuto por paso hace que la suma de la tabla supere el reloj cuando varios pasos comparten el mismo minuto.
