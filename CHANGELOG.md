# Changelog

## 0.5.0 — 2026-10-24
- Nueva skill `adr` y carpeta `.docs/004_adr/` (índice): decisiones de arquitectura con contexto,
  alternativas descartadas y consecuencias. `/sdd-plan` y `/sdd-cierre` proponen el ADR (sin crearlo
  sin visto bueno).
- Nueva skill `migracion-bd`: cómo se escriben los scripts `sql/` (orden, encabezado, idempotencia,
  rollback). El reviewer los revisa con ella; `sdd` y `/sdd-plan` la referencian.
- Nueva skill `seguridad-basica`: búsqueda de secretos antes del commit y checklist OWASP ligero.
  `git-commit` (fase 2) y el reviewer (`validate`) la invocan.
- `AGENTS.md`: 3 filas nuevas en la tabla de skills.
- Nuevo comando `/sdd-estructura` y `.agents/validar-estructura.md`: el agente valida la estructura del kit
  (enlaces, réplicas, skills, agentes, comandos, índices, codificación) y propone correcciones sin aplicarlas.
- `sync-agents.md` paso 8: al agregar una skill, agente o comando, sugiere replicarlo en las carpetas de cada
  herramienta (o excluirlo si es una prueba local). `.agents/README.md` lo referencia al agregar cada tipo.
- `INSTALAR.md`: elige modo al empezar. **Modo proyecto** instala solo `template/`; **Modo mantenimiento**
  (clon del kit) conserva el conocimiento propio versionado (specs, ADR, memoria, reglas) y pregunta antes de copiar.
- Actualización de un proyecto que ya tiene el kit: `ACTUALIZAR.md`, en lugar de reinstalar encima.
- Manifiestos de huellas por versión en `manifiestos/` (fuera de `template/`, no se distribuyen).
- Marcador de versión `.agents/.kit-version`, escrito al instalar en modo proyecto.
- Chequeo E20, solo en el repo del kit: las huellas de `template/` coinciden con el manifiesto de `VERSION`.
- Numeración en un proyecto: el consecutivo de specs, deudas y ADR pasa a 4 dígitos (`0001`). Las carpetas de
  sección de `.docs/` y el consecutivo del clon del kit siguen en 3. Lo aplican los comandos `/sdd-*`, las skills
  `adr`, `git-commit`, `migracion-bd` y `sdd`, y el chequeo E12. Un proyecto que ya tenga carpetas de 3 dígitos
  verá la regla nueva al actualizar; esa regla de convivencia no está definida todavía.
- Menos lectura por rol y por comando, y una sola fuente por fase del flujo `sdd`. El cambio de
  redacción en `AGENTS.md` llega a un proyecto por fusión manual.
- La skill `sdd` manda que el chat lance al coordinator y no ejecute las fases.
- Si una fase no cabe en un solo paso, sigue en esa misma entrada y deja constancia en el avance. Un pedido de hacerla en el chat se atiende en la sesión.
- Modelo explícito por rol; `omitClaudeMd` en planner, reviewer y coordinator; se quitan `mode` y
  `permissions`; el reviewer ya no tiene `Write` y la evidencia formal la escribe la sesión;
  `restricciones-de-rol.md` es NUEVO (grupo A) y se incorpora junto con los agentes; «Qué lee cada rol»
  llega por fusión manual. Quien personalizó un agente: (1) quitar `mode`/`permissions` (se ignoran);
  (2) el reviewer ya no tiene `Write`; (3) sin `model` el agente hereda el de la sesión; (4) sin
  `omitClaudeMd` el agente sigue cargando `AGENTS.md` completo. Hay que actualizar `reviewer.md` y
  `sdd-validate.md` a la vez: si el reviewer es el anterior y el comando el nuevo, la sesión escribe la
  evidencia y solo se pierde la garantía de que el reviewer no escriba; al revés, el procedimiento queda
  desfasado porque el reviewer devuelve el contenido y la sesión lo escribe.

## 0.4.0 — 2026-10-20
- Se eliminan `install.ps1` e `install.sh`: el kit es genérico. Nuevo `INSTALAR.md` con instrucciones para que el
  agente detecte SO/shell y genere un script o dé el paso a paso (sin sobrescribir).
- Nuevo `PREFERENCIAS.md` en la raíz del proyecto (en `.gitignore`): directivas personales por usuario/entorno.
  `AGENTS.md`: paso 10 de "Cada sesión". Plantilla en `.agents/preferencias.ejemplo.md` (ejemplo: Zed y
  "limpiar contexto"). El napkin aclara que sus User Directives son del equipo.

## 0.3.0 — 2026-10-09
- Nueva skill `formato-respuesta`: línea de cierre "quién responde" (rol con cuadro de color, skill, contexto,
  desvíos) en cada respuesta. `AGENTS.md`: paso 9 de "Cada sesión" y fila en la tabla de skills.

## 0.2.0 — 2026-10-07
- `sync-agents.md`: nueva fila `.github/copilot-instructions.md` → `AGENTS.md`; paso 7 que detecta archivos
  versionados que el `.gitignore` ya ignora y propone `git rm --cached`; notas sobre copias heredadas.
- `skills/git-commit`: la fase 2 revisa `git ls-files -ci --exclude-standard`.
- `.agents/README.md`: fila de GitHub Copilot.

## 0.1.0 — 2026-10-07
- Primera versión genérica, extraída del flujo SDD de COMPRE_sistema.
- Incluye: `AGENTS.md` como fuente de verdad, `.agents/` (4 agentes, 13 comandos `/sdd*` con `/sdd-init`,
  skills `sdd`, `napkin` y `git-commit`), `.docs/` (constitución, índice de planes, deuda técnica)
  y `sync-agents.md` (enlaces: solo documento, el entorno decide cómo crearlos).
- Incluye `.gitignore` (archivos locales de agentes y carpetas de enlaces por herramienta).
- No incluye skills de dominio, `plan-implementacion` (legado), `pruebas-implementacion` ni `deploy-project`.
