# 04 — Tareas: Convivencia de la numeración de 3 y 4 dígitos
Leyenda: `[ ]` Pendiente · `[~]` En curso · `[x]` Hecho · `[!]` Bloqueado

Spec: `01_spec.md` (aprobada). Plan: `03_plan.md`. Total estimado: **89 min (≈ 1,5 h)** en 8 fases y 30 pasos.

## Reglas comunes (se definen una vez y los pasos las citan)
**L — Lint por archivo tocado** (`AGENTS.md` › Comandos), sobre cada archivo que el paso edita:
- `file -i <archivo>` da `utf-8`.
- Sin BOM: `grep -l $'^\xEF\xBB\xBF' <archivo>` sale vacío.
- `name` del frontmatter igual al nombre de su carpeta (donde el archivo lo tiene).
- Rutas citadas con comillas inversas existen (no aplica a `manifiestos/`).
- En `template/`, sin marcadores `<<…>>` salvo `<<NOMBRE_PROYECTO>>` e `<<IDIOMA>>`: `grep -n '<<' <archivo>`.

**P — Archivo compartido** (todo archivo fuera de esta carpeta, porque la `004_ModeloYPermisosAgentes` o la `006_RutaSiNoDelega` pueden haberlo editado):
1. Antes de editar, relee el archivo y guarda una copia previa en el directorio temporal de la sesión (`bak=$(mktemp -d)` una vez; `cp <archivo> "$bak"/…` conservando el nombre relativo).
2. Edita solo lo propio, ubicándolo por título de sección o por el texto exacto, no por número de línea.
3. Muestra `diff "$bak"/<copia> <archivo>`: deben aparecer únicamente las líneas del paso. Si aparece algo ajeno, no lo reviertas: detente y avisa.

**Reglas de la sesión:**
- Ningún paso revierte trabajo ajeno.
- Todo ensayo corre en `mktemp -d`, nunca en un proyecto real ni en el kit real. Rutas absolutas.
- Ningún agente hace `git commit`, `push`, `fetch` ni `pull`. El commit es aparte, con la skill `git-commit` y confirmación del usuario.
- No se guardan scripts en el repo (ADR 004). La regla es prosa: el agente la aplica leyendo « Numeración única » y declara el resultado, que se acredita con `ls`/`grep`/`diff`.
- El implementer no escribe `05_avance.md`: devuelve la evidencia a la sesión principal.

## F0 — Preparación (estimado: 5 min)
**PRECONDICIÓN: la `004_ModeloYPermisosAgentes` terminó sus ediciones de `template/` (decisión 12 del avance). Mientras no sea cierto, no se edita `template/`. La `006_RutaSiNoDelega` también puede editar `template/`: se coordina aparte y P protege cada edición.**

- [x] **F0.1 Comprobar la precondición.** RF-19
  - Hecho cuando:
    - [x] Lectura del `PUNTO DE RETOMO` de `004_ModeloYPermisosAgentes/05_avance.md`: sus pasos de edición de `template/` están `[x]` (solo falta, a lo sumo, su regeneración del manifiesto). Si no, el paso queda `[!]` y se informa
    - [x] `git status --short template manifiestos CHANGELOG.md` anotado como línea base de lo ya cambiado por otros
    - [x] Verificación manual del usuario: No aplica
- [x] **F0.2 Línea base de la numeración.** RF-12, RF-13, RF-19
  - Hecho cuando:
    - [x] `find template -type f | wc -l` anotado (41 si nadie añadió archivos)
    - [x] `grep -rnE "NNNN?|0001|[34] dígitos" template --include=*.md` guardado como lista de antes (se compara en F2.8)
    - [x] `bak=$(mktemp -d)` creado; la ruta se anota para P
    - [x] Verificación manual: No aplica

