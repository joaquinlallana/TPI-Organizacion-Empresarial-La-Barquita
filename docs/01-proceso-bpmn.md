# Proceso de registro, liquidación y facturación de un servicio (BPMN)

Modelo del proceso que el módulo va a transformar: desde que el cliente confirma un pedido en la aplicación hasta que recibe el comprobante electrónico con CAE. Está dibujado en notación **BPMN 2.0** con draw.io.

- Archivo editable: [`bpmn/proceso-parte-liquidacion-facturacion.drawio`](../bpmn/proceso-parte-liquidacion-facturacion.drawio) (tres páginas, una por fase).
- Imágenes: [`bpmn/bpmn-f1.png`](../bpmn/bpmn-f1.png), [`bpmn/bpmn-f2.png`](../bpmn/bpmn-f2.png), [`bpmn/bpmn-f3.png`](../bpmn/bpmn-f3.png).

## Qué se modela

| Elemento | Decisión |
|---|---|
| Situación | Proceso **futuro**, con el módulo funcionando. El proceso actual (parte en papel y carga manual en administración) es un supuesto de trabajo de la Entrega N.º 1 y no se diagrama. |
| Tipo de proceso | De negocio, **operativo o clave**: impacta directamente sobre el cliente, porque sin factura válida y sin respaldo documental el buque no puede acreditar el servicio. |
| Pools | **La Barquita S.R.L.** (expandido), **Cliente** y **ARCA** (cerrados, porque su funcionamiento interno no forma parte del proceso). Entre pools solo hay flujos de mensaje. |
| Lanes | Jefe de Operaciones, Tripulación, Sistema (módulo) y Facturación y Cobranzas. Salen del organigrama de la Entrega N.º 3, salvo el lane Sistema, que representa el comportamiento automatizado. |
| Fases | 1) Prestación y registro del parte, 2) Validación y liquidación, 3) Facturación electrónica. Cada fase termina en una salida intermedia y se conecta con la siguiente con un evento de enlace (A y B). |

## Elementos utilizados

| Elemento BPMN | Dónde aparece |
|---|---|
| Evento de inicio de mensaje | *Pedido confirmado en la app* (lo dispara el Cliente). |
| Evento de fin de mensaje | *Comprobante enviado al cliente*. |
| Evento intermedio sin especificar | *Conexión recuperada*: el proceso espera hasta que el dispositivo vuelve a tener señal. |
| Eventos intermedios de enlace | A: parte registrado; B: liquidación aprobada. |
| Tarea de usuario | Asignar, Cargar, Revisar, Corregir, Aprobar. |
| Tarea manual | Prestar el servicio. |
| Tarea de script | Verificar conexión; Guardar el parte en el dispositivo. |
| Tarea de servicio | Registrar el parte; Registrar el comprobante. |
| Tarea de regla de negocio | Calcular la liquidación con la tarifa vigente. |
| Tareas de envío y recepción | Solicitar el CAE a ARCA; Recibir la respuesta de ARCA. |
| Compuertas exclusivas | ¿Hay conexión?, ¿Parte completo y con respaldo?, ¿Importe correcto?, ¿CAE otorgado? |
| Objeto de datos y almacén | Parte de servicio [BORRADOR]; Base de datos del módulo (TARIFA, COMPROBANTE). |
| Anotaciones | Regla de no duplicación (uuid del dispositivo) y regla de respaldo obligatorio. |

Convenciones de la cátedra aplicadas: el flujo va de izquierda a derecha; las compuertas exclusivas se dibujan sin la X y con la pregunta abajo; todas las salidas de una compuerta tienen etiqueta; cada decisión tiene antes una tarea que la evalúa; no se dibujan compuertas convergentes cuando no evitan que una compuerta sea a la vez divergente y convergente.

## Excepciones del proceso

| Compuerta | Camino estándar | Excepción | Origen en entregas anteriores |
|---|---|---|---|
| ¿Hay conexión? | Sí: el parte se registra en el servidor. | No: se guarda en el dispositivo y se sincroniza cuando vuelve la señal. | Amenaza A4 y debilidad D4 (E2); meta M2; campo `uuid_dispositivo` (E3). |
| ¿Parte completo y con respaldo? | Sí: el parte queda VALIDADO. | No: queda OBSERVADO y la tripulación lo corrige. | Cadena crítica de residuos peligrosos (E1); regla de negocio 1, `exige_respaldo` (E3). |
| ¿Importe correcto? | Correcto: se aprueba. | Requiere ajuste: se registra el ajuste y su motivo. | Regla de negocio 4, `ajuste_manual` y `motivo_ajuste` (E3). |
| ¿CAE otorgado? | Otorgado: se registra el comprobante y se envía. | Rechazado: Facturación corrige los datos y se vuelve a solicitar. | Debilidad D3 (E2); meta M4; estado RECHAZADO del comprobante (E3). |

## Estados del parte (máquina de estados)

El campo `estado` de PARTE_SERVICIO se comporta como una máquina de estados finita. Las compuertas del BPMN son los eventos que disparan las transiciones.

| Estado actual | Evento | Estado siguiente | Acción |
|---|---|---|---|
| BORRADOR | Parte registrado en el servidor | PENDIENTE | Aparece en la bandeja del Jefe de Operaciones. |
| PENDIENTE | Parte validado | VALIDADO | Se habilita la liquidación. |
| PENDIENTE | Parte observado | OBSERVADO | Se notifica a la tripulación con el motivo. |
| OBSERVADO | Parte corregido | PENDIENTE | Vuelve a revisión. |
