# 02 — Análisis de impacto: Pausa, origen del kit y contexto bajo demanda

## 1. Resumen del requerimiento
Una versión del kit (0.13.0) que atiende los issues #4, #6, #7, #8, #10 y #11 y las deudas 0009 a
0014, en ocho bloques (A a H de `01_spec.md`). El trabajo es solo Markdown, en las dos capas: la raíz
(entorno de mantenimiento) y `template/`.

### 1.1 Antecedentes
Carpetas del índice que coinciden (todas `Cerrado`):

| Carpeta | Qué se reutiliza | Qué contradice o cambia |
|---|---|---|
| `0015_ArranqueLigero` | Método de medición en bytes (CP-10), escenario base de 12 110 bytes, criterio del ADR 0032, CP-08 (`AGENTS.md` viejo con `.agents/` nuevo) | El paso 8 sigue leyendo carpetas; «Antecedentes» y «Deuda técnica» siguen en `AGENTS.md` |
| `0012_EstatusDeSpecYCierre` | Leyenda de estatus, E22, «Quién escribe» | La línea «Quién escribe» gana `Planificado`, `En curso` y `En pruebas` (RF-28) y la reapertura de `Descartado` (RF-29) |
| `0016_CierreEnUnaCorrida` | Cierre con commits en una corrida | Ninguna |
| `0010_FalsoPositivoInitPendiente` | E21 y la cadena del marcador | Añade un chequeo solo para el repo del kit (RF-30) |
| `0008_GuardrailsBdSoloLectura` | Regla 1 de Guardrails y su excepción de QA | Se condensa la redacción sin perder piezas (RF-12) |
| `0014_ConfiguracionGuiada` | Patrón de «sugerir, no aplicar»; auditor beta (lo que quedó de #9) | Ninguna |
| `001_ActualizarKit` | Grupos A a D de `ACTUALIZAR.md`, `.kit-version`, «Commit propuesto» | El commit único pasa a tres grupos (RF-9) |
| `003_OptimizarArnes` | Línea base en bytes | Ninguna |

### 1.2 Deuda técnica relacionada
Todas entran en este requerimiento: 0009 (RF-15), 0010 (RF-30), 0011 (RF-27), 0012 (RF-28), 0013
(RF-29) y 0014 (RF-16). No queda deuda abierta de estos módulos fuera de la spec. El cierre las pasa a
`Resuelta`, o las deja `Abierta` si alguna queda parcial.

## 2. Módulos, menú, tablas y archivos involucrados
### 2.1 Menú / opciones / rutas de acceso
Comandos `/sdd-*`. Hay dos nuevos: `/sdd-pausa` (por carpeta, como `/sdd-status`) y `/sdd-version`
(del kit, como `/sdd-estructura`). El coordinator enruta los dos.

### 2.2 Tablas (existentes y nuevas)
No aplica: no hay base de datos. Las «tablas» del kit que cambian son estas:
- tabla de fases de `skills/sdd/SKILL.md`;
- «Cómo decides qué hacer» de `coordinator.md`;
- índice de checks de `validar-estructura.md` (E10 y E13 cambian; entran E24, E25 y E26);
- PUNTO DE RETOMO de `avance-y-tiempos.md` y `plantillas.md`.

### 2.3 Archivos de código (ruta, capa)
Capa `template/` (se replica en `.agents/` de la raíz con `cp`):

| Archivo | Bloque | Cambio |
|---|---|---|
| `template/AGENTS.md` | C, D, B, E | Saca «Antecedentes» y «Deuda técnica» con una remisión cada una; condensa la regla 1; paso 8 sobre índices; excepción de red en «Git»; línea de contexto |
| `template/.agents/skills/sdd/antecedentes-y-deuda.md` (nuevo) | C | Fuente única del cuerpo de «Antecedentes» y «Deuda técnica» |
| `template/.agents/skills/sdd/topes-y-modos.md` | C | Definición de cambio pequeño dentro de «Modo ligero» |
| `template/.agents/agents/coordinator.md` | A, B, C, G | Filas de ruteo de `/sdd-pausa`, `/sdd-version` y `/sdd-constitucion`; la fila «fix chico» remite al modo ligero |
| `template/.agents/skills/sdd/SKILL.md` | A, B | Filas «Pausa» y «Versión del kit» en la tabla de fases |
| `template/.agents/commands/sdd-pausa.md` (nuevo) | A | RF-1 a RF-4 |
| `template/.agents/commands/sdd-status.md` | A | RF-5 |
| `template/.agents/skills/sdd/avance-y-tiempos.md` | A, G | Marca de pausa en el PUNTO DE RETOMO; hueco largo desde la marca |
| `template/.agents/skills/sdd/plantillas.md` | A | Fila «Pausa» en la plantilla de `05_avance.md` |
| `template/.agents/commands/sdd-version.md` (nuevo) | B | RF-7 |
| `template/.agents/commands/sdd-estructura.md` | D, H | Descripción y escritura de la bandera (RF-18, RF-19) |
| `template/.agents/validar-estructura.md` | B, D, E, G, H | E10 (exentos), E13 (remisión a la leyenda), E24 (marcador de la raíz), E25 (`.kit-origen`), E26 (`005_contexto`) |
| `template/.agents/sync-agents.md` | C | «Chequeo de arranque» con la lectura reducida de carpetas por archivo (RF-16) |
| `template/.agents/commands/sdd-init.md` | C, E, F | Guía de formato y Build (RF-14); BD de solo lectura (RF-15); contexto propuesto (RF-21); detección de stack (RF-24); allow (RF-26) |
| `template/.agents/skills/sdd/restricciones-de-rol.md` | C, F | `deny` con clientes de BD (RF-15); remisión del allow (RF-26) |
| `template/.agents/catalogo-skills.md` (nuevo) | F | Catálogo de señales (RF-24, RF-25) |
| `template/.agents/commands/sdd-cierre.md` | E, G | Propuesta de contexto (RF-22); pulido de la deuda 0013 (RF-29) |
| `template/.agents/commands/sdd-plan.md`, `sdd-implement.md` | G | Remiten a «Quién escribe» para escribir la celda (RF-28) |
| `template/.agents/README.md` | B, E, F | Árbol (catálogo, `005_contexto`, `.kit-origen`) y «Roles y comandos» |
| `template/.docs/002_specs/README.md` | D, E, G | «Quién escribe» (RF-28, RF-29); «Numeración única» incluye `005_contexto` |
| `template/.docs/005_contexto/README.md` (nuevo) | E | Índice vacío y guía (RF-20) |

Capa raíz (versionada):

| Archivo | Cambio |
|---|---|
| `AGENTS.md` | Lo mismo que `template/AGENTS.md`, más su paso 0 y sus secciones propias |
| `.docs/002_specs/README.md` | «Quién escribe» y «Numeración única», en espejo de la plantilla |
| `.docs/005_contexto/README.md` (nuevo) | Índice vacío (supuesto 7) |
| `.docs/004_adr/0033…`, `0034…`, `0035…` y su índice | ADR de RNF-6 |
| `INSTALAR.md` | Escribe `.agents/.kit-origen` (RF-6) |
| `ACTUALIZAR.md` | `.kit-origen` (RF-6), tres commits (RF-9), recomendación de skills al final (RF-24), Grupo B 0.13.0 (RF-31) |
| `README.md` | Lista de comandos: `/sdd-pausa` y `/sdd-version` |
| `VersionAgentKitSdd`, `ChangelogAgentKitSdd.md`, `manifiestos/0.13.0.md` | RF-31 |

### 2.4 Código legado o sin uso
- `.docs/002_specs/0012_EstatusDeSpecYCierre/01_spec.md` (cerrada): no se edita (RF-29).
- Los `AGENTS.md` de proyectos ya instalados (Grupo B): no se reemplazan. Conservan su «Antecedentes» y
  su «Deuda técnica», y siguen funcionando (CP-08 de la 0015 como patrón).
- Los `.docs/` de proyectos ya instalados (Grupo C): no reciben la nueva «Quién escribe» ni
  `005_contexto` salvo que falten. Se documenta en `ACTUALIZAR.md`.

## 3. Datos reales
No aplica: no hay base de datos. Mediciones del 2026-11-22 13:45 (`LC_ALL=C wc -c`):

| Medida | Raíz | `template/` |
|---|---|---|
| `AGENTS.md` | 12 267 | 11 061 |
| «Guardrails» (sección) | 3 048 | 3 048 |
| Regla 1 sola | 2 640 | 2 640 |
| «Antecedentes» | 893 | 893 |
| «Deuda técnica» | 691 | 697 |
| «Cada sesión» | 2 456 | 2 354 |

- `validar-estructura.md` tiene E1 a E23.
- El último ADR es el 0032 y la última deuda, la 0014.
- `template/.gitignore` ya ignora `.agents/local/`.

## 4. Flujo actual
- **Parar:** el agente improvisa el punto de retomo y no hay marca de pausa. `/sdd-status` solo lee.
- **Versión:** `.agents/.kit-version` sin origen; no hay procedimiento para comparar con la versión
  publicada.
- **Arranque:** el paso 8 lista carpetas. `/sdd-estructura` no deja constancia de su resultado.
- **Contexto:** `/sdd-init` vuelca todo en `AGENTS.md`. No hay sección de contexto.
- **Skills:** el paso 5 de `/sdd-init` lista candidatas a criterio propio. `ACTUALIZAR.md` no
  recomienda.
- **Celda del índice:** solo la escriben la apertura y el cierre.

## 5. Reglas de negocio: conocidas vs código
| Regla | Dónde vive | Efecto de esta spec |
|---|---|---|
| Una sola fuente por regla (ADR 0024, `MEMORY.md`) | Leyenda, deuda, tiempos, numeración | Las reglas nuevas (cambio pequeño, marca de pausa, «Quién escribe», catálogo) se definen una vez y lo demás remite |
| Grupo B nunca se reemplaza | `ACTUALIZAR.md` › Grupo B | Los cambios de `AGENTS.md` se incorporan a mano; RF-31 lo documenta |
| Grupo C se respeta | `ACTUALIZAR.md` › Grupo C | `005_contexto` se ofrece como NUEVO; la nueva «Quién escribe» no llega a los `.docs/` existentes |
| Solo Markdown (ADR 004), sin hooks (ADR 011) | ADR | Los comandos nuevos son Markdown; los permisos solo se sugieren |
| Regla Git (NUNCA `fetch`, etc.) | `AGENTS.md` › Git | Gana una única excepción acotada (RF-8, ADR 0034) |
| Principio 2 de la constitución del kit | `constitution.md` | El cambio pequeño va por el modo ligero (RF-13), sin conflicto |

## 6. Riesgos y casos borde
1. **Romper referencias por nombre** al sacar «Antecedentes» y «Deuda técnica». Mitigación: el
   encabezado se queda en `AGENTS.md` con su remisión; E11 y CP de referencias.
2. **Perder una pieza de la regla 1** al condensar. Mitigación: lista de comprobación de RF-12, pieza
   por pieza, en `07_casos_prueba.md`; revisión independiente.
3. **Huella vieja en el manifiesto** si `template/` cambia tras generarlo (trampa de `MEMORY.md`).
   Mitigación: el manifiesto se genera al final y se recalcula tras cada corrección.
4. **Spec grande**: muchos archivos compartidos entre bloques (`sdd-init`, `validar-estructura`,
   `coordinator`, `AGENTS.md`). Mitigación: un solo bloque por archivo y fase, en orden, sin
   implementers en paralelo sobre el mismo archivo (lección de la 0015).
5. **Red en `/sdd-version`**: el clon temporal podría quedarse si se corta. Mitigación: borrado del
   temporal y ninguna ruta dentro del proyecto.
6. **RNF-1 justo**: la cuenta estimada deja unos 270 bytes de margen en `template/AGENTS.md`.
   Mitigación: se mide al terminar F1 y antes de seguir.
7. **Proyecto con `AGENTS.md` viejo** y `.agents/` nuevo: el paso 6 viejo apunta a su propia
   «Antecedentes» y no se rompe. El chequeo de arranque nuevo funciona con el paso 7 viejo, como en
   CP-08 de la 0015.

## 7. Preguntas abiertas
Ninguna. Las decisiones de diseño con alternativas están en `03_plan.md` › 6.
