# 04 — Tareas: Pausa, origen del kit y contexto bajo demanda
Leyenda: `[ ]` Pendiente · `[~]` En curso · `[x]` Hecho · `[!]` Bloqueado

Reglas comunes a todos los pasos:
- «Chequeos de Comandos» = `AGENTS.md` › Comandos › Lint / sintaxis en cada archivo tocado: `file -i`
  da utf-8, sin BOM, `name` del frontmatter = nombre de su carpeta (si lo lleva), rutas citadas
  existentes y, en `template/`, sin marcadores `<<…>>` salvo `<<NOMBRE_PROYECTO>>` e `<<IDIOMA>>`.
- Lo de `template/.agents/` se replica en `.agents/` de la raíz solo en F8.2.
- Una regla, una fuente (RNF-2): la fuente es la de `03_plan.md` › 1; lo demás remite.
- Ningún paso relaja la regla 1 de Guardrails ni la regla Git, salvo RF-8 (RNF-7).
- Verificación manual UI: no aplica en ningún paso (solo Markdown).
- Evidencia de datos: no aplica en ningún paso (sin base de datos).

## F0 — Línea base (estimado: 0.25 h)
- [x] **F0.1 Medidas y copias de partida.** RNF-1, RF-12
  - Hecho cuando:
    - [x] Bytes (`LC_ALL=C wc -c`) de `AGENTS.md` y `template/AGENTS.md`, y de sus secciones
      «Guardrails», regla 1, «Antecedentes», «Deuda técnica» y «Cada sesión», anotados en
      `07_casos_prueba.md` › Corridas (CP-01).
    - [x] Copias de `AGENTS.md` y `template/AGENTS.md` en `.agents/local/0017-base/` (no versionado,
      comprobado con `git check-ignore`).
    - [x] Las 10 piezas de la regla 1 de CP-02 localizadas en el texto actual (línea de cada una).
- [x] **F0.2 `/sdd-estructura` de partida.** RF-10, RF-18, RF-23, RF-30
  - Hecho cuando:
    - [x] Corrida en el clon y en un ensayo de instalación (`mktemp -d`), con el resumen de `FALLA` y
      `AVISO` de cada una guardado en `.agents/local/0017-base/estructura-*.md`.

## F1 — `AGENTS.md` y arranque (estimado: 1.5 h)
- [x] **F1.1 Crear `template/.agents/skills/sdd/antecedentes-y-deuda.md`.** RF-11
  - Hecho cuando:
    - [x] Lleva el cuerpo de «Antecedentes» (6 puntos) y de «Deuda técnica» (4 puntos) de
      `template/AGENTS.md`, con el mismo sentido y dos secciones con esos mismos nombres.
    - [x] Las rutas citadas existen en `template/`.
    - [x] Chequeos de Comandos OK.
- [x] **F1.2 `template/AGENTS.md`.** RF-8, RF-11, RF-12, RF-17, RF-21
  - Hecho cuando:
    - [x] «Antecedentes» y «Deuda técnica» conservan su encabezado con la condición de cuándo se leen y
      la remisión a `antecedentes-y-deuda.md`.
    - [x] Regla 1 de Guardrails ≤ 1 900 bytes, con las 10 piezas de CP-02 señalables.
    - [x] Paso 8 de «Cada sesión» revisa prefijos repetidos solo en las tablas de índice de `002_specs`,
      `003_deuda`, `004_adr` y, si existe, `005_contexto`; repetido = dos filas del mismo índice con el
      mismo entero; no lista carpetas.
    - [x] «Git» lleva una línea: `/sdd-version` es la única excepción de red (`git ls-remote` y clon
      temporal); `push`/`pull`/`fetch`/`sync` siguen prohibidos.
    - [x] Una línea remite al índice `.docs/005_contexto/README.md` (contexto bajo demanda).
    - [x] `LC_ALL=C wc -c template/AGENTS.md` ≤ 9 800. Chequeos de Comandos OK.
- [x] **F1.3 `AGENTS.md` de la raíz en espejo.** RF-8, RF-11, RF-12, RF-17, RF-21
  - Hecho cuando:
    - [x] Los mismos cambios de F1.2; la regla 1 idéntica byte a byte a la de `template/AGENTS.md`
      (`diff` de la sección vacío).
    - [x] Las secciones propias de la raíz (paso 0, reglas de mantenimiento) intactas.
    - [x] `LC_ALL=C wc -c AGENTS.md` ≤ 11 000. Chequeos de Comandos OK.
