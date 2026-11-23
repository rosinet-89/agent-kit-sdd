---
description: SDD · Asistente de configuración de cada dev (paso a paso, repetible)
argument-hint: [paso]
---

Asistente de configuración de cada dev (no es una fase del flujo). Sin argumento recorre todos los pasos; con `[paso]` (modelos, subagentes, clarify, entornos, presentación o bitacora) cambia solo ese.

Qué lee: `.agents/configuracion.ejemplo.md` (pasos, sugeridos, formato del archivo local, tabla de equivalencias y auditor), `.agents/local/configuracion.md` si existe, `.agents/.kit-version` y, para detectar herramientas, el criterio de `.agents/commands/sdd-init.md` › «4. Aplicar», paso 3 (se cita, no se copia).

Qué hace:

1. Elige qué pasos ofrecer. Sin archivo local: todos. Con archivo: muestra el valor actual de cada paso y permite cambiar uno o más. Si lo invoca la actualización (`ACTUALIZAR.md`): solo los pasos sin valor (nombre fijo de la lista de `.agents/configuracion.ejemplo.md` › «Formato del archivo local» que no está como sección `##` en el local) y conserva los demás. Si lo invoca `/sdd-init`: todos, fuera del tope de su entrevista.
2. Presenta un paso a la vez: contexto en 2 líneas como máximo y opciones, con el sugerido como primera. En el paso de modelos muestra la fila de la herramienta detectada, con la alternativa de las celdas con «o» y el esfuerzo como sugerencia (el esfuerzo no se escribe). Cubre también los roles del ruteo (p. ej. `auxiliar`), con su nivel según `.agents/skills/sdd/restricciones-de-rol.md` › «Modelo por rol»; no copies valores aquí. El paso de bitácora se presenta como beta y dice qué guarda y qué no, según `.agents/configuracion.ejemplo.md` › «Paso 6. Bitácora de uso (beta)» y `.agents/skills/bitacora-uso/SKILL.md` › «Privacidad» (por remisión).
3. Si la persona salta un paso o todo el asistente, aplica el sugerido. Lo dice en el resumen del paso 10.
4. Un valor inválido o ilegible toma el sugerido y avisa en una línea.
5. Entornos: antes de declarar destinos muestra la advertencia de `.agents/entornos.ejemplo.md` › «Advertencia (S-8)» (por remisión) y pide confirmación por cada destino. Los escribe en `.agents/local/entornos.md` con el formato de ese ejemplo.
6. Auditor externo (beta): no es un paso. Solo se ofrece si la persona lo pide o si se detectan dos herramientas; entonces lo sugiere una sola vez, con la combinación probada del ejemplo. Si se activa, escribe `.agents/local/auditor.md` con el formato del ejemplo. Por omisión ese archivo no existe.
7. Escribe `.agents/local/configuracion.md`: primera línea `version: ` seguida del contenido de `.agents/.kit-version`, y luego una sección `##` por cada nombre fijo de la lista de `.agents/configuracion.ejemplo.md` › «Formato del archivo local». Escribe solo en `.agents/local/`.
8. Si `PREFERENCIAS.md` trae directivas de presentación o de dónde corre cada rol, propone moverlas a la configuración local y dejar allí solo la remisión. No lo hace sin visto bueno.
9. Si cambió algún modelo, sugiere `/sdd-enlaces` para crear o ajustar los adaptadores de rol (`.agents/sync-agents.md` › «Carpeta de agentes por archivo»).
10. Muestra un resumen por paso: el valor y si fue sugerido, más una línea con los sugeridos aplicados (paso 3).

Qué no hace: no escribe archivos versionados ni en `.agents/agents/`. No crea enlaces ni adaptadores (eso es `/sdd-enlaces`). No guarda credenciales, solo nombres de variables de entorno. No copia valores sugeridos que ya tienen fuente: remite a ella. Cumple `AGENTS.md` › Guardrails.
