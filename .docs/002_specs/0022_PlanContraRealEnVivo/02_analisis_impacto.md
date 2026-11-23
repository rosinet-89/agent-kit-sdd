# 02 — Análisis de impacto: Plan contra real en vivo

## 1. Resumen del requerimiento
La tabla Pasos de `05_avance.md` suma Estimado, Real y Delta, se siembra al aprobar las tareas y se
actualiza en la misma escritura que cada fila de Tiempos. Una fila «Tiempo» en el PUNTO DE RETOMO
lleva los totales. El estado lee esos números y el cierre los completa sin crear otra tabla (S1).

### 1.1 Antecedentes
- `0022_PlanContraRealEnVivo`: es esta carpeta, registrada a las 19:25 con la idea de partida.
- `0019_FlujoEficiente`: cascada, lotes y olas. Se reutiliza la regla «la fila de lote cuenta en la
  fase de su primer paso», que vive en `avance-y-tiempos.md` › «Mínimo y lote». Hasta ahora, «los
  lotes paralelos suman cada uno sus minutos» solo estaba escrito en las tablas de plan contra real de
  0019, 0020 y 0021; aquí pasa a la fuente (RF-3).
- `0020_RuteoDeModelos`: su decisión 19 es el origen de esta idea. Sus tablas de plan contra real
  muestran estimados de 4 a 5 veces lo real. El ajuste de los estimados queda fuera de alcance.
- `0021_BitacoraDeUso`: escribir la fila de Tiempos y la entrada de la bitácora en una sola escritura
  (`MEMORY.md`, 2026-11-22). Aquí se agrega a esa misma escritura la actualización de la tabla.
- `0012_EstatusDeSpecYCierre` y `0016_CierreEnUnaCorrida`: leyenda de estatus y criterio 1 (una sola
  tabla de plan contra real). RF-13 y RF-14 ajustan ese texto sin cambiar cómo se cuentan los pasos.
- Nada contradice: ningún antecedente escribe la tabla antes del cierre.

### 1.2 Deuda técnica relacionada
Ninguna abierta en `.docs/003_deuda/README.md` (revisado el 2026-11-22: de 001 a 0014, todas
`Resuelta`).

## 2. Módulos, menú, tablas y archivos involucrados
### 2.1 Menú / opciones / rutas de acceso
Comandos `/sdd-tasks` (siembra), `/sdd-implement` (la sesión registra cada paso o lote), `/sdd-status`
(lee) y `/sdd-cierre` (completa).
### 2.2 Tablas
No aplica (sin BD).
### 2.3 Archivos
| Archivo | Hoy | Impacto |
|---|---|---|
| `template/.agents/skills/sdd/avance-y-tiempos.md` | Estructura (6 secciones), PUNTO DE RETOMO, Tiempos, «Plan contra real (al cierre)» | Fuente de la regla nueva: tabla Pasos con tiempos, fila «Tiempo», cierre que completa |
| `template/.agents/skills/sdd/plantillas.md` | Plantilla de `05_avance.md` con «## Pasos» vacía | Columnas de la tabla Pasos y fila «Tiempo» |
| `template/.agents/commands/sdd-tasks.md` | Punto 4: muestra fases y estimados, anota la aprobación | Al aprobar, la sesión siembra la tabla (remite a la fuente) |
| `template/.agents/commands/sdd-implement.md` | Punto 8: remite la fila de lote a «Mínimo y lote» | Remite también el real del lote en la tabla |
| `template/.agents/commands/sdd-status.md` | «horas acumuladas» sumando Tiempos | Lee la tabla y la fila «Tiempo» si existen (RF-11) |
| `template/.agents/commands/sdd-cierre.md` | Punto 6: escribe la tabla de plan contra real; línea de ajuste «si se desvió fuerte» | Completa la tabla viva; línea de ajuste siempre (S5) |
| `template/.docs/002_specs/README.md` y `.docs/002_specs/README.md` | Leyenda: criterio 1 y «Un paso que cuenta» (texto idéntico en ambos) | RF-13 y RF-14 |
| `ChangelogAgentKitSdd.md`, `VersionAgentKitSdd`, `manifiestos/0.17.0.md` | 0.16.0 | Versión 0.17.0 |
| `.agents/` de la raíz (local) | Réplica de `template/.agents` | `cp` y `diff -rq` |

`agents/coordinator.md` no cambia: lleva `05_avance.md` según el contrato y no copia sus reglas. Lo
mismo `sdd-change.md`: los estimados se vuelven a sembrar al reaprobar las tareas (RF-10), que es
`/sdd-tasks`. `validar-estructura.md` (E13, E22) remite a la leyenda y no cambia.
### 2.4 Código legado
Las tablas «Plan contra real» de las specs cerradas se quedan como están (fuera de alcance).

## 3. Datos reales
No aplica: no hay BD. Medición en el repo (2026-11-22): «plan contra real» aparece en 4 archivos de
`template/` (`avance-y-tiempos.md`, `plantillas.md`, `sdd-cierre.md` y el README de specs) y en ningún
doc de la raíz (`README.md`, `INSTALAR.md`). La leyenda de la raíz y la de `template/` son idénticas
(`diff` vacío).

## 4. Flujo actual
1. `tasks`: estimado por fase en `03_plan.md` › 7 (o «estimado: N» en `04_tasks.md`).
2. Cada paso: la sesión escribe una fila en Tiempos (y la de la bitácora, si está activa).
3. A media implementación: hay que abrir el plan y sumar Tiempos a mano.
4. `/sdd-status`: suma las horas desde Tiempos.
5. `/sdd-cierre`: escribe la tabla de plan contra real y una línea de ajuste «si se desvió fuerte».

## 5. Reglas de negocio: conocidas vs código
- Lote: cuenta en la fase de su primer paso (`avance-y-tiempos.md`). Los lotes paralelos suman cada
  uno los suyos: estaba solo en las tablas de 0019 a 0021, no en la fuente.
- Una fila con `pausa por confirmar` no entra al total (contrato). Se respeta (RF-6).
- La leyenda cuenta como paso cada fila de implementación de la tabla Pasos. RF-14 protege ese conteo
  frente a las filas nuevas.

## 6. Riesgos y casos borde
- **Conteo de pasos:** las filas de fase, Total y Documentación SDD podrían contarse como pasos en
  `/sdd-status`, E22 o la etapa de la carpeta. Mitigación: RF-14, escrito en la leyenda.
- **Costo:** actualizar la tabla en una escritura aparte suma lectura de caché por llamada (`MEMORY.md`,
  2026-11-22). Mitigación: la misma escritura que la fila de Tiempos.
- **Descuadre:** la tabla viva y Tiempos pueden no cuadrar. Mitigación: manda Tiempos y el cierre
  recalcula (RF-12).
- **Huella del manifiesto:** cualquier cambio a `template/` después de E20 deja la huella vieja (trampa
  de `MEMORY.md`). Mitigación: el manifiesto va en la última fase.
- **Réplica:** el `.agents/` de la raíz no está en Git. Mitigación: `diff -rq` con las excepciones
  aceptadas.

## 7. Preguntas abiertas
- ¿Se registra S1 (la tabla Pasos con tiempos reemplaza a la del cierre) como ADR? Hay una alternativa
  real (mantener dos tablas) y efecto duradero. Se propone el ADR 0039; no se crea sin visto bueno.
