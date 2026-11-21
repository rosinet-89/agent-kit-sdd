# 05 — Avance: Falso positivo de SDD-INIT-PENDIENTE después de /sdd-init
Leyenda: `[ ]` Pendiente · `[~]` En curso · `[x]` Hecho · `[!]` Bloqueado

## PUNTO DE RETOMO — leer esto primero
| Dato | Contenido |
|---|---|
| Última sesión | 2026-11-21 10:04 |
| Fase actual | `cierre`: `Cerrado` (2026-11-21 10:04) |
| Último paso cerrado | F4.5: ensayo mktemp: espacio inicial, BOM antes del titulo, BOM antes del comentario, 
 suelto, segundo comentario y marcador movido dan FALLA; control OK; copia borrada |
| Siguiente paso | Ninguno |
| Bloqueos abiertos | Ninguno |
| Validación del conjunto | Aprobada: validate del 2026-11-01 19:07, APROBADO (Pruebas). Registrada en el punto de retomo el 2026-11-21 con el visto bueno de la persona |
| Aviso o ticket | Issue #3 cerrado en GitHub; lo confirmó la persona el 2026-11-21 |
| Estado BD | No aplica |
| Código | Solo Markdown: `template/AGENTS.md`, `AGENTS.md` raíz, `README.md`, 6 piezas de `template/.agents/` con su réplica en `.agents/` (fuera de Git), ADR 0020 y su fila. Sin commits |

