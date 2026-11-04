# 02 — Análisis de impacto: Enlaces tras un pull

## 1. Resumen del requerimiento
Tras un pull que cambia el kit, la sesión avisa una sola vez qué cambió y qué enlaces faltan (paso 7 de «Cada sesión», solo lectura). Un comando nuevo, `/sdd-enlaces`, es el único que escribe: crea, corrige o retira enlaces con confirmación, asume los pasos destructivos de `sync-agents.md` (7 y 8) y registra el estado «visto» local en `.agents/local/kit-visto`. Sin scripts (ADR 004).

### 1.1 Antecedentes (índice de planes)
Revisado `.docs/002_specs/README.md` y las carpetas con grep de «enlace», «sync-agents», «sdd-estructura».
- `001_ActualizarKit`: coincide en `ACTUALIZAR.md` (sección Enlaces y «Conflicto de ruta», que ya usan los estados de `sync-agents.md`). Se reutiliza: estados OK/FALTA/DIFERENTE/OCUPADO. No contradice; la sección Enlaces se ajusta para remitir a `/sdd-enlaces`.
- `002_Kit050`: coincide en `/sdd-estructura` y E1–E20 (`validar-estructura.md`; con 0010 pasa a E1–E21). Se reutiliza E1–E3; contradice, en parte, la frase «aplícala una por una» de `sdd-estructura.md` (RF-11), que se corrige.
- `0008_GuardrailsBdSoloLectura` y `0010_FalsoPositivoInitPendiente` (sesiones de otros agentes, sin tocar): comparten 0.6.0 y editan `AGENTS.md` y `template/AGENTS.md`. Orden de fusión: 0008, 0010, 0009.

### 1.2 Deuda técnica relacionada
`.docs/003_deuda/README.md`: deudas 001–008 resueltas; ninguna abierta del módulo. Este feature no resuelve ni deja deuda. Dos diferidos aceptados por el usuario (ver §6 y `03_plan.md` §6): detectar réplicas e ignorados versionados al arranque (hoy lo hacía el paso 7; pasa a `/sdd-enlaces` y E2/E3, ADR-B) y copias desactualizadas.

## 2. Módulos, menú, tablas y archivos involucrados
### 2.1 Menú / opciones / rutas de acceso
- Arranque de sesión: paso 7 de «Cada sesión» (`AGENTS.md`, `template/AGENTS.md`).
- Comando nuevo `/sdd-enlaces` (sesión principal) y `/sdd-estructura` (solo lectura).
- Documentación al mantenedor y al equipo: `ACTUALIZAR.md` (raíz, solo del kit) y el aviso del paso 7 (que viaja al proyecto).

### 2.2 Tablas (existentes y nuevas)
No aplica: no hay base de datos. «Tabla» de enlaces = la de `sync-agents.md` (13 filas; no cambia). Estado nuevo: `.agents/local/kit-visto` (texto plano local).

### 2.3 Archivos de código (ruta, capa)
Verificado el 2026-10-31 con `ls`/`diff -rq`: `template/.agents` y `.agents` coinciden salvo `napkin.md` (esperado) y `.agents/skills/caveman` (solo en el clon, preexistente, no es del kit). El clon `.agents/` es local (excluido en `.git/info/exclude`; solo `napkin.md` se versiona).

