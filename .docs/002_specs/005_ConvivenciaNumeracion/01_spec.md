# 01 — Spec: Convivencia de la numeración de 3 y 4 dígitos
Estado: implementada
Ticket: sin ticket

## Contexto y objetivo
En 0.5.0 (sin publicar) el consecutivo de specs, deudas y ADR de un proyecto pasa a 4 dígitos (`0001`). Las carpetas de sección de `.docs/` y el clon del kit siguen en 3. Un proyecto instalado con 0.4.0 ya tiene ítems de 3 dígitos (`017_…`, `003_…`) y, al actualizar, recibe comandos, skills y `AGENTS.md` que describen 4 dígitos. Ninguna pieza dice qué ancho usa un ítem nuevo cuando ya existen ítems de otro ancho, cómo se compara un número con otro, ni qué hacer con los README de `.docs/` del proyecto, que el procedimiento de actualización no reemplaza (grupo C) y que conservan la regla de 0.4.0. El `CHANGELOG.md` de 0.5.0 lo declara como no definido.

Origen: deuda `004_ConvivenciaNumeracion3y4Digitos`.

Objetivo: que un proyecto que actualiza a 0.5.0 sepa qué número usar, no renumere por error y no vea reglas contradictorias entre sus piezas.

Decisiones ya tomadas (registradas en `05_avance.md`): un ítem nuevo conserva el ancho de su carpeta y se compara por valor numérico; la fuente única es la sección « Numeración única » del índice de specs; con anchos mezclados manda el ancho del ítem de mayor valor; E12 informa la mezcla sin fallar y el duplicado por valor sigue siendo fallo; una carpeta vacía toma el ancho de las otras; la actualización detecta por nombres de carpeta, sin leer el contenido de ningún README, y ofrece el texto de la regla sin escribir sin confirmación; las piezas llevan una regla mínima de una frase y remiten a la fuente.

## Usuarios y roles
- Persona que mantiene un proyecto con el kit instalado y lo actualiza a 0.5.0.
- Persona que arranca un proyecto nuevo con 0.5.0.
- Agentes (coordinator, planner, implementer, reviewer) que asignan números, revisan duplicados y preparan commits.
- Mantenedor del kit (clon del kit, ancho de 3 dígitos).

## Historias de usuario
- HU-1. Como persona que mantiene un proyecto actualizado, quiero que el agente sepa con qué ancho numerar un ítem nuevo cuando ya hay carpetas en otro ancho, para no elegir un número ambiguo.
- HU-2. Como persona que mantiene un proyecto actualizado, quiero que ninguna carpeta existente se renumere por cambiar la regla, para no romper las citas que otras specs, deudas y ADR hacen de ella.
- HU-3. Como agente, quiero una sola fuente de la regla de numeración, con una regla mínima de una frase en cada pieza, para decidir igual aunque el índice del proyecto sea el de 0.4.0.
- HU-4. Como persona que actualiza, quiero que el procedimiento me ofrezca, sin leer mis README ni escribir en ellos por su cuenta, el texto de la regla para los tres índices (specs, deuda y ADR), para decidir si lo incorporo o lo mantengo como está.
- HU-5. Como agente que revisa estructura y prepara commits, quiero comparar prefijos por valor numérico, para detectar duplicados aunque difiera el ancho (`017` y `0017`).
- HU-6. Como mantenedor del kit, quiero que la regla se compruebe en un proyecto simulado con resultados observables y que las verificaciones del arnés sigan vigentes, para publicar 0.5.0 sin la frase «no está definida todavía».

