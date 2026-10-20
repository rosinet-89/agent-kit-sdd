# Instalar el kit en un proyecto

Este documento es para el **agente** (y para quien prefiera hacerlo a mano). El kit no trae scripts
de instalación: dependen del SO y del shell, y el kit es genérico. Instalar = **copiar el contenido
de `template/` a la raíz del proyecto destino, sin sobrescribir nada**.

## Qué hace el agente

1. Pregunta (o deduce) la ruta del proyecto destino. Debe existir.
2. Detecta el SO y el shell disponibles (Windows: PowerShell o Git Bash; macOS/Linux: sh/zsh/bash).
   No asumas ninguno.
3. Lista los archivos de `template/` (incluidos los ocultos: `.agents/`, `.docs/`, `.gitignore`)
   y marca cuáles ya existen en el destino.
4. Ofrece **una** de estas dos salidas y espera la elección del usuario:
   - **Script:** genera un script corto para su SO/shell, lo muestra y lo ejecuta solo con
     confirmación. Guárdalo fuera del kit y del destino (carpeta temporal); no lo dejes en el repo.
   - **Paso a paso:** da los comandos de copia (uno por carpeta/archivo) para que el usuario los corra.
5. Reglas de la copia, sea script o manual:
   - **Nunca sobrescribir** un archivo existente: se omite y se reporta.
   - Si el destino ya tiene `.gitignore`, `AGENTS.md`, `CLAUDE.md` o `MEMORY.md`, **no los
     reemplaces**: muestra qué líneas o secciones del kit faltan y propón fusionarlas a mano
     (en `.gitignore`: las reglas de `template/.gitignore`, incluida `PREFERENCIAS.md`).
   - No crear enlaces hacia `.claude/`, `.cursor/`, etc.: eso lo propone el agente dentro del
     proyecto con `.agents/sync-agents.md`.
6. Al terminar, reporta: copiados, omitidos y pendientes de fusionar, más la versión del kit
   (archivo `VERSION`).
7. Siguiente paso para el usuario: abrir el **proyecto** (no el kit) con su agente y correr `/sdd-init`.

## A mano (sin agente)

Copia el contenido de `template/` (con archivos ocultos) a la raíz del proyecto, sin pisar lo que
exista. Ejemplos de la idea, adáptalos a tu shell:

```sh
# macOS / Linux (cp -n no sobrescribe)
cp -Rn template/. /ruta/al/proyecto/
```
```powershell
# Windows: copia omitiendo los existentes
robocopy template C:\ruta\al\proyecto /E /XC /XN /XO
```

Revisa después el `.gitignore` del proyecto y fusiona a mano las reglas de `template/.gitignore`.
