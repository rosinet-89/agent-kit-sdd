# 01 — Spec: Falso positivo de SDD-INIT-PENDIENTE tras /sdd-init
Estado: cerrada
Ticket: issue #3 de rosinet-89/agent-kit-sdd

## Contexto y objetivo
El kit decide que un proyecto está "sin personalizar" cuando `AGENTS.md` contiene `SDD-INIT-PENDIENTE`. Esa regla se cita en el paso 1 de «Cada sesión» y en la fila coordinator de «Qué lee cada rol» (`AGENTS.md`), en la skill sdd, en el comando /sdd, en el coordinator, en /sdd-init, en el README y en el `AGENTS.md` de la raíz del repo del kit (dos frases que hoy contienen el literal).

/sdd-init (sección 4, paso 2) borra el comentario HTML con el marcador, pero el texto de la propia regla sigue en `template/AGENTS.md` (paso 1 de «Cada sesión» y fila coordinator). Un agente que compruebe de forma literal (grep o "contiene") encuentra el literal en un proyecto ya personalizado, sugiere /sdd-init otra vez y bloquea los comandos /sdd*.

Objetivo: que, tras /sdd-init, `AGENTS.md` ya no contenga el literal, que la regla se siga entendiendo, y que un chequeo del kit lo vigile.

### Antecedentes
- 004_ModeloYPermisosAgentes (02 §3.3 y 04 F1.1) detectó este falso positivo en el repo raíz y lo esquivó con una copia; no lo resolvió.
- 003 y 007 tocaron /sdd-init, sin abordar esto.
- No hay deuda abierta relacionada. Versión actual del kit: 0.5.0. Este cambio viaja en la 0.6.0, compartida con los cambios paralelos 0008 y 0009.

## Usuarios y roles
- Persona que instala el kit y ejecuta /sdd-init.
- Agentes (coordinator y resto) que evalúan el estado de instalación al arrancar sesión.
- Mantenedor del kit, que ejecuta `validar-estructura`.

## Historias de usuario
- HU-1. Como persona que acaba de ejecutar /sdd-init, quiero que los comandos /sdd* funcionen sin que se me pida volver a ejecutarlo, para empezar a trabajar.
- HU-2. Como agente, quiero una descripción del marcador que se entienda sin contener el literal completo, para no confundir la regla con el marcador.
- HU-3. Como mantenedor, quiero que `validar-estructura` detecte esta regresión, para que no vuelva a entrar. Excepción declarada: E21 cubre proyectos instalados y la copia de prueba; la raíz del clon (sin `.kit-version`, que no se le da) se cubre solo con el grep manual de RF-1 en la validación de esta spec.
- HU-4. Como responsable de un proyecto ya instalado, quiero saber que debo incorporar el cambio a mano, para corregir mi `AGENTS.md`.

## Definiciones
- Literal: la cadena completa `SDD-INIT-PENDIENTE`.
- Marcador real: el comentario HTML de `template/AGENTS.md`, en la primera línea no vacía tras el título (la línea 1; la plantilla deja una línea en blanco entre ambos), que /sdd-init borra («<!-- SDD-INIT-PENDIENTE: este marcador lo quita /sdd-init al terminar la personalización. -->»). Es la fuente normativa del marcador; se cita por su texto, no por número de línea.
- Cadena del marcador: copia literal del marcador real en `validar-estructura.md` (y `sdd-estructura.md` si lo muestra), contra la que E21 compara. Cadena copiada ausente o alterada = FALLA. Si `validar-estructura.md` y `sdd-estructura.md` difieren, manda `validar-estructura.md`, y la diferencia es FALLA del kit cuando ambos existen con `template/`. Existe porque un proyecto instalado no necesita `template/`. Es una excepción explícita al principio 3 de la constitución («Cada regla vive en una sola fuente»): el marcador consta en 3 sitios (comentario de la plantilla, `validar-estructura.md` y, si lo muestra, `sdd-estructura.md`), y E21 es el control cruzado que verifica que coinciden (RF-6).
- Comparación del marcador: se ignora solo el fin de línea (CRLF o LF) y los espacios o tabuladores finales. Cualquier otro cambio (espacios internos o iniciales, mayúsculas, texto distinto, comentario multilínea, BOM, `\r` suelto dentro de la línea) hace que no sea el marcador exacto.
- Marcador válido: la línea de `AGENTS.md` que, tras la comparación anterior, coincide con la cadena del marcador, en la primera línea no vacía tras el título y una sola vez. Un segundo comentario idéntico en otra posición, o repetido, no es marcador.
- Regla del cero: el literal en `AGENTS.md` solo puede aparecer dentro del marcador válido. Plantilla y proyecto pendiente: aparece exactamente una vez, dentro del marcador. Proyecto personalizado (tras /sdd-init) y raíz del repo del kit: 0 veces. Rige solo para `AGENTS.md`, y expresamente para `template/AGENTS.md` (literal una sola vez, dentro del marcador) y para el `AGENTS.md` de la raíz (0 veces). Es una regla propia, distinta de las piezas exentas.
- Fuente única de la regla de detección: el paso 1 de «Cada sesión» de `AGENTS.md`. Describe el marcador sin escribir el literal y remite al comentario de `template/AGENTS.md` como definición. Las demás piezas remiten al paso 1.
- Piezas exentas: las piezas distintas de `AGENTS.md` (`template/AGENTS.md` y el de la raíz no lo son: los rige la regla del cero) a las que la regla del cero no aplica, incluidas las réplicas de `template/.agents/`; pueden nombrar el literal (también en enlaces o rutas), pero no están obligadas a hacerlo. Lista en Alcance; fuera de ella no se permite.
- Cita entre archivos: otros archivos se citan por la frase o encabezado, no por número de línea.
- Grupo B: archivos personalizables que el proyecto incorpora a mano (ver `ACTUALIZAR.md`).

