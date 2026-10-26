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

| Prefijo | Carpeta | Qué resuelve | Estatus (al último avance) | Palabras clave | Pendiente / deuda |
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

Esta es la única fuente de la regla de numeración. Las demás piezas remiten aquí.

Las carpetas de sección de `.docs/` se quedan en **3 dígitos** y no se renumeran:
`001_constitucion`, `002_specs`, `003_deuda`, `004_adr`. No son ítems.

**Ancho.**
- Un **ítem** es una carpeta de `.docs/002_specs/`, o un archivo de `.docs/003_deuda/` o `.docs/004_adr/`, cuyo nombre empieza con dígitos y `_`. `README.md` y los marcadores de carpeta vacía no son ítems. Un nombre que no empieza por dígitos y `_` (`abc_Mal`) no entra en el consecutivo ni en el ancho y no se renombra.
- **Ancho válido**: un prefijo numérico vale su entero (`17` = `017` = `0017` = `00000017`). Los ceros de relleno y el ancho escrito no son fallo y el nombre no se reescribe. No se marca fallo por el ancho escrito. Dos ítems de la misma carpeta con el mismo entero son duplicado y es `FALLA`. Esa igualdad no es `AVISO`. `015` y `0015` son el mismo entero. Un prefijo relleno cuenta para el entero y el duplicado, y no fija el ancho de la carpeta con su cantidad de dígitos. El desbordamiento sin relleno (`1000` en ancho 3) sigue en el ADR 014 y no es este borde.
- **Ancho de una carpeta**: el del ítem de mayor valor numérico, sin que un prefijo relleno lo fije con su cantidad de dígitos. En un proyecto, un ítem nuevo conserva el ancho de su carpeta. **Empate de valor con anchos distintos** (`017_A` y `0017_B`): es un duplicado que se resuelve antes (« Revisión de duplicados »); mientras no se resuelva, el ancho de la carpeta es el del ítem publicado en la rama remota y, si ninguno lo está, el mayor ancho. Cada carpeta (specs, deuda, ADR) lleva el suyo: un ADR nuevo en una carpeta de ADR de 3 dígitos usa 3 aunque las specs estén en 4.
- **Carpeta vacía**: usa el ancho del ítem de mayor valor numérico entre las otras dos carpetas con ítems; si empatan en valor, manda el orden specs, deuda, ADR. Si ninguna tiene ítems (proyecto nuevo), usa 4 dígitos.
- **Clon del kit** (hay `template/`, `VersionAgentKitSdd` y `ChangelogAgentKitSdd.md` en la raíz): un ítem nuevo se escribe con 4 dígitos. Los ya creados no se renumeran (`001` sigue `001`). En el clon, el ancho ya escrito no es `FALLA` y una mezcla válida no es `AVISO`. El `AVISO` de mezcla válida sigue siendo solo de proyecto. El chequeo E12 lo comprueba. Un ítem con más dígitos que el ancho declarado porque su valor los necesita (1000 en una carpeta de 3) es un desbordamiento, no un ancho distinto del declarado.
- **Desbordamiento**: si el valor siguiente necesita más dígitos que el ancho de la carpeta (1000 en una de 3), se usan los que necesita, sin rellenar ni truncar. La carpeta pasa a tener anchos mezclados y su ancho pasa a ser el del ítem de mayor valor.

Ejemplos de rutas (sin número):

```
.docs/002_specs/<prefijo>_SpecUno/
.docs/003_deuda/<prefijo>_DeudaUno.md
.docs/004_adr/<prefijo>_Titulo.md
```

**Siguiente libre** (carpeta nueva):
1. Siguiente = mayor valor numérico + 1. El prefijo se lee como entero (`017` y `0017` valen 17), nunca como texto. Se cuentan la copia local **y** la rama remota ya descargada: `git ls-tree --name-only @{u} .docs/002_specs/` (u `origin/<rama principal>` si la rama no tiene upstream). Es solo lectura; el agente no hace `fetch` ni `pull`. El ancho no interviene en la comparación: solo se aplica al escribir el número nuevo.
2. Si no hay rama remota ni upstream, el cálculo usa solo la copia local; si hay duplicado, se pregunta cuál conservar.
3. Los huecos no se rellenan ni se reutiliza un número ya usado, sea cual sea el ancho.
4. Recomendar al usuario subir pronto la carpeta con su fila del índice, para reservar el número.

**Revisión de duplicados.** El agente la hace al iniciar sesión, al asignar un número y antes de preparar un commit que toque esta carpeta, `.docs/003_deuda/` o `.docs/004_adr/` (skill `git-commit`). Varias personas abren ítems en paralelo, así que dos ramas pueden tomar el mismo número. El contador de cada carpeta es independiente.
1. Hay duplicado si dos ítems comparten **valor numérico** en la copia local (`017_A` y `0017_B`), o si un ítem local sin publicar usa un valor que la rama remota ya tiene con otro nombre o con otro ancho.
2. Con el mismo nombre tras el prefijo (`017_A` remoto y `0017_A` local), es probable que sea el mismo ítem con el prefijo escrito en otro ancho: se propone renombrar el local al nombre publicado, con confirmación. Solo si el usuario indica que son ítems distintos se trata como duplicado.
3. Conserva el número la que ya está en la rama remota (se publicó primero). La otra pasa al siguiente libre, contando local y remota. Si ninguna está publicada, o no hay rama remota, se pregunta cuál conservar.
4. El agente avisa con los dos nombres y el número propuesto, y no renombra sin confirmación. Al confirmar: `git mv` de la carpeta, su fila en el índice y cada referencia al nombre viejo (búsqueda con `grep`), incluidas las de su propio avance, `sql/` y las deudas que la citen.

**Regla:** nunca se renumera por un cambio de ancho ni de versión del kit: solo se renombra para resolver un duplicado, con confirmación.

**Referencias cortas** (`017`):
- La cita al ítem que conservó el número sigue válida.
- La cita al ítem renombrado por un duplicado se actualiza como parte del renombrado.
- Mientras `017` identifique a dos ítems sin resolver, la cita es ambigua: usa el nombre completo de la carpeta.
