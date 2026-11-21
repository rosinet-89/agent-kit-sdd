# 02 — Análisis de impacto: Configuración guiada del kit

## 1. Resumen del requerimiento
Un asistente (`/sdd-config`, invocado también por `/sdd-init` y por la actualización) configura paso a paso, por dev y sin versionar, los modelos por rol, los roles que corren siempre como subagente, el tope y el modo de corrección de clarify, los entornos de QA y la presentación de las respuestas. El kit solo sugiere (`.agents/configuracion.ejemplo.md`); lo elegido vive en `.agents/local/`. Los modelos propios se aplican con adaptadores de rol en la carpeta de agentes de cada herramienta, nunca en `.agents/agents/`. Auditor externo opcional y beta. Spec aprobada el 2026-11-21 14:53 (RF-1 a RF-22).

### 1.1 Antecedentes
- `0013_ModelosPorAgente` (`Cerrado`): fija la tabla «Modelo por rol», el paso 3 de `/sdd-init` que escribe `inherit` y E23. Esta spec reemplaza ese paso (RF-15) y «la tabla del proyecto» (RF-16); se reutiliza el criterio de detección de herramientas de `/sdd-init` (RF-9).
- `007_DeudasAbiertas` (`Cerrado`): registró que `model` está soportado en Cursor, Gemini CLI, GitHub Copilot y Kiro (deuda 005, registro de campos de `restricciones-de-rol.md`, lectura del 2026-10-25). Se reutiliza en F0.
- `0009_EnlacesTrasPull` (`Cerrado`): `/sdd-enlaces`, estado «visto» y chequeo de arranque; aquí se extienden (RF-17, RF-18).
- `0011_ReducirCeremoniaSdd` (`Cerrado`): fuente única del tope de clarify en `topes-y-modos.md`; aquí se añade el modo de corrección (RF-22) sin mover la fuente.
- `0015_ArranqueLigero` (aprobada, sin plan): toca `AGENTS.md` y `sync-agents.md` › «Chequeo de arranque», igual que esta. Su criterio (nada que no haga falta en cada turno va en `AGENTS.md`) se cumple aquí. Ver §6, riesgo 1.

### 1.2 Deuda técnica relacionada
Ninguna abierta del módulo (`sdd-init`, `sdd-enlaces`, `sync-agents`, `restricciones-de-rol`). La 0012 (comandos de fase no actualizan la celda del índice) no se toca.

## 2. Módulos, menú, tablas y archivos involucrados
### 2.1 Menú / opciones / rutas de acceso
- Comando nuevo `/sdd-config`. Entradas existentes: `/sdd-init` (instalar), `ACTUALIZAR.md` (actualizar), chequeo de arranque (aviso), `/sdd-enlaces` (aplicar modelos), `/sdd-clarify` (modo de corrección).

### 2.2 Tablas (existentes y nuevas)
No aplica: el kit no tiene base de datos.

### 2.3 Archivos de código (ruta, capa)
Todos en `template/` (se distribuye) salvo los marcados; la raíz `.agents/` se replica con `cp`.

| Archivo | Cambio | RF |
|---|---|---|
| `template/.agents/configuracion.ejemplo.md` | Nuevo: pasos, valores sugeridos (citando su fuente), tabla de equivalencias por herramienta y nivel, sección del auditor | RF-6, RF-10, RF-13 |
| `template/.agents/commands/sdd-config.md` | Nuevo: el asistente | RF-2 a RF-4, RF-7, RF-9, RF-12, RF-20 |
| `template/.agents/commands/sdd-init.md` | Invoca el asistente tras la entrevista; reemplaza el paso 3 (`inherit`) | RF-1, RF-15 |
| `ACTUALIZAR.md` (raíz, distribuible) | Invoca el asistente para los pasos sin valor; caso `model: inherit` heredado | RF-2 |
| `template/.agents/preferencias.ejemplo.md` | Remisión a `.agents/local/configuracion.md` | RF-5 |
| `template/AGENTS.md` | `/sdd-config` en la lista de comandos; paso 10 lee la sección «Presentación» | RF-19 |
| `template/.agents/skills/sdd/restricciones-de-rol.md` | «La tabla del proyecto» pasa a la configuración local | RF-16 |
| `template/.agents/skills/sdd/topes-y-modos.md` | «pasada n de N», modo de corrección | RF-21, RF-22 |
| `template/.agents/skills/sdd/avance-y-tiempos.md` | «pasada n de N» | RF-21 |
| `template/.agents/commands/sdd-clarify.md` | Modo automático o con aprobación | RF-22 |
| `template/.agents/skills/sdd/SKILL.md` | «Dónde corre cada rol» lee los roles configurados; sección nueva «Auditor externo (beta)» | RF-6.2, RF-8 a RF-11 |
| `template/.agents/sync-agents.md` | Carpeta de agentes por archivo, adaptador de rol, chequeo de arranque | RF-14, RF-17, RF-18 |
| `template/.agents/commands/sdd-enlaces.md` | Crea y corrige carpetas por archivo y adaptadores | RF-14, RF-17 |
| `template/.agents/validar-estructura.md` | E3 y E23 reconocen adaptadores; adaptador viejo | RF-17 |
| `template/.agents/README.md`, `template/.agents/agents/coordinator.md` | Lista de comandos | — |
| `README.md`, `INSTALAR.md` (raíz) | Mención de `/sdd-config` | — |
| `VersionAgentKitSdd`, `ChangelogAgentKitSdd.md`, `manifiestos/0.10.0.md` | Versión 0.10.0 | Criterios |
| `AGENTS.md` (raíz, versionado) | Lista de comandos | — |

