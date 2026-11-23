# 01 — Spec: Bitácora por subagente
Estado: cerrada
Ticket: sin ticket

## Contexto y objetivo
La skill `bitacora-uso` (spec `0021_BitacoraDeUso`, kit 0.16.0) completa los tokens de cada entrada
sumando, dentro de la ventana Inicio–Fin de su fila de Tiempos, los mensajes de la sesión principal y
de todos los subagentes. Con lotes en paralelo cada lote recibe los tokens de toda la ola. En
`0022_PlanContraRealEnVivo`, ola 1, la bitácora dio 3,18 M tokens a cada uno de los 5 lotes; lo real
fue 2,49 M de coordinación de la sesión principal y entre 77 mil y 171 mil por lote (de 19 a 41 veces
menos). El total de Sonnet del día salió inflado 2,9 veces. Además, una ventana con mensajes de dos
sesiones del proyecto (por ejemplo tras un `/clear`) deja la entrada en «no disponible (sesiones
paralelas)»: pasó dos veces en la 0022. `MEMORY.md` lo registra como trampa vigente.

Objetivo: que cada entrada sume solo los tokens que gastó quien hizo el tramo. Una entrada delegada
suma el transcript de su subagente. Una entrada en el chat suma la sesión que la hizo. La
coordinación de la sesión principal mientras trabajan los subagentes va en una entrada aparte. Así
los totales por modelo y por rol se leen sin ajustes a mano.

Origen: recomendación 3 del reporte de la bitácora del 2026-11-22, tras el cierre de la 0022.

## Usuarios y roles
- Dev con la bitácora activa en una spec: lee el reporte para levantar issues de rendimiento.
- Sesión principal: única que escribe la bitácora (sin cambio).
- Subagentes: no escriben la bitácora; su lanzamiento devuelve el id que la sesión anota.

## Historias de usuario
- HU-1. Como dev, quiero que cada lote de una ola tenga solo sus tokens, para comparar el costo real
  de delegar contra hacerlo en el chat.
- HU-2. Como dev, quiero ver aparte cuánto gastó la sesión principal coordinando una ola, para saber
  si conviene delegar pasos cortos.
- HU-3. Como dev, quiero que un tramo hecho tras un `/clear` o con otra sesión abierta tenga sus
  tokens, sin quedar «no disponible».

## Definiciones
- **Id de sesión:** identificador de la sesión principal que da la herramienta. En Claude Code, la
  variable `CLAUDE_CODE_SESSION_ID`, que coincide con el nombre del `<sesión>.jsonl`.
- **Id de subagente:** identificador que la herramienta devuelve al lanzar un subagente. En Claude
  Code, el `agentId`; su transcript es `<sesión>/subagents/agent-<id>.jsonl`.
- **Entrada en el chat:** entrada de un tramo que hizo la sesión principal, con el rol en el chat.
- **Entrada delegada:** entrada de un tramo (o lote) que hizo un subagente lanzado en ese tramo.
- **Entrada de coordinación:** entrada con rol `chat` que reúne los mensajes de la sesión principal
  en la ventana de una o más entradas delegadas que no cuenta ninguna entrada en el chat.
- **Ventana de delegación:** de Inicio a Fin de las filas de Tiempos de las entradas delegadas que se
  escriben en una misma escritura (una ola, o un solo lote).

## Requisitos funcionales (EARS)
- RF-1: CUANDO la sesión principal escribe una entrada, EL SISTEMA anota dos columnas nuevas:
  «Sesión», con el id de sesión de quien escribe, y «Subagente», con el id del subagente si es una
  entrada delegada o vacío si no lo es. SI la herramienta no da el id, la columna lleva «no disponible».
- RF-2: CUANDO un tramo o un lote lo hace un subagente, EL SISTEMA escribe una entrada delegada con el
  rol, el modelo y el id que devolvió ese lanzamiento, en la misma escritura que su fila de Tiempos.
