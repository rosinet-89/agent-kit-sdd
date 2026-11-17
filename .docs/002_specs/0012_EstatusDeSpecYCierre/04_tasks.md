# 04 — Tareas: Estatus de spec y criterio de cerrado
Leyenda: `[ ]` Pendiente · `[~]` En curso · `[x]` Hecho · `[!]` Bloqueado

Modo completo. Cambio al kit: solo Markdown. Build: No aplica. Consulta de datos: No aplica. No hay `sql/`. Cada fase depende de la anterior. F1–F6 editan la fuente (`template/` o la raíz). La réplica de `.agents/` espera a F7. F8 corre cuando `template/` ya no cambia. F9 y F10 no editan `template/`.

Decisiones ya aprobadas (no se reabren; se aplica la recomendación de `03_plan.md` › sección 6):

1. La leyenda vive en una sección `## Leyenda de estatus` de los README de specs. La definición de deuda, bajo `## Estados` de los README de deuda. Comandos, skill, plantilla y `AGENTS.md` remiten y no copian la tabla.
2. Chequeo nuevo E22, después de E21, solo `AVISO`. E13 no se edita.
3. Los puntos 5 y 6 de Antecedentes remiten a la leyenda, en `template/AGENTS.md` y en `AGENTS.md`.
4. La misma leyenda y la misma definición de deuda se copian en los README del clon.
5. Versión `0.8.0`. Alternativa descartada: `0.7.1`.

Total del plan: 8.1 h. 40 pasos. Un paso toca como máximo dos archivos. No se reclasifican las filas ya escritas del índice de specs ni las filas 0009, 0010 y 0011 de deuda.

## F0 — Línea base (estimado: 0.4 h)
No se edita nada. Si un chequeo falla por algo ajeno a este cambio, el paso queda `[!]` y no empieza F1.

- [x] **F0.1 Anotar E19.** —
  - Resuelto (2026-11-16 16:03): la persona acepta `claudio.md` como parte de la línea base (diferencia previa y ajena a este cambio). Línea base de E19 fuera de `.agents/local/`: `napkin.md` (distinto), `skills/caveman` (solo en la réplica) y `agents/claudio.md` (solo en la réplica). F7 exigirá que la réplica no añada rutas a ESTA lista.
  - Bloqueo previo (2026-11-16 16:01): `diff -rq template/.agents .agents` fuera de `.agents/local/` muestra `napkin.md` y `skills/caveman` (aceptados) y además `Sólo en .agents/agents: claudio.md`. Ajeno a este cambio. Decisión de la persona pendiente: aceptarlo como parte de la línea base o resolverlo antes de F1.
  - Hecho cuando:
    - [ ] Se corrió `diff -rq template/.agents .agents` y el resultado quedó en el reporte del paso (la sesión lo anota en `05_avance.md`). Este paso no crea ni edita archivos
    - [ ] Al leer la salida se ignoran las rutas bajo `.agents/local/`
    - [ ] La salida, fuera de `.agents/local/`, solo menciona `napkin.md` y `skills/caveman`, o un subconjunto. Cualquier otra ruta para el paso y se reporta
    - [ ] Build: No aplica. Evidencia de datos: No aplica. Verificación manual: No aplica

- [x] **F0.2 Anotar E20 contra `manifiestos/0.7.0.md`.** —
  - Resultado (2026-11-16 16:02, ejecutado por la sesión: el shell de Cursor rechazó el comando de Corlos dos veces): prueba de humo OK con las tres huellas (`fa0bc484…`, `6dcdde67…`, `e3b0c442…`); `manifiestos/0.7.0.md` 44 filas, 44 coinciden, 0 difieren; `template/` 44 archivos y el manifiesto 44 rutas, misma lista; manifiestos sin cambios en Git.
  - Hecho cuando:
    - [ ] En el shell, sin versionar script, se cargó `kit_huella` de `ACTUALIZAR.md` › `### Linux (probado)` y la prueba de humo de `## Prueba de humo` dio las tres huellas de su tabla (`fa0bc48477b5b4d064f39b799e388c78e64c80fe29f08aee6686a8c842112630`, `6dcdde67a08228b75fac8acbe91cf6653b1a7679025e2493f5d9d8c3190db398`, `e3b0c44298fc1c149afbf4c8996fb92427ae41e4649b934ca495991b7852b855`). Si una no coincide, se informa y no se sigue
    - [ ] Cada fila de `manifiestos/0.7.0.md` (ruta sin el prefijo `template/`) coincide con `kit_huella` de `template/<ruta>`. Cada archivo regular bajo `template/` tiene fila y no sobra ninguna
    - [ ] `manifiestos/0.4.0.md`, `manifiestos/0.5.0.md`, `manifiestos/0.6.0.md` y `manifiestos/0.7.0.md` no se modificaron
    - [ ] Build: No aplica. Evidencia de datos: No aplica. Verificación manual: No aplica

- [x] **F0.3 Ensayo de instalación en un temporal.** —
  - Resultado (2026-11-16 16:02, ejecutado por la sesión: el shell de Cursor rechazó el comando de Corlos dos veces): `cp -R --update=none template/. "$tmp"/` terminó en 0 y `$tmp/.docs/002_specs/README.md` existe, `$tmp` bajo `/tmp` (fuera del clon); tras alterar `AGENTS.md` en el temporal, el segundo `cp` no lo pisó; el temporal se borró.
  - Hecho cuando:
    - [ ] `tmp=$(mktemp -d); cp -R --update=none template/. "$tmp"/` termina en 0 y `$tmp/.docs/002_specs/README.md` existe. El temporal no es el clon ni otro proyecto
    - [ ] Tras alterar a propósito un archivo ya copiado dentro de `$tmp`, un segundo `cp -R --update=none template/. "$tmp"/` no lo pisa (`cmp` contra `template/` sigue distinto)
    - [ ] El temporal se borra al terminar (`rm -rf "$tmp"`)
    - [ ] Build: No aplica. Evidencia de datos: No aplica. Verificación manual: No aplica

## F1 — Leyenda en los dos README de specs (estimado: 1.2 h)
RF-1, RF-2, RF-7, RF-10. Depende de F0.

