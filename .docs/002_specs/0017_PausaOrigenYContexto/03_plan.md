# 03 — Plan: Pausa, origen del kit y contexto bajo demanda
Spec: 01_spec.md (aprobada 2026-11-22 13:54)

## 1. Ubicación y capas
- Solo Markdown (constitución, principios 3 y 6; ADR 004).
- Lo distribuible se edita en `template/` y se replica en `.agents/` de la raíz con `cp`. Se comprueba
  con `diff -rq template/.agents .agents` y solo se admiten las diferencias ya aceptadas en `MEMORY.md`.
- La raíz versiona `AGENTS.md`, `.docs/` y los docs distribuibles (`README.md`, `INSTALAR.md`,
  `ACTUALIZAR.md`, `VersionAgentKitSdd`, `ChangelogAgentKitSdd.md`).
- Cada regla nueva tiene una sola fuente; lo demás remite:

| Regla nueva | Fuente única |
|---|---|
| Cambio pequeño | `skills/sdd/topes-y-modos.md` › «Modo ligero» |
| Marca de pausa y lista «al retomar» | `skills/sdd/avance-y-tiempos.md` › PUNTO DE RETOMO |
| Quién escribe la celda | `.docs/002_specs/README.md` › «Leyenda de estatus» › «Quién escribe» |
| Antecedentes y deuda (procedimiento) | `skills/sdd/antecedentes-y-deuda.md` (nuevo) |
| Catálogo de señales | `.agents/catalogo-skills.md` (nuevo) |
| Bandera de estructura | `.agents/validar-estructura.md`, fuente de los chequeos; `sdd-estructura.md` remite |
| Consulta de versión y origen | `commands/sdd-version.md` (nuevo); `INSTALAR.md` y `ACTUALIZAR.md` escriben el origen y remiten para consultar |
| Excepción de red | `AGENTS.md` › Git (una línea) y ADR 0034 |

