# 03 — Plan: Convivencia de la numeración de 3 y 4 dígitos
Spec: 01_spec.md (aprobada 2026-10-25)

## 1. Ubicación y capas
El kit es solo Markdown (constitución, principios 1, 3 y 6; ADR 004). El cambio vive en `template/` (producto), en tres documentos de la raíz que el kit ya versiona (`ACTUALIZAR.md`, `CHANGELOG.md`, `manifiestos/0.5.0.md`) y en las frases del clon que afirman el ancho de un proyecto. No hay capas de aplicación, pantallas ni base de datos.

Patrón:
- **Una fuente:** la sección « Numeración única » del índice de specs (`template/.docs/002_specs/README.md`). Los índices de deuda y de ADR, `AGENTS.md`, las skills, los comandos y E12 remiten a ella por el nombre de la sección (ADR 009, principio 3).
- **Única repetición permitida (RF-13):** la frase mínima de la decisión 1, sin cifras de ancho.
- **Grupos de `ACTUALIZAR.md` (ADR 007):** los README de `.docs/` son grupo C y no se leen ni se reemplazan. `AGENTS.md` es grupo B (llega por fusión manual). Los comandos, skills y `validar-estructura.md` son grupo A.
- **Orden de edición:** el que fija la decisión 6. Quien edita un archivo lo relee en ese momento y parte de su contenido vigente.

## 2. Archivos que se crean o modifican
No se crea ningún archivo en `template/`. Todos los cambios son ediciones.

| Archivo | Capa | Responsabilidad | RF |
|---|---|---|---|
| `template/.docs/002_specs/README.md` | Producto (grupo C) | Reescribe « Numeración única » (decisión 5): ancho de carpeta, carpeta vacía, valor numérico, desbordamiento, huecos, duplicados, mismo nombre, sin remota, sin renumerar, referencias cortas. Cambia el encabezado `NNN` de la tabla por `Prefijo` | RF-1, RF-2, RF-3, RF-4, RF-5, RF-6, RF-7, RF-8, RF-9, RF-10, RF-11 |
| `template/.docs/003_deuda/README.md` | Producto (grupo C) | Quita « 4 dígitos en un proyecto »; remite a la fuente; unifica `NNN`/`NNNN` en `<prefijo>` | RF-12 |
| `template/.docs/004_adr/README.md` | Producto (grupo C) | Igual que deuda; `Reemplazado por <prefijo>`; columna `Prefijo` | RF-12 |
| `template/AGENTS.md` | Producto (grupo B) | « Deuda técnica » (placeholder), « Formato y numeración » (frase mínima y remisión), nota de `sdd` en « Skills de proyecto » (sin ancho absoluto). Se ubica por título de sección | RF-12, RF-13 |
| `template/.agents/README.md` | Producto (grupo A) | Árbol de carpetas con `<prefijo>` y sin « 4 dígitos » | RF-12 |
| `template/.agents/skills/sdd/SKILL.md` | Producto (grupo A) | Descripción y árbol con `<prefijo>`; la línea de ancho pasa a la frase mínima y la remisión; el ejemplo `0017_ReporteVentasPorZona` pasa a un nombre sin prefijo | RF-12, RF-13 |
| `template/.agents/skills/adr/SKILL.md` | Producto (grupo A) | Descripción, paso 2, ruta, plantilla (`ADR <prefijo>`, `Reemplazado por <prefijo>`) y frase mínima | RF-12, RF-13 |
| `template/.agents/skills/git-commit/SKILL.md` | Producto (grupo A) | Fase 2, punto 4: revisar por valor numérico; frase mínima; remisión | RF-13, RF-15 |
| `template/.agents/skills/migracion-bd/SKILL.md` | Producto (grupo A) | Descripción y encabezado con `<prefijo_Feature>` (hoy mezcla `<NNN>` y `<NNNN>`) | RF-12 |
| `template/.agents/commands/sdd-spec.md` | Producto (grupo A) | `argument-hint` y paso 2: frase mínima y remisión; sin « 4 dígitos » | RF-12, RF-13 |
| `template/.agents/commands/sdd-clarify.md`, `sdd-plan.md`, `sdd-tasks.md`, `sdd-implement.md`, `sdd-validate.md`, `sdd-flujo.md`, `sdd-cierre.md`, `sdd-change.md`, `sdd-status.md` | Producto (grupo A) | Solo el `argument-hint`: `<prefijo_NombreFeature>` | RF-12 |
| `template/.agents/validar-estructura.md` | Producto (grupo A) | Fila E12 y pasos 1 a 3: tercer estado AVISO (decisión 2). E20 no se redefine | RF-14 |
| `ACTUALIZAR.md` | Docs raíz (fuera de `template/`, no entra al manifiesto) | Sección `## Aviso de numeración`, línea en `## Reporte`, párrafo en `## Registro «mantener»`, frase en `## Marcador al cerrar`, entrada del índice y un caso límite (decisión 3) | RF-16, RF-17 |
| `CHANGELOG.md` | Docs raíz | Reescribe las dos líneas de 0.5.0 (decisión 4) | RF-18 |
| `manifiestos/0.5.0.md` | Docs raíz | Regenerado entero al final, con el algoritmo de `ACTUALIZAR.md` | RF-19 |
| `AGENTS.md` (raíz), `.docs/002_specs/README.md` y `.docs/004_adr/README.md` (raíz) | Mantenimiento del clon | Quitar « en un proyecto el ancho es de 4 » y remitir a la fuente. El clon sigue en 3 | RF-3, RF-12 |
| `.docs/002_specs/005_ConvivenciaNumeracion/05_avance.md` | Esta carpeta | Evidencia por RF y por caso. Lo escribe la sesión principal | RF-20 |

