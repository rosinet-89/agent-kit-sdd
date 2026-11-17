# Índice de planes de implementación

Punto de control de `.docs/002_specs/`. Sirve para saber, **antes de analizar algo
nuevo**, si ya se tocó ese proceso y qué carpeta leer, sin recorrerlas todas.

## Cómo usarlo

1. Busca aquí el módulo, la pantalla, la tabla o la palabra clave de lo que vas a analizar.
2. Si hay coincidencia, lee **solo esa carpeta**: primero el `PUNTO DE RETOMO` de su `05_avance.md`.
3. Si la coincidencia es parcial, anótala como dependencia en `02_analisis_impacto.md`.
4. Revisa también la deuda técnica abierta del módulo: `.docs/003_deuda/README.md`.
5. Este archivo se actualiza en cada `cierre` de feature (`/sdd-cierre`) y al abrir una carpeta
   nueva (`/sdd-spec`). Si la fila del índice no coincide con la carpeta, al consultar esa carpeta manda la carpeta; para responder el pendiente desde el índice manda la celda y no se abren las carpetas.

## Formato de cada carpeta

Toda carpeta nace en formato SDD (skill `sdd`): `01_spec` … `06_flujo`, `05_avance.md`, `sql/`. El modo ligero admite menos documentos (`.agents/skills/sdd/topes-y-modos.md`).
Una carpeta sin `01_spec.md` es de otro origen: preguntar al usuario antes de seguir.

Estatus: ver "Leyenda de estatus".

## Leyenda de estatus

La respuesta del índice es la celda.

La celda, al escribirla, empieza por la palabra exacta, con sus mayúsculas y tildes, seguida de ` (AAAA-MM-DD): ` y de un detalle no vacío. La fecha es la del último avance y es la única.

Palabras: `Análisis`, `Planificado`, `En curso`, `En pruebas`, `Implementado`, `Diferido`, `Cerrado`, `Descartado`.

El grupo es el de la celda: `Análisis` y `Planificado`, diseño; `En curso` y `En pruebas`, desarrollo; `Implementado`, por cerrar; `Diferido`, aparte, y se informa aparte; `Cerrado`, cerrado; `Descartado`, no se hará; `sin clasificar`, sin clasificar (pendiente sí). La etapa de la carpeta no asigna grupo ni pendiente.

Pendiente = la celda no es un `Cerrado` bien formado ni un `Descartado` bien formado. Pendiente de desarrollo = pendiente salvo `Implementado` y `Diferido`. Incluye diseño, el grupo desarrollo y `sin clasificar`. No es el grupo desarrollo.

La línea del documento es la línea `Estado:` de `01_spec.md`. Bien formado: `Cerrado` exige la forma de la celda, la línea del documento en `cerrada` y una sola fila `cierre` (leer no juzga los criterios); `Descartado` exige la forma de la celda, la línea del documento en `descartada`, y el motivo y quién en la columna de pendiente de la fila del índice; `Diferido` exige la forma de la celda, el motivo y las deudas en esa columna, y el motivo y el estatus de regreso en la anotación de la pausa del punto de retomo. En `Análisis`, `Planificado`, `En curso`, `En pruebas` e `Implementado` la palabra clasifica aunque falten fecha o detalle, y el chequeo avisa. Una palabra fuera de la lista, una fila del índice ausente, o un `Cerrado`, `Descartado` o `Diferido` sin su forma se leen como `sin clasificar`. `sin clasificar` no se escribe.

Si la celda y la etapa discrepan, los conteos siguen la celda. Un desfase en `Planificado`, `En curso` o `En pruebas` no cambia los conteos. El estado y el chequeo avisan y no escriben.

Estados de la línea del documento, en minúsculas: `borrador`, `aprobada`, `analisis`, `implementada`, `cerrada`, `descartada`. `analisis` va sin tilde. `implementada` no es cerrado. `cerrada` y `descartada` solo acompañan a un `Cerrado` o un `Descartado` bien formados.