## Definiciones
- **Valor numérico de un prefijo:** el entero que resulta de leer sus dígitos sin ceros a la izquierda (`017` y `0017` valen 17).
- **Ítem:** carpeta de `.docs/002_specs/`, o archivo de `.docs/003_deuda/` o `.docs/004_adr/`, cuyo nombre empieza con un prefijo de dígitos seguido de `_`. Los archivos sin prefijo numérico (`README.md`, marcadores de carpeta vacía y similares) no son ítems: no cuentan para el siguiente libre, para el ancho ni para decidir si una carpeta está vacía. Las carpetas de sección de `.docs/` tampoco son ítems.
- **Ancho válido de un prefijo:** 3 o 4 dígitos, o los dígitos que el valor necesita sin ceros de relleno cuando supera 9999. Un prefijo de 1 o 2 dígitos, o con ceros de relleno por encima de ese criterio, tiene ancho inválido.
- **Ancho de una carpeta de ítems:** el ancho del ítem de mayor valor numérico entre los que tienen ancho válido.
- **Anchos mezclados:** una misma carpeta de ítems con ítems válidos de dos o más anchos, incluido el caso en que el desbordamiento (RF-5) añade un ítem de 4 dígitos a una carpeta de 3.
- **Convivencia:** la regla que dice qué ancho usa un ítem nuevo y cómo se compara cuando hay ítems de otro ancho.
- **Proyecto actualizado:** proyecto instalado con una versión anterior a 0.5.0 que se lleva a 0.5.0.
- **Aviso de numeración:** bloque del reporte de la actualización que informa el ancho detectado por carpeta y ofrece el texto de la regla (RF-16 y RF-17).
- **AVISO (E12):** tercer estado de un chequeo, junto a `OK` y `FALLA`: informa un hecho que no es defecto y no cambia el resultado global.