- [x] **F1.1 Escribir `## Leyenda de estatus` en el README de specs de `template/`.** RF-1, RF-2, RF-7
  - Corrección (2026-11-16 16:16): una revisión independiente del reviewer de Claude (15 casillas se cumplían; 6 bloqueantes de equivalencia con la spec) llevó a una segunda pasada de Corlos que los atendió; el `diff` de la sesión lo confirma. Aclaración propia de Corlos, no literal en la spec y pendiente del visto bueno de la persona: «todo paso que cuenta está hecho» exige al menos un paso de implementación (una carpeta con plan y ningún paso empezado es `Planificado`).
  - Hecho cuando:
    - [ ] `file -i template/.docs/002_specs/README.md` muestra `charset=utf-8` o `charset=us-ascii`; `grep -l $'^\xEF\xBB\xBF' template/.docs/002_specs/README.md` sale vacío
    - [ ] Existe el encabezado `## Leyenda de estatus`. La línea de estatus bajo `## Formato de cada carpeta` remite a ese encabezado y ya no enumera un subconjunto de palabras
    - [ ] El bullet de `## Reglas de mantenimiento` ya no dice que el estatus sale del `PUNTO DE RETOMO`. Aparece que el pendiente, el pendiente de desarrollo, el grupo y el cerrado salen de la celda
    - [ ] Aparecen las ocho palabras exactas: `Análisis`, `Planificado`, `En curso`, `En pruebas`, `Implementado`, `Diferido`, `Cerrado`, `Descartado`
    - [ ] Aparece la forma de la celda: la palabra exacta, seguida de ` (AAAA-MM-DD): ` y de un detalle no vacío. La fecha es la del último avance y es la única
    - [ ] Aparece: pendiente = la celda no es un `Cerrado` bien formado ni un `Descartado` bien formado. Pendiente de desarrollo = pendiente salvo `Implementado` y `Diferido`. El grupo es el de la celda. `sin clasificar` no se escribe
    - [ ] Aparece el desfase: si la celda y la etapa discrepan, los conteos siguen la celda; el estado y el chequeo avisan y no escriben
    - [ ] Aparece quién escribe: la apertura, `Análisis`; el cierre, `Implementado`, `Cerrado` o `Descartado`; la persona, `Diferido` con motivo y estatus de regreso, y al reactivar devuelve la celda. La línea del documento no cambia en `Diferido`
    - [ ] Aparecen los estados del documento en minúsculas: `borrador`, `aprobada`, `analisis`, `implementada`, `cerrada`, `descartada`. `analisis` va sin tilde. `implementada` no es cerrado
    - [ ] Aparecen la comprobación previa y los tres resultados: parar sin escribir `Implementado`, `Cerrado`, `implementada` ni `cerrada` si la previa falla; `Implementado` e `implementada` si falta un criterio; `Cerrado` y `cerrada` si los cinco pasan. Con la línea `analisis` no se exige la validación del conjunto. `Descartado` y `Diferido` no usan la previa
    - [ ] Aparecen los cinco criterios, cada uno con su dato y su valor: fila `cierre` y tabla de plan contra real; commits pendientes de este requerimiento (y `DEPLOY-<ID>` solo si el proyecto los usa); aviso o ticket en el punto de retomo; columna de pendiente solo con enlaces a deudas numeradas cuyo detalle tiene visto bueno, o vacía; deuda que bloquea por Atendida por, remitiendo al README de deuda y sin pegar esa tabla
    - [ ] Aparece el orden de etapa de la carpeta (la primera que cuadra) y el caso sin punto de retomo, para que el estado lo cite y no lo copie
    - [ ] Aparece la reapertura: `Cerrado` vuelve a `Implementado` e `implementada`; `Descartado` vuelve a `Análisis` y `borrador` si el punto de retomo no conserva otra etapa; motivo, quién y fecha quedan en el punto de retomo; una pausa no baja un `Cerrado` bien formado
    - [ ] `## Numeración única (sin prefijos repetidos)` y el párrafo «Esta es la única fuente de la regla de numeración» no cambian. El índice de la plantilla sigue vacío
    - [ ] No hay marcadores `<<…>>` nuevos. Evidencia de datos: No aplica. Verificación manual: No aplica

- [x] **F1.2 Copiar la misma leyenda en el README de specs del clon, sin tocar las filas.** RF-1, RF-2, RF-7, RF-10, RF-11
  - Resultado (2026-11-16 16:16, ejecutado por la sesión: copia mecánica exacta, no la hace Corlos porque Cursor no puede copiar): `## Leyenda de estatus` idéntica en `template/.docs/002_specs/README.md` y `.docs/002_specs/README.md` (`diff` vacío); línea 13, línea de estatus y bullet de mantenimiento iguales a los de la plantilla; la tabla `## Índice` (12 filas) no cambió (mismo hash); `## Numeración única` del clon idéntica a HEAD; utf-8 sin BOM.
  - Hecho cuando:
    - [ ] `file -i .docs/002_specs/README.md` muestra `charset=utf-8` o `charset=us-ascii`; `grep -l $'^\xEF\xBB\xBF' .docs/002_specs/README.md` sale vacío
    - [ ] `## Leyenda de estatus` contiene las mismas frases exigidas en F1.1. El bullet del punto de retomo deja de ser la fuente del pendiente, igual que en la plantilla
    - [ ] Ninguna celda de la tabla `## Índice` cambia respecto del texto anterior a este paso (sigue, entre otras, la fila `0012`). No se aplica el mapa `Plan` / `Spec aprobada` / `Implementado en local`
    - [ ] `## Numeración única (sin prefijos repetidos)` sigue remitiendo a `template/.docs/002_specs/README.md` y no se reescribe
    - [ ] Evidencia de datos: No aplica. Verificación manual: No aplica

## F2 — Definición de deuda (estimado: 0.5 h)
RF-8. Depende de F1.

