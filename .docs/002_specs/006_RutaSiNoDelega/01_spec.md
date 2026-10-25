# 01 — Spec: Ruta si no delega
Estado: implementada
Ticket: sin ticket

## Contexto y objetivo

El 2026-10-25 el flujo se detuvo con un feature ya aprobado, en la fase `plan`. La atención estaba en el chat. La regla vigente dice que, si quien aplica el flujo es el chat y no el coordinator, el chat lanza al coordinator y no ejecuta las fases. El chat lanzó al coordinator. El coordinator eligió la fase `plan` y debía delegar en el planner. Iba como subagente: en esa configuración no puede abrir a otro subagente, y no redacta los documentos de diseño de la fase. Se detuvo, avisó que el planner no se pudo invocar, y la fase no se hizo. El mismo chat lanzó después al planner y esa vía sí avanzó.

Un cambio al kit tiene que poder seguir aunque un agente concreto no pueda delegar, y la fase no se queda esperando un agente intermedio. La prioridad es avanzar el desarrollo. La ruta diseñada se usa cuando quien atiende puede lanzar en un solo paso al rol de la fase. Si esa ruta insertaría un agente que a su vez tendría que delegar, o si el usuario pide que la fase se haga en el chat, la sesión continúa la misma fase por la otra ruta, hace el mismo trabajo y deja constancia en el avance del feature, con hora, en el momento. En el caso de arriba la constancia dice, en sustancia, que el coordinator no pudo delegar y que el chat lanzó al planner. La otra ruta es la excepción anotada. El camino de todos los días sigue siendo agentes y subagentes como ya están diseñados.

Esta spec completa la regla ya cerrada —el chat lanza al coordinator y no ejecuta las fases— cuando esa delegación no se puede hacer, cuando insertaría un segundo paso, o cuando el usuario pide que la fase se haga en el chat. El comportamiento es del kit, para cualquier proyecto que lo instale y para cualquier herramienta de agentes.

## Usuarios y roles

- Usuario del flujo: pide el trabajo y da el visto bueno en las puertas de aprobación de cada fase.
- Sesión principal o chat: puede lanzar subagentes. Si puede lanzar en un solo paso al rol de la fase, lo lanza. Si el usuario pidió que la fase se haga en el chat, hace ese trabajo en la sesión. Escribe el avance del feature.
- coordinator: enruta cuando la atención entra por él y delega en el subagente de la fase. Si no puede delegar, informa el porqué en una línea y suelta la fase.
- planner, implementer y reviewer: subagentes en los que la fase ya delega. Cada uno sigue redactando lo que el flujo ya le asigna. Esta spec no cambia ese reparto.
- El cierre, el estado y la validación de estructura del kit siguen en la sesión principal.

## Historias de usuario

- HU-1. Como usuario del flujo, quiero que la fase siga adelante cuando un agente no puede delegar, para que el desarrollo no se quede detenido.
- HU-2. Como sesión que sí puede lanzar subagentes, quiero tomar esa misma fase por la otra ruta, para hacer su trabajo con las mismas puertas de aprobación y los mismos documentos.
- HU-3. Como quien retoma el feature, quiero la constancia en el avance, con hora, para saber qué rol no pudo delegar, por qué, qué ruta se usó y quién lanzó al subagente.
- HU-4. Como quien instala el kit en cualquier proyecto, quiero este comportamiento en el flujo, para que valga en cualquier herramienta de agentes y no quede como preferencia personal ni como nota de un solo repositorio.

## Definiciones (solo si hay términos ambiguos)

- Ruta diseñada. Quien atiende la fase lanza en un solo paso al rol que esa fase nombra (planner, implementer o reviewer) y ese rol hace el trabajo. Si la atención entra por el coordinator y él puede lanzar ese rol, él enruta.
- Un solo paso. Hay un lanzamiento entre quien escribe el avance y el rol que hace el trabajo de la fase. Un agente intermedio que solo volvería a delegar es un segundo paso.
- Otra ruta. La sesión que atiende sigue la misma fase sin ese segundo paso. Si la delegación no se pudo hacer, lanza ella al rol que la fase pide. Si el usuario pidió que la fase se haga en el chat, hace ella el trabajo de la fase. Escribe el avance. Respeta las mismas puertas de aprobación y los mismos documentos.
- No poder delegar. Ocurre cuando el agente no puede abrir al subagente, el subagente no arranca, o la herramienta para abrirlo no está. Un subagente que sí arrancó y devolvió un resultado queda fuera de esta definición.
- Avance del feature. Registro de la fase donde quedan la constancia y los tiempos. No se crea otro registro para este caso. La constancia y los tiempos de una fase van en la misma fila.

## Requisitos funcionales (EARS)