## Requisitos funcionales (EARS)
- RF-1: CUANDO se asigna número a un ítem nuevo en un proyecto y su carpeta de ítems ya tiene ítems, EL SISTEMA usa el ancho de esa carpeta (definición arriba), sin pasar a 4 dígitos por su cuenta. Cada carpeta de ítems conserva su propio ancho: un ADR nuevo en una carpeta de ADR de 3 dígitos usa 3 aunque las specs ya estén en 4. RF-5 (desbordamiento) y RF-7 (duplicado) tienen precedencia sobre este requisito.
- RF-2: CUANDO se asigna número a un ítem nuevo en un proyecto y su carpeta de ítems no tiene ítems, EL SISTEMA usa el ancho del ítem de mayor valor numérico entre las otras carpetas de ítems con ítems; si dos de ellas empatan en valor, manda el orden specs, deuda, ADR; y si ninguna tiene ítems (proyecto nuevo), usa 4 dígitos (`0001`).
- RF-3: CUANDO la copia es el clon del kit, EL SISTEMA numera con el ancho que declara su índice (3 dígitos) y no cambia con esta spec.
- RF-4: CUANDO se calcula el siguiente libre, EL SISTEMA toma el mayor prefijo por valor numérico entre los ítems de la copia local y de la rama remota ya descargada, suma 1 y aplica el ancho que corresponda según RF-1 a RF-3, sin comparar los prefijos como texto. Si hay un duplicado sin resolver (RF-7), lo avisa antes de asignar.
- RF-5: CUANDO el siguiente valor necesita más dígitos que el ancho de la carpeta (por ejemplo, 1000 en una carpeta de 3 dígitos), EL SISTEMA usa los dígitos que ese valor necesita, sin rellenar ni truncar. Ese requisito gana sobre RF-1; después de asignarlo, el ancho de la carpeta pasa a ser el del ítem de mayor valor (4) y la carpeta cuenta como de anchos mezclados.
- RF-6: CUANDO existen huecos en la numeración, EL SISTEMA no los rellena ni reutiliza un número ya usado, cualquiera sea el ancho.
- RF-7: CUANDO dos ítems de la misma carpeta comparten valor numérico aunque difieran en ancho (por ejemplo, `017_A` y `0017_B`, incluso en dos ramas), EL SISTEMA los trata como duplicado: conserva el número del que ya está en la rama remota, propone para el otro el siguiente libre según RF-4 y no renombra sin confirmación del usuario. Si ninguno está publicado, pregunta cuál conservar.
- RF-8: CUANDO los dos ítems de un duplicado por valor tienen el mismo nombre después del prefijo (por ejemplo, `017_A` en la rama remota y `0017_A` en la copia local), EL SISTEMA lo informa como probable mismo ítem con el prefijo escrito en otro ancho y propone renombrar el local al nombre publicado, con confirmación; solo si el usuario indica que son ítems distintos aplica RF-7.
- RF-9: CUANDO no hay rama remota ni upstream, EL SISTEMA calcula el siguiente libre solo con la copia local (RF-4) y, si hay un duplicado, pregunta cuál conservar.
- RF-10: CUANDO cambia el ancho de la regla o se actualiza el kit, EL SISTEMA no renumera ni propone renumerar ítems existentes por esa razón; solo propone renombrar para resolver un duplicado (RF-7, RF-8), con confirmación, y entonces actualiza el índice y cada referencia al nombre viejo.
- RF-11: CUANDO un documento cita un ítem por su número corto (`017`), EL SISTEMA distingue tres situaciones: la cita al ítem que conservó el número sigue válida; la cita al ítem renombrado por un duplicado se actualiza como parte de RF-10; y mientras `017` identifique a dos ítems sin resolver, la cita es ambigua y se usa el nombre completo de la carpeta.
- RF-12: EL SISTEMA define la regla de convivencia (ancho, siguiente libre, comparación por valor, duplicados) en una sola fuente, la sección « Numeración única » del índice de specs, y las demás piezas remiten a ella para el detalle. Las piezas afectadas son: `template/AGENTS.md` (« Deuda técnica », « Formato y numeración » y la nota de la skill `sdd`), la skill `sdd` (la línea de ancho y su ejemplo), la skill `adr`, la skill `git-commit` (Fase 2), la skill `migracion-bd` (su descripción y su encabezado, que hoy mezclan `NNN` y `NNNN`), el comando `sdd-spec`, los `argument-hint` `<NNNN_…>` de los comandos `sdd-*`, `.agents/README.md` (árbol de carpetas), los índices de deuda y de ADR (que mezclan `NNN` y `NNNN`) y el chequeo E12. Un placeholder o un ejemplo se escribe sin afirmar un ancho como regla; la notación única se decide en el plan.
- RF-13: EL SISTEMA incluye en `sdd-spec`, `git-commit`, la skill `adr` y las demás piezas del grupo A o B que asignan o revisan números una regla mínima de una frase (« un ítem nuevo conserva el ancho de su carpeta ») además de la remisión, para que funcionen aunque el índice del proyecto sea el de 0.4.0. Es una excepción acotada a RF-12: esa frase es la única repetición permitida y no lleva cifras de ancho.
- RF-14: CUANDO el chequeo E12 revisa una copia, EL SISTEMA compara los prefijos por valor numérico y anota: `FALLA` ante un duplicado por valor aunque difiera el ancho; `FALLA` ante un ítem de ancho inválido; en el clon del kit, `FALLA` ante un ítem de ancho distinto del que declara su índice; y `AVISO` ante una carpeta de un proyecto con anchos mezclados válidos y sin duplicados. Los `AVISO` se listan aparte de la tabla de fallos, con la carpeta y el hecho; no impiden el cierre `estructura OK`, que indica cuántos avisos hubo, y no alteran E20. El texto de E12 deja de exigir « 4 dígitos en un proyecto » y remite a la fuente única.
- RF-15: CUANDO la skill `git-commit` revisa la numeración única antes de un commit, EL SISTEMA compara por valor numérico contra la copia local y la rama remota descargada, con la misma regla que RF-7 y RF-8.
- RF-16: CUANDO `ACTUALIZAR.md` actualiza un proyecto a 0.5.0 y alguna de las carpetas `.docs/002_specs/`, `.docs/003_deuda/` o `.docs/004_adr/` tiene al menos un ítem de 3 dígitos (decidido solo por los nombres de carpeta y archivo), EL SISTEMA añade al reporte un aviso de numeración, aparte de las cuentas existentes, con: el ancho detectado en cada carpeta, si hay anchos mezclados y la constancia de que no se renumera nada. El aviso se muestra en cada actualización hasta que el usuario lo marque «mantener» en el registro que ya existe; sin marcador de versión el comportamiento es el mismo. Si ninguna carpeta tiene ítems de 3 dígitos, no hay aviso.
- RF-17: CUANDO se muestra el aviso de numeración, EL SISTEMA ofrece el texto de la regla para los tres índices (specs, deuda y ADR) sin leer su contenido y sin compararlos, y solo escribe en un índice si el usuario lo confirma. El aviso no cuenta como pendiente, no bloquea el marcador de versión y no cambia la definición del grupo C. La decisión «mantener» acredita que el usuario ya lo vio y lo oculta mientras no cambie el texto ofrecido. `ACTUALIZAR.md` remite a la fuente única y no nombra un ancho absoluto fuera de ella.
- RF-18: CUANDO se cambia el `CHANGELOG.md` de 0.5.0, EL SISTEMA reemplaza la frase « esa regla de convivencia no está definida todavía » y la línea que afirma que el consecutivo « pasa a 4 dígitos » y que lo aplican los comandos, las skills y E12, de modo que describan la regla de convivencia (proyecto nuevo en 4; proyecto con ítems conserva el ancho de cada carpeta).
- RF-19: CUANDO se cambia cualquier archivo de `template/`, EL SISTEMA regenera `manifiestos/0.5.0.md` una sola vez, al final, por quien cierre segundo la implementación frente a la `004_ModeloYPermisosAgentes`, sobre el contenido vigente de `template/`, y repite E20 en el orden de `AGENTS.md` › Comandos. Los cambios a `template/AGENTS.md` se ubican por el título de la sección (« Formato y numeración »), no por línea, de modo que valgan si la `004` reordena secciones. `ACTUALIZAR.md` vive solo en la raíz del kit, fuera de `template/`, y su cambio no entra al manifiesto.
- RF-20: CUANDO se valida esta spec, EL SISTEMA demuestra la regla en un proyecto simulado creado con `mktemp -d` (nunca en el kit real ni en un proyecto real), con los resultados observables de los Criterios de finalización, y deja el resultado en `05_avance.md`.

