# Mecanismos de control y auditoría técnica

El control sigue las cuatro etapas vistas en la materia: **establecer estándares**, **medir** el desempeño, **comparar** con el estándar y **corregir** los desvíos, retroalimentando el proceso. Los indicadores se organizan en tres niveles: el proceso modelado en BPMN, el sistema en funcionamiento y el desarrollo del proyecto.

Los valores marcados con (P) son estándares propuestos en esta entrega como supuesto de trabajo; deben confirmarse con La Barquita en el relevamiento (tarea A1). Los demás vienen de las metas de la Entrega N.º 2.

## 1. Indicadores del proceso

| ID | Indicador | Cálculo | Estándar | Responsable | Acción correctiva ante un desvío |
|---|---|---|---|---|---|
| IP1 | Ciclo servicio-factura | % de servicios con comprobante emitido dentro del día hábil siguiente al fin del servicio | ≥ 90 % (M6) | Facturación y Cobranzas | Ver con IP2 si la demora está en la validación o en la facturación, y actuar en ese tramo. |
| IP2 | Tiempo de validación | Horas hábiles entre PENDIENTE y VALIDADO u OBSERVADO | 90 % en ≤ 4 h (P) | Jefe de Operaciones | Si se repite, delegar la validación en un segundo responsable por punto operativo. |
| IP3 | Partes observados | Partes observados / partes registrados, por mes | ≤ 10 % (P) | Jefe de Operaciones | Revisar con qué tripulación o servicio se concentran y reforzar la capacitación (tarea I2). |
| IP4 | Validaciones sin respaldo | Partes con `exige_respaldo` validados sin documento verificado | 0 | Calidad y Medio Ambiente | Es un incumplimiento de la regla de negocio 1: bloquear la liquidación y auditar el parte. |
| IP5 | Liquidaciones ajustadas | Liquidaciones con `ajuste_manual = true` / total | ≤ 5 % (P) | Ventas y Tarifas | Si se superan, revisar las tarifas cargadas: los ajustes frecuentes indican una tarifa mal definida. |
| IP6 | Rechazos de ARCA | Solicitudes de CAE rechazadas / total | ≤ 2 % (P) | Facturación y Cobranzas | Agrupar por motivo de rechazo y corregir el dato de origen (cliente, punto de venta o importe). |
| IP7 | Adopción del parte digital | Servicios con parte digital / servicios prestados, durante la operación en paralelo | ≥ 95 % (P) | Jefe de Operaciones | Identificar a las tripulaciones que siguen en papel; trabajar con el patrón de más experiencia (estructura informal, E3). |

## 2. Indicadores del sistema (evaluación operacional)

| ID | Indicador | Estándar | Cómo se mide |
|---|---|---|---|
| IS1 | Tiempo de guardado de un parte y de cálculo de una liquidación | 95 % ≤ 2 s; liquidación ≤ 5 s (M1) | Prueba de carga (P3) y registro de tiempos en producción. |
| IS2 | Sincronización sin conexión | 100 % sin pérdidas ni duplicados en ≤ 10 min (M2) | Prueba de campo (P4); en producción, partes duplicados detectados = 0. |
| IS3 | Escalabilidad | Historial de un cliente en ≤ 3 s con 60.000 partes (M3) | Prueba de carga (P3). |
| IS4 | Exactitud de la liquidación | 0 diferencias con el cálculo manual en 50 partes (M4) | Prueba P2; en producción, IP5 como control continuo. |

## 3. Indicadores del desarrollo

| ID | Indicador | Estándar | Fuente |
|---|---|---|---|
| ID1 | Cambios integrados a `main` mediante Pull Request revisado | 100 % (salvo el commit inicial) | Historial del repositorio. |
| ID2 | Commits con mensaje descriptivo (verbo en infinitivo + qué se cambió) | 100 % | Historial del repositorio. |
| ID3 | Desvío en cada hito del Gantt (H1 a H4) | 0 días sin usar la reserva; cierre antes del 30/06/2027 (M5) | Comparación del Gantt con el tablero. |
| ID4 | Historias en "Listo" que cumplen todos sus criterios de aceptación | 100 % | Revisión de cada *issue* antes de cerrarlo. |

## 4. Quién audita y cuándo

- **Calidad y Medio Ambiente** (staff, depende de la Gerencia General) revisa mensualmente IP3, IP4 e IP7. Está fuera de Operaciones, así que no audita el trabajo de su propio superior (principio de separación de funciones, E3).
- **Sistemas de Información** revisa los indicadores IS1 a IS4 en cada versión y los de desarrollo en cada hito.
- **Gerencia General** recibe el tablero de indicadores en cada hito y decide las acciones correctivas que exceden a un área.

## 5. Ajuste al diccionario de datos que surge del control

Al definir IP2 e IP3 apareció un faltante: el diccionario de la Entrega N.º 3 guarda solo el estado **actual** del parte. No registra cuándo cambió ni cuántas veces fue observado, así que esos dos indicadores no se podrían medir. Se propone agregar la entidad `HISTORIAL_ESTADO_PARTE` (ver [`diccionario-de-datos.md`](diccionario-de-datos.md)), que registra cada transición de la máquina de estados del parte.
