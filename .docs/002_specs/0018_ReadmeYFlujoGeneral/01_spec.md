# 01 — Spec: README del repo con flujo general del kit
Estado: cerrada
Ticket: sin ticket

## Contexto y objetivo
El `README.md` de la raíz describe el contenido de `template/` y el primer uso, pero no muestra el
flujo de punta a punta (instalar, personalizar, configurar, trabajar un requerimiento, actualizar),
no tiene un ejemplo de uso y su lista de comandos y archivos quedó corta frente a 0.13.0
(`/sdd-enlaces`, `/sdd-estructura`, `/sdd-pausa`, `/sdd-version`, `.docs/005_contexto/`,
`catalogo-skills.md`, `configuracion.ejemplo.md`, `entornos.ejemplo.md`). Objetivo: que quien llega
al repo entienda en una lectura qué hace el kit, cómo se instala, se configura, se usa y se
actualiza, con un diagrama del flujo general. De paso, el índice de specs deja de enlazar deudas ya
resueltas en las filas `0012` y `0015`.

## Requisitos funcionales (EARS)
- RF-1: CUANDO la persona abre `README.md`, EL README muestra un diagrama Mermaid del flujo general:
  instalar → `/sdd-init` → `/sdd-config` → trabajo con `/sdd` → actualizar → `/sdd-enlaces`, y debajo la misma
  secuencia como lista numerada (para visores sin Mermaid).
- RF-2: EL README muestra un diagrama Mermaid del ciclo de un requerimiento (spec → clarify → plan →
  tareas → implement → validate → flujo → cierre), con las puertas de aprobación de la persona y la
  diferencia entre modo completo y ligero.
- RF-3: EL README tiene la sección «Instalar»: resumen de pasos y remisión a `INSTALAR.md`, sin
  copiar su procedimiento.
- RF-4: EL README tiene la sección «Actualizar»: `/sdd-version` para saber si hay versión nueva,
  regla de no pisar lo personalizado (solo archivos KIT-VIEJO) y remisión a `ACTUALIZAR.md`.
- RF-5: EL README tiene la sección «Configurar»: qué se configura y dónde (`/sdd-init`,
  `/sdd-config`, `/sdd-enlaces`, `PREFERENCIAS.md`, `.agents/local/configuracion.md`,
  `.agents/local/entornos.md`), en una tabla de una línea por pieza, con un diagrama de quién lee
  cada archivo. Los archivos locales (no versionados) se nombran como destino y se enlaza su
  plantilla existente (`template/.agents/*.ejemplo.md`).
- RF-6: EL README tiene un «Ejemplo de uso» de un requerimiento de punta a punta con los comandos
  reales, en no más de 15 líneas.
- RF-7: EL README lista los comandos `/sdd*` y los roles con una línea cada uno y remite a
  `.agents/README.md` › «Roles y comandos» como fuente; la lista coincide con
  `template/.agents/commands/` y `template/.agents/agents/`.
- RF-8: CUANDO se implemente esta spec, las filas `0012` y `0015` del índice de specs no enlazan deudas
  en «Pendiente / deuda» (las 0012, 0013 y 0014 están `Resuelta` por `0017`).
- RF-9 (cambio 2026-11-22): EL README muestra un árbol comentado de un proyecto con el kit
  instalado: qué se versiona y qué es local (según `template/.gitignore`), y una carpeta de spec con
  sus documentos `01`–`07` y `sql/`.

## Casos límite
- Visor sin Mermaid (algunos IDE, terminal): la lista numerada de RF-1 basta para seguir el flujo.
- Ruta local que no existe en el repo (`.agents/local/…`, `PREFERENCIAS.md`): se nombra como archivo
  que crea cada dev y no cuenta como ruta citada para el lint; la ruta enlazada es la plantilla.
- Un comando o archivo cambia de nombre en otra versión: el README remite a la fuente y no copia
  reglas, para que el desfase sea solo de nombres.

## Criterios de finalización
- Lint de `AGENTS.md` › Comandos OK en los archivos tocados (UTF-8 sin BOM, rutas citadas existentes).
- Ninguna regla del kit se redefine en el README: solo resume y remite.
- RF-1 a RF-9 verificados en validate.

## Modo
`ligero`. Condiciones de [topes-y-modos.md](../../../.agents/skills/sdd/topes-y-modos.md):
1. Sí: toca `README.md` y el índice de specs (2 archivos).
2. Sí: no hay otra spec en curso (17 de 17 `Cerrado`).
3. Sí: no cambia Guardrails, constitución, numeración ni contratos; el README resume y remite.
4. Sí: sin ADR; Mermaid dentro del README es la opción que no añade archivos (supuesto 1).
5. Sí: sin `sql/`.
6. Sí: solo documentación; ningún comando, agente ni chequeo cambia lo que hace.
7. Sí: estimado 45 min.
8. Sí, con lectura: el único archivo del producto es `README.md`; el índice se toca solo como
   registro del flujo (filas), como en toda spec.

## Dudas abiertas
Ninguna. Supuestos explícitos (la persona los puede revertir al aprobar):
1. El flujo va dentro de `README.md` y no en un archivo nuevo: otro doc en la raíz cambiaría la
   lista de distribuibles de `AGENTS.md` › Reglas de negocio (regla global).
2. Sin subir versión: `template/` no cambia y el manifiesto sería idéntico al de 0.13.0. Si la
   persona prefiere 0.13.1, se añade la tarea de bump y changelog.
3. Idioma español, como el resto de docs del kit.