- [x] **F2.1 Definir deuda cerrada, pendiente y bloqueo en la plantilla.** RF-8
  - Hecho cuando:
    - [ ] `file -i template/.docs/003_deuda/README.md` muestra `charset=utf-8` o `charset=us-ascii`; `grep -l $'^\xEF\xBB\xBF' template/.docs/003_deuda/README.md` sale vacío
    - [ ] Bajo `## Estados` siguen las cuatro palabras `Abierta`, `En atención`, `Resuelta` y `Descartada`, y la frase de que `Resuelta` y `Descartada` no se borran
    - [ ] Aparece: deuda cerrada = `Resuelta` o `Descartada`, palabra exacta, salvo `Resuelta` sin «Cómo se atendió», que es deuda pendiente sin clasificar
    - [ ] Aparece: deuda pendiente = `Abierta`, `En atención` u otra palabra. Esa otra es deuda pendiente sin clasificar
    - [ ] Aparece: bloquea `Cerrado` una `En atención` o una pendiente sin clasificar cuya columna Atendida por es este requerimiento. Una `Abierta` no bloquea. El criterio de `Cerrado` remite a `.docs/002_specs/README.md` › «Leyenda de estatus» y no pega la leyenda
    - [ ] Aparece: si el índice y el detalle discrepan, manda el detalle; hay un solo Atendida por
    - [ ] La tabla `## Índice` de la plantilla sigue vacía. Evidencia de datos: No aplica. Verificación manual: No aplica

- [x] **F2.2 Copiar la misma definición en el README de deuda del clon.** RF-8
  - Resultado (2026-11-16 17:15, ejecutado por la sesión: copia mecánica exacta, Cursor no puede copiar): la sección `## Estados` del README de deuda del clon es idéntica a la de la plantilla (`diff` vacío); el resto del archivo, incluidas las filas 0009, 0010 y 0011, no cambió respecto de la copia previa; utf-8 sin BOM.
  - Hecho cuando:
    - [ ] `file -i .docs/003_deuda/README.md` muestra `charset=utf-8` o `charset=us-ascii`; `grep -l $'^\xEF\xBB\xBF' .docs/003_deuda/README.md` sale vacío
    - [ ] Bajo `## Estados` están las mismas frases exigidas en F2.1
    - [ ] Las filas 0009, 0010 y 0011 siguen en `Abierta`. No cambia su enlace, origen, prioridad ni fecha
    - [ ] Evidencia de datos: No aplica. Verificación manual: No aplica

## F3 — Comandos de apertura, cierre y estado (estimado: 1.3 h)
RF-1, RF-3, RF-4, RF-5, RF-7, RF-8, RF-12. Solo `template/.agents/commands/`. Depende de F1 y F2. No se toca la réplica `.agents/commands/`.

- [x] **F3.1 La apertura escribe `Análisis` con la forma de la celda.** RF-1
  - Hecho cuando:
    - [ ] `file -i template/.agents/commands/sdd-spec.md` muestra `charset=utf-8` o `charset=us-ascii`; `grep -l $'^\xEF\xBB\xBF' template/.agents/commands/sdd-spec.md` sale vacío
    - [ ] Sigue creando la fila en `Análisis` y `01_spec.md` en `Estado: borrador`
    - [ ] Dice que la celda remite a la leyenda y no la repite
    - [ ] El frontmatter no gana un campo `name` (este comando no lo tiene). Cada ruta nueva entre comillas inversas existe. Evidencia de datos: No aplica. Verificación manual: No aplica

- [x] **F3.2 El cierre deja la previa y los tres resultados.** RF-1, RF-4
  - Hecho cuando:
    - [ ] `file -i template/.agents/commands/sdd-cierre.md` muestra `charset=utf-8` o `charset=us-ascii`; `grep -l $'^\xEF\xBB\xBF' template/.agents/commands/sdd-cierre.md` sale vacío
    - [ ] Ya no está la orden de cambiar siempre `01_spec.md` a `Estado: implementada`
    - [ ] Antes de escribir `Implementado`, `Cerrado`, `implementada` o `cerrada` exige la comprobación previa de `.docs/002_specs/README.md` › «Leyenda de estatus» y no la pega
    - [ ] Con la línea `analisis` no se exige la validación del conjunto, según la leyenda
    - [ ] Si la previa pasó y falta un criterio, la celda queda en `Implementado` y el documento en `implementada`. Si los cinco pasan, la celda queda en `Cerrado` y el documento en `cerrada`. Esos dos escritos no cuentan como commit pendiente según el criterio 2 de la leyenda, sin enumerar archivos
    - [ ] Los cinco criterios se citan desde `.docs/002_specs/README.md` › «Leyenda de estatus» y no se vuelven a redactar
    - [ ] Siguen la toma de hora, la propuesta de `MEMORY.md` sin escribirla y el commit solo con confirmación explícita. Evidencia de datos: No aplica. Verificación manual: No aplica

- [x] **F3.3 El descarte y las transiciones de deuda.** RF-5, RF-8
  - Hecho cuando:
    - [ ] En `template/.agents/commands/sdd-cierre.md`, el descarte escribe `Descartado` y `descartada` sin la comprobación previa. Motivo y quién quedan en el punto de retomo
    - [ ] Por cada `En atención` con Atendida por de este requerimiento, el cierre elige `Abierta` o `Descartada` con la decisión de la persona, según las transiciones de `.docs/003_deuda/README.md` › `## Estados`. Una `Abierta` cuyo origen es este sigue `Abierta`, salvo que esa decisión la descarte. La que atiende otro no se toca
    - [ ] El agente propone y no escribe la deuda: la escribe este cierre, con la decisión de la persona. No se crea deuda sin visto bueno
    - [ ] `file -i` y el `grep` del BOM siguen vacíos de BOM y en utf-8 o us-ascii, como en F3.2. Evidencia de datos: No aplica. Verificación manual: No aplica

- [x] **F3.4 La reapertura.** RF-12
  - Hecho cuando:
    - [ ] En `template/.agents/commands/sdd-cierre.md`, reabrir `Cerrado` lo devuelve a `Implementado` e `implementada`. Reabrir `Descartado` lo devuelve a `Análisis` y `borrador` si el punto de retomo no conserva otra etapa
    - [ ] Motivo, quién y fecha quedan en el punto de retomo. Lo escribe el cierre
    - [ ] Aparece que una pausa no baja un `Cerrado` bien formado: primero se reabre y después se anota la pausa
    - [ ] La codificación del archivo sigue en utf-8 o us-ascii y sin BOM. Evidencia de datos: No aplica. Verificación manual: No aplica

