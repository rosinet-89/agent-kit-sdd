# 01 — Spec: Actualizar un proyecto a una versión nueva del kit
Estado: implementada
Ticket: sin ticket

## Contexto y objetivo
Hoy el kit solo sabe instalarse: copia lo que falta y nunca sobrescribe. En un proyecto que ya lo
tiene instalado, volver a instalar una versión nueva deja el proyecto a medias: se copian las piezas
nuevas, pero las que cambiaron no se reemplazan y las instrucciones del proyecto no se enteran de las
novedades (quedan skills sin conectar). Además el proyecto no guarda con qué versión del kit se
instaló, así que el agente no distingue "esto es del kit, solo está viejo" de "esto lo personalizó el
equipo".

Objetivo: que el usuario pueda llevar un proyecto con el kit instalado a una versión nueva, sin perder
nada de lo personalizado, revisando antes lo que se va a cambiar, y pudiendo repetir el proceso sin
efectos secundarios.

Decisiones de contexto (ya tomadas):
- Este requerimiento sale **dentro de la versión 0.5.0**. Esa versión aún no se ha publicado (sus
  cambios son locales) y ningún proyecto real la tiene. Por eso 0.5.0 nace con marcador de versión y su
  manifiesto se genera al cerrar este requerimiento, con el estado final de la versión. El primer salto
  real que soporta el procedimiento es 0.4.0 a 0.5.0.
- Excepción a un principio: la constitución dice que la copia nunca sobrescribe. Esta spec introduce
  **una sola excepción**: reemplazar archivos KIT-VIEJO (idénticos a la versión base, nunca
  personalizados), con confirmación explícita y mostrando la diferencia. La instalación nueva sigue sin
  sobrescribir nada. La excepción exige enmendar el principio de la constitución y registrar la
  decisión en un ADR.
- Propiedad de los manifiestos: los manifiestos de versión son del kit y viven con la documentación
  de la raíz del kit; **no se distribuyen dentro de lo que se copia a los proyectos**. Así ningún
  manifiesto debe clasificarse a sí mismo ni a otro manifiesto (sin circularidad). El procedimiento
  corre con el kit disponible localmente, por lo que tiene los manifiestos a mano.
- Fuera del manifiesto: el marcador de versión no lo distribuye el kit; lo escribe el procedimiento en
  cada proyecto. La versión del kit (dato de la raíz del kit) y el marcador (versión instalada en el
  proyecto) son datos distintos y no se contradicen: el primero describe al kit, el segundo a la
  instalación.

Hechos verificados del kit:
- No existen etiquetas de versión en el repositorio del kit, y la actualización no debe depender de
  ellas ni del historial de Git.
- El historial solo conserva el contenido de la versión 0.4.0 (commit `861bb99`) y de las posteriores.
  Las versiones 0.1.0 a 0.3.0 no tienen contenido recuperable; solo se describen en el registro de
  cambios. Por eso el manifiesto de 0.4.0 se genera una sola vez desde el commit `861bb99` y de ahí en
  adelante cada versión trae el suyo.
- Ningún proyecto instalado con 0.4.0 o anterior tiene marcador de versión.

## Usuarios y roles
- **Responsable del proyecto** (persona que usa el kit): decide qué se actualiza y confirma cada cambio.
- **Agente**: ejecuta el procedimiento y presenta el plan de cambios; no decide por el responsable.
- **Mantenedor del kit**: publica versiones y debe dejar al kit en condiciones de ser actualizable.

## Historias de usuario
- HU-1. Como responsable de un proyecto con el kit instalado, quiero actualizarlo a una versión nueva
  para recibir las skills, agentes y comandos nuevos sin perder mis personalizaciones.
- HU-2. Como responsable, quiero ver antes de aplicar qué se agregará, qué se reemplazará, qué se
  respetará y qué requiere fusión manual, para confirmar con conocimiento.
- HU-3. Como responsable de un proyecto instalado sin marcador de versión, quiero poder actualizarlo
  igualmente, para no quedar excluido por haber instalado antes.
