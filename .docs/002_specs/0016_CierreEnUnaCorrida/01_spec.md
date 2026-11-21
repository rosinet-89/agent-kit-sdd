# 01 — Spec: Cierre en una corrida con sus commits
Estado: cerrada
Ticket: sin ticket

## Contexto y objetivo
El criterio 2 de `.docs/002_specs/README.md` › «Leyenda de estatus» exige que no quede ningún commit
pendiente del requerimiento para escribir `Cerrado`. En el orden habitual (implementar → validar →
cierre → commit), al correr el cierre el trabajo sigue sin commit: el resultado es `Implementado`, y
para llegar a `Cerrado` hacen falta los commits, una segunda corrida y un commit más solo para el
cierre. Pasó en `0013_ModelosPorAgente` y en `0014_ConfiguracionGuiada` (decisión 27 de su avance).

Objetivo: que el cierre llegue a `Cerrado` en una sola corrida, con los commits del requerimiento y
los del propio cierre confirmados por la persona en esa misma corrida, sin dejar nunca un `Cerrado`
cuyo trabajo no está registrado en Git.

## Usuarios y roles
- Persona que cierra un requerimiento: confirma o rechaza los commits.
- Sesión principal: corre `/sdd-cierre`, escribe el estatus y prepara los commits (skill `git-commit`).

## Historias de usuario
- HU-1. Como persona que cierra un requerimiento, quiero confirmar una vez los commits y que la
  spec quede `Cerrado`, para no correr el cierre dos veces ni hacer un commit solo para el estatus.

## Definiciones
- **Commits del cierre:** los commits que el cierre propone en su corrida: los cambios sin registrar
  del requerimiento (uno por módulo, según `git-commit`) y, al final, el de los archivos que escribe
  el propio cierre (avance, línea `Estado:`, fila del índice y deuda que el cierre toque).

## Requisitos funcionales (EARS)
- RF-1: CUANDO el cierre hacia `Cerrado` pasa la comprobación previa y los criterios 1, 3, 4 y 5,
  EL SISTEMA escribe `Cerrado` y `cerrada`, la fila `cierre` y su decisión, y después propone los
  commits del cierre, sin hacerlos.
- RF-2: Los commits del cierre DEBEN incluir todo cambio sin registrar del requerimiento y los
  archivos que la corrida escribió; el de los archivos del cierre va al final. La propuesta lista
  mensaje y archivos de cada commit y pide una sola confirmación para el conjunto.
- RF-3: CUANDO la persona confirma, EL SISTEMA hace los commits en ese orden y el `Cerrado` queda.
- RF-4: CUANDO la persona no confirma, EL SISTEMA cambia en la misma corrida la celda a
  `Implementado` y la línea a `implementada`, enumera el criterio 2 entre los que faltan, anota la
  decisión y no hace ningún commit.
- RF-5: SI un commit falla después de confirmar, EL SISTEMA se detiene, cambia la celda a
  `Implementado` y la línea a `implementada` sin hacer el commit de los archivos del cierre, y dice
  qué commit falló y cuáles quedaron hechos.
- RF-6: SI al llegar al criterio 2 no queda ningún cambio sin registrar del requerimiento, EL SISTEMA
  propone solo el commit de los archivos del cierre, con la misma regla de RF-3 a RF-5.
- RF-7: El criterio 2 DEBE leerse en la leyenda como «no queda un cambio del requerimiento fuera de
  los commits confirmados en la corrida del cierre», y sus tres resultados DEBEN decir que, si pasan
  la previa y los criterios 1, 3, 4 y 5, el cierre escribe `Cerrado` y `cerrada` y el criterio 2 se
  decide en esa misma corrida con los commits (RF-3 a RF-5). La leyenda es la única fuente;
  `sdd-cierre` la cita y no la copia. Las dos copias de la leyenda (`template/` y el índice del clon, ADR 0027)
  quedan iguales.

## Requisitos no funcionales
- Fuente única: la regla vive en la leyenda; el comando describe el orden de pasos y remite.
- Ningún guardrail ni regla de Git cambia: nunca `git commit` sin confirmación explícita, nunca push.

## Casos límite
- Cierre que da `Implementado` por otro criterio (1, 3, 4 o 5): no cambia; los commits siguen el
  punto «si se pide» actual.
- Descarte y reapertura: no cambian.
- Cambios ajenos al requerimiento en el árbol: no entran en los commits del cierre; se informan.
- La persona confirma solo una parte de los commits: cuenta como no confirmar (RF-4).
- La sesión se corta entre escribir `Cerrado` y la confirmación: el árbol queda con `Cerrado` sin
  commits. La siguiente corrida de `/sdd-cierre` lo trata como no confirmado: rehace la propuesta
  de commits (RF-2) sin volver a escribir la fila `cierre` (se actualiza, criterio 1).

## Fuera de alcance
- Push o publicación: siguen prohibidos.
- Proyecto sin Git y `DEPLOY-<ID>`: sin cambio respecto a la leyenda actual.
- La deuda 0013 (pulido de `sdd-cierre.md`) y la deuda 0012 (celdas de fases intermedias).

## Criterios de finalización
1. Un cierre con trabajo sin commit llega a `Cerrado` en una corrida, con una sola confirmación.
2. Sin confirmación, la celda queda `Implementado` en la misma corrida y no hay commits.
3. Las dos copias de la leyenda son iguales; el kit sube de versión con changelog y manifiesto.

## Dudas abiertas
Ninguna.