## F1 — Fuente única « Numeración única » (estimado: 15 min)
- [x] **F1.1 Ancho en « Numeración única » del índice de specs.** RF-1, RF-2, RF-3, RF-5
  - Archivo: `template/.docs/002_specs/README.md`. Conserva el párrafo de las carpetas de sección en 3. Redacta (decisión 5 del plan): ítem, ancho válido, ancho de carpeta, carpeta vacía (mayor valor entre las otras; empate specs, deuda, ADR; ninguna: 4), clon (ancho que declara su índice), desbordamiento (1000 en una carpeta de 3) y la frase mínima de la decisión 1. Los ejemplos de rutas no llevan número.
  - Hecho cuando:
    - [x] L y P sobre el archivo
    - [x] `grep -n "ancho válido\|mayor valor\|Un ítem nuevo conserva el ancho de su carpeta" <archivo>` encuentra las tres
    - [x] `diff` solo muestra cambios dentro de « Numeración única »
    - [x] Verificación manual: No aplica (se aprueba en F1.3)
- [x] **F1.2 Siguiente libre, huecos y rama remota ausente.** RF-4, RF-6, RF-9
  - Redacta: « Siguiente = mayor valor numérico + 1… nunca como texto… el ancho no interviene en la comparación », huecos sin reutilizar y el cálculo solo con la copia local cuando no hay remota ni upstream (pregunta cuál conservar si hay duplicado).
  - Hecho cuando:
    - [x] L y P
    - [x] `grep -n "valor numérico\|no hay rama remota\|upstream" <archivo>` encuentra el cálculo por valor y el caso sin remota
    - [x] `git ls-tree --name-only @{u}` sigue citado tal cual
    - [x] Verificación manual: No aplica
- [x] **F1.3 Duplicados por valor, nunca renumerar y referencias cortas.** RF-7, RF-8, RF-10, RF-11
  - Redacta: el paso 1 de la revisión de duplicados pasa de « con otro nombre » a « con otro nombre o con otro ancho »; mismo nombre = probable mismo ítem (renombrar el local al publicado, con confirmación); nunca se renumera por cambio de ancho o de versión; las tres situaciones de la cita corta.
  - Hecho cuando:
    - [x] L y P
    - [x] `grep -n "otro ancho\|mismo nombre\|nunca se renumera\|cita" <archivo>` encuentra los cuatro puntos
    - [x] Se conservan: conserva el publicado, confirmar antes de `git mv`, referencias por `grep`
    - [x] Verificación manual del usuario: abrir `template/.docs/002_specs/README.md` › « Numeración única » y aprobar el texto de la regla; debe verse una sola regla, sin contradicción entre ancho, valor y duplicados
- [x] **F1.4 Encabezado de la tabla del índice de specs.** RF-12
  - Cambia el encabezado `NNN` de la tabla por `Prefijo`.
  - Hecho cuando:
    - [x] L y P
    - [x] `grep -n "^| Prefijo" <archivo>` encuentra el encabezado y `grep -n "^| NNN" <archivo>` sale vacío
    - [x] Verificación manual: No aplica
- [x] **F1.5 Índices de deuda y de ADR remiten a la fuente.** RF-12
  - Archivos: `template/.docs/003_deuda/README.md` y `template/.docs/004_adr/README.md`. Quita « 4 dígitos en un proyecto »; remite a « Numeración única »; unifica `NNN`/`NNNN` en `<prefijo>` (incluidos `# <prefijo> — <Título>`, `Reemplazado por <prefijo>` y las columnas `Prefijo`). Las carpetas de sección siguen en 3.
  - Hecho cuando:
    - [x] L y P en cada archivo
    - [x] `grep -nE "NNNN?|4 dígitos" <archivo>` sale vacío en ambos
    - [x] `grep -n "Numeración única" <archivo>` encuentra la remisión
    - [x] Verificación manual: No aplica

## F2 — Piezas que remiten a la fuente (estimado: 15 min)
Frase mínima exacta (decisión 1 del plan): « Un ítem nuevo conserva el ancho de su carpeta (detalle: `.docs/002_specs/README.md` › « Numeración única »). » Los placeholders son `<prefijo>`; los ejemplos no llevan número.