Quién escribe: la apertura, `Análisis`; el cierre, `Implementado`, `Cerrado` o `Descartado`; la persona, `Diferido` con motivo y estatus de regreso, y al reactivar devuelve la celda. La línea del documento no cambia en `Diferido`. El cierre no escribe `Planificado`, `En curso`, `En pruebas` ni `Diferido`. La persona anota la pausa en el punto de retomo y al reactivar la retira. Sin fila del índice, el cierre crea esa única fila del índice. Con la fila del índice duplicada, el cierre se detiene sin escribir.

Un paso que cuenta es una fila de implementación en la tabla Pasos de `05_avance.md`. Hecho: `[x]`. Empezado: `[x]`, `[~]` o `[!]`. Paso vacío: ni empezado ni hecho. Prueba, flujo y cierre no cuentan. Un paso diferido, con decisión y deuda propuesta, deja de contar. Hay plan si, en completo, está el documento de plan y, en ligero, el plan va en las tareas.

Comprobación previa, antes de escribir `Implementado`, `Cerrado`, `implementada` o `cerrada`: todo paso de implementación que cuenta está hecho; no hay verificaciones manuales pendientes en pruebas ni en el punto de retomo; el flujo está aprobado si el modo lo incluye; no hay `pausa por confirmar`; el punto de retomo registra la validación del conjunto aprobada. Tres resultados: si la previa falla, parar sin escribir `Implementado`, `Cerrado`, `implementada` ni `cerrada`; si falta un criterio, `Implementado` e `implementada` y se enumeran los que faltan; si los cinco pasan, `Cerrado` y `cerrada`. Con la línea `analisis` no se exige la validación del conjunto («todo paso está hecho» se cumple si no queda un paso de implementación abierto). `Descartado` y `Diferido` no usan la previa. `implementada` y `cerrada` son el resultado del cierre y no cuentan como commit pendiente.

Criterios, cada uno con su dato y su valor:

1. Fila `cierre` y tabla de plan contra real. Valor: una sola fila `cierre` y una sola tabla; si ya existen, se actualizan. Vale para `Cerrado` y para `Implementado`.
2. Commits pendientes de este requerimiento: los cambios sin registrar de este requerimiento, salvo los archivos que el mismo cierre escribe en la corrida. Valor: no queda ninguno. `DEPLOY-<ID>` solo si el proyecto los usa.
3. Aviso o ticket en el punto de retomo. El aviso aplica si hay una comunicación abierta anotada; el ticket, si la spec o el punto de retomo lo nombran. Valor: si había aviso o ticket, se cumplió ahí; si hay ticket y no consta la confirmación, falla; sin nota y sin ticket, se cumple. El agente no cierra ni consulta el gestor.
4. Columna de pendiente de la fila del índice. Valor, en `Cerrado` e `Implementado`: solo enlaces a deudas numeradas cuyo detalle tiene visto bueno, o vacía. Sin visto bueno, falla. Un texto libre no es enlace. En `Análisis`, `Planificado`, `En curso` y `En pruebas`, ese texto es el siguiente paso: no coincide si nombra `plan`, `tareas`, `flujo`, `validar` o `cierre` y eso ya está (documento de plan, tareas, documento de flujo, validación registrada en el punto de retomo, o fila `cierre`); si no nombra ninguno, no hay aviso.
5. Deuda que bloquea por Atendida por. Valor: no queda una que bloquee. Qué deuda bloquea y las transiciones están en `.docs/003_deuda/README.md` › Estados. Sin la decisión de la persona, falla.

Productivo y los cambios de datos no son criterio de `Cerrado`. No existe `Liberado`.

Etapa de la carpeta (la primera que cuadra). El estado la cita y no la copia. «Todo paso que cuenta está hecho» exige al menos un paso de implementación, de modo que una carpeta con plan y ningún paso empezado es `Planificado`.

