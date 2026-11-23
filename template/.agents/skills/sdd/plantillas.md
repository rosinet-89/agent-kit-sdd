# Plantillas SDD

Copiar la sección que toque. Todo en UTF-8. Las secciones que no apliquen se
dejan con "No aplica", no se borran.

## 01_spec.md

```markdown
# 01 — Spec: <Nombre del feature>
Estado: borrador | aprobada | analisis | implementada | cerrada | descartada
Ticket: <COM-NNN o "sin ticket">

## Contexto y objetivo
## Usuarios y roles
## Historias de usuario
- HU-1. Como <rol>, quiero <acción> para <beneficio>.
## Definiciones (solo si hay términos ambiguos)
## Requisitos funcionales (EARS)
- RF-1: CUANDO <evento>, EL SISTEMA <respuesta>.
## Requisitos no funcionales
## Casos límite
## Fuera de alcance
## Criterios de finalización
## Dudas abiertas
- [ACLARAR: <duda>]
```

Spec corta (modo ligero): omite Usuarios y roles, Historias, Definiciones, Requisitos no funcionales y Fuera de alcance, con el límite de 5 000 bytes y 8 RF. Reglas en [topes-y-modos.md](topes-y-modos.md).

## 02_analisis_impacto.md

```markdown
# 02 — Análisis de impacto: <Nombre>

## 1. Resumen del requerimiento
### 1.1 Antecedentes (índice de planes: carpetas que coinciden, qué se reutiliza y qué contradice; o "sin antecedentes en el índice" + grep que lo confirma)
### 1.2 Deuda técnica relacionada (`.docs/003_deuda/README.md`: deudas abiertas del módulo y si este feature las resuelve o las deja; o "ninguna")
## 2. Módulos, menú, tablas y archivos involucrados
### 2.1 Menú / opciones / rutas de acceso
### 2.2 Tablas (existentes y nuevas)
### 2.3 Archivos de código (ruta, capa)
### 2.4 Código legado o sin uso (no tocar, sí documentar)
## 3. Datos reales (SELECT de solo lectura, con fecha y BD)
## 4. Flujo actual
## 5. Reglas de negocio: conocidas vs código
## 6. Riesgos y casos borde
## 7. Preguntas abiertas
```

## 03_plan.md

```markdown
# 03 — Plan: <Nombre>
Spec: 01_spec.md (aprobada AAAA-MM-DD HH:MM)

## 1. Ubicación y capas
Módulo, ruta y patrón de la arquitectura del proyecto (ver `AGENTS.md` y la constitución).
## 2. Archivos que se crean o modifican
| Archivo | Capa | Responsabilidad | RF |
## 3. Base de datos
| Script `sql/` | Tipo (DDL/seed) | Rollback |
## 4. Seguridad
Autenticación y permisos, protección contra doble envío e idempotencia, validación de entradas, según `AGENTS.md`.
## 5. Liberación / piloto
Estrategia de liberación (feature flag, usuarios piloto) o "No aplica".
## 6. Decisiones
| Decisión | Alternativa descartada | Por qué |
## 7. Fases y estimado
| Fase | Contenido | RF | Estimado (h) |
| F0 | Diagnóstico / preparación | — | |
## 8. Criterios de aceptación
```

## 04_tasks.md

```markdown
# 04 — Tareas: <Nombre>
Leyenda: `[ ]` Pendiente · `[~]` En curso · `[x]` Hecho · `[!]` Bloqueado

## F1 — <nombre de la fase> (estimado: 2–3 h)
- [ ] **F1.1 <descripción>.** RF-1, RF-2
  - Hecho cuando:
    - [ ] Comandos de lint / sintaxis / prueba de `AGENTS.md` › Comandos OK en `<archivo>`
    - [ ] Convenciones de archivo de `AGENTS.md` (encoding, formato) respetadas
    - [ ] Evidencia de datos: <consulta de solo lectura y resultado esperado>
    - [ ] Verificación manual UI: <qué revisar> (o "No aplica")
```

En modo ligero, `04_tasks.md` lleva antes de las fases una sección «Plan» (archivos que se tocan, decisiones y estimado) y no pasa de 8 000 bytes. Si el cambio se distribuye, incluye la tarea de bump y changelog.

## 05_avance.md

Estructura y reglas en [avance-y-tiempos.md](avance-y-tiempos.md).