- RF-3: CUANDO una fila de Tiempos reúne trabajo en el chat y uno o más subagentes (por ejemplo,
  `validate` con `tester`), EL SISTEMA escribe una entrada en el chat y una entrada delegada por cada
  subagente, todas con el paso de esa fila, en la misma escritura. Esta regla reemplaza «una fila de
  Tiempos, una entrada» de `bitacora-uso` › «Registro por tramo»; un lote sigue siendo una entrada.
- RF-4: CUANDO la sesión principal escribe una o más entradas delegadas, EL SISTEMA escribe en esa
  misma escritura una sola entrada de coordinación para su ventana de delegación, con rol `chat`, el
  modelo de la sesión, la nota «coordinación» y los minutos vacíos. SI esa escritura ya lleva una
  entrada en el chat con la misma ventana (RF-3), no escribe la de coordinación.
- RF-5: CUANDO completa los tokens de una entrada delegada, EL SISTEMA suma solo el transcript de su
  subagente, sin ventana. SI ese mismo id aparece en más de una entrada (un subagente reanudado), cada
  entrada suma solo los mensajes de ese transcript que caen en su ventana.
- RF-6: CUANDO completa los tokens de una entrada en el chat que trae id de sesión, EL SISTEMA suma
  solo los mensajes de esa sesión en su ventana, sin los de los subagentes ni los de otras sesiones,
  y no marca «no disponible (sesiones paralelas)».
- RF-7: CUANDO completa los tokens de una entrada de coordinación, EL SISTEMA suma los mensajes de la
  sesión anotada en su ventana que ninguna entrada en el chat de la misma spec ya contó.
- RF-8: MIENTRAS completa los tokens de una spec, EL SISTEMA cuenta cada `message.id` una sola vez en
  toda su bitácora; si dos entradas lo alcanzan, la entrada en el chat prevalece sobre la de
  coordinación y, entre dos de coordinación, la escrita primero.
- RF-9: SI una entrada no trae las columnas «Sesión» y «Subagente» (escrita antes de esta versión),
  o es una entrada en el chat o de coordinación con «Sesión» en «no disponible», EL SISTEMA la completa con la regla anterior (ventana sobre la sesión principal y los subagentes, y
  «no disponible (sesiones paralelas)» con más de una sesión) y no la reescribe.
- RF-10: CUANDO arma el reporte, EL SISTEMA suma los tokens de cada entrada una vez, sin ajuste por
  ola; muestra la coordinación dentro del rol `chat` y, aparte, su total; y los minutos de las
  entradas de coordinación no se suman a los totales de minutos. Las entradas de coordinación
  cuentan como tramos en los hallazgos, marcadas «coordinación».
- RF-11: SI la herramienta no da id de subagente o no deja un transcript por subagente, o el
  transcript del id no existe, EL SISTEMA deja esa entrada en «no disponible» y nunca estima.
- RF-12: EL SISTEMA declara, por herramienta, en la tabla «Fuente de tokens», dónde se lee el id de
  sesión y dónde está el transcript de cada subagente; una herramienta sin esos datos queda en
  «no disponible».
- RF-13: CUANDO escribe la entrada de un turno de una actividad, EL SISTEMA anota «Sesión» y deja
  «Subagente» vacío; al completar tokens suma los mensajes de esa sesión y de sus subagentes en la
  ventana del turno, sin los de otras sesiones. Sin id de sesión rige RF-9.

## Requisitos no funcionales
- Privacidad sin cambio: además de los campos ya permitidos, solo se leen nombres de archivo e ids;
  nunca contenido de mensajes.
- Costo: las entradas nuevas (delegadas, de coordinación) van en la misma escritura que su fila de
  Tiempos; no hay llamadas aparte (trampa de `MEMORY.md` del 2026-11-22).
- Kit genérico (constitución 1): las reglas hablan de id de sesión y de subagente; lo propio de
  Claude Code vive solo en la tabla «Fuente de tokens».