1. Pausa anotada en el punto de retomo → `Diferido`, salvo un `Cerrado` bien formado.
2. Validación del conjunto aprobada y, si el modo incluye flujo, flujo aprobado → `Implementado`.
3. Todo paso que cuenta está hecho y falta esa validación o ese flujo → `En pruebas`.
4. Hay un paso empezado y no todos → `En curso`.
5. Hay plan y ninguno empezado → `Planificado`.
6. Si no → `Análisis`.

El modo sale de la fila de modo del punto de retomo. Sin fila de modo no se exige el flujo. Carpeta anterior sin fila de modo: con plan, `Planificado`; sin plan, `Análisis`. La validación y el flujo están aprobados solo si el punto de retomo los registra. Sin punto de retomo, manda la línea del documento: `implementada` → `Implementado`, `descartada` → `Descartado`, `analisis` → `Análisis`, `cerrada` solo si `Cerrado` está bien formado, y el resto → `Análisis`.

Reapertura: `Cerrado` y `Descartado` son terminales; reabrirlos lo decide la persona. `Cerrado` vuelve a `Implementado` e `implementada`. `Descartado` vuelve a `Análisis` y `borrador` si el punto de retomo no conserva otra etapa. Motivo, quién y fecha quedan en el punto de retomo. Lo escribe el cierre. Una pausa no baja un `Cerrado` bien formado.

## Índice

| Prefijo | Carpeta | Qué resuelve | Estatus (al último avance) | Palabras clave | Pendiente / deuda |
|---|---|---|---|---|---|

## Mapa por tema

| Tema | Carpetas |
|---|---|

## Relaciones entre proyectos

(vacío)

## Reglas de mantenimiento

- Una fila por carpeta, con estatus, fecha del último avance y pendiente. Sin detalle: el detalle
  vive en la carpeta.
- El pendiente, el pendiente de desarrollo, el grupo y el cerrado salen de la celda (ver "Leyenda de estatus").
- Numeración: ver "Numeración única". No se reutilizan números, aunque queden huecos.

## Numeración única (sin prefijos repetidos)

Esta es la única fuente de la regla de numeración. Las demás piezas remiten aquí.

Las carpetas de sección de `.docs/` se quedan en **3 dígitos** y no se renumeran:
`001_constitucion`, `002_specs`, `003_deuda`, `004_adr`. No son ítems.

**Ancho.**
- Un **ítem** es una carpeta de `.docs/002_specs/`, o un archivo de `.docs/003_deuda/` o `.docs/004_adr/`, cuyo nombre empieza con dígitos y `_`. `README.md` y los marcadores de carpeta vacía no son ítems. Un nombre que no empieza por dígitos y `_` (`abc_Mal`) no entra en el consecutivo ni en el ancho y no se renombra.
- **Ancho válido**: un prefijo numérico vale su entero (`17` = `017` = `0017` = `00000017`). Los ceros de relleno y el ancho escrito no son fallo y el nombre no se reescribe. No se marca fallo por el ancho escrito. Dos ítems de la misma carpeta con el mismo entero son duplicado y es `FALLA`. Esa igualdad no es `AVISO`. `015` y `0015` son el mismo entero. Un prefijo relleno cuenta para el entero y el duplicado, y no fija el ancho de la carpeta con su cantidad de dígitos. El desbordamiento sin relleno (`1000` en ancho 3) sigue en el ADR 014 y no es este borde.
- **Ancho de una carpeta**: el del ítem de mayor valor numérico, sin que un prefijo relleno lo fije con su cantidad de dígitos. En un proyecto, un ítem nuevo conserva el ancho de su carpeta. **Empate de valor con anchos distintos** (`017_A` y `0017_B`): es un duplicado que se resuelve antes (« Revisión de duplicados »); mientras no se resuelva, el ancho de la carpeta es el del ítem publicado en la rama remota y, si ninguno lo está, el mayor ancho. Cada carpeta (specs, deuda, ADR) lleva el suyo: un ADR nuevo en una carpeta de ADR de 3 dígitos usa 3 aunque las specs estén en 4.
- **Carpeta vacía**: usa el ancho del ítem de mayor valor numérico entre las otras dos carpetas con ítems; si empatan en valor, manda el orden specs, deuda, ADR. Si ninguna tiene ítems (proyecto nuevo), usa 4 dígitos.
- **Clon del kit** (hay `template/`, `VersionAgentKitSdd` y `ChangelogAgentKitSdd.md` en la raíz): un ítem nuevo se escribe con 4 dígitos. Los ya creados no se renumeran (`001` sigue `001`). En el clon, el ancho ya escrito no es `FALLA` y una mezcla válida no es `AVISO`. El `AVISO` de mezcla válida sigue siendo solo de proyecto. El chequeo E12 lo comprueba. Un ítem con más dígitos que el ancho declarado porque su valor los necesita (1000 en una carpeta de 3) es un desbordamiento, no un ancho distinto del declarado.
- **Desbordamiento**: si el valor siguiente necesita más dígitos que el ancho de la carpeta (1000 en una de 3), se usan los que necesita, sin rellenar ni truncar. La carpeta pasa a tener anchos mezclados y su ancho pasa a ser el del ítem de mayor valor.