- [x] **F3.5 El estado informa la etapa y no modifica archivos.** RF-3, RF-7
  - Hecho cuando:
    - [ ] `file -i template/.agents/commands/sdd-status.md` muestra `charset=utf-8` o `charset=us-ascii`; `grep -l $'^\xEF\xBB\xBF' template/.agents/commands/sdd-status.md` sale vacío
    - [ ] Sigue la frase «No modifica archivos»
    - [ ] Al consultar una carpeta, la etapa es la de `## Leyenda de estatus` (la cita, no copia el orden). Si la celda no coincide, avisa y no escribe
    - [ ] Distingue el conteo del índice (la celda, sin abrir cada carpeta) de la consulta de una carpeta
    - [ ] Las deudas de esa carpeta se localizan por la columna Atendida por de `.docs/003_deuda/README.md`, sin recorrer cada detalle
    - [ ] No se añade `name` al frontmatter. Evidencia de datos: No aplica. Verificación manual: No aplica

## F4 — Skill, plantilla y AGENTS.md (estimado: 0.6 h)
RF-1, RF-6, RF-7, RNF-2. Depende de F1. La skill y la plantilla solo en `template/.agents/skills/sdd/`. Los dos `AGENTS.md` sí se editan aquí: no son la réplica de `.agents/`.

- [x] **F4.1 Sumar `cerrada` y `descartada` al `Estado` de la skill.** RF-1, RNF-2
  - Hecho cuando:
    - [ ] `file -i template/.agents/skills/sdd/SKILL.md` muestra `charset=utf-8` o `charset=us-ascii`; `grep -l $'^\xEF\xBB\xBF' template/.agents/skills/sdd/SKILL.md` sale vacío
    - [ ] `name: sdd` sigue igual (es el nombre de la carpeta `sdd`)
    - [ ] La lista de `Estado` de `01_spec.md` queda `borrador | aprobada | analisis | implementada | cerrada | descartada`. `analisis` sigue sin tilde
    - [ ] El archivo no contiene la tabla de las ocho palabras ni el encabezado `Leyenda de estatus`
    - [ ] Evidencia de datos: No aplica. Verificación manual: No aplica

- [x] **F4.2 Sumar `cerrada` y `descartada` a la plantilla de `01_spec.md`.** RF-1, RNF-2
  - Hecho cuando:
    - [ ] `file -i template/.agents/skills/sdd/plantillas.md` muestra `charset=utf-8` o `charset=us-ascii`; `grep -l $'^\xEF\xBB\xBF' template/.agents/skills/sdd/plantillas.md` sale vacío
    - [ ] La línea `Estado:` de la sección `## 01_spec.md` queda `borrador | aprobada | analisis | implementada | cerrada | descartada`
    - [ ] El resto de la plantilla no copia la leyenda. Evidencia de datos: No aplica. Verificación manual: No aplica

- [x] **F4.3 Los puntos 5 y 6 de Antecedentes en `template/AGENTS.md` remiten a la leyenda.** RF-6, RF-7
  - Hecho cuando:
    - [ ] `file -i template/AGENTS.md` muestra `charset=utf-8` o `charset=us-ascii`; `grep -l $'^\xEF\xBB\xBF' template/AGENTS.md` sale vacío
    - [ ] El punto 5 ya no dice «manda la carpeta». Dice que, si la celda y la etapa discrepan, los conteos siguen la celda, se avisa y no se escribe, y remite a `.docs/002_specs/README.md` › «Leyenda de estatus»
    - [ ] El punto 6 ya no dice que cualquier estatus lo hacen solo `/sdd-spec` y `/sdd-cierre`. Dice que la apertura deja `Análisis`, que el cierre escribe `Implementado`, `Cerrado` y `Descartado`, y que la persona escribe `Diferido` con motivo y estatus de regreso. Remite a la misma leyenda
    - [ ] Siguen los marcadores `<<NOMBRE_PROYECTO>>` e `<<IDIOMA>>`. No hay marcadores `<<…>>` nuevos
    - [ ] No existe un comando nuevo de pausa bajo `template/.agents/commands/`. Evidencia de datos: No aplica. Verificación manual: No aplica

- [x] **F4.4 Las mismas dos frases en el `AGENTS.md` de la raíz.** RF-6, RF-7
  - Hecho cuando:
    - [ ] `file -i AGENTS.md` muestra `charset=utf-8` o `charset=us-ascii`; `grep -l $'^\xEF\xBB\xBF' AGENTS.md` sale vacío
    - [ ] Los puntos 5 y 6 de `## Antecedentes` dicen lo mismo que F4.3, remitidos a `.docs/002_specs/README.md` › «Leyenda de estatus»
    - [ ] Ya no aparecen «manda la carpeta» ni «Lo hacen `/sdd-spec` y `/sdd-cierre`» en esos dos puntos
    - [ ] Evidencia de datos: No aplica. Verificación manual: No aplica

## F5 — Chequeo de estructura (estimado: 1.0 h)
RF-9. Depende de F1. Solo en `template/.agents/`. E12, E13 y E21 no cambian de hecho.

- [x] **F5.1 Añadir E22 y pasar el alcance a E1–E22.** RF-9
  - Hecho cuando:
    - [ ] `file -i template/.agents/validar-estructura.md` muestra `charset=utf-8` o `charset=us-ascii`; `grep -l $'^\xEF\xBB\xBF' template/.agents/validar-estructura.md` sale vacío
    - [ ] La fila E22 está en la tabla después de E21. Es `AVISO`, no `FALLA`, y no escribe
    - [ ] E22 cubre: palabra fuera de la lista, celda mal formada, fecha o detalle faltantes, `Cerrado`, `Descartado` o `Diferido` sin su forma, siguiente paso que no coincide, fila ausente, duplicada o desfasada de la etapa
    - [ ] E22 cita `.docs/002_specs/README.md` › «Leyenda de estatus» y no la pega. Si esa leyenda no trae `Cerrado` y `Descartado`, el aviso lo dice y no reescribe el índice
    - [ ] `## Alcance` dice que la sesión corre E1 a E22. El mensaje sin fallos dice `estructura OK (E1–E22, N archivos revisados)`
    - [ ] El texto de «Qué se comprueba» de E12, E13 y E21 queda igual que antes de este paso. E19 y E20 tampoco cambian de hecho
    - [ ] Evidencia de datos: No aplica. Verificación manual: No aplica