## 2. Archivos que se crean o modifican
| Archivo | Capa | Responsabilidad | RF |
|---|---|---|---|
| `template/AGENTS.md` y `AGENTS.md` | distribuible / raíz | Remisiones de «Antecedentes» y «Deuda técnica»; regla 1 condensada; paso 8 sobre índices; línea de excepción en «Git»; línea de contexto | 8, 11, 12, 17, 21 |
| `template/.agents/skills/sdd/antecedentes-y-deuda.md` (nuevo) | skill | Cuerpo de «Antecedentes» (6 puntos) y de «Deuda técnica» (4 puntos), movido sin cambiar su sentido | 11 |
| `template/.agents/skills/sdd/topes-y-modos.md` | skill | Definición de cambio pequeño en «Modo ligero» | 13 |
| `template/.agents/agents/coordinator.md` | agente | Ruteo de `/sdd-pausa`, `/sdd-version` y `/sdd-constitucion`; las líneas 16 y 42 («fix chico») remiten al modo ligero | 1, 7, 13, 27 |
| `template/.agents/skills/sdd/SKILL.md` | skill | Filas «Pausa» (`/sdd-pausa`) y «Versión del kit» (`/sdd-version`) en la tabla de fases | 1, 7 |
| `template/.agents/commands/sdd-pausa.md` (nuevo) | comando | Procedimiento de pausa | 1–4 |
| `template/.agents/commands/sdd-status.md` | comando | Lista «al retomar» con comprobaciones cerradas de solo lectura | 5 |
| `template/.agents/skills/sdd/avance-y-tiempos.md` | skill | Fila «Pausa» del PUNTO DE RETOMO (hora y lista «al retomar»); el hueco largo se mide desde la marca | 3, 5 |
| `template/.agents/skills/sdd/plantillas.md` | skill | Fila «Pausa» en la plantilla `05_avance.md`, que remite a avance-y-tiempos | 3 |
| `template/.agents/commands/sdd-version.md` (nuevo) | comando | Procedimiento «¿tengo la última versión?» | 7, 8 |
| `INSTALAR.md` | raíz distribuible | Paso que escribe `.agents/.kit-origen` junto a `.kit-version` | 6 |
| `ACTUALIZAR.md` | raíz distribuible | Comparar o crear `.kit-origen`; tres grupos de commit; recomendación de skills al final; nota de Grupo B y Grupo C para 0.13.0 | 6, 9, 24, 31 |
| `template/.agents/commands/sdd-estructura.md` | comando | Descripción («lee, reporta y deja la bandera local») y paso final que escribe la bandera | 18, 19 |
| `template/.agents/validar-estructura.md` | doc de apoyo | E10 con exentos; E13 remite a la leyenda; E24, E25 y E26 nuevos; sección «Bandera de estructura» | 10, 18, 19, 23, 27, 29, 30 |
| `template/.agents/sync-agents.md` | doc de apoyo | «Chequeo de arranque»: en una carpeta por archivo solo comprueba la entrada; «Qué lee» coherente | 16 |
| `template/.agents/commands/sdd-init.md` | comando | Guía de formato y Build; pregunta de BD de solo lectura; propuesta de contexto; detección de stack en el paso 5; allow con los comandos de verificación | 14, 15, 21, 24, 26 |
| `template/.agents/skills/sdd/restricciones-de-rol.md` | skill | `deny` con 7 clientes de BD; el allow remite a RF-26 | 15, 26 |
| `template/.agents/catalogo-skills.md` (nuevo) | doc de apoyo | Tabla de señales, procedimiento de cruce y revisión de skills externas | 24, 25 |
| `template/.agents/commands/sdd-cierre.md` | comando | Propuesta de contexto; tramos duplicados → remisiones; frase del punto 3 | 22, 29 |
| `template/.agents/commands/sdd-plan.md`, `sdd-implement.md` | comando | Una línea que remite a «Quién escribe» | 28 |
| `template/.agents/README.md` | doc de apoyo | Árbol (catálogo, `005_contexto`, `.kit-origen`); «Roles y comandos» con los dos comandos nuevos | 1, 7, 20 |
| `template/.docs/002_specs/README.md` y `.docs/002_specs/README.md` | docs | «Quién escribe» con las tres palabras intermedias y la reapertura; «Numeración única» incluye `005_contexto` | 17, 20, 28, 29 |
| `template/.docs/005_contexto/README.md` y `.docs/005_contexto/README.md` (nuevos) | docs | Índice vacío y guía | 20 |
| `.docs/004_adr/0033…`, `0034…`, `0035…` y `README.md` | raíz | ADR de RNF-6 | — |
| `README.md` | raíz distribuible | Lista de comandos | 1, 7 |
| `VersionAgentKitSdd`, `ChangelogAgentKitSdd.md`, `manifiestos/0.13.0.md` | publicación | 0.13.0 | 31 |
| `.agents/**` de la raíz | réplica local | `cp` de cada archivo tocado en `template/.agents/` | RNF-8 |

## 3. Base de datos
No aplica: no hay `sql/`.

## 4. Seguridad
- Sin autenticación ni datos. Riesgos y controles:
  - **Red (RF-7, RF-8).** Solo `git ls-remote` y `git clone --depth 1` en un `mktemp -d` fuera del
    proyecto, borrado al terminar. Sin interacción (`GIT_TERMINAL_PROMPT=0`) y sin credenciales: si el
    remoto las pide, se para. No se escribe nada en el proyecto ni en el clon local.
  - **Ejecución desde un archivo (RF-5).** `/sdd-status` solo ejecuta las tres comprobaciones de la
    lista cerrada. Cualquier otra línea de «al retomar» se muestra y no se ejecuta.
  - **Bandera (RF-18).** Antes de escribirla, `git check-ignore`. Nunca `git add`.
  - **Permisos (RF-15, RF-26).** Los fragmentos `deny` y `allow` siguen siendo texto que se muestra.
    El `allow` solo admite los comandos de verificación de solo lectura del proyecto.
  - **Skills externas (RF-25).** Se revisan antes de usarlas: sin scripts ejecutables ni secretos,
    con la skill `seguridad-basica`.
  - **Regla 1 (RF-12).** Comprobación pieza por pieza antes y después; revisión independiente en
    validate.