## Pasos
- [x] spec (19:26 a 19:28)
- [x] clarify (aprobada 2026-11-01 14:49)
- [x] plan (aprobado 2026-11-01 15:03)
- [x] tasks (aprobadas 2026-11-01 15:17)
- [x] F0.1 (2026-11-01 16:05): 0008 implementada y pulida (Guardrails en AGENTS.md, template/ y 11 archivos; ADR 0019); 0009 en espera, sin t
- [x] F0.2 (2026-11-01 16:06): frases a cambiar localizadas en las líneas esperadas; diff -rq solo napkin.md y skills/caveman; verificado por
- [x] F0.3 (2026-11-01 16:07): E1 a E20 en 4 líneas esperadas de validar-estructura y sdd-estructura; 20 filas E
- [x] F1.1 (2026-11-01 16:08): paso 1 de Cada sesion reformulado en template/AGENTS.md sin literal; cuenta del literal 2; extraccion de posic
- [x] F1.2 (2026-11-01 16:09): fila coordinator de template/AGENTS.md remite al paso 1 sin literal; cuenta del literal baja de 2 a 1 (solo el
- [x] F1.3 (2026-11-01 16:09): paso 1 de la raiz reformulado sin literal; cuenta del literal en AGENTS.md raiz baja de 2 a 1; lint OK
- [x] F1.4 (2026-11-01 16:10): fila coordinator de la raiz remite al paso 1; grep del literal en AGENTS.md raiz = 0; lint OK
- [x] F2.1 (2026-11-01 16:11): sdd.md de template/ remite al paso 1 en dos frases, sin literal; lint OK; sin copiar a .agents/ (F2.6)
- [x] F2.2 (2026-11-01 16:12): coordinator.md de template/ remite al paso 1 en dos frases; Guardrails de 0008 intacto; lint OK
- [x] F2.3 (2026-11-01 16:13): skills/sdd/SKILL.md de template/ remite al paso 1, sin literal; lint OK
- [x] F2.4 (2026-11-01 16:13): sdd-init.md de template/ reformula la frase ya no contiene; solo queda la linea que nombra lo que se borra; li
- [x] F2.5 (2026-11-01 16:14): README.md linea Mientras exista reformulada sin literal; queda solo la linea que explica que se quita el marca
- [x] F2.6 (2026-11-01 16:15): cuatro piezas copiadas a .agents/; diff -rq solo napkin.md y skills/caveman; grep contiene vacio
- [x] F3.1 (2026-11-01 16:16): fila E21 en template/validar-estructura.md y rangos E1 a E21; 21 filas E; lint OK
- [x] F3.2 (2026-11-01 16:17): Detalle de E21 (intro y pasos 1 a 5) insertado en template/validar-estructura.md; lint OK; falta la cadena (F3
- [x] F3.3 (2026-11-01 16:17): cadena del marcador y 4 mensajes de FALLA en template/validar-estructura.md; diff cadena vs comentario de la p
- [x] F3.4 (2026-11-01 16:18): sdd-estructura.md de template/ pasa a E1 a E21 en dos frases; sin literal; lint OK
- [x] F3.5 (2026-11-01 16:19): validar-estructura.md y sdd-estructura.md copiados a .agents/; diff -rq solo napkin.md y skills/caveman
- [x] F4.1 (2026-11-01 16:20): lint de 9 archivos tocados: utf-8 sin BOM, rutas existentes, sin marcadores extra; la cadena del marcador no s
- [x] F4.2 (2026-11-01 18:59): template/AGENTS.md: literal 1 y extraccion igual a la cadena (comprobado con comparacion exacta); AGENTS.md ra
- [x] F4.3 (2026-11-01 19:00): 6 piezas de RF-5 remiten al paso 1 sin regla propia; grep contiene vacio; paso 1 se entiende por lectura
- [x] F4.4 (2026-11-01 19:01): ensayo en mktemp -d con .kit-version a mano, sin template/: 7 casos (tal cual OK, borrado OK, literal suelto F
- [x] F4.5 (2026-11-01 19:03): ensayo mktemp: espacio inicial, BOM antes del titulo, BOM antes del comentario, \r suelto, segundo comentario 
- [x] F4.6 (2026-11-01 19:04): ensayo con template/ presente: iguales OK; comentario de la plantilla alterado FALLA por desincronia; sin temp
- [x] F4.7 (2026-11-01 19:04): 21 filas E; sin E1 a E20 residual; literal en sdd-estructura = 0; replicas identicas; versionado/changelog/man
- [x] F5.1 (2026-11-01 19:05): lineas de Changelog 0.6.0 (grupo B) y nota de merge dejadas en 05_avance.md; VersionAgentKitSdd, Changelog y m
- [x] ADR.1 (2026-11-01 19:05): ls local: 0019 existe (0008); rama remota (@{u}) llega a 0018; siguiente libre 0020
- [x] ADR.2 (2026-11-01 19:06): ADR 0020 creado en .docs/004_adr/ (0020_MarcadorSinLiteralFueraDelComentario.md); lint OK; fuera de template/ 
- [x] ADR.3 (2026-11-01 19:07): fila 0020 en .docs/004_adr/README.md y referencia al ADR en 03_plan.md; template/.docs sin cambios
- [x] implementación F0 a F5 y ADR (2026-11-01 19:10)
- [x] validate (APROBADO, 2026-11-01 19:10)

## Decisiones
1. **[2026-10-31 19:26] Número 0010 asignado por quien invoca, con 4 dígitos.** Las carpetas existentes llegan a `007` (ancho 3); un ítem nuevo debería conservar ese ancho (`.docs/004_adr/014_ConvivenciaDeAnchosDeNumeracion.md`). Se respeta la asignación del usuario y se deja como pregunta abierta.
2. **[2026-10-31 19:26] Antecedentes.** Sin carpeta que coincida de lleno. Parciales: `004_ModeloYPermisosAgentes` (anotó el falso positivo en la raíz y lo esquivó con una copia sin el literal; no tocó `template/AGENTS.md`), `003_OptimizarArnes` y `007_DeudasAbiertas` (tocaron `/sdd-init` y la lectura del marcador). Deuda abierta relacionada: ninguna (001 a 008 resueltas). Origen: issue #3 de `rosinet-89/agent-kit-sdd`.
3. **[2026-10-31 20:38] Respuestas del usuario (vía coordinador).** Versión única 0.6.0 compartida con 0008 y 0009, manifiesto `manifiestos/0.6.0.md`, orden de fusión 0008, 0010, 0009. Carpeta 0010 de 4 dígitos se mantiene. Entra el `AGENTS.md` de la raíz (líneas 10 y 82). E21 nuevo en `/sdd-estructura` con sus réplicas. Se sigue la propuesta del issue (describir el marcador sin el literal). Alternativa descartada: buscar el comentario completo en 6 o más piezas.
4. **[2026-10-31 23:12] Segunda ronda del usuario.** E21 reconoce solo el comentario marcador exacto. No se permite citar el literal en el `AGENTS.md` de un proyecto (cero coincidencias fuera del comentario exacto). Sin `AGENTS.md`, E21 da FALLA.
5. **[2026-10-31 23:12] Decisiones del coordinador, pendientes de visto bueno del usuario.** Fuente única de la regla de detección: el paso 1 de «Cada sesión»; las demás piezas remiten. `VersionAgentKitSdd` a 0.6.0 en el alcance de RF-8. Manifiesto y Changelog 0.6.0 se crean una sola vez al final, tras cerrar 0009.
6. **[2026-10-31 23:29] Tercera ronda del usuario.** Regla del cero: tras /sdd-init, grep del literal en `AGENTS.md` = 0; en la plantilla y en proyectos pendientes el literal solo está en el comentario exacto; la raíz da 0 absoluto. El cero rige solo para `AGENTS.md`; las demás piezas pueden nombrar el marcador (lista de excepciones en la spec). La definición normativa del marcador es el comentario de la plantilla; el paso 1 lo describe sin literal; E21 lo compara sin redefinirlo. Visto bueno a la fuente única (paso 1, principio 3 de la constitución), a 0.6.0 con manifiesto y Changelog al final tras 0009, y a vigilar la raíz con el grep de RF-1.
7. **[2026-10-31 23:37] Cuarta ronda del usuario.** E21 compara contra la cadena exacta del comentario marcador copiada en `validar-estructura.md` (y `sdd-estructura.md` si la muestra), excepción permitida. La fuente normativa sigue siendo el comentario de la plantilla, citado por texto. E20 verifica que coincidan. Un proyecto instalado no necesita `template/`.
8. **[2026-10-31 23:37] Reglas menores del planner, a confirmar al aprobar.** Segundo comentario idéntico = FALLA. La comparación ignora CRLF/LF y espacios o tabuladores finales. Enlaces con el literal solo en las piezas listadas. Citas de otros archivos por frase, no por número. `ACTUALIZAR.md` fuera de las excepciones (no cita el literal); la nota de grupo B va en el Changelog. Manifiesto y Changelog 0.6.0 son una dependencia de 0009, no un bloqueo.
9. **[2026-11-01 13:48] Quinta ronda del usuario.** La sincronía entre la cadena copiada y el comentario de `template/AGENTS.md` la verifica E21 cuando `template/` existe; no se toca E20. La cadena en 3 sitios es excepción explícita al principio 3, con E21 como control cruzado. El ensayo crea `.kit-version` a mano en una copia de prueba (`mktemp -d`) antes de correr E21, con y sin literal fuera del marcador. Menores: marcador repetido = FALLA; CRLF/LF y espacios finales ignorados; «pueden» = piezas exentas, no obligación; «justo bajo el título» = primera línea no vacía tras el título; enlace simbólico se lee como su destino (regla del planner, a confirmar).
10. **[2026-11-01 13:58] Sexta ronda del usuario.** La raíz se cubre solo con el grep manual de RF-1 (excepción de HU-3); no se le da `.kit-version`. Cadena ausente o alterada = FALLA; si difieren `validar-estructura.md` y `sdd-estructura.md`, manda `validar-estructura.md`. Criterio de posición con comando `sed | awk | sed`, que quita `\r` antes de awk. `template/AGENTS.md` y la raíz se rigen por la regla del cero, no son piezas exentas. Los menores van al plan. Enlace simbólico leído como su destino: propuesta, pendiente de confirmación del usuario.
11. **[2026-11-01 14:49] Spec aprobada por el usuario.** `Estado: aprobada`. Confirmó que un `AGENTS.md` que sea enlace simbólico se lee como su destino.
12. **[2026-11-01 15:01] Plan redactado.** 15 archivos (7 de `template/`, 6 réplicas en `.agents/`, `AGENTS.md` raíz y `README.md`), 6 fases, 105 min estimados. E21 en el bloque `sesión`; orden de pasos y mensajes de FALLA fijados. Las rutas que no existen en un proyecto instalado (`template/AGENTS.md`, `.kit-version`) van sin comillas inversas para no dar FALLA en E11. Decisiones a aprobar por el usuario: 2 (el bump de `VersionAgentKitSdd` lo hace la F6 de 0009, no 0010; desvía de RF-8), 5 (rutas sin comillas inversas) y 7 (ADR candidato, no creado).
13. **[2026-11-01 15:03] Plan aprobado por el usuario.** Decisión 2: `VersionAgentKitSdd` a 0.6.0 lo sube la F6 de 0009; 0010 solo deja sus líneas de grupo B en este archivo (aclaración a RF-8 y Alcance, sin `/sdd-change`). Decisión 5: rutas inexistentes en un proyecto instalado, sin comillas inversas en las piezas nuevas. Decisión 7: sí se crea un ADR al cerrar, con la skill `adr` y el siguiente número libre (0008 reserva el 0019, así que a esta le tocaría el 0020); cubre el marcador sin literal fuera del comentario, la regla del cero, el paso 1 como fuente única y la cadena copiada con E21 como control cruzado (excepción al principio 3).
14. **[2026-11-01 15:11] Aclaración a la spec y ADR en el plan.** Se anotó en `01_spec.md` (sin `/sdd-change`, `Estado: aprobada`) que el bump a 0.6.0 lo hace la F6 de 0009 y que las rutas inexistentes en un proyecto instalado van sin comillas inversas. Se añadió al plan y a las tareas la fase ADR (ADR 0020 con la skill `adr`, tras la fusión de 0008 que reserva el 0019): 115 min en total.
15. **[2026-11-01 15:17] Tareas aprobadas por el usuario.** No se implementa todavía: el orden es 0008, 0010, 0009.
16. **[2026-11-01 16:05] Implementación autorizada por el usuario (tareas aprobadas 2026-11-01 15:17; 0008 ya implementada). F0.1: 0008 tocó ya AGENTS.md, template/AGENTS.md, coordinator.md, sdd-init.md y skills/sdd/SKILL.md; 0009 no ha tocado nada.**
17. **[2026-11-01 18:58] Retomada tras corte por error de API.** Estado real comprobado antes de seguir: F0.1 a F4.1 hechos y coherentes con el avance (template/ y `.agents/` con `diff -rq` solo `napkin.md` y `skills/caveman`; 21 filas E en ambos `validar-estructura.md`; literal en `template/AGENTS.md` = 1 (solo el marcador) y en `AGENTS.md` raíz = 0; `grep 'contiene' …` vacío). F4.2 y F4.3 no dejaron nada a medias: eran de solo lectura, no hay restos `/tmp/tmp.*` y no se escribió nada en el repo. Se rehace F4.2 desde cero. Marcas `[x]` en `04_tasks.md`: solo las de F2.5, F3.4 y F3.5 (las puso un implementer); el resto se marca al cierre.

## Líneas para el Changelog 0.6.0 (grupo B) y nota de merge
Para que las pegue quien cierre 0009. No están en `ChangelogAgentKitSdd.md`.

```markdown
- Falso positivo tras `/sdd-init`: el paso 1 de «Cada sesión» y la fila coordinator de `AGENTS.md`
  describen el marcador de instalación pendiente sin escribir el literal y remiten al comentario de
  la plantilla. `/sdd`, el coordinator, la skill `sdd`, `/sdd-init` y el README remiten al paso 1.
- Nuevo chequeo E21 en `validar-estructura` (bloque sesión, solo con `.agents/.kit-version`): FALLA si
  `AGENTS.md` no existe, si contiene el literal fuera del marcador válido, o si la cadena del
  marcador falta o no coincide con la plantilla (con `template/` presente).
- `AGENTS.md` es grupo B: los proyectos existentes incorporan a mano el paso 1 y la fila coordinator
  nuevos. Hasta entonces, E21 da FALLA en un proyecto ya personalizado con una versión anterior
  (esperado).
```

Nota de merge: orden 0008, 0010, 0009. El bump de `VersionAgentKitSdd` a 0.6.0, el Changelog y el manifiesto los hace una sola vez la F6 de 0009. Archivos de `template/` cambiados por 0010 (7, para recalcular sus huellas en el manifiesto): `AGENTS.md`, `.agents/commands/sdd.md`, `.agents/commands/sdd-init.md`, `.agents/commands/sdd-estructura.md`, `.agents/agents/coordinator.md`, `.agents/skills/sdd/SKILL.md`, `.agents/validar-estructura.md`.
18. **[2026-11-01 19:10] Validate APROBADO.** Veredicto del reviewer RF-1 a RF-8 sin bloqueantes. Hallazgos: MEDIA (punto de retomo desfasado, corregido aquí) y BAJA (plan contra real, completado abajo). `reviewer.md` conserva «E1–E20» (lo cambia 0009, decisión 8 del plan). Marcas `[x]` de `04_tasks.md` puestas por la sesión principal al terminar (hasta F4.7 las dejó sin marcar la mayoría de implementers).
19. **[2026-11-02 16:51] Implementación aprobada por el usuario.** `/sdd-cierre` y commits quedan para después, con su confirmación.
20. **[2026-11-03 21:33] Cierre.** Se cierra sin `06_flujo.md` (el usuario pidió cerrar las tres specs; igual que 004 a 007). `01_spec.md` pasa a `Estado: implementada`. Deuda: ninguna deuda existente se resuelve (001 a 008 ya estaban resueltas); se proponen altas, sin crearlas. MEMORY: se proponen entradas, sin escribirlas. Hallazgo al cerrar: en `template/.agents/validar-estructura.md` la fila E1 cambió después de congelar `manifiestos/0.6.0.md` (obra de 0009, no de 0010) y la réplica `.agents/validar-estructura.md` no la tiene; ver el reporte.

## Pruebas
- 2026-11-01 13:58 Comando de posición de `01_spec.md` probado sobre copias LF y CRLF de `template/AGENTS.md` en el scratchpad: ambas imprimen el comentario marcador exacto. Repo sin tocar.
- [2026-11-01 19:07] validate (reviewer): APROBADO. Lint de 22 archivos utf-8 sin BOM; RF-1 a RF-8 cumplen; ensayo en mktemp -d (tal cual OK, borrado OK, literal suelto FALLA, CRLF y espacios finales OK, espacio inicial FALLA, duplicado FALLA, sin AGENTS.md FALLA); seguridad modo completo sin hallazgos; verificación manual de UI: No aplica. F4.4 a F4.6: ensayos propios del implementer en mktemp -d, ya borrados.

## Tiempos
| Paso | Inicio | Fin | Minutos | Pausa | Nota |
|---|---|---|---|---|---|
| spec | 2026-10-31 19:26 | 2026-10-31 19:28 | 2 | 0 | planner |
| clarify | 2026-10-31 19:28 | 2026-11-01 13:58 | — | — | 7 pasadas; incluye espera de respuestas del usuario, minutos activos no medidos |
| plan | 2026-11-01 14:49 | 2026-11-01 15:01 | — | — | planner + revisión del reviewer + corrección; incluye esperas |
| tasks | 2026-11-01 15:02 | 2026-11-01 15:11 | — | — | planner + revisión + corrección; incluye esperas |
| F0.1 | 2026-11-01 16:04 | 2026-11-01 16:05 | 1 | 0 | 0008 implementada y pulida (Guardrails en AGENTS.md, template/ y 11 archivos; ADR 0019); 0 |
| F0.2 | 2026-11-01 16:05 | 2026-11-01 16:06 | 1 | 0 | frases a cambiar localizadas en las líneas esperadas; diff -rq solo napkin.md y skills/cav |
| F0.3 | 2026-11-01 16:06 | 2026-11-01 16:07 | 1 | 0 | E1 a E20 en 4 líneas esperadas de validar-estructura y sdd-estructura; 20 filas E |
| F1.1 | 2026-11-01 16:07 | 2026-11-01 16:08 | 1 | 0 | paso 1 de Cada sesion reformulado en template/AGENTS.md sin literal; cuenta del literal 2; |
| F1.2 | 2026-11-01 16:08 | 2026-11-01 16:09 | 1 | 0 | fila coordinator de template/AGENTS.md remite al paso 1 sin literal; cuenta del literal ba |
| F1.3 | 2026-11-01 16:09 | 2026-11-01 16:09 | 0 | 0 | paso 1 de la raiz reformulado sin literal; cuenta del literal en AGENTS.md raiz baja de 2  |
| F1.4 | 2026-11-01 16:09 | 2026-11-01 16:10 | 1 | 0 | fila coordinator de la raiz remite al paso 1; grep del literal en AGENTS.md raiz = 0; lint |
| F2.1 | 2026-11-01 16:10 | 2026-11-01 16:11 | 1 | 0 | sdd.md de template/ remite al paso 1 en dos frases, sin literal; lint OK; sin copiar a .ag |
| F2.2 | 2026-11-01 16:11 | 2026-11-01 16:12 | 1 | 0 | coordinator.md de template/ remite al paso 1 en dos frases; Guardrails de 0008 intacto; li |
| F2.3 | 2026-11-01 16:12 | 2026-11-01 16:13 | 1 | 0 | skills/sdd/SKILL.md de template/ remite al paso 1, sin literal; lint OK |
| F2.4 | 2026-11-01 16:13 | 2026-11-01 16:13 | 0 | 0 | sdd-init.md de template/ reformula la frase ya no contiene; solo queda la linea que nombra |
| F2.5 | 2026-11-01 16:13 | 2026-11-01 16:14 | 1 | 0 | README.md linea Mientras exista reformulada sin literal; queda solo la linea que explica q |
| F2.6 | 2026-11-01 16:14 | 2026-11-01 16:15 | 1 | 0 | cuatro piezas copiadas a .agents/; diff -rq solo napkin.md y skills/caveman; grep contiene |
| F3.1 | 2026-11-01 16:15 | 2026-11-01 16:16 | 1 | 0 | fila E21 en template/validar-estructura.md y rangos E1 a E21; 21 filas E; lint OK |
| F3.2 | 2026-11-01 16:16 | 2026-11-01 16:17 | 1 | 0 | Detalle de E21 (intro y pasos 1 a 5) insertado en template/validar-estructura.md; lint OK; |
| F3.3 | 2026-11-01 16:17 | 2026-11-01 16:17 | 0 | 0 | cadena del marcador y 4 mensajes de FALLA en template/validar-estructura.md; diff cadena v |
| F3.4 | 2026-11-01 16:17 | 2026-11-01 16:18 | 1 | 0 | sdd-estructura.md de template/ pasa a E1 a E21 en dos frases; sin literal; lint OK |
| F3.5 | 2026-11-01 16:18 | 2026-11-01 16:19 | 1 | 0 | validar-estructura.md y sdd-estructura.md copiados a .agents/; diff -rq solo napkin.md y s |
| F4.1 | 2026-11-01 16:19 | 2026-11-01 16:20 | 1 | 0 | lint de 9 archivos tocados: utf-8 sin BOM, rutas existentes, sin marcadores extra; la cade |
| F4.2 | 2026-11-01 18:59 | 2026-11-01 18:59 | 0 | 0 | template/AGENTS.md: literal 1 y extraccion igual a la cadena (comprobado con comparacion e |
| F4.3 | 2026-11-01 18:59 | 2026-11-01 19:00 | 1 | 0 | 6 piezas de RF-5 remiten al paso 1 sin regla propia; grep contiene vacio; paso 1 se entien |
| F4.4 | 2026-11-01 19:00 | 2026-11-01 19:01 | 1 | 0 | ensayo en mktemp -d con .kit-version a mano, sin template/: 7 casos (tal cual OK, borrado  |
| F4.5 | 2026-11-01 19:01 | 2026-11-01 19:03 | 2 | 0 | ensayo mktemp: espacio inicial, BOM antes del titulo, BOM antes del comentario, \r suelto, |
| F4.6 | 2026-11-01 19:03 | 2026-11-01 19:04 | 1 | 0 | ensayo con template/ presente: iguales OK; comentario de la plantilla alterado FALLA por d |
| F4.7 | 2026-11-01 19:04 | 2026-11-01 19:04 | 0 | 0 | 21 filas E; sin E1 a E20 residual; literal en sdd-estructura = 0; replicas identicas; vers |
| F5.1 | 2026-11-01 19:04 | 2026-11-01 19:05 | 1 | 0 | lineas de Changelog 0.6.0 (grupo B) y nota de merge dejadas en 05_avance.md; VersionAgentK |
| ADR.1 | 2026-11-01 19:05 | 2026-11-01 19:05 | 0 | 0 | ls local: 0019 existe (0008); rama remota (@{u}) llega a 0018; siguiente libre 0020 |
| ADR.2 | 2026-11-01 19:05 | 2026-11-01 19:06 | 1 | 0 | ADR 0020 creado en .docs/004_adr/ (0020_MarcadorSinLiteralFueraDelComentario.md); lint OK; |
| ADR.3 | 2026-11-01 19:06 | 2026-11-01 19:07 | 1 | 0 | fila 0020 en .docs/004_adr/README.md y referencia al ADR en 03_plan.md; template/.docs sin |
| validate | 2026-11-01 19:07 | 2026-11-01 19:10 | — | 0 | reviewer; incluye lectura de 22 archivos |
| cierre | 2026-11-03 21:32 | 2026-11-03 21:33 | — | 0 | sesión principal; verificación de manifiesto y E20 en solo lectura; re-cierre 2026-11-21 a `Cerrado` (corrida conjunta de 8 carpetas con la leyenda de la 0012): validación registrada en el retomo y columna del índice vaciada |

## Plan contra real
| Fase | Estimado (min) | Real (min) | Nota |
|---|---|---|---|
| F0 preparación | 10 | 3 | Solo lectura |
| F1 `AGENTS.md` | 15 | 3 | Cuatro Edit mínimos |
| F2 piezas de RF-5 | 20 | 5 | Cinco archivos y la copia a `.agents/` |
| F3 E21 | 25 | 4 | Fila, detalle, cadena, `sdd-estructura.md` y copia |
| F4 verificación | 30 | 6 | Ensayos en `mktemp -d`; los más largos fueron F4.4 a F4.6 |
| F5 grupo B | 5 | 1 | Lo escribe la sesión principal |
| ADR | 10 | 2 | ADR 0020, fila de índice y enlace |
| **Total implementación** | **115** | **24** | Suma de 29 filas por paso, medidas al minuto |
| validate | — | 3 | Reviewer; 16:07 a 16:10 del 2026-11-01 |

Spec, clarify, plan y tasks no se cuentan: incluyen esperas de respuestas del usuario (clarify: 7 pasadas en un día). El corte por error de API (16:20 a 18:58 del 2026-11-01) no es trabajo.

Para el siguiente plan: en un feature de solo Markdown con un implementer por paso, el real fue una quinta parte del estimado (24 contra 115). La estimación por fase sobrestimó todas; los pasos de lectura y copia valen 1 a 3 min. El tiempo grande estuvo en las rondas de clarify (7), no en implementar.
