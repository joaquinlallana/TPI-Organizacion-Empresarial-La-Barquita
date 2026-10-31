# Módulo de parte de servicio digital, liquidación y facturación para La Barquita S.R.L.

**Trabajo Práctico Integrador · Organización Empresarial**
Tecnicatura Universitaria en Programación a Distancia (TUPaD) · Universidad Tecnológica Nacional · Comisión N.º 12 · 2026

| | |
|---|---|
| **Integrante** | Joaquin Dalmiro Lallana (equipo técnico unipersonal) |
| **Docente titular** | Bruno, Carolina |
| **Docente tutor** | Varberde Thompson, Francisco Alejandro |
| **Tablero del backlog** | GitHub Projects: [Backlog TPI La Barquita](https://github.com/users/joaquinlallana/projects/2) |

## Descripción del sistema

La Barquita S.R.L. es una PyME de servicios portuarios de San Lorenzo (Santa Fe) que opera desde 2005 en el corredor del río Paraná. Su aplicación móvil ya permite a los clientes pedir servicios y seguir su estado, pero el tramo posterior (registrar lo que efectivamente se ejecutó y convertirlo en liquidación y factura) no está digitalizado.

El módulo propuesto cubre ese tramo:

1. La **tripulación** carga el parte de servicio desde el celular al terminar la prestación, incluso sin señal, y adjunta la documentación respaldatoria.
2. El **Jefe de Operaciones** valida u observa el parte.
3. El **sistema** calcula la liquidación con la tarifa vigente.
4. **Facturación y Cobranzas** aprueba la liquidación y el sistema emite el comprobante electrónico con CAE de ARCA.
5. Pedido, parte, documentación y factura quedan vinculados en un único registro.

Este repositorio aloja la **documentación técnica** del proyecto. La codificación empieza el 31/12/2026 según el cronograma de la Entrega N.º 2.

## Contenido

| Ruta | Contenido |
|---|---|
| [`docs/01-proceso-bpmn.md`](docs/01-proceso-bpmn.md) | Modelo BPMN del proceso, decisiones de notación, excepciones y estados del parte. |
| [`bpmn/`](bpmn/) | Diagrama editable en draw.io (`.drawio`, tres páginas) e imágenes PNG por fase. |
| [`docs/02-historias-de-usuario.md`](docs/02-historias-de-usuario.md) | Las tres historias de usuario con criterios de aceptación y prioridad. |
| [`docs/03-control-y-auditoria.md`](docs/03-control-y-auditoria.md) | Indicadores de control, estándares de desempeño y responsables de la auditoría. |
| [`docs/diccionario-de-datos.md`](docs/diccionario-de-datos.md) | Diccionario de datos del módulo (Entrega N.º 3, con el ajuste de la Entrega N.º 4). |
| [`docs/entregas/`](docs/entregas/) | Entregas N.º 1, 2 y 3 en PDF. |
| [`CHANGELOG.md`](CHANGELOG.md) | Cambios relevantes por versión de la documentación. |

## Forma de trabajo

- **Ramas.** `main` contiene solo documentación revisada. Cada cambio se hace en una rama secundaria con prefijo según su tipo: `docs/` para documentación, `feature/` para una funcionalidad o un cambio de diseño, `fix/` para correcciones.
- **Commits.** Mensajes en español, con un verbo en infinitivo y lo que cambia (por ejemplo: `Agregar modelo BPMN de la fase de facturación`). Cuando corresponde, el mensaje referencia el *issue* (`#3`).
- **Pull Requests.** Todo cambio entra a `main` mediante un Pull Request que explica qué se cambió, por qué y cómo revisarlo (plantilla en `.github/pull_request_template.md`). Como el equipo es unipersonal, la revisión de cada PR la hace el autor con la lista de verificación de la plantilla; no hubo revisión de un par (debilidad D1 de la Entrega N.º 2).
- **Backlog.** Las historias de usuario y las tareas son *issues* del repositorio, organizados en el tablero de GitHub Projects con los estados **Por hacer**, **En progreso** y **Listo**.

## Declaración de Uso de IA

En esta entrega usé un asistente de inteligencia artificial (Claude, de Anthropic) dentro de un proyecto configurado para la materia. A continuación detallo qué le pedí, por qué lo pedí de esa forma y qué corregí.

### Prompts utilizados y justificación

| Prompt | Qué pedía | Por qué lo formulé así |
|---|---|---|
| Instrucciones del proyecto ("Asistente universitario para Trabajo Práctico Integrador") | Trabajar por etapas: comprender la consigna, descomponerla en una tabla, definir una estrategia y recién después redactar; señalar qué información es externa; revisar el borrador como profesor, investigador y corrector. | Sin estas reglas, el asistente tiende a redactar enseguida y a mezclar definiciones de distinto origen. Obligarlo a analizar antes de escribir y a marcar lo externo me permite controlar de dónde sale cada afirmación. |
| Consigna de la Entrega N.º 4 (PDF) más las tres entregas anteriores | Analizar la consigna y detectar ambigüedades antes de producir. | Quería que el trabajo continuara las decisiones ya tomadas (La Barquita, el módulo, el organigrama y el diccionario) en lugar de empezar de cero. |
| "Creá el documento en Google Docs; el BPMN hacelo en draw.io; el tablero en GitHub Projects, siempre que no haya indicación de usar Jira" | Definir las herramientas de la entrega. | Elegí draw.io porque es donde vengo trabajando los diagramas, y GitHub Projects porque la consigna permite Jira o GitHub Projects y este último se integra con los *issues* y los Pull Requests del mismo repositorio. |

### Controles críticos sobre los resultados

- **Historias de usuario.** La consigna habla de tres historias "previamente definidas", pero en mis entregas anteriores no estaban escritas en formato formal (solo figuraba la tarea A4 del Gantt). El asistente lo señaló y las definimos en esta entrega; lo dejo dicho en lugar de presentarlas como si ya existieran.
- **Estados del tablero.** Mostrar las historias como "Listo" contradecía mi propio cronograma, que empieza la codificación el 31/12/2026. Por eso en el tablero avanzan las tareas de especificación y las historias quedan "Por hacer" en cuanto a su desarrollo.
- **Notación BPMN.** Verifiqué el diagrama contra las reglas de notación BPMN: flujo de izquierda a derecha, compuertas sin la X y con la pregunta abajo, salidas etiquetadas, una tarea de evaluación antes de cada decisión y flujos de mensaje solo entre pools. El evento de espera de conexión se dejó como evento intermedio sin especificar.
- **Estándares de control.** Los valores que no salen de las metas de la Entrega N.º 2 están marcados como propuestos, porque no tengo datos reales de La Barquita para fijarlos.
- **Correcciones propias.** Sobre lo que propuso el asistente, tomé decisiones propias: elegí draw.io para el BPMN y GitHub Projects para el tablero; pedí reescribir el trabajo sin referencias al material de estudio; modifiqué el formato del documento de la entrega; y, como hice el trabajo solo, decidí no incluir a otra persona como revisora. Por eso la revisión del Pull Request del BPMN la hice yo con la lista de verificación, y ajusté los textos para que no afirmaran una revisión de pares que no existió.

La asistencia por IA no me exime de la veracidad ni de la coherencia conceptual del trabajo: cualquier error del modelo lo asumo como propio.