Cobertura: RF-1 a RF-11 en la fuente; RF-12 y RF-13 en las 20 piezas; RF-14 en `validar-estructura.md`; RF-15 en `git-commit`; RF-16 y RF-17 en `ACTUALIZAR.md`; RF-18 en `CHANGELOG.md`; RF-19 en el manifiesto y el orden de la decisión 6; RF-20 en la validación (F7).

No se editan: ADR 007 y 008, la clasificación de grupos y la definición de E20 en `ACTUALIZAR.md`, `plantillas.md`, la `004_ModeloYPermisosAgentes` ni la `006_RutaSiNoDelega`.

## 3. Base de datos
No aplica. Sin `sql/`, sin DDL ni seed, sin rollback.

## 4. Seguridad
El cambio es documentación del kit: sin datos de proyecto, sin secretos, sin scripts versionados (RNF-1).
- Búsqueda de secretos sobre lo que entra al commit, en el modo « solo secretos » de `seguridad-basica`.
- No se sobrescribe nada de un proyecto sin confirmación (ADR 007, RNF-4): el aviso solo ofrece texto y no lee los README.
- La prueba de F7 corre en `mktemp -d` con rutas absolutas; nunca toca el repo del kit ni un proyecto real.
- El remoto simulado de la prueba (`git init --bare` en `mktemp -d`) no tiene credenciales ni URL real.

## 5. Liberación / piloto
Dentro de 0.5.0 sin publicar: `VERSION` y la fecha del bloque de 0.5.0 no cambian. No hay feature flag ni piloto. La verificación previa a publicar es la de F7, en `mktemp -d`.