- Al validar: `seguridad-basica` sobre lo que se versiona.

## 5. Liberación / piloto
- Versión 0.13.0, con manifiesto. Los proyectos existentes reciben lo del Grupo A al actualizar.
- `AGENTS.md` y `.gitignore` (Grupo B) se incorporan a mano, guiados por `ACTUALIZAR.md`.
- Lo nuevo de `.docs/` (Grupo C) se ofrece si falta: `005_contexto/README.md`. Lo que cambia en un
  README existente (la «Quién escribe» nueva) se documenta para incorporarlo a mano.
- No hay feature flag. `/sdd-pausa` y `/sdd-version` son bajo demanda.

## 6. Decisiones
| Decisión | Alternativa descartada | Por qué |
|---|---|---|
| D1. El cuerpo de «Antecedentes» y «Deuda técnica» va a `skills/sdd/antecedentes-y-deuda.md`. `AGENTS.md` conserva los dos encabezados con condición y remisión (supuesto 5) | (a) Los README de `002_specs` y `003_deuda`. (b) Una sección de `SKILL.md` | (a) son Grupo C: un proyecto que actualiza no los recibe y la remisión quedaría rota. (b) carga la skill entera para leer 1,5 KB. Un archivo de la skill es Grupo A y se lee solo cuando hace falta |
| D2. El cambio pequeño se define en «Modo ligero» de `topes-y-modos.md` | Definirlo en `AGENTS.md` | `topes-y-modos.md` ya es la fuente del modo ligero y no carga cada turno |
| D3. Marca de pausa: fila opcional «Pausa» del PUNTO DE RETOMO, con hora, motivo y lista «al retomar»; se borra al retomar (supuesto 3) | Una fila «pausa» en la tabla de tiempos | La tabla de tiempos ya tiene la columna Pausa; una fila suelta rompería la regla de una fila por fase |
| D4. `/sdd-version` es un comando nuevo; `ACTUALIZAR.md` remite a él (supuesto 4) | Una sección de `ACTUALIZAR.md` | El comando es Grupo A, se enruta y se puede pedir en cualquier momento |
| D5. El catálogo vive en `template/.agents/catalogo-skills.md` (supuesto 6) | En `skills/sdd/` o en la raíz sin distribuir | Es genérico y de apoyo, como `configuracion.ejemplo.md`; no es parte del flujo `sdd` |
| D6. Chequeos nuevos E24 (marcador de la raíz del kit), E25 (`.kit-origen`) y E26 (`005_contexto`) | Ampliar E21 y E13 | Cada chequeo queda con un solo propósito, y E21 sigue siendo solo de proyectos |
| D7. `/sdd-pausa` y `/sdd-version` entran a la tabla de fases (como Estado y Estructura). `/sdd-constitucion`, `/sdd-init` y `/sdd-config` son exentos explícitos de E10, con ruteo en el coordinator | Todos en la tabla de fases | Decisión de la persona para la constitución (13 del avance). Los otros dos ya son auxiliares |
| D8. La celda la escribe la sesión al registrar el cierre de `plan`, del primer paso y del último paso; la regla vive en «Quién escribe» | Cada comando | Decisión de la persona (12 del avance) |
| D9. La raíz del kit también lleva `.docs/005_contexto/README.md` (supuesto 7) | Solo en `template/` | E26 se comporta igual en las dos capas |
| D10. Tres ADR: 0033 (salida de «Antecedentes» y «Deuda técnica», D1), 0034 (excepción de red, RF-8) y 0035 (sección `005_contexto`) | Un solo ADR para todo | Son decisiones independientes y con alternativas propias. Se crean con visto bueno de la persona (`sdd-plan`, paso 3) |