## Requisitos funcionales (EARS)
- RF-1: EL SISTEMA cumplirá la regla del cero en `AGENTS.md` en cuatro casos: plantilla y proyecto pendiente (el literal exactamente una vez, dentro del marcador válido), proyecto personalizado tras /sdd-init (0) y raíz del repo del kit, ya personalizada y sin `.kit-version` (0). La raíz la vigila el grep de este requisito, no E21.
- RF-2: EL SISTEMA describirá en el paso 1 de «Cada sesión» de `template/AGENTS.md` el marcador de instalación pendiente sin escribir el literal, remitiendo al comentario marcador de esa plantilla como definición y aclarando que solo ese comentario exacto (que /sdd-init borra) cuenta y que la propia descripción no es marcador. La fila coordinator de «Qué lee cada rol» remite al paso 1 sin literal.
- RF-3: CUANDO `AGENTS.md` contiene el marcador válido, EL SISTEMA seguirá detectando el proyecto como sin personalizar y sugiriendo /sdd-init.
- RF-4: CUANDO `AGENTS.md` no contiene el marcador válido, EL SISTEMA permitirá los comandos /sdd* sin sugerir /sdd-init.
- RF-5: EL SISTEMA hará que cada pieza que cita la regla (skill sdd, /sdd, coordinator, /sdd-init, README y fila coordinator) remita al paso 1 de «Cada sesión» de `AGENTS.md` sin definir otra regla de detección. «Define otra regla» = enuncia condiciones de reconocimiento distintas a las del paso 1 (otra posición, otro texto, «contiene» en lugar de coincidencia con el marcador válido). Nombrar el literal solo para remitir al paso 1 no es definir otra regla. Verificable: en cada pieza hay una frase que remite al paso 1 y ninguna condición de reconocimiento que el paso 1 no contenga. (Constitución, principio 3.)
- RF-6: CUANDO se ejecute `validar-estructura` en un proyecto con `.kit-version`, EL SISTEMA marcará FALLA en el chequeo nuevo E21 si:
  - `AGENTS.md` no existe;
  - `AGENTS.md` contiene el literal fuera del marcador válido (incluye marcador repetido, segundo comentario idéntico en otra posición y variantes del comentario);
  - la cadena del marcador falta o está alterada en `validar-estructura.md`;
  - `template/` existe y la cadena copiada en `validar-estructura.md` no coincide con el comentario marcador de `template/AGENTS.md` (comparación definida arriba), o `sdd-estructura.md` la muestra y difiere de la de `validar-estructura.md`. Esta tercera FALLA se ejerce en la copia de prueba con `template/` presente.
  
  Si `template/` no existe (proyecto instalado), E21 solo compara `AGENTS.md` contra la cadena copiada. E21 no modifica E20. No se permite citar el literal en `AGENTS.md`; quien lo necesite lo describe sin el literal. Quien cambie el marcador actualiza la cadena copiada en el mismo cambio. Reglas de borde: `AGENTS.md` vacío sin literal = sin marcador, sin FALLA; un enlace simbólico se lee como el archivo al que apunta.
