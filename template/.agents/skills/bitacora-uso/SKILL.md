---
name: bitacora-uso
description: Bitácora de uso (beta), local por dev. Registra por tramo qué modelo y rol trabajó, cuánto tardó y cuántos tokens gastó, por spec o por actividad suelta, y arma un reporte en el chat listo para copiar a un issue. Usar al decidir si una spec se registra (`/sdd-spec`), al escribir una fila de Tiempos con la bitácora activa, al completar tokens (`/sdd-cierre`), al configurar el interruptor (`/sdd-config`) o al pedir el reporte. Disparadores - "bitácora", "registrar actividad", "reporte de la bitácora", "tokens por modelo", "bitácora de uso", `/sdd-config`, `/sdd-spec`, `/sdd-cierre`.
---

# Skill: Bitácora de uso (beta)

Es beta, local por dev y no se versiona: todo vive en `.agents/local/`, fuera de Git. Esta skill es la
fuente única de sus reglas; los comandos y el contrato de tiempos remiten aquí y no las copian.

## Activación
- Interruptor global: sección `## Bitácora` de `.agents/local/configuracion.md`, con `encendida` o
  `apagada` (se cambia en `/sdd-config`; formato en `.agents/configuracion.ejemplo.md`).
- Sin la sección, o sin el archivo, vale `apagada`; no hay aviso extra.
- Decisión por spec: sí o no a registrar una spec concreta, guardada en su bitácora local (no en
  `05_avance.md`).
- `/sdd-spec`, al crear la carpeta o al trabajar una sin decisión de este dev: una sola pregunta antes
  de redactar. Sugerido sí si el interruptor está `encendida`; sugerido no si está `apagada` (solo
  registra si la persona elige sí).
- Otro comando sin decisión por spec de este dev (la abrió otro o es anterior a esta versión): aplica
  el interruptor global sin preguntar.
- Spec con bitácora activa: decisión sí, o sin decisión y el interruptor `encendida`.
- Cambiar la decisión rige desde ese momento y conserva las entradas ya escritas.
- Apagar el interruptor a mitad: las specs con decisión sí siguen; las que dependían del interruptor
  dejan de anotar. Encenderlo a media spec: registra desde ese tramo, sin reconstruir los anteriores.
- Otro dev que continúa la spec: rige su interruptor; cada dev tiene su propia bitácora de esa spec.

## Formato
- Un archivo Markdown por spec o actividad en `.agents/local/bitacora/`: `<carpeta>.md` (nombre de la
  carpeta de la spec) y `actividad-<etiqueta>.md`.
- La carpeta se crea con la primera entrada que se escribe.
- Encabezado: la decisión por spec con su fecha y hora, o, en una actividad, `estado: abierta` o
  `estado: cerrada`. Después, la tabla de entradas (una fila por tramo).
- Columnas: fecha y hora (el Inicio de su fila de Tiempos, o el inicio del turno en una actividad),
  carpeta, paso, rol, modelo, motivo, minutos, nota, entrada, salida, lectura de caché y escritura de
  caché, más «Sesión» (id de sesión de quien escribe) y «Subagente» (id del subagente en una entrada
  delegada; vacío si no lo es). Sin id que dé la herramienta, la columna lleva «no disponible». En una
  actividad, carpeta y paso van vacíos y la etiqueta va en el encabezado.
- Tipos de entrada: en el chat (tramo de la sesión principal), delegada (tramo o lote de un subagente
  lanzado en él) y de coordinación (mensajes de la sesión principal en la ventana de las delegadas;
  rol `chat`, nota «coordinación», minutos vacíos).
- Marcas en los tokens: «pendiente», «no disponible», «no disponible (sesiones paralelas)». Marca en
  la nota: «ventana con pausa».
- Archivo ilegible o con formato viejo: aviso en una línea y no se sobrescribe sin confirmar.

```markdown
# Bitácora: 0099_EjemploFeature
Decisión por spec: sí (2026-01-15 10:02)

| Fecha y hora | Carpeta | Paso | Rol | Modelo | Motivo | Min | Nota | Entrada | Salida | Caché leída | Caché escrita | Sesión | Subagente |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| 2026-01-15 10:40 | 0099_EjemploFeature | F1 | implementer | claude-sonnet-x | | 12 | cumple lint | 1.200 | 3.400 | 80.000 | 9.000 | s-0001 | a-01 |
| 2026-01-15 10:40 | 0099_EjemploFeature | F2 | implementer | claude-sonnet-x | | 15 | cumple lint | pendiente | pendiente | pendiente | pendiente | s-0001 | a-02 |
| 2026-01-15 10:40 | 0099_EjemploFeature | F1-F2 | chat | claude-opus-x | | | coordinación | pendiente | pendiente | pendiente | pendiente | s-0001 | |
```