- [x] **F5.2 El comando de estructura dice E1–E22.** RF-9
  - Hecho cuando:
    - [ ] `file -i template/.agents/commands/sdd-estructura.md` muestra `charset=utf-8` o `charset=us-ascii`; `grep -l $'^\xEF\xBB\xBF' template/.agents/commands/sdd-estructura.md` sale vacío
    - [ ] «Qué hace» dice los chequeos E1 a E22. «Qué no hace» dice E1–E22
    - [ ] Sigue sin redefinir qué comprueba cada chequeo, sin redefinir E20, sin escribir y sin lanzar un subagente
    - [ ] No queda la cadena `E1 a E21` ni `E1–E21`. Evidencia de datos: No aplica. Verificación manual: No aplica

- [x] **F5.3 E22 cubre texto libre y deuda fuera de los cuatro valores; `reviewer.md` dice E1–E22.** RF-9
  - Hecho cuando:
    - [ ] `file -i template/.agents/validar-estructura.md` muestra `charset=utf-8` o `charset=us-ascii`; `grep -l $'^\xEF\xBB\xBF' template/.agents/validar-estructura.md` sale vacío
    - [ ] `file -i template/.agents/agents/reviewer.md` muestra `charset=utf-8` o `charset=us-ascii`; `grep -l $'^\xEF\xBB\xBF' template/.agents/agents/reviewer.md` sale vacío
    - [ ] E22 menciona como `AVISO` el texto libre en la columna de pendiente de la fila del índice de un `Cerrado` o `Implementado`, y una deuda con estado fuera de los cuatro valores o `Resuelta` sin «Cómo se atendió»
    - [ ] `grep -n 'E1–E21' template/.agents/agents/reviewer.md` sale vacío y el archivo dice E1–E22
    - [ ] Evidencia de datos: No aplica. Verificación manual: No aplica

## F6 — Nota de reclasificación (estimado: 0.4 h)
RF-10, RF-11. Depende de F1.

- [x] **F6.1 Nota en `ACTUALIZAR.md` entre el grupo C y el grupo D.** RF-10, RF-11
  - Hecho cuando:
    - [ ] `file -i ACTUALIZAR.md` muestra `charset=utf-8` o `charset=us-ascii`; `grep -l $'^\xEF\xBB\xBF' ACTUALIZAR.md` sale vacío
    - [ ] Hay un encabezado `### Reclasificación de filas ya escritas` inmediatamente después del cuerpo de `### Grupo C` y antes de `### Grupo D`
    - [ ] El mapa aparece así: `Plan` → `Planificado`; `Spec aprobada` → `Análisis` sin plan y `Planificado` con plan; `Implementado en local` → `Implementado`. Un `Cerrado` ya escrito sigue la fila de la leyenda
    - [ ] Dice que guía a la persona y que el procedimiento no reescribe el índice. Dice que no es un pendiente que bloquee `.agents/.kit-version`
    - [ ] `### Grupo C` sigue diciendo que, si existen, `.docs/` no se compara, no se reemplaza ni se muestra como faltante
    - [ ] `## Huella` no cambia. Evidencia de datos: No aplica. Verificación manual: No aplica

## F7 — Réplica (estimado: 0.4 h)
RF-1, RF-3, RF-4, RF-5, RF-9, RF-12. Depende de F3, F4 y F5. Única fase que copia `.agents/` desde `template/.agents`. No se edita a mano la réplica. No se copia `napkin.md` ni `skills/caveman`.

- [x] **F7.1 Copiar `sdd-spec.md` y `sdd-cierre.md`.** RF-1, RF-4, RF-5, RF-12
  - Hecho cuando:
    - [ ] `cp template/.agents/commands/sdd-spec.md .agents/commands/sdd-spec.md` y `cp template/.agents/commands/sdd-cierre.md .agents/commands/sdd-cierre.md`
    - [ ] `diff -q template/.agents/commands/sdd-spec.md .agents/commands/sdd-spec.md` y el mismo `diff -q` de `sdd-cierre.md` no imprimen diferencia
    - [ ] Evidencia de datos: No aplica. Verificación manual: No aplica

- [x] **F7.2 Copiar `sdd-status.md` y `sdd-estructura.md`.** RF-3, RF-9
  - Hecho cuando:
    - [ ] `cp template/.agents/commands/sdd-status.md .agents/commands/sdd-status.md` y `cp template/.agents/commands/sdd-estructura.md .agents/commands/sdd-estructura.md`
    - [ ] `diff -q` de cada par no imprime diferencia
    - [ ] Evidencia de datos: No aplica. Verificación manual: No aplica

- [x] **F7.3 Copiar `SKILL.md` y `plantillas.md`.** RF-1
  - Hecho cuando:
    - [ ] `cp template/.agents/skills/sdd/SKILL.md .agents/skills/sdd/SKILL.md` y `cp template/.agents/skills/sdd/plantillas.md .agents/skills/sdd/plantillas.md`
    - [ ] `diff -q` de cada par no imprime diferencia
    - [ ] Evidencia de datos: No aplica. Verificación manual: No aplica

- [x] **F7.4 Copiar `validar-estructura.md`.** RF-9
  - Hecho cuando:
    - [ ] `cp template/.agents/validar-estructura.md .agents/validar-estructura.md`
    - [ ] `diff -q template/.agents/validar-estructura.md .agents/validar-estructura.md` no imprime diferencia
    - [ ] Evidencia de datos: No aplica. Verificación manual: No aplica

- [x] **F7.5 Comprobar que la réplica no añade rutas y que Git no la muestra.** RF-1, RF-3, RF-4, RF-5, RF-9, RF-12
  - Resultado (2026-11-16 17:29, ejecutado por la sesión): `diff -rq template/.agents .agents` fuera de `.agents/local/` solo muestra `agents/claudio.md`, `napkin.md` y `skills/caveman`, es decir, la línea base de F0.1; los 8 archivos copiados dan `diff -q` vacío; `git status` de la réplica sale vacío (está ignorada).
  - Hecho cuando:
    - [ ] `diff -rq template/.agents .agents`, ignorando `.agents/local/`, no añade rutas respecto de la línea base anotada en F0.1. Los ocho archivos copiados no aparecen como diferencia
    - [ ] `git status --short -- .agents/commands/sdd-spec.md .agents/commands/sdd-cierre.md .agents/commands/sdd-status.md .agents/commands/sdd-estructura.md .agents/skills/sdd/SKILL.md .agents/skills/sdd/plantillas.md .agents/validar-estructura.md .agents/agents/reviewer.md` sale vacío
    - [ ] Este paso no edita archivos. Evidencia de datos: No aplica. Verificación manual: No aplica

