---
description: SDD · Cierra el feature - previa, tres resultados, descarte y reapertura
argument-hint: <prefijo_NombreFeature>
---

Fase `cierre`. Carpeta: `.docs/002_specs/$1/`. La ejecuta la sesión principal.

Qué lee: en `05_avance.md`, la tabla de Pasos, Pruebas y PUNTO DE RETOMO, incluida la fila de modo. La línea `Estado:` de `01_spec.md`. Solo si la fila de modo incluye flujo, si `06_flujo.md` está aprobado; sin fila de modo no se exige el flujo (`.agents/skills/sdd/topes-y-modos.md`). No lee `04_tasks.md`. Deuda en `.docs/003_deuda/README.md` › `## Estados`. Un ADR, si se propone, con `.agents/skills/adr/SKILL.md`. La fila del índice, en `.docs/002_specs/README.md`. La forma de la celda, la comprobación previa, los cinco criterios, quién escribe y la reapertura, en `.docs/002_specs/README.md` › «Leyenda de estatus»: se citan y no se copian. Plan contra real y la fila de tiempos, en `.agents/skills/sdd/avance-y-tiempos.md`.

Qué hace:

Hay tres caminos. La persona elige descarte o reapertura; si no, el camino es el cierre hacia `Cerrado`. Este cierre no escribe `Diferido`, `Planificado`, `En curso` ni `En pruebas`. La persona escribe `Diferido`.

1. Toma la hora (`date '+%Y-%m-%d %H:%M'`).
2. Si la fila del índice está duplicada, se detiene sin escribir. La fila `cierre` y la fila de modo no son esa fila.
3. Cada celda que este cierre escribe usa la forma de `.docs/002_specs/README.md` › «Leyenda de estatus» y no la repite. No pega la tabla de las ocho palabras. Si no hay fila del índice, la crea al escribir esa celda, en el punto en que escribe la palabra de estatus, y no antes de la comprobación previa cuando el camino la exige.

**Cierre hacia `Cerrado`.** No es descarte ni reapertura. El orden es este: comprobación previa; después la fila `cierre` y la tabla de plan contra real; después los criterios 3 a 5; después la celda de la fila del índice y la línea `Estado:`; al final, el criterio 2 con los commits del punto 16.

4. Antes de escribir `Implementado`, `Cerrado`, `implementada` o `cerrada` exige la comprobación previa de `.docs/002_specs/README.md` › «Leyenda de estatus». Si falla, se detiene, lista el fallo y no escribe esas palabras. Qué paso cuenta, cuál está hecho y cuál deja de contar está en esa leyenda y no se copia.
5. Con la línea `analisis`, el caso es el de `.docs/002_specs/README.md` › «Leyenda de estatus» y no se repite su redacción.
6. Si la previa pasó, antes de juzgar los demás criterios escribe o actualiza en `05_avance.md` la fila `cierre` y la tabla de plan contra real. Es el criterio 1 de `.docs/002_specs/README.md` › «Leyenda de estatus» y no se redacta aquí. No se juzga ese criterio antes de dejarlas escritas. El formato de esa fila y de esa tabla está en `.agents/skills/sdd/avance-y-tiempos.md` y no se copia. Agrega una línea de qué ajustar en el siguiente plan si alguna fase se desvió fuerte.
7. Después juzga los criterios 3 a 5 citándolos desde `.docs/002_specs/README.md` › «Leyenda de estatus» y no los vuelve a redactar. El criterio 2 se decide al final, con los commits del punto 16.
8. Según ese resultado escribe la celda de la fila del índice y la línea `Estado:` de `01_spec.md`, con los tres resultados de `.docs/002_specs/README.md` › «Leyenda de estatus», y no repite su redacción. Si escribe `Cerrado`, la corrida sigue hasta el punto 16, después de la deuda y la memoria.
9. La columna de pendiente de la fila del índice, en `Implementado` o `Cerrado`, sigue el criterio 4 de la leyenda y no se redacta aquí.

**Descarte.** Lo pide la persona.

