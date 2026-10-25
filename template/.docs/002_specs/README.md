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

Las carpetas de sección de `.docs/` se quedan en **3 dígitos** y no se renumeran:
`001_constitucion`, `002_specs`, `003_deuda`, `004_adr`.

El consecutivo de **4 dígitos** en un proyecto (`0001`, `0002`, …) vale para los ítems de
specs, de deuda **y de ADR**. No se limita a las carpetas `001`, `002` y `003`:

```
.docs/002_specs/0001_SpecUno/
.docs/003_deuda/0001_DeudaUno.md
.docs/004_adr/0001_Titulo.md
```

`001_constitucion` no numera ítems. `004_adr` es carpeta de sección (el nombre sigue en 3
dígitos); cada decisión dentro lleva su propio `0001`.

En el clon del kit (hay `template/`, `VERSION` y `CHANGELOG.md` en la raíz) el ancho de los
ítems es el que ya declara el índice de esa copia; el chequeo E12 lo comprueba. No cambies el
ancho del clon ni el de un proyecto.

El prefijo no se repite dentro de su carpeta. Varias personas abren ítems en paralelo, así que
dos ramas pueden tomar el mismo número; el agente lo detecta y propone corregirlo. Specs, deuda
y ADR comparten esta regla de ancho y de duplicados. El contador de cada carpeta es
independiente.

**Al asignar un número** (carpeta nueva):
1. Siguiente = prefijo mayor + 1, mirando la copia local **y** la rama remota ya descargada: `git ls-tree --name-only @{u} .docs/002_specs/` (u `origin/<rama principal>` si la rama no tiene upstream). Es solo lectura; el agente no hace `fetch` ni `pull`.
2. Recomendar al usuario subir pronto la carpeta con su fila del índice, para reservar el número.

**Revisión de duplicados.** El agente la hace al iniciar sesión, al asignar un número y antes de preparar un commit que toque esta carpeta, `.docs/003_deuda/` o `.docs/004_adr/` (skill `git-commit`):
1. Hay duplicado si dos carpetas comparten prefijo en la copia local, o si una carpeta local sin publicar usa un prefijo que la rama remota ya tiene con otro nombre.
2. Conserva el número la que ya está en la rama remota (se publicó primero). La otra pasa al siguiente libre: prefijo mayor + 1, contando local y remota. Si ninguna está publicada, o no hay rama remota, se pregunta cuál conservar.
3. El agente avisa con los dos nombres y el número propuesto, y no renombra sin confirmación. Al confirmar: `git mv` de la carpeta, su fila en el índice y cada referencia al nombre viejo (búsqueda con `grep`), incluidas las de su propio avance, `sql/` y las deudas que la citen.
4. Un número no se reutiliza: los huecos se quedan como están.
