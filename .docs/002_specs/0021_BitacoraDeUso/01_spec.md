# 01 — Spec: Bitácora de uso (beta)
Estado: cerrada
Ticket: sin ticket

## Contexto y objetivo
Hoy el kit mide tiempos por fase o paso (tabla Tiempos de `05_avance.md`, con el modelo y el motivo
del lanzamiento en la Nota desde `0020_RuteoDeModelos`), pero no guarda cuántos tokens gastó cada
tramo ni deja una vista por modelo y rol. Las mediciones que sí existen son piezas personales sueltas
(`turno_tokens.py`, `metricas_subagentes.py`, skill `reporte-sesiones`), fuera del kit.

Objetivo: que cada dev pueda llevar, si quiere, una **bitácora de uso** local: qué modelo hizo qué
tarea, cuánto tardó, cuántos tokens de entrada y salida gastó y qué notas dejó la corrida, por spec o
por actividad suelta. Con esa bitácora se levantan issues de mejora de rendimiento con datos reales,
empezando por medir el efecto del ruteo de 0020. Se publica como beta.

## Usuarios y roles
- **Dev** que usa el kit: activa o desactiva la bitácora, decide por spec y pide el reporte.
- **Sesión principal**: es la única que escribe la bitácora, igual que `05_avance.md`.
- **Mantenedor del kit**: recibe los reportes en forma de issue.

## Historias de usuario
- HU-1. Como dev, quiero activar la bitácora una vez en mi configuración para no decidirlo en cada
  sesión.
- HU-2. Como dev, quiero decidir al abrir cada spec si se registra, para no medir lo que no me
  interesa.
- HU-3. Como dev, quiero ver por tramo qué modelo y rol trabajó, con su tiempo y sus tokens, para
  saber dónde se va el costo.
- HU-4. Como dev, quiero registrar también actividades fuera del flujo `sdd`, con una etiqueta.
- HU-5. Como dev, quiero un resumen listo para pegar en un issue, sin el texto de mis prompts.

## Definiciones
- **Bitácora:** archivo local por dev con las entradas de una spec o de una actividad. Vive en
  `.agents/local/` y no se versiona.
- **Tramo:** cada fila que la sesión agrega a la tabla Tiempos de `05_avance.md` (fase, paso o lote).
  Fuera del flujo `sdd`, cada petición de la persona dentro de una actividad (RF-9).
- **Interruptor global:** valor de la configuración local del dev (`.agents/local/configuracion.md`)
  que dice si la bitácora está activa por omisión.
- **Decisión por spec:** sí o no a registrar una spec concreta. Se guarda en la bitácora local, no en
  `05_avance.md`.
- **Spec con bitácora activa:** spec cuya decisión por spec es sí o, sin decisión para ese dev, con
  el interruptor global encendido (RF-7).
- **Actividad:** trabajo fuera de una carpeta de `.docs/002_specs/`, identificado por una etiqueta
  corta que da la persona.

## Requisitos funcionales (EARS)

Configuración
- RF-1: EL SISTEMA guardará el interruptor global como un paso nuevo de `/sdd-config`, en su propia
  sección del archivo local y en la lista de pasos de `.agents/configuracion.ejemplo.md` › «Formato
  del archivo local». El valor sugerido es **apagado**.
- RF-2: CUANDO `/sdd-config` presente ese paso, EL SISTEMA dirá que la bitácora es beta, que es local
  y no se versiona, y qué datos guarda y cuáles no (RF-13).
- RF-3: CUANDO el archivo local no tenga la sección de la bitácora (instalación o actualización
  anterior), EL SISTEMA la tomará como apagada, sin aviso adicional al del paso faltante que ya da el
  chequeo de arranque.

Decisión por spec
- RF-4: CUANDO `/sdd-spec` cree una carpeta, o trabaje una existente sin decisión por spec de ese
  dev, y el interruptor esté encendido, EL SISTEMA hará una
  sola pregunta antes de redactar la spec: registrar en la bitácora, sí (sugerido) o no.
