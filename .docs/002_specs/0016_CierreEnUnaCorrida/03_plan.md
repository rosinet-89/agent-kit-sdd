# 03 — Plan: Cierre en una corrida con sus commits
Spec: 01_spec.md (aprobada 2026-11-21 15:55)

## 1. Ubicación y capas
Kit distribuible (`template/`) y su réplica en la raíz; la leyenda se copia en el índice del clon
(ADR 0027).

## 2. Archivos que se crean o modifican
| Archivo | Capa | Responsabilidad | RF |
|---|---|---|---|
| `template/.docs/002_specs/README.md` | índice | Criterio 2 y frase de los tres resultados | RF-1, RF-7 |
| `.docs/002_specs/README.md` | índice del clon | Misma leyenda | RF-7 |
| `template/.agents/commands/sdd-cierre.md` | comando | Orden del cierre; puntos 7, 8 y 16 (propuesta, confirmación, bajada, fallo) | RF-1 a RF-6 |
| `.agents/commands/sdd-cierre.md` | réplica | Copia | — |
| `VersionAgentKitSdd`, `ChangelogAgentKitSdd.md`, `manifiestos/0.11.0.md` | publicación | 0.11.0 | Criterio 3 |
| `.docs/004_adr/0031_CierreConSusCommitsEnUnaCorrida.md` y su fila | ADR | Decisión | — |

## 3. Base de datos
No aplica.

## 4. Seguridad
Sin cambios en Guardrails ni en Git: confirmación explícita antes de cada `git commit`, sin push,
`git add` explícito. Secretos: la búsqueda de `seguridad-basica` sigue en la fase 2 de `git-commit`.

## 5. Liberación / piloto
Versión 0.11.0. Piloto: el cierre de esta misma spec usa la regla nueva.

## 6. Decisiones
| Decisión | Alternativa descartada | Por qué |
|---|---|---|
| Escribir `Cerrado` y decidir el criterio 2 con los commits en la misma corrida ([ADR 0031](../../004_adr/0031_CierreConSusCommitsEnUnaCorrida.md)) | Commits primero y segunda corrida (regla actual) | Doble ciclo y un commit solo para el estatus |
| Bajar a `Implementado` si no hay confirmación | Dejar `Cerrado` con aviso | `Cerrado` sin commits es justo lo que el criterio 2 evita |
| Sin flujo (`06_flujo.md` no aplica) | Generar el flujo | Cambia el kit; no hay operación de usuario final (criterio de la persona en la 0014, decisión 25) |

## 7. Fases y estimado
Estimado por lote (lección de la 0013 y la 0014).

| Fase | Contenido | RF | Estimado (min) |
|---|---|---|---|
| F1 | Leyenda (dos copias) y `sdd-cierre.md` con réplica | RF-1 a RF-7 | 5 |
| F2 | Versión 0.11.0, changelog, manifiesto, lint y ensayo de instalación | Criterio 3 | 5 |

## 8. Criterios de aceptación
Los de la spec. Las dos leyendas iguales (`diff` vacío), réplica igual a `template/`, manifiesto que
coincide con `template/`, lint por archivo.