- [x] **F2.1 `template/AGENTS.md`.** RF-12, RF-13
  - Tres lugares, ubicados por título: « Deuda técnica » (placeholder), « Formato y numeración » (frase mínima y remisión, sin « 4 dígitos ») y la nota de `sdd` en « Skills de proyecto ». Las carpetas de sección siguen en 3. No se toca « Qué lee cada rol ».
  - Hecho cuando:
    - [x] L y P
    - [x] `grep -nE "NNNN?|4 dígitos|0001" template/AGENTS.md` sale vacío
    - [x] `grep -c "Un ítem nuevo conserva el ancho de su carpeta" template/AGENTS.md` da 1
    - [x] `diff` no muestra cambios en « Qué lee cada rol »
    - [x] Verificación manual: No aplica
- [x] **F2.2 `.agents/README.md` y skill `sdd`.** RF-12, RF-13
  - Árbol de `.agents/README.md` con `<prefijo>`. En `skills/sdd/SKILL.md`: descripción y árbol con `<prefijo>`, la línea de ancho pasa a la frase mínima y el ejemplo `0017_ReporteVentasPorZona` pasa a `ReporteVentasPorZona`.
  - Hecho cuando:
    - [x] L y P en ambos
    - [x] `grep -nE "NNNN?|4 dígitos|0001|0017" <archivo>` sale vacío en ambos
    - [x] La frase mínima aparece una vez en la skill `sdd`
    - [x] `diff` de `SKILL.md` no toca la frase que añadió la `004` junto al enlace de `avance-y-tiempos.md`
    - [x] Verificación manual: No aplica
- [x] **F2.3 Skill `adr`.** RF-12, RF-13
  - Descripción, paso 2 (frase mínima y remisión), ruta `.docs/004_adr/<prefijo>_<TituloPascalCase>.md` y plantilla (`# ADR <prefijo>:`, `Reemplazado por <prefijo>`).
  - Hecho cuando:
    - [x] L y P
    - [x] `grep -nE "NNNN?|4 dígitos|0001" template/.agents/skills/adr/SKILL.md` sale vacío
    - [x] La frase mínima aparece una vez
    - [x] Verificación manual: No aplica
- [x] **F2.4 Skill `git-commit`.** RF-13, RF-15
  - Fase 2, punto 4: « Revisa por valor numérico del prefijo; » + frase mínima + remisión; sin « 4 dígitos ». Conserva: detener el commit ante choque, proponer el siguiente libre, no renombrar sin confirmación.
  - Hecho cuando:
    - [x] L y P
    - [x] `grep -n "valor numérico" template/.agents/skills/git-commit/SKILL.md` encuentra el punto 4 y `grep -nE "4 dígitos|0001" <archivo>` sale vacío
    - [x] La frase mínima aparece una vez
    - [x] Verificación manual: No aplica
- [x] **F2.5 Skill `migracion-bd`.** RF-12
  - Descripción (`<NNN>`) y encabezado del SQL (`<NNNN_Feature>`) pasan a `<prefijo_Feature>`. No se toca `NN_<tipo>_<desc>.sql`.
  - Hecho cuando:
    - [x] L y P
    - [x] `grep -nE "<NNNN?" template/.agents/skills/migracion-bd/SKILL.md` sale vacío y `grep -n "NN_<tipo>" <archivo>` sigue encontrando el orden de scripts
    - [x] Verificación manual: No aplica
- [x] **F2.6 Comando `sdd-spec`.** RF-12, RF-13
  - `argument-hint` con `<prefijo_NombreFeature | NombreFeature>`; el paso 2 usa la frase mínima y remite a la fuente, sin « 4 dígitos ». Conserva: confirmar el número, avisar si el prefijo existe, recomendar subir la carpeta.
  - Hecho cuando:
    - [x] L y P
    - [x] `grep -nE "NNNN?|4 dígitos|0001" template/.agents/commands/sdd-spec.md` sale vacío
    - [x] La frase mínima aparece una vez
    - [x] Verificación manual: No aplica
