# 02 — Análisis de impacto: Flujo eficiente

## 1. Resumen del requerimiento
Cuatro ajustes al flujo `sdd` (RF-1 a RF-16):
- comprobación antes de lanzar subagentes, sin agentes fuera del kit;
- `/sdd-implement` en cascada con lotes y olas;
- clarify con autochequeo y pasadas por delta;
- chequeos de cierre declarados en `AGENTS.md`, con E20 en el repo del kit.

### 1.1 Antecedentes
- `0013_ModelosPorAgente`: creó «Dónde corre cada rol» y «Modelo por rol». Se reutiliza tal cual; RF-1
  agrega la comprobación previa sin cambiar la regla de mismo modelo.
- `0014_ConfiguracionGuiada`: creó `## Subagentes` y `## Clarify` en `.agents/local/configuracion.md`,
  y el modo de corrección. RF-1 cita `## Subagentes` y RF-14 deja intactos el tope y el modo.
- `0016_CierreEnUnaCorrida`: fijó el orden del cierre. RF-15 se inserta en la comprobación previa, que
  va primero, sin tocar el resto del orden.
- `0017_PausaOrigenYContexto`: agrupó pasos en 12 lanzamientos (el precedente de RF-7), tocó E20 y
  `sdd-estructura` («no lanza un subagente»), y dejó «Mínimo y lote» en `avance-y-tiempos.md`.
- No contradice ninguna; endurece y completa.

### 1.2 Deuda técnica relacionada
Ninguna abierta (`.docs/003_deuda/README.md`: todas `Resuelta`).

## 2. Módulos, menú, tablas y archivos involucrados
### 2.1 Menú / opciones / rutas de acceso
Comandos `/sdd-implement`, `/sdd-clarify`, `/sdd-cierre`, `/sdd-tasks` y `/sdd-init`.

### 2.2 Tablas
No aplica.

### 2.3 Archivos

| Archivo | Qué cambia | RF |
|---|---|---|
| `template/.agents/skills/sdd/SKILL.md` | «Dónde corre cada rol»: comprobación previa y lista cerrada; fila «Implementación» de la tabla de fases (paso o lote) | 1, 2, 5 |
| `template/.agents/skills/sdd/avance-y-tiempos.md` | Nota con el motivo del lanzamiento; fila de lote en «Mínimo y lote» | 3, 10 |
| `template/.agents/commands/sdd-tasks.md` | Sin pasos que deleguen la validación de estructura; siguiente paso `/sdd-implement $1` | 4, 5 |
| `template/.agents/commands/sdd-implement.md` | Tres formas (todo, fase, paso), tabla previa, lotes, olas, revisión por lote, paro en lo ajeno | 5 a 9 |
| `template/.agents/agents/implementer.md` | La descripción y el cuerpo admiten un lote | 7 |
| `template/.agents/skills/sdd/topes-y-modos.md` | Autochequeo del planner y pasadas por delta | 11 a 14 |
| `template/.agents/commands/sdd-clarify.md` | Remite al autochequeo y al delta | 11, 12 |
| `template/.agents/commands/sdd-cierre.md` | La comprobación previa corre los chequeos de cierre | 15 |
| `template/AGENTS.md` | Línea «Chequeos de cierre: No aplica» en Comandos | 15 |
| `template/.agents/commands/sdd-init.md` | Propone los chequeos de cierre junto con Comandos | 15 |
| `AGENTS.md` (raíz) | Chequeo de cierre E20 cuando cambia `VersionAgentKitSdd` | 16 |
| `README.md` (raíz) | Diagrama y ejemplo «un paso a la vez»; tablas de comando y rol | 5 |
| `VersionAgentKitSdd`, `ChangelogAgentKitSdd.md`, `manifiestos/0.14.0.md` | Versión 0.14.0 | RNF-3 |

Réplica: cada archivo de `template/.agents/` se copia a `.agents/` (trampa de `MEMORY.md`, 2026-11-03).

### 2.4 Código legado o sin uso
No aplica.

## 3. Datos reales
No aplica (sin BD). Medición de origen: reporte de sesiones del 2026-11-21 y 2026-11-22. Los lotes
de 0017 promediaron unos 30 s por lote; los pasos sueltos de 0013, unos 13 s y 15 000 a 20 000
tokens de escritura de caché por arranque.

## 4. Flujo actual
- `/sdd-implement` hace un paso y para; la cascada solo existe como preferencia personal.
- Clarify revisa la spec entera en cada pasada.
- La regla de dónde corre cada rol existe, pero no exige una comprobación antes de cada lanzamiento
  ni prohíbe agentes genéricos.
- El cierre no corre E20.

## 5. Reglas de negocio: conocidas vs código
- La separación entre producir y validar es por rol (`SKILL.md`). El autochequeo de RF-11 no la
  sustituye.
- «Mínimo y lote» prohíbe filas de lote de 0 minutos. RF-10 respeta ese mínimo.
- Constitución, principio 1: E20 y los manifiestos no entran en `template/`.

## 6. Riesgos y casos borde
- `template/AGENTS.md` está en 9 603 bytes tras 0015 y 0017. Una línea suma unos 40 bytes: se mide.
- Un lote mal formado puede esconder un fallo. Lo mitigan RF-8 (revisión del diff de cada paso) y el
  tope de 4 pasos.
- Coordinator: `coordinator.md` remite a `sdd-implement.md` y no cambia. Su lista de herramientas ya
  excluye `general-purpose`.
- E9: `argument-hint` de `sdd-implement` debe seguir presente si cambia.

## 7. Preguntas abiertas
Ninguna. Decisión marcada como candidata a ADR: cascada por omisión con lotes (S-1, S-2), en
`03_plan.md` › Decisiones.