| Ruta | Capa | Cambio |
|---|---|---|
| `template/.agents/commands/sdd-enlaces.md` y `.agents/commands/sdd-enlaces.md` | Producto + réplica | Nuevo |
| `template/.agents/sync-agents.md` y réplica | Producto + réplica | Pasos 3–8 pasan a `/sdd-enlaces`; formato de «visto»; chequeo de arranque (se omite si falta `.agents/.kit-version` y existe `template/`) |
| `template/.agents/validar-estructura.md` y réplica | Producto + réplica | E1 (sobre herramientas declaradas; no declaradas = AVISO o fuera de E1; excepciones/copias como AVISO), E19 (ignora `local/`), «Cómo responder» 4; «E1 a E20» -> «E1 a E21» si 0010 está fusionada |
| `template/.agents/commands/sdd-estructura.md` y réplica | Producto + réplica | Quita «aplícala una por una» |
| `template/AGENTS.md` y `AGENTS.md` | Producto + entorno versionado | Paso 7; «Roles y comandos» |
| `template/.agents/README.md` y réplica | Producto + réplica | Árbol (`local/`), «Agregar…» y «Validar»; citas a «paso 8» en líneas 59, 66 y 75 (verificadas) pasan a `/sdd-enlaces` |
| `template/.agents/skills/sdd/SKILL.md`, `agents/coordinator.md`, `commands/sdd.md` y réplicas | Producto + réplica | Fila/lista de `/sdd-enlaces` (como `/sdd-estructura`); «E1 a E20» -> «E1 a E21» donde aparezca |
| `template/.agents/commands/sdd-init.md` y réplica; `INSTALAR.md` | Producto + réplica; manual raíz | Una frase: sugerir `/sdd-enlaces` |
| `ACTUALIZAR.md` | Manual raíz | Sección nueva, `## Enlaces`, `## Huérfanos` |
| `VersionAgentKitSdd`, `ChangelogAgentKitSdd.md`, `manifiestos/0.6.0.md` | Raíz | Solo el bump final |

`.gitignore` (raíz) y `template/.gitignore`: ya contienen `.agents/local/` (línea 4 en ambos). `git check-ignore -v .agents/local/kit-visto` lo confirma en el clon. No se modifican.

### 2.4 Código legado o sin uso (no tocar, sí documentar)
- Kiro en `sync-agents.md`: fuera de la tabla, no se toca.
- `.agents/skills/caveman` solo en el clon: ajeno al kit, no se toca.
- Rutas heredadas `.github/agents/*.agent.md`: las cubre la nota de `sync-agents.md` (OCUPADO).

## 3. Datos reales (SELECT de solo lectura, con fecha y BD)
No aplica. No hay BD (`AGENTS.md` › Comandos: «Consulta de datos de solo lectura: No aplica»). No se consultó nada.

## 4. Flujo actual
1. Paso 7 de «Cada sesión»: la sesión revisa los 13 enlaces y réplicas; si falta algo, sugiere crearlo. Es pasivo y sin memoria: no sabe si el kit cambió, ni qué herramientas usa la persona; avisa de todas las filas.
2. `sync-agents.md` pasos 3–8: sugerencia, mecanismo (symlink, junction, copia), `git rm --cached`, réplicas huérfanas/faltantes. Todo «propón» y ejecuta tras confirmar, dentro de la misma sesión de arranque.
3. `/sdd-estructura`: E1–E3 reportan y luego «aplícala una por una» (mezcla lectura y escritura).
4. `ACTUALIZAR.md` › Enlaces: «verifica y propón»; no dice qué hacer después de un pull.
5. Resultado: tras actualizar a 0.5.0 hizo falta un mensaje manual.

## 5. Reglas de negocio: conocidas vs código
| Regla | Conocida (spec/ADR) | En el código hoy |
|---|---|---|
| Solo Markdown, sin scripts | ADR 004, constitución §5 | Cumple |
| `OCUPADO` nunca se borra ni mueve | `sync-agents.md` paso 5 | Cumple (texto) |
| Nunca sobrescribe | Constitución §5, ADR 007 | Cumple; solo reemplaza/retira enlaces con confirmación |
| Cada regla en una sola fuente | Constitución §3, ADR 009 | `sync-agents.md` es la fuente de estados y pasos; hay referencias numéricas («paso 8») en README, E2/E3, `AGENTS.md` |
| `.agents/local/` ignorada | Spec RF-3 | Ya en ambos `.gitignore`; proyectos viejos con `.gitignore` personalizado pueden no tenerla |
| Arranque sin escritura | RF-11 | Hoy el paso 7 «sugiere» y los pasos destructivos se ejecutan en el arranque |
| Existencia de `.agents/.kit-version` | `ACTUALIZAR.md` › Marcador | En proyectos instalados sí; en el clon del kit no existe (ahí se omite el chequeo por la presencia de `template/`) |

