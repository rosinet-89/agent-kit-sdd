---
name: formato-respuesta
description: Linea de cierre "quien responde" en cada respuesta al usuario - rol con cuadro de color, skill usada, contexto y desvios; va al final del mensaje o justo antes de pedir una decision. Activa por defecto en toda sesion y en todos los agentes (chat, coordinator, planner, implementer, reviewer); solo se quita si el usuario lo pide. Disparadores - "quien responde", "linea de contexto", "color del rol", "quitar linea de quien responde", "formato de respuesta".
---

# Formato de respuesta: línea "quién responde"

Activa por defecto. Sirve para que el usuario vea de un vistazo quién contestó, con qué skill y
contexto, y si hubo un desvío de las convenciones del repo (`AGENTS.md`, napkin, constitución,
flujo `sdd`).

## La línea

Una sola línea, sin viñetas:

```
🟦 **Chat** · Skill: <nombre | ninguna> · Contexto: <archivos o comandos clave> · Desvíos: <cuál | ninguno>
```

Cuadro de color por rol (la terminal no admite colores arbitrarios en markdown; el cuadro es lo
que se distingue rápido):

| Rol | Cuadro |
|---|---|
| Chat (sesión principal) | 🟦 |
| coordinator | 🟪 |
| planner | 🟩 |
| implementer | 🟧 |
| reviewer | 🟥 |

- **Skill:** las que se leyeron y aplicaron en esta respuesta, separadas por coma.
- **Contexto:** 2 a 4 archivos o comandos que sostienen la respuesta, no la lista completa.
- **Desvíos:** cualquier cosa hecha fuera de la convención (no pasar `/sdd` por el coordinator,
  no leer napkin o constitución, editar fuera del plan). Si no hubo: `ninguno`.
- Si el trabajo lo hizo un subagente y el coordinator o el chat lo transmiten, el cuadro es el
  de quien transmite y el rol del subagente va en Contexto (p. ej. `Contexto: planner → 03_plan.md`).

## Dónde va

| Caso | Posición |
|---|---|
| La respuesta termina sin pregunta | Última línea del mensaje |
| La respuesta pide una decisión, aprobación o confirmación | Justo antes de la pregunta, para que se vea junto a ella |
| Nunca | Al inicio: con el scroll queda oculta |

Se omite solo en respuestas triviales de una frase. Nunca se omite en una pausa por aprobación.

## Quitarla o cambiarla

Solo si el usuario lo pide. La preferencia es de cada persona, no del repo: se guarda en la
memoria personal de la herramienta (o en su configuración local), no se edita esta skill ni
`AGENTS.md`. Si el equipo quiere cambiar el formato para todos, se edita esta skill.
