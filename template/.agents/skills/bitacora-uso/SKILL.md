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
  caché. En una actividad, carpeta y paso van vacíos y la etiqueta va en el encabezado.
- Marcas en los tokens: «pendiente», «no disponible», «no disponible (sesiones paralelas)». Marca en
  la nota: «ventana con pausa».
- Archivo ilegible o con formato viejo: aviso en una línea y no se sobrescribe sin confirmar.

```markdown
# Bitácora: 0099_EjemploFeature
Decisión por spec: sí (2026-01-15 10:02)

| Fecha y hora | Carpeta | Paso | Rol | Modelo | Motivo | Min | Nota | Entrada | Salida | Caché leída | Caché escrita |
|---|---|---|---|---|---|---|---|---|---|---|---|
| 2026-01-15 10:40 | 0099_EjemploFeature | F1 | implementer | claude-sonnet-x | | 12 | cumple lint | 1.200 | 3.400 | 80.000 | 9.000 |
| 2026-01-15 11:05 | 0099_EjemploFeature | F2 | reviewer | claude-opus-x | relanzamiento | 8 | ventana con pausa | pendiente | pendiente | pendiente | pendiente |
```

## Registro por tramo
- Disparador: la sesión principal escribe una fila en Tiempos (formato y regla de lote en
  `.agents/skills/sdd/avance-y-tiempos.md` › «Tiempos reales»). En ese mismo momento agrega una entrada,
  en la misma escritura que la fila (el mismo comando o la misma edición), nunca en una llamada aparte:
  cada llamada vuelve a leer todo el contexto y multiplica el costo de la bitácora.
- Campos: fecha y hora, carpeta, paso, rol, modelo (el mismo dato de la Nota), motivo del lanzamiento
  si lo hubo (motivos en `.agents/skills/sdd/SKILL.md` › «Dónde corre cada rol»), minutos y una nota
  corta. Los cuatro tokens quedan «pendiente».
- Un lote es una entrada, como es una fila en Tiempos. Fila con pausa mayor que 0: la nota lleva
  «ventana con pausa».
- Solo la sesión principal escribe. El subagente devuelve su modelo y no toca la bitácora.
- Con una actividad abierta, un comando `/sdd-*` de una spec con bitácora activa escribe en la de la
  spec, no en la actividad.
- Spec sin bitácora activa: no se escribe nada.

## Actividad
- Trabajo fuera de una carpeta de specs. Se abre solo si la persona la nombra con una etiqueta; no se
  registran solos los turnos del chat.
- Una entrada por turno (de su mensaje a la respuesta), con los mismos campos de «Registro por tramo»
  pero sin carpeta ni paso, y con la etiqueta.
- Termina cuando la persona la cierra, cambia de etiqueta o termina la sesión. Al abrir otra sesión,
  una actividad con `estado: abierta` de otra sesión cuenta como cerrada.
- Interruptor `apagada`: una línea que lo dice y cómo encenderlo (`/sdd-config`); no se registra.

## Fuente de tokens
Cada herramienta declara dónde dejan sus datos los tokens. Sin fila: «no disponible».

| Herramienta | Fuente |
|---|---|
| Claude Code | Transcripts locales `*.jsonl` en `$CLAUDE_CONFIG_DIR/projects/` (o `~/.claude/projects/` si esa variable no existe), en la carpeta del proyecto: su ruta absoluta con cada `/` cambiado por `-`. Cada sesión es un `*.jsonl`; los subagentes están en `<sesión>/subagents/*.jsonl`. |
| Cursor | «no disponible» |
| OpenCode | «no disponible» |
| Otra herramienta | «no disponible» |

## Completar tokens
- Cuándo: en cada corrida de `/sdd-cierre` sobre una spec con bitácora activa (en los tres caminos,
  aunque la corrida se detenga después) y al pedir el reporte.
- Ventana de cada entrada: de Inicio a Fin de su fila de Tiempos; en una actividad, del inicio del
  turno más sus minutos. En minutos completos y convertida a UTC con la zona del sistema.
- Se leen solo las líneas de tipo `assistant` cuyo `timestamp` cae en la ventana, de la sesión
  principal y de los `subagents/*.jsonl`. Cada `message.id` cuenta una vez (un mensaje con varios
  bloques repite su `usage`).
- Se suman `input_tokens`, `output_tokens`, `cache_read_input_tokens` y `cache_creation_input_tokens`;
  la caché va aparte de entrada y salida.
- Más de una sesión del proyecto con mensajes en la ventana: «no disponible (sesiones paralelas)»,
  salvo que la persona elija la sesión.
- Tramo que no se ubica en la fuente, o fuente ausente, borrada o rotada: «no disponible». Nunca se
  estima.
- Fila con pausa: se suman los tokens de la ventana y la entrada ya lleva «ventana con pausa».
- Solo se leen `timestamp`, `message.id`, `message.model` y `message.usage`. El comando de lectura es de
  un solo uso: no se guarda, no se versiona y no imprime contenido de mensajes.

## Reporte
- Se pide en lenguaje natural («reporte de la bitácora de 0099») para una spec, una actividad o un
  rango de fechas. No hay comando `/sdd-*` propio.
- Primero completa los tokens pendientes (ver «Completar tokens»).
- Sale en el chat, listo para copiar: las entradas por tramo, los totales de minutos y de tokens por
  modelo y por rol, y estos hallazgos, ni uno más:
  - los 3 tramos con más tokens;
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
- Se guardan solo métricas (tiempos, tokens, modelo, rol) y notas cortas.
- Nunca el texto de prompts ni de respuestas, credenciales, nombres de secretos ni datos personales.
- Todo queda en `.agents/local/bitacora/`, fuera de Git; el reporte no sale de la máquina salvo que la
  persona lo copie.
