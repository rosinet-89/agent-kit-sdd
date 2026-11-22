# Catálogo de señales y skills (apoyo)

Documento de apoyo, genérico y sin nombres de proyectos. Sirve para sugerir skills según el stack
del proyecto. No instala ni borra nada: solo detecta, cruza y propone.

## Cuándo se usa

- En el paso de skills candidatas de `/sdd-init`.
- Al final de `ACTUALIZAR.md`.
- A pedido de la persona.

## Detección

1. Lee los archivos que existan: manifiestos de dependencias, lockfiles, configuración de build,
   esquemas, infraestructura y CI. No hay lista fija de lenguajes ni de herramientas.
2. No abras `.env` ni archivos de credenciales.
3. Si no reconoces nada, dilo y pregunta a la persona.
4. Si las señales se contradicen, infórmalas tal cual y decide la persona.

## Tabla de señales

| Señal detectada | Tipo de skill sugerida | Motivo |
|---|---|---|
| Manifiesto de dependencias de un framework web | Convenciones del framework (estructura, rutas, errores) | El código nuevo debe seguir las mismas convenciones |
| Carpeta de migraciones | Cambios de esquema con rollback (skill del kit `migracion-bd`) | Hay esquema versionado que cambia |
| Esquema de BD o ORM | Acceso a datos y consultas de solo lectura | Los agentes consultan, no escriben (`AGENTS.md` › Guardrails) |
| Archivos de CI | Verificación previa al commit | Lo que CI exige se comprueba antes |
| Dockerfile o infraestructura como código | Despliegue y entornos | Un cambio mal hecho afecta a todo el entorno |
| Pruebas automatizadas | Cómo escribir y correr las pruebas | Mantener un solo estilo de prueba |
| Front-end con componentes | Convenciones de componentes y estilos | Consistencia visual y de estructura |
| Secretos o configuración por entorno | Seguridad básica (skill del kit `seguridad-basica`) | Evitar secretos en archivos versionados |

## Cruce con `.agents/skills/`

Cruza las señales con las carpetas de `.agents/skills/`. Entrega una tabla:

| Skill | Motivo (señal) | Estado | Cómo crearla |
|---|---|---|---|
| nombre de la skill | señal que la justifica | ya instalada, sugerida, candidata nueva o ya no aplica | remite a `.agents/README.md` (crear una skill) |

- ya instalada: existe en `.agents/skills/` y la señal sigue.
- sugerida: es una skill del kit que aún no se usa y la señal la pide.
- candidata nueva: ninguna skill cubre la señal; se propone crearla.
- ya no aplica: existe, pero su señal desapareció. Se informa; no se borra.

## Skill traída de fuera

Revísala antes de usarla: sin scripts ejecutables ni secretos (skill `seguridad-basica`). Sin
revisión no se adopta.