- RF-6a: EL SISTEMA actualizará `.agents/commands/sdd-estructura.md` (hoy dice E1 a E20) y `.agents/validar-estructura.md` para incluir E21, con sus réplicas idénticas en `template/.agents/`.
- RF-7: EL SISTEMA redactará todo texto nuevo de este cambio sin reintroducir el literal en `template/AGENTS.md` fuera del marcador ni en el `AGENTS.md` de la raíz (reformulando allí las dos frases que hoy lo contienen para que el grep dé 0). Las piezas exentas pueden nombrar el literal.
- RF-8: CUANDO se publique el cambio, EL SISTEMA subirá `VersionAgentKitSdd` a 0.6.0 y añadirá una nota en `ChangelogAgentKitSdd.md` indicando que los proyectos existentes lo incorporan a mano (grupo B). La nota va en el Changelog y no en `ACTUALIZAR.md`, que describe el procedimiento general. El manifiesto `manifiestos/0.6.0.md` y la entrada de Changelog 0.6.0 se crean UNA sola vez, al final, tras cerrar 0009; cada spec (0008, 0009, 0010) aporta sus líneas y esta no crea el manifiesto por su cuenta. Orden de fusión: 0008, luego 0010, luego 0009.

> Aclaración (2026-11-01, aprobada por el usuario): el bump único de `VersionAgentKitSdd` a 0.6.0 lo hace la F6 de 0009, junto con el Changelog y el manifiesto 0.6.0. Esta spec no sube la versión: solo deja sus líneas de grupo B en su `05_avance.md`. Además, en las piezas nuevas las rutas que no existen en un proyecto instalado (p. ej. la plantilla `template/AGENTS.md`, `.agents/.kit-version`, `template/`) van sin comillas inversas. No cambia ningún RF ni el Estado.

## Dependencias
- 0008: precede en el orden de fusión (RF-8).
- 0009: crea al final el manifiesto `manifiestos/0.6.0.md` y la entrada de Changelog 0.6.0 (RF-8). Dependencia ordinaria de versión; no bloquea cerrar esta spec.

## Requisitos no funcionales
- `template/` sigue genérico, sin stack (principio 1). Cada regla en una sola fuente (principio 3): la regla de detección tiene como única fuente el paso 1 de «Cada sesión»; las demás piezas solo remiten a él. Única excepción: la cadena del marcador en 3 sitios, con E21 como control cruzado (ver Definiciones).
- UTF-8 sin BOM, en español.
- Cambiar `template/` exige versión y manifiesto nuevos (0.6.0, `manifiestos/0.6.0.md`; ver Dependencias).
- Las réplicas en `template/.agents/` quedan idénticas a las piezas de `.agents/` (salvo napkin y skill caveman).

## Casos límite
- La descripción nueva de la regla no reintroduce el literal en ninguna línea de `AGENTS.md`, tampoco en bloques de código ni en enlaces o rutas. En la plantilla solo queda el marcador; tras /sdd-init y en la raíz, el grep da 0.
- /sdd-init necesita nombrar lo que borra, y E21 lo que busca: es lícito porque son piezas exentas.
- El marcador exacto debe seguir detectándose (no vale una comprobación tan laxa que lo ignore). Una variante (espacios internos o iniciales, mayúsculas, otro texto, comentario multilínea, BOM, `\r` suelto) no es el marcador exacto y, si contiene el literal, E21 da FALLA.
- CRLF o espacios finales tras el comentario: se ignoran y el marcador sigue siendo válido.
- Segundo comentario idéntico en otra posición, o marcador repetido: E21 da FALLA.
- Marcador de `template/AGENTS.md` cambiado por un tercero: con `template/` presente (copia de prueba), E21 da FALLA por desincronía con la cadena copiada. En un proyecto instalado no se detecta, porque no hay plantilla con la que comparar.
- `AGENTS.md` inexistente: E21 da FALLA. `AGENTS.md` vacío sin literal: sin marcador, sin FALLA.
- Proyecto instalado y personalizado con una versión anterior: su `AGENTS.md` conserva el falso positivo, E21 da FALLA y es el comportamiento esperado hasta que incorpore el cambio a mano.
- Repo raíz (clon de mantenimiento): ya personalizado, sin marcador, sin `.kit-version`; dos frases contienen hoy el literal y se reformulan (RF-1, RF-7). No se le da `.kit-version`; E21 no aplica y lo vigila solo el grep manual de RF-1 en la validación de esta spec (excepción de HU-3).
- Menores diferidos al plan: redacción exacta de la frase de remisión en cada pieza, orden de los chequeos dentro de E21 y texto del mensaje de FALLA.
- La carpeta del feature conserva 4 dígitos (0010) por decisión del usuario.
- Otros agentes pueden editar `template/AGENTS.md` en paralelo; conviene integrar sin pisar.