- RF-1: MIENTRAS quien atiende la fase puede lanzar en un solo paso al rol que la fase nombra, EL SISTEMA atiende esa entrada por la ruta diseñada.
- RF-2: CUANDO un agente no puede delegar, o la ruta diseñada insertaría un agente que a su vez tendría que delegar, EL SISTEMA no detiene la fase, no espera, no reintenta esa misma llamada y sigue la misma fase por la otra ruta.
- RF-3: CUANDO el flujo toma la otra ruta porque no se pudo delegar o porque insertaría un segundo paso, EL SISTEMA hace que la sesión que atiende lance al rol que la fase pide y que esa sesión escriba el avance del feature.
- RF-4: CUANDO el flujo toma la otra ruta, EL SISTEMA ejecuta el mismo trabajo de esa fase, con las mismas puertas de aprobación y los mismos documentos, y mantiene la fase en curso hasta su propia puerta de aprobación. El rol que la fase nombra redacta esos documentos, salvo en el caso del RF-12.
- RF-5: CUANDO el flujo toma la otra ruta, EL SISTEMA deja constancia en el avance del feature, con hora, en el momento: qué rol no pudo delegar o qué pidió el usuario, por qué en una línea, qué ruta se usó y quién hizo el trabajo o lanzó al subagente.
- RF-6: SI la otra ruta tampoco puede lanzar al rol de la fase y el usuario no pidió hacerla en el chat, ENTONCES EL SISTEMA se detiene, se lo dice al usuario en una línea y no insiste.
- RF-7: EL SISTEMA deja el cierre, el estado y la validación de estructura del kit en la sesión principal.
- RF-8: SI una entrada usó la otra ruta o se detuvo porque no pudo lanzar al rol de la fase, ENTONCES EL SISTEMA vuelve a intentar la ruta diseñada en la próxima entrada a una fase, salvo que en esa entrada el usuario pida de nuevo que se haga en el chat.
- RF-9: CUANDO una fase pasa por un intento que no pudo delegar y luego sigue por la otra ruta, EL SISTEMA anota una sola fila de tiempos de esa fase. Los minutos del intento entran en esa fila y la nota dice que hubo un intento que no delegó.
- RF-10: CUANDO el agente que no pudo delegar no devuelve texto, EL SISTEMA escribe la línea del porqué la sesión que toma la otra ruta, con la causa que esa sesión vio: no puede abrir al subagente, el subagente no arranca, o la herramienta para abrirlo no está.
- RF-11: CUANDO la fase nombra más de un rol, EL SISTEMA los lanza en el orden que la fase ya define. SI un rol posterior no puede lanzarse, ENTONCES EL SISTEMA conserva lo que los anteriores ya devolvieron, detiene la fase, se lo dice al usuario en una línea y no relanza a los que ya arrancaron.
- RF-12: CUANDO el usuario pide que la fase se haga en el chat, EL SISTEMA no intenta la ruta diseñada en esa entrada, hace el trabajo de la fase en esa sesión y deja constancia de que la ruta la eligió el usuario.

## Requisitos no funcionales

- RNF-1: El comportamiento es del kit. Vale para cualquier proyecto que lo instale y para cualquier herramienta de agentes. No es una preferencia personal ni una nota de un solo repositorio.
- RNF-2: El porqué de no poder delegar cabe en una línea. El aviso de parada cabe en una línea.
- RNF-3: La constancia vive en el avance del feature. No hay un segundo registro.
- RNF-4: La fase no queda a la espera de que la herramienta permita a un subagente abrir a otro.

## Casos límite

- Si el subagente de la fase arrancó, aunque el resultado venga incompleto o erróneo, esa fase no se repite por la otra ruta.
- Si en la misma fase la llamada ya falló por no poder delegar, esa llamada no se repite.
- La constancia de una entrada no convierte a la otra ruta en el camino de la entrada siguiente.
- Sin el pedido del RF-12, la sesión no redacta los documentos de la fase para destrabar el flujo: lanza a quien la fase ya nombra.
- Cierre, estado y validación de estructura siguen atendiéndose en la sesión principal.
- Si las dos rutas no pueden lanzar al rol de la fase y no hay pedido del RF-12, no hay una tercera tentativa.
- El intento que no delegó y el trabajo que sí se hizo cuentan en la misma fila de tiempos.

## Fuera de alcance

- Cambiar modelos, permisos o la cabecera de definición de los agentes. Eso queda en el feature de modelo y permisos de agentes, ya aprobado.
- La numeración de tres y de cuatro dígitos.
- Pedir que la herramienta permita a un subagente abrir a otro. El kit se adapta a la configuración que hay.
- Reabrir o reescribir el feature ya cerrado que fijó que el chat lanza al coordinator y no ejecuta las fases.
- Saltar una puerta de aprobación o dejar de anotar los tiempos.
- Redactar los documentos de la fase desde la sesión cuando el usuario no pidió que esa fase se haga en el chat.

## Criterios de finalización

- Si quien atiende puede lanzar en un solo paso al rol de la fase, esa entrada usa la ruta diseñada.
- Si no puede, o si ese camino insertaría un agente que a su vez tendría que delegar, la fase sigue por la otra ruta en la misma entrada, con constancia, o se detiene con una línea al usuario cuando esa ruta tampoco puede lanzar y no hay pedido de hacerla en el chat.
- Si el usuario pide que la fase se haga en el chat, esa entrada se hace en la sesión y la constancia dice que la ruta la eligió él.
- Los minutos del intento que no delegó quedan en la misma fila de la fase.
- La fase no queda esperando un cambio de configuración ni repite la misma llamada.
- La entrada siguiente vuelve a empezar por la ruta diseñada, salvo un pedido nuevo de hacerla en el chat.
- Cierre, estado y validación de estructura siguen en la sesión principal.
- Las puertas de aprobación y el registro de tiempos se mantienen en las dos rutas.

## Dudas abiertas

Ninguna.
