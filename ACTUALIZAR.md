# Actualizar el kit en un proyecto

Este documento es para el **agente** (y para quien prefiera hacerlo a mano). El kit no trae un
comando de actualización ni scripts: el agente sigue estos pasos con el kit que ya tiene en disco.

## Índice

- [Para qué sirve y qué no](#para-qué-sirve-y-qué-no)
- [Precondiciones](#precondiciones)
- [Integridad del kit](#integridad-del-kit)
- [Huella](#huella)
- [Prueba de humo](#prueba-de-humo)
- [Ejemplos por sistema](#ejemplos-por-sistema)
- [Versión destino](#versión-destino)
- [Marcador de versión](#marcador-de-versión)
- [Al día](#al-día)
- [Marcador mayor que la versión destino](#marcador-mayor-que-la-versión-destino)
- [Clasificación](#clasificación)
- [Base sin manifiesto](#base-sin-manifiesto)
- [Huérfanos](#huérfanos)
- [Grupos por ruta](#grupos-por-ruta)
- [Plan completo](#plan-completo)
- [Confirmación](#confirmación)
- [Aplicación](#aplicación)
- [Registro «mantener»](#registro-mantener)
- [Conflicto de ruta](#conflicto-de-ruta)
- [Marcador al cerrar](#marcador-al-cerrar)
- [Reporte](#reporte)
- [Aviso de numeración](#aviso-de-numeración)
- [Enlaces](#enlaces)
- [Commit propuesto](#commit-propuesto)
- [Idempotencia y retoma](#idempotencia-y-retoma)
- [Casos límite y errores frecuentes](#casos-límite-y-errores-frecuentes)

## Para qué sirve y qué no

Sirve para actualizar un proyecto que **ya tiene el kit instalado**.

Queda fuera de este procedimiento:

- un comando nuevo;
- Git, el historial y las etiquetas del repositorio;
- la red.

Con el kit disponible en local basta.

## Precondiciones

Antes de modificar nada, comprueba todo lo siguiente. Si falla una, informa y detente.

1. El kit está en local y su raíz tiene `template/`, `VersionAgentKitSdd` y `manifiestos/`.
2. Existe el manifiesto de la versión destino: un archivo de `manifiestos/` cuyo nombre es esa
   versión y la extensión `.md` (`manifiestos/<VERSION>.md`). Si no existe, informa y no
   modifiques nada.
3. El entorno puede calcular SHA-256. La prueba de humo de abajo lo confirma. Si no se puede
   calcular, informa y no modifiques nada.
4. Si la raíz del **destino** tiene `template/`, `VersionAgentKitSdd` y `manifiestos/`, ese destino es el
   clon del kit. No corras este procedimiento. El refresco del clon ya está en `INSTALAR.md`,
   sección «Modo mantenimiento» (deuda 001, resuelta).

## Integridad del kit

Antes de modificar el proyecto, calcula la huella de cada archivo de `template/` y compárala con
`manifiestos/<VERSION>.md`. Si las huellas no coinciden, no modifiques nada.

## Huella

La huella es SHA-256 en hexadecimal minúscula (64 caracteres) de los bytes ya normalizados.
No se decodifica el texto. No entran los permisos ni las mayúsculas o minúsculas del nombre.

Normalización, en este orden:

1. Quitar el BOM inicial `EF BB BF`.
2. Cambiar CRLF por LF.
3. Quitar espacios y tabuladores al final de cada línea.
4. Quitar las líneas vacías finales y terminar con un solo LF. Un archivo vacío queda en 0 bytes.

La huella de 0 bytes es `e3b0c44298fc1c149afbf4c8996fb92427ae41e4649b934ca495991b7852b855`.

## Prueba de humo

Obligatoria, en un directorio temporal fuera del kit y del proyecto, **antes de modificar nada**.
Carga primero la función de Linux (sección siguiente) y corre este bloque. El mismo texto en LF y
el mismo texto con BOM, CRLF, espacios y tabuladores finales deben dar la misma huella. Una
palabra distinta debe dar otra. El archivo vacío debe dar la huella de 0 bytes.

Texto limpio, con LF: `linea uno`, `linea dos`, una línea vacía, `linea tres`. El sucio es ese
texto con BOM `EF BB BF`, CRLF, un espacio final en `linea uno`, un tabulador final en
`linea dos`, un espacio y un tabulador finales en `linea tres`, y dos líneas vacías al final. La
palabra distinta cambia `dos` por `otra`.

| Caso | Huella |
|---|---|
| Limpio y sucio (la misma) | `fa0bc48477b5b4d064f39b799e388c78e64c80fe29f08aee6686a8c842112630` |
| Palabra distinta (`linea otra`) | `6dcdde67a08228b75fac8acbe91cf6653b1a7679025e2493f5d9d8c3190db398` |
| Vacío | `e3b0c44298fc1c149afbf4c8996fb92427ae41e4649b934ca495991b7852b855` |

```bash
dir=$(mktemp -d)
printf 'linea uno\nlinea dos\n\nlinea tres\n' > "$dir/limpio"
printf '\xef\xbb\xbflinea uno \r\nlinea dos\t\r\n\r\nlinea tres \t\r\n\r\n\r\n' > "$dir/sucio"
printf 'linea uno\nlinea otra\n\nlinea tres\n' > "$dir/distinta"
: > "$dir/vacio"
kit_huella "$dir/limpio"
kit_huella "$dir/sucio"
kit_huella "$dir/distinta"
kit_huella "$dir/vacio"
```

Si no se puede calcular la huella, o si alguna de las tres no coincide con la tabla, informa y no
modifiques nada.

## Ejemplos por sistema

El agente detecta el entorno y usa un solo ejemplo. Los ejemplos son bloques de este documento:
no se guardan como archivos del kit ni del proyecto. En cualquier sistema, la prueba de humo tiene
que dar las tres huellas de la tabla antes de seguir.

### Linux (probado)

Probado el 2026-10-25. Esta es la función:

```bash
kit_huella() {
  (
    LC_ALL=C
    export LC_ALL
    sed -e $'1s/^\xef\xbb\xbf//' -e $'s/\r$//' -e $'s/[ \t]*$//' -- "$1" \
      | awk 'NF { for (i = 1; i <= n; i++) print ""; n = 0; print; next } { n++ }' \
      | sha256sum \
      | awk 'NR==1 { print $1 }'
  )
}
```

### macOS (parcial)

`shasum -a 256` es el comando de la suma. El 2026-10-25 el binario Perl de `shasum` de este Ubuntu
coincidió con `sha256sum` sobre bytes **ya normalizados**. El `sed` y el `awk` de BSD no se
probaron: esa parte queda **no probado**. No uses la normalización de BSD hasta que la prueba de
humo dé las tres huellas.

```sh
# Sobre bytes ya normalizados. La normalización con sed/awk de BSD: no probado.
shasum -a 256 archivo-ya-normalizado | awk 'NR==1 { print $1 }'
```

### Windows (no probado)

En este Ubuntu no hay PowerShell (2026-10-25). El bloque es un ejemplo **no probado**. En Windows
se usa PowerShell. Si no hay SHA-256 nativo, un script temporal de `python3` fuera del kit y del
proyecto; ese script no se versiona.

```powershell
# NO PROBADO. La normalización es la de la sección Huella; este bloque no la implementa.
# Sobre bytes ya normalizados:
(Get-FileHash -Algorithm SHA256 -Path .\archivo-normalizado).Hash.ToLower()
```

### python3 (probado)

Probado el 2026-10-25 con `python3 -I` (Python 3.12.3) y esta normalización: las tres huellas de
la prueba de humo coincidieron. La suma es `hashlib.sha256(normalize(datos)).hexdigest()` sobre
los bytes del archivo.

```python
def normalize(data: bytes) -> bytes:
    if data.startswith(b"\xef\xbb\xbf"):
        data = data[3:]
    data = data.replace(b"\r\n", b"\n")
    lines = [ln.rstrip(b" \t") for ln in data.split(b"\n")]
    while lines and lines[-1] == b"":
        lines.pop()
    if not lines:
        return b""
    return b"\n".join(lines) + b"\n"
```

Un archivo que termina en CR sin LF no está en la prueba. `sed` quita ese CR final; Python lo
conserva, porque solo sustituye la pareja CR LF. El comando de Linux queda como está.

## Versión destino

La versión destino es el contenido de una sola línea del archivo `VersionAgentKitSdd` del kit que se está
usando.

No propongas cambios hasta conocer la versión base y la versión destino.

## Marcador de versión

En el proyecto, el marcador es `.agents/.kit-version`: una sola línea, versionable.

Es válido si tiene una sola línea con una versión numérica. Se toleran BOM, espacios y salto de
línea final. Esa versión es la base.

Vacío, varias líneas, solo espacios o no numérico es malformado. No lo uses, infórmalo y procede
como sin marcador. No modifiques el archivo del marcador hasta terminar.

Si el proyecto no tiene marcador, o está malformado, asume 0.4.0 como versión base, di que es una
suposición y pide confirmación antes de continuar. Si el usuario indica otra versión, úsala. Si
no la sabe o no responde, trátala como base sin manifiesto o detente sin modificar nada.

### Vacío

Si `.agents/.kit-version` existe y está vacío, es malformado: no lo uses, infórmalo y procede
como sin marcador. No modifiques el archivo del marcador hasta terminar.

### Varias líneas

Si el marcador tiene varias líneas, es malformado: no lo uses, infórmalo y procede como sin
marcador. No modifiques el archivo del marcador hasta terminar. El salto de línea final de una
sola línea se tolera y no cuenta como segunda línea.

### Solo espacios

Si el marcador solo tiene espacios o tabuladores, es malformado: no lo uses, infórmalo y procede
como sin marcador. No modifiques el archivo del marcador hasta terminar. Los espacios alrededor
de una versión numérica se toleran.

### BOM

Un BOM al inicio se tolera, igual que los espacios y el salto de línea final. Si al quitarlos
queda una sola versión numérica, el marcador es válido y esa versión es la base.

### No numérico

Si la línea no es una versión numérica, es malformado: no lo uses, infórmalo y procede como sin
marcador. No modifiques el archivo del marcador hasta terminar.

## Al día

Si la versión base es igual a la destino y no hay elementos pendientes registrados, informa que
el proyecto está al día y no modifiques nada.

## Marcador mayor que la versión destino

Si el marcador es mayor que la versión destino (la del kit disponible), avisa y no modifiques
nada.

Si la versión destino no tiene manifiesto, aplica la precondición 2: informa y no modifiques
nada.

## Clasificación

Cuando conozcas la versión base y la destino, asigna a cada archivo del kit una sola clase según
esta tabla, comparando las huellas con los manifiestos de ambas versiones. Se evalúa de arriba
abajo y se para en la primera fila que cumple.

B es la huella en el manifiesto de la base. D es la huella en el manifiesto del destino. P es la
huella del archivo en el proyecto.

| # | Condición | Clase | Qué se muestra / hace |
|---|---|---|---|
| 1 | P no existe y D existe | NUEVO | Se propone copiar |
| 2 | D no existe y P existe y B existe | RETIRADO | Solo se informa; si P difiere de B, se avisa que además fue modificado |
| 3 | D existe y P = D | IGUAL | Nada (incluye el caso en que el kit no cambió entre versiones) |
| 4 | D existe, P ≠ D, B existe y P = B | KIT-VIEJO | Diferencia B→D; se propone reemplazar |
| 5 | D existe, P ≠ D, y (B no existe o P ≠ B) | PERSONALIZADO | Si B existe y B ≠ D: diferencia B→D para fusionar a mano. Si B = D o B no existe: no hay cambio del kit que mostrar; se respeta y se informa |

## Base sin manifiesto

Si no hay manifiesto para la versión base (por ejemplo, versiones anteriores a 0.4.0 o una
versión sin manifiesto como 0.4.1), o el manifiesto no incluye el archivo, clasifica con «B no
existe». Nunca como KIT-VIEJO. Avísalo.

## Huérfanos

Un archivo del proyecto que no está en ningún manifiesto (base ni destino) es un huérfano: no se
clasifica, no se toca y se informa aparte.

## Grupos por ruta

La ruta es la del proyecto, sin el prefijo `template/`. El grupo lo da la ruta, no el manifiesto.
La tabla de la sección Clasificación vale solo para el grupo A. Se evalúa de arriba abajo y se
para en la primera fila que cumple.

| # | Ruta | Grupo |
|---|---|---|
| 1 | Exactamente `PREFERENCIAS.md` | D |
| 2 | Exactamente `AGENTS.md`, `CLAUDE.md` o `.gitignore` | B |
| 3 | Bajo `.agents/`, salvo exactamente `.agents/napkin.md` | A |
| 4 | Cualquier otra | C |

La fila 4 es el grupo C: `MEMORY.md`, `.agents/napkin.md`, todo `.docs/` y cualquier ruta que no
cae en A, B ni D. Una ruta nueva sin regla se trata como grupo C (solo si falta).

`.agents/preferencias.ejemplo.md` y `.agents/skills/napkin/LICENSE` son del grupo A. El archivo
`.agents/napkin.md` no lo es.

Ante duda sobre si un archivo fue personalizado, trátalo como personalizado y respétalo.

### Grupo A

Archivos del kit puros: agentes, comandos, skills y documentos de apoyo de `.agents/`, salvo las
notas de trabajo (`.agents/napkin.md`).

Pasan por la clasificación de la sección Clasificación.

### Grupo B

Exactamente `AGENTS.md`, `CLAUDE.md` y `.gitignore`. Nunca se reemplazan ni se clasifican. Solo se
muestran los faltantes, para que el usuario los incorpore a mano o los marque «mantener».

### Faltante

Solo el grupo B. Un faltante es un elemento que una versión intermedia o la destino agregó al kit
respecto de la base y que el archivo del proyecto no contiene: una fila de la tabla de skills, un
paso de «Cada sesión» o una regla de ignorados.

Se compara por identidad del elemento (nombre de la skill, propósito del paso, regla), no por
redacción exacta.

`ChangelogAgentKitSdd.md` es la guía de qué se agregó. Sin Git no hay contenido de la base: la lista es
conservadora (lo que el destino tiene y el proyecto no). El usuario lo incorpora o lo marca
«mantener». No exijas Git.

No cuentan como faltante:

- marcadores de personalización ya sustituidos;
- secciones propias del proyecto;
- un elemento registrado como «mantener» (por ejemplo, una fila borrada a propósito).

### Grupo C

`MEMORY.md`, `.agents/napkin.md` y todo `.docs/`. Si no existen, se ofrecen como NUEVO. Si existen,
no se comparan, no se reemplazan ni se muestran faltantes. `.docs/` mezcla plantillas del kit con
documentos del proyecto, por eso toda su existencia se respeta.

Una ruta que no cae en A, B ni D recibe este mismo trato.

### Grupo D

`PREFERENCIAS.md`. Nunca se toca ni se muestra. Si no existe, aplica lo que ya indica la
instalación: el paso 10 de `AGENTS.md` lo crea copiando `.agents/preferencias.ejemplo.md` sin las
entradas de ejemplo. No está en `template/`.

### Archivo de grupo B o C eliminado

Si el proyecto eliminó un archivo de grupo B o C, ofrécelo como NUEVO y avisa que requiere
personalización posterior, o déjalo marcado «mantener».

## Plan completo

Antes de modificar cualquier archivo del proyecto, muestra el plan completo agrupado por clase,
en el orden de la tabla de Clasificación, y espera la confirmación de la sección siguiente.
No uses `git diff` ni el historial.

- **NUEVO.** Lista cada archivo que se propone copiar.
- **RETIRADO.** Lista cada archivo.
- **IGUAL.** No hay acción ni diferencia.
- **KIT-VIEJO.** La diferencia B→D, definida abajo.
- **PERSONALIZADO.** Lo definido abajo.

Qué se muestra de la base se define solo aquí.

### KIT-VIEJO

En KIT-VIEJO, P = B: el archivo del proyecto es el contenido de la base. Muestra la diferencia
entre ese archivo y el archivo de la misma ruta en `template/` del kit. Compara el contenido de
uno con el contenido del otro. Esa es la diferencia B→D visible.

### PERSONALIZADO con cambio del kit

Si B existe y B ≠ D, no hay copia del texto de la base, solo su huella. No inventes el texto
viejo. Muestra que las huellas B y D difieren, muestra el contenido del archivo de esa ruta en
`template/` y di que el texto de la base no está. El usuario lo fusiona a mano o lo marca
«mantener».

Si B = D o B no existe, no hay cambio del kit que mostrar: se respeta y se informa. Ya está en
la fila 5 de la tabla de Clasificación. No lo muestres como si hubiera diferencia.

## Confirmación

La confirmación es por archivo o por grupo de clase. Confirmar un grupo cuyas diferencias se
mostraron equivale a confirmar cada archivo del grupo. El usuario puede excluir un archivo
marcándolo «mantener».

Sin esa confirmación no apliques nada.

## Aplicación

Aplica solo lo confirmado: copia los NUEVO desde `template/` a la ruta del proyecto y reemplaza
los KIT-VIEJO.

Reemplazar KIT-VIEJO es la única excepción a «la copia nunca sobrescribe». Solo vale para
archivos KIT-VIEJO, con la diferencia ya mostrada y la confirmación explícita.

PERSONALIZADO no se modifica. RETIRADO no se borra; solo se informa.

Si el proyecto no usa Git, el respaldo previo del archivo reemplazado es opcional y va fuera del
proyecto.

## Registro «mantener»

`.agents/.kit-mantener` es un archivo de texto sin extensión, en el proyecto. Una línea por
decisión:

```text
ruta o ruta › elemento | clase | huella del destino al decidir | versión destino | AAAA-MM-DD
```

La fecha es el día en que el usuario marca «mantener». La huella es la de la sección Huella. Se
vuelve a proponer solo si la huella del destino actual difiere de la guardada.

Para un elemento del grupo B, la primera columna es `ruta › elemento` y la huella es la del texto
de ese elemento tomado de `template/`.

Incluye un NUEVO que el proyecto borró a propósito: al marcarlo «mantener» queda en este registro.

Para el aviso de numeración (`## Aviso de numeración`), la primera columna es
`aviso de numeración`, la clase es `aviso` y la huella es la del texto de la sección
« Numeración única » de `template/.docs/002_specs/README.md`; los fragmentos de los README de
deuda y de ADR no entran en la huella. El aviso reaparece solo si esa huella cambia.
El fragmento va desde la línea del título `## Numeración única (sin prefijos repetidos)` hasta la
línea anterior al siguiente encabezado de nivel 2 (`## `). Se normaliza como en `## Huella` y su
huella se calcula sobre ese texto como si fuera un archivo.

## Conflicto de ruta

Si el destino de un NUEVO está ocupado por otro tipo (una carpeta donde iría un archivo, un
enlace simbólico, o una carpeta real de una herramienta donde iría un enlace), no lo sobrescribas
ni lo muevas. Infórmalo como conflicto y cuéntalo como pendiente.

Clasifica esa ruta con los estados de `template/.agents/sync-agents.md`: `OK`, `FALTA`,
`DIFERENTE`, `OCUPADO`. Sigue las reglas de ese archivo. `OCUPADO` nunca se borra ni se mueve:
se informa y decide el usuario.

Este documento es el procedimiento de actualización. No hay comando nuevo.

## Marcador al cerrar

Escribe `.agents/.kit-version` con la versión destino solo cuando cada elemento del plan quedó
aplicado o con «mantener» explícito, y no queda ninguno de estos pendientes:

- PERSONALIZADO con cambio del kit sin resolver;
- faltantes del grupo B sin incorporar ni marcar «mantener»;
- conflictos.

El aviso de numeración no es un pendiente y no bloquea el marcador (`## Aviso de numeración`).

El archivo queda en una sola línea numérica, la de la versión destino. El formato es el de
`## Marcador de versión`.

Si queda alguno, no cambies el marcador. En la siguiente corrida, lístalos. Un rechazo total
no cambia nada, ni el marcador. Una aprobación parcial con pendientes tampoco lo cambia.

## Reporte

Al terminar, cuenta por separado:

- **Nuevos.**
- **Reemplazados.**
- **Sin cambios (IGUAL).**
- **Respetados:** PERSONALIZADO sin cambio del kit que mostrar, más los marcados «mantener».
- **Pendientes de fusión manual:** PERSONALIZADO con cambio del kit sin resolver, más faltantes
  del grupo B sin resolver, más conflictos.
- **Retirados.**

Los huérfanos van aparte y no entran en esas cuentas (`## Huérfanos`). El aviso de numeración
también va aparte y no entra en esas cuentas (`## Aviso de numeración`).

## Aviso de numeración

Un bloque más del reporte. Lo calcula la sesión al armarlo, **solo por nombres**: lista
`.docs/002_specs/`, `.docs/003_deuda/` y `.docs/004_adr/` y mira los nombres de carpeta y de
archivo. No lee el contenido de ningún README ni compara su texto: el grupo C no cambia
(`### Grupo C`).

Aparece si al menos una de las tres carpetas tiene un ítem con prefijo de 3 dígitos y `_`. Si
ninguna lo tiene, no hay aviso. Se calcula igual con o sin marcador de versión.

Contenido:

- El ancho detectado en cada carpeta.
- Si hay anchos mezclados dentro de una carpeta.
- La constancia de que no se renumera ni se renombra nada.
- El texto ofrecido: la sección « Numeración única » de `template/.docs/002_specs/README.md`
  (la única fuente de la regla; este documento no repite ningún ancho) y los párrafos
  equivalentes de los README de deuda y de ADR de `template/`.

Solo escribe en un README del proyecto si el usuario lo confirma, con la diferencia a la vista,
y uno por uno. Sin confirmación no cambia nada.

El aviso no cuenta como pendiente y no bloquea el marcador (`## Marcador al cerrar`). Se muestra
en cada actualización hasta que el usuario lo marque «mantener» (`## Registro «mantener»`).

## Enlaces

Tras el reporte, verifica los enlaces hacia las carpetas de cada herramienta según
`template/.agents/sync-agents.md`. Si faltan o están mal, propón corregirlos. No ejecutes nada
sin confirmación.

## Commit propuesto

No crees el commit.

Si el proyecto usa Git, propón un commit aparte con solo los archivos que esta actualización
tocó y espera confirmación explícita. Sigue `template/.agents/skills/git-commit/SKILL.md`.
Antes de proponerlo, sigue `template/.agents/skills/seguridad-basica/SKILL.md`.

Si el árbol ya tenía cambios sin guardar antes de empezar, avisa antes de proponer el commit,
para no mezclar ajenos.

Si el proyecto no usa Git, el commit no aplica. El resto, incluido el marcador, sigue igual.

## Idempotencia y retoma

Repetir la misma actualización sobre un proyecto ya al día no cambia nada (`## Al día`).

Si se interrumpió después de reemplazar y antes del marcador, el marcador sigue en la versión
anterior. Al repetir, lo ya aplicado sale IGUAL y solo se propone lo restante.

Lo fusionado a mano que no coincide ni con la base ni con el destino sale PERSONALIZADO hasta
que se marque «mantener».

## Casos límite y errores frecuentes

1. Sin marcador, instalado con 0.4.0: asumir 0.4.0 y confirmar. `## Marcador de versión`.
2. Sin marcador, instalado con 0.1.0 a 0.3.0: asumir 0.4.0; lo que difiera es PERSONALIZADO
   sin cambio comparable. `## Marcador de versión` y `## Base sin manifiesto`.
3. Marcador malformado (vacío, varias líneas, solo espacios, BOM, no numérico) o versión sin
   manifiesto (ejemplo 0.4.1): BOM y espacios se toleran; el resto se informa y se procede
   como sin marcador o con base sin manifiesto. `## Marcador de versión` y
   `## Base sin manifiesto`.
4. Marcador mayor que el kit disponible, o destino menor que el marcador: aviso y sin cambios.
   `## Marcador mayor que la versión destino`.
5. Destino sin manifiesto: aviso y sin cambios. `## Precondiciones` (punto 2) y
   `## Marcador mayor que la versión destino`.
6. El kit no cambió el archivo y el proyecto lo tiene igual: IGUAL, sin acción.
   `## Clasificación` (fila 3).
7. El kit no cambió el archivo y el proyecto lo personalizó: PERSONALIZADO sin cambio del kit;
   se respeta y se informa. `## Clasificación` (fila 5) y
   `### PERSONALIZADO con cambio del kit`.
8. Retirado por el kit y modificado por el proyecto: RETIRADO con aviso; no se borra.
   `## Clasificación` (fila 2) y `## Aplicación`.
9. Huérfano (ningún manifiesto lo lista): se informa aparte; no entra en las cifras del
   reporte. `## Huérfanos` y `## Reporte`.
10. Borrado a propósito de un archivo del kit: aparece como NUEVO; «mantener» y no vuelve a
    aparecer en la misma versión. `## Clasificación` (fila 1) y `## Registro «mantener»`.
11. Eliminó un archivo de fusión manual (grupo B o C): se ofrece como NUEVO o «mantener».
    `### Archivo de grupo B o C eliminado`.
12. Movió o renombró una carpeta del kit: la ruta de origen sale NUEVO y la carpeta en el sitio
    nuevo se informa como huérfano. No adivines el renombre.
13. Ruta ocupada por otro tipo: conflicto; no se sobrescribe ni se mueve.
    `## Conflicto de ruta`.
14. Interrumpida tras reemplazar y antes del marcador. `## Idempotencia y retoma`.
15. Aprobación parcial: el marcador no cambia mientras queden pendientes.
    `## Marcador al cerrar`.
16. Rechaza todo: no cambia nada, ni el marcador. `## Marcador al cerrar`.
17. Árbol con cambios sin guardar: aviso antes de proponer el commit. `## Commit propuesto`.
18. Sin Git: sin commit, con marcador. `## Commit propuesto`.
19. Fin de línea distinto, BOM o espacios finales: no generan falsos PERSONALIZADO.
    `## Huella` y `## Clasificación` (fila 3).
20. Nombres que difieren solo en mayúsculas, en un sistema que no las distingue: avisa y no
    dupliques el archivo. Los permisos de ejecución no cuentan para la clasificación
    (`## Huella`).
21. Entorno sin herramienta de huella: se informa y no se modifica nada. `## Precondiciones`
    (punto 3) y `## Prueba de humo`.
22. Aviso de numeración: se decide solo por nombres, no cuenta como pendiente ni bloquea el
    marcador, y «mantener» lo oculta mientras no cambie el texto ofrecido.
    `## Aviso de numeración` y `## Registro «mantener»`.

