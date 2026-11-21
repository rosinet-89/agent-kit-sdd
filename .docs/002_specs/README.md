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

| NNN | Carpeta | Qué resuelve | Estatus (al último avance) | Palabras clave | Pendiente / deuda |
|---|---|---|---|---|---|
| 001 | `001_ActualizarKit` | Actualizar un proyecto con el kit instalado a una versión nueva, sin pisar lo personalizado | Cerrado (2026-11-21): cumple los cinco criterios; validación registrada en el retomo | actualizar, .kit-version, ACTUALIZAR.md, INSTALAR.md, migración 0.4.0 |  |
| 002 | `002_Kit050` | Registro retroactivo de lo ya publicado en 0.5.0, sin comportamiento nuevo | Cerrado (2026-11-21): cumple los cinco criterios; validación registrada en el retomo (retroactivo) | 0.5.0, adr, migracion-bd, seguridad-basica, sdd-estructura, modo mantenimiento |  |
| 003 | `003_OptimizarArnes` | Reducir tokens y pasos innecesarios de agentes y comandos sin bajar la calidad | Cerrado (2026-11-21): cumple los cinco criterios; validación registrada en el retomo | tokens, agentes, comandos, lecturas, permisos, status, implementer |  |
| 004 | `004_ModeloYPermisosAgentes` | Modelo por rol y permisos reales del frontmatter de los 4 agentes (atiende la deuda 003) | Cerrado (2026-11-21): cumple los cinco criterios; validación registrada en el retomo | model, permissionMode, tools, omitClaudeMd, mode, permissions, frontmatter, agentes |  |
| 005 | `005_ConvivenciaNumeracion` | Regla de convivencia de carpetas de 3 y 4 dígitos en un proyecto que se actualiza (atiende la deuda 004) | Cerrado (2026-11-21): cumple los cinco criterios; validación registrada en el retomo | numeración, 3 dígitos, 4 dígitos, E12, siguiente libre, actualizar |  |
| 006 | `006_RutaSiNoDelega` | Si un agente no puede delegar, el flujo sigue por otra ruta y deja constancia, sin quedarse atascado | Cerrado (2026-11-21): cumple los cinco criterios; validación registrada en el retomo | delegar, subagente, coordinator, planner, ruta alterna, atasco, Cursor |  |
| 007 | `007_DeudasAbiertas` | Las cuatro deudas abiertas: frontmatter en otras herramientas, sugerencia de allow y deny, nombres que chocan y límites de la numeración | Cerrado (2026-11-21): cumple los cinco criterios; validación registrada en el retomo | frontmatter, sdd-init, allow, deny, nombres, VERSION, CHANGELOG, E12, numeración |  |
| 0009 | `0009_EnlacesTrasPull` | Detectar cambios del kit tras un pull y ofrecer crear o corregir los enlaces de cada herramienta (issue #2) | Cerrado (2026-11-21): cumple los cinco criterios; validación registrada en el retomo; issue #2 cerrado | enlaces, pull, .kit-version, kit-visto, sdd-estructura, sync-agents, junction, Windows |  |
| 0008 | `0008_GuardrailsBdSoloLectura` | Sección global «Guardrails» en AGENTS.md: ningún agente escribe en la base de datos, dentro o fuera del flujo (issue #1) | Cerrado (2026-11-21): cumple los cinco criterios; validación registrada en el retomo; issue #1 cerrado | guardrails, base de datos, solo lectura, AGENTS.md, omitClaudeMd, migracion-bd, restricciones-de-rol |  |
| 0010 | `0010_FalsoPositivoInitPendiente` | La regla de «proyecto sin personalizar» no deja el marcador literal en `AGENTS.md` tras `/sdd-init`; chequeo nuevo E21 (issue #3) | Cerrado (2026-11-21): cumple los cinco criterios; validación registrada en el retomo; issue #3 cerrado | SDD-INIT-PENDIENTE, sdd-init, falso positivo, marcador, AGENTS.md, validar-estructura, E21 |  |
| 0011 | `0011_ReducirCeremoniaSdd` | Reducir tokens del flujo SDD en el proceso: tope de 3 pasadas de clarify y de marcadores `[ACLARAR]`, modo ligero para cambios chicos y revisión cruzada como modo del reviewer (0.7.0) | Cerrado (2026-11-21): cumple los cinco criterios; validación registrada en el retomo | tokens, clarify, modo ligero, ceremonia, revisión cruzada, topes-y-modos, spec-kit, 0.7.0 |  |
| 0012 | `0012_EstatusDeSpecYCierre` | Definir los estatus de una spec (incluido un terminal: `Cerrado` y `Descartado`), qué cuenta como pendiente y cuándo una spec o una deuda está cerrada (issue #5) | Cerrado (2026-11-16): cumple los cinco criterios; issue #5 cerrado; kit 0.8.0 en 5 commits locales | estatus, cerrado, descartado, pendiente, sdd-cierre, sdd-status, deuda, índice | [deuda 0012](../003_deuda/0012_CadaComandoDeFaseActualizaLaCelda.md), [deuda 0013](../003_deuda/0013_PulidoDeLaSpec0012.md) |

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

Cada carpeta lleva un prefijo `NNN_` que no se repite. En este clon, la numeración remite a la declaración de `template/.docs/002_specs/README.md` › « Numeración única ».
El ancho en un proyecto y la convivencia de anchos están en
`template/.docs/002_specs/README.md` › « Numeración única ». Las carpetas de sección de `.docs/` siguen en 3.
Varias personas abren carpetas en paralelo, así que dos ramas pueden tomar el mismo número; el agente lo detecta y propone corregirlo. La misma regla aplica a `.docs/003_deuda/` y a `.docs/004_adr/`.

**Al asignar un número** (carpeta nueva):
1. Siguiente = prefijo mayor + 1, mirando la copia local **y** la rama remota ya descargada: `git ls-tree --name-only @{u} .docs/002_specs/` (u `origin/<rama principal>` si la rama no tiene upstream). Es solo lectura; el agente no hace `fetch` ni `pull`.
2. Recomendar al usuario subir pronto la carpeta con su fila del índice, para reservar el número.

**Revisión de duplicados.** El agente la hace al iniciar sesión, al asignar un número y antes de preparar un commit que toque esta carpeta o `.docs/003_deuda/` (skill `git-commit`):
1. Hay duplicado si dos carpetas comparten prefijo en la copia local, o si una carpeta local sin publicar usa un prefijo que la rama remota ya tiene con otro nombre.
2. Conserva el número la que ya está en la rama remota (se publicó primero). La otra pasa al siguiente libre: prefijo mayor + 1, contando local y remota. Si ninguna está publicada, o no hay rama remota, se pregunta cuál conservar.
3. El agente avisa con los dos nombres y el número propuesto, y no renombra sin confirmación. Al confirmar: `git mv` de la carpeta, su fila en el índice y cada referencia al nombre viejo (búsqueda con `grep`), incluidas las de su propio avance, `sql/` y las deudas que la citen.
4. Un número no se reutiliza: los huecos se quedan como están.