- [x] **F1.4 Cambio pequeño en `template/.agents/skills/sdd/topes-y-modos.md`.** RF-13
  - Hecho cuando:
    - [x] «Modo ligero» define cambio pequeño (bug acotado, error tipográfico o de tipos, ≤ 3 archivos,
      no añade regla): siempre modo ligero, con carpeta y con «Antecedentes»; con duda, no es pequeño.
    - [x] Chequeos de Comandos OK.
- [x] **F1.5 «Chequeo de arranque» de `template/.agents/sync-agents.md`.** RF-16
  - Hecho cuando:
    - [x] En una carpeta de agentes por archivo solo comprueba que cada rol tenga su entrada; la
      validez del adaptador queda a `/sdd-enlaces` y E3/E23.
    - [x] «Qué lee» declara esa lectura reducida y no depende de secciones que no lee.
    - [x] Chequeos de Comandos OK.

## F2 — Pausa y retomo (estimado: 1 h)
- [x] **F2.1 Marca de pausa en `template/.agents/skills/sdd/avance-y-tiempos.md`.** RF-3, RF-5
  - Hecho cuando:
    - [x] PUNTO DE RETOMO tiene la fila opcional «Pausa» (hora de inicio, motivo, lista «al retomar»),
      que se actualiza sin duplicarse y se borra al retomar.
    - [x] «Hueco largo» se mide desde la hora de la marca si existe. La pausa no suma tiempo.
    - [x] Chequeos de Comandos OK.
- [x] **F2.2 Fila «Pausa» en la plantilla `05_avance.md` de `template/.agents/skills/sdd/plantillas.md`.** RF-3
  - Hecho cuando:
    - [x] La fila existe y remite a `avance-y-tiempos.md` sin copiar su regla.
    - [x] Chequeos de Comandos OK.
- [x] **F2.3 Crear `template/.agents/commands/sdd-pausa.md`.** RF-1, RF-2, RF-3, RF-4
  - Hecho cuando:
    - [x] Frontmatter `description` y `argument-hint` como los demás comandos.
    - [x] Cubre: hora y carpeta activa (pregunta si no la hay); inventario de trabajo en segundo plano
      pedido a la herramienta (o «inventario sin dato de la herramienta»), sin detener nada sin
      confirmación; marca y lista «al retomar» en el PUNTO DE RETOMO; decisión con hora; resumen de
      `git status` sin commit; `.agents/local/retomo-sesion.md` solo si la persona lo pide.
    - [x] Solo escribe `05_avance.md` y ese archivo local. Casos límite de la spec cubiertos.
    - [x] Chequeos de Comandos OK.
- [x] **F2.4 Lista «al retomar» en `template/.agents/commands/sdd-status.md`.** RF-5
  - Hecho cuando:
    - [x] Ejecuta solo la lista cerrada (`git status`, existencia de un archivo o recibo, estado de un
      trabajo consultado a la herramienta); lo demás lo muestra y no lo ejecuta; informa qué cambió.
    - [x] Sigue sin modificar archivos. Chequeos de Comandos OK.

## F3 — Origen y versión (estimado: 1 h)
- [x] **F3.1 Crear `template/.agents/commands/sdd-version.md`.** RF-7, RF-8
  - Hecho cuando:
    - [x] Procedimiento de 4 pasos de RF-7 (origen, `git ls-remote`, clon temporal `--depth 1` en
      `mktemp -d` o clon local si su `HEAD` coincide, tres respuestas); `GIT_TERMINAL_PROMPT=0`; si
      pide credenciales, para; borra el temporal; en el repo del kit no aplica.
    - [x] Remite a `ACTUALIZAR.md` para actualizar y no aplica nada. Chequeos de Comandos OK.
- [x] **F3.2 `.agents/.kit-origen` en `INSTALAR.md`.** RF-6
  - Hecho cuando:
    - [x] El paso que escribe `.kit-version` escribe también `.kit-origen` (`url` del `origin` del
      clon y `rama` actual; sin remoto, se pide la URL); `.agents/local/kit-origen` opcional.
    - [x] Chequeos de Comandos OK.
- [x] **F3.3 `.kit-origen` y tres commits en `ACTUALIZAR.md`.** RF-6, RF-9
  - Hecho cuando:
    - [x] Compara o crea `.kit-origen` y propone el cambio con visto bueno.
    - [x] «Commit propuesto» propone los tres grupos de RF-9, cada uno con confirmación y
      `git-commit`; grupo vacío no se propone; nunca `git add .` ni `-A`.
    - [x] Remite a `/sdd-version` para consultar. Chequeos de Comandos OK.

## F4 — Contexto del proyecto (estimado: 0.75 h)
- [x] **F4.1 Crear `template/.docs/005_contexto/README.md` y `.docs/005_contexto/README.md`.** RF-20
  - Hecho cuando:
    - [x] Índice vacío con columnas archivo, qué responde, cuándo leerlo y última verificación; guía
      (numeración, «Objetivo» y «Última verificación», tipos abiertos, framework → skill, histórico →
      `.docs/legacy/`). Genérico (RNF-4). Iguales en las dos capas.
    - [x] Chequeos de Comandos OK.