- HU-4. Como mantenedor del kit, quiero que cada proyecto quede marcado con la versión instalada, para
  que futuras actualizaciones sepan de dónde parten.
- HU-5. Como responsable, quiero que repetir la actualización no cambie nada si ya estoy al día, y que
  una actualización a medias se pueda retomar, para ejecutarla sin miedo.
- HU-6. Como responsable, quiero poder decir "mantener" sobre algo que el kit propone y que no quiero,
  y que no vuelva a molestarme en cada corrida.

## Definiciones
- **Marcador de versión:** dato de una sola línea, guardado dentro de la carpeta de configuración de
  agentes del proyecto y versionado en Git (si el proyecto usa Git), con la versión del kit instalada o
  actualizada por última vez. Todo el equipo comparte la misma base. Es un archivo propio del
  procedimiento: no pertenece al kit, no está en ningún manifiesto y no se clasifica.
- **Manifiesto de versión:** lista que el kit trae por cada versión publicada, con cada archivo que
  distribuye y su huella. Permite saber cómo era un archivo en esa versión sin Git ni etiquetas.
- **Huella:** valor determinista calculado sobre el contenido **normalizado** de un archivo.
  Normalización previa, la misma para todos los manifiestos y para el archivo del proyecto: quitar la
  marca de orden de bytes (BOM), quitar espacios y tabuladores al final de cada línea y unificar el fin
  de línea (CRLF y LF equivalentes), con un único salto de línea final. Dos contenidos que difieran solo
  en esos aspectos producen la misma huella; no entran en la huella los permisos ni las
  mayúsculas/minúsculas del nombre.
- **Versión base:** versión de la que parte el proyecto (la del marcador o, si falta, 0.4.0 confirmada
  por el usuario).
- **Versión destino:** la versión del kit que se usa para actualizar.
- **Mantener:** decisión explícita del usuario de no aplicar una propuesta del kit sobre un archivo o
  elemento, registrada de forma persistente en el proyecto y asociada a la versión destino. Mientras el
  kit no vuelva a cambiar ese elemento en una versión posterior, no se vuelve a proponer.
- **Clasificación** de un archivo del kit. Con **B** = huella en el manifiesto de la base, **D** = huella
  en el manifiesto del destino y **P** = huella del archivo en el proyecto, cada archivo recibe **una
  sola clase**, evaluando de arriba abajo y deteniéndose en la primera fila que cumple:

| # | Condición | Clase | Qué se muestra / hace |
|---|---|---|---|
| 1 | P no existe y D existe | NUEVO | Se propone copiar |
| 2 | D no existe y P existe y B existe | RETIRADO | Solo se informa; si P difiere de B, se avisa que además fue modificado |
| 3 | D existe y P = D | IGUAL | Nada (incluye el caso en que el kit no cambió entre versiones) |
| 4 | D existe, P ≠ D, B existe y P = B | KIT-VIEJO | Diferencia B→D; se propone reemplazar |
| 5 | D existe, P ≠ D, y (B no existe o P ≠ B) | PERSONALIZADO | Si B existe y B ≠ D: diferencia B→D para fusionar a mano. Si B = D o B no existe: no hay cambio del kit que mostrar; se respeta y se informa |

  Un archivo del proyecto que no está en ningún manifiesto (base ni destino) es un **huérfano**: no se
  clasifica, no se toca y se informa aparte.
- **Grupos de archivos** (qué pasa por la clasificación):
  - **Grupo A, archivos del kit puros** (agentes, comandos, skills, documentos de apoyo de la carpeta de
    configuración de agentes, salvo las notas de trabajo del agente): pasan por la clasificación y por
    RF-8, RF-9 y RF-10.
  - **Grupo B, fusión manual con faltantes** (`AGENTS.md`, `CLAUDE.md`, `.gitignore`): nunca se
    reemplazan ni se clasifican; solo se muestran los elementos del kit que faltan (RF-11).
  - **Grupo C, propios del proyecto, solo si faltan** (`MEMORY.md`, las notas de trabajo del agente, la
    constitución, los índices y todo el contenido de `.docs/`): si no existen, se ofrecen como NUEVO;
    si existen, no se comparan, no se reemplazan ni se muestran faltantes. `.docs/` mezcla plantillas
    del kit con documentos del proyecto, por eso toda su existencia se respeta.
  - **Grupo D, personales** (`PREFERENCIAS.md`): nunca se tocan ni se muestran; si no existe, se aplica
    lo que ya indica la instalación.