10. El descarte escribe `Descartado` y la línea `Estado:` en `descartada` sin la comprobación previa. Motivo y quién quedan en el punto de retomo. Para que el `Descartado` quede bien formado, también quedan en la columna de pendiente de la fila del índice: se escriben ahí y no se inventa otro texto.
11. Por cada `En atención` con Atendida por de este requerimiento, el cierre elige `Abierta` o `Descartada` con la decisión de la persona, según las transiciones de `.docs/003_deuda/README.md` › `## Estados`. Una `Abierta` cuyo origen es este sigue `Abierta`, salvo que esa decisión la descarte. La que atiende otro no se toca. No se pega esa sección.

**Reapertura.** La decide la persona. `Cerrado` y `Descartado` son terminales.

12. Reabrir `Cerrado` lo devuelve a `Implementado` y la línea `Estado:` a `implementada`. Reabrir `Descartado` lo devuelve a `Análisis` y `borrador` si el punto de retomo no conserva otra etapa. Si la conserva, se pregunta a la persona. Motivo, quién y fecha quedan en el punto de retomo. Lo escribe el cierre. Reabrir un `Cerrado` pasa por la comprobación previa del punto 4 y no vuelve a juzgar los cinco criterios: el destino es el de este punto. Si la previa falla, se detiene, lista el fallo y no escribe `Implementado` ni `implementada`. Reabrir un `Descartado` no las escribe y no usa la previa.
13. Una pausa no baja un `Cerrado` bien formado: primero se reabre y después se anota la pausa. La pausa y `Diferido` los escribe la persona, no este cierre. Al diferir no corre el cierre: la persona deja esa elección en el detalle al anotar la pausa.

**Deuda, memoria y commit.** En los tres caminos, y no si el punto 4 detuvo la corrida.

14. El agente propone y no escribe la deuda: la escribe este cierre, con la decisión de la persona. No se crea deuda sin visto bueno. En el camino de cierre hacia `Cerrado`, las deudas `En atención` con Atendida por de este requerimiento pasan, en la misma corrida, a `Resuelta` (con «Atendida por» y «Cómo se atendió») o vuelven a `Abierta` o `Descartada` por decisión de la persona. Lo diferido se propone dar de alta y no se crea sin ese visto bueno. El descarte sigue el punto 11. En la reapertura, y en lo que este punto no nombra, las transiciones son las de `.docs/003_deuda/README.md` › `## Estados` y no se pegan. El criterio 5 de la leyenda se cita y no se redacta: sin la decisión de la persona, falla.
15. Propón entradas para `MEMORY.md`. No las escribas sin aprobación. Si una decisión tiene alternativas reales y efecto duradero, propón un ADR en vez de una línea de memoria. Tampoco lo crees sin visto bueno.
16. Commits. Si el punto 8 escribió `Cerrado`, este punto es obligatorio y decide el criterio 2; en los demás caminos, solo si se pide. Sigue `.agents/skills/git-commit/SKILL.md`:
    - Propone los commits: los cambios sin registrar del requerimiento, uno por módulo, y al final el de los archivos que escribió esta corrida (avance, línea `Estado:`, fila del índice y deuda). Si no queda cambio del requerimiento, solo ese último. Los cambios ajenos no entran: se informan.
    - Muestra el mensaje y los archivos de cada uno y pide una sola confirmación para el conjunto. Confirmar solo una parte cuenta como no confirmar.
    - Confirmados: hace los commits en ese orden y el `Cerrado` queda.
    - Sin confirmación: en la misma corrida cambia la celda a `Implementado` y la línea `Estado:` a `implementada`, enumera el criterio 2 entre los que faltan, anota la decisión en `05_avance.md` y no hace ningún commit.
    - Si un commit falla: se detiene, cambia la celda a `Implementado` y la línea a `implementada` sin hacer el commit de los archivos del cierre, y dice qué commit falló y cuáles quedaron hechos.
    - Si una corrida anterior se cortó con `Cerrado` y sin sus commits, esta corrida lo trata como no confirmado: rehace la propuesta y actualiza la fila `cierre` sin duplicarla.
    Nunca hagas `git commit` sin confirmación explícita ni push.

Qué no hace: no lee las tareas completas. No copia la leyenda ni la sección `## Estados` de la deuda. No copia el formato de la fila de tiempos. No crea deuda, memoria ni ADR sin visto bueno. No hace `git commit` sin confirmación explícita. No escribe `Diferido`.
