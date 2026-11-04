# 02 — Análisis de impacto: Guardrails globales, la base de datos solo se lee
Fecha: 2026-10-31

## 1. Resumen del requerimiento
Hoy la regla «los agentes solo leen la base de datos» está repartida en `AGENTS.md` › Comandos, la skill `sdd`, `restricciones-de-rol.md` (solo `planner`), la skill `migracion-bd`, los agentes, tres comandos (`sdd-implement`, `sdd-constitucion`, `sdd-init`) y la constitución que propone `/sdd-init`. Ninguna cubre a un agente que trabaja fuera del flujo `sdd`, y `planner`, `reviewer` y `coordinator` no reciben `AGENTS.md` inyectado (`omitClaudeMd`). Se escribe la regla una vez, en `template/AGENTS.md` › «Guardrails (todos los agentes, siempre)», se retira la redacción propia de las demás piezas (lista fija de RF-13) y se repite el cambio en la raíz del clon (RF-19). Es un cambio solo de Markdown, sin código ni datos.

### 1.1 Antecedentes (índice de planes)
- Sin coincidencia directa en `.docs/002_specs/README.md` para «guardrails» ni «solo lectura».
- Relacionadas y se reutilizan:
  - `002_Kit050`: origen de la skill `migracion-bd`. Se conserva su procedimiento de escritura del SQL; solo cambia la frase que fija la prohibición.
  - `003_OptimizarArnes` (ADR 009, fuente única por fase): la regla vive una vez y el resto remite. 0008 aplica ese criterio a la BD.
  - `004_ModeloYPermisosAgentes` (ADR 011, 012, 013): `restricciones-de-rol.md` como fuente única de restricciones, `omitClaudeMd` en tres roles y modelo explícito. De ahí sale que la regla no pueda depender de la inyección de `AGENTS.md`.
- No contradice ninguna; amplía la fila de `planner` de `restricciones-de-rol.md` sin ampliar permisos.
- Paralelas (misma versión 0.6.0): `0009_EnlacesTrasPull` y `0010_FalsoPositivoInitPendiente`, ambas sin cerrar y con archivos comunes (ver 6).

### 1.2 Deuda técnica relacionada
Ninguna abierta (`.docs/003_deuda/README.md`).

## 2. Módulos, menú, tablas y archivos involucrados
### 2.1 Menú / opciones / rutas de acceso
No aplica: no hay pantallas. Se tocan piezas del kit que los agentes leen: `AGENTS.md` (sección nueva), los 4 agentes, dos skills y tres comandos.

### 2.2 Tablas (existentes y nuevas)
No aplica. No hay tablas ni scripts `sql/`.

### 2.3 Archivos de código (ruta, capa)
Todos son Markdown. Capas del kit: instrucciones globales, agentes, skills, comandos.

El árbol `template/` y la raíz del clon son iguales en `.agents/` (comprobado con `diff -q` el 2026-10-31: sin diferencias en los diez archivos de `.agents/` afectados). Solo `AGENTS.md` difiere entre ambos, por su contenido propio.

| # | Archivo en `template/` | Réplica en la raíz | RF |
|---|---|---|---|
| 1 | `template/AGENTS.md` | `AGENTS.md` (con lo propio del clon) | 1 a 8, 10, 11, 19 |
| 2 | `template/.agents/agents/coordinator.md` | `.agents/agents/coordinator.md` | 12 |
| 3 | `template/.agents/agents/implementer.md` | `.agents/agents/implementer.md` | 12, 13 |
| 4 | `template/.agents/agents/planner.md` | `.agents/agents/planner.md` | 12 |
| 5 | `template/.agents/agents/reviewer.md` | `.agents/agents/reviewer.md` | 12, 13 |
| 6 | `template/.agents/skills/migracion-bd/SKILL.md` | `.agents/skills/migracion-bd/SKILL.md` | 13 |
| 7 | `template/.agents/skills/sdd/SKILL.md` | `.agents/skills/sdd/SKILL.md` | 13 |
| 8 | `template/.agents/skills/sdd/restricciones-de-rol.md` | `.agents/skills/sdd/restricciones-de-rol.md` | 14, 15 |
| 9 | `template/.agents/commands/sdd-implement.md` | `.agents/commands/sdd-implement.md` | 13 |
| 10 | `template/.agents/commands/sdd-constitucion.md` | `.agents/commands/sdd-constitucion.md` | 13 |
| 11 | `template/.agents/commands/sdd-init.md` | `.agents/commands/sdd-init.md` | 13 |

