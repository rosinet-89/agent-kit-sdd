# 03 — Plan: Configuración guiada del kit
Spec: 01_spec.md (aprobada 2026-11-21 14:53) · Plan aprobado 2026-11-21 14:59

## 1. Ubicación y capas
- Producto: `template/` y docs distribuibles de la raíz (`ACTUALIZAR.md`, `INSTALAR.md`, `README.md`, versión, changelog). Conocimiento del kit: `.docs/`, `AGENTS.md` raíz. Herramienta local: `.agents/` raíz, réplica de `template/.agents` con `cp` (constitución 3, ADR 001).
- Patrón: cada regla en una sola fuente; los demás archivos remiten. Fuentes de esta spec:
  - Pasos, valores sugeridos y tabla de equivalencias: `configuracion.ejemplo.md` (cita las fuentes que ya existen, no las copia).
  - Procedimiento del asistente: `commands/sdd-config.md`.
  - Carpeta de agentes por archivo y adaptador de rol: `sync-agents.md`.
  - Modo de corrección de clarify: `topes-y-modos.md`.
  - Auditor externo: `SKILL.md` › «Auditor externo (beta)».

## 2. Archivos que se crean o modifican
| Archivo | Capa | Responsabilidad | RF |
|---|---|---|---|
| `template/.agents/configuracion.ejemplo.md` (nuevo) | template | Pasos con contexto ≤ 2 líneas, valor sugerido (remitiendo a su fuente), formato de `.agents/local/configuracion.md` (primera línea `version:`; una sección por paso), tabla de equivalencias (Cursor, Antigravity, Codex, OpenCode; niveles alto, medio, bajo; effort como sugerencia; fecha de la tabla), formato de `auditor.md` sin credenciales | RF-6, RF-10, RF-13, RF-18 |
| `template/.agents/commands/sdd-config.md` (nuevo) | template | Asistente: lee ejemplo y configuración local, pregunta solo pasos sin valor (actualización) o todos con valor actual (RF-12), uno por uno, salto con sugerido y línea final, valores inválidos (RF-7), auditor si se pide o hay dos herramientas (RF-8, RF-9), S-8 y confirmación por destino (RF-20), migración propuesta de `PREFERENCIAS.md`, al final sugiere `/sdd-enlaces` si cambió algún modelo. No escribe archivos versionados | RF-2 a RF-5, RF-7 a RF-10, RF-12, RF-20 |
| `template/.agents/commands/sdd-init.md` | template | Tras la entrevista invoca `/sdd-config` (fuera del tope); paso 3 deja de escribir `model:` y remite a RF-15 | RF-1, RF-15 |
| `ACTUALIZAR.md` | raíz distribuible | Tras actualizar: `/sdd-config` para pasos sin valor; detectar `model: inherit` heredado y proponer pasarlo a adaptadores | RF-2, caso límite |
| `template/.agents/preferencias.ejemplo.md` | template | Una línea de remisión a `.agents/local/configuracion.md` | RF-5 |
| `template/AGENTS.md` | template | `/sdd-config` en «Roles y comandos»; paso 10 de «Cada sesión»: tras `PREFERENCIAS.md`, la sección «Presentación» de la configuración local | RF-19 |
| `template/.agents/skills/sdd/restricciones-de-rol.md` | template | «La tabla del proyecto» → sección de modelos de la configuración local | RF-16 |
| `template/.agents/skills/sdd/topes-y-modos.md` | template | «pasada n de N» (N del dev que corre la pasada, conteo por spec); sección «Modo de corrección» (automático y con aprobación, supuestos marcados, aprobación de la persona) | RF-21, RF-22 |
| `template/.agents/skills/sdd/avance-y-tiempos.md` | template | «pasada n de N», remite a `topes-y-modos.md` | RF-21 |
| `template/.agents/commands/sdd-clarify.md` | template | Pasos 3 y 4 según el modo de corrección (remite) | RF-22 |
| `template/.agents/skills/sdd/SKILL.md` | template | «Dónde corre cada rol»: además del pedido en el chat, los roles configurados como subagente; sección «Auditor externo (beta)» | RF-6.2, RF-8 a RF-11 |
| `template/.agents/sync-agents.md` | template | Definición de carpeta por archivo y adaptador de rol (generaliza la de OpenCode); filas de la tabla; «Chequeo de arranque» suma el aviso de configuración | RF-14, RF-17, RF-18 |
| `template/.agents/commands/sdd-enlaces.md` | template | Crea por archivo la carpeta de herramientas distintas de Claude Code; en Claude Code, por archivo solo con modelos propios; regenera adaptadores viejos conservando `model:` | RF-14, RF-17 |
| `template/.agents/validar-estructura.md` | template | E3: adaptador válido en cualquier carpeta por archivo; E23: compara también el `model:` de los adaptadores con la configuración local; aviso de adaptador viejo | RF-17 |
| `template/.agents/README.md`, `template/.agents/agents/coordinator.md` | template | Lista de comandos y archivos | — |
| `README.md`, `INSTALAR.md` | raíz distribuible | Mención de `/sdd-config` | — |
| `AGENTS.md` | raíz versionada | Lista de comandos | — |
| `VersionAgentKitSdd`, `ChangelogAgentKitSdd.md`, `manifiestos/0.10.0.md` | raíz | 0.10.0 | Criterios |
| `PREFERENCIAS.md` (raíz, local) | local | No se toca en la implementación; la migración la propone `/sdd-config` cuando la persona lo corra | — |