## 6. Decisiones
| Decisión | Alternativa descartada | Por qué |
|---|---|---|
| **(1) Frase mínima (D2).** Texto exacto, en una línea: « Un ítem nuevo conserva el ancho de su carpeta (detalle: `.docs/002_specs/README.md` › « Numeración única »). » Va en `template/AGENTS.md` › « Formato y numeración », en el paso 2 de `sdd-spec`, en la Fase 2 de `git-commit`, en el paso 2 de la skill `adr` y en la línea de ancho de la skill `sdd`. En `git-commit` se antepone « Revisa por valor numérico del prefijo; » porque RF-15 lo exige. Los placeholders usan `<prefijo>` (`<prefijo>_<Feature>`, `<prefijo_NombreFeature>`) y los ejemplos no llevan número. Los índices de deuda y de ADR, `.agents/README.md` y `migracion-bd` no llevan la frase: solo remiten o usan el placeholder | (a) Copiar la regla completa en cada pieza; (b) dejar solo la remisión | (a) se desfasa (el problema que motivó la ADR 009). (b) deja las piezas sin criterio si el README del proyecto es el de 0.4.0 y afirma 3 dígitos o no dice nada. La frase no lleva cifras: no contradice a ninguna versión del README. Se verifica con un `grep` de la frase exacta |
| **(2) Estado AVISO de E12.** La fila E12 pasa a: « Sin prefijos repetidos **por valor numérico** en los ítems de `.docs/002_specs/`, `.docs/003_deuda/` y `.docs/004_adr/`, con el ancho de `.docs/002_specs/README.md` › « Numeración única ». `FALLA`: duplicado por valor aunque difiera el ancho; ancho inválido (ni 3 ni 4 dígitos, ni los que el valor necesite); y, en el clon, un ítem de ancho distinto del que declara su índice. `AVISO`: carpeta de un proyecto con anchos mezclados válidos y sin duplicados. Las carpetas de sección se quedan en 3 ». Paso 1: anota `OK`, `AVISO` o `FALLA`. Paso 2: tras la tabla de fallos, un bloque **Avisos** (carpeta y hecho, una línea cada uno) que no es una corrección. Paso 3: sin fallos, `estructura OK (E1–E20, N archivos revisados)`; si hubo avisos, se añade `, M avisos` y se listan en **Avisos**. E20 y su fila no se tocan | (a) Tratar la mezcla como `FALLA`; (b) no anotarla; (c) un chequeo nuevo E21 | (a) bloquea cierres de proyectos legítimos (decisión 5 del avance); (b) oculta lo que la spec pide informar; (c) cambia el conteo « E1–E20 » que usan el comando, los agentes y E20, fuera del alcance |
| **(3) Aviso de numeración en `ACTUALIZAR.md`.** Sección nueva `## Aviso de numeración`, entre `## Reporte` y `## Enlaces`, con entrada en el índice. Lo calcula la sesión al armar el reporte, solo por nombres: lista las tres carpetas y, si alguna tiene al menos un ítem de 3 dígitos (nombre con prefijo de 3 dígitos y `_`), muestra: el ancho detectado por carpeta, si hay anchos mezclados, que no se renumera nada y el texto ofrecido (la sección « Numeración única » de `template/.docs/002_specs/README.md` más los párrafos equivalentes de los README de deuda y ADR). Escribe en un README solo si el usuario confirma, con la diferencia a la vista. `## Reporte` añade que el aviso va aparte y no entra en las cuentas. `## Marcador al cerrar` añade que no cuenta como pendiente y no bloquea el marcador. **«Mantener»:** línea del registro existente (`.agents/.kit-mantener`) con primera columna `aviso de numeración`, clase `aviso` y la huella del texto de la sección « Numeración única » tomado de `template/` (la de los otros dos fragmentos no entra); reaparece solo si esa huella cambia. Sin marcador de versión, mismo comportamiento. El texto de `ACTUALIZAR.md` remite a la fuente y no nombra un ancho absoluto | (a) Un grupo nuevo de `ACTUALIZAR.md`; (b) leer los README para detectar la regla vieja; (c) escribir el texto sin preguntar; (d) mostrarlo en cada corrida sin «mantener» | (a) cambia la clasificación (fuera de alcance); (b) viola la decisión D1 y el grupo C; (c) viola ADR 007 y RNF-4; (d) ruido repetido |
| **(4) CHANGELOG de 0.5.0.** Se reemplazan las dos líneas de la viñeta « Numeración en un proyecto » (líneas 22-25) por dos viñetas: (i) « Numeración: un proyecto nuevo numera specs, deudas y ADR con 4 dígitos (`0001`); un proyecto con ítems conserva el ancho de cada carpeta (`017_` sigue en 3). El siguiente libre se calcula por valor numérico (`017` = `0017`) y nunca se renumera. Las carpetas de sección de `.docs/` y el clon del kit siguen en 3. Lo aplican los comandos `/sdd-*`, las skills `adr`, `git-commit`, `migracion-bd` y `sdd`, y el chequeo E12, que informa los anchos mezclados como `AVISO` »; (ii) « `ACTUALIZAR.md` avisa, por los nombres de carpeta y sin leer los README, si el proyecto tiene ítems de 3 dígitos, y ofrece el texto de la regla; solo escribe si lo confirmas. Mientras no lo incorpores, las piezas aplican la regla mínima « un ítem nuevo conserva el ancho de su carpeta » ». Se edita con una sustitución exacta sobre esas líneas, partiendo del archivo vigente, sin tocar las viñetas de la `004` | (a) Añadir una viñeta aparte y dejar la vieja; (b) cambiar la fecha o `VERSION` | (a) la vieja contradice a RF-1; (b) 0.5.0 sigue sin publicar y la fecha no es de esta spec |
| **(5) Regla del siguiente libre en « Numeración única ».** La sección conserva el párrafo de las carpetas de sección en 3 y se reorganiza así: *Ancho* (ítem, ancho válido, ancho de carpeta, carpeta vacía con el orden specs, deuda, ADR, clon); *Siguiente libre* (« Siguiente = mayor valor numérico + 1. El prefijo se lee como entero (`017` y `0017` valen 17), nunca como texto. Se cuentan la copia local y la rama remota ya descargada; el ancho no interviene en la comparación, solo se aplica al escribir el número nuevo. Si el valor necesita más dígitos que el ancho de la carpeta, se usan los que necesita »); *Huecos*; *Revisión de duplicados* (el paso 1 compara por valor; se añade el caso de mismo nombre y la rama sin remota); *Nunca se renumera* y *Referencias cortas*. Se mantienen sin cambio los comandos `git ls-tree` y las reglas de conservar el publicado y confirmar antes de renombrar. La frase « con otro nombre » del paso 1 se sustituye por « con otro nombre o con otro ancho » | (a) Dejar « prefijo mayor + 1 » sin aclarar; (b) un documento nuevo del grupo A con la regla | (a) deja la comparación abierta; (b) alternativa ya descartada por el usuario (decisión 3 del avance) |
| **(6) Orden respecto de la 004 y manifiesto.** Las fases de documentación no tocan `template/`. La implementación de esta spec va después de que la `004` termine sus ediciones de `template/` (decisión del usuario); la `006` se coordina aparte. El manifiesto lo regenera una sola vez quien cierre segundo: si la `004` ya cerró, lo regenera esta spec en F6; si no, esta spec lo deja anotado como pendiente en `05_avance.md` y repite E20 y los chequeos cuando la `004` lo regenere. Cada edición relee el archivo, ubica por título de sección y no por línea, y no revierte trabajo ajeno. La sesión principal serializa también frente a la `006` | (a) Implementar ya en paralelo con la `004`; (b) regenerar el manifiesto cada uno al terminar | (a) ya se acordó serie (decisión 1 del avance); (b) el manifiesto del primero queda viejo al instante y repetirlo crea un E20 en rojo intermedio |