## Requisitos no funcionales
- RNF-1 (sin scripts): solo Markdown; no se agrega código ejecutable ni scripts versionados (ADR 004). La regla es prosa que ejecuta un agente; su comprobación usa comandos de `AGENTS.md` › Comandos y listados de la copia simulada.
- RNF-2 (una fuente, con excepción y limitación): la regla vive en un solo lugar (principio 3 de la constitución, ADR 009). La única repetición permitida es la frase mínima de RF-13. Limitación conocida: la fuente única es un archivo del grupo C que el kit no mantiene en un proyecto ya instalado, y puede ser el de 0.4.0; la mitigación es el aviso con el texto ofrecido (RF-16, RF-17) y la frase mínima de cada pieza (RF-13).
- RNF-3 (genérico): nada propio de un proyecto entra a `template/` (principio 1); los ejemplos usan nombres neutros.
- RNF-4 (no destructivo): la actualización no sobrescribe el grupo C sin confirmación (ADR 007, principio 5), no lee su contenido para esta regla y nada se renombra sin confirmación.
- RNF-5 (lectura acotada): la regla no añade lectura obligatoria a los roles; las piezas remiten por nombre de sección.
- RNF-6 (compatibilidad con la 004): no modifica ni contradice la `004_ModeloYPermisosAgentes`. Las fases de documentación no tocan `template/`; la implementación va en serie, y `CHANGELOG.md`, `template/AGENTS.md` y el manifiesto se editan partiendo del contenido vigente (RF-19).
- RNF-7 (codificación): documentos en español, UTF-8 sin BOM, `name` de frontmatter igual a su carpeta, rutas citadas existentes.
- RNF-8 (verificable): cada RF se comprueba con una lectura, un `ls`/`grep` o un caso del proyecto simulado.

