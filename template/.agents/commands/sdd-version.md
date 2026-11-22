---
description: SDD · Dice si tienes la última versión del kit consultando su origen (solo lee, no actualiza)
argument-hint:
---

Consulta si el proyecto tiene la última versión del kit. No aplica nada: para actualizar, remite a `ACTUALIZAR.md`.

Qué lee: `.agents/.kit-version` (versión instalada), `.agents/.kit-origen` (`url` y `rama`, una por línea, formato `clave: valor`) y, si existe, `.agents/local/kit-origen` (ruta de un clon local del kit, no versionada). De la fuente remota, solo `VersionAgentKitSdd` y `ChangelogAgentKitSdd.md`.

Qué hace, en este orden:
1. Lee la versión instalada y el origen. Si falta el origen, lo dice, pide la `url` (y la `rama`) y propone guardarla en `.agents/.kit-origen`; no la escribe sin visto bueno.
2. Obtiene la punta remota: `GIT_TERMINAL_PROMPT=0 git ls-remote <url> <rama>`.
3. Lee la versión publicada y el changelog de un clon temporal: `tmp=$(mktemp -d)` y `GIT_TERMINAL_PROMPT=0 git clone --depth 1 --branch <rama> <url> "$tmp"`, siempre fuera del proyecto y del clon local. Al terminar corre `rm -rf "$tmp"`. Si el clon local de `.agents/local/kit-origen` tiene el `HEAD` igual a la punta remota, lee de él y no clona.
4. Responde una de tres cosas:
   - **Al día:** la versión instalada es la publicada.
   - **Versión nueva:** dice cuál y muestra las entradas del changelog entre ambas.
   - **Clon local atrasado:** su `HEAD` no coincide con la punta remota; lo dice y deja a la persona traerlo.

Casos límite:
- Sin red o la URL no responde: lo dice, no reintenta y no da una versión supuesta.
- La rama ya no existe en el remoto: lo informa y pide la rama.
- Sin `.agents/.kit-origen` (instalado antes de 0.13.0): paso 1; `ACTUALIZAR.md` también lo crea.
- Si el remoto pide credenciales, para y lo dice. No las pases a comandos, archivos ni al chat. Sugiere usar el clon local o que la persona corra la consulta.

Qué no hace: no actualiza, no copia ni cambia archivos del proyecto salvo guardar el origen con visto bueno. No usa `push`, `pull`, `fetch` ni `sync`. Esta consulta (`git ls-remote` y el clon temporal) es la única excepción de red: ver `AGENTS.md` › Git.

En el repositorio del kit (tiene `template/` y no tiene `.agents/.kit-version`) no aplica: lo dice en una línea, porque el clon ya es el kit.