- RF-5: CUANDO se dé el mismo caso de RF-4 con el interruptor apagado, EL SISTEMA hará esa misma
  pregunta con no como sugerido, y solo registrará la spec si la persona elige sí.
- RF-6: EL SISTEMA guardará la decisión por spec en la bitácora local de ese dev. `05_avance.md` no
  la registra. CUANDO la persona pida cambiarla, EL SISTEMA aplicará la nueva desde ese momento y
  conservará las entradas ya escritas.
- RF-7: CUANDO un comando distinto de `/sdd-spec` trabaje una spec sin decisión por spec para ese dev
  (la abrió otro, o se abrió antes de esta versión), EL SISTEMA aplicará su interruptor global sin
  preguntar.

Registro por tramo
- RF-8: CUANDO la sesión principal agregue una fila a la tabla Tiempos de una spec con la bitácora
  activa, EL SISTEMA agregará en ese mismo momento una entrada con: fecha y hora, carpeta, paso, rol,
  modelo (el mismo dato de la Nota), motivo del lanzamiento si lo hubo, minutos y una nota corta. Los
  tokens (RF-12) quedan como pendientes.
- RF-9: CUANDO la persona pida registrar una actividad fuera del flujo `sdd` con una etiqueta y el
  interruptor esté encendido, EL SISTEMA anotará cada petición de la persona dentro de esa actividad
  (un turno: de su mensaje a la respuesta) con los mismos campos de RF-8, sin carpeta ni paso y con
  la etiqueta, hasta que la persona la cierre, cambie de etiqueta o
  termine la sesión.
- RF-10: CUANDO la persona pida registrar una actividad y el interruptor esté apagado, EL SISTEMA dirá
  en una línea que está apagado y cómo encenderlo (`/sdd-config`), y no registrará.
- RF-11: EL SISTEMA no escribirá la bitácora desde un subagente: el subagente devuelve su modelo y la
  sesión principal anota la entrada, como en `05_avance.md`.

Tokens
- RF-12: CUANDO corra `/sdd-cierre` sobre una spec con la bitácora activa, sea cual sea su resultado y
  en cada corrida, o la persona pida el reporte, EL SISTEMA completará los tokens pendientes de cada entrada (entrada, salida y, si la fuente los
  separa, lectura y escritura de caché) desde la fuente de la
  herramienta, que la skill declara por herramienta (en Claude Code, sus transcripts locales
  `.jsonl`). Si la herramienta no deja esa fuente, o el tramo no se puede ubicar en ella, anotará
  «no disponible» y no estimará.

Privacidad
- RF-13: EL SISTEMA guardará en la bitácora solo métricas y notas cortas: nunca el texto de los
  prompts ni de las respuestas, credenciales, nombres de secretos ni datos personales.

Reporte
- RF-14: CUANDO la persona pida el reporte de una spec, una actividad o un rango de fechas, EL
  SISTEMA mostrará en el chat un resumen listo para copiar: entradas por tramo, totales de minutos y
  tokens por modelo y por rol, y estos hallazgos (lista cerrada): los 3 tramos con más tokens, los
  tramos con relanzamiento, los tramos cuyo modelo no es el de su rol según
  `restricciones-de-rol.md` › «Modelo por rol», y cuántos tramos quedaron con tokens «no disponible».
- RF-15: EL SISTEMA no escribirá el reporte en archivos versionados, no abrirá issues y no usará la
  red.

Skill y beta
- RF-16: EL SISTEMA tendrá una skill nueva del kit, registrada en la tabla de skills de
  `template/AGENTS.md`, que es la fuente única del formato de la bitácora, de la fuente de tokens por
  herramienta, del registro por tramo y del reporte. Los comandos que la usan (`/sdd-config`,
  `/sdd-spec`, `/sdd-cierre`) remiten a ella y no copian sus reglas.
