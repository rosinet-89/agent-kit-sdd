---
name: git-commit
description: >
  Protocolo estricto para preparar commits de Git en este repositorio. Usar esta skill
  siempre que el agente deba registrar cambios en Git. Prepara el stage, valida con los
  comandos de verificación del proyecto y construye el mensaje, pero NUNCA ejecuta el
  commit sin la confirmación explícita del usuario.
---

# Skill: Git Commits

## Reglas absolutas

| # | Regla |
|---|---|
| 1 | **PROHIBIDO** ejecutar `git commit` automáticamente. Siempre pedir confirmación. |
| 2 | **NUNCA** `git push`, `git sync`, `git fetch` ni `git pull`. Solo commits locales. |
| 3 | **NUNCA** un único commit con todos los archivos mezclados. |
| 4 | Un commit por módulo o agrupación lógica. |
| 5 | Mensajes en el idioma definido en `AGENTS.md` › Git, cortos (máx. 72 caracteres). |
| 6 | `git add <archivos>` explícito; nunca `git add .` ni `git add -A`. |
| 7 | Atribución (`Co-Authored-By` u otras líneas): solo si `AGENTS.md` o `MEMORY.md` lo indican. Por defecto el mensaje es solo el subject. |

---

## Fase 1 — Detección del contexto

```bash
git status --short
git diff --stat HEAD
```

Clasificar cada archivo en grupos lógicos (módulo, capa, docs, config).

---

## Fase 2 — Verificación previa (pre-commit gate)

1. **Lint / sintaxis**: los comandos de `AGENTS.md` › Comandos sobre cada archivo incluido.
2. **Residuos**: sin archivos temporales, credenciales ni código de depuración. La búsqueda de
   secretos sobre lo que entra al commit sigue `.agents/skills/seguridad-basica/SKILL.md` (sección 1);
   si hay un hallazgo con valor real, detener el commit.
3. **Versionados e ignorados**: `git ls-files -ci --exclude-standard`. Si lista algo (archivos que
   Git ya rastrea y el `.gitignore` ignora), no incluirlos en el commit; proponer al usuario un
   commit aparte con `git rm --cached <rutas>` (se conservan en disco). No ejecutarlo sin confirmación.
4. **Numeración única**: si el commit incluye carpetas de `.docs/002_specs/` o archivos de
   `.docs/003_deuda/` o `.docs/004_adr/`, revisar que su prefijo no exista ya con otro nombre en la copia
   local ni en la rama remota descargada (`git ls-tree --name-only @{u} <carpeta>`, sin fetch).
   El ancho es el de "Numeración única": 4 dígitos en un proyecto (`0001`) para specs, deudas y ADR;
   3 dígitos en el clon del kit. Las carpetas de sección de `.docs/` siguen en 3.
   Si choca, detener el commit, avisar y proponer el siguiente libre (regla completa en
   `.docs/002_specs/README.md` › Numeración única). No renombrar sin confirmación.

---

## Fase 3 — Construcción del mensaje

Formato: `<tipo>(<módulo>): <descripción en imperativo, máx 72 chars>`

Tipos: `feat`, `fix`, `refactor`, `docs`, `chore`, `style`, `test`.

Una sola línea. Sin cuerpo, salvo que el usuario pida lo contrario.

Escribir el subject en un archivo temporal (ej. `_git_msg.txt`) y commitear con:

```bash
git commit --cleanup=verbatim -F _git_msg.txt
```

---

## Fase 4 — Preparación del commit

1. **Staging**: `git add ruta/archivo`.
2. **Verificación**: `git status --short`.
3. **SOLICITAR CONFIRMACIÓN**: mensaje propuesto y archivos incluidos.
4. Tras el `git commit`, verificar con `git log -1 --format=%B` que el mensaje es el esperado
   (algunos IDEs agregan trailers por su cuenta). Si no, enmendar solo si HEAD lo acabas de crear
   tú y **no** se hizo push. Borrar `_git_msg.txt` al terminar.

---

## Deny list

- `git commit` sin permiso explícito del usuario.
- `git push`, `git sync`, `git pull`, `git fetch`.
- Commits con archivos que no pasan los comandos de verificación.
- `git add .` o `git add -A`.

## Checklist de salida

- [ ] El usuario confirmó la acción.
- [ ] Un commit por módulo.
- [ ] El mensaje sigue el estándar (idioma del proyecto, 72 chars, tipo(módulo)).
- [ ] **NO SE HIZO PUSH**.
