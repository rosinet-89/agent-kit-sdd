# 04 — Tareas: Actualizar un proyecto a una versión nueva del kit
Leyenda: `[ ]` Pendiente · `[~]` En curso · `[x]` Hecho · `[!]` Bloqueado
Spec: 01_spec.md (aprobada) · Plan: 03_plan.md · Total estimado: 14,5 h

## Reglas comunes a todos los pasos
- **Lint (L).** Cada vez que un paso diga "Lint L en `<archivos>`", se corre sobre cada archivo tocado
  (comandos de `AGENTS.md` › Comandos):
  1. `file -i <archivo>` da `charset=utf-8` (o `us-ascii`).
  2. `grep -l $'^\xEF\xBB\xBF' <archivo>` no devuelve nada (sin BOM).
  3. En archivos con frontmatter (agentes, comandos, skills): `name` = nombre de su carpeta.
  4. Rutas citadas existentes (E11: `AGENTS.md`, `.agents/**/*.md`, `.docs/**/README.md`). **No aplica**
     a `manifiestos/*.md` ni a `ACTUALIZAR.md` (RNF-9); en `ACTUALIZAR.md` se comprueba a mano que las
     rutas del kit citadas existan.
  5. En archivos de `template/`: `grep -n '<<' <archivo>` sin marcadores `<<…>>` salvo
     `<<NOMBRE_PROYECTO>>` e `<<IDIOMA>>`.
- **Ensayos solo en `mktemp -d`.** Nunca en un proyecto real ni en el kit real. Lo que se extrae con
  `git archive` va fuera del repo. Los agentes no hacen `git commit`, `push`, `pull` ni `fetch`.