- [x] **F4.2 «Numeración única» incluye `005_contexto` en `template/.docs/002_specs/README.md` y `.docs/002_specs/README.md`.** RF-17, RF-20
  - Hecho cuando:
    - [x] La sección nombra `005_contexto` junto a specs, deuda y ADR, igual en las dos capas.
    - [x] Chequeos de Comandos OK.
- [x] **F4.3 Propuesta de contexto en `template/.agents/commands/sdd-init.md`.** RF-21
  - Hecho cuando:
    - [x] La propuesta lista documentos de contexto (nombre, qué responde, cuándo leerlo) como las
      skills candidatas; se crean después uno por uno con visto bueno.
    - [x] Chequeos de Comandos OK.
- [x] **F4.4 Propuesta de contexto en `template/.agents/commands/sdd-cierre.md`.** RF-22
  - Hecho cuando:
    - [x] Si el requerimiento tocó algo descrito por un documento del índice, propone actualizarlo y
      su fecha, sin escribir sin visto bueno; sin `005_contexto/`, nada.
    - [x] Chequeos de Comandos OK.

## F5 — Stack y permisos (estimado: 1 h)
- [x] **F5.1 Crear `template/.agents/catalogo-skills.md`.** RF-24, RF-25
  - Hecho cuando:
    - [x] Tabla de señales (señal, tipo de skill, motivo), procedimiento de detección (sin lista fija
      de lenguajes, sin abrir `.env` ni credenciales) y de cruce con `.agents/skills/` (estados: ya
      instalada, sugerida, candidata nueva, ya no aplica; cómo crearla); revisión de una skill
      externa (sin scripts ni secretos, `seguridad-basica`). Sin fuentes externas ni proyectos.
    - [x] Chequeos de Comandos OK.
- [x] **F5.2 Stack, formato, BD de solo lectura y allow en `template/.agents/commands/sdd-init.md`.** RF-14, RF-15, RF-24, RF-26
  - Hecho cuando:
    - [x] RF-14: formato y análisis acotados a archivos tocados o solo verificar; un valor sin
      confirmar dice quién y cuándo.
    - [x] RF-15: pregunta por un usuario de BD de solo lectura y deja constancia en «Comandos».
    - [x] RF-24: el paso 5 detecta el stack con el catálogo y remite a él.
    - [x] RF-26: el paso 7 sustituye «el allow queda genérico…» por la regla nueva (comandos de
      verificación de solo lectura del proyecto; si no hay, genérico).
    - [x] Chequeos de Comandos OK.
- [x] **F5.3 `deny` y allow en `template/.agents/skills/sdd/restricciones-de-rol.md`.** RF-15, RF-26
  - Hecho cuando:
    - [x] El fragmento `permissions.deny` suma `mysql`, `psql`, `sqlcmd`, `sqlplus`, `sqlite3`,
      `mongosh` y `redis-cli`, como sugerencia.
    - [x] El allow remite a `sdd-init.md` (RF-26) sin copiar la regla. Chequeos de Comandos OK.
- [x] **F5.4 Recomendación de skills al final de `ACTUALIZAR.md`.** RF-24
  - Hecho cuando:
    - [x] Al terminar la actualización ofrece la detección del catálogo, sin instalar nada.
    - [x] Chequeos de Comandos OK.

## F6 — Estatus y pulido (estimado: 0.75 h)
- [x] **F6.1 «Quién escribe» en `template/.docs/002_specs/README.md` y `.docs/002_specs/README.md`.** RF-28, RF-29
  - Hecho cuando:
    - [x] La sesión principal escribe `Planificado`, `En curso` y `En pruebas` al registrar el cierre
      de `plan`, del primer paso y del último paso de implementación; incluye la reapertura de un
      `Descartado` por el cierre. Igual en las dos capas.
    - [x] Chequeos de Comandos OK.
- [x] **F6.2 Remisión en `template/.agents/commands/sdd-plan.md` y `sdd-implement.md`.** RF-28
  - Hecho cuando:
    - [x] Una línea en cada uno remite a «Quién escribe», sin copiar la regla.
    - [x] Chequeos de Comandos OK.
- [x] **F6.3 Pulido de la deuda 0013 en `template/.agents/commands/sdd-cierre.md`.** RF-29
  - Hecho cuando:
    - [x] Los tramos duplicados remiten a la leyenda y al README de deuda, sin perder lo que exigen
      RF-5 y RF-12 de la 0012; la frase del punto 3 reescrita.
    - [x] Chequeos de Comandos OK.