- [x] **F2.7 `argument-hint` de los otros nueve comandos.** RF-12
  - `sdd-clarify`, `sdd-plan`, `sdd-tasks`, `sdd-implement`, `sdd-validate`, `sdd-flujo`, `sdd-cierre`, `sdd-change` y `sdd-status`: solo la línea 3, `<NNNN_NombreFeature>` pasa a `<prefijo_NombreFeature>` (los argumentos que siguen se conservan). `sdd-validate.md` es de la `004`: solo se toca la línea 3.
  - Hecho cuando:
    - [x] L y P en cada uno de los nueve
    - [x] `diff` de cada archivo muestra una sola línea cambiada
    - [x] `grep -rn "NNNN" template/.agents/commands` sale vacío
    - [x] Verificación manual: No aplica
- [x] **F2.8 Cierre de la fase: barrido de `template/`.** RF-12, RF-13
  - Hecho cuando:
    - [x] `grep -rnE "NNNN?|0001|[34] dígitos" template --include=*.md` comparado con la lista de F0.2: solo quedan las menciones legítimas (carpetas de sección en 3; `NN_<tipo>` del SQL; la fila de E12 hasta F3; `COM-NNN` de `plantillas.md`)
    - [x] `grep -rn "Un ítem nuevo conserva el ancho de su carpeta" template` lista exactamente `AGENTS.md`, `sdd/SKILL.md`, `adr/SKILL.md`, `git-commit/SKILL.md` y `sdd-spec.md`, y la fuente
    - [x] Ninguna pieza afirma un ancho absoluto fuera de « Numeración única »
    - [x] Verificación manual: No aplica

## F3 — Chequeo E12 con estado AVISO (estimado: 6 min)
- [x] **F3.1 Fila E12 de `validar-estructura.md`.** RF-14
  - Texto de la decisión 2 del plan: duplicado por valor, ancho inválido y (en el clon) ancho distinto del declarado son `FALLA`; mezcla válida sin duplicados es `AVISO`; remite a « Numeración única »; sin « 4 dígitos en un proyecto ». La fila E20 no se toca.
  - Hecho cuando:
    - [x] L y P sobre `template/.agents/validar-estructura.md`
    - [x] `grep -n "^| E12" <archivo>` muestra `valor numérico`, `FALLA` y `AVISO`; `grep -n "4 dígitos" <archivo>` sale vacío
    - [x] `diff` no muestra cambios en la fila E20
    - [x] Verificación manual: No aplica
- [x] **F3.2 Pasos 1 a 3 del procedimiento.** RF-14
  - Paso 1 anota `OK`, `AVISO` o `FALLA`; paso 2 añade el bloque **Avisos** tras la tabla de fallos (no es una corrección); paso 3: `estructura OK (E1–E20, N archivos revisados)` más `, M avisos` si los hubo.
  - Hecho cuando:
    - [x] L y P
    - [x] `grep -n "Avisos\|AVISO" <archivo>` encuentra los tres pasos
    - [x] `grep -n "E1–E20" <archivo>` se conserva
    - [x] Verificación manual: No aplica

## F4 — Aviso de numeración en `ACTUALIZAR.md` (estimado: 12 min)
- [x] **F4.1 Sección `## Aviso de numeración` y entrada del índice.** RF-16, RF-17
  - Entre `## Reporte` y `## Enlaces`. Detección solo por nombres: al menos un ítem de 3 dígitos en `.docs/002_specs/`, `.docs/003_deuda/` o `.docs/004_adr/`. Contenido: ancho detectado por carpeta, si hay anchos mezclados, constancia de que no se renumera y el texto ofrecido; escribe en un README solo con confirmación y la diferencia a la vista. Sin marcador de versión, igual. No lee los README y no nombra un ancho absoluto: remite a la fuente.
  - Hecho cuando:
    - [x] L y P sobre `ACTUALIZAR.md`
    - [x] `grep -n "Aviso de numeración" ACTUALIZAR.md` encuentra el índice y la sección; el enlace del índice coincide con el título
    - [x] `grep -nE "contenido del README|lee el README" ACTUALIZAR.md` no introduce lectura de README; `grep -n "4 dígitos" ACTUALIZAR.md` sale vacío
    - [x] Verificación manual del usuario: abrir `ACTUALIZAR.md` › `## Aviso de numeración` y aprobar el aviso; debe verse el criterio sí/no por nombres, el texto ofrecido y que no se escribe sin confirmar