## 6. Riesgos y casos borde
- Conflicto de edición en `AGENTS.md`/`template/AGENTS.md` con 0008 y 0010. Mitigación: orden 0008, 0010, 0009; releer antes de editar; Edit mínimo (solo paso 7 y «Roles y comandos»).
- Regresión de detección: al quitar de la sesión los antiguos pasos 7 y 8 de `sync-agents.md` (ignorados versionados, réplicas huérfanas/faltantes), el arranque deja de detectarlos. Aceptado por el usuario; se compensa con el disparador al agregar piezas, `/sdd-enlaces` y E2/E3. Se registra en ADR-B (cierre).
- Falta `.agents/.kit-version` (decisión del usuario, opción A): se aplica RF-1b literal (falta «visto» = todo nuevo; `kit-visto` siempre lleva `version: X.Y.Z`). Excepción única de arranque: si además existe `template/` (clon del kit, no un proyecto), el chequeo de arranque se omite y no hay aviso. No hay `version: ninguna` ni excepción a la spec.
- Colisión con 0010: añade el chequeo E21, así que «E1 a E20» pasa a «E1 a E21» en `validar-estructura.md`, `sdd-estructura.md` y réplicas, `sdd-init.md` (pasos 4.2/4.3), `skills/sdd/SKILL.md`, `commands/sdd.md` y `agents/coordinator.md`. 0009 debe releer esos archivos tras fusionar 0008 y 0010 y partir de una rama base que tenga ambas.
- Proyectos existentes y el paso 7: vive en `AGENTS.md` (no lo pisa una actualización del kit), así que esos proyectos lo incorporan a mano (se consigna en `ACTUALIZAR.md` y en el changelog).
- `diff -rq template/.agents .agents` (E19) mostraría `Sólo en .agents: local` tras correr `/sdd-enlaces` en el clon. Se acota E19 para ignorar `local/`.
- `ACTUALIZAR.md` › Huérfanos reportaría `.agents/local/kit-visto` (no está en ningún manifiesto). Se aclara que no es huérfano.
- Copia (`OK (copia)`) no se sincroniza: tras una pieza nueva queda desactualizada. Se avisa; no se resuelve (posible deuda).
- Persona que no usa ninguna herramienta de la tabla: el aviso se repite cada sesión (decisión 7, confirmada por el usuario: cero herramientas no es éxito).
- Dos sesiones en paralelo: gana la última escritura (spec); sin daño.
- Spec RF-1d solo nombra FALTA/DIFERENTE; el plan trata OCUPADO no registrado como pendiente (decisión 3). Es una **ampliación de RF-1d** a OCUPADO no registrado, coherente con RF-4 y RF-8. Aprobada por el usuario: se anota en la spec en el cierre (F6), sin `/sdd-change`; este plan no edita la spec.

## 7. Preguntas abiertas
Ninguna. Las decisiones pendientes quedaron resueltas por el usuario el 2026-11-01 (`03_plan.md` §8).

## Delta Cambio 1 (2026-11-03): E1, E7, ejemplo del aviso y git-commit
Alcance: RF-15 a RF-18 de `01_spec.md` › «Cambio 1» (aprobado 2026-11-03 21:28). Lo anterior de este documento no cambia. Verificado el 2026-11-03 con lectura de los archivos citados; sin BD (`AGENTS.md` › Comandos: «No aplica»).

### D.1 Qué falta de verdad (estado vigente leído)
- **E1** (`template/.agents/validar-estructura.md:18`) YA dice: se mide sobre las herramientas declaradas en `kit-visto`; las no declaradas, `excepcion:` y `copia:` son `AVISO`; cada fila `OK`, sin `OCUPADO` ni `DIFERENTE`. Lo que falta es solo el caso **sin «visto» legible**: el texto dice «sobre todas las filas» y exige `OK`, así que toda `FALTA` es FALLA. Además se contradice: sin «visto» ninguna herramienta está declarada, y por la regla general todas serían `AVISO`. Efecto real: en el clon del kit (sin «visto») las 7 filas FALTA de Gemini y GitHub dan FALLA (bloquea F5.2 y F6.9). Hay que fijar la severidad por estado.
- **E7** (línea 24) exige fila en «Skills de proyecto» para cada skill de `.agents/skills/`. Falla en el clon por `adr`, `migracion-bd`, `seguridad-basica` (están en `template/AGENTS.md`, líneas 98-100, y no en `AGENTS.md` raíz, tabla líneas 92-99) y por `caveman` (solo local, ajena al kit).
- **Ejemplo del aviso** (`sync-agents.md:116-120`): el paso 4 (líneas 108-111) manda los enlaces pendientes a la línea 2, pero el ejemplo los pone en la línea 1 («...faltan 2 enlaces de claude.») y la línea 2 solo recomienda.
- **git-commit** (`skills/git-commit/SKILL.md:43-45`, paso 3) repite `git ls-files -ci --exclude-standard` y la propuesta `git rm --cached`, que ya viven en `sync-agents.md` paso 7 (líneas 35-40): dos fuentes (constitución §3).