- **Faltante** (grupo B): elemento que una versión intermedia o la destino agregó al kit respecto de la
  base (una fila de la tabla de skills, un paso de "Cada sesión", una regla de ignorados) y que el
  archivo del proyecto no contiene, comparando por identidad del elemento (nombre de la skill, propósito
  del paso, regla) y no por redacción exacta. No cuentan como faltante: marcadores de personalización ya
  sustituidos, secciones propias del proyecto, ni un elemento que el usuario registró como "mantener"
  (por ejemplo, una fila borrada a propósito).

## Requisitos funcionales (EARS)
- RF-1: CUANDO el usuario pida actualizar un proyecto, EL SISTEMA DEBE tomar como versión destino la del
  kit que se está usando, determinar la versión base y no proponer cambios hasta conocer ambas.
- RF-2: SI el proyecto tiene marcador de versión válido, ENTONCES EL SISTEMA DEBE usarlo como versión
  base. Es válido si tiene una sola línea con una versión numérica (se toleran BOM, espacios y salto de
  línea final); vacío, varias líneas o no numérico es **malformado**: EL SISTEMA no lo usa, lo informa y
  procede como en RF-3 sin modificarlo hasta terminar.
- RF-3: SI el proyecto no tiene marcador de versión (o está malformado), ENTONCES EL SISTEMA DEBE asumir
  0.4.0 como versión base, decir al usuario que es una suposición y pedir su confirmación antes de
  continuar. SI el usuario no confirma e indica otra versión, EL SISTEMA DEBE usarla; SI no la sabe o
  no responde, EL SISTEMA DEBE tratarla como base sin manifiesto (RF-6) o detenerse sin modificar nada.
- RF-4: SI la versión base es igual a la destino y no hay elementos pendientes registrados, ENTONCES EL
  SISTEMA DEBE informar que el proyecto está al día y no modificar nada.
- RF-5: CUANDO EL SISTEMA conozca la versión base y la destino, EL SISTEMA DEBE asignar a cada archivo
  del kit una sola clase según la tabla de decisión de las Definiciones, comparando huellas contra los
  manifiestos de ambas versiones.
- RF-6: SI no hay manifiesto para la versión base (por ejemplo, versiones anteriores a 0.4.0 o una
  versión sin manifiesto como 0.4.1) o el manifiesto no incluye el archivo, ENTONCES EL SISTEMA DEBE
  clasificar con "B no existe" (nunca como KIT-VIEJO) y avisarlo.
- RF-7: ANTES de modificar cualquier archivo del proyecto, EL SISTEMA DEBE mostrar el plan completo
  agrupado por clase, con la diferencia para cada KIT-VIEJO y cada PERSONALIZADO con cambio del kit, y
  esperar confirmación explícita.
- RF-8: CUANDO el usuario confirme, EL SISTEMA DEBE aplicar únicamente lo confirmado: copiar los NUEVO y
  reemplazar los KIT-VIEJO. Esta es la **única excepción** a "la copia nunca sobrescribe" y solo vale
  para archivos KIT-VIEJO, con diferencia mostrada y confirmación explícita. La confirmación se admite
  por archivo o por grupo de clase; confirmar un grupo cuyas diferencias se mostraron equivale a confirmar
  el reemplazo de cada archivo del grupo. El usuario puede excluir cualquier archivo individual
  marcándolo "mantener".
- RF-9: EL SISTEMA NO DEBE modificar ni borrar los archivos PERSONALIZADO; DEBE mostrar la diferencia del
  kit (si existe) para que el usuario la fusione a mano o la deje como "mantener".
