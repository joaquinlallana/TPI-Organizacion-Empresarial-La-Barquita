# Presentación para el Coloquio Integrador

Presentación de diapositivas que resume las cuatro entregas del TPI en el orden del proceso administrativo: diagnóstico, planeación, organización y control. Cada diapositiva tiene notas del orador con el guion.

- **Diapositivas:** [Coloquio Integrador · La Barquita](https://claude.ai/artifact/CmBHQCmyjEcE8zGDkGt8a3) (se descargan en PDF o PowerPoint desde el menú Compartir).
- **Duración estimada:** 15 minutos, más las preguntas.

## Estructura

| N.º | Diapositiva | Parte del TPI | Tiempo aprox. |
|---|---|---|---|
| 1 | Portada | — | 0:30 |
| 2 | El caso: una PyME portuaria donde el tiempo de respuesta es el producto | Parte I | 0:45 |
| 3 | La brecha: la app digitalizó el pedido, el resto del ciclo no | Parte I | 1:00 |
| 4 | “La información no documenta el servicio: es parte del servicio” | Parte I | 0:30 |
| 5 | Lectura sistémica: cuatro consecuencias visibles solo desde el todo | Parte I | 1:00 |
| 6 | FODA del proyecto y dictamen de viabilidad | Parte II | 1:00 |
| 7 | Objetivo y metas M1 a M6 | Parte II | 0:45 |
| 8 | Cronograma: 27 de 29 elementos en el camino crítico | Parte II | 0:45 |
| 9 | Organigrama: el circuito cruza tres departamentos | Parte III | 1:00 |
| 10 | Diccionario de datos: seis entidades y reglas de negocio | Parte III | 0:45 |
| 11 | El proceso en BPMN 2.0, en tres fases | Parte IV | 1:00 |
| 12 | Excepciones del proceso y su origen | Parte IV | 0:45 |
| 13 | Corrección posterior a la devolución: evento intermedio condicional | Parte IV · #21 | 0:45 |
| 14 | Repositorio y backlog | Parte IV | 0:45 |
| 15 | Un ciclo de control completo (ejemplo IP1 e IP2) | Parte IV | 1:00 |
| 16 | El control corrigió el diseño: HISTORIAL_ESTADO_PARTE | Parte IV | 0:45 |
| 17 | Uso crítico de IA | Parte IV | 0:45 |
| 18 | Conclusión: la separación de funciones apareció cuatro veces | Conclusión | 0:45 |
| 19 | Preguntas | — | — |

## Preguntas probables y dónde está la respuesta

| Pregunta | Respuesta breve | Fuente |
|---|---|---|
| ¿Por qué un evento condicional y no de mensaje? | Nadie envía la señal: el proceso espera que la condición “hay conexión con el servidor” se cumpla. | `docs/01-proceso-bpmn.md` |
| ¿Por qué Sistemas de Información va en la línea y no en el staff? | Decide sobre el sistema (prioridades, versiones, proveedor) y cambia el método de Operaciones; no solo asesora. | Documento integrado, Parte III, 1.4 |
| ¿Por qué casi todo el Gantt es crítico? | Equipo unipersonal: las precedencias de recurso vuelven secuenciales tareas que podrían ser paralelas (D1). | Documento integrado, Parte II, 3.4 |
| ¿Qué pasa si el proveedor no da acceso a la API? | Es la condición 1 de viabilidad: sin acceso antes del 03/12/2026, el proyecto se replantea antes del diseño de la integración. | Documento integrado, Parte II, 1.5 |
| ¿Por qué las historias siguen en “Por hacer”? | La codificación empieza el 31/12/2026; lo terminado es su especificación. | `docs/02-historias-de-usuario.md` |
| ¿Cómo se mide el tiempo de validación? | Con HISTORIAL_ESTADO_PARTE, agregado al detectar que el diccionario no lo permitía. | `docs/03-control-y-auditoria.md` |