- [x] **F7.6 Copiar `reviewer.md`.** RF-9
  - Hecho cuando:
    - [ ] `cp template/.agents/agents/reviewer.md .agents/agents/reviewer.md`
    - [ ] `diff -q template/.agents/agents/reviewer.md .agents/agents/reviewer.md` no imprime diferencia
    - [ ] Evidencia de datos: No aplica. Verificación manual: No aplica

## F8 — Versión, changelog y manifiesto (estimado: 0.7 h)
Criterio de finalización, E20. Depende de F1–F7. Número aprobado: `0.8.0`. Después de esta fase, `template/` no se vuelve a editar.

- [x] **F8.1 Dejar `VersionAgentKitSdd` en `0.8.0`.** Criterio de finalización
  - Hecho cuando:
    - [ ] `file -i VersionAgentKitSdd` muestra `charset=utf-8` o `charset=us-ascii`; `grep -l $'^\xEF\xBB\xBF' VersionAgentKitSdd` sale vacío
    - [ ] El archivo tiene una sola línea y es `0.8.0`
    - [ ] Evidencia de datos: No aplica. Verificación manual: No aplica

- [x] **F8.2 Entrada `0.8.0` en el changelog.** Criterio de finalización, RF-11
  - Hecho cuando:
    - [ ] `file -i ChangelogAgentKitSdd.md` muestra `charset=utf-8` o `charset=us-ascii`; `grep -l $'^\xEF\xBB\xBF' ChangelogAgentKitSdd.md` sale vacío
    - [ ] Abre una entrada `## 0.8.0 — <fecha>` con la fecha de `date '+%Y-%m-%d'` del día en que corre el paso, encima de `## 0.7.0 — 2026-11-03`
    - [ ] Describe el comportamiento nuevo: estatus terminal `Cerrado` y `Descartado`, tres resultados del cierre y el chequeo E22
    - [ ] Dice que `AGENTS.md` es grupo B y se incorpora a mano
    - [ ] Repite el mapa de `ACTUALIZAR.md` › `### Reclasificación de filas ya escritas` y no redacta otra leyenda
    - [ ] Evidencia de datos: No aplica. Verificación manual: No aplica

- [x] **F8.3 Regenerar entero `manifiestos/0.8.0.md`.** E20, criterio de finalización
  - Resultado (2026-11-16 17:29, implementer de Claude; verificado de forma independiente por la sesión): prueba de humo con las tres huellas; `manifiestos/0.8.0.md` con 44 filas, 44 coinciden al recalcularlas, misma lista que `find template -type f` (44); manifiestos 0.4.0 a 0.7.0 sin cambios en Git. `VersionAgentKitSdd` = `0.8.0`; entrada `## 0.8.0 — 2026-11-16` del changelog con el comportamiento nuevo, la nota de grupo B y el mapa de reclasificación.
  - Hecho cuando:
    - [ ] La prueba de humo de `ACTUALIZAR.md` › `## Prueba de humo`, con `kit_huella` de `### Linux (probado)`, dio otra vez las tres huellas de esa tabla. La función corre en el shell y no se versiona
    - [ ] Existe `manifiestos/0.8.0.md`, con el formato de `manifiestos/0.7.0.md`: título `# Manifiesto 0.8.0`, una línea que dice que salió de `template/` al congelar la versión 0.8.0 y que la huella es la de `ACTUALIZAR.md`, y la tabla `Ruta | Huella`
    - [ ] Cada ruta es la de un archivo regular bajo `template/`, sin el prefijo `template/`, ordenada con `LC_ALL=C sort`. La huella es la de `kit_huella` (SHA-256 hexadecimal en minúsculas, 64 caracteres). No hay fila de más ni archivo de `template/` sin fila
    - [ ] `manifiestos/0.4.0.md`, `manifiestos/0.5.0.md`, `manifiestos/0.6.0.md` y `manifiestos/0.7.0.md` no cambiaron. Las huellas no se editaron a mano
    - [ ] `file -i manifiestos/0.8.0.md` muestra `charset=utf-8` o `charset=us-ascii` y no tiene BOM. A `manifiestos/` no se le exige que cada ruta citada exista fuera de esta tabla. Evidencia de datos: No aplica. Verificación manual: No aplica

## F9 — ADR del clon (estimado: 0.9 h)
Decisiones 1 a 4. Puede ir después de F8. No toca `template/` ni `template/.docs/004_adr/README.md`. Cada archivo sigue la plantilla de la skill `adr` (`.agents/skills/adr/SKILL.md`): Estado, Fecha, Origen, Contexto, Decisión, Alternativas descartadas, Consecuencias. Estado `Aceptado`. Origen `0012_EstatusDeSpecYCierre`. Fecha: la de `date '+%Y-%m-%d'` del día del paso. Un ADR no enlaza reescribiendo `03_plan.md`: ese plan ya nombra 0024–0027. La fila del índice usa la tabla de `.docs/004_adr/README.md`.

- [x] **F9.1 ADR 0024, decisión 1, y su fila.** Decisiones
  - Hecho cuando:
    - [ ] Existe `.docs/004_adr/0024_LeyendaDeEstatusEnElIndice.md`. `file -i` muestra `charset=utf-8` o `charset=us-ascii` y `grep -l $'^\xEF\xBB\xBF'` sale vacío
    - [ ] La decisión es la sección `## Leyenda de estatus` en el README de specs y la definición bajo `## Estados` en el README de deuda. Comandos, skill, plantilla y `AGENTS.md` remiten y no copian la tabla
    - [ ] La alternativa descartada es una pieza nueva al estilo de `topes-y-modos.md`, o copiar la tabla en cada comando
    - [ ] `.docs/004_adr/README.md` tiene la fila `0024` con estado `Aceptado`, la fecha del paso y origen `0012_EstatusDeSpecYCierre`. El índice de ADR de `template/` no se toca
    - [ ] Evidencia de datos: No aplica. Verificación manual: No aplica

