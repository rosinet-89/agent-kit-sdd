# 01 — Spec: Pausa, origen del kit y contexto bajo demanda
Estado: cerrada
Ticket: issues #4, #6, #7, #8, #10 y #11 de GitHub; deudas 0009, 0010, 0011, 0012, 0013 y 0014

## Contexto y objetivo
Tras 0.12.0 (`0015_ArranqueLigero`) quedan seis issues abiertos y seis deudas que tocan las mismas
piezas: `AGENTS.md`, `/sdd-init`, `/sdd-status`, `/sdd-cierre`, `validar-estructura` e
`INSTALAR.md`/`ACTUALIZAR.md`. La persona decidió atenderlos en una sola spec y una sola versión del
kit. El issue #9 queda fuera: se cerró como «no se hará» (ver «Fuera de alcance»).

Hechos medidos el 2026-11-22 (bytes, `LC_ALL=C wc -c`):
- `template/AGENTS.md` pesa 11 061 bytes y la raíz, 12 267. «Guardrails» pesa 3 048 bytes y su
  regla 1 sola, 2 640. «Antecedentes» pesa 893 y «Deuda técnica», 697 (697 en la plantilla).
- El paso 8 de «Cada sesión» revisa prefijos repetidos recorriendo las carpetas de `.docs/`, aunque
  cada índice ya trae la lista (#4).
- Para dejar el punto de retomo al parar, hoy el agente improvisa, y nada comprueba al volver que el
  estado siga siendo el anotado (#11). Pasó en esta misma sesión.
- El proyecto sabe qué versión del kit tiene (`.agents/.kit-version`), pero no de dónde viene, y no
  puede responder «¿tengo la última versión?» sin preguntar la URL (#6).
- El kit no tiene un lugar para el contexto del proyecto que se lea bajo demanda (#10), ni recomienda
  skills a partir de señales del proyecto (#8).

Objetivo, por bloques. Cada uno se puede verificar por separado:
- **A. Pausa y retomo** (#11): comando `/sdd-pausa` y comprobación al volver en `/sdd-status`.
- **B. Origen del kit y versión** (#6): registrar URL y rama, responder «¿tengo la última versión?»
  y cerrar `ACTUALIZAR.md` con commits separados.
- **C. `AGENTS.md` más ligero** (#7 pendiente, deudas 0009 y 0014): sacar «Antecedentes» y «Deuda
  técnica» a lectura bajo demanda, condensar la regla 1 de Guardrails sin perder nada, definir el
  cambio pequeño y orientar mejor a `/sdd-init` en formato y Build.
- **D. Arranque solo con índices** (#4 pendiente): paso 8 sobre las tablas de índice y bandera
  `.agents/local/estructura-ok`.
- **E. Contexto del proyecto** (#10): sección `.docs/005_contexto/`.
- **F. Skills y permisos según el stack** (#8).
- **G. Estatus de fase y pulido** (deudas 0011, 0012 y 0013).
- **H. Chequeo del marcador en la raíz del kit** (deuda 0010).

Alcance de capas: como en 0015, se cubren la raíz (entorno de mantenimiento) y `template/`, porque
«Cada sesión», las secciones que leen los roles y los chequeos son el mismo texto en las dos capas.
El `.agents/` de la raíz es la réplica local de `template/.agents/`.

## Usuarios y roles
- **Persona que usa el kit en un proyecto:** para, retoma, instala, actualiza y pregunta por la
  versión.
- **Persona que mantiene el kit:** trabaja en la raíz del clon.
- **Sesión principal y roles SDD** (`coordinator`, `planner`, `implementer`, `reviewer`, `tester`,
  `explore`): leen `AGENTS.md` y las secciones que nombra su definición. Los que declaran
  `omitClaudeMd` dependen de leer las secciones por su nombre.

## Historias de usuario
- HU-1. Como persona que para a media spec, quiero un comando que deje el punto de retomo completo,
  con lo que está corriendo y la hora de la pausa, para que quien retome no tenga que reconstruirlo.
- HU-2. Como persona que vuelve, quiero que `/sdd-status` compruebe lo que quedó anotado al parar,
  para saber si algo cambió.
- HU-3. Como persona de un proyecto, quiero preguntar «¿tengo la última versión del kit?» y recibir
  una respuesta sin dar la URL, para decidir si actualizo.
- HU-4. Como persona que actualiza, quiero que el cierre de la actualización proponga commits
  separados por tipo, para no mezclar el kit con el avance de mis specs.
- HU-5. Como sesión principal, quiero que `AGENTS.md` cargue solo lo que se usa en cada turno, sin
  perder ninguna regla de los Guardrails, para gastar menos contexto.
- HU-6. Como persona que pide un arreglo chico, quiero una definición clara de cambio pequeño que lo
  lleve siempre al modo ligero, para no pagar una spec completa por un error tipográfico ni saltarme
  el flujo.
- HU-7. Como sesión principal, quiero revisar prefijos repetidos leyendo solo las tablas de índice y
  saber si la estructura ya se validó para esta versión, para no recorrer carpetas en cada sesión.
- HU-8. Como persona de un proyecto, quiero un lugar numerado e indexado para el contexto del
  proyecto que se lea bajo demanda, con fecha de verificación, para no engordar `AGENTS.md` ni dejar
  documentos viejos sin aviso.
- HU-9. Como persona que instala o actualiza, quiero que el kit me recomiende skills y permisos de
  solo lectura a partir de lo que detecta en el proyecto, para no descubrirlos a mano.
- HU-10. Como persona que consulta el índice, quiero que la celda de una spec refleje su fase real,
  para que «qué está pendiente» no muestre etapas viejas.

## Definiciones
- **Carpeta activa:** la carpeta SDD que nombra la persona o, si no la nombra, la única carpeta con la
  que se trabajó en la sesión. Con ninguna o con varias, no hay carpeta activa y se pregunta.
- **Pausa:** la persona pide parar el trabajo de una carpeta SDD. No es la pausa de la tabla de
  tiempos, aunque la alimenta.
- **Lista «al retomar»:** comprobaciones de solo lectura y siguiente acción que `/sdd-pausa` deja en
  el punto de retomo.
- **Trabajo en segundo plano:** subagente o proceso lanzado en la sesión que sigue vivo al parar.
- **Origen del kit:** URL del repositorio y rama desde los que se instaló o actualizó el kit.
- **Versión publicada:** la línea de `VersionAgentKitSdd` en la punta de la rama del origen.
- **Clon temporal:** `git clone --depth 1` en un directorio de `mktemp -d`, fuera del proyecto, que se
  borra al terminar.
- **Cambio pequeño:** bug acotado, error tipográfico o de tipos, que toca a lo sumo 3 archivos y no
  añade una regla. Va siempre por el modo ligero, con carpeta. No hay camino fuera del flujo.
- **Bandera de estructura:** `.agents/local/estructura-ok`, archivo local no versionado con
  `alineada`, `version` y `fecha`.
- **Documento de contexto:** archivo numerado de `.docs/005_contexto/` que responde una pregunta del
  proyecto (negocio, estructura, procesos o integraciones) y se lee bajo demanda.
- **Catálogo de señales:** tabla genérica del kit con señal detectada, tipo de skill sugerida y
  motivo. No nombra proyectos ni fuentes externas.

## Requisitos funcionales (EARS)

### A. Pausa y retomo (#11)
- RF-1: CUANDO la persona corre `/sdd-pausa [motivo]`, EL SISTEMA toma la hora e identifica la
  carpeta activa (ver «Definiciones»). Si no la hay, pregunta cuál y no supone.
- RF-2: CUANDO `/sdd-pausa` corre y hay trabajo en segundo plano, EL SISTEMA lo lista. De cada uno da
  hora de inicio, dónde está su recibo o estado, cómo comprobarlo y cómo cancelarlo. Pregunta qué
  hacer con cada uno y no detiene nada sin confirmación. El dato lo pide a la herramienta, sin
  suponer un mecanismo concreto. Si la herramienta no lo expone, pregunta a la persona y anota
  «inventario sin dato de la herramienta».
- RF-3: CUANDO `/sdd-pausa` termina, EL SISTEMA deja en el PUNTO DE RETOMO de `05_avance.md`:
  - fase, último paso cerrado, bloqueos y código;
  - la hora de inicio de la pausa;
  - una lista «al retomar» (qué comprobar primero, qué hacer según el resultado y el comando
    `/sdd-*` que sigue);
  - una decisión con hora, «la persona pide parar», con el motivo si lo dio.

  La pausa no suma tiempo. La forma de la marca vive en `avance-y-tiempos.md`.
- RF-4: CUANDO `/sdd-pausa` termina, EL SISTEMA resume `git status` sin hacer commit (sin Git, lo
  omite y lo dice). Si la persona
  lo pide, también deja los pendientes que no son de la carpeta en un archivo local no versionado de
  `.agents/local/`. Solo escribe `05_avance.md` y ese archivo.
- RF-5: CUANDO `/sdd-status` encuentra una lista «al retomar», EL SISTEMA ejecuta solo las
  comprobaciones de una lista cerrada de solo lectura: `git status`, existencia de un archivo o recibo,
  y estado de un trabajo en segundo plano consultado a la herramienta. Informa qué cambió desde la
  pausa. Una comprobación fuera de esa lista la muestra a la persona y no la ejecuta. Sigue sin
  modificar archivos. Con la hora de
  la pausa, el hueco largo de `avance-y-tiempos.md` se pregunta desde ese punto.

### B. Origen del kit y versión (#6)
- RF-6: CUANDO se instala el kit (`INSTALAR.md`), EL SISTEMA escribe `.agents/.kit-origen` con `url`
  y `rama`, como hace hoy con `.agents/.kit-version`. CUANDO se actualiza (`ACTUALIZAR.md`), EL
  SISTEMA compara `url` y `rama` del archivo con las del origen desde el que se actualiza. Si falta,
  lo crea; si difieren, propone el cambio y lo escribe con visto bueno. La `url` es la del remoto
  `origin` del clon desde el que se copia, y la `rama`, su rama actual. Si ese clon no tiene remoto,
  se pide la URL a la persona. Si la persona tiene un clon local del kit, su ruta va en
  `.agents/local/kit-origen`, que no se versiona y que el kit no pide si no hay clon.
- RF-7: CUANDO la persona pregunta si tiene la última versión, EL SISTEMA sigue un procedimiento fijo:
  1. Lee la versión instalada y el origen. Si falta el origen, lo dice, pide la URL y propone
     guardarla.
  2. Obtiene la punta remota con `git ls-remote`.
  3. Lee la versión publicada y el changelog de un clon temporal, o del clon local si su `HEAD`
     coincide con la punta remota.
  4. Responde una de tres cosas: al día; hay versión nueva, con las entradas del changelog entre
     ambas; o el clon local está atrasado.

  Para actualizar remite a `ACTUALIZAR.md` y no aplica nada. Las consultas corren sin interacción y
  sin credenciales. Si el remoto las pide, para, lo dice y sugiere usar el clon local de la persona o
  que la persona corra la consulta (Guardrails: no se pasan credenciales). En el repositorio del kit,
  el procedimiento no aplica: lo dice en una línea, porque el clon ya es el kit.
- RF-8: EL SISTEMA declara, en la regla Git de las dos capas de `AGENTS.md`, que el procedimiento de
  RF-7 es la única excepción de red. Se limita a `git ls-remote` y al clon temporal, que nunca toca el
  repositorio del proyecto ni el clon local y se borra al terminar. `push`, `pull`, `fetch` y `sync`
  siguen prohibidos.
- RF-9: CUANDO `ACTUALIZAR.md` llega al commit propuesto, EL SISTEMA propone commits separados, cada
  uno con su confirmación y con la skill `git-commit`. Un grupo vacío no se propone. Nunca usa
  `git add .` ni `-A`. Los grupos son:
  1. Kit: archivos del kit añadidos o reemplazados, `.agents/.kit-version` y `.agents/.kit-origen`.
  2. Configuración del proyecto que la actualización tocó: los del Grupo B (`AGENTS.md`, `.gitignore`)
     y los permisos versionados, si los hay.
  3. Lo propio del proyecto que ya estaba sin guardar (por ejemplo, el avance de una spec), solo si la
     persona pide incluirlo.
- RF-10: CUANDO `/sdd-estructura` corre en un proyecto instalado, EL SISTEMA avisa (no falla) si falta
  `.agents/.kit-origen` o si está mal formado. No usa la red.

### C. `AGENTS.md` más ligero (#7, deudas 0009 y 0014)
- RF-11: EL SISTEMA saca de `AGENTS.md`, en las dos capas, el cuerpo de «Antecedentes» y de «Deuda
  técnica» a una sola fuente de lectura bajo demanda. En `AGENTS.md` deja, para cada una, la
  condición de cuándo se lee y su remisión.
  - El paso 6 de «Cada sesión» y las remisiones de los roles, comandos y chequeos que las citan siguen
    resolviendo por nombre.
  - Los roles con `omitClaudeMd` encuentran el mismo texto.
- RF-12: EL SISTEMA condensa la regla 1 de Guardrails, en las dos capas e idéntica en ambas, a 1 900
  bytes o menos.
  - Conserva la lista cerrada de permitidos, los ejemplos de prohibidos, la precedencia, la escritura
    indirecta, el trato de los Comandos, la entrega del script con rollback, quién escribe en `sql/`,
    la consulta de datos y la excepción de QA de `tester` con su modo seguro.
  - Cada una de esas piezas se puede señalar en el texto nuevo. La sección sigue siendo la que leen
    todos los roles.
- RF-13: EL SISTEMA define el cambio pequeño (ver «Definiciones») en una sola fuente: siempre va por
  el modo ligero, con carpeta y con «Antecedentes». La frase de `coordinator.md:16` («fix chico que no
  merece carpeta… fuera de este flujo») deja de ofrecer un camino fuera del flujo y remite a esa
  definición. Con duda, el cambio no es pequeño y se evalúa con las condiciones del modo ligero.
- RF-14: CUANDO `/sdd-init` redacta «Comandos» y «Stack y convenciones de código», EL SISTEMA orienta
  dos cosas:
  - Formateo y análisis se acotan a los archivos tocados, o se quedan en solo verificar, y no
    reformatean archivos legados enteros.
  - En la propuesta sigue la marca `(por confirmar)` del paso 3. En lo que se escribe en
    `AGENTS.md`, un valor sin confirmar dice quién lo confirma y cuándo, y no queda como
    «(por confirmar)» indefinido.
- RF-15: CUANDO `/sdd-init` entrevista sobre la consulta de datos, EL SISTEMA pregunta si existe un
  usuario de base de datos de solo lectura y deja constancia en `AGENTS.md` › Comandos (deuda 0009,
  punto 1). El fragmento opcional `permissions.deny` de `restricciones-de-rol.md` suma los clientes
  de línea de comandos de base de datos `mysql`, `psql`, `sqlcmd`, `sqlplus`, `sqlite3`, `mongosh` y
  `redis-cli`, y sigue siendo solo una sugerencia (deuda 0009, punto 2).
- RF-16: CUANDO el chequeo de arranque revisa una carpeta de agentes por archivo, EL SISTEMA comprueba
  solo que cada rol tenga su entrada (enlace o adaptador presente). La validez del adaptador la deja a
  `/sdd-enlaces` y a `/sdd-estructura` (E3, E23), que conservan la definición completa de `OK` de
  «Estados de enlace». «Chequeo de arranque» declara esa lectura reducida, y su «Qué lee» no depende
  de secciones que no lee (deuda 0014, opción a).

### D. Arranque solo con índices (#4)
- RF-17: CUANDO corre el paso 8 de «Cada sesión», EL SISTEMA revisa prefijos repetidos leyendo solo
  las tablas de índice de `.docs/002_specs/README.md`, `.docs/003_deuda/README.md`,
  `.docs/004_adr/README.md` y, si existe, `.docs/005_contexto/README.md`. Hay repetido solo si dos
  filas del mismo índice comparten el entero del prefijo. No lista carpetas ni abre planes, deudas, ADR
  ni documentos de contexto. `git ls-tree` contra la rama remota se queda para asignar un número y para
  el commit. Una carpeta o archivo sin fila deja de verse en el arranque: lo detecta E13 en
  `/sdd-estructura`, bajo demanda (supuesto 8).
- RF-18: CUANDO `/sdd-estructura` termina, EL SISTEMA escribe `.agents/local/estructura-ok`
  (UTF-8 sin BOM) con:
  - `alineada: si` si no hubo `FALLA` (un `AVISO` no la baja), o `no` si hubo alguna;
  - `version` y `fecha`.

  Antes comprueba con `git check-ignore` que el archivo está ignorado; si no lo está, no lo escribe y
  avisa. Nunca `git add`, ni con `-f`. Es la única escritura de `/sdd-estructura`, y su descripción
  y las remisiones que dicen «solo lee y reporta» pasan a decir que lee, reporta y deja esa bandera
  local. En el repositorio del kit, que no tiene `.kit-version`, `version` es la de
  `VersionAgentKitSdd`.
- RF-19: CUANDO la persona pregunta si la estructura está alineada, EL SISTEMA lee la bandera.
  Vigente es `alineada: si` con `version` igual a `.agents/.kit-version` (en el repositorio del kit,
  igual a `VersionAgentKitSdd`). Si falta, está corrupta o es
  de otra versión, responde «sin bandera vigente» y no corre el checklist por su cuenta. El arranque
  no la lee ni la menciona, y el aviso del arranque sigue recomendando solo `/sdd-enlaces`.

### E. Contexto del proyecto (#10)
- RF-20: EL SISTEMA entrega `.docs/005_contexto/README.md` como índice vacío con guía. Cada fila lleva
  archivo, qué responde, cuándo leerlo y fecha de última verificación.
  - Los documentos usan la numeración de «Numeración única» y abren con «Objetivo» y «Última
    verificación». Los tipos de documento son una guía abierta, sin atarse a un lenguaje.
  - Las reglas genéricas de un framework van a una skill de dominio, no aquí.
  - Lo histórico va a `.docs/legacy/`, fuera del índice.
- RF-21: CUANDO `/sdd-init` arma su propuesta, EL SISTEMA propone la lista de documentos de contexto
  (nombre, qué responde, cuándo leerlo), como hace con las skills candidatas. Se crean después, uno
  por uno y con visto bueno. `AGENTS.md` lleva una línea con la remisión al índice.
- RF-22: CUANDO `/sdd-cierre` cierra un requerimiento que tocó algo descrito por un documento del
  índice de contexto, EL SISTEMA propone actualizarlo y su fecha. No escribe sin visto bueno. Sin
  `.docs/005_contexto/`, no propone nada.
- RF-23: CUANDO `/sdd-estructura` revisa `.docs/005_contexto/`, EL SISTEMA avisa (no falla) por un
  documento sin fila, una fila sin documento, o un documento sin «Última verificación» o con una fecha
  de más de 180 días (supuesto 9).

### F. Skills y permisos según el stack (#8)
- RF-24: CUANDO `/sdd-init` (paso de skills candidatas) o el final de `ACTUALIZAR.md` lo indican, o la
  persona lo pide, EL SISTEMA detecta el stack por señales. Lee manifiestos, lockfiles, configuración
  de build, esquemas, infraestructura y CI que existan, sin una lista fija de lenguajes. No abre
  `.env` ni archivos de credenciales. Si no reconoce nada, lo dice y pregunta.
- RF-25: EL SISTEMA cruza las señales con el catálogo de señales y con las skills instaladas en
  `.agents/skills/`. Entrega una tabla con skill, motivo (señal), estado (ya instalada, sugerida,
  candidata nueva o ya no aplica) y cómo crearla. No instala, no borra y no nombra fuentes externas.
  Una skill traída de fuera se revisa antes de usarla: sin scripts ejecutables ni secretos.
- RF-26: CUANDO `/sdd-init` muestra el fragmento `permissions.allow`, EL SISTEMA lo puede llenar con
  los comandos de verificación de solo lectura de `AGENTS.md` › Comandos del proyecto (formato,
  análisis, pruebas que no escriben en una base de datos), sin inventar comandos. Sigue siendo texto
  que solo se muestra. Esto sustituye la regla vigente del paso 7 de `sdd-init.md` («el allow queda
  genérico… no traigas comandos de este kit ni de ningún proyecto»): sigue sin traer comandos de este
  kit ni de otro proyecto, y si «Comandos» no tiene verificación de solo lectura, el allow queda
  genérico como hoy.

### G. Estatus de fase y pulido (deudas 0011, 0012 y 0013)
- RF-27: EL SISTEMA trata `/sdd-constitucion` como comando auxiliar: tiene fila de ruteo en el
  `coordinator` (como `sdd-config.md`) y no tiene fila en la tabla de fases de la skill `sdd`, porque no
  pertenece a una carpeta de spec. E10 lo exenta de la tabla de fases y deja de avisar por esa causa.
- RF-28: CUANDO la sesión principal registra en `05_avance.md` el cierre de `plan`, del primer paso
  `Fn.m` o del último paso de implementación, EL SISTEMA escribe en la celda del índice `Planificado`,
  `En curso` o `En pruebas`, respectivamente, con la forma de la leyenda. La regla de quién escribe cada palabra vive solo en la línea «Quién escribe» de
  la leyenda, y los comandos remiten. E22 sigue avisando de un desfase.
- RF-29: EL SISTEMA atiende los cuatro puntos de la deuda 0013:
  - `sdd-cierre.md` remite a la leyenda y al README de deuda en los tramos duplicados, sin perder lo
    que exigen RF-5 y RF-12 de la 0012;
  - la tensión entre «Transiciones» y el supuesto 12 de la spec 0012 se resuelve en la leyenda de
    estatus («Quién escribe» incluye la reapertura de un `Descartado` por el cierre). La spec 0012,
    ya `cerrada`, no se edita: su deuda remite a la leyenda;
  - se reescribe la frase del punto 3;
  - E13 remite a la leyenda para el desfase.

### H. Chequeo del marcador en la raíz del kit (deuda 0010)
- RF-30: CUANDO `/sdd-estructura` corre en el repositorio del kit (el que tiene `template/` y no
  tiene `.agents/.kit-version`), EL SISTEMA cuenta el marcador de instalación pendiente en el
  `AGENTS.md` de la raíz, compara su cadena con la de `template/AGENTS.md` e informa. La raíz no gana
  `.kit-version`.

### Publicación
- RF-31: EL SISTEMA publica la versión 0.13.0 con `VersionAgentKitSdd`, una entrada en
  `ChangelogAgentKitSdd.md`, `manifiestos/0.13.0.md` y, en `ACTUALIZAR.md`, lo que un proyecto con
  `AGENTS.md` y `.gitignore` propios (Grupo B) incorpora a mano.

## Requisitos no funcionales
- RNF-1 (medible): `template/AGENTS.md` ≤ 9 800 bytes (−11 % desde 11 061) y `AGENTS.md` de la raíz
  ≤ 11 000 (−10 % desde 12 267), medidos con `LC_ALL=C wc -c` tras la última fase. Cuenta estimada:
  salen ≈ 2 300 bytes («Antecedentes», «Deuda técnica» y lo que se condensa de la regla 1) y entran
  ≈ 800 (remisiones, cambio pequeño, excepción de red y línea de contexto). La lectura del arranque
  del escenario base de CP-10 de la 0015 no sube de 12 110 bytes.
- RNF-2: una sola fuente por regla. La leyenda de estatus, la definición de deuda, el contrato de
  tiempos y la numeración siguen en su archivo, y lo demás remite (ADR 0024, `MEMORY.md`).
- RNF-3: solo Markdown, sin scripts versionados (ADR 004) y sin hooks (ADR 011). Los comandos nuevos
  van en `template/.agents/commands/` con su frontmatter.
- RNF-4: nada propio de un proyecto entra a `template/`. El catálogo de señales y la guía de contexto
  son genéricos.
- RNF-5: instalar sigue copiando sin pisar, y actualizar solo reemplaza KIT-VIEJO confirmado (ADR 007).
  Los archivos nuevos se añaden al manifiesto.
- RNF-6: lo que cambia una regla global o abre una sección de `.docs/` se registra como ADR. Se
  esperan al menos la sección `005_contexto`, la excepción de red de RF-8 y la salida de
  «Antecedentes» y «Deuda técnica» de `AGENTS.md`.
- RNF-7: ningún cambio relaja la regla 1 de Guardrails ni la regla Git, salvo la excepción acotada de
  RF-8.
- RNF-8: `diff -rq template/.agents .agents` sin diferencias nuevas fuera de las ya aceptadas en
  `MEMORY.md`.

## Casos límite
- `/sdd-pausa` sin carpeta activa ni candidata: lo dice y no escribe.
- `/sdd-pausa` con un subagente que no responde al pedir su estado: se anota «estado desconocido» con
  cómo comprobarlo, y no se cancela.
- Dos `/sdd-pausa` seguidos: el segundo actualiza la misma marca y la misma lista, sin duplicarlas.
- `/sdd-status` con una lista «al retomar» que nombra un recibo que ya no existe: lo informa como
  cambio y no falla.
- Sin red, o la URL del origen no responde: RF-7 lo dice, no reintenta y no da una versión supuesta.
- La rama del origen ya no existe en el remoto: lo informa y pide la rama.
- Proyecto instalado antes de 0.13.0, sin `.kit-origen`: RF-10 avisa y `ACTUALIZAR.md` lo crea.
- La bandera existe pero `.agents/local/` no está ignorado: no se escribe y se avisa (RF-18).
- Un `AGENTS.md` viejo (0.12.0) con `.agents/` nuevo: sus pasos 6 y 8 siguen resolviendo por nombre,
  como en CP-08 de la 0015.
- Dos filas de índices distintos con el mismo entero: no es repetido (RF-17).
- Un proyecto sin `.docs/005_contexto/` (instalado antes): `/sdd-estructura` no falla.
- Señales contradictorias del stack (dos lenguajes): se informan las dos y la persona decide.

## Fuera de alcance
- Issue #9 (segundo agente con relevo por cuota): cerrado como «no se hará» el 2026-11-22. El
  envoltorio exige un script (choca con el ADR 004) y lo útil ya está en `0014_ConfiguracionGuiada`.
- Hooks, scripts o permisos que el kit aplique por su cuenta: solo se sugieren como texto.
- Instalar skills o consultar repositorios externos de skills.
- Que `/sdd-status` escriba algo o registre tiempo.
- Convertir la raíz del kit en proyecto instalado (`.kit-version` en la raíz).
- Generar documentos de contexto en `/sdd-init` sin visto bueno.

## Criterios de finalización
- RF-1 a RF-31 validados uno por uno, con evidencia en `07_casos_prueba.md`.
- RNF-1 medido y cumplido, y RNF-8 sin diferencias nuevas.
- Chequeos de `AGENTS.md` › Comandos en cada archivo tocado, y ensayo de instalación sin pisar.
- `/sdd-estructura` en el clon y en un ensayo de instalación sin `FALLA` nueva frente a la línea base.
- ADR de RNF-6 registrados con su fila de índice.
- Al cerrar: las deudas 0009 a 0014 pasan a `Resuelta` con «Cómo se atendió». Si alguna queda
  parcial, sigue `Abierta` y se dice qué falta. La sesión propone cerrar los issues #4, #6, #7, #8,
  #10 y #11 con un comentario de qué los resolvió, y los cierra solo con confirmación de la persona,
  después del push que hace la persona.

## Supuestos (la persona los puede revertir al aprobar)
1. La lista «al retomar» la ejecuta `/sdd-status`, no el `coordinator`, y no hay alias
   `/sdd-retomar` (#11, por decidir del issue).
2. Los pendientes que no son de una carpeta van a `.agents/local/retomo-sesion.md` (#11).
3. La marca de pausa es una línea del PUNTO DE RETOMO, no una fila de tiempos.
4. La consulta de versión es un comando nuevo de solo lectura, `/sdd-version`, enlazado desde
   `ACTUALIZAR.md`, y no una sección suelta.
5. La fuente única de «Antecedentes» y «Deuda técnica» la elige el plan entre la skill `sdd` y los
   README de `.docs/002_specs/` y `.docs/003_deuda/`.
6. El catálogo de señales vive en `template/`, porque es genérico, y el resultado de la detección se
   queda en el proyecto.
7. La raíz del kit también tiene `.docs/005_contexto/README.md` con el índice vacío, para que los
   chequeos se comporten igual en las dos capas.
8. (marcado para revisión) El arranque deja de detectar una carpeta o archivo sin fila de índice con
   prefijo repetido; queda a E13, bajo demanda. Es lo que pide #4. Alternativa: mantener el recorrido
   de carpetas.
9. (marcado para revisión) Umbral de antigüedad de un documento de contexto: 180 días.

## Dudas abiertas
Ninguna. Los tres marcadores se resolvieron el 2026-11-22 (ver `05_avance.md`, decisiones 11 a 13):
cambio pequeño en modo ligero de siempre (RF-13), celda escrita por la sesión al registrar (RF-28) y
`/sdd-constitucion` como comando auxiliar (RF-27).

## Modo (lista de condiciones de `topes-y-modos.md`)
1. ≤ 3 archivos: **no**. Toca más de 20 (`AGENTS.md` ×2, comandos, skill `sdd`, `validar-estructura`,
   `INSTALAR.md`, `ACTUALIZAR.md`, índices).
2. Sin archivos compartidos con otra spec en curso: **sí**. No hay otra spec abierta en el índice.
3. No cambia reglas globales: **no**. Toca Guardrails (regla 1), la regla Git y «Cada sesión».
4. Sin ADR: **no**. RNF-6 prevé al menos tres.
5. Sin `sql/`: **sí**. Solo Markdown.
6. Sin comportamiento nuevo: **no**. Hay comandos nuevos (`/sdd-pausa`, `/sdd-version`) y chequeos
   nuevos.
7. ≤ 1 hora: **no**. Ocho bloques.
8. Un solo módulo: **no**. Toca `agents`, `commands`, `skills`, `.docs` y los docs de la raíz.

Modo propuesto: **completo**.
