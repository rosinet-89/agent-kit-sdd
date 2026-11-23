# 03 — Plan: Bitácora de uso (beta)
Spec: 01_spec.md (aprobada 2026-11-22 20:14)

## 1. Ubicación y capas
Repo del kit: lo distribuible en `template/` y los docs de la raíz (`AGENTS.md` › Reglas de negocio).
Capas del kit: skill nueva (fuente única) → configuración de ejemplo → comandos que remiten →
contrato de tiempos (una línea de remisión) → `AGENTS.md` (fila de skill) → docs, versión y
manifiesto. Solo Markdown (constitución, principio 5).

Nombre y secciones fijas de la skill (los demás archivos remiten a ellas, por eso F1 a F4 van en
una sola ola): `template/.agents/skills/bitacora-uso/SKILL.md` con `## Activación`, `## Formato`,
`## Registro por tramo`, `## Actividad`, `## Fuente de tokens`, `## Completar tokens`, `## Reporte`
y `## Privacidad`.

## 2. Archivos que se crean o modifican
| Archivo | Capa | Responsabilidad | RF |
|---|---|---|---|
| `template/.agents/skills/bitacora-uso/SKILL.md` (nuevo) | skill | Frontmatter (`name: bitacora-uso`, `description` ≤ 1024 con disparadores). «Activación»: interruptor, decisión por spec, pregunta de `/sdd-spec` con sugerido según el interruptor, RF-7, cambio de decisión, apagado a mitad. «Formato»: archivo por spec o actividad, encabezado y tabla de entradas, carpeta creada al primer uso, formato ilegible. «Registro por tramo»: campos, lote, pausa, solo la sesión principal, actividad abierta con `/sdd-*`. «Actividad»: etiqueta, turno, cierre, interruptor apagado. «Fuente de tokens»: tabla por herramienta. «Completar tokens»: ventana, deduplicación, subagentes, «no disponible». «Reporte»: entradas, totales y los cuatro hallazgos. «Privacidad» | 6, 8–16 |
| `template/.agents/configuracion.ejemplo.md` | config | `## Bitácora` en la lista fija de «Formato del archivo local»; «Paso 6. Bitácora de uso (beta)»: contexto (beta, local, qué guarda y qué no, por remisión a «Privacidad»), valores `encendida`/`apagada`, sugerido `apagada` | 1, 2, 3, 17 |
| `template/.agents/commands/sdd-config.md` | comando | `bitacora` en la lista de `[paso]`; en el paso 2, el de bitácora se presenta según el ejemplo y la skill (sin copiar) | 1, 2, 17 |
| `template/.agents/commands/sdd-spec.md` | comando | Paso nuevo antes del 4: si no hay decisión por spec de este dev, una pregunta según la skill › «Activación» | 4, 5 |
| `template/.agents/commands/sdd-cierre.md` | comando | En los tres caminos, cada corrida: completar tokens según la skill › «Completar tokens», si la spec tiene bitácora activa | 12 |
| `template/.agents/skills/sdd/avance-y-tiempos.md` | contrato | En «Tiempos reales», una viñeta: con bitácora activa la sesión principal agrega la entrada al escribir la fila (skill › «Registro por tramo») | 8, 11 |
| `template/AGENTS.md` y `AGENTS.md` (raíz) | instrucciones | Fila «Bitácora de uso por dev (beta): registro por tramo y reporte de tokens» → `.agents/skills/bitacora-uso/SKILL.md` | 16 |
| `README.md` | doc | `bitacora-uso` en el árbol de `template/`; una línea de la bitácora beta (local, apagada por omisión, `/sdd-config`) | 17 |
| `INSTALAR.md` | doc | Paso 7 de «Qué hace el agente»: la bitácora de uso (beta) se activa en `/sdd-config` | 17 |
| `ChangelogAgentKitSdd.md`, `VersionAgentKitSdd` | versión | Entrada 0.16.0 con «beta»; versión 0.16.0 | 17 |
| `manifiestos/0.16.0.md` (nuevo) | manifiesto | Huellas de `template/` tras el último cambio (E20) | — |
| `.agents/` de la raíz (local) | réplica | `cp` de lo tocado en `template/.agents` y `diff -rq` | — |

## 3. Base de datos
No aplica: sin `sql/`.

## 4. Seguridad
- Todo lo escrito queda en `.agents/local/bitacora/`, fuera de Git (`template/.gitignore` ya excluye
  `.agents/local/`).
- Lectura de transcripts: solo los campos `timestamp`, `message.id`, `message.model` y
  `message.usage`; nunca se imprime ni se guarda contenido de mensajes (RF-13). El comando de lectura
  es de un solo uso, no se versiona ni se guarda.
- Sin red, sin issue automático, sin archivos versionados con el reporte (RF-15).
- `seguridad-basica` en el commit: la skill no cita rutas personales ni ids de sesión reales.

## 5. Liberación / piloto
Beta y apagada por omisión (RF-1). Piloto: esta misma copia del kit, activándola para la spec
`0022_PlanContraRealEnVivo` después de instalar 0.16.0 en el clon (fuera de esta spec).