- [x] **F9.2 ADR 0025, decisión 2, y su fila.** Decisiones
  - Hecho cuando:
    - [ ] Existe `.docs/004_adr/0025_AvisoDeEstatusTrasE21.md`, en utf-8 o us-ascii y sin BOM
    - [ ] La decisión es E22 después de E21, solo `AVISO`, sin escribir. E13 no se edita
    - [ ] La alternativa descartada es ampliar E13
    - [ ] `.docs/004_adr/README.md` tiene la fila `0025` en el mismo formato que la de 0024
    - [ ] Evidencia de datos: No aplica. Verificación manual: No aplica

- [x] **F9.3 ADR 0026, decisión 3, y su fila.** Decisiones
  - Hecho cuando:
    - [ ] Existe `.docs/004_adr/0026_AntecedentesRemitenALaLeyenda.md`, en utf-8 o us-ascii y sin BOM
    - [ ] La decisión es que los puntos 5 y 6 de Antecedentes remiten a la leyenda, en `template/AGENTS.md` y en `AGENTS.md`
    - [ ] La alternativa descartada es dejar «manda la carpeta» y «lo hacen `/sdd-spec` y `/sdd-cierre`»
    - [ ] `.docs/004_adr/README.md` tiene la fila `0026`
    - [ ] Evidencia de datos: No aplica. Verificación manual: No aplica

- [x] **F9.4 ADR 0027, decisión 4, y su fila.** Decisiones
  - Hecho cuando:
    - [ ] Existe `.docs/004_adr/0027_LeyendaCopiadaEnElIndiceDelClon.md`, en utf-8 o us-ascii y sin BOM
    - [ ] La decisión es copiar la leyenda en `.docs/002_specs/README.md` y la definición de deuda en `.docs/003_deuda/README.md`, en el mismo cambio que `template/`
    - [ ] La alternativa descartada es remitir a `template/` como ya hace la numeración
    - [ ] `.docs/004_adr/README.md` tiene la fila `0027`. No hay hueco ni número ocupado por un rechazo: 0024, 0025, 0026 y 0027 existen
    - [ ] Evidencia de datos: No aplica. Verificación manual: No aplica

## F10 — Validación (estimado: 0.7 h)
RF-1 a RF-12. Depende de F0–F9. No reclasifica filas y no edita `template/`. Cada criterio se comprueba leyendo el archivo que nombra `03_plan.md` › sección 8. Si uno falla, el paso queda `[!]` y se reporta; no se corrige dentro de F10.

- [x] **F10.1 Codificación de cada archivo escrito.** Criterio de finalización
  - Resultado (2026-11-16 17:30, ejecutado por la sesión): los 31 archivos de la lista en utf-8 o us-ascii, sin BOM y existentes; `name: sdd` en `template/.agents/skills/sdd/SKILL.md` y su réplica; ningún comando tocado tiene `name`; las únicas rutas citadas que no resuelven (`.github/copilot-instructions.md` y `skills/sdd/topes-y-modos.md` en el changelog, `skills/sdd/SKILL.md` en `validar-estructura.md`) ya estaban en HEAD y el diff no añade líneas que las citen; en `template/` los marcadores `<<…>>` siguen siendo `<<NOMBRE_PROYECTO>>`, `<<IDIOMA>>` y el `<<…>>` que ya traía `validar-estructura.md`.
  - Hecho cuando:
    - [ ] En cada ruta de la lista de abajo, `file -i` muestra `charset=utf-8` o `charset=us-ascii` y `grep -l $'^\xEF\xBB\xBF'` sale vacío
    - [ ] La lista es: `template/.docs/002_specs/README.md`, `.docs/002_specs/README.md`, `template/.docs/003_deuda/README.md`, `.docs/003_deuda/README.md`, `template/.agents/commands/sdd-spec.md`, `.agents/commands/sdd-spec.md`, `template/.agents/commands/sdd-cierre.md`, `.agents/commands/sdd-cierre.md`, `template/.agents/commands/sdd-status.md`, `.agents/commands/sdd-status.md`, `template/.agents/commands/sdd-estructura.md`, `.agents/commands/sdd-estructura.md`, `template/.agents/validar-estructura.md`, `.agents/validar-estructura.md`, `template/.agents/agents/reviewer.md`, `.agents/agents/reviewer.md`, `template/.agents/skills/sdd/SKILL.md`, `.agents/skills/sdd/SKILL.md`, `template/.agents/skills/sdd/plantillas.md`, `.agents/skills/sdd/plantillas.md`, `template/AGENTS.md`, `AGENTS.md`, `ACTUALIZAR.md`, `VersionAgentKitSdd`, `ChangelogAgentKitSdd.md`, `manifiestos/0.8.0.md`, `.docs/004_adr/0024_LeyendaDeEstatusEnElIndice.md`, `.docs/004_adr/0025_AvisoDeEstatusTrasE21.md`, `.docs/004_adr/0026_AntecedentesRemitenALaLeyenda.md`, `.docs/004_adr/0027_LeyendaCopiadaEnElIndiceDelClon.md`, `.docs/004_adr/README.md`
    - [ ] `name` del frontmatter de `template/.agents/skills/sdd/SKILL.md` y de su réplica es `sdd`. Los comandos tocados siguen sin campo `name`
    - [ ] Cada ruta citada con comillas inversas en los archivos tocados, salvo `manifiestos/`, existe
    - [ ] En `template/` los únicos marcadores `<<…>>` siguen siendo `<<NOMBRE_PROYECTO>>` e `<<IDIOMA>>`
    - [ ] Build: No aplica. Evidencia de datos: No aplica. Verificación manual: No aplica

- [x] **F10.2 Comprobar RF-1 y RF-2.** RF-1, RF-2
  - Hecho cuando:
    - [ ] RF-1: `sdd-spec.md` crea la fila en `Análisis` y el documento en `borrador`, y la forma de la celda remite a la leyenda. `sdd-cierre.md` solo escribe `Implementado`, `Cerrado` o `Descartado`, y el documento `implementada`, `cerrada` o `descartada`. `SKILL.md` y `plantillas.md` listan `cerrada` y `descartada` junto a `borrador`, `aprobada`, `analisis` e `implementada`. Cada archivo coincide con su copia en `.agents/` (`diff -q` vacío)
    - [ ] RF-2: la `## Leyenda de estatus` de `template/.docs/002_specs/README.md` y la de `.docs/002_specs/README.md` nombran las ocho palabras, pendiente, pendiente de desarrollo, grupo y cerrado, y dicen que la respuesta es la celda. `sdd-status.md` distingue el conteo del índice de la consulta de una carpeta y no pide abrir cada carpeta para ese conteo
    - [ ] Evidencia de datos: No aplica. Verificación manual: No aplica