- [x] **F4.2 `## Reporte` y `## Marcador al cerrar`.** RF-16, RF-17
  - `## Reporte`: el aviso va aparte y no entra en las cuentas. `## Marcador al cerrar`: no cuenta como pendiente ni bloquea el marcador.
  - Hecho cuando:
    - [x] L y P
    - [x] `grep -n "aviso de numeración" ACTUALIZAR.md` encuentra ambas menciones
    - [x] `diff` no cambia la lista de pendientes existente ni la definición del grupo C
    - [x] Verificación manual: No aplica
- [x] **F4.3 Registro «mantener» y caso límite.** RF-17
  - `## Registro «mantener»`: primera columna `aviso de numeración`, clase `aviso`, huella del texto de la sección « Numeración única » de `template/` (los otros dos fragmentos no entran); reaparece solo si esa huella cambia. Un caso en `## Casos límite y errores frecuentes`.
  - Hecho cuando:
    - [x] L y P
    - [x] `grep -n "clase.*aviso\|aviso de numeración" ACTUALIZAR.md` encuentra el registro y el caso límite
    - [x] El formato de la línea sigue siendo `ruta o ruta › elemento | clase | huella | versión | fecha`
    - [x] Verificación manual: No aplica

## F5 — CHANGELOG y alineación del clon (estimado: 6 min)
- [x] **F5.1 `CHANGELOG.md` de 0.5.0.** RF-18
  - Sustitución exacta de la viñeta « Numeración en un proyecto… no está definida todavía » por las dos viñetas de la decisión 4 del plan. No toca las viñetas de la `004`, `VERSION` ni la fecha.
  - Hecho cuando:
    - [x] L y P sobre `CHANGELOG.md`
    - [x] `grep -n "no está definida todavía\|pasa a 4 dígitos" CHANGELOG.md` sale vacío
    - [x] `grep -n "conserva el ancho\|valor numérico\|AVISO" CHANGELOG.md` encuentra las viñetas nuevas
    - [x] `cat VERSION` sigue en `0.5.0` y el encabezado `## 0.5.0 — 2026-10-24` no cambió
    - [x] Verificación manual: No aplica
- [x] **F5.2 Frases del clon.** RF-3, RF-12
  - `AGENTS.md` (raíz) líneas « Formato y numeración » y nota de `sdd`; `.docs/002_specs/README.md` y `.docs/004_adr/README.md` (raíz): quitar « en un proyecto el ancho es de 4 » y remitir a la fuente (`template/.docs/002_specs/README.md`). Cambio mínimo: el clon sigue en 3 y no se renumera.
  - Hecho cuando:
    - [x] L y P en los tres (sin la regla de `template/`; `name` y marcadores `<<` no aplican fuera de `template/`)
    - [x] `grep -nE "proyecto el ancho es de 4|en un proyecto, 4|ancho es de 4" AGENTS.md .docs/002_specs/README.md .docs/004_adr/README.md` sale vacío
    - [x] `grep -n "3 dígitos" AGENTS.md` conserva la afirmación del clon
    - [x] `diff` de `AGENTS.md` no toca « Qué lee cada rol » (de la `004`)
    - [x] Verificación manual: No aplica

## F6 — Manifiesto (estimado: 5 min)
Se hace UNA vez, después de todos los cambios. Toda edición posterior de `template/` obliga a repetir F6 y E20.

