# 01 — Spec: Optimizar el arnés SDD (agentes y comandos)
Estado: implementada
Ticket: sin ticket

## Contexto y objetivo
Los 4 agentes y los 14 comandos del kit gastan contexto (tokens) en lecturas que el paso no usa, repiten
pasos que otro rol ya hizo, reescriben procedimientos que ya viven en otra pieza y, en algunos casos,
hacen trabajo que no es de su rol. Revisados todos, la medición de referencia (carpeta de un feature
grande, bytes ÷ 4 ≈ tokens) muestra:
- Un paso del `implementer` lee lo que su instrucción pide (spec, plan y tareas completos, más
  `AGENTS.md`, la skill `sdd` y la constitución): ~85 KB (~21 mil tokens). Con 32 pasos son ~680 mil
  tokens de lectura, contra ~4 a 6 mil realmente necesarios por paso.
- `/sdd-status` lee los seis documentos completos y `MEMORY.md` (~119 KB, ~30 mil tokens) para responder
  en unas líneas.
- El `coordinator` lee al empezar piezas que no usa; el `planner` lee todo el contexto del proyecto aun
  para redactar tareas o flujo; `/sdd` lee ~20 KB antes de decidir el comando aunque sea `status`;
  `/sdd-spec` lee lo que luego relee el `planner`.
- Cada subagente repite los pasos de arranque de sesión que la sesión principal ya hizo.
- El mismo procedimiento existe en cuatro copias (descripción del `coordinator`, tabla de la skill,
  comando, agente) que ya se desfasaron entre sí (numeración de carpetas, cierre sin deuda ni ADR,
  lista de comandos incompleta).
- Hay trabajo fuera de rol o ambiguo (quién ejecuta la validación de estructura; `/sdd-validate` y
  `/sdd-flujo` revisan el diff completo aunque haya trabajo de otro feature) y registros de tiempo que
  admiten filas "en lote" con 0 minutos.

Objetivo: reducir el consumo de contexto de los agentes y comandos y eliminar duplicados, pasos
repetidos y trabajo fuera de rol, **sin bajar la calidad**: no se quita ninguna verificación ni
resultado, solo lecturas que el paso no usa, repeticiones y tareas ajenas al rol.

