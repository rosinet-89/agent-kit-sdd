---
name: seguridad-basica
description: Procedimiento ligero de seguridad antes de un commit o al validar un feature - búsqueda de secretos y credenciales en lo que se va a versionar, y checklist corto de riesgos tipo OWASP en el código tocado. Usar desde la Fase 2 de `git-commit`, desde el reviewer en `/sdd-validate` y cuando el usuario pida revisar secretos o seguridad básica. No sustituye una auditoría ni el comando `/security-review`. Disparadores - "secretos", "credenciales", "API key", "token", "OWASP", "inyección", "revisión de seguridad", "antes del commit".
---

# Skill: Seguridad básica

Refuerza la regla dura de `AGENTS.md`: **nunca secretos ni credenciales en archivos versionados**.
Solo lee y reporta: no corrige ni borra nada sin que el usuario lo pida.

## Modo

La búsqueda de secretos (sección 1) aplica siempre. Hay dos modos:

- **Solo secretos.** El cambio es solo documentación: prosa que explica, describe o enumera, sin
  añadir ni modificar código ejecutable. Se aplica la sección 1 y se dice que el modo fue "solo secretos".
- **Completo.** El cambio toca código ejecutable, datos, accesos o secretos. Se aplican la sección 1
  y el checklist de la sección 2. Ante duda, este modo.

Código ejecutable: lo que un agente, un shell o una herramienta ejecuta o interpreta como
instrucción operativa (scripts, comandos de shell o bloques copiables, configuración con permisos,
`tools` o `mode` en el frontmatter, SQL, y cualquier cambio de datos, accesos o secretos). Un
`.md` cuenta como código ejecutable solo si añade o modifica uno de esos elementos.

## 1. Búsqueda de secretos (antes de cada commit)

Sobre **lo que entra al commit**, no sobre todo el repo:

```bash
git diff --cached --name-only
git diff --cached -U0 | grep -nEi \
  '(password|passwd|pwd|secret|token|api[_-]?key|private[_-]?key|authorization|bearer|connection ?string|BEGIN [A-Z ]*PRIVATE KEY)'
```

Qué buscar y qué se hace con cada hallazgo:

| Hallazgo | Acción |
|---|---|
| Valor real (clave, token, contraseña, cadena de conexión con usuario y clave) | **Detener el commit**, avisar, proponer moverlo a variable de entorno o archivo local ignorado |
| Archivo `.env`, `*.pem`, `*.key`, volcado de BD, respaldo | Detener; proponer ignorarlo en `.gitignore` |
| Solo el nombre de la variable o un ejemplo (`TOKEN=<tu-token>`) | Aceptable si el valor es evidentemente falso |
| Duda | Preguntar al usuario; no asumir |

Si un secreto **ya se versionó** en un commit previo, no basta borrarlo: está en el historial.
Avisar al usuario que hay que rotarlo; reescribir historial lo decide él.

Nunca se pega el valor del secreto en la respuesta, en `05_avance.md` ni en `MEMORY.md`: se
cita archivo y línea.

## 2. Checklist de código tocado

Solo sobre los archivos del `git diff`. Marca lo que aplica a ese cambio:

- [ ] **Entradas:** datos del usuario validados y, en consultas, parametrizados (sin concatenar SQL).
- [ ] **Salidas:** contenido del usuario escapado al mostrarse (XSS) o al ir a un shell o archivo.
- [ ] **Autenticación y permisos:** cada acción nueva verifica quién es el usuario y qué puede hacer.
- [ ] **Datos sensibles:** no se registran en logs ni en mensajes de error; no viajan en la URL.
- [ ] **Archivos y rutas:** sin rutas armadas con datos del usuario sin validar; subidas con tipo y tamaño acotados.
- [ ] **Dependencias:** las nuevas son necesarias, conocidas y no traen scripts de instalación opacos.
- [ ] **Configuración:** sin modos de depuración, usuarios por defecto ni permisos abiertos en lo versionado.

## 3. Reporte

Una línea por hallazgo, severidad primero:

```
CRÍTICO | ALTO | MEDIO | BAJO — <archivo>:<línea> — <riesgo> — <qué hacer>
```

Sin hallazgos: `seguridad-basica: sin hallazgos en <N> archivos`.

## Límites

- Es una revisión rápida, no una auditoría. Para análisis profundo, usar la revisión de seguridad
  de la herramienta o una persona.
- Lo que se difiere (un riesgo MEDIO o BAJO aceptado) se **propone** como deuda en
  `.docs/003_deuda/`; no se crea sin visto bueno.