- RF-10: EL SISTEMA NO DEBE borrar los archivos RETIRADO; DEBE informarlos para decisión del usuario.
- RF-11: EL SISTEMA NO DEBE reemplazar los archivos del grupo B; DEBE mostrar los faltantes (según su
  definición) para que el usuario los incorpore a mano o los marque como "mantener".
- RF-12: CUANDO todo elemento del plan haya quedado aplicado o con "mantener" explícito, y no queden
  fusiones pendientes (PERSONALIZADO con cambio del kit sin resolver, faltantes sin incorporar ni
  mantener, conflictos), EL SISTEMA DEBE registrar la versión destino en el marcador. SI quedan
  pendientes, EL SISTEMA NO DEBE cambiar el marcador y DEBE listarlos en la siguiente corrida. SI el
  usuario rechaza todo, el marcador no cambia.
- RF-13: CUANDO se instale el kit por primera vez en un proyecto, EL SISTEMA DEBE dejar el marcador de
  versión registrado con la versión instalada, de modo que quede versionable en Git.
- RF-14: CUANDO la actualización termine, EL SISTEMA DEBE reportar cuántos archivos quedaron: nuevos,
  reemplazados, sin cambios (IGUAL), respetados (PERSONALIZADO sin cambio del kit que mostrar, y los
  marcados "mantener"), pendientes de fusión manual (PERSONALIZADO con cambio del kit sin resolver más
  faltantes del grupo B sin resolver más conflictos) y retirados; los huérfanos se reportan aparte y no
  entran en esas cuentas. EL SISTEMA DEBE además verificar los enlaces hacia las carpetas de cada
  herramienta y, si faltan, proponer corregirlos según las reglas de enlaces del kit, sin ejecutar nada
  sin confirmación.
- RF-15: CUANDO se repita la actualización sobre un proyecto ya actualizado, EL SISTEMA DEBE dejarlo sin
  cambios. CUANDO una actualización haya quedado a medias, EL SISTEMA DEBE poder retomarla: lo ya
  aplicado sale como IGUAL y solo se propone lo restante.
- RF-16: EL SISTEMA NO DEBE crear un commit por su cuenta; SI el proyecto usa Git, DEBE proponer un
  commit aparte con solo los archivos que la actualización tocó y esperar confirmación explícita. SI el
  árbol de trabajo tenía cambios sin guardar antes de empezar, DEBE avisarlo antes de proponer el commit
  para no mezclar cambios ajenos. SI el proyecto no usa Git, el commit no aplica y el resto del
  procedimiento (incluido el marcador) funciona igual.
- RF-17: CUANDO el mantenedor publique una versión del kit, EL KIT DEBE incluir el manifiesto de esa
  versión, de modo que la clasificación no dependa de Git ni de etiquetas. El manifiesto de 0.4.0 se
  genera desde el commit `861bb99`; el de 0.5.0 se genera al cerrar este requerimiento con el estado
  final de la versión. Cada manifiesto es del kit y no se distribuye a los proyectos.
- RF-18: EL KIT DEBE documentar el procedimiento de actualización en un documento propio, junto al de
  instalación, y el documento de instalación DEBE enlazarlo. La actualización no agrega comandos
  nuevos.
- RF-19: EL KIT DEBE definir la huella y su normalización de forma determinista y verificable: el mismo
  archivo con fin de línea CRLF o LF, con o sin BOM o con espacios finales distintos produce la misma
  huella, y el contenido que difiere de verdad produce otra.
- RF-20: CUANDO EL SISTEMA actualice, EL SISTEMA DEBE aplicar a cada archivo el tratamiento de su grupo
  (A, B, C o D) definido en las Definiciones. SI un archivo del grupo B o C fue eliminado por el
  proyecto, ENTONCES EL SISTEMA DEBE ofrecerlo como NUEVO (avisando que requiere personalización
  posterior) o dejarlo como "mantener".