## Registro por tramo
- Disparador: la sesión principal escribe una fila en Tiempos (formato y regla de lote en
  `.agents/skills/sdd/avance-y-tiempos.md` › «Tiempos reales»). En ese mismo momento agrega una entrada,
  en la misma escritura que la fila (el mismo comando o la misma edición), nunca en una llamada aparte:
  cada llamada vuelve a leer todo el contexto y multiplica el costo de la bitácora.
- Campos: fecha y hora, carpeta, paso, rol, modelo (el mismo dato de la Nota), motivo del lanzamiento
  si lo hubo (motivos en `.agents/skills/sdd/SKILL.md` › «Dónde corre cada rol»), minutos y una nota
  corta. Los cuatro tokens quedan «pendiente».
- Un lote sigue siendo una entrada. Fila con pausa mayor que 0: la nota lleva «ventana con pausa».
- Tramo o lote hecho por un subagente: entrada delegada con su rol, modelo y el id que devolvió el
  lanzamiento (la sesión lo anota al recibirlo), en la misma escritura que su fila. Un relanzamiento
  tiene otro id y su propia entrada. Sin id: «Subagente» en «no disponible».
- Una fila con trabajo en el chat y subagentes (por ejemplo, `validate` con `tester`) da una entrada
  en el chat y una delegada por subagente, todas con el paso de esa fila (ya no es «una fila, una
  entrada»).
- Con una o más entradas delegadas en una escritura, se agrega una sola entrada de coordinación para
  su ventana de delegación (de Inicio a Fin de esas filas), con rol `chat`, el modelo de la sesión,
  nota «coordinación» y minutos vacíos; con pausa, «ventana con pausa». No se escribe si esa misma
  escritura ya lleva una entrada en el chat con la misma ventana.
- Solo la sesión principal escribe. El subagente devuelve su modelo y no toca la bitácora.
- Con una actividad abierta, un comando `/sdd-*` de una spec con bitácora activa escribe en la de la
  spec, no en la actividad.
- Spec sin bitácora activa: no se escribe nada.

## Actividad
- Trabajo fuera de una carpeta de specs. Se abre solo si la persona la nombra con una etiqueta; no se
  registran solos los turnos del chat.
- Una entrada por turno (de su mensaje a la respuesta), con los mismos campos de «Registro por tramo»
  pero sin carpeta ni paso, y con la etiqueta. Anota «Sesión» y deja «Subagente» vacío; el turno
  incluye a sus subagentes (no se reparte entre roles).
- Termina cuando la persona la cierra, cambia de etiqueta o termina la sesión. Al abrir otra sesión,
  una actividad con `estado: abierta` de otra sesión cuenta como cerrada.
- Interruptor `apagada`: una línea que lo dice y cómo encenderlo (`/sdd-config`); no se registra.

## Fuente de tokens
Cada herramienta declara dónde dejan sus datos los tokens. Sin fila: «no disponible».

| Herramienta | Fuente | Id de sesión | Transcript del subagente |
|---|---|---|---|
| Claude Code | Transcripts locales `*.jsonl` en `$CLAUDE_CONFIG_DIR/projects/` (o `~/.claude/projects/` si esa variable no existe), en la carpeta del proyecto: su ruta absoluta con cada `/` cambiado por `-`. Cada sesión es un `*.jsonl`; los subagentes están en `<sesión>/subagents/*.jsonl`. | Variable `CLAUDE_CODE_SESSION_ID` (nombre del `<sesión>.jsonl`) | `<sesión>/subagents/agent-<id>.jsonl`, con el id que devuelve el lanzamiento |
| Cursor | «no disponible» | «no disponible» | «no disponible» |
| OpenCode | «no disponible» | «no disponible» | «no disponible» |
| Otra herramienta | «no disponible» | «no disponible» | «no disponible» |

## Completar tokens
- Cuándo: en cada corrida de `/sdd-cierre` sobre una spec con bitácora activa (en los tres caminos,
  aunque la corrida se detenga después) y al pedir el reporte.