## Casos límite
1. **Proyecto nuevo:** sin ítems en ninguna carpeta, todo en 4 dígitos desde `0001` (RF-2).
2. **Proyecto de 0.4.0 con carpetas en 3:** el siguiente ítem sigue en 3 (RF-1); no se renombra nada (RF-10); el reporte lo informa (RF-16).
3. **Proyecto con anchos ya mezclados** (`017_` y `0018_`): el ancho lo marca el ítem de mayor valor (RF-1); el orden alfabético del listado no coincide con el numérico y eso no se corrige renumerando; E12 lo anota como `AVISO` (RF-14).
4. **Proyecto con huecos** (`001`, `002`, `005`): el siguiente es el 6; no se rellenan el 3 ni el 4 (RF-6).
5. **Dos ramas con el mismo número y distinto ancho** (`017_A` en una, `0017_B` en otra): duplicado por valor; conserva el publicado; el otro pasa al siguiente libre (RF-7).
6. **Mismo nombre con distinto ancho** (`017_A` remoto y `0017_A` local): probable mismo ítem; se propone renombrar el local al nombre publicado (RF-8).
7. **Carpeta de 3 dígitos que llega a 999:** el siguiente es `1000` (RF-5); la carpeta queda con anchos mezclados, que E12 solo anota como `AVISO`.
8. **Carpeta de ítems vacía en un proyecto actualizado** (por ejemplo, sin ADR): usa el ancho del ítem de mayor valor entre las otras carpetas (RF-2).
9. **Carpetas con anchos distintos entre sí:** una carpeta de ADR en 3 dígitos y specs en 4 que genera un ADR nuevo usa 3, porque la carpeta de ADR tiene ítems (RF-1).
10. **Clon del kit:** 3 dígitos, sin cambio (RF-3); un ítem de otro ancho es `FALLA` en E12 (RF-14).
11. **Índices del proyecto con la regla de 0.4.0** (grupo C no reemplazado): el aviso aparece por los nombres de carpeta y ofrece el texto de la regla para los tres índices sin escribir (RF-16, RF-17); mientras no se incorpore, las piezas aplican su frase mínima (RF-13).
12. **Proyecto de 0.4.0 sin ningún ítem:** no hay aviso (RF-16); las piezas aplican RF-2 (4 dígitos) y su frase mínima. Es una limitación aceptada: sin ítems no hay conflicto observable.
13. **Proyecto sin Git o sin rama remota:** RF-9.
14. **Referencia corta a un ítem renombrado o ambigua:** RF-11.
15. **Archivos sin prefijo numérico en la carpeta** (`README.md`): no son ítems; una carpeta que solo los contiene está vacía (RF-2).
16. **Prefijos de 1, 2 o 5 o más dígitos con ceros de relleno** (`7_`, `07_`, `00017_`): ancho inválido, `FALLA` en E12 con propuesta de renombrar con confirmación; su valor sí cuenta para el siguiente libre.

## Fuera de alcance
- Renumerar ítems existentes, de forma automática o con una herramienta de migración.
- Cambiar el ancho del clon del kit o de las carpetas de sección de `.docs/`.
- Cambiar la clasificación en grupos A/B/C/D de `ACTUALIZAR.md`, hacer que el grupo C se compare o se reemplace, o leer el contenido de un archivo del grupo C para esta regla.
- Subir la versión (se queda en 0.5.0 sin publicar) y publicar.
- Cualquier cambio del modelo, permisos o roles de los agentes (`004_ModeloYPermisosAgentes`).
- Ordenar el listado de carpetas por valor numérico.
- Mover la fuente única a un archivo nuevo del grupo A (alternativa descartada).