- RF-21: CUANDO el usuario marque "mantener" sobre un archivo o elemento, EL SISTEMA DEBE registrarlo de
  forma persistente en el proyecto y no volver a proponerlo en corridas siguientes de la misma versión
  destino; SI una versión posterior vuelve a cambiar ese elemento, ENTONCES EL SISTEMA DEBE proponerlo
  de nuevo. Esto incluye un NUEVO que el proyecto borró a propósito.
- RF-22: CUANDO el destino de una instalación ya tenga el kit instalado (marcador presente o estructura
  del kit presente), EL SISTEMA DEBE remitir al procedimiento de actualización en lugar de tratarlo
  como proyecto existente sin kit. SI la instalación se hace a mano sin agente, ENTONCES el documento de
  instalación DEBE indicar el paso manual que deja el marcador.
- RF-23: CUANDO un archivo NUEVO tenga como destino una ruta ocupada por algo de otro tipo (carpeta
  donde debería ir un archivo, enlace simbólico, o carpeta real de una herramienta donde debería ir un
  enlace), EL SISTEMA NO DEBE sobrescribirlo ni moverlo: DEBE informarlo como conflicto, contarlo como
  pendiente y tratarlo según las reglas de estado de enlaces del kit.
- RF-24: SI la versión destino no tiene manifiesto, O SI el marcador es mayor que la versión destino
  (que es la del kit disponible), ENTONCES EL SISTEMA DEBE avisarlo y no modificar nada.
- RF-25: SI el entorno no puede calcular huellas, ENTONCES EL SISTEMA DEBE informarlo y no modificar nada,
  sin degradar a suposiciones.

## Requisitos no funcionales
- RNF-1: El procedimiento no incluye scripts ni código ejecutable versionado en el kit; lo ejecuta el
  agente (consistente con el ADR 004: solo Markdown).
- RNF-2: Funciona sin importar sistema operativo ni shell; el agente detecta el entorno.
- RNF-3: Idempotente: aplicar dos veces la misma actualización produce el mismo resultado que una.
- RNF-4: Seguro por defecto: ante duda sobre si un archivo fue personalizado, se trata como
  personalizado.
- RNF-5: El kit sigue siendo genérico: el procedimiento, los manifiestos y el marcador no incluyen datos
  de ningún proyecto ni secretos.
- RNF-6: Archivos en UTF-8 sin BOM y textos en español.
- RNF-7: El procedimiento no depende de la red, de Git ni de etiquetas del repositorio del kit; basta
  con el kit disponible localmente con sus manifiestos. En un proyecto sin Git solo se omite lo propio
  de Git.
- RNF-8: Para calcular huellas el agente puede generar un script temporal fuera del repositorio del
  kit y del proyecto (lo permite el ADR 004); no se versiona ningún script.
- RNF-9: Los manifiestos que listen rutas retiradas no deben romper la validación de "rutas citadas
  existentes" de los chequeos del kit: se documenta que esa regla no aplica a los manifiestos.

## Casos límite
- Proyecto sin marcador instalado con 0.4.0 (caso más común hoy): se asume 0.4.0 y se confirma (RF-3).
- Proyecto instalado con 0.1.0 a 0.3.0 sin marcador: se asume 0.4.0 y lo que difiera se trata como
  PERSONALIZADO sin cambio comparable (RF-6); más fusión manual, pero posible.
- Marcador malformado (vacío, varias líneas, solo espacios, con BOM, no numérico) o con versión sin
  manifiesto (por ejemplo 0.4.1): BOM y espacios se toleran; el resto se informa y se procede como sin
  marcador o con base sin manifiesto (RF-2, RF-3, RF-6).
- Marcador mayor que la versión del kit disponible, o destino menor que el marcador: aviso y sin
  cambios (RF-24).
- Destino sin manifiesto: aviso y sin cambios (RF-24).
- El kit no cambió un archivo entre base y destino y el proyecto lo tiene igual: IGUAL, sin acción.
- El kit no cambió un archivo y el proyecto lo personalizó: PERSONALIZADO sin cambio del kit; se
  respeta y se informa.