Decisiones de contexto (ya tomadas):
- Es un cambio al producto: el kit distribuible. Pasa por el flujo SDD (principio 2 de la constitución).
- Otra sesión del responsable puede editar en paralelo archivos que este feature también toca. El
  trabajo parte del contenido actual de esos archivos y no revierte cambios ajenos (ver "Requisito
  previo de coordinación" en los criterios de finalización).
- Conserva lo ya implementado en `001_ActualizarKit` (procedimiento de actualización, manifiestos,
  ADR 007 y 008): este feature no cambia qué se clasifica ni cómo se reemplaza, y protege la definición
  del chequeo E20 y su redacción en `validar-estructura.md`.
- `AGENTS.md` es del grupo B del procedimiento de actualización (fusión manual): los cambios que este
  feature haga en `AGENTS.md` llegan a los proyectos ya instalados por fusión manual, no por
  reemplazo (ver RF-12 y casos límite).
- El kit sigue siendo solo Markdown, sin scripts (ADR 004) y genérico (principio 1).
- "Sin publicar" significa: no hay `push` ni distribución a ningún proyecto; 0.5.0 es solo local. Este
  feature entra en 0.5.0 y el `CHANGELOG.md` de 0.5.0 gana una línea que lo refleja.

## Usuarios y roles
- **Responsable del proyecto** (usa el kit): quiere respuestas iguales de buenas con menos consumo.
- **Agentes** (`coordinator`, `planner`, `implementer`, `reviewer`): reciben instrucciones más precisas
  sobre qué leer y qué no.
- **Sesión principal**: la que dirige el flujo; es la única que escribe `05_avance.md`.
- **Mantenedor del kit**: debe poder comprobar que cada procedimiento tiene una sola fuente.

## Historias de usuario
- HU-1. Como responsable, quiero que un paso de implementación lea solo lo que ese paso necesita, para
  que un feature grande no multiplique el consumo por el número de pasos.
- HU-2. Como responsable, quiero que `/sdd-status` y los comandos de consulta respondan leyendo el
  mínimo, para no pagar decenas de miles de tokens por una respuesta de pocas líneas.
- HU-3. Como responsable, quiero que cada subagente no repita el arranque que la sesión principal ya
  hizo, para no pagar dos veces la misma lectura.
- HU-4. Como mantenedor, quiero que cada procedimiento viva en una sola pieza y las demás la
  referencien, para que no se desfasen.
- HU-5. Como responsable, quiero que cada rol haga solo lo suyo y que ningún paso se asuma o se omita,
  para confiar en el resultado.
- HU-6. Como responsable, quiero poder comprobar que la calidad no bajó tras la optimización, para
  aceptar el cambio con evidencia.

## Definiciones
- **Lectura** (de un rol o comando): lo que su instrucción **manda leer**, no lo que realmente leyó en
  una ejecución. Se cuenta en bytes de los archivos o de las secciones citadas, más el texto del agente
  y del comando que lo invoca (porque también se cargan). Bytes ÷ 4 ≈ tokens solo como referencia.
- **Sección**: desde su encabezado hasta el siguiente de igual o mayor nivel. Un **paso** es el bloque
  `Fn.m` de `04_tasks.md`; un **RF** es su texto completo en `01_spec.md` más los casos límite que lo
  nombran; la **parte del plan que cubre un paso** es la(s) sección(es) de `03_plan.md` que nombran ese
  `Fn.m`, alguno de los RF del paso o el archivo que el paso toca.
- **Lectura mínima de un paso**: paso en `04_tasks.md` (con su "Hecho cuando") + los RF que el paso cita
  + la parte del plan que lo cubre + las secciones de `AGENTS.md` que el rol usa (p. ej. Comandos y
  convenciones de código) + de la constitución y de la skill `sdd` solo las secciones que el paso o el
  plan citan. Ni los documentos ni la constitución ni la skill completos por defecto.
- **Lecturas permitidas (lista cerrada)** para ampliar la lectura mínima de un paso, con un tope de 3
  secciones adicionales por paso: (a) una sección del plan o de la spec nombrada por un RF o paso
  citado; (b) una decisión de `05_avance.md` que el paso cite o de la que dependa; (c) una sección de
  `AGENTS.md` distinta de las que el rol ya usa. Fuera de esa lista, o por encima del tope, el
  subagente no amplía: lo devuelve a la sesión principal. Leer un documento completo **no** es una
  ampliación legítima salvo el caso de respaldo de RF-2.
- **Procedimiento**: la secuencia de acciones de una fase. Su **fuente única** es una sola pieza del
  kit; las demás solo la nombran y enlazan.
- **Fuente única por fase**: el procedimiento de cada fase vive en su comando `/sdd-<fase>`. El agente
  declara solo qué lee, qué hace y qué no hace; la tabla de la skill `sdd` solo asocia fase, documento
  y rol; la descripción del `coordinator` solo rutea. Excepciones, cada una con su dueño: el contenido
  de cada documento (plantillas de la skill `sdd`), las reglas de tiempos y de `05_avance` (contrato de
  avance y tiempos) y el arranque de sesión (`AGENTS.md`).
- **Cadena** (RF-9): la secuencia, dentro de una misma petición, sesión principal → `/sdd-spec` →
  `planner`. La **sesión principal retiene** lo leído y lo entrega al subagente (extracto citado) o le
  indica que no lo relea.
- **Código ejecutable** (RF-11): todo lo que un agente, un shell o una herramienta ejecuta o interpreta
  como instrucción operativa: scripts, comandos de shell o bloques de código copiables, configuración
  con permisos, `tools` o `mode` en el frontmatter, SQL y cualquier cambio que toque datos, accesos o
  secretos. La prosa que solo explica, describe o enumera no lo es. En este kit los `.md` son
  instrucciones para agentes, así que un `.md` solo cuenta como código ejecutable si añade o modifica
  alguno de los elementos anteriores, no por ser `.md`.
- **Línea base**: medición de la lectura por rol y por comando antes del cambio, con el mismo método y
  el mismo caso de referencia. Se registra **antes de cambiar nada** (ningún archivo de `template/` ni
  de los documentos de la raíz) en el `05_avance.md` de esta carpeta. La carpeta `001_ActualizarKit`
  puede cambiar después: por eso la línea base congela los tamaños en bytes de los documentos y
  secciones del caso, y la medición posterior usa esos mismos tamaños para los documentos y los tamaños
  actuales para las piezas del kit.
- **Pasos de referencia**: tres pasos de `04_tasks.md` de `001_ActualizarKit`, fijados por identificador
  en la línea base: el de **menor**, el de **mediana** y el de **mayor** lectura mínima en bytes.
- **Medición "por paso del `implementer`"**: la lectura del `implementer` ejecutando cada paso de
  referencia, antes y después. La métrica de RNF-1 es la del paso de mediana; los otros dos se
  informan (el de menor y el de mayor no pueden aumentar).

## Requisitos funcionales (EARS)

Lecturas por rol y fase
- RF-1: CUANDO el `implementer` ejecute un paso `Fn.m`, EL SISTEMA LE INDICARÁ leer su lectura mínima
  (definida arriba) y NO los documentos completos. Localización: el paso se ubica por su identificador
  `Fn.m`; los RF, por los identificadores `RF-n` que el paso cita; la parte del plan, por la definición
  de arriba. SI el paso no cita ningún RF, ENTONCES leerá la fase `Fn` de las tareas y avisará a la
  sesión principal, que decide qué RF aplican. SI el plan no tiene sección que cubra el paso, ENTONCES
  leerá el plan completo y lo avisará. Un documento de menos de 4 KB puede leerse completo: nunca debe
  costar más localizar que leer.
- RF-2: CUANDO un paso necesite contexto fuera de su lectura mínima, EL SISTEMA PERMITIRÁ ampliar la
  lectura solo dentro de las "lecturas permitidas" (lista cerrada y tope de 3), sin leer el documento
  completo por defecto. El `implementer` y el `reviewer` no escriben `05_avance.md`: **devuelven el
  motivo** de cada ampliación (y los avisos de RF-1) en su respuesta a la sesión principal, que lo anota
  en las Decisiones de `05_avance.md`. El único caso de lectura completa es el respaldo de RF-1 (paso
  sin RF o plan sin sección), con aviso.
- RF-3: CUANDO el `reviewer` verifique un paso o fase, EL SISTEMA LE INDICARÁ leer el checklist
  "Hecho cuando" de ese paso o fase **y** los criterios de aceptación de los RF que el paso cita
  (localizados como en RF-1), y no las tareas ni la spec completas. Las verificaciones de RF-24 se
  conservan íntegras.
- RF-4: CUANDO se ejecute `/sdd-cierre`, EL SISTEMA COMPROBARÁ que todo está hecho sin leer las tareas
  completas: localizará por búsqueda de patrón los pasos cuyo estado en la tabla de Pasos de
  `05_avance.md` no sea `[x]` (leyenda: `[ ]` pendiente, `[~]` en curso, `[!]` bloqueado). Un paso
  `[ ]`, `[~]` o `[!]` cuenta como no hecho e impide el cierre, salvo que el usuario decida diferirlo
  y quede registrado (con propuesta de deuda). Las verificaciones manuales pendientes se detectan en las
  secciones Pruebas y PUNTO DE RETOMO de `05_avance.md`. El resto de comprobaciones del cierre (deuda,
  ADR, índices) se conserva.
- RF-5: CUANDO se ejecute `/sdd-status`, EL SISTEMA RESPONDERÁ leyendo solo: la línea `Estado` de
  `01_spec.md`, la lista de documentos presentes de la carpeta, las secciones PUNTO DE RETOMO, Pasos,
  Pruebas y Tiempos de `05_avance.md`, y el conteo de pasos de `04_tasks.md` por búsqueda de sus
  identificadores. Con eso entrega lo que hoy entrega: fase y Estado de la spec; punto de retomo; pasos
  hechos y pendientes (x de y); verificaciones manuales pendientes; horas acumuladas y filas con
  `pausa por confirmar`; y el siguiente paso con su comando `/sdd-*`. NO leerá `01`–`06` completos ni
  `MEMORY.md`, salvo que el usuario pida detalle de uno. SI no hay `05_avance.md`, deducirá la fase
  por los documentos presentes y el `Estado` de la spec.
- RF-6: CUANDO el `planner` redacte un documento, EL SISTEMA LE INDICARÁ leer solo el contexto que ese
  documento usa, declarado por documento en su instrucción (por ejemplo, para tareas: spec, plan,
  plantilla y `AGENTS.md` › Comandos; para flujo: spec, plan, avance y el alcance acotado de RF-19), y
  NO todo el contexto del proyecto en cada tarea.
- RF-7: CUANDO el `coordinator` inicie, EL SISTEMA LE INDICARÁ leer solo lo que necesita para decidir y
  delegar, y diferir el resto hasta que una fase lo requiera (la constitución y `MEMORY` no se leen
  para dirigir el flujo; el índice de specs solo para ubicar una carpeta).
- RF-8: CUANDO se ejecute `/sdd`, EL SISTEMA ELEGIRÁ el comando con lo mínimo para decidirlo y cargará
  la skill y el contrato de avance solo si la fase elegida los usa; para `status` o `estructura` no se
  cargan.
- RF-9: CUANDO `/sdd-spec` y el `planner` necesiten el mismo contexto (constitución, memoria), EL
  SISTEMA LO LEERÁ UNA sola vez en la cadena: la sesión principal lo retiene y lo pasa en la
  delegación, o indica al `planner` que no lo relea. SI el `planner` necesita algo que no se le
  entregó, lo lee de forma acotada.
- RF-10: CUANDO se ejecuten `/sdd-init` o `/sdd-constitucion` y deban inspeccionar código, EL SISTEMA
  LIMITARÁ la inspección a **30 archivos o 150 KB acumulados, lo que ocurra primero**, y pedirá
  confirmación para ampliarlo. El tope se declara en el texto de `/sdd-init` y `/sdd-constitucion`
  lo referencia. Estos dos comandos no tienen meta de reducción (su lectura es el código del proyecto,
  acotado por el tope), pero entran en la tabla de RF-23 con su lectura declarada.
- RF-11: CUANDO `/sdd-validate` o el `reviewer` apliquen la seguridad, EL SISTEMA CLASIFICARÁ el
  alcance del cambio y aplicará la skill `seguridad-basica` completa si toca código ejecutable (según
  la definición), datos, accesos o secretos; si es solo documentación, aplicará solo la verificación de
  secretos y lo dirá. Clasifica el `reviewer` al validar, con ese criterio, y declara la clasificación
  en su reporte; ante duda, aplica la skill completa. La búsqueda de secretos aplica **siempre**. La
  regla de los dos alcances vive en la skill `seguridad-basica`, que declara cuál es el modo
  "solo secretos"; `/sdd-validate` y el `reviewer` la referencian sin repetirla.

Arranque de subagentes
- RF-12: CUANDO la sesión principal delegue en un subagente, EL SISTEMA NO SUPONDRÁ que la herramienta
  inyecta `AGENTS.md` al subagente: cada subagente leerá de `AGENTS.md` solo las secciones que su rol
  usa (p. ej. Comandos y convenciones de código) y NO ejecutará los pasos de arranque de sesión que la
  sesión principal ya hizo (verificación de enlaces, prefijos repetidos, preferencias). No hay que
  verificar el comportamiento de la herramienta. Los cambios en `AGENTS.md` (de `template/` y de la
  raíz del clon) se hacen sin divergir en lo genérico y llegan a los proyectos por fusión manual
  (grupo B: el procedimiento los muestra como faltantes).
- RF-13: CUANDO una instrucción de arranque deba ejecutarse en un subagente, EL SISTEMA LO DECLARARÁ
  explícitamente en su instrucción, y cada rol declarará qué secciones de `AGENTS.md` lee, en lugar
  de depender de una lectura por defecto o de una herencia supuesta.

Una fuente por procedimiento
- RF-14: EL SISTEMA TENDRÁ para cada fase un solo lugar que defina su procedimiento (su comando, según
  la "Fuente única por fase"); las demás piezas lo nombrarán sin reescribirlo. Se verifica así: para
  cada fase se registra una tabla fase → fuente única → piezas que la referencian, y se comprueba
  por lectura cruzada que en ninguna otra pieza aparece la secuencia de pasos de esa fase, solo su
  nombre y un enlace.
- RF-15: EL SISTEMA ELIMINARÁ las discrepancias entre copias: numeración de carpetas coherente con la
  regla de E12 (4 dígitos en proyectos, 3 en el clon del kit; las carpetas de sección siempre 3), de
  modo que ninguna pieza de `template/` cita 3 dígitos para specs, deudas y ADR; el cierre de **todo**
  feature (`/sdd-cierre` y toda pieza que lo describa) revisa deuda y ADR; y la lista de comandos del
  punto de entrada completa (incluye `sdd-estructura`).
- RF-16: CUANDO una regla aplique a varias fases, EL SISTEMA LA DEFINIRÁ una sola vez en su dueño y las
  fases la referenciarán. Reglas multi-fase afectadas (lista cerrada): (1) pausa de más de 30 minutos y
  filas `pausa por confirmar`; (2) formato de la fila de tiempo por paso (inicio, fin, minutos; RF-22);
  (3) quién escribe `05_avance.md` (solo la sesión principal); (4) leyenda de estados de los pasos;
  (5) el arranque de sesión (dueño: `AGENTS.md`; RF-12). Las reglas 1 a 4 tienen como dueño el contrato
  de avance y tiempos de la skill `sdd`.
- RF-17: CUANDO `/sdd-plan` y `/sdd-tasks` pidan un documento que el `planner` y la skill ya definen,
  EL SISTEMA LOS HARÁ referenciar esa definición, sin repetir el contenido del documento.

Trabajo en su rol, sin pasos asumidos ni innecesarios
- RF-18: CUANDO se ejecute `/sdd-estructura`, EL SISTEMA LO EJECUTARÁ la sesión principal, sin lanzar
  un subagente; el `reviewer` conserva `validate` y no ejecuta la validación de estructura. Todas las
  comprobaciones E1 a E20 las ejecuta la sesión principal, y el checklist de estructura se actualiza
  para decirlo una sola vez (en su alcance, no fila por fila), sin cambiar qué comprueba ni la
  definición de E20. Criterio de aceptación: el checklist, el comando y el `reviewer` coinciden en
  quién ejecuta, y no queda ninguna instrucción que mande una comprobación E1–E20 a un subagente.
- RF-19: CUANDO se ejecute `/sdd-validate` o `/sdd-flujo`, EL SISTEMA ACOTARÁ lo que revisa al alcance
  del feature: los archivos citados en avance, plan y tareas **más** los archivos nuevos sin seguimiento
  (los lista el estado de Git, no el `git diff`) que estén en la carpeta del feature o citados. Todo
  otro archivo modificado o sin seguimiento en el árbol se informa como "ajeno o no atribuible" sin
  revisarlo como propio; como la lista de citados puede estar incompleta, el usuario decide si algún
  ajeno pertenece al feature. Para archivos con seguimiento se usa su diferencia; para los nuevos, su
  contenido. Si un archivo mezcla cambios del feature y ajenos, se avisa. Sin Git, el alcance es solo
  lo citado.
- RF-20: CUANDO `/sdd-clarify` deba mostrar el cambio de la spec y el archivo no tenga seguimiento en
  Git (o el proyecto no use Git), EL SISTEMA MOSTRARÁ el contenido modificado por otro medio y no
  dependerá de `git diff`.
- RF-21: CUANDO un agente o comando no deba confirmar ni escribir algo (por ejemplo, el `implementer`
  no registra en Git; el `reviewer` no corrige código), EL SISTEMA LO DECLARARÁ en su texto, de modo que
  esa declaración coincida con la regla del proyecto (`AGENTS.md` › Git y reglas duras). Es un requisito
  de documentación, verificable por lectura cruzada. **No es una garantía técnica**: la garantía por
  permisos del frontmatter queda en la deuda posterior (fuera de alcance).
- RF-22: CUANDO se registre un tiempo en un feature abierto desde esta versión, EL SISTEMA EXIGIRÁ un
  inicio y un fin reales por paso, con su tiempo medido (mínimo 1 minuto por paso real), y PROHIBIRÁ
  registrar filas en lote con 0 minutos para cerrar varios pasos. La única excepción es un paso trivial
  marcado con la frase exacta **"sin trabajo real"** en la columna Nota de su fila. Los features
  existentes (p. ej. `001_ActualizarKit`) siguen válidos con sus filas actuales (RNF-5): ninguna
  comprobación del kit los rechaza ni exige reescribirlos.

Calidad verificable
- RF-23: CUANDO termine la optimización, EL SISTEMA PERMITIRÁ comparar contra la línea base la lectura
  (según la definición) por rol (los 4 agentes) y por comando (los 14), y mostrar la reducción, para el
  mismo caso de referencia y los tres pasos de referencia del `implementer`. La medición es sobre
  `template/` (el producto), no sobre la copia local de `.agents/`, y sobre el **contenido final** de
  `template/`.
- RF-24: CUANDO se compare antes y después, EL SISTEMA DEMOSTRARÁ que cada verificación existente
  sigue presente en el procedimiento resultante: criterios de aceptación, "Hecho cuando", chequeos de
  `AGENTS.md` › Comandos, revisión de seguridad con alcance código, cierre con deuda y ADR, y el
  contenido de `/sdd-status` (fase y Estado, punto de retomo, pasos hechos y pendientes, verificaciones
  manuales pendientes, horas y `pausa por confirmar`, siguiente comando). Cada una con la pieza donde
  sigue.

## Requisitos no funcionales
- RNF-1: La lectura por paso del `implementer` (paso de mediana de los pasos de referencia) y la de
  `/sdd-status` se reducen al menos un 70 % frente a la línea base, medidas con la definición de
  "Lectura" sobre el caso de referencia `001_ActualizarKit`. Para el `implementer` cuentan también
  `AGENTS.md`, la skill `sdd` y la constitución, solo en las secciones que la lectura mínima incluye.
- RNF-2: Sin pérdida de calidad: ninguna verificación, resultado o documento obligatorio del flujo se
  elimina; cada recorte se justifica por "el paso no usa esa lectura" o "otra pieza ya lo hace".
- RNF-3: El kit sigue siendo solo Markdown, UTF-8 sin BOM, en español, genérico y sin scripts.
- RNF-4: Los cambios parten del contenido actual de cada archivo; ningún cambio ajeno en curso se
  revierte.
- RNF-5: Compatibilidad: los documentos de features ya existentes (formato de `01`–`06`) siguen siendo
  válidos sin migración, y la clasificación y reemplazo del procedimiento de actualización no cambian.
- RNF-6: Cada pieza modificada (agente o comando) declara tres cosas: qué lee, qué hace y qué no hace.
  Se verifica por lectura de cada pieza modificada.

## Casos límite
- Un paso cuyo contexto citado es insuficiente: se amplía dentro de las lecturas permitidas (RF-2); si
  excede el tope, lo devuelve a la sesión principal.
- Tareas sin "Hecho cuando", paso sin RF citado o plan sin sección que lo cubra: lectura de respaldo con
  aviso (RF-1); el aviso queda en las Decisiones de `05_avance.md`.
- Un feature pequeño: la lectura acotada nunca supera la del documento completo (los documentos de menos
  de 4 KB se leen enteros).
- Un cambio de documentación que toca también una regla de seguridad (secretos): no se omite la
  verificación de secretos (RF-11).
- Un archivo del feature con cambios ajenos mezclados en el mismo archivo: se avisa en lugar de
  atribuirlo o revisarlo como propio (RF-19).
- Dos sesiones editando el mismo archivo: antes de editar se vuelve a leer el archivo, se edita solo la
  parte propia y, si cambió desde la última lectura, se avisa; nunca se sobrescribe con una copia leída
  antes.
- Un feature creado a mano o previo (sin punto de retomo, sin secciones esperadas, con filas de tiempo
  en lote): sigue válido (RNF-5). `/sdd-status` indica lo que falta y lee solo Pasos y Tiempos de
  `05_avance.md`, no el documento completo.
- Un paso que toca un archivo cuyo contexto vive solo en `AGENTS.md` › Comandos: esa sección ya está en
  la lectura mínima; no se amplía.
- Un paso que depende de decisiones de `05_avance.md`: se lee la decisión citada (lectura permitida b).
- Un proyecto que ya tiene el kit instalado: las piezas del grupo A modificadas llegan por el
  procedimiento de actualización (ADR 007): se reemplazan si son KIT-VIEJO y se respetan si fueron
  personalizadas. `AGENTS.md` es del grupo B: nunca se reemplaza; sus cambios llegan como faltantes por
  fusión manual. Como el faltante se compara por identidad del elemento y no por redacción, un cambio de
  redacción en un elemento ya existente no se detecta: el `CHANGELOG.md` lo describe.
- Copias locales del clon: la medición es sobre `template/`. Tras cambiar `template/.agents/`, la
  comprobación E19 (copia local vs `template/`) mostrará diferencias hasta refrescar la copia local por
  `INSTALAR.md` › Modo mantenimiento; el refresco se propone y se hace solo con confirmación. Los demás
  chequeos de réplicas de `/sdd-estructura` deben seguir pasando.
- El tope de `/sdd-init` y `/sdd-constitucion` demasiado bajo para un proyecto muy grande: se pide
  confirmación para ampliarlo, no se lee sin límite.
- Feature de pocos pasos: la reducción medible se verifica en el caso de referencia, no en el feature
  pequeño; en este solo se exige no leer más que antes.

## Fuera de alcance
- Cambiar el contenido de las skills de dominio o de equipo, salvo lo que exigen RF-11 (agregar a
  `seguridad-basica` el modo "solo secretos" y la regla de alcance, una sola vez) y RF-16 (el contrato
  de avance y tiempos de la skill `sdd`). RF-16 no afecta a `seguridad-basica`.
- Reducir el tamaño de los documentos de los features (spec, plan, tareas): se optimiza qué se lee de
  ellos, no cómo se redactan.
- Cambiar el procedimiento de actualización, los manifiestos o su formato (ADR 007 y 008), qué se
  clasifica y cómo se reemplaza, y la definición de E20 y su redacción.
- Agregar scripts, herramientas externas o medición automática (ADR 004).
- Cambiar el estilo de la línea "quién responde" o el formato de respuesta.
- Decisiones sobre qué modelo usa cada rol (`model:`) y sobre los permisos del frontmatter (`mode`,
  `permissions`, escritura del `reviewer`, shell del `implementer`): quedan fuera. Se darán de alta como
  deuda técnica, previa verificación del comportamiento real de Claude Code. Por eso RF-21 es solo
  documentación y no ofrece garantía.
- Verificar cómo la herramienta pasa `AGENTS.md` a los subagentes (RF-12 no lo supone).
- Re-ejecutar un feature completo para comparar calidad (la prueba es la de RF-23 y RF-24).

## Criterios de finalización
- Requisito previo de coordinación (antes de editar cualquier pieza): la sesión principal deja en
  `05_avance.md` una entrada fechada, con la confirmación del usuario, que documenta (a) con quién del
  equipo se coordinó la edición en paralelo, (b) quién commitea los features previos pendientes y
  (c) el estado del árbol (qué archivos tienen cambios ajenos). Esa entrada es la evidencia del acuerdo.
  La implementación parte del contenido actual de esos archivos y no revierte trabajo ajeno.
- La línea base está registrada en `05_avance.md` de esta carpeta antes del primer cambio (definición
  de "Línea base").
- Cada RF-1 a RF-22 tiene su cambio reflejado en la pieza correspondiente y se puede señalar dónde.
- RF-23 cumplido: tabla antes/después por los 4 roles y los 14 comandos y por los 3 pasos de referencia,
  con el mismo método y caso de referencia; el paso de mediana del `implementer` y `/sdd-status`
  alcanzan la reducción de RNF-1; ninguna fila aumenta sin justificación.
- RF-24 cumplido: lista de verificaciones existentes, cada una con la pieza donde sigue presente.
  Con ello se cubren los 24 RF (RF-1 a RF-24).
- Cada procedimiento tiene una sola fuente; ninguna pieza reescribe el de otra (RF-14, con su tabla).
- `AGENTS.md` de `template/` y de la raíz del clon quedan sin divergir en lo genérico (RF-12).
- Cierre en este orden: primero todos los cambios en `template/` y en los docs de la raíz; después se
  regenera el manifiesto de 0.5.0 según el procedimiento de `001_ActualizarKit`; E20 pasa; se repite el
  contraste de 0.4.0 contra 0.5.0 y los chequeos de `AGENTS.md` › Comandos tras regenerar; y la
  medición de RF-23/RF-24 se hace sobre ese contenido final. Si `template/` cambia después, se
  regenera el manifiesto y se repite la medición.
- Los chequeos de `AGENTS.md` › Comandos pasan en cada archivo tocado y el ensayo de instalación no
  pisa nada. Los chequeos de réplicas de `/sdd-estructura` siguen pasando, y la copia local del clon
  se refresca (con confirmación) o queda registrada como pendiente.
- La prueba de calidad consiste solo en RF-23 y RF-24. Sin regresión.
- Este feature sale dentro de 0.5.0 sin publicar; el `CHANGELOG.md` de 0.5.0 gana una línea que lo
  refleja.

## Dudas abiertas
Ninguna.
