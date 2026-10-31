# Historias de usuario y backlog

El backlog se gestiona en **GitHub Projects** (tablero vinculado a este repositorio). Cada historia es un *issue* con la etiqueta `historia-de-usuario`, sus criterios de aceptación y su prioridad. Las tareas de esta etapa son *issues* propios, vinculados a la historia que especifican.

## Roles

| Rol (Scrum) | Quién |
|---|---|
| Product Owner | Sección Sistemas de Información de La Barquita, contraparte del proyecto (supuesto de trabajo). |
| Scrum Master y equipo de desarrollo | Joaquin Lallana (equipo unipersonal, debilidad D1 de la Entrega N.º 2). |

## HU1. Cargar el parte de servicio sin depender de la señal

**Como** tripulante, **quiero** cargar el parte de servicio desde el celular al terminar la prestación, aunque no tenga señal, **para** no tener que completarlo en papel y que la administración lo reciba sin errores de transcripción.

- **Prioridad:** alta.
- **Criterios de aceptación:**
  1. El parte no se puede enviar sin servicio, embarcación, fecha y hora de inicio y de fin, y cantidad mayor que cero.
  2. Se pueden adjuntar fotos o PDF de la documentación respaldatoria.
  3. Sin conexión, el parte queda guardado en el dispositivo y se sincroniza dentro de los 10 minutos de recuperada la señal, sin pérdidas ni duplicados (meta M2).
  4. Al registrarse en el servidor, el parte pasa de BORRADOR a PENDIENTE.
- **Trazabilidad:** BPMN fase 1 (lanes Tripulación y Sistema); tarea C2 del Gantt; entidades PARTE_SERVICIO y DOCUMENTO_RESPALDATORIO.

## HU2. Validar u observar los partes pendientes

**Como** Jefe de Operaciones, **quiero** revisar cada parte pendiente junto con su documentación y validarlo u observarlo, **para** que solo se facture lo que efectivamente se prestó y con el respaldo que exige la normativa.

- **Prioridad:** alta.
- **Criterios de aceptación:**
  1. Existe una bandeja con los partes en estado PENDIENTE, ordenados por antigüedad.
  2. Si la tarifa del servicio tiene `exige_respaldo = true`, el sistema no permite validar el parte sin al menos un documento verificado.
  3. Observar un parte exige escribir el motivo, que le llega a la tripulación.
  4. Al validar queda registrado quién validó (`legajo_validador`) y cuándo.
- **Trazabilidad:** BPMN fase 2 (compuerta ¿Parte completo y con respaldo?); tarea C3 del Gantt; regla de negocio 1 del diccionario.

## HU3. Liquidar y facturar sin cálculos manuales

**Como** responsable de Facturación y Cobranzas, **quiero** que el sistema calcule la liquidación con la tarifa vigente y emita el comprobante electrónico con CAE, **para** facturar dentro del día hábil siguiente a la prestación sin hacer cálculos a mano.

- **Prioridad:** alta.
- **Criterios de aceptación:**
  1. La liquidación usa la tarifa vigente en la fecha de inicio del servicio, no en la fecha del cálculo.
  2. Cualquier ajuste del importe exige registrar el motivo.
  3. El comprobante se solicita a ARCA y, si se rechaza, el sistema muestra el motivo y permite corregir y volver a solicitar.
  4. El comprobante autorizado queda vinculado al pedido, al parte y a la liquidación.
- **Trazabilidad:** BPMN fases 2 y 3; tareas C4, C5 y C6 del Gantt; metas M4 y M6.

## Estados del tablero

| Columna | Qué significa en esta etapa |
|---|---|
| Por hacer | Trabajo planificado que todavía no empezó. Las tres historias están acá en cuanto a su **desarrollo**, porque según el Gantt la codificación empieza el 31/12/2026. |
| En progreso | Trabajo que se está haciendo ahora. |
| Listo | Trabajo terminado y revisado. Las tareas de **especificación** de cada historia (criterios, modelado en BPMN y datos) están en esta columna. |

Separar la especificación del desarrollo evita mostrar como terminado algo que el cronograma todavía no empezó.
