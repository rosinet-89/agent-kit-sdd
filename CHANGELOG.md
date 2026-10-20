# Changelog

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
