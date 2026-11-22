---
description: SDD · Instrucción inicial - documenta los pormenores del proyecto y personaliza el kit (se corre una sola vez)
argument-hint: <contexto adicional opcional>
---
Personalización inicial del kit SDD en este proyecto. Usa la skill `sdd`. NO escribas código ni
archivos hasta que yo apruebe la propuesta (paso 3).

Qué lee: el reconocimiento de la sección 1, con tope de 30 archivos o 150 KB acumulados, lo que
ocurra primero. Para pasar ese tope, pide confirmación.

Qué hace: entrevista, propone y, tras la aprobación, personaliza el proyecto.

Qué no hace: no escribe nada antes de esa aprobación. No tiene meta de reducción de lectura.

Si, según el paso 1 de «Cada sesión» de `AGENTS.md`, la instalación ya no está pendiente, el proyecto ya está personalizado: dímelo y
pregunta si quiero revisar o actualizar algo en concreto. También entonces, lee y muestra enteras
las secciones «Fragmento opcional permissions.deny» y «Fragmento opcional permissions.allow» de
`.agents/skills/sdd/restricciones-de-rol.md`, tal cual. El allow se llena según la sección 3,
punto 7. No los apliques, no los actives y no los copies a `.claude/`, a un settings versionado
ni a ninguna configuración del repo.

Contexto adicional: $ARGUMENTS

## 1. Reconocimiento (solo lectura)

Tope: 30 archivos o 150 KB acumulados, lo que ocurra primero. Si hace falta más, pide confirmación
antes de seguir. No amplíes el tope por tu cuenta.

Lee, sin modificar nada: `README*`, manifiestos de dependencias (`package.json`, `composer.json`,
`pom.xml`, `requirements.txt`, `go.mod`, `*.csproj`, etc.), configuración de Docker/CI, la
estructura de carpetas, `git log` reciente, documentación existente y muestras representativas de
código (capas, nombres, manejo de sesión/errores, acceso a datos).

## 2. Entrevista

Con lo que no puedas deducir, pregúntame de UNA en UNA (máximo 6), con tu mejor suposición como
opción por defecto. Temas posibles: nombre del proyecto, idioma de código/UI/docs/commits, cómo se
ejecuta lint/pruebas/build, cómo se consulta la base de datos (ver Guardrails; o si no aplica) y si
existe un usuario de BD de solo lectura (solo si sí o no; nunca pidas ni anotes credenciales),
reglas de negocio que no se pueden romper, qué herramientas de IA usa el equipo.

## 3. Propuesta (NO escribir todavía)

Preséntame, por separado y con su evidencia (archivo donde lo verificaste; lo que no, márcalo
`(por confirmar)`):

1. **`AGENTS.md`**: valores para `<<NOMBRE_PROYECTO>>`, `<<IDIOMA>>` y las secciones "Stack y
   convenciones de código", "Comandos" y "Reglas de negocio y del sistema". En "Comandos" y "Stack":
   formato y análisis se acotan a los archivos tocados o quedan en solo verificar, sin reformatear
   legados enteros. En "Comandos" › consulta de datos, anota si existe usuario de BD de solo lectura.
   `(por confirmar)` es solo de la propuesta: lo que se escriba dice quién lo confirma y cuándo.
2. **Constitución** (`.docs/001_constitucion/constitution.md`): 6 principios innegociables, cortos
   y verificables (sí/no), máximo 15 líneas, que cubran stack, el plan manda (flujo `sdd`), capas,
   verificación, datos y seguridad, y codificación e idioma. Indica qué partes del código ya los
   incumplen.
3. **`.agents/napkin.md`**: las 3 a 8 trampas o comandos de entorno que ya detectaste.
4. **`MEMORY.md`**: decisiones duraderas que ya se ven en el repo, si las hay.
5. **Skills de dominio candidatas** (nombre, qué cubriría y disparadores). Detecta el stack con
   `.agents/catalogo-skills.md` (señales, cruce con `.agents/skills/`), sin copiar su procedimiento.
   Solo la lista; se crean después una por una con las reglas de `.agents/README.md`.
6. **Ajustes a agentes o comandos** si el proyecto necesita algo distinto (por ejemplo, comandos de
   verificación específicos). Solo si es necesario.
7. **Deny y allow, solo mostrar.** Lee y muestra enteras las secciones «Fragmento opcional
   permissions.deny» y «Fragmento opcional permissions.allow» de
   `.agents/skills/sdd/restricciones-de-rol.md`. Tal cual: no las reescribas ni pegues otra copia.
   El fragmento `permissions.allow` se puede llenar con los comandos de verificación de solo
   lectura de «Comandos» del proyecto (formato, análisis, pruebas que no escriben en una BD), sin
   inventar. No traigas comandos de este kit ni de otro proyecto. Sin verificación de solo lectura,
   queda genérico. Es texto que solo se muestra: no lo apliques, no lo actives y no lo copies a
   `.claude/`, a un settings versionado ni a ninguna configuración del repo.
8. **Documentos de contexto** (nombre, qué responde, cuándo leerlo) para `.docs/005_contexto/`
   (índice y guía en `.docs/005_contexto/README.md`). Solo la lista; se crean después, uno por uno
   y con visto bueno. No los generes.

## 4. Aplicar (tras mi aprobación)

1. Escribe lo aprobado. Para cada archivo editado, respeta el encoding y las herramientas de edición
   permitidas que acabamos de documentar.
2. Quita el marcador `SDD-INIT-PENDIENTE` de `AGENTS.md`.
3. No escribas `model:` en `.agents/agents/`. Ese frontmatter conserva los valores de
   `.agents/skills/sdd/restricciones-de-rol.md` › «Modelo por rol» (remite a ella y no la copies).
   El modelo de cada dev se elige en `/sdd-config` y lo aplica `/sdd-enlaces` con adaptadores de
   rol (`.agents/sync-agents.md`). Detección de herramientas, que `/sdd-config` reutiliza:
   - Lee la línea `herramientas:` de `.agents/local/kit-visto` (formato en
     `.agents/sync-agents.md` › «Estado «visto»»). Si el archivo no existe, usa las carpetas
     presentes: `.claude/`, `.cursor/`, `.gemini/`, `.github/` (solo cuenta con
     `copilot-instructions.md` o la carpeta `agents/`) y `.kiro/`.
4. Invoca el asistente `/sdd-config` (`.agents/commands/sdd-config.md`), fuera del tope de la
   entrevista de la sección 2 y tras aplicar lo aprobado.
5. Sugiere correr `/sdd-enlaces` para crear los enlaces de tus herramientas (sin ejecutar nada sin
   mi confirmación; no escribas el estado «visto»).
6. Dime el siguiente paso: `/sdd <mi primer requerimiento>`.
7. Si te lo pido, prepara el commit con `.agents/skills/git-commit/SKILL.md`. Nunca hagas
   `git commit` sin mi confirmación explícita.
8. No escribas el deny ni el allow. No los apliques, no los actives y no los copies a `.claude/`,
   a un settings versionado ni a ninguna configuración del repo.
