---
description: SDD · Pausa - deja el punto de retomo con hora, trabajo en segundo plano y lista «al retomar»
argument-hint: [motivo]
---

Para el trabajo de una carpeta SDD y deja el punto de retomo completo. La ejecuta la sesión principal. No recibe carpeta: el motivo es todo `$ARGUMENTS` (vacío = sin motivo), opcional. Regla: `.agents/README.md` › «Argumentos de un comando».

Qué lee: `.agents/skills/sdd/avance-y-tiempos.md` › «Marca de pausa» (forma de la marca); de la carpeta activa, el PUNTO DE RETOMO de `05_avance.md`. No lee `01`–`06` completos.

Qué hace:
1. Toma la hora con `date '+%Y-%m-%d %H:%M'` e identifica la carpeta activa: la que nombra la persona o la única con la que se trabajó en la sesión. Con ninguna o con varias, pregunta cuál y no supone; si no hay candidata, lo dice y no escribe.
2. Inventario de trabajo en segundo plano (subagentes o procesos lanzados en la sesión que siguen vivos). Lo pide a la herramienta, sin suponer un mecanismo. De cada uno da hora de inicio, dónde está su recibo o estado, cómo comprobarlo y cómo cancelarlo, y pregunta qué hacer. No detiene nada sin confirmación. Si no responde, anota «estado desconocido» con cómo comprobarlo y no lo cancela. Si la herramienta no expone el dato, pregunta a la persona y anota «inventario sin dato de la herramienta».
3. Escribe en el PUNTO DE RETOMO de `05_avance.md`: fase, último paso cerrado, bloqueos y código al día; la fila «Pausa» (hora de inicio, motivo si lo hay y lista «al retomar»: qué comprobar primero, qué hacer según el resultado y el comando `/sdd-*` que sigue). Si ya había una fila «Pausa», la actualiza, no la duplica. Las comprobaciones de la lista se redactan dentro de la lista cerrada de `/sdd-status`.
4. Anota en Decisiones, con hora, «la persona pide parar» y el motivo si lo dio. La pausa no suma tiempo ni crea fila de tiempos.
5. Resume `git status` sin hacer commit. Sin Git, lo omite y lo dice.
6. Solo si la persona lo pide, deja los pendientes que no son de la carpeta en `.agents/local/retomo-sesion.md` (no versionado).

Qué no hace: no escribe nada fuera de `05_avance.md` y de `.agents/local/retomo-sesion.md`. No cambia la celda del índice ni la etapa (no es `Diferido`). No hace commit, no cancela ni detiene trabajo sin confirmación y no inicia otro paso. Las restricciones de rol: `.agents/skills/sdd/restricciones-de-rol.md`.