```markdown
# 05 — Avance: <Nombre>
Leyenda: `[ ]` Pendiente · `[~]` En curso · `[x]` Hecho · `[!]` Bloqueado

## PUNTO DE RETOMO — leer esto primero
| Dato | Contenido |
|---|---|
| Modo | completo \| ligero (con la lista de condiciones evaluada; ver [topes-y-modos.md](topes-y-modos.md)) |
| Última sesión | |
| Fase actual | |
| Último paso cerrado | |
| Siguiente paso | |
| Bloqueos abiertos | Ninguno |
| Estado BD | |
| Código | |
| Tiempo | (aparece al aprobar las tareas; ver [avance-y-tiempos.md](avance-y-tiempos.md) › «Pasos con tiempos») |
| Pausa | (opcional; solo `/sdd-pausa`. Ver [avance-y-tiempos.md](avance-y-tiempos.md) › «Marca de pausa») |

## Pasos
Forma y reglas en [avance-y-tiempos.md](avance-y-tiempos.md) › «Pasos con tiempos» (filas de fase `**Fn — <nombre>**`, filas de paso, `**Total implementación**` y `**Documentación SDD**`).
| Paso | Estado | Estimado | Real | Delta | Nota |
|---|---|---|---|---|---|
## Decisiones
## Pruebas
## Tiempos
| Paso | Inicio | Fin | Minutos | Pausa | Nota |
|---|---|---|---|---|---|
## Plan contra real
```

## 06_flujo.md

Para el usuario final y soporte.

```markdown
# 06 — Flujo operativo: <Nombre>

## 1. ¿Qué es y para qué sirve?
## 2. ¿Quién lo usa? (roles y permisos)
## 3. ¿Cuándo aplica?
## 4. Flujo paso a paso (acceso → pantalla → acción → resultado)
## 5. Estatus y transiciones
## 6. Notificaciones (quién recibe qué)
## 7. Piloto / liberación (si aplica)
## 8. Errores frecuentes y qué hacer
## 9. Fuera de alcance
## 10. Referencias (01_spec, 03_plan, pantallas)
```

## 07_casos_prueba.md

Solo si el feature tiene pasos verificables por ejecución. Sin ellos no se crea.
Lo genera el planner en `tasks`, antes de implementar.

```markdown
# 07 — Casos de prueba: <Nombre>

## Casos
### CP-01 <qué se comprueba>
- RF: RF-1
- Datos de partida: <estado inicial; credenciales solo por nombre de variable, por ejemplo `QA_USUARIO`>
- Destino: `https://qa.ejemplo.test` (`esquema://host[:puerto]`, `local` o `ninguno`)
- Pasos:
  1. Abrir `https://qa.ejemplo.test/login`.
  2. Llenar el campo con id `usuario` con el valor de `QA_USUARIO`.
  3. Pulsar el botón con id `entrar`.
- Resultado esperado: el texto del encabezado es exactamente `Inicio`.
- Herramienta: <la que declara AGENTS.md › Comandos> | a mano
- Escribe: sí | no

## Corridas
| Caso | Fecha | Estado | Obtenido / nota |
|---|---|---|---|
| CP-01 | AAAA-MM-DD | obtenido | |

## Evidencia formal (opcional)
Solo si se pidió evidencia formal. Si no, "No aplica".
```

Reglas:
- Estados de una corrida (lista cerrada): `obtenido` (hay resultado, sin comparar), `bloqueado` (no se pudo correr), `manual` (lo confirma la persona), `ok` y `falla` (comparación literal hecha).
- Un caso que usa una aplicación con base de datos, incluso solo para iniciar sesión o navegar, escribe (`sí`). La duda también cuenta como `sí`. Qué se ejecuta y qué se entrega: `AGENTS.md` › Guardrails, regla 1.
- De las credenciales solo se escribe el nombre de la variable de entorno; nunca su valor, ni aquí, ni en el encargo, ni en la configuración de entornos.
- Una comparación es literal cuando el resultado esperado es un texto o valor exacto y basta compararlo carácter por carácter con lo obtenido. Si requiere juicio, la deja al reviewer.
- Con herramienta de apoyo declarada, los pasos usan sus acciones (abrir URL, llenar el campo con id, pulsar, comparar texto). Sin ella, el caso es a mano.
- Resultados y confirmaciones manuales de la persona se anotan solo aquí; el avance lleva un enlace.
- Compatibilidad: un `07_pruebas_qa.md` de un feature anterior se conserva sin renombrar, y validate lo lee como evidencia.