- RF-17: EL SISTEMA declarará la bitácora como beta en `README.md`, `INSTALAR.md`, el paso de
  `/sdd-config` y el changelog de la versión.

## Requisitos no funcionales
- Solo Markdown: no se versiona código ejecutable ni scripts (constitución, principio 5; `AGENTS.md`
  › Stack). La lectura de los transcripts la describe la skill como instrucciones.
- Local y por dev: todo lo que escribe la bitácora queda en `.agents/local/`, que ya está fuera de
  Git (`template/.gitignore`).
- Sin costo apreciable por tramo: una entrada corta por fila de Tiempos. No agrega lecturas al
  arranque de sesión (spec `0015_ArranqueLigero`).
- Kit genérico: sin nombres de proyecto ni de herramienta fuera de la tabla de fuentes de tokens.
- Cada regla en una sola fuente: la skill nueva. `avance-y-tiempos.md` sigue siendo la fuente de la
  tabla Tiempos y no se duplica.

## Casos límite
- La persona apaga el interruptor global a mitad: las specs con decisión sí siguen registrándose; las
  que dependían del interruptor (RF-7) dejan de anotar entradas nuevas. Las escritas se conservan.
- Se enciende la bitácora a media spec: se registra desde ese tramo; no se reconstruyen los
  anteriores.
- Otro dev continúa la spec: rige su interruptor global (RF-7); cada dev tiene su propia bitácora de
  esa spec.
- Varias sesiones paralelas sobre la misma spec, o lotes y olas paralelas: una entrada por fila de
  Tiempos, con la misma regla de lote de `avance-y-tiempos.md`.
- Tramo en una herramienta sin transcripts (Cursor, OpenCode): modelo y minutos sí; tokens
  «no disponible».
- Transcript borrado o rotado antes del reporte: «no disponible» en esos tramos.
- Reporte de una spec sin entradas: se dice que no hay datos y no se inventan.
- Actividad abierta y la persona corre un comando `/sdd-*` de una spec con bitácora activa: el tramo
  va a la bitácora de la spec, no a la actividad.
- No existe todavía la carpeta de bitácoras en `.agents/local/`: se crea al escribir la primera
  entrada.
- Bitácora local ilegible o con formato viejo: aviso en una línea; no se sobrescribe sin confirmar.

## Fuera de alcance
- Plan contra real en vivo en `05_avance.md`: spec `0022_PlanContraRealEnVivo`.
- Calcular costo en dinero (precios por modelo): la bitácora guarda tokens; el costo puede venir
  después.
- Abrir el issue de forma automática o enviar datos fuera de la máquina.
- Consolidar bitácoras de varios devs.
- Reconstruir tramos de specs ya cerradas.
- Que la bitácora sustituya a la tabla Tiempos.

## Criterios de finalización
- Los 17 RF validados por el reviewer, cada uno con su evidencia.
- Ensayo de instalación: la skill nueva y los archivos tocados se copian sin pisar nada.
- Lint de `AGENTS.md` › Comandos en cada archivo tocado y E20 con el manifiesto de la versión nueva.
- La palabra beta aparece en los cuatro lugares de RF-17.

## Dudas abiertas
Sin marcadores. Supuestos explícitos (la persona puede revertirlos al aprobar):
- S-1: el reporte se pide a la skill en lenguaje natural («reporte de la bitácora de 0021»). No hay
  un comando `/sdd-*` nuevo.
- S-2: una actividad fuera del flujo solo se registra si la persona la nombra con su etiqueta. No se
  registran solos todos los turnos del chat.
- S-3 (para revisión, alcance): los tokens de caché se guardan aparte de entrada y salida cuando la
  fuente los separa (RF-12), porque en este repo son la mayor parte del costo (`MEMORY.md`, 2026-11-22).