## Criterios de finalización
- Los RF-1 a RF-20 se cumplen y están marcados en `05_avance.md` con su evidencia.
- La regla de convivencia vive en una sola fuente; las piezas de RF-12 solo remiten y llevan la frase mínima de RF-13; ninguna afirma un ancho absoluto que la contradiga.
- E12 (con el estado `AVISO`) y la revisión de duplicados de `git-commit` comparan por valor numérico.
- `ACTUALIZAR.md` y `CHANGELOG.md` reflejan la regla; la frase « no está definida todavía » ya no aparece.
- La prueba en `mktemp -d` acredita estos resultados, cada uno con el listado (`ls`) de la copia simulada antes y después, que debe ser idéntico salvo el ítem nuevo, o con el `grep` que lo muestra:
  - Proyecto nuevo, sin ítems: propone `0001`.
  - Carpeta con `001`, `002`, `017`: propone `018`.
  - Carpeta con `017` y `0018`: propone `0019`.
  - Carpeta con `001`, `002`, `005`: propone `006`.
  - Carpeta con `999`: propone `1000`.
  - Carpeta de ADR vacía, specs con `0017` y deuda con `003`: propone `0001` en el ADR.
  - Carpeta de ADR con `003` y specs en 4 dígitos: el ADR nuevo es `004`.
  - Clon simulado con ítems de 3 dígitos: propone 3 dígitos y E12 falla ante uno de 4.
  - Dos ramas: remoto simulado creado con `git init --bare` en `mktemp -d`, dos copias clonadas de él, una con `017_A` publicada y otra con `0017_B` local; la salida de `git ls-tree --name-only @{u}` y el listado local acreditan el duplicado, se conserva el publicado y se propone el siguiente libre, sin renombrar.
  - Mismo nombre con distinto ancho: se propone renombrar el local al publicado, sin ejecutarlo.
  - Sin rama remota: el cálculo usa solo la copia local.
  - Referencia corta: un `grep` de `017` muestra la cita sin cambios para el ítem que conservó el número y la cita actualizada para el renombrado tras confirmar.
  - Aviso (caso 11): con ítems de 3 dígitos aparece y ningún README cambia (`diff` vacío contra la copia previa); con solo ítems de 4 dígitos o sin ítems no aparece; tras marcar «mantener» no se repite; el marcador de versión se escribe igual.
  - E12: `FALLA` por duplicado y por ancho inválido, `AVISO` por mezcla, y el cierre indica el número de avisos.
- No desaparecen estas verificaciones: E12 (sin prefijos repetidos, por valor), E13 (índice contra carpetas), E20 (huellas contra `manifiestos/0.5.0.md`), el ensayo de instalación sin pisar nada, el contraste 0.4.0 contra 0.5.0 de `ACTUALIZAR.md`, y los chequeos de lint por archivo de `AGENTS.md` › Comandos (UTF-8, sin BOM, `name` igual a la carpeta, rutas existentes, sin marcadores `<<…>>` salvo `<<NOMBRE_PROYECTO>>` e `<<IDIOMA>>` en `template/`).
- `manifiestos/0.5.0.md` regenerado al final con E20 en verde (RF-19).
- La deuda `004_ConvivenciaNumeracion3y4Digitos` pasa a `Resuelta` al cerrar (`/sdd-cierre`).

## Dudas abiertas
Ninguna. Las cinco dudas de la primera versión y las dos decisiones posteriores (detección por nombres de carpeta sin leer el contenido de los README; regla mínima de una frase en cada pieza) las resolvió el usuario. La fuente única en un archivo del grupo C que el kit no mantiene es una limitación conocida (RNF-2), no una duda.