Total: 11 archivos en `template/` y 11 en la raíz.

Nota de versionado (verificada en `.git/info/exclude`): en la raíz, `.agents/*` está excluido de Git salvo `.agents/napkin.md`. Las 10 réplicas de `.agents/` quedan en disco pero no aparecen en `git status`; solo `AGENTS.md` de la raíz y los archivos de `template/` se versionan. La réplica se comprueba con `diff -rq template/.agents .agents` (chequeo E19), no con Git. Ese diff admite solo dos diferencias previas y ajenas a 0008: `napkin.md` y `skills/caveman` (propios del clon); cualquier otra es una réplica rota.

### 2.4 Código legado o sin uso (no tocar, sí documentar)
- Fuera de la lista fija de RF-13, no se tocan: el encabezado «EJECUCIÓN MANUAL…» y la línea «Lo ejecuta el usuario…» de `migracion-bd`, la fila de `reviewer` sobre la consulta «Verificar», la línea 20 de `planner.md` («Las consultas de datos son de solo lectura…»: procedimiento) y las descripciones de `planner`, `reviewer` y `sdd-validate` («consultas de solo lectura»: capacidad, no prohibición).
- `ACTUALIZAR.md`, la constitución del kit, `INSTALAR.md`, `README.md`, `manifiestos/` y `VersionAgentKitSdd` no cambian en 0008 (RF-16, RF-17).

## 3. Datos reales (SELECT de solo lectura, con fecha y BD)
No aplica. No hay base de datos que consultar. Comprobaciones de archivos hechas el 2026-10-31 sobre el repo (origen: lectura de archivos y `grep`):
- La lista cerrada mínima de RF-9 (y una ampliada, con `-w`) da cero coincidencias en `template/` hoy. Se usa `-w` para que «rails» no coincida con «guardrails».
- Líneas de RF-13 localizadas en las dos copias: `migracion-bd/SKILL.md` líneas 3, 8, 13, 58, 63; `sdd/SKILL.md` 127 y 136; `implementer.md` 3 y 13; `reviewer.md` 21; `sdd-implement.md` 12; `sdd-constitucion.md` 25; `sdd-init.md` 38. Coinciden con la spec.
- `restricciones-de-rol.md` línea 54 dice «Ocho filas»; la tabla tiene 8 filas (líneas 58 a 65).
- Siguiente número de ADR libre: 0019 (último existente, `0018_…`).

## 4. Flujo actual
1. `/sdd-init` pide a la persona cómo se consulta la BD «en solo lectura» y propone una constitución con un principio 5 que repite la regla.
2. `AGENTS.md` › Comandos lleva la línea «Consulta de datos de solo lectura».
3. La skill `sdd` (línea de Datos y anti-patrón) y `migracion-bd` (regla de oro y anti-patrón) fijan que el SQL se genera y lo ejecuta una persona.
4. `implementer.md` (línea 13), `reviewer.md` (línea 21, «No ejecutas escrituras de datos ni cambios de esquema») y `sdd-implement.md` dicen que no ejecutan SQL.
5. `restricciones-de-rol.md` registra la regla para `implementer` (junto a Git) y para `planner` (consulta en modo lectura). `coordinator` y `reviewer` no tienen fila propia de BD.
6. `planner`, `reviewer` y `coordinator` declaran `omitClaudeMd` y solo leen de `AGENTS.md` las secciones que su instrucción nombra. Ninguna es «Guardrails», que no existe.
7. Un agente fuera del flujo `sdd` no tiene ninguna regla explícita de BD.

## 5. Reglas de negocio: conocidas vs código
| Regla | Fuente conocida (spec/decisión) | Dónde está hoy en el código | Estado |
|---|---|---|---|
| La BD solo se lee, en cualquier entorno | RF-3, RF-4 | Repartida; solo flujo `sdd` y `planner` | Por concentrar en «Guardrails» |
| Lista cerrada de lectura con precedencia | Definiciones | No existe | Nueva |
| Comando de `Comandos` que puede escribir: no se ejecuta, ni contra BD local | RF-5, RF-8 (decisión firme) | No existe | Nueva |
| El script lo escribe solo `implementer`; los demás roles lo describen en el chat | RF-6b | Implícito en «Qué no hace» de `planner` y `coordinator` | Se verifica en el guardrail, sin cambiar esos «Qué no hace» |
| Regla = instrucción, no garantía | RF-14, RF-15 | Parcial (filas 2 y 3 de `restricciones-de-rol.md`) | Fila nueva |
| Fuente única | ADR 009 | Violada para la BD | Se corrige |