### 2.4 Código legado o sin uso
- La regla de `inherit` en `.agents/agents/` del paso 3 de `/sdd-init` (0013) queda reemplazada; los proyectos que ya la aplicaron se tratan en la actualización (caso límite de la spec).

## 3. Datos reales
No aplica (sin BD). Medición del 2026-11-21 sobre el clon: `.claude/agents`, `.cursor/agents` son enlaces de carpeta a `../.agents/agents` (`ls -la .claude/`; `sync-agents.md` tabla de enlaces); `.opencode/agents/` ya usa adaptadores. Versión actual del kit: 0.9.0.

## 4. Flujo actual
`/sdd-init`: reconocimiento → entrevista (≤ 6 preguntas) → propuesta → aplicar (paso 3 escribe `model:` según herramientas detectadas) → sugiere `/sdd-enlaces`. No hay configuración por dev salvo `PREFERENCIAS.md` (texto libre) y `entornos.md`. `/sdd-clarify` pide aprobación tras cada pasada.

## 5. Reglas de negocio: conocidas vs código
- Fuente única de modelos de Claude Code: `restricciones-de-rol.md` › «Modelo por rol» (se mantiene).
- Fuente única del tope de clarify: `topes-y-modos.md` (se mantiene; el valor del dev se lee en `/sdd-clarify`).
- ADR 007: la actualización solo reemplaza archivos KIT-VIEJO; `.agents/local/` y adaptadores no son del kit.
- ADR 004 y constitución 5: sin scripts; el asistente es un comando Markdown que sigue el agente.

## 6. Riesgos y casos borde
1. **Choque con la 0015** en `template/AGENTS.md` y `sync-agents.md` › «Chequeo de arranque». Mitigación: implementar la 0014 antes de la 0015; la 0015 mide bytes después.
2. **Herramientas sin formato de agente** (Codex, Antigravity: sin fila en la tabla de enlaces de `sync-agents.md`): el modelo sugerido de su fila no se puede aplicar por rol. Se muestra como sugerencia para la sesión y rige `inherit` (caso límite de la spec).
3. **Adaptador de Claude Code**: cuerpo que solo remite; el subagente debe leer `.agents/agents/<rol>.md` al arrancar (todos los roles tienen `Read`). Se verifica en F3 con un ensayo.
4. **Enlace por archivo relativo** `../../.agents/agents/<rol>.md`: se comprueba con `readlink -f` en el ensayo.
5. **Credenciales en `auditor.md`** (el comando del auditor podría llevar una clave): mismo criterio que `entornos.ejemplo.md`: sin credenciales, solo el nombre de la variable de entorno.
6. **Nombres de modelos** de la tabla envejecen: el ejemplo lleva fecha de la tabla y remite a la persona para actualizarla.

## 7. Preguntas abiertas
Ninguna que bloquee. Candidatos a ADR (decide la persona): (a) configuración por dev no versionada con sugerencias en un ejemplo; (b) carpeta de agentes por archivo con adaptadores en toda herramienta distinta de Claude Code (reemplaza parte del ADR 0028).