## 3. Base de datos
No aplica.

## 4. Seguridad
- Guardrails intactos: el auditor es solo lectura y cumple `AGENTS.md` › Guardrails (RF-11); la excepción de QA sigue siendo solo de `tester`.
- Entornos: S-8 antes de declarar y confirmación por destino (RF-20).
- Sin credenciales en `.agents/local/configuracion.md` ni en `auditor.md` (solo nombres de variables de entorno), mismo criterio que `entornos.ejemplo.md`.
- `/sdd-config` y `/sdd-enlaces` no escriben archivos versionados; los adaptadores y enlaces se crean con confirmación (ADR 007).
- Validación de entradas: valores fuera de las opciones del paso → sugerido + aviso (RF-7).

## 5. Liberación / piloto
Versión 0.10.0. Piloto: este clon (modo mantenimiento) corre `/sdd-config` tras la réplica, con Claude Code y Cursor (combinación probada del auditor).

## 6. Decisiones
| Decisión | Alternativa descartada | Por qué |
|---|---|---|
| Asistente como comando propio `/sdd-config` | Dentro de `/sdd-init` | `/sdd-init` corre una vez; el asistente se repite (RF-12) y lo necesita un dev nuevo |
| Adaptador de rol generaliza el de OpenCode | Formato propio por herramienta | Una definición; OpenCode ya prueba el patrón |
| Herramientas distintas de Claude Code siempre por archivo | Enlace de carpeta hasta que el dev configure | Evita que lean `model: opus` (RF-17) |
| Codex y Antigravity: tabla solo como sugerencia | Crear carpetas para ellas | No tienen fila en la tabla de enlaces ni formato de agente registrado; fuera de alcance |
| Implementar antes que la 0015 | Después | La 0015 mide bytes del `AGENTS.md`; medir sobre el texto final |
| Configuración por dev: [ADR 0029](../../004_adr/0029_ConfiguracionPorDevNoVersionada.md); carpeta por archivo: [ADR 0030](../../004_adr/0030_CarpetaDeAgentesPorArchivo.md) | Solo decisiones en `05_avance.md` | Efecto duradero y alternativas reales |

## 7. Fases y estimado
| Fase | Contenido | RF | Estimado (min) |
|---|---|---|---|
| F0 | Diagnóstico: confirmar con el registro de campos qué herramientas admiten `model` por rol; copias de los archivos a tocar; base para el manifiesto | — | 10 |
| F1 | Sugerencias y fuentes: `configuracion.ejemplo.md`, `preferencias.ejemplo.md`, `restricciones-de-rol.md`, `topes-y-modos.md`, `avance-y-tiempos.md` | RF-5, RF-6, RF-10, RF-13, RF-16, RF-18, RF-21, RF-22 | 30 |
| F2 | Asistente y entradas: `sdd-config.md`, `sdd-init.md`, `ACTUALIZAR.md`, `template/AGENTS.md`, listas de comandos (README, INSTALAR, coordinator, `.agents/README.md`, `AGENTS.md` raíz) | RF-1 a RF-4, RF-7 a RF-9, RF-12, RF-15, RF-19, RF-20 | 35 |
| F3 | Enlaces y estructura: `sync-agents.md`, `sdd-enlaces.md`, `validar-estructura.md`; ensayo de carpeta por archivo y adaptador en temporal | RF-14, RF-17, RF-18 | 35 |
| F4 | Clarify y auditor: `sdd-clarify.md`, `SKILL.md` | RF-6.2, RF-8 a RF-11, RF-22 | 25 |
| F5 | Cierre técnico: réplica a `.agents/` raíz (`diff -rq` con las diferencias aceptadas), lint por archivo, ensayo de instalación en `mktemp -d`, versión 0.10.0, changelog, manifiesto | Criterios | 20 |
| | **Total** | | **155** |

## 8. Criterios de aceptación
- Cada RF tiene al menos un archivo en §2 y una fase en §7.
- Ensayo de instalación en `mktemp -d` con salto total: `.agents/local/configuracion.md` con los sugeridos, `.claude/agents` enlace de carpeta, ningún aviso; `git status` del temporal sin cambios versionados por la configuración.
- Ensayo con modelos propios: adaptadores en `.claude/agents/` y `.cursor/agents/`, enlaces por archivo para el resto (`readlink -f` resuelve), `.agents/agents/` sin cambios.
- Ensayo de actualización: valores propios conservados; paso nuevo avisado por el chequeo de arranque.
- Ninguna fuente única duplicada (grep de los valores sugeridos: `opus`, «3 pasadas», solo en su fuente).
- Lint de `AGENTS.md` › Comandos en cada archivo tocado; `diff -rq template/.agents .agents` solo con las diferencias aceptadas de `MEMORY.md`.
- `/sdd-validate` RF por RF; versión, changelog y manifiesto 0.10.0.
