# ADR 0021: Estado local `kit-visto` y aviso al arranque; solo `/sdd-enlaces` escribe

- **Estado:** Aceptado
- **Fecha:** 2026-11-02
- **Origen:** `0009_EnlacesTrasPull`

## Contexto
Tras un `pull` o un clon nuevo, el kit puede traer una versión distinta, piezas nuevas o retiradas, y enlaces de herramienta que faltan. Nada lo avisaba, y la persona no sabía qué había cambiado ni qué enlaces crear (issue #2 de rosinet-89/agent-kit-sdd). El aviso necesita comparar contra «lo último que la persona ya vio», que es distinto en cada máquina. El kit es solo Markdown, sin scripts (ADR 004).

## Decisión
El estado «visto» vive en `.agents/local/kit-visto`: texto plano local, ignorado por Git, con versión, herramientas declaradas, excepciones, copias y piezas. El paso 7 de «Cada sesión» solo lee ese estado y muestra un único aviso de hasta 3 líneas que recomienda `/sdd-enlaces`. Solo `/sdd-enlaces` escribe «visto», y solo cuando termina con éxito sobre las herramientas declaradas. Las reglas del chequeo y el formato viven en `sync-agents.md`; `AGENTS.md` remite.

## Alternativas descartadas
| Alternativa | Por qué no |
|---|---|
| Hook Git post-merge que avise o cree los enlaces | Es un script y el kit no versiona scripts (ADR 004); además no cubre un clon nuevo. Queda como opción manual de cada persona |
| Marcador versionado de «visto» | Cada persona lo cambiaría en su rama: ruido y conflictos en los merges, y «visto» es por máquina |
| Sin estado (comparar solo contra el árbol actual) | No distingue qué cambió desde la última vez; el aviso se repetiría siempre o nunca |

## Consecuencias
Se gana un aviso preciso y sin escritura al arrancar; la separación lectura (arranque) y escritura (`/sdd-enlaces`) puede reutilizarse en otros features. Se paga un archivo local más que cada máquina mantiene, y quien no declara herramientas ve el aviso cada sesión. Sin `.agents/.kit-version` ni `template/` no hay versión legible: se crean los enlaces pero no se escribe «visto». Cambiar el formato de «visto» exige actualizar `sync-agents.md` y el chequeo de arranque.