- [x] **F6.1 Regenerar `manifiestos/0.5.0.md`.** RF-19
  - Con el algoritmo de `ACTUALIZAR.md` › Huella, sobre cada archivo de `template/` y sin guardar scripts en el repo. Mismo encabezado, tabla `Ruta | Huella`. Si la `004` aún no regeneró, esta regeneración también cubre lo suyo (quien cierre segundo la hace una vez); si la `004` regenera después, F6 y E20 se repiten.
  - Hecho cuando:
    - [x] L sobre el manifiesto (sin la comprobación de rutas)
    - [x] `grep -c "^| " manifiestos/0.5.0.md` iguala `find template -type f | wc -l` más 2 (encabezado y separador)
    - [x] `diff` de la copia previa contra el nuevo muestra solo huellas de archivos cambiados por las tres specs
    - [x] Verificación manual: No aplica
- [x] **F6.2 E20.** RF-19
  - Hecho cuando:
    - [x] Recálculo de huellas de `template/` contra `manifiestos/0.5.0.md`: 0 diferencias, 0 archivos sin fila, 0 filas sin archivo
    - [x] `git status --short template manifiestos` solo lista archivos de esta spec y de las otras
    - [x] Verificación manual: No aplica

## F7 — Validación (estimado: 25 min)
La hace el reviewer con la sesión principal. Todo en `mktemp -d` (`sim=$(mktemp -d)`), con rutas absolutas y sin `cd` al kit real.

- [x] **F7.1 Ensayo de instalación.** RF-20
  - Hecho cuando:
    - [ ] `tmp=$(mktemp -d); cp -R --update=none template/. "$tmp"/` copia todo sin pisar nada (`find "$tmp" -type f | wc -l` iguala el de `template/`)
    - [ ] No quedan marcadores `<<…>>` salvo los dos permitidos: `grep -rn '<<' "$tmp"`
    - [ ] Verificación manual: No aplica
- [x] **F7.2 Casos de numeración en un proyecto simulado.** RF-1 a RF-6, RF-20
  - Un proyecto simulado en `$sim` (copia de `template/` más carpetas vacías de ítems). Por caso: el agente aplica « Numeración única » y declara el número; `ls` antes y después idéntico salvo el ítem nuevo.
  - Hecho cuando:
    - [ ] Proyecto nuevo, sin ítems: propone `0001`
    - [ ] `001`, `002`, `017`: propone `018`
    - [ ] `017` y `0018`: propone `0019`
    - [ ] `001`, `002`, `005`: propone `006`
    - [ ] `999`: propone `1000`
    - [ ] ADR vacío, specs con `0017` y deuda con `003`: propone `0001` en el ADR
    - [ ] ADR con `003` y specs en 4 dígitos: el ADR nuevo es `004`
    - [ ] Clon simulado con ítems de 3 dígitos: propone 3 dígitos
    - [ ] Archivos sin prefijo (`README.md`) no cuentan: carpeta con solo `README.md` está vacía
    - [ ] Verificación manual: No aplica
- [x] **F7.3 Duplicados, remoto simulado, sin remota y referencias.** RF-7 a RF-11, RF-20
  - Remoto: `git init --bare "$sim/remoto.git"`, dos clonaciones en `$sim`, `017_A` publicada y `0017_B` local. Estos comandos `git` corren solo dentro de `$sim`, no en el kit.
  - Hecho cuando:
    - [ ] `git -C <copia> ls-tree --name-only @{u}` y el `ls` local muestran el duplicado por valor; se conserva el publicado y se propone el siguiente libre, sin renombrar (`ls` igual antes y después)
    - [ ] Mismo nombre con distinto ancho (`017_A` remoto, `0017_A` local): se propone renombrar el local al publicado, sin ejecutarlo
    - [ ] Sin rama remota ni upstream: el siguiente libre se calcula con la copia local; ante duplicado, pregunta cuál conservar
    - [ ] Referencia corta: `grep -rn "017" <copia>` muestra la cita sin cambios para el ítem que conservó el número, y la cita actualizada para el renombrado tras confirmar
    - [ ] Nada se renumera por cambio de ancho (`ls` idéntico)
    - [ ] Verificación manual: No aplica