- [x] **F10.3 Comprobar RF-3, RF-4, RF-5 y RF-12.** RF-3, RF-4, RF-5, RF-12
  - Hecho cuando:
    - [ ] RF-3: `template/.agents/commands/sdd-status.md` y su réplica informan la etapa de esa carpeta, avisan si la celda no coincide, declaran que no modifican archivos y localizan deudas por la columna Atendida por, sin recorrer cada detalle
    - [ ] RF-4: `sdd-cierre.md` exige la previa y los cinco criterios, y deja uno de tres resultados (parar sin escribir esas palabras; `Implementado` e `implementada` si falta un criterio; `Cerrado` y `cerrada` si los cinco pasan). Esos dos documentos no cuentan como commit pendiente. Con la línea `analisis` no exige la validación del conjunto. Ya no está la orden que fijaba siempre `implementada`. Los criterios citan la leyenda y no la vuelven a redactar
    - [ ] RF-5: el tramo de descarte de `sdd-cierre.md` escribe `Descartado` y `descartada` sin la previa, y elige por cada `En atención` de este requerimiento según las transiciones de deuda
    - [ ] RF-12: el tramo de reapertura devuelve `Cerrado` a `Implementado` e `implementada`, y `Descartado` a `Análisis` y `borrador` si el punto de retomo no conserva otra etapa. Motivo, quién y fecha quedan en el punto de retomo. Una pausa no baja un `Cerrado` bien formado
    - [ ] Evidencia de datos: No aplica. Verificación manual: No aplica

- [x] **F10.4 Comprobar RF-6 y RF-7.** RF-6, RF-7
  - Hecho cuando:
    - [ ] RF-6: no hay un comando nuevo de pausa en `template/.agents/commands/` ni en `.agents/commands/`. La leyenda dice que la persona escribe `Diferido` con motivo y estatus de regreso y que al reactivar devuelve esa celda, y que la línea del documento no cambia. Los puntos 5 y 6 de `template/AGENTS.md` y de `AGENTS.md` ya no dicen que solo `/sdd-spec` y `/sdd-cierre` actualizan cualquier estatus
    - [ ] RF-7: la leyenda dice que, si la celda y la etapa discrepan, los conteos siguen la celda y el estado y el chequeo avisan y no escriben. `sdd-status.md` y la fila E22 citan ese párrafo
    - [ ] Evidencia de datos: No aplica. Verificación manual: No aplica

- [x] **F10.5 Comprobar RF-8 y RF-9.** RF-8, RF-9
  - Hecho cuando:
    - [ ] RF-8: los dos README de deuda dicen qué deuda está cerrada y cuál está pendiente, y que una `En atención` o una pendiente sin clasificar con Atendida por de este requerimiento bloquea `Cerrado`. Siguen `Abierta`, `En atención`, `Resuelta` y `Descartada`. Las filas 0009, 0010 y 0011 siguen en `Abierta`
    - [ ] RF-9: E22 existe después de E21 en `template/.agents/validar-estructura.md` y en la réplica, es `AVISO`, no escribe y cubre palabra fuera de la lista, celda mal formada, fecha o detalle faltantes, siguiente paso que no coincide, fila ausente, duplicada o desfasada. El alcance, el mensaje `estructura OK` y `sdd-estructura.md` dicen E1–E22. El hecho de E12, E13 y E21 es el que había antes de F5
    - [ ] Evidencia de datos: No aplica. Verificación manual: No aplica

- [x] **F10.6 Comprobar RF-10 y RF-11.** RF-10, RF-11
  - Hecho cuando:
    - [ ] RF-10: `ACTUALIZAR.md` › `### Reclasificación de filas ya escritas` trae el mapa `Plan` → `Planificado`, `Spec aprobada` → `Análisis` sin plan y `Planificado` con plan, `Implementado en local` → `Implementado`. Las celdas de `## Índice` en `.docs/002_specs/README.md` no fueron reescritas por F1 (siguen el texto previo a esa fase, salvo la leyenda y el bullet nuevos)
    - [ ] RF-11: la nota dice que el procedimiento no reescribe el índice. `### Grupo C` sigue sin comparar ni reemplazar `.docs/`. La entrada `## 0.8.0` del changelog repite el mapa y no es una tercera leyenda
    - [ ] Evidencia de datos: No aplica. Verificación manual: No aplica

- [x] **F10.7 Cerrar con E19, E20, el ensayo y RNF-1.** RF-1 a RF-12, RNF-1
  - Resultado (2026-11-16 17:32): RF-1 a RF-12 y RNF-1 a RNF-4 comprobados por el reviewer de Claude sobre el estado definitivo (F10.2 a F10.6); E19 igual a la línea base, E20 44 de 44 y ensayo de instalación sin pisar, por la sesión (el reviewer recalculó E20 de forma independiente: coincide).
  - Hecho cuando:
    - [ ] `diff -rq template/.agents .agents`, ignorando `.agents/local/`, es la línea base de F0.1: no añade rutas. Los archivos de `template/.agents` replicados en F7 no salen como diferencia
    - [ ] Recalcular con `kit_huella` (`ACTUALIZAR.md` › `### Linux`) coincide con `manifiestos/0.8.0.md`: ni falta una fila, ni sobra una fila, ni la huella difiere. `VersionAgentKitSdd` sigue siendo la sola línea `0.8.0`
    - [ ] `tmp=$(mktemp -d); cp -R --update=none template/. "$tmp"/` copia sin pisar, en un temporal y nunca en un proyecto real. `$tmp/.docs/002_specs/README.md` contiene `## Leyenda de estatus`. El temporal se borra al terminar
    - [ ] Dos lecturas de las mismas celdas de la leyenda, la misma línea de documento y la misma fila `cierre` dan el mismo pendiente, el mismo pendiente de desarrollo, los mismos grupos y la misma respuesta de cerrado (RNF-1). El ejemplo se arma con la leyenda, sin modificar índices
    - [ ] Build: No aplica. Evidencia de datos: No aplica. Verificación manual: No aplica
