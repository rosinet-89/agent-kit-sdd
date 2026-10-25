# Índice de planes de implementación

Punto de control de `.docs/002_specs/`. Sirve para saber, **antes de analizar algo
nuevo**, si ya se tocó ese proceso y qué carpeta leer, sin recorrerlas todas.

## Cómo usarlo

1. Busca aquí el módulo, la pantalla, la tabla o la palabra clave de lo que vas a analizar.
2. Si hay coincidencia, lee **solo esa carpeta**: primero el `PUNTO DE RETOMO` de su `05_avance.md`.
3. Si la coincidencia es parcial, anótala como dependencia en `02_analisis_impacto.md`.
4. Revisa también la deuda técnica abierta del módulo: `.docs/003_deuda/README.md`.
5. Este archivo se actualiza en cada `cierre` de feature (`/sdd-cierre`) y al abrir una carpeta
   nueva (`/sdd-spec`). Si una fila no coincide con la carpeta, manda la carpeta.

## Formato de cada carpeta

Toda carpeta nace en formato SDD (skill `sdd`): `01_spec` … `06_flujo`, `05_avance.md`, `sql/`.
Una carpeta sin `01_spec.md` es de otro origen: preguntar al usuario antes de seguir.

Estatus: `Análisis` · `Planificado` (análisis y plan, sin código) · `En curso` · `En pruebas`
(código listo, falta validar) · `Implementado` · `Diferido`.

## Índice

| NNN | Carpeta | Qué resuelve | Estatus (al último avance) | Palabras clave | Pendiente / deuda |
|---|---|---|---|---|---|
| 001 | `001_ActualizarKit` | Actualizar un proyecto con el kit instalado a una versión nueva, sin pisar lo personalizado | Implementado (2026-10-25) | actualizar, .kit-version, ACTUALIZAR.md, INSTALAR.md, migración 0.4.0 | cierre 2026-10-25 12:50; flujo aprobado; commits `f5f083b`, `63f586d`, `e7c2187`, `bb4b185`, `fc98212`; MEMORY escrita |
| 002 | `002_Kit050` | Registro retroactivo de lo ya publicado en 0.5.0, sin comportamiento nuevo | Implementado (retroactivo, 2026-10-25) | 0.5.0, adr, migracion-bd, seguridad-basica, sdd-estructura, modo mantenimiento | deuda 002 resuelta; sin `02`–`04` ni `06`; tiempos de los commits no reconstruidos |
| 003 | `003_OptimizarArnes` | Reducir tokens y pasos innecesarios de agentes y comandos sin bajar la calidad | Implementado (2026-10-25) | tokens, agentes, comandos, lecturas, permisos, status, implementer | cierre 2026-10-25 14:23; flujo aprobado; ADR 009 aceptado; commits `e170ad4`, `307ab59`, `ccd7906`; deuda 003 resuelta por la `004` |
| 004 | `004_ModeloYPermisosAgentes` | Modelo por rol y permisos reales del frontmatter de los 4 agentes (atiende la deuda 003) | Implementado (2026-10-25) | model, permissionMode, tools, omitClaudeMd, mode, permissions, frontmatter, agentes | cierre 2026-10-25 16:13; sin `06_flujo` por decisión del usuario; deuda 003 resuelta; ADR 011, 012 y 013 aceptados; deuda 006 abierta; refresco de `.agents/` hecho el 2026-10-25 (napkin y caveman locales conservados) |
| 005 | `005_ConvivenciaNumeracion` | Regla de convivencia de carpetas de 3 y 4 dígitos en un proyecto que se actualiza (atiende la deuda 004) | Implementado (2026-10-25) | numeración, 3 dígitos, 4 dígitos, E12, siguiente libre, actualizar | cierre 2026-10-25 16:50; sin `06_flujo` por decisión del usuario; deuda 004 resuelta; manifiesto y E20 al día; ADR 014 y 015 aceptados; deuda 008 abierta; MEMORY escrita (decisión 31) |
| 006 | `006_RutaSiNoDelega` | Si un agente no puede delegar, el flujo sigue por otra ruta y deja constancia, sin quedarse atascado | Implementado (2026-10-25) | delegar, subagente, coordinator, planner, ruta alterna, atasco, Cursor | cierre 2026-10-25 15:50; sin `06_flujo` por decisión del usuario; manifiesto y E20 regenerados por la 005 (F6 16:44, F8 16:49) |

## Mapa por tema

| Tema | Carpetas |
|---|---|

## Relaciones entre proyectos

(vacío)

## Reglas de mantenimiento

- Una fila por carpeta, con estatus, fecha del último avance y pendiente. Sin detalle: el detalle
  vive en la carpeta.
- Estatus y fechas salen del `PUNTO DE RETOMO` de cada carpeta, no de memoria.
- Numeración: ver "Numeración única". No se reutilizan números, aunque queden huecos.

## Numeración única (sin prefijos repetidos)

Cada carpeta lleva un prefijo `NNN_` de 3 dígitos que no se repite. En este clon no se sube a 4.
El ancho en un proyecto y la convivencia de anchos están en
`template/.docs/002_specs/README.md` › « Numeración única ». Las carpetas de sección de `.docs/` siguen en 3.
Varias personas abren carpetas en paralelo, así que dos ramas pueden tomar el mismo número; el agente lo detecta y propone corregirlo. La misma regla aplica a `.docs/003_deuda/` y a `.docs/004_adr/`.

**Al asignar un número** (carpeta nueva):
1. Siguiente = prefijo mayor + 1, mirando la copia local **y** la rama remota ya descargada: `git ls-tree --name-only @{u} .docs/002_specs/` (u `origin/<rama principal>` si la rama no tiene upstream). Es solo lectura; el agente no hace `fetch` ni `pull`.
2. Recomendar al usuario subir pronto la carpeta con su fila del índice, para reservar el número.

**Revisión de duplicados.** El agente la hace al iniciar sesión, al asignar un número y antes de preparar un commit que toque esta carpeta o `.docs/003_deuda/` (skill `git-commit`):
1. Hay duplicado si dos carpetas comparten prefijo en la copia local, o si una carpeta local sin publicar usa un prefijo que la rama remota ya tiene con otro nombre.
2. Conserva el número la que ya está en la rama remota (se publicó primero). La otra pasa al siguiente libre: prefijo mayor + 1, contando local y remota. Si ninguna está publicada, o no hay rama remota, se pregunta cuál conservar.
3. El agente avisa con los dos nombres y el número propuesto, y no renombra sin confirmación. Al confirmar: `git mv` de la carpeta, su fila en el índice y cada referencia al nombre viejo (búsqueda con `grep`), incluidas las de su propio avance, `sql/` y las deudas que la citen.
4. Un número no se reutiliza: los huecos se quedan como están.