Ejemplos de rutas (sin número):

```
.docs/002_specs/<prefijo>_SpecUno/
.docs/003_deuda/<prefijo>_DeudaUno.md
.docs/004_adr/<prefijo>_Titulo.md
```

**Siguiente libre** (carpeta nueva):
1. Siguiente = mayor valor numérico + 1. El prefijo se lee como entero (`017` y `0017` valen 17), nunca como texto. Se cuentan la copia local **y** la rama remota ya descargada: `git ls-tree --name-only @{u} .docs/002_specs/` (u `origin/<rama principal>` si la rama no tiene upstream). Es solo lectura; el agente no hace `fetch` ni `pull`. El ancho no interviene en la comparación: solo se aplica al escribir el número nuevo.
2. Si no hay rama remota ni upstream, el cálculo usa solo la copia local; si hay duplicado, se pregunta cuál conservar.
3. Los huecos no se rellenan ni se reutiliza un número ya usado, sea cual sea el ancho.
4. Recomendar al usuario subir pronto la carpeta con su fila del índice, para reservar el número.

**Revisión de duplicados.** El agente la hace al iniciar sesión, al asignar un número y antes de preparar un commit que toque esta carpeta, `.docs/003_deuda/` o `.docs/004_adr/` (skill `git-commit`). Varias personas abren ítems en paralelo, así que dos ramas pueden tomar el mismo número. El contador de cada carpeta es independiente.
1. Hay duplicado si dos ítems comparten **valor numérico** en la copia local (`017_A` y `0017_B`), o si un ítem local sin publicar usa un valor que la rama remota ya tiene con otro nombre o con otro ancho.
2. Con el mismo nombre tras el prefijo (`017_A` remoto y `0017_A` local), es probable que sea el mismo ítem con el prefijo escrito en otro ancho: se propone renombrar el local al nombre publicado, con confirmación. Solo si el usuario indica que son ítems distintos se trata como duplicado.
3. Conserva el número la que ya está en la rama remota (se publicó primero). La otra pasa al siguiente libre, contando local y remota. Si ninguna está publicada, o no hay rama remota, se pregunta cuál conservar.
4. El agente avisa con los dos nombres y el número propuesto, y no renombra sin confirmación. Al confirmar: `git mv` de la carpeta, su fila en el índice y cada referencia al nombre viejo (búsqueda con `grep`), incluidas las de su propio avance, `sql/` y las deudas que la citen.

**Regla:** nunca se renumera por un cambio de ancho ni de versión del kit: solo se renombra para resolver un duplicado, con confirmación.

**Referencias cortas** (`017`):
- La cita al ítem que conservó el número sigue válida.
- La cita al ítem renombrado por un duplicado se actualiza como parte del renombrado.
- Mientras `017` identifique a dos ítems sin resolver, la cita es ambigua: usa el nombre completo de la carpeta.