- [x] **F7.4 Aviso al actualizar y E12.** RF-14, RF-15, RF-16, RF-17, RF-20
  - Hecho cuando:
    - [ ] Proyecto simulado de 0.4.0 con ítems de 3 dígitos: aparece el aviso con el ancho por carpeta; `diff -r` de los README contra la copia previa sale vacío; sin escribir
    - [ ] Con solo ítems de 4 dígitos, o sin ítems: no hay aviso
    - [ ] Tras «mantener» (línea en el registro simulado) no se repite; el marcador de versión se escribe igual
    - [ ] E12: `FALLA` por duplicado y por ancho inválido (`7_`, `00017_`); `AVISO` por mezcla; en el clon simulado `FALLA` ante un ítem de 4; el cierre indica el número de avisos
    - [ ] Verificación manual: No aplica
- [x] **F7.5 Chequeos finales del arnés.** RF-14, RF-19, RF-20
  - Hecho cuando:
    - [ ] E13: cada índice de `.docs/002_specs/`, `.docs/003_deuda/` y `.docs/004_adr/` coincide con sus carpetas o archivos
    - [ ] E20 en verde (F6.2), repetido si hubo cualquier edición posterior de `template/`
    - [ ] Contraste 0.4.0 contra 0.5.0 de `ACTUALIZAR.md` sobre un proyecto simulado de 0.4.0: la clasificación y los grupos no cambian
    - [ ] L sobre cada archivo tocado en F1 a F5 (lista con `git status --short`)
    - [ ] `grep -rn "no está definida todavía" CHANGELOG.md` vacío
    - [ ] Búsqueda de secretos sobre lo que entraría al commit, en modo « solo secretos » de `seguridad-basica`: sin hallazgos
    - [ ] Verificación manual: No aplica
- [x] **F7.6 Aprobación del usuario y entrega de evidencia.** RF-20
  - Hecho cuando:
    - [x] Las evidencias de F7.1 a F7.5 se entregan a la sesión principal, que las deja en `05_avance.md`
    - [x] Verificación manual del usuario: revisar `template/.docs/002_specs/README.md` › « Numeración única » y `ACTUALIZAR.md` › `## Aviso de numeración`, y ver la lista de resultados por caso; debe coincidir con los resultados observables de los criterios de finalización de la spec
    - [x] No queda ningún `git commit`, `push` ni `fetch` ejecutado por el agente

## F8 — Correcciones de la validación (estimado: 10 min; fase añadida el 2026-10-25)
Origen: ambigüedades de redacción halladas por los reviewers en F7, aprobadas por el usuario. Cada paso edita solo lo propio con el protocolo P (copia previa en `/tmp/tmp.WZy8FH7TII/`). `template/` se descongela: al terminar hay que repetir F6.

- [x] **F8.1 Fuente y E12 en `template/`.** RF-1, RF-5, RF-10, RF-14
  - Edita `template/.docs/002_specs/README.md` › « Numeración única »: (a) **empate de valor con anchos distintos** (`017_A` y `0017_B`): es un duplicado que se resuelve antes (« Revisión de duplicados »); mientras no se resuelva, el ancho de la carpeta es el del ítem publicado en la rama remota y, si ninguno lo está, el mayor ancho; (b) **desbordamiento en el clon**: un ítem con más dígitos que el ancho declarado porque el valor lo necesita (1000 en una carpeta de 3) no es un ancho distinto del declarado.
  - Edita `template/.agents/validar-estructura.md`, solo la fila E12 (no toques E20): (a) la excepción de desbordamiento del clon (misma frase que la fuente, sin repetir anchos absolutos); (b) en una carpeta con mezcla válida y un ítem inválido, el resultado es `FALLA` y no se añade `AVISO` por esa misma carpeta.
  - Hecho cuando:
    - [x] L sobre los dos archivos; `diff` contra la copia previa y `git diff --numstat` contra HEAD con solo líneas propias
    - [x] `grep -n "publicado" template/.docs/002_specs/README.md` encuentra el desempate; `grep -n "desbordamiento" template/.agents/validar-estructura.md` encuentra la excepción
    - [x] Verificación manual: No aplica