- **Cambios ajenos ya presentes (no son de este feature).** `INSTALAR.md` (subsección "Refrescar la
  copia local"), `template/.agents/validar-estructura.md` (E19) y también `template/AGENTS.md`,
  `template/.docs/*/README.md` tienen cambios sin guardar. Todo paso que edite esos archivos parte de su
  contenido **actual**, no los revierte (`git checkout`/`git restore` prohibidos sobre ellos) y, antes de
  editar, guarda una copia en `mktemp -d` para mostrar con `diff` que solo se agregó lo propio del paso.
- **Orden y congelamiento.** F0 antes de F1. F4 después de F2 y F3. F5 sobre F4. **Toda edición de
  `template/` posterior a F4 obliga a repetir F4** (E20). Si en F5 se descubre un hueco en
  `ACTUALIZAR.md` (fuera de `template/`), se corrige ahí sin repetir F4.
- El manifiesto de 0.5.0 se genera sobre el **árbol de trabajo** de `template/` (no sobre `HEAD`), porque
  hay cambios sin guardar que ya forman parte de la versión.

---

## F0 — Preparación: huella y manifiesto de 0.4.0 (estimado: 1,5 h)
- [x] **F0.1 Fijar la huella y la prueba de humo (Linux).** RF-19, RF-25
  - Hacer: en un `mktemp -d` fuera del repo, repetir la prueba del análisis 3.5 con el algoritmo del plan
    (decisión 2): quitar BOM, CRLF a LF, quitar espacios/tabuladores finales, quitar líneas vacías finales
    y terminar en un solo LF; `sha256sum` en minúscula. Dejar el comando exacto (una línea o función de
    shell) anotado en la respuesta, para que F1.1 lo documente tal cual se probó. Revisar también el
    equivalente `shasum -a 256` (macOS), el de PowerShell y el de `python3 -I` para Windows, como texto
    para `ACTUALIZAR.md` (aquí solo se prueba lo que exista; lo no probado se marca "no probado").
  - Hecho cuando:
    - [x] Texto en LF, el mismo con BOM + CRLF + espacios finales, dan **la misma** huella (64 hex)
    - [x] Un texto con una palabra distinta da **otra** huella
    - [x] Archivo vacío da la huella de 0 bytes (`e3b0c442…b855`)
    - [x] Salida de las tres huellas pegada en la respuesta; el directorio temporal queda fuera del repo
    - [x] Lint L: No aplica (no se escribe ningún archivo del repo)
    - [x] Verificación manual: No aplica
- [x] **F0.2 Calcular las huellas de 0.4.0 desde `861bb99`.** RF-17, RF-19
  - Hacer: `git archive 861bb99 template | tar -x -C <mktemp -d>` (fuera del repo); calcular la huella de
    cada archivo con el comando de F0.1, con rutas relativas a la raíz del proyecto (sin el prefijo
    `template/`), ordenadas por ruta; incluir archivos ocultos (`.gitignore`, `.agents/…`, `.docs/…`).
  - Hecho cuando:
    - [x] `git ls-tree -r --name-only 861bb99 -- template | wc -l` = **35** y el listado calculado tiene 35 rutas
    - [x] `diff` entre ambas listas (sin el prefijo `template/`) vacío
    - [x] Contraprueba: huellas de 3 archivos elegidos al azar recalculadas con `python3 -I` (script temporal
          fuera del repo) coinciden
    - [x] Lint L: No aplica todavía (la tabla se escribe en F0.3)
    - [x] Verificación manual: No aplica
- [x] **F0.3 Escribir `manifiestos/0.4.0.md`.** RF-17, RF-19, RNF-5, RNF-6
  - Hacer: crear `/home/cromero/dev/agent-kit-sdd/manifiestos/0.4.0.md`: título, una frase de uso
    (de dónde salió: commit `861bb99`; algoritmo de huella remitiendo a `ACTUALIZAR.md`; no se distribuye),
    y tabla `Ruta | Huella` ordenada por ruta, una fila por archivo (35). Sin columna de grupo. Sin datos
    de proyecto.
  - Hecho cuando:
    - [x] Lint L (puntos 1 y 2; el 3, 4 y 5 no aplican) en `manifiestos/0.4.0.md`
    - [x] Conteo: `grep -c '^| `\?[.A-Za-z]' manifiestos/0.4.0.md` coincide con 35 filas de datos (ajustar el patrón al formato real) y cada huella tiene 64 hex (`grep -Ec '[0-9a-f]{64}'` = 35)
    - [x] Filas ordenadas por ruta (`LC_ALL=C sort -c` sobre la columna de rutas sin error)
    - [x] Ninguna ruta empieza con `template/` ni está fuera del conjunto de `git ls-tree` de F0.2
    - [x] Verificación manual del usuario: abrir `manifiestos/0.4.0.md` y confirmar que el formato (tabla
          `Ruta | Huella`, 35 filas, nota de origen) es el que quiere para todos los manifiestos futuros
          (confirmado 2026-10-25 11:18)

## F1 — `ACTUALIZAR.md` (estimado: 4,0 h)
Requisito: F0 cerrada (el algoritmo se documenta tal como se probó).
- [x] **F1.1 Cabecera, precondiciones, huella y prueba de humo.** RF-1, RF-19, RF-24, RF-25, RNF-1, RNF-2, RNF-7, RNF-8
  - Hacer: crear `/home/cromero/dev/agent-kit-sdd/ACTUALIZAR.md` (Markdown español, junto a `INSTALAR.md`):
    para qué sirve y qué no (sin comando nuevo, sin Git ni red necesarios); precondiciones (kit disponible
    localmente con `template/`, `VERSION` y `manifiestos/`; manifiesto del destino existe; entorno con
    SHA-256; **no correr sobre el clon del kit**, remite a la deuda 001; integridad: las huellas de
    `template/` coinciden con `manifiestos/<VERSION>.md` o no se modifica nada); definición exacta de la
    huella y su normalización; prueba de humo obligatoria previa; ejemplos por SO (Linux probado; macOS,
    PowerShell y `python3` temporal fuera de los repos, con la marca de lo no probado).
  - Hecho cuando:
    - [x] Lint L (puntos 1 y 2; el 4 se hace a mano: toda ruta del kit citada existe) en `ACTUALIZAR.md`
    - [x] El comando de huella del documento es **el mismo** de F0.1 (se vuelve a correr la prueba de humo copiando el bloque del documento)
    - [x] Sin scripts ni credenciales; los ejemplos son comandos en bloques, no archivos
    - [x] Verificación manual: No aplica (la revisión del usuario es en F1.6)
- [x] **F1.2 Versión base/destino, marcador y clasificación.** RF-1, RF-2, RF-3, RF-4, RF-5, RF-6, RF-24
  - Hacer: añadir a `ACTUALIZAR.md`: destino = versión del kit en uso (`VERSION`); lectura y validación del
    marcador `.agents/.kit-version` (válido, malformado, tolerancia a BOM/espacios, mayor que el destino);
    suposición 0.4.0 con confirmación del usuario (o versión indicada, o base sin manifiesto); "al día" si
    base = destino y sin pendientes; tabla de decisión de 5 filas con B, D, P (literal de la spec,
    primera fila que cumple); RF-6 (B no existe nunca es KIT-VIEJO); huérfanos fuera de la clasificación.
  - Hecho cuando:
    - [x] Lint L (1 y 2) en `ACTUALIZAR.md`
    - [x] La tabla de clasificación del documento tiene las 5 clases en el mismo orden y condiciones que `01_spec.md` (comparación visual lado a lado)
    - [x] Cada caso del marcador de los casos límite (vacío, varias líneas, solo espacios, BOM, no numérico, mayor, sin manifiesto) tiene una instrucción
    - [x] Verificación manual: No aplica
- [x] **F1.3 Grupos A/B/C/D por ruta y faltantes.** RF-11, RF-20, RNF-4
  - Hacer: añadir a `ACTUALIZAR.md` la regla por ruta: A (`.agents/**` salvo `napkin.md`), B (`AGENTS.md`,
    `CLAUDE.md`, `.gitignore`), C (`MEMORY.md`, `.agents/napkin.md`, todo `.docs/`), D (`PREFERENCIAS.md`);
    ruta nueva sin regla = grupo C (decisión 11); tratamiento de cada grupo; definición de faltante del
    grupo B por identidad del elemento (fila de skill, paso de "Cada sesión", regla de ignorados) usando
    el `CHANGELOG.md` como guía y lista conservadora; qué no cuenta como faltante; archivo de grupo B o C
    eliminado se ofrece como NUEVO o "mantener".
  - Hecho cuando:
    - [x] Lint L (1 y 2) en `ACTUALIZAR.md`
    - [x] Evidencia: aplicar la regla de rutas a `find template -type f` da A = 32, B = 3, C = 6, D = 0 (análisis 3.3); pegar el conteo
    - [x] Verificación manual: No aplica
- [x] **F1.4 Plan, confirmación, aplicación, "mantener" y conflictos.** RF-7, RF-8, RF-9, RF-10, RF-21, RF-23
  - Hacer: añadir a `ACTUALIZAR.md`: plan completo agrupado por clase con diferencia B→D para cada
    KIT-VIEJO y PERSONALIZADO con cambio del kit (cómo obtener la diferencia sin Git: la base no tiene
    contenido, solo huella; definir qué se muestra cuando no hay contenido de B); confirmación por archivo
    o grupo; aplicación (copiar NUEVO, reemplazar KIT-VIEJO, única excepción, respaldo opcional fuera del
    proyecto si no hay Git); PERSONALIZADO y RETIRADO nunca se tocan; "mantener" y formato exacto del
    registro `.agents/.kit-mantener` (`ruta o ruta › elemento | clase | huella del destino | versión | AAAA-MM-DD`),
    cuándo se vuelve a proponer; NUEVO borrado a propósito; ruta ocupada de otro tipo = conflicto, pendiente,
    sin sobrescribir (remite a los estados de `sync-agents.md`).
  - Hecho cuando:
    - [x] Lint L (1 y 2) en `ACTUALIZAR.md`
    - [x] Rutas citadas a `template/.agents/sync-agents.md` y sus estados (`OCUPADO`, etc.) existen: `grep -n 'OCUPADO' template/.agents/sync-agents.md` devuelve líneas
    - [x] El documento dice en un solo lugar y sin ambigüedad que reemplazar KIT-VIEJO es la única excepción a "no sobrescribir"
    - [x] Verificación manual: No aplica
- [x] **F1.5 Marcador, reporte, enlaces, commit, idempotencia y retoma.** RF-12, RF-14, RF-15, RF-16, RF-18, RNF-3
  - Hacer: añadir a `ACTUALIZAR.md`: cuándo se escribe el marcador (sin pendientes; parcial o rechazo
    total no lo mueven); formato del reporte (nuevos, reemplazados, IGUAL, respetados, pendientes,
    retirados; huérfanos aparte); verificación de enlaces con las reglas de `sync-agents.md` sin ejecutar
    nada sin confirmación; commit aparte propuesto con solo los archivos tocados (aviso de árbol sucio,
    `git-commit`, `seguridad-basica`; sin Git no aplica); idempotencia y retoma (lo ya aplicado sale
    IGUAL); sección de casos límite y errores frecuentes.
  - Hecho cuando:
    - [x] Lint L (1 y 2) en `ACTUALIZAR.md`
    - [x] Cada caso límite de `01_spec.md` (sección "Casos límite", 21 viñetas) tiene una instrucción localizable en el documento (lista de comprobación pegada en la respuesta)
    - [x] Rutas citadas (`template/.agents/skills/git-commit/SKILL.md`, `seguridad-basica`, `sync-agents.md`) existen
    - [x] Verificación manual: No aplica
- [x] **F1.6 Revisión del usuario de `ACTUALIZAR.md`.** RF-18
  - Hecho cuando:
    - [x] Verificación manual del usuario: abrir `ACTUALIZAR.md`; debe verse el índice de secciones de F1.1 a F1.5, una sola tabla de clasificación, la lista de grupos por ruta y el formato de `.kit-mantener`; el usuario responde "aprobado" o lista cambios
    - [x] Si hay cambios, se aplican y se vuelve a correr Lint L (1 y 2) en `ACTUALIZAR.md`
    - [x] No se pasa a F3 con documento sin aprobar (F3 y F5 lo citan)

## F2 — Cambios en `template/` (estimado: 1,5 h)
- [x] **F2.1 `template/AGENTS.md`: una línea en "Reglas duras".** RF-18, RF-22
  - Hacer: guardar copia previa en `mktemp -d`; agregar **una** línea: no reinstalar encima; el paso a una
    versión nueva usa el procedimiento de actualización del kit; la versión instalada está en el marcador.
    **Sin comillas inversas** en la ruta del documento ni del marcador (E11 correría en el proyecto).
  - Hecho cuando:
    - [x] Lint L (1, 2 y 5) en `template/AGENTS.md`
    - [x] `diff <copia previa> template/AGENTS.md` muestra solo esa línea añadida (los cambios ajenos previos intactos)
    - [x] `grep -n 'kit-version\|ACTUALIZAR' template/AGENTS.md` no devuelve rutas entre comillas inversas
    - [x] Verificación manual: No aplica
- [x] **F2.2 `template/.agents/README.md`: árbol.** RF-13, RF-21
  - Hacer: en el árbol, agregar `.kit-version` y `.kit-mantener` como archivos del procedimiento,
    versionables, que no pertenecen al kit; sin comillas inversas que E11 interprete como ruta inexistente
    (describir como texto de árbol, igual que el resto del árbol).
  - Hecho cuando:
    - [x] Lint L (1, 2, 4 y 5) en `template/.agents/README.md`
    - [x] `diff <copia previa> template/.agents/README.md` muestra solo lo añadido
    - [x] Verificación manual: No aplica
- [x] **F2.3 `template/.agents/validar-estructura.md`: E20, E11 y "E1–E20".** RF-17, RNF-9
  - Hacer: guardar copia previa (contiene E19, que **no** se toca); agregar E20 (solo kit): recalcula la
    huella de cada archivo de `template/` con el algoritmo de `ACTUALIZAR.md` y la compara con
    `manifiestos/<VERSION>.md`; falla por diferencia, archivo sin fila o fila sin archivo; sin aplicación
    en proyectos instalados (no hay `manifiestos/`). Aclarar en E11 que no aplica a manifiestos. Cambiar el
    texto final a "E1–E20".
  - Hecho cuando:
    - [x] Lint L (1, 2, 3 si tiene frontmatter, 4 y 5) en `template/.agents/validar-estructura.md`
    - [x] `diff <copia previa> template/.agents/validar-estructura.md` muestra solo E20, la aclaración de E11 y el cambio a "E1–E20"; E19 intacto
    - [x] `grep -c 'E20' template/.agents/validar-estructura.md` ≥ 2 y no queda ningún "E1–E19" suelto
    - [x] Verificación manual: No aplica
- [x] **F2.4 Refrescar la copia local de `.agents/` (no es entregable).** (sin RF; consecuencia de F2.2 y F2.3)
  - Hacer: copiar `napkin.md` a un `mktemp -d`; ver `diff -rq template/.agents .agents` y confirmar que solo
    difieren los archivos tocados en F2 y `napkin.md`; `cp -R template/.agents/. .agents/`; restaurar el
    `napkin.md` guardado. Pedir confirmación al usuario antes si el `diff` previo muestra diferencias
    distintas de las esperadas.
  - Hecho cuando:
    - [x] `diff -rq template/.agents .agents` muestra **solo** `napkin.md` (E19)
    - [x] `napkin.md` idéntico al guardado (`cmp`)
    - [x] `git status --short` no muestra archivos nuevos bajo `.agents/` (siguen excluidos)
    - [x] Verificación manual: No aplica

## F3 — Documentos de la raíz (estimado: 2,0 h)
Requisito: F1.6 aprobada.
- [x] **F3.1 `INSTALAR.md`: detección de kit instalado, marcador y enlace.** RF-13, RF-18, RF-22
  - Hacer: guardar copia previa (la subsección "Refrescar la copia local" es ajena y se conserva tal cual);
    agregar enlace a `ACTUALIZAR.md` en el encabezado; paso previo "¿ya tiene el kit?" (marcador o
    `.agents/skills/sdd/SKILL.md`) que remite y detiene la instalación; en **Modo proyecto**, paso que
    escribe `.agents/.kit-version` copiando la versión del archivo `VERSION` del kit usado (no a mano);
    "A mano": `printf '%s\n' <versión> > .agents/.kit-version` y su equivalente en PowerShell; modo
    mantenimiento no escribe marcador (decisión 9; si el usuario no ha confirmado la pregunta 7.1, dejar
    anotado "pendiente" en la respuesta).
  - Hecho cuando:
    - [x] Lint L (1 y 2) en `INSTALAR.md`; rutas del kit citadas existen
    - [x] `diff <copia previa> INSTALAR.md`: solo cambios de este paso; "Refrescar la copia local" intacta
    - [x] Los pasos de copia siguen sin sobrescribir (`cp -Rn`/equivalentes sin cambio)
    - [x] Verificación manual del usuario: abrir `INSTALAR.md`; debe verse el paso previo de detección, el paso del marcador solo en Modo proyecto y el enlace a `ACTUALIZAR.md`
- [x] **F3.2 `README.md`: árbol, Instalación y regla vieja.** RF-18
  - Hacer: agregar `ACTUALIZAR.md` y `manifiestos/` al árbol; en Instalación, enlace a actualizar; reescribir
    la regla "Para actualizar un proyecto existente, compara…" para que remita al procedimiento.
  - Hecho cuando:
    - [x] Lint L (1 y 2) en `README.md`
    - [x] `grep -n 'compara su' README.md` ya no devuelve la regla vieja
    - [x] `ACTUALIZAR.md` y `manifiestos/` aparecen en el árbol y existen
    - [x] Verificación manual: No aplica
- [x] **F3.3 `AGENTS.md` raíz: regla de negocio y excepción RNF-9.** RF-8, RNF-9
  - Hacer: en "Reglas de negocio y del sistema", precisar que la instalación nunca sobrescribe y que la
    actualización solo reemplaza KIT-VIEJO confirmados (ADR 007); manifiestos fuera de `template/` y no
    se distribuyen; en la línea de lint de Comandos, exceptuar los manifiestos de "rutas citadas
    existentes". Sin tocar nada más.
  - Hecho cuando:
    - [x] Lint L (1, 2 y 4) en `AGENTS.md`
    - [x] `diff <copia previa> AGENTS.md` muestra solo esas líneas
    - [x] La regla cita el ADR 007 y su ruta existe
    - [x] Verificación manual: No aplica
- [x] **F3.4 `CHANGELOG.md` y `VERSION`.** RF-17, RF-18
  - Hacer: dentro de la entrada de 0.5.0 (sin crear versión nueva), anotar: actualización de proyectos,
    manifiestos, marcador de versión, E20. `VERSION` solo se verifica.
  - Hecho cuando:
    - [x] Lint L (1 y 2) en `CHANGELOG.md`
    - [x] `cat VERSION` = `0.5.0` y la entrada más reciente de `CHANGELOG.md` es 0.5.0 con las 4 menciones
    - [x] Verificación manual: No aplica

## F4 — Congelar `template/` y manifiesto de 0.5.0 (estimado: 1,0 h)
Requisito: F2 y F3 cerradas. **Desde F4.1 `template/` queda congelado: cualquier edición posterior obliga a repetir F4.**
- [x] **F4.1 Congelar `template/` y verificarlo.** RF-17
  - Hecho cuando:
    - [x] Lint L (1, 2, 3 y 5) corrido sobre **todos** los `.md` de `template/` (`find template -type f`), sin hallazgos
    - [x] `find template -type f | wc -l` = 41 (si F2/F3 cambió el conteo, anotar el nuevo y usarlo en F4.2)
    - [x] `git status --short template` anotado como línea base del congelamiento en la respuesta (para que la sesión principal lo ponga en `05_avance.md`)
    - [x] Verificación manual: No aplica
- [x] **F4.2 Generar `manifiestos/0.5.0.md`.** RF-17, RF-19
  - Hacer: crear `/home/cromero/dev/agent-kit-sdd/manifiestos/0.5.0.md` con el mismo formato aprobado en
    F0.3 y las huellas del árbol de trabajo de `template/` (41 filas, rutas sin `template/`).
  - Hecho cuando:
    - [x] Lint L (1 y 2) en `manifiestos/0.5.0.md`
    - [x] Filas = `find template -type f | wc -l`; `diff` entre rutas del manifiesto y del `find` vacío; 64 hex en cada fila
    - [x] Contraste medido respecto de `manifiestos/0.4.0.md`: solo en 0.5.0 = **6**; solo en 0.4.0 = **0**; huella distinta = **19**; huella igual = **16**. El oráculo 3.4 decía 6 / 0 / 8 / 19
    - [x] Verificación manual: No aplica
- [x] **F4.3 Probar E20 (detecta una edición y la restauración).** RF-17, RNF-9
  - Hacer: en `mktemp -d`, copiar `template/`, `VERSION` y `manifiestos/` (no el real); aplicar E20 como
    lo describe `validar-estructura.md`; luego editar un archivo en la copia, repetir; restaurarlo,
    repetir; borrar un archivo y repetir; agregar uno y repetir.
  - Hecho cuando:
    - [x] E20 sin fallos sobre la copia limpia
    - [x] E20 falla por **diferencia** con el archivo editado, por **archivo sin fila** con el añadido y por **fila sin archivo** con el borrado
    - [x] E20 vuelve a pasar tras restaurar
    - [x] `template/` real sin cambios: `git status --short template` igual que en F4.1
    - [x] Verificación manual: No aplica

## F5 — Validación en directorio temporal (estimado: 3,5 h)
Requisito: F4 cerrada. Quien ejecuta sigue `ACTUALIZAR.md` al pie de la letra como si fuera el agente de un
proyecto. Un hueco del documento se corrige en `ACTUALIZAR.md` (y se repite el paso); si exige tocar
`template/`, se repite F4.
- [x] **F5.1 Ensayo de instalación y marcador.** RF-13, RF-22; AGENTS.md › Comandos › Test
  - Hacer: `tmp=$(mktemp -d); cp -R --update=none template/. "$tmp"/`; luego los pasos del Modo proyecto
    de `INSTALAR.md` que escriben el marcador; y la detección de kit ya instalado repitiendo la instalación.
  - Hecho cuando:
    - [x] La copia contiene los 41 archivos (`find "$tmp" -type f | wc -l`) y una segunda copia con `--update=none` no pisa nada (huellas de antes y después iguales)
    - [x] `.agents/.kit-version` tiene una línea `0.5.0` igual a `VERSION` (`cmp`)
    - [x] Al re-instalar sobre `$tmp`, el procedimiento de `INSTALAR.md` remite a `ACTUALIZAR.md` y no copia
    - [x] Verificación manual: No aplica
- [x] **F5.2 Armar el proyecto simulado 0.4.0 y la copia del kit sin `.git`.** RF-6, RF-20, RF-23
  - Hacer: en `mktemp -d` (proyecto `P`): `git archive 861bb99 template` extraído como proyecto, **sin**
    marcador, con: un A editado, un A borrado, `AGENTS.md` sin fusionar, un huérfano, un archivo en CRLF
    (equivalente en contenido a su original), un archivo en una ruta ocupada (carpeta donde va un archivo).
    En otro `mktemp -d` (kit `K`): copia de `template/`, `VERSION`, `manifiestos/`, `ACTUALIZAR.md` **sin `.git`**
    (`find K -name .git` vacío).
  - Hecho cuando:
    - [x] Lista de las 6 personalizaciones creadas pegada en la respuesta
    - [x] `find K -name .git` y `find P -name .git` vacíos (RNF-7: sin acceso al historial)
    - [x] Verificación manual: No aplica
- [x] **F5.3 Condiciones de parada y marcador anómalo (sin modificar nada).** RF-2, RF-3, RF-24, RF-25, RF-19
  - Hacer: sobre copias de `P`: marcador vacío, de varias líneas, solo espacios, con BOM+espacios (se tolera),
    no numérico; marcador `0.9.0` (mayor); destino sin manifiesto (kit `K` sin `0.5.0.md`); base `0.4.1`
    (sin manifiesto, RF-6); entorno sin SHA-256 (`PATH` restringido); prueba de humo.
  - Hecho cuando:
    - [x] Por cada caso, el comportamiento coincide con la spec (malformado: informa y procede como RF-3; mayor o destino sin manifiesto: aviso y huellas de `P` iguales antes y después; sin SHA-256: informa y no modifica)
    - [x] `diff -r` de la copia de `P` antes/después vacío en los casos de "no modificar nada"
    - [x] CRLF/BOM/espacios finales no generan PERSONALIZADO (el archivo CRLF sale IGUAL, RF-19)
    - [x] Verificación manual: No aplica
- [x] **F5.4 Clasificación y plan sobre `P` (sin aplicar).** RF-1, RF-3, RF-5, RF-7, RF-9, RF-11, RF-14, RF-20
  - Hecho cuando:
    - [x] Se pide y registra la suposición 0.4.0 con confirmación (RF-3)
    - [x] Conteos medidos (el oráculo 3.4 decía NUEVO 6 / KIT-VIEJO 8 / IGUAL 19). Limpio, solo grupo A: NUEVO 5, KIT-VIEJO 16, IGUAL 11. Con las personalizaciones: NUEVO 6, KIT-VIEJO 15, IGUAL 10, PERSONALIZADO 1, RETIRADO 0, conflicto 1. El grupo C ausente (`.docs/004_adr/README.md`) entra en los NUEVO.
    - [x] `AGENTS.md` aparece como grupo B con faltantes (3 filas de skills, `/sdd-estructura`, paso 7 ampliado, la línea de F2.1), `CLAUDE.md` y `.gitignore` sin cambios
    - [x] Huérfano informado aparte; ruta ocupada informada como conflicto (RF-23)
    - [x] El plan se muestra completo con diferencias antes de modificar nada (`diff -r` de `P` vacío)
    - [x] Verificación manual: No aplica
- [x] **F5.5 Rechazo total y aprobación parcial.** RF-8, RF-12
  - Hecho cuando:
    - [x] Rechazo total: `diff -r` de `P` vacío y marcador inexistente (o igual)
    - [x] Aprobación parcial (solo NUEVO, o un grupo): se aplica solo lo confirmado, queda pendiente listado, marcador **sin cambio**
    - [x] Verificación manual: No aplica
- [x] **F5.6 Aplicación completa, marcador y reporte.** RF-8, RF-9, RF-12, RF-14, RF-16
  - Hecho cuando:
    - [x] Tras confirmar todo lo aplicable y resolver o marcar "mantener" lo pendiente, `.agents/.kit-version` = `0.5.0` (una línea)
    - [x] KIT-VIEJO ahora con huella = destino; PERSONALIZADO, grupo B, C, D y huérfano con huella igual a antes
    - [x] Reporte con las cifras de RF-14 que cuadran con la tabla de F5.4 (pegar ambas)
    - [x] Verificación de enlaces propuesta (sin ejecutar) y, en un `P` con Git de `mktemp -d`, aviso de árbol sucio y propuesta de commit sin ejecutarlo (`git log` sin commits nuevos)
    - [x] Verificación manual: No aplica
- [x] **F5.7 Segunda corrida, interrupción y retoma.** RF-4, RF-15, RNF-3
  - Hecho cuando:
    - [x] Segunda corrida: "al día", `diff -r` vacío, sin escribir nada
    - [x] Interrupción simulada (reemplazo hecho, marcador sin escribir): al repetir, lo aplicado sale IGUAL y solo se propone lo restante; el marcador queda en `0.5.0` solo al final
    - [x] Un archivo fusionado a mano distinto de B y D aparece PERSONALIZADO hasta marcarlo "mantener"
    - [x] Verificación manual: No aplica
- [x] **F5.8 "Mantener", RETIRADO, rutas ocupadas y sin Git.** RF-6, RF-10, RF-16, RF-21, RF-23
  - Hecho cuando:
    - [x] "Mantener" sobre un A borrado a propósito y sobre un elemento de `AGENTS.md`: queda en `.agents/.kit-mantener` con el formato del documento y no se vuelve a proponer en la corrida siguiente
    - [x] Simulando un destino nuevo donde el kit cambió ese elemento (copia de `K` con un archivo modificado y su manifiesto recalculado), se vuelve a proponer
    - [x] RETIRADO sintético (fila en el manifiesto base sin archivo en el destino): solo informa y, si P difiere de B, avisa; nada se borra
    - [x] Ruta ocupada por carpeta, por enlace simbólico y carpeta real de herramienta donde iría un enlace: conflicto, contado como pendiente, sin sobrescribir ni mover
    - [x] Proyecto sin Git: sin commit propuesto y con marcador escrito
    - [x] Verificación manual: No aplica
- [x] **F5.9 Chequeos finales de lint y `/sdd-estructura`.** RNF-6, RNF-9, criterios de finalización
  - Hecho cuando:
    - [x] Lint L (los 5 puntos que apliquen) sobre cada archivo tocado: `ACTUALIZAR.md`, `manifiestos/0.4.0.md`, `manifiestos/0.5.0.md`, `INSTALAR.md`, `README.md`, `CHANGELOG.md`, `AGENTS.md`, `template/AGENTS.md`, `template/.agents/README.md`, `template/.agents/validar-estructura.md`
    - [x] Chequeos E1–E20 de `validar-estructura.md` sobre el kit, sin fallos nuevos atribuibles a este feature (E11 sin tocar manifiestos; E19 solo difiere en `napkin.md`; E20 sin fallos)
    - [x] `git status --short` lista solo archivos esperados (nuevos: `ACTUALIZAR.md`, `manifiestos/`; modificados: los de la tabla del plan); `template/` sin cambios desde F4.1 (si cambió, repetir F4)
    - [x] Verificación manual del usuario: revisar el informe de F5 (cuentas esperadas vs reales y casos límite); aprobar el cierre de pruebas

## F6 — Cierre (estimado: 1,0 h)
- [x] **F6.1 Regenerar el manifiesto si `template/` cambió.** RF-17
  - Hecho cuando:
    - [x] E20 sin fallos (o, si hubo cambio en `template/` desde F4, se repite F4.2 y F4.3 y luego E20 sin fallos)
    - [x] Verificación manual: No aplica
- [x] **F6.2 Revisión de seguridad.** RNF-5
  - Hecho cuando:
    - [x] Skill `seguridad-basica` aplicada sobre los archivos a versionar; sin credenciales, rutas personales ni datos de proyecto en `ACTUALIZAR.md`, `manifiestos/`, `INSTALAR.md`, `README.md`, `CHANGELOG.md`, `template/`
    - [x] Verificación manual: No aplica
- [x] **F6.3 Índices, deudas y propuestas (sin ejecutar commits).** RF-16, RF-17
  - Hacer: actualizar la fila 001 de `.docs/002_specs/README.md` (estatus del cierre); citar las deudas
    001 y 002 como **abiertas**, sin cambiarlas; **proponer** (sin crear) el ADR 008 (formato de
    manifiestos, huella y marcador); proponer los commits por módulo (`git add <archivos>` explícitos,
    mensajes en español, imperativo, ≤ 72 caracteres, con la atribución que corresponda) y esperar
    confirmación del usuario; no ejecutar `git commit`, `push`, `pull` ni `fetch`.
  - Hecho cuando:
    - [x] Lint L (1 y 2) en `.docs/002_specs/README.md`; prefijos `NNN_` sin repetir en `.docs/002_specs/` y `.docs/003_deuda/`
    - [x] Propuesta de commits y de ADR 008 entregada en la respuesta; deudas 001 y 002 siguen `Resuelta` (el texto pedía `Abierta`; no se reabren)
    - [x] Verificación manual del usuario: cierre confirmado el 2026-10-25 12:30; commits no; ADR 008 creado el 2026-10-25 12:31 a pedido del usuario

---

## Cobertura RF → pasos
| RF | Pasos |
|---|---|
| RF-1 | F1.1, F1.2, F5.4 |
| RF-2 | F1.2, F5.3 |
| RF-3 | F1.2, F5.3, F5.4 |
| RF-4 | F1.2, F5.7 |
| RF-5 | F1.2, F5.4 |
| RF-6 | F1.2, F5.2, F5.3, F5.8 |
| RF-7 | F1.4, F5.4 |
| RF-8 | F1.4, F3.3, F5.5, F5.6 |
| RF-9 | F1.4, F5.4, F5.6 |
| RF-10 | F1.4, F5.8 |
| RF-11 | F1.3, F5.4 |
| RF-12 | F1.5, F5.5, F5.6 |
| RF-13 | F2.2, F3.1, F5.1 |
| RF-14 | F1.5, F5.4, F5.6 |
| RF-15 | F1.5, F5.7 |
| RF-16 | F1.5, F5.6, F5.8, F6.3 |
| RF-17 | F0.2, F0.3, F2.3, F3.4, F4.1 a F4.3, F6.1, F6.3 |
| RF-18 | F1.5, F1.6, F2.1, F3.1, F3.2, F3.4 |
| RF-19 | F0.1 a F0.3, F1.1, F4.2, F5.3 |
| RF-20 | F1.3, F5.2, F5.4 |
| RF-21 | F1.4, F2.2, F5.8 |
| RF-22 | F2.1, F3.1, F5.1 |
| RF-23 | F1.4, F5.2, F5.4, F5.8 |
| RF-24 | F1.1, F1.2, F5.3 |
| RF-25 | F0.1, F1.1, F5.3 |

RNF: RNF-1, 2, 7, 8 en F1.1 y F5.2/F5.3; RNF-3 en F1.5 y F5.7; RNF-4 en F1.3; RNF-5 en F0.3 y F6.2;
RNF-6 en F0.3 y F5.9; RNF-9 en F2.3, F3.3, F4.3 y F5.9.

## Resumen de horas
| Fase | Pasos | Horas |
|---|---|---|
| F0 | 3 | 1,5 |
| F1 | 6 | 4,0 |
| F2 | 4 | 1,5 |
| F3 | 4 | 2,0 |
| F4 | 3 | 1,0 |
| F5 | 9 | 3,5 |
| F6 | 3 | 1,0 |
| **Total** | **32** | **14,5** |