- Una regla, una fuente: la forma de la entrada y del cálculo vive en `bitacora-uso`; los comandos y
  el contrato de avance remiten.

## Casos límite
- Ola de 5 lotes en paralelo: 5 entradas delegadas y 1 de coordinación, en una escritura.
- Subagente en segundo plano que sigue después del Fin de su fila: sus tokens entran igual (sin
  ventana, RF-5).
- Relanzamiento: el segundo lanzamiento tiene otro id y su propia entrada, con motivo
  «relanzamiento».
- El lanzamiento falla y no devuelve id: entrada delegada con «Subagente» «no disponible» y tokens
  «no disponible»; la coordinación se escribe igual.
- Subagente lanzado por otro subagente (coordinator como subagente): el id lo devuelve quien lanzó en
  su respuesta. Supuesto: si no llega, «no disponible».
- Envoltorio de otra herramienta (`corlos`, `claudio`): la entrada suma solo el transcript del
  envoltorio; la nota dice «tokens del envoltorio». Los de la otra herramienta quedan fuera.
- Tramo en el chat que cruza un `/clear`: la entrada lleva la sesión que escribe; los mensajes de la
  sesión anterior no entran. Supuesto: se acepta esa pérdida.
- Entrada en el chat y delegación con ventanas que se cruzan (paso en el chat durante una ola): la
  entrada en el chat se queda esos mensajes (RF-8); la coordinación, el resto.
- Ventana de delegación con pausa: la coordinación lleva «ventana con pausa»; las delegadas no la
  necesitan.
- Bitácora mixta (entradas viejas y nuevas): cada una con su regla (RF-9); el reporte las suma.

## Fuera de alcance
- Reescribir o recalcular las bitácoras de 0021 y 0022.
- Tokens dentro de Cursor, OpenCode u otra herramienta externa.
- Repartir la coordinación entre los lotes de una ola.
- Un comando `/sdd-*` nuevo para la bitácora.
- Cambiar la fila de Tiempos de `05_avance.md` (su forma sigue igual).

## Criterios de finalización
- La skill `bitacora-uso` cumple RF-1 a RF-13, y los archivos que la citan remiten a ella sin copiar
  la regla.
- Prueba con los transcripts de la 0022, ola 1 (5 lotes de 21:03): con los ids de cada lote, cada
  entrada delegada da el total de su transcript (L1 a L5: de 45 345 a 140 814 tokens, medidos en el
  plan) y la coordinación, con su ventana de delegación (21:03 a 21:06) menos los mensajes que ya
  cuentan las entradas en el chat F0.1 y F6.4 (RF-7, RF-8), da 1 033 088; la suma de las 6
  (≈ 1,42 M) es menor que la de la regla anterior (15,9 M).
- Prueba de dos sesiones: una entrada en el chat con id de sesión, en una ventana con mensajes de
  otra sesión, da sus tokens y no «no disponible (sesiones paralelas)».
- Una bitácora anterior (0022) se completa y se reporta igual que antes (RF-9).
- Bump de versión (0.18.0), changelog, manifiesto y E20 en verde; lint de `AGENTS.md` › Comandos en
  cada archivo tocado; ensayo de instalación sin pisar nada.

## Dudas abiertas
Sin marcadores. Supuestos explícitos, reversibles al aprobar:
- La coordinación se escribe al registrar (RF-4), no al completar tokens.
- Los minutos de la coordinación quedan vacíos para no contar dos veces el tiempo.
- `tester` dentro de `validate` lleva su propia entrada (RF-3): una fila de Tiempos puede dar más de
  una entrada.
- Una actividad sigue con una entrada por turno que incluye a sus subagentes (RF-13): un turno no se
  reparte entre roles. Marcado para revisión.
- La coordinación entra en los hallazgos del reporte como un tramo más (RF-10). Marcado para revisión.