- [x] **F8.2 Huella del aviso en `ACTUALIZAR.md`.** RF-17
  - En `## Registro «mantener»`, define los límites del fragmento cuya huella identifica el aviso: desde la línea del título `## Numeración única (sin prefijos repetidos)` hasta la línea anterior al siguiente encabezado de nivel 2 (`## `), con la normalización de `## Huella` y calculada sobre ese texto como si fuera un archivo.
  - Hecho cuando:
    - [x] L sobre `ACTUALIZAR.md`; `git diff --numstat` contra HEAD con solo líneas propias (no se toca `## Clasificación` ni `## Grupos por ruta`)
    - [x] `ACTUALIZAR.md` no nombra un ancho absoluto fuera de la fuente
    - [x] Verificación manual: No aplica
- [x] **F8.3 Repetir F6.** RF-19
  - Regenera `manifiestos/0.5.0.md` (una vez) y repite E20 con las cifras reales (42 de 42, 0 diferencias). Cambian solo las filas de `template/.docs/002_specs/README.md` y `template/.agents/validar-estructura.md`.
  - Hecho cuando:
    - [x] `git diff --numstat -- manifiestos/0.5.0.md` sin filas ajenas; cabecera y orden intactos
    - [x] E20: 42 de 42, 0 diferencias, 0 sin fila, 0 sin archivo
    - [x] Verificación manual: No aplica
- [x] **F8.4 Revalidar.** RF-14, RF-20
  - Un reviewer repite en `mktemp -d` los casos de E12 afectados (duplicado por valor con anchos distintos, ancho inválido, mezcla válida, mezcla más inválido, clon con `0099_` y con `1000_`) y la huella del aviso, y confirma E20.
  - Hecho cuando:
    - [x] Cada caso da el resultado de la fila E12 nueva y la sesión principal anota la evidencia
    - [ ] Verificación manual del usuario: ver la lista de resultados

## Estimado
| Fase | Pasos | Minutos |
|---|---|---|
| F0 Preparación | F0.1–F0.2 | 5 |
| F1 Fuente única | F1.1–F1.5 | 15 |
| F2 Piezas | F2.1–F2.8 | 15 |
| F3 Chequeo E12 | F3.1–F3.2 | 6 |
| F4 `ACTUALIZAR.md` | F4.1–F4.3 | 12 |
| F5 CHANGELOG y clon | F5.1–F5.2 | 6 |
| F6 Manifiesto | F6.1–F6.2 | 5 |
| F7 Validación | F7.1–F7.6 | 25 |
| **Total** | 30 pasos | **89 min (≈ 1,5 h)** |

## Cobertura RF → pasos
| RF | Pasos |
|---|---|
| RF-1, RF-2, RF-5 | F1.1, F7.2 |
| RF-3 | F1.1, F5.2, F7.2 |
| RF-4, RF-6 | F1.2, F7.2 |
| RF-7, RF-8 | F1.3, F7.3 |
| RF-9 | F1.2, F7.3 |
| RF-10, RF-11 | F1.3, F7.3 |
| RF-12 | F0.2, F1.4, F1.5, F2.1–F2.8, F5.2 |
| RF-13 | F0.2, F2.1–F2.4, F2.6, F2.8 |
| RF-14 | F3.1, F3.2, F7.4, F7.5 |
| RF-15 | F2.4, F7.4 |
| RF-16, RF-17 | F4.1–F4.3, F7.4 |
| RF-18 | F5.1, F7.5 |
| RF-19 | F0.1, F0.2, F6.1, F6.2, F7.5 |
| RF-20 | F7.1–F7.6 |