## F7 — Estructura y ruteo (estimado: 1 h)
- [x] **F7.1 `template/.agents/validar-estructura.md`.** RF-10, RF-19, RF-23, RF-27, RF-29, RF-30
  - Hecho cuando:
    - [x] E10 exenta `/sdd-constitucion`, `/sdd-init` y `/sdd-config`; E13 remite a la leyenda para el
      desfase; E24 (marcador en la raíz del kit), E25 (`.kit-origen`, aviso, sin red) y E26
      (`005_contexto`, aviso, 180 días) nuevos; sección «Bandera de estructura» (RF-18, RF-19).
    - [x] Chequeos de Comandos OK.
- [x] **F7.2 `template/.agents/commands/sdd-estructura.md`.** RF-18, RF-19
  - Hecho cuando:
    - [x] Descripción «lee, reporta y deja la bandera local»; paso final que escribe la bandera tras
      `git check-ignore` y remite a «Bandera de estructura».
    - [x] Las remisiones que dicen «solo lee y reporta» actualizadas (búsqueda con `grep`).
    - [x] Chequeos de Comandos OK.
- [x] **F7.3 Ruteo en `template/.agents/agents/coordinator.md`.** RF-1, RF-7, RF-13, RF-27
  - Hecho cuando:
    - [x] Filas de `/sdd-pausa`, `/sdd-version` y `/sdd-constitucion`; las líneas de «fix chico»
      remiten al cambio pequeño del modo ligero y no ofrecen camino fuera del flujo.
    - [x] Chequeos de Comandos OK.
- [x] **F7.4 Tabla de fases de `template/.agents/skills/sdd/SKILL.md`.** RF-1, RF-7
  - Hecho cuando:
    - [x] Filas «Pausa» (`/sdd-pausa`) y «Versión del kit» (`/sdd-version`), sesión principal.
    - [x] Chequeos de Comandos OK.
- [x] **F7.5 `template/.agents/README.md`.** RF-1, RF-7, RF-20
  - Hecho cuando:
    - [x] Árbol con `catalogo-skills.md`, `antecedentes-y-deuda.md`, `005_contexto`, `.kit-origen`;
      «Roles y comandos» con `/sdd-pausa` y `/sdd-version`.
    - [x] Chequeos de Comandos OK.
- [x] **F7.6 Lista de comandos de `README.md` de la raíz.** RF-1, RF-7
  - Hecho cuando:
    - [x] `/sdd-pausa` y `/sdd-version` en la lista. Chequeos de Comandos OK.

## F8 — ADR, réplica y versión (estimado: 1 h)
- [x] **F8.1 ADR 0033, 0034 y 0035 con su fila de índice.** RNF-6
  - Hecho cuando:
    - [x] Skill `adr`: 0033 (salida de «Antecedentes» y «Deuda técnica», D1), 0034 (excepción de red,
      RF-8), 0035 (sección `005_contexto`), con alternativas y consecuencias; filas en
      `.docs/004_adr/README.md`.
    - [x] Chequeos de Comandos OK.
- [x] **F8.2 Réplica en `.agents/` de la raíz.** RNF-8
  - Hecho cuando:
    - [x] `cp` de cada archivo tocado o nuevo de `template/.agents/`.
    - [x] `diff -rq template/.agents .agents` solo con las diferencias aceptadas en `MEMORY.md`.
- [x] **F8.3 Versión 0.13.0.** RF-31
  - Hecho cuando:
    - [x] `VersionAgentKitSdd` = 0.13.0; entrada en `ChangelogAgentKitSdd.md`; `ACTUALIZAR.md`
      documenta lo que el Grupo B (`AGENTS.md`, `.gitignore`) y el Grupo C (`005_contexto`, «Quién
      escribe», «Numeración única») incorporan a mano.
    - [x] Chequeos de Comandos OK.
- [x] **F8.4 `manifiestos/0.13.0.md`.** RF-31, RNF-5
  - Hecho cuando:
    - [x] Generado al final, con las huellas de `template/` actuales (incluye los archivos nuevos);
      cada huella coincide.
    - [x] Chequeos de Comandos OK.

## F9 — Casos de prueba (estimado: 1 h)
- [x] **F9.1 Corrida de `07_casos_prueba.md`.** Todos los RF
  - Hecho cuando:
    - [x] Cada caso con corrida `obtenido`, `manual`, `ok` o `falla` anotada en `07_casos_prueba.md`.
    - [x] Ensayo de instalación sin pisar nada; `/sdd-estructura` en el clon y en el ensayo sin
      `FALLA` nueva frente a F0.2; RNF-1 medido.
