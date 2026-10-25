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

Si `AGENTS.md` ya no contiene `SDD-INIT-PENDIENTE`, el proyecto ya está personalizado: dímelo y
pregunta si quiero revisar o actualizar algo en concreto.

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
ejecuta lint/pruebas/build, cómo se consulta la base de datos en solo lectura (o si no aplica),
reglas de negocio que no se pueden romper, qué herramientas de IA usa el equipo.

## 3. Propuesta (NO escribir todavía)

Preséntame, por separado y con su evidencia (archivo donde lo verificaste; lo que no, márcalo
`(por confirmar)`):

1. **`AGENTS.md`**: valores para `<<NOMBRE_PROYECTO>>`, `<<IDIOMA>>` y las secciones "Stack y
   convenciones de código", "Comandos" y "Reglas de negocio y del sistema".
2. **Constitución** (`.docs/001_constitucion/constitution.md`): 6 principios innegociables, cortos
   y verificables (sí/no), máximo 15 líneas, que cubran stack, el plan manda (flujo `sdd`), capas,
   verificación, datos y seguridad, y codificación e idioma. Indica qué partes del código ya los
   incumplen.
3. **`.agents/napkin.md`**: las 3 a 8 trampas o comandos de entorno que ya detectaste.
4. **`MEMORY.md`**: decisiones duraderas que ya se ven en el repo, si las hay.
5. **Skills de dominio candidatas** (nombre, qué cubriría y disparadores). Solo la lista; se crean
   después una por una con las reglas de `.agents/README.md`.
6. **Ajustes a agentes o comandos** si el proyecto necesita algo distinto (por ejemplo, comandos de
   verificación específicos). Solo si es necesario.

## 4. Aplicar (tras mi aprobación)

1. Escribe lo aprobado. Para cada archivo editado, respeta el encoding y las herramientas de edición
   permitidas que acabamos de documentar.
2. Quita el marcador `SDD-INIT-PENDIENTE` de `AGENTS.md`.
3. Verifica los enlaces con `.agents/sync-agents.md` y sugiere crear los que falten (sin ejecutar
   nada sin mi confirmación).
4. Dime el siguiente paso: `/sdd <mi primer requerimiento>`.
5. Si te lo pido, prepara el commit con `.agents/skills/git-commit/SKILL.md`. Nunca hagas
   `git commit` sin mi confirmación explícita.