- Archivo retirado por el kit y modificado por el proyecto: RETIRADO con aviso; no se borra.
- Archivo extra del proyecto que ningún manifiesto lista (huérfano): se informa aparte, no cuenta en
  las cifras de RF-14.
- Proyecto que borró a propósito un archivo del kit: aparece como NUEVO; el usuario lo marca "mantener"
  y no vuelve a aparecer (RF-21).
- Proyecto que eliminó un archivo de fusión manual: se ofrece como NUEVO o "mantener" (RF-20).
- Proyecto que movió o renombró una carpeta del kit: NUEVO más huérfano; no se adivina el renombre.
- Ruta de destino ocupada por algo de otro tipo: conflicto (RF-23).
- Actualización interrumpida tras reemplazar y antes del marcador: el marcador sigue en la versión
  anterior; al repetir, lo ya reemplazado sale IGUAL y se retoma (RF-15). Lo fusionado a mano que no
  coincide ni con la base ni con el destino aparece PERSONALIZADO hasta que se marque "mantener".
- Aprobación parcial: el marcador no cambia mientras queden pendientes (RF-12).
- Usuario que rechaza todo: no cambia nada, ni el marcador.
- Árbol de trabajo con cambios sin guardar: aviso antes de proponer el commit (RF-16).
- Proyecto sin Git: sin commit, con marcador (RF-16).
- Fin de línea distinto, BOM o espacios finales: no generan falsos PERSONALIZADO (RF-19).
- Nombres que difieren solo en mayúsculas/minúsculas en sistemas que no distinguen: se informa como
  aviso, sin duplicar el archivo. Los permisos de ejecución no cuentan para la clasificación.
- Entorno sin herramienta de huella: se informa y no se modifica nada (RF-25).

## Fuera de alcance
- Fusión automática de archivos de fusión manual o PERSONALIZADO.
- Retroceder (downgrade) a una versión anterior del kit.
- Etiquetar o publicar versiones del kit más allá de entregar el manifiesto de cada una.
- Capa de usuario o configuración personal fuera del proyecto.
- Reconstruir el contenido de las versiones 0.1.0 a 0.3.0.
- Actualizar el propio clon del kit en modo mantenimiento (su parte local de la instalación). Sigue
  fuera de esta spec. La deuda `.docs/003_deuda/001_ActualizarCloneDelKit.md` ya está resuelta en
  `INSTALAR.md` › Modo mantenimiento; no se reabre aquí.
- Un comando nuevo para actualizar.
- Actualizar varios proyectos en una sola ejecución.

## Criterios de finalización
- Un proyecto instalado con 0.4.0 y sin marcador se actualiza a 0.5.0: skills nuevas copiadas y
  conectadas, KIT-VIEJO reemplazados tras confirmación, personalizados intactos, marcador de una línea
  registrado y versionable.
- Una segunda ejecución sobre ese proyecto informa "al día" sin cambios.
- Una instalación nueva deja el marcador registrado, y una aprobación parcial o un rechazo total dejan
  el marcador sin cambiar.
- Existen los manifiestos de 0.4.0 (generado desde `861bb99`) y de 0.5.0; el de 0.5.0 es consistente
  con lo que distribuye el kit en su versión vigente, y esa consistencia es verificable.
- La huella y su normalización están definidas y un archivo con CRLF y otro con LF dan la misma huella.
- El procedimiento está documentado aparte y enlazado desde la instalación (RF-17, RF-18, RF-22).
- La constitución está enmendada (el principio de "la copia nunca sobrescribe" refleja la excepción de
  KIT-VIEJO) y existe el ADR que registra esa decisión.
- `VERSION` y `CHANGELOG.md` de 0.5.0 reflejan este requerimiento.
- El ensayo de instalación sigue sin pisar nada.
- Los chequeos de `AGENTS.md` › Comandos pasan en cada archivo tocado; los manifiestos que listan rutas
  retiradas están exceptuados de "rutas citadas existentes" (RNF-9).
- Se probó en un directorio temporal (nunca en un proyecto real) y funciona sin acceso al historial
  de Git del kit.

## Dudas abiertas
Ninguna.