- Ventana de cada entrada: de Inicio a Fin de su fila de Tiempos; en una actividad, del inicio del
  turno más sus minutos. En minutos completos y convertida a UTC con la zona del sistema.
- Se leen solo las líneas de tipo `assistant`. Qué se suma depende de la entrada:
  - Delegada: todo el transcript de su subagente, sin ventana (aunque siga tras el Fin de su fila).
    Si el mismo id está en más de una entrada (subagente reanudado), cada una suma solo los mensajes
    de ese transcript que caen en su ventana.
  - En el chat con id de sesión: los mensajes de esa sesión en su ventana, sin subagentes ni otras
    sesiones; no marca «no disponible (sesiones paralelas)».
  - De coordinación: los mensajes de su sesión en su ventana que ninguna entrada en el chat de la
    spec ya contó.
  - Cada `message.id` cuenta una sola vez en toda la bitácora de la spec (un mensaje con varios
    bloques repite su `usage`); si dos entradas lo alcanzan, gana la entrada en el chat sobre la de
    coordinación y, entre coordinaciones, la escrita primero.
  - Entrada sin las columnas «Sesión» y «Subagente» (anterior a esta versión), o en el chat o de
    coordinación con «Sesión» en «no disponible»: ventana sobre la sesión principal y los
    subagentes, con «no disponible (sesiones paralelas)» si hay más de una sesión (salvo que la
    persona elija la sesión); no se reescribe.
  - Actividad: la sesión anotada y sus subagentes en la ventana del turno; sin id de sesión, la regla
    anterior.
- Se suman `input_tokens`, `output_tokens`, `cache_read_input_tokens` y `cache_creation_input_tokens`;
  la caché va aparte de entrada y salida.
- Sin id de subagente, sin transcript por subagente o con el transcript del id ausente: «no
  disponible». Tramo que no se ubica en la fuente, o fuente ausente, borrada o rotada: «no
  disponible». Nunca se estima.
- Envoltorio de otra herramienta: suma solo su transcript; la nota dice «tokens del envoltorio».
- Fila con pausa: se suman los tokens de la ventana y la entrada ya lleva «ventana con pausa».
- Solo se leen `timestamp`, `message.id`, `message.model` y `message.usage`, y nombres de archivo e
  ids de sesión y de subagente. El comando de lectura es de un solo uso: no se guarda, no se versiona
  y no imprime contenido de mensajes.

## Reporte
- Se pide en lenguaje natural («reporte de la bitácora de 0099») para una spec, una actividad o un
  rango de fechas. No hay comando `/sdd-*` propio.
- Primero completa los tokens pendientes (ver «Completar tokens»).
- Sale en el chat, listo para copiar: las entradas por tramo, los totales de minutos y de tokens por
  modelo y por rol, y estos hallazgos, ni uno más. Los tokens de cada entrada se suman una vez, sin
  ajuste por ola. Las entradas de coordinación van dentro del rol `chat` y su total se muestra
  aparte; sus minutos (vacíos) no entran en los totales de minutos.
  - los 3 tramos con más tokens (la coordinación cuenta como tramo, marcada «coordinación»);
  - los tramos con relanzamiento (motivo «relanzamiento»);
  - los tramos cuyo modelo no es el de su rol según `.agents/skills/sdd/restricciones-de-rol.md` ›
    «Modelo por rol»: si el id del modelo contiene un alias de esa tabla (`opus`, `sonnet`, `haiku`),
    debe ser el de su rol; si no contiene ninguno, o el rol no tiene fila en esa tabla (por ejemplo,
    el chat de una actividad), «sin comparar»;
  - cuántos tramos quedaron con tokens «no disponible».
- En los totales, los tramos con «ventana con pausa» se suman aparte (no es un hallazgo más).
- Sin entradas: lo dice y no inventa datos.
- No escribe archivos versionados, no abre issues y no usa la red.

## Privacidad
- Se guardan solo métricas (tiempos, tokens, modelo, rol, ids de sesión y de subagente) y notas
  cortas.
- Se leen también nombres de archivo e ids, nunca contenido de mensajes.
- Nunca el texto de prompts ni de respuestas, credenciales, nombres de secretos ni datos personales.
- Todo queda en `.agents/local/bitacora/`, fuera de Git; el reporte no sale de la máquina salvo que la
  persona lo copie.