## 6. Decisiones
| Decisión | Alternativa descartada | Por qué |
|---|---|---|
| 1. Skill `bitacora-uso` | `bitacora`; `metricas` | Nombre que dice qué registra; `metricas` choca con piezas personales |
| 2. Un archivo Markdown por spec o actividad en `.agents/local/bitacora/` (`<carpeta>.md`, `actividad-<etiqueta>.md`), con encabezado (decisión por spec y su hora, `estado` de la actividad) y tabla de entradas | Un solo archivo para todo; JSONL | Legible y editable sin código; la decisión por spec vive junto a sus entradas (RF-6); por spec el reporte lee un archivo |
| 3. Sección `## Bitácora` con `encendida` o `apagada`; paso `bitacora` | Valor dentro de `## Presentación` | Un paso, una sección: la lista fija y el chequeo de arranque ya funcionan así |
| 4. El disparo de RF-8 vive en una viñeta de `avance-y-tiempos.md` que remite a la skill | Una línea en cada comando que escribe Tiempos | La fila de Tiempos es el disparador por definición; una sola fuente cubre todos los comandos |
| 5. Tokens por ventana del tramo: Inicio a Fin (minuto completo) convertidos a UTC, sobre la sesión principal y sus `subagents/*.jsonl`, una vez por `message.id`, cuatro campos de `usage`. Más de una sesión del proyecto en la ventana: «no disponible (sesiones paralelas)» salvo que la persona elija | Guardar el id de sesión al registrar; estimar | El agente no ve su id de sesión; estimar lo prohíbe RF-12 |
| 6. Carpeta de transcripts: `$CLAUDE_CONFIG_DIR/projects/<ruta con / como ->/`, o `~/.claude/...` sin esa variable | Solo `~/.claude` | Medido en este repo: `CLAUDE_CONFIG_DIR` apunta a otra carpeta (02 › 3) |
| 7. Fila con pausa > 0: se suman los tokens de la ventana y la entrada lleva «ventana con pausa» | Marcar «no disponible» | Conserva el dato; el reporte lo separa |
| 8. Hallazgo «modelo distinto del rol»: en Claude Code, el alias de «Modelo por rol» (`opus`, `sonnet`, `haiku`) contenido en el id; en otra herramienta, «sin comparar» | Comparar con `## Modelos` local | RF-14 cita «Modelo por rol»; los ids de otras herramientas no traen el alias |
| 9. Una actividad dura lo que la sesión que la abrió; al abrir otra sesión, una `estado: abierta` cuenta como cerrada | Pedir cierre explícito siempre | El agente no detecta el fin de sesión (RF-9) |
| 10. Versión 0.16.0 | 0.15.1 | Skill y paso de configuración nuevos |
| 11. ADR candidato: decisiones 2 y 5 (almacenamiento y captura de tokens); registrado como [ADR 0038](../../004_adr/0038_BitacoraEnMarkdownLocalYTokensPorVentana.md) | Sin ADR | Alternativas reales y efecto duradero; se crea solo con visto bueno |

## 7. Fases y estimado
Estimado en minutos, por ola (`MEMORY.md`, 2026-11-22). F1 a F4 tocan archivos disjuntos: una ola.

| Fase | Contenido | RF | Estimado (min) |
|---|---|---|---|
| F0 | Preparación: releer las secciones citadas y confirmar los nombres de sección fijos | — | 3 |
| F1 | Skill `bitacora-uso/SKILL.md` | 6, 8–16 | 15 |
| F2 | Configuración: `configuracion.ejemplo.md` y `sdd-config.md` | 1, 2, 3, 17 | 6 |
| F3 | Remisiones: `sdd-spec.md`, `sdd-cierre.md`, `avance-y-tiempos.md`, `AGENTS.md` (plantilla y raíz) | 4, 5, 7, 8, 11, 12, 16 | 8 |
| F4 | Docs: `README.md`, `INSTALAR.md`, changelog y versión | 17 | 5 |
| F5 | Réplica (`cp` y `diff -rq`), `/sdd-enlaces` con confirmación, ensayo de instalación, lint por archivo, E5–E7 | criterios 2 y 3 | 8 |
| F6 | Manifiesto 0.16.0 y E20 (después del último cambio a `template/`) | criterio 3 | 5 |
| | **Total** (ola F1–F4 cuenta por su fase más larga) | | **≈ 36** |

## 8. Criterios de aceptación
- Los 17 RF con evidencia del reviewer en la skill y en los archivos que remiten.
- Ninguna regla de la bitácora copiada fuera de la skill: los comandos, el ejemplo y el contrato
  solo remiten (grep de las secciones citadas).
- Ensayo de instalación en `mktemp -d`: `skills/bitacora-uso/` y lo tocado se copian sin pisar nada.
- Lint de `AGENTS.md` › Comandos en cada archivo tocado; E20 en verde con `manifiestos/0.16.0.md`.
- «beta» en `README.md`, `INSTALAR.md`, el paso de `/sdd-config` y el changelog 0.16.0.