Sin contradicciones entre spec y código. El criterio de duda sobre un comando de `Comandos` (si no se sabe si escribe en una BD, no se ejecuta y se entrega a la persona) quedó confirmado por el usuario, va dentro del guardrail 1 (decisión 3 de `03_plan.md`) y la spec lo recoge como aclaración en RF-5 y RF-8.

## 6. Riesgos y casos borde
- Ediciones concurrentes con 0009 y 0010. Ambas tocan archivos de `template/` que 0008 también toca (`template/AGENTS.md`, comandos `.agents/`, posiblemente `sdd-init.md`) y ninguna está cerrada. Orden de fusión fijado: 0008, 0010, 0009. Mitigación: Edit mínimo con `old_string` acotado, nunca reescribir un archivo entero, releer cada archivo justo antes de editarlo y comprobar al final con `diff -rq template/.agents .agents`. Como el `.agents/` de la raíz no es versionado, un cambio ajeno ahí no se ve en `git status`: hay que comparar con `diff`.
- Ventana de E20: entre la fusión de 0008 y el cierre de 0009 el chequeo E20 (versión y manifiesto coherentes) puede no pasar, porque cambian archivos de `template/` sin manifiesto nuevo. Aceptada por el usuario. 0008 se valida sin manifiesto 0.6.0.
- Una frase retirada deja de decir algo que alguien leía: al reducir `sdd/SKILL.md` línea 136 a una remisión se pierde ahí el anti-patrón «SQL suelto fuera de `sql/`». No se pierde del kit: sigue en `migracion-bd/SKILL.md` línea 63. Es decisión intencional (es regla de escritura del script, no de la prohibición).
- Principio 5 de la constitución propuesta: queda solo la remisión (RF-13) y pierde «`sql/` con rollback, sesión/permisos, sin secretos». Esos temas siguen en `migracion-bd` y `seguridad-basica`.
- Verificación a ojo: la lista fija se revisa línea a línea, no por `grep`. Un grep de sentencias daría falsos positivos con las reglas de redacción del SQL entregado, que quedan excluidas.
- Proyectos existentes: `AGENTS.md` es grupo B; la sección llega como faltante y se incorpora a mano. Los agentes salen KIT-VIEJO o PERSONALIZADO. Si no se fusiona, esos proyectos siguen con agentes que remiten a una sección que su `AGENTS.md` no tiene. Se avisa en el changelog (RF-17).
- Garantía: la regla es una instrucción. Con `Bash` disponible, `planner`, `reviewer`, `coordinator` e `implementer` podrían ejecutar SQL de escritura si desobedecen. Se declara con honestidad en `restricciones-de-rol.md` y se recomienda un usuario de BD de solo lectura (RF-14, RF-15).
- Casos borde de la spec que el plan debe respetar: `SET` de sesión, `EXPLAIN ANALYZE`, CTE con escritura, `SELECT … INTO`, `FOR UPDATE`; pruebas, build o app que escriben en BD (aunque sea local); la conversación que pide explícitamente ejecutar; `Comandos` con «No aplica» (el agente no consulta); contador de filas de 8 a 9 verificado contra la tabla real.

## 7. Preguntas abiertas
Sin preguntas abiertas. Resueltas por el usuario:
1. `reviewer.md` › «Qué no hace» (línea 21) entra en la lista fija de RF-13 y se reemplaza por la remisión a «Guardrails». Mismo cambio en la raíz.
2. Criterio de duda sobre comandos de `Comandos`: confirmado dentro del guardrail 1 (ante duda, no se ejecuta y se entrega a la persona); aclarado en RF-5 y RF-8.
3. El ADR 0019 se crea en F7 de forma incondicional (RF-18).
4. Principio 5 de la constitución que propone `/sdd-init`: solo remisión a «Guardrails».
5. Las líneas de changelog de 0008 van al `05_avance.md` de 0008 para pegarlas al cerrar 0009.