### Candidatas a ADR (no se crean)
- **A.** La regla de convivencia: conservar el ancho de cada carpeta, comparar por valor y no renumerar nunca (decisiones 1 y 5). Hay una alternativa real (pasar a 4 con transición) y un efecto duradero sobre todo proyecto actualizado. La numeración la fija el índice al crearlo; no se debe tomar un número ahora.
- **B.** La fuente única vive en un archivo del grupo C y la actualización avisa por nombres, sin leerlo (decisión 3). Alternativas reales: un archivo del grupo A, o comparar el README. Efecto: deja una limitación conocida (RNF-2).
La decisión (2) (estado AVISO) no se propone como ADR: es un detalle del procedimiento del chequeo.

## 7. Fases y estimado
Estimado en minutos porque solo se edita Markdown (la `003` mostró que 0,25 h por fase sobreestima unas 10 veces).

| Fase | Contenido | RF | Estimado |
|---|---|---|---|
| F0 | Preparación: estado de la `004` (debe haber terminado sus ediciones de `template/`) y de la `006`, que se coordina aparte; releer cada archivo del plan al tocarlo | RF-19 | 5 min |
| F1 | Fuente única: « Numeración única » (decisión 5) y encabezado `Prefijo` del índice de specs; remisiones y placeholders en los README de deuda y ADR | RF-1 a RF-11, RF-12 | 15 min |
| F2 | Piezas: `template/AGENTS.md`, `.agents/README.md`, skills `sdd`, `adr`, `git-commit`, `migracion-bd`, `sdd-spec` y los 9 `argument-hint`. Frase mínima de la decisión 1 | RF-12, RF-13, RF-15 | 15 min |
| F3 | `validar-estructura.md`: fila E12 y pasos 1 a 3 con AVISO (decisión 2) | RF-14 | 6 min |
| F4 | `ACTUALIZAR.md`: aviso, reporte, «mantener», marcador al cerrar, índice y caso límite (decisión 3) | RF-16, RF-17 | 12 min |
| F5 | `CHANGELOG.md` (decisión 4) y las frases del clon (`AGENTS.md` de la raíz y los README de specs y ADR) | RF-18, RF-12 | 6 min |
| F6 | Manifiesto: regenerar `manifiestos/0.5.0.md` y repetir E20 (decisión 6) | RF-19 | 5 min |
| F7 | Validación (abajo) | RF-20, todos | 25 min |
| | **Total** | | **89 min (≈ 1,5 h)** |

