# 05 — Avance: Guardrails, la base de datos solo se lee
Leyenda: `[ ]` Pendiente · `[~]` En curso · `[x]` Hecho · `[!]` Bloqueado

## PUNTO DE RETOMO — leer esto primero
| Dato | Contenido |
|---|---|
| Última sesión | 2026-11-21 10:04 |
| Fase actual | `cierre`: `Cerrado` (2026-11-21 10:04) |
| Último paso cerrado | cierre de 0008 (spec `implementada`, plan contra real, índice), 2026-11-03 21:33 |
| Siguiente paso | Ninguno |
| Bloqueos abiertos | Ninguno |
| Validación del conjunto | Aprobada: validate del reviewer APROBADO de F0 a F6; RF-16 a RF-18 cumplidos con F7 (Pruebas). Registrada en el punto de retomo el 2026-11-21 con el visto bueno de la persona |
| Aviso o ticket | Issue #1 cerrado en GitHub; lo confirmó la persona el 2026-11-21 |
| Estado BD | No aplica |
| Código | Solo Markdown: `template/` (11 archivos) y `AGENTS.md` de la raíz; réplica en `.agents/` de la raíz (fuera de Git); ADR 0019. Sin commits |

## Pasos
## Decisiones
1. **[2026-10-31 19:26] Origen: issue #1 de rosinet-89/agent-kit-sdd.** Carpeta `0008` asignada por el usuario (4 dígitos). Antecedentes: sin coincidencia directa en el índice; relacionadas `002_Kit050` (migracion-bd), `003_OptimizarArnes` (fuente única, ADR 009) y `004_ModeloYPermisosAgentes` (omitClaudeMd, ADR 011 y 012). Deuda: ninguna abierta del tema. Otras specs en paralelo: 0009 y 0010.
2. **[2026-10-31 19:31] Spec en borrador y clarify: CAMBIOS NECESARIOS.** Planner entregó 5 HU y 18 RF con 2 dudas abiertas. Reviewer devolvió 20 hallazgos (bloqueantes: 1, 2, 10, 11, 13, 18). La spec sigue en `borrador`; sin aprobar. No se implementó ni se hizo commit.
3. **[2026-10-31 20:41] Respuestas del usuario aplicadas a la spec.** Versión 0.6.0 única para 0008, 0009 y 0010 (fusión 0008, 0010, 0009); solo enlace a Guardrails en skill sdd y constituciones; SQL permitido en el template; lista cerrada de lectura con precedencia (lo no listado, prohibido); script sql/ lo genera planner o implementer si reviewer/coordinator no pueden; se ajustan las filas y el contador de restricciones-de-rol.md; ACTUALIZAR.md y la constitución del kit sin cambios (solo changelog).
4. **[2026-10-31 20:41] Segundo clarify: CAMBIOS NECESARIOS (leve).** Bloqueantes: (1) RF-6b choca con planner.md (no escribe sql/), coordinator y la fila de restricciones-de-rol; (2) RF-5 vs RF-8 sobre código que el agente entrega; (3) alcance de RF-13 sin cerrar (implementer, sdd-implement, anti-patrón de sdd, migracion-bd). Menores: lista de lectura que depende del issue, verificación de RF-9 con nombres de herramientas de agente, quién crea el manifiesto 0.6.0 y la entrada del changelog compartida, réplica en la raíz del clon, tope de lecturas y colisión de «Guardrails» con napkin. La spec sigue en `borrador`.
5. **[2026-10-31 23:15] Segunda ronda del usuario aplicada.** Script `sql/` lo escribe solo implementer (planner.md y su fila no cambian; planner/reviewer/coordinator lo describen en el chat). La prohibición recae solo sobre lo que el agente ejecuta, no sobre el código que entrega. migracion-bd conserva su explicación y solo la prohibición remite a Guardrails. Manifiesto 0.6.0 y changelog una sola vez al final, tras 0009. La raíz del clon también recibe Guardrails.
6. **[2026-10-31 23:15] Tercer clarify: CAMBIOS NECESARIOS; hallazgos resueltos por la sesión con criterio razonable.** Lista fija de líneas a retirar (RF-13) en lugar de grep por sentencias; variantes de SELECT con efecto, prohibidas por precedencia; RF-6b vive solo en el guardrail 1; se conservan dos enlaces en la línea de Datos de sdd; el tope de lecturas existe solo en sdd-implement; lista mínima de términos de RF-9. Quedan 2 dudas para el usuario: comandos de AGENTS.md que pueden escribir en una BD (propuesta: no se ejecutan) y ventana de E20 (propuesta: se acepta). La spec sigue en `borrador`.
7. **[2026-10-31 23:29] Tercera ronda del usuario y clarify final: APROBABLE.** Comandos que pueden escribir en una BD no se ejecutan, ni en BD local o descartable (decisión firme). Ventana de E20 aceptada: 0008 se valida sin manifiesto 0.6.0 y E20 se verifica una vez tras 0009. Las líneas :3, :8, :58 y :63 de migracion-bd entran en la lista fija de RF-13; :14, :27 y :59 se conservan. Menores del reviewer, sin bloqueo: criterio de duda para comandos de Comandos (igual que el SQL: ante duda no se ejecuta) y aclarar que se pierde en sdd el anti-patrón «SQL suelto fuera de sql/», que sigue en migracion-bd. La spec sigue en `borrador` hasta que el usuario la apruebe.
8. **[2026-10-31 23:33] Spec aprobada por el usuario.** `Estado: aprobada (2026-10-31 23:33)`.
9. **[2026-10-31 23:42] Plan escrito (`02`, `03`; 145 min estimados, F0 a F7) y revisado: CAMBIOS NECESARIOS leves.** Pendientes: (1) RF-18 exige ADR y el plan lo deja condicionado al visto bueno; (2) criterio E19 mal formulado: `diff -rq template/.agents .agents` muestra también `skills/caveman`; (3) `reviewer.md` › «Qué no hace» repite la prohibición y no está en la lista fija de RF-13: decidir si entra. Menores opcionales: aclarar la frase común en `planner.md` y el «Además…» de la nota de «Qué lee cada rol». Sin `04_tasks`.
10. **[2026-11-01 13:52] Respuestas del usuario al plan incorporadas; revisión final APROBABLE.** `reviewer.md`:21 entra en la lista de RF-13; criterio de duda de los comandos (ante duda no se ejecuta) confirmado y anotado en RF-5 y RF-8; ADR 0019 incondicional en F7; principio 5 de la constitución solo con remisión; las líneas de changelog de 0008 se guardan aquí para pegarlas al cerrar 0009; criterio E19 admite `napkin.md` y `skills/caveman`. Estimado: 150 min (F2 sube a 20). Opcionales del reviewer sin corregir en `02`: §1 dice «dos comandos» (son tres), §3 omite `reviewer.md`:21, §2.4 deja esa viñeta bajo «no tocar» y §4 paso 4 no nombra `reviewer.md`:21. El plan sigue sin aprobar.
11. **[2026-11-01 13:56] Plan aprobado por el usuario** («Procede» tras leer el resumen). Arranca la fase `tasks`; se corrigen antes los 4 opcionales de `02`.
12. **[2026-11-01 14:01] Tareas escritas y revisadas: APROBABLE.** `04_tasks.md`: 25 pasos en 8 fases, 150 min. Los 4 opcionales de `02` quedaron corregidos. Observaciones menores del reviewer: F7.1 escribe en `05_avance.md`, así que lo hace el coordinator (el implementer solo entrega el texto); el grep de «lista cerrada» de F6.1 puede dar coincidencias ajenas; el conteo de filas de F4.1 incluye la cabecera y se comprueba leyendo la tabla. Tareas sin aprobar.
13. **[2026-11-01 14:51] Tareas aprobadas por el usuario.** F7.1 aclarado: el coordinator escribe en `05_avance.md` y el implementer entrega el texto. El usuario pidió NO implementar todavía: se espera a que 0010 tenga plan y tareas para respetar el orden 0008, 0010, 0009. Sin commits.
## Líneas de changelog de 0008 (para pegar al cerrar 0009 en la entrada 0.6.0 de `ChangelogAgentKitSdd.md`)
```markdown
- `AGENTS.md` incorpora la sección «Guardrails (todos los agentes, siempre)», con la regla 1: la base de
  datos solo se lee. Los agentes, las skills `sdd` y `migracion-bd`, los comandos de implementación,
  constitución e inicio y `restricciones-de-rol.md` remiten a ella; esta suma la fila correspondiente
  (instrucción, no garantía) y la recomendación de un usuario de BD de solo lectura.
- `AGENTS.md` es grupo B: en un proyecto existente la sección llega como faltante y se incorpora a
  mano, junto a la nota «Qué lee cada rol» de 0.5.0. Los agentes (grupo A) salen KIT-VIEJO o
  PERSONALIZADO según cada proyecto.
```
14. **[2026-11-01 15:44] Implementación F0 a F7 ejecutada, un implementer por paso.** Sin choques con 0009 ni 0010 (ninguno había tocado `template/`). Archivos: `template/AGENTS.md`, 4 agentes, `migracion-bd/SKILL.md`, `sdd/SKILL.md`, `restricciones-de-rol.md`, `sdd-implement.md`, `sdd-constitucion.md`, `sdd-init.md`; raíz: `AGENTS.md` y réplica de los 10 archivos de `.agents/`; `.docs/004_adr/0019_ReglaGlobalBdSoloLectura.md` y su fila. Sin manifiesto, versión ni changelog (RF-16). El 0019 estaba libre en local y en la rama remota. Observaciones opcionales sin aplicar (no estaban en el plan): `coordinator.md`:10 queda «"Git" y "Reglas duras" y Guardrails» (dos «y»); `reviewer.md`:21, `implementer.md`:13 y `sdd-implement.md`:12 mezclan forma negativa y positiva; `sdd-init.md`:38 tiene dos paréntesis seguidos; `migracion-bd/SKILL.md`:8 reflujo largo.
15. **[2026-11-01 16:02] Implementación aprobada por el usuario** y pedido de aplicar las 4 observaciones de redacción.
16. **[2026-11-01 16:04] Pulido de redacción aplicado.** `coordinator.md`:10 («"Git", "Reglas duras" y "Guardrails"»); `reviewer.md`:21, `implementer.md`:13 y `sdd-implement.md`:12 cierran con «Sin excepciones: `AGENTS.md` › Guardrails.»; `sdd-init.md`:38 («(ver Guardrails; o si no aplica)»); `migracion-bd/SKILL.md`:8-9 reflujo. Seis archivos en `template/` replicados a la raíz. `diff -rq template/.agents .agents` solo `napkin.md` y `skills/caveman`; utf-8 sin BOM; las remisiones de RF-13 intactas. Sin cierre ni commits.
17. **[2026-11-03 21:33] Cierre ejecutado a pedido del usuario («cerrar las tres specs»).** Comprobado en solo lectura: las dos viñetas de changelog de 0008 ya están en `ChangelogAgentKitSdd.md` 0.6.0 (pegadas por 0009), y los 11 archivos de `template/` que tocó 0008 coinciden con la huella de `manifiestos/0.6.0.md` (huella calculada ahora). El ADR 0019 está en `.docs/004_adr/README.md`. No hay `06_flujo.md`: el feature es solo Markdown y el usuario no lo pidió; por confirmar por el usuario, como en las specs 003 a 007. Sin deuda resuelta ni creada; sin commits.
## Pruebas
## Pruebas realizadas
- F0.1/F0.2 (implementer, solo lectura): sin cambios de 0009/0010 en `template/`; líneas de RF-13 localizadas; `diff -rq template/.agents .agents` previo solo `napkin.md` y `skills/caveman`.
- F6.1 a F6.5 (reviewer, solo lectura): lista fija de RF-13 leída en `template/` y raíz; RF-9 con cero coincidencias; 4 agentes con Guardrails; «Nueve filas» con 9 filas; E19 con las dos diferencias admitidas; utf-8 sin BOM; alcance no excedido.
- validate (reviewer): APROBADO F0 a F6; RF-1 a RF-15 y RF-19 CUMPLE; RF-16, RF-17 y RF-18 PENDIENTE hasta F7, ya hecho (ADR 0019 verificado por existencia y fila de índice; viñetas en este archivo). E20 no se evalúa por la ventana aceptada.
## Tiempos
| Paso | Inicio | Fin | Minutos | Pausa | Nota |
|---|---|---|---|---|---|
| plan | 2026-10-31 23:33 | 2026-10-31 23:42 | — | — | Incluye 02, 03 y su revisión. Minutos exactos no medidos (el reloj de la sesión saltó entre llamadas); sin cierre |
| tasks | 2026-11-01 13:56 | 2026-11-01 14:01 | ~5 | — | Incluye 04, corrección de 02 y revisión |
| implement F0-F7 | 2026-11-01 15:17 | 2026-11-01 15:44 | ~28 | — | Un implementer por paso; incluye F6 (reviewer), validate y ADR. Estimado 150 min |
| pulido de redacción | 2026-11-01 16:02 | 2026-11-01 16:04 | ~3 | — | Seis archivos de template/ y réplica a la raíz |
| cierre | 2026-11-03 21:33 | 2026-11-03 21:33 | ~5 | — | Comprobaciones de changelog, manifiesto, ADR 0019 e índice; re-cierre 2026-11-21 a `Cerrado` (corrida conjunta de 8 carpetas con la leyenda de la 0012): validación registrada en el retomo y columna del índice vaciada |
## Plan contra real
| Fase | Estimado (min) | Real (min) | Nota |
|---|---|---|---|
| plan (02 y 03) | — | no medido | El reloj de la sesión saltó entre llamadas; sin minutos |
| tasks (04) | — | ~5 | Incluye corrección de 02 y revisión |
| F0 a F7 (25 pasos, un implementer por paso, F6, validate y ADR 0019) | 150 | ~28 | 15:17 a 15:44 |
| Pulido de redacción (aprobado por el usuario) | — | ~3 | Seis archivos y réplica |
| Total de implementación | 150 | ~31 | Real ≈ 21 % del estimado |

Ajuste para el siguiente plan: en un feature solo Markdown con un implementer por paso, el real ronda un quinto del estimado; estimar los pasos de edición mínima en 1 a 3 min cada uno. La parte lenta fue esperar decisiones del usuario (clarify de tres rondas y aprobaciones), no la edición.