### D.2 Archivos del delta
| Ruta | Capa | Cambio |
|---|---|---|
| `template/.agents/validar-estructura.md` y `.agents/validar-estructura.md` | Producto + réplica | E1 (caso sin «visto»), E7 (exención en el repo del kit) |
| `AGENTS.md` (raíz) | Entorno versionado | 3 filas en «Skills de proyecto» (`adr`, `migracion-bd`, `seguridad-basica`), copiadas de `template/AGENTS.md:98-100`. `template/AGENTS.md` no cambia |
| `template/.agents/sync-agents.md` y `.agents/sync-agents.md` | Producto + réplica | Ejemplo del «Chequeo de arranque» y aclaración de la línea 2 |
| `template/.agents/skills/git-commit/SKILL.md` y `.agents/skills/git-commit/SKILL.md` | Producto + réplica | Paso 3 remite a `sync-agents.md` paso 7 |
| `manifiestos/0.6.0.md` | Raíz | Recalcular huellas de `.agents/validar-estructura.md`, `.agents/sync-agents.md`, `.agents/skills/git-commit/SKILL.md` (E20). `template/AGENTS.md` no cambia: su huella no varía |
| `ChangelogAgentKitSdd.md` | Raíz | Una línea en 0.6.0 |
Réplicas verificadas con `diff` el 2026-11-03: la de git-commit es idéntica hoy (spec RF-18).

### D.3 Reglas de negocio: conocidas vs código
| Regla | Conocida | En el código hoy |
|---|---|---|
| Sin «visto» ninguna herramienta está declarada (RF-1b, RF-5) | Spec, plan decisión 17 | E1 sin «visto» mide todas las filas con `FALTA` = FALLA: inconsistente |
| `OCUPADO`/`DIFERENTE` nunca se ocultan (RF-8) | Spec | Se conservan como FALLA sin `excepcion:` |
| Una regla, una fuente (constitución §3) | Constitución | git-commit repite la regla de `sync-agents.md` paso 7 |
| El aviso: enlaces pendientes en la línea 2 | `sync-agents.md` paso 4 | El ejemplo los pone en la línea 1 |

### D.4 Riesgos y casos borde del delta
- Sin «visto» en un proyecto instalado: `FALTA` pasa de FALLA a AVISO; es el comportamiento pedido (RF-16), el aviso remite a `/sdd-enlaces`. `DIFERENTE`/`OCUPADO` siguen como FALLA.
- «visto» corrupto cuenta como sin «visto» (RF-1b).
- Exención de E7 solo con `template/` presente; un clon con skill local del mismo nombre que una del kit exige fila (está en `template/`). No se relaja la fila que apunta a una skill inexistente.
- La exención de E7 mide `.agents/skills/` contra `template/.agents/skills/`: cualquier skill local nueva en el clon queda exenta sin lista fija.
- Huellas: el cambio de `validar-estructura.md`, `sync-agents.md` y `git-commit/SKILL.md` invalida E20 hasta recalcular; hacerlo tras la última edición.
- **Ampliación leve del «Fuera de alcance»** (decisión explícita, ver `03_plan.md` Delta D-2): RF-17 y RF-18 son coherencias de texto fuera de E1/E7; la spec ya los incluye en el Cambio 1, aprobado.
- Sin conflicto de edición esperado: 0008 y 0010 ya están fusionados en el árbol de trabajo; releer `AGENTS.md` antes de editar.

### D.5 Preguntas abiertas
Ninguna.
