# Plantillas SDD

Copiar la sección que toque. Todo en UTF-8. Las secciones que no apliquen se
dejan con "No aplica", no se borran.

## 01_spec.md

```markdown
# 01 — Spec: <Nombre del feature>
Estado: borrador | aprobada | analisis | implementada
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
- [NECESITA ACLARACIÓN] <duda>
```

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

## 05_avance.md

Estructura y reglas en [avance-y-tiempos.md](avance-y-tiempos.md).

```markdown
# 05 — Avance: <Nombre>
Leyenda: `[ ]` Pendiente · `[~]` En curso · `[x]` Hecho · `[!]` Bloqueado

## PUNTO DE RETOMO — leer esto primero
| Dato | Contenido |
|---|---|
| Última sesión | |
| Fase actual | |
| Último paso cerrado | |
| Siguiente paso | |
| Bloqueos abiertos | Ninguno |
| Estado BD | |
| Código | |

## Pasos
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