**F7, validación.** La hace el reviewer con la sesión principal, en `mktemp -d`; nada en el kit real ni en un proyecto real:
1. Ensayo de instalación de `AGENTS.md` › Comandos (`cp -R --update=none template/. "$tmp"/`): copia todo y no pisa nada.
2. Proyecto simulado por caso de la spec. Para cada uno: el número que propone y el listado (`ls`) antes y después (idéntico salvo el ítem nuevo), y el `grep` que lo acredita. Casos: nuevo (`0001`); `001,002,017` (`018`); `017` y `0018` (`0019`); `001,002,005` (`006`); `999` (`1000`); ADR vacío con specs `0017` y deuda `003` (`0001`); ADR `003` con specs en 4 (`004`); clon simulado (3 dígitos; E12 falla ante uno de 4); mismo nombre con distinto ancho (propone renombrar el local, sin ejecutar); sin rama remota; referencia corta con `grep`.
3. Dos ramas: remoto simulado con `git init --bare` en `mktemp -d`, dos clonaciones, `017_A` publicada y `0017_B` local; `git ls-tree --name-only @{u}` y el listado local acreditan el duplicado; se conserva el publicado y se propone el siguiente libre, sin renombrar.
4. Aviso: con ítems de 3 dígitos aparece y `diff` de los README contra la copia previa sale vacío; sin ítems o solo con 4 dígitos no aparece; tras «mantener» no se repite; el marcador de versión se escribe igual.
5. E12 en cada situación (duplicado, ancho inválido, mezcla, clon) con el resultado `FALLA` o `AVISO` esperado y el cierre con el número de avisos. E13 sobre los tres índices. E20 en verde con el manifiesto regenerado.
6. Contraste 0.4.0 contra 0.5.0 de `ACTUALIZAR.md` sobre un proyecto simulado de 0.4.0 (sin cambiar el procedimiento de clasificación).
7. Chequeos de `AGENTS.md` › Comandos en cada archivo tocado (UTF-8, sin BOM, `name` igual a la carpeta, rutas existentes, sin marcadores `<<…>>` salvo los dos permitidos) y `grep` de la frase mínima y de que no quedan « 4 dígitos en un proyecto » ni `NNNN` como regla en `template/`.

## 8. Criterios de aceptación
Mapeados a los criterios de finalización de la spec:

| Criterio de finalización | Se acepta cuando |
|---|---|
| RF-1 a RF-20 cumplidos con evidencia en `05_avance.md` | Cada RF tiene una fila de evidencia (lectura, `ls`/`grep` o caso simulado) |
| Una sola fuente; piezas solo remiten y llevan la frase mínima | `grep` de la frase exacta encuentra las piezas de la decisión 1; ninguna pieza de `template/` afirma un ancho absoluto fuera de la fuente |
| E12 con AVISO y `git-commit` comparan por valor | La fila E12 y la Fase 2 dicen « valor numérico »; la prueba da `FALLA` por duplicado y `AVISO` por mezcla |
| `ACTUALIZAR.md` y `CHANGELOG.md` reflejan la regla | Existe `## Aviso de numeración`; el changelog ya no contiene « no está definida todavía » |
| Prueba en `mktemp -d` con los resultados de la spec | Los resultados de F7, puntos 2 a 5, coinciden con la lista de resultados observables de la spec; `ls` idéntico antes y después |
| Verificaciones que no desaparecen | E12, E13, E20, ensayo de instalación, contraste 0.4.0 contra 0.5.0 y lint por archivo pasan |
| Manifiesto regenerado al final con E20 en verde | `manifiestos/0.5.0.md` tiene 41 filas y E20 sin diferencias |
| Deuda 004 resuelta | `/sdd-cierre` la pasa a `Resuelta` con « Atendida por » y « Cómo se atendió » |