## 7. Fases y estimado
Las fases van en orden. Ningún archivo se edita en dos fases a la vez, y no hay implementers en
paralelo sobre el mismo archivo.

| Fase | Contenido | RF | Estimado (h) |
|---|---|---|---|
| F0 | Línea base: medidas de `AGENTS.md`, copias para el protocolo P, `/sdd-estructura` en el clon y en un ensayo de instalación, lista de piezas de la regla 1 | — | 0.25 |
| F1 | `AGENTS.md` en las dos capas y el arranque: `antecedentes-y-deuda.md`, regla 1, paso 8, línea de «Git», línea de contexto; `topes-y-modos.md` (cambio pequeño); `sync-agents.md` (0014). Medición de RNF-1 | 8, 11, 12, 13, 16, 17, 21 (línea) | 1.5 |
| F2 | Pausa y retomo: `sdd-pausa.md`, `sdd-status.md`, `avance-y-tiempos.md`, `plantillas.md` | 1–5 | 1 |
| F3 | Origen y versión: `sdd-version.md`, `INSTALAR.md`, `ACTUALIZAR.md` (`.kit-origen` y tres commits) | 6, 7, 9 | 1 |
| F4 | Contexto: los dos `005_contexto/README.md`, `sdd-init.md` (propuesta), `sdd-cierre.md` (RF-22), «Numeración única» | 20, 21, 22 | 0.75 |
| F5 | Stack y permisos: `catalogo-skills.md`, `sdd-init.md` (RF-14, RF-15, RF-24, RF-26), `restricciones-de-rol.md`, `ACTUALIZAR.md` (recomendación al final) | 14, 15, 24, 25, 26 | 1 |
| F6 | Estatus y pulido: «Quién escribe» en los dos README, `sdd-plan.md`, `sdd-implement.md`, `sdd-cierre.md` (RF-29) | 28, 29 | 0.75 |
| F7 | Estructura y ruteo: `validar-estructura.md` (E10, E13, E24, E25, E26 y bandera), `sdd-estructura.md`, `coordinator.md`, `SKILL.md`, `.agents/README.md`, `README.md` | 10, 18, 19, 23, 27, 30 (y el ruteo de 1, 7 y 13) | 1 |
| F8 | ADR 0033 a 0035 con su índice; réplica en `.agents/`; 0.13.0 (versión, changelog, manifiesto, Grupo B y C en `ACTUALIZAR.md`) | 31 | 1 |
| F9 | Casos de prueba: chequeos de Comandos, ensayo de instalación, `/sdd-estructura`, mediciones y escenarios del arranque | todos | 1 |
| | **Total** | | **9.25** |

## 8. Criterios de aceptación
1. Cada RF tiene al menos un caso en `07_casos_prueba.md` con estado `ok` o la confirmación manual de
   la persona.
2. RNF-1: `template/AGENTS.md` ≤ 9 800 bytes, la raíz ≤ 11 000, y la lectura del arranque del
   escenario base ≤ 12 110.
3. RF-12: las piezas de la regla 1 de F0 se señalan, una a una, en el texto nuevo, y el texto es
   idéntico en las dos capas.
4. E11 sin rutas nuevas rotas. Las referencias a «Antecedentes» y «Deuda técnica» resuelven en las dos
   capas.
5. `/sdd-estructura` en el clon y en el ensayo de instalación sin `FALLA` nueva frente a F0. E24, E25
   y E26 responden como dicen sus filas.
6. `diff -rq template/.agents .agents` solo con las diferencias aceptadas (RNF-8). El ensayo de
   instalación copia todo sin pisar.
7. Cada archivo tocado pasa los chequeos de `AGENTS.md` › Comandos. `manifiestos/0.13.0.md` coincide
   con `template/` en cada huella.
8. ADR 0033 a 0035 creados con visto bueno y con su fila de índice.