## Alcance
- `template/AGENTS.md` (paso 1 de «Cada sesión» y fila coordinator) y las piezas que citan la regla (RF-5).
- Piezas exentas: `/sdd-init`, E21 y su texto (con la cadena del marcador) en `validar-estructura.md` y `sdd-estructura.md`, README, skill sdd, comando /sdd, coordinator y sus réplicas en `template/.agents/`, `ChangelogAgentKitSdd.md`, manifiestos, la nota de grupo B y los historiales de `.docs/`. Todas remiten al paso 1 y no definen otra regla.
- `ACTUALIZAR.md` NO es pieza exenta: hoy no cita el literal y esta spec no lo añade. Si hiciera falta, se amplía la lista con /sdd-change.
- `AGENTS.md` de la raíz del repo del kit (las dos frases con el literal), sin marcador.
- E21 nuevo en `.agents/commands/sdd-estructura.md` y `.agents/validar-estructura.md`, con réplicas en `template/.agents/`.
- (Ver Aclaración tras RF-8: la versión la sube 0009.) `VersionAgentKitSdd` a 0.6.0 y nota de grupo B en `ChangelogAgentKitSdd.md`. El manifiesto 0.6.0 y el Changelog 0.6.0 se crean al final, tras 0009 (no los crea esta spec).

## Fuera de alcance
- Cambiar el mecanismo de detección (p. ej. un archivo de estado en lugar del marcador).
- Cambiar el resto de pasos de /sdd-init.
- Migración automática de proyectos ya instalados.

## Criterios de finalización
- Marcador válido, verificable por posición con dos comandos (viven aquí, no en `AGENTS.md`): el `grep` solo cuenta ocurrencias, con `grep -o 'SDD-INIT-PENDIENTE' AGENTS.md | wc -l`; la posición y el texto se comprueban extrayendo la primera línea no vacía tras el título y comparándola con la cadena: `sed 's/\r$//' AGENTS.md | awk 'f&&NF{print;exit} /^# /{f=1}' | sed 's/[[:space:]]*$//'` debe ser igual a la cadena del marcador. Es marcador válido si la cuenta es 1 y la comparación coincide; si la cuenta es 0 no hay marcador, y en cualquier otro caso con el literal hay FALLA.
- Regla del cero: en el ensayo de instalación, tras /sdd-init `grep SDD-INIT-PENDIENTE AGENTS.md` da 0; en el `AGENTS.md` de la raíz también da 0 (grep manual, única vigilancia de la raíz). En la plantilla y en un proyecto pendiente da 1 (solo el marcador válido).
- Con el marcador válido presente se detecta; sin él, no. Un CRLF o espacios finales tras el comentario no lo invalidan.
- El ensayo de instalación se hace en una copia de prueba creada con `mktemp -d`, fuera del repo, sin pisar nada (principio 4). En esa copia se crea `.kit-version` a mano antes de correr E21, y se comprueba E21 sin el literal fuera del marcador (pasa) y con el literal fuera del marcador (FALLA). Se ejecuta con `template/` presente (comparación contra la plantilla) y sin `template/` (solo contra la cadena copiada).
- Con `template/` presente, E21 da FALLA si la cadena copiada en `validar-estructura.md` (o `sdd-estructura.md`) y el comentario de `template/AGENTS.md` se desincronizan.
- La regla del paso 1 y la fila coordinator se entienden por lectura.
- `validar-estructura` incluye E21 (RF-6), documentado en `sdd-estructura.md` y `validar-estructura.md` con sus réplicas; da FALLA con literal fuera del marcador válido, con el marcador repetido o sin `AGENTS.md`, y pasa en el caso normal.
- Cada pieza de RF-5 contiene una frase que remite al paso 1 y ninguna condición de reconocimiento ajena a él.
- `VersionAgentKitSdd` en 0.6.0 y nota de grupo B aportada al Changelog 0.6.0.
- Comandos de `AGENTS.md` › Comandos ejecutados sobre los archivos tocados.

## Decisiones
- Del usuario: E21 reconoce solo el marcador exacto; no se permite citar el literal en el `AGENTS.md` de un proyecto; sin `AGENTS.md`, E21 da FALLA; E21 solo con `.kit-version`.
- Del usuario: regla del cero unificada (ver Definiciones), solo para `AGENTS.md`, con piezas exentas listadas.
- Del usuario: la fuente normativa del marcador es el comentario de `template/AGENTS.md`; el paso 1 lo describe sin literal y remite a él. E21 compara contra la cadena copiada y, si `template/` existe, también contra la plantilla; E20 no interviene. La cadena en 3 sitios es excepción explícita al principio 3, con E21 como control cruzado.
- Del usuario: fuente única de la regla de detección = paso 1 de «Cada sesión»; versión 0.6.0 con manifiesto y Changelog creados una sola vez al final, tras 0009; la raíz se vigila con el grep de RF-1.
- Del planificador (a confirmar al aprobar): CRLF/LF y espacios finales ignorados, cualquier otra variante no es marcador; segundo comentario idéntico en otra posición da FALLA; `ACTUALIZAR.md` fuera de las piezas exentas; «define otra regla» = condiciones distintas a las del paso 1; la dependencia de 0009 no bloquea.
- Decisión del usuario: un enlace simbólico se lee como su destino.

## Dudas abiertas
- Ninguna.
