# Diccionario de datos del módulo

Versión inicial tomada de la Entrega N.º 3 (tarea D1 del cronograma). PEDIDO y CLIENTE ya existen en la aplicación del proveedor y no se definen acá; el módulo los referencia por su código entero (supuesto de trabajo).

Convenciones: nombres en minúscula, sin tildes y con guion bajo; la clave primaria lleva el prefijo `id_`; las claves foráneas usan el mismo nombre que la clave a la que apuntan. Los importes son `Decimal` para evitar errores de redondeo (meta M4).

## PARTE_SERVICIO (alias: parte)

Registro de lo efectivamente ejecutado en una prestación. Carga la tripulación; valida u observa el Jefe de Operaciones; consultan Facturación y Cobranzas, Atención al Cliente y Calidad y Medio Ambiente.

| Campo | Tipo | Restricciones / valores | Descripción |
|---|---|---|---|
| id_parte | Integer | Clave primaria; autoincremental | Identificador del parte en la base central. |
| uuid_dispositivo | String(36) | Único; obligatorio | Código que genera el celular al crear el parte; evita duplicados al sincronizar (M2). |
| id_pedido | Integer | FK a PEDIDO; obligatorio | Pedido de la aplicación que originó el servicio. |
| codigo_servicio | String(10) | Obligatorio; debe existir en TARIFA | Tipo de servicio prestado. |
| embarcacion | String(30) | Obligatorio; de la flota propia | Embarcación que prestó el servicio. |
| fecha_hora_inicio | DateTime | Obligatorio | Inicio real; define qué tarifa estaba vigente. |
| fecha_hora_fin | DateTime | Obligatorio; posterior al inicio | Fin real de la prestación. |
| cantidad | Decimal(10,2) | Obligatorio; mayor que 0 | Horas, toneladas, m³; la unidad la fija la tarifa. |
| latitud, longitud | Decimal(9,6) | Opcionales | Posición donde se cerró el parte. |
| legajo_tripulante | Integer | Obligatorio | Tripulante que cargó el parte. |
| estado | String(12) | BORRADOR, PENDIENTE, VALIDADO u OBSERVADO; por defecto BORRADOR | Solo un parte VALIDADO puede liquidarse. |
| legajo_validador | Integer | Nulo hasta la validación | Responsable de Operaciones que validó. |

## DOCUMENTO_RESPALDATORIO (alias: adjunto)

Documentación aduanera, sanitaria o ambiental que respalda un parte. Carga la tripulación; verifica Operaciones.

| Campo | Tipo | Restricciones / valores | Descripción |
|---|---|---|---|
| id_documento | Integer | Clave primaria; autoincremental | Identificador del documento. |
| id_parte | Integer | FK a PARTE_SERVICIO; obligatorio | Parte al que respalda. |
| tipo_documento | String(20) | MANIFIESTO_RESIDUOS, ADUANERO, SANITARIO u OTRO | Clase de documento. |
| numero_documento | String(30) | Obligatorio si es MANIFIESTO_RESIDUOS | Número del documento original. |
| ruta_archivo | String(255) | Obligatorio | Ubicación del archivo. |
| fecha_carga | DateTime | Obligatorio; lo asigna el sistema | Momento en que se adjuntó. |
| verificado | Boolean | Por defecto false | Si Operaciones controló el documento. |

## TARIFA

Precio de cada servicio en un período. Carga y modifica Ventas y Tarifas.

| Campo | Tipo | Restricciones / valores | Descripción |
|---|---|---|---|
| id_tarifa | Integer | Clave primaria; autoincremental | Identificador de la tarifa. |
| codigo_servicio | String(10) | Obligatorio | Servicio al que se aplica. |
| descripcion | String(80) | Obligatorio | Nombre del servicio en el comprobante. |
| unidad_medida | String(10) | HORA, TONELADA, M3 o UNIDAD | Unidad de la cantidad del parte. |
| precio_unitario | Decimal(12,2) | Mayor que 0 | Precio por unidad, sin impuestos. |
| vigencia_desde | Date | Obligatorio | Primer día en que rige. |
| vigencia_hasta | Date | Nulo = vigente | Último día en que rige. |
| exige_respaldo | Boolean | Por defecto false | Si es true, el parte no se valida sin documento verificado. |
| id_cliente | Integer | FK a CLIENTE; nulo = tarifa general | Tarifa especial pactada con un cliente. |

## LIQUIDACION

Cálculo del importe de un parte validado. La genera el sistema; aprueba o ajusta Facturación y Cobranzas.

| Campo | Tipo | Restricciones / valores | Descripción |
|---|---|---|---|
| id_liquidacion | Integer | Clave primaria; autoincremental | Identificador. |
| id_parte | Integer | FK a PARTE_SERVICIO; única | Cada parte se liquida una sola vez. |
| id_tarifa | Integer | FK a TARIFA | Tarifa vigente a la fecha de inicio del servicio. |
| importe_neto | Decimal(12,2) | cantidad × precio_unitario | Importe sin impuestos. |
| fecha_calculo | DateTime | Lo asigna el sistema | Momento del cálculo. |
| ajuste_manual | Boolean | Por defecto false | Si Facturación modificó el importe. |
| motivo_ajuste | String(150) | Obligatorio si hay ajuste | Deja constancia de la excepción. |
| estado | String(10) | CALCULADA, APROBADA, FACTURADA o ANULADA | Situación de la liquidación. |

## COMPROBANTE (alias: factura)

Comprobante electrónico emitido a partir de una liquidación aprobada. Un comprobante autorizado no se modifica: se anula con nota de crédito.

| Campo | Tipo | Restricciones / valores | Descripción |
|---|---|---|---|
| id_comprobante | Integer | Clave primaria; autoincremental | Identificador interno. |
| id_liquidacion | Integer | FK a LIQUIDACION | Liquidación que se factura. |
| tipo_comprobante | String(15) | FACTURA_A, FACTURA_B, NOTA_CREDITO_A o NOTA_CREDITO_B | Según la condición fiscal del cliente. |
| punto_venta | Integer | Entre 1 y 99999 | Punto de venta habilitado ante ARCA. |
| numero | Integer | Entre 1 y 99999999; único por tipo y punto de venta | Número correlativo. |
| fecha_emision | Date | Obligatorio | Fecha de emisión. |
| importe_total | Decimal(12,2) | importe_neto + IVA | Total facturado. |
| cae | String(14) | 14 dígitos; nulo hasta la autorización | Código de Autorización Electrónico. |
| vencimiento_cae | Date | Nulo hasta la autorización | Vencimiento del CAE. |
| estado | String(10) | PENDIENTE, AUTORIZADO o RECHAZADO | Resultado de la solicitud a ARCA. |

## Reglas de negocio

1. **Barrera contra la cadena crítica:** un parte cuyo servicio tiene `exige_respaldo = true` no pasa a VALIDADO sin al menos un documento verificado.
2. **Tarifa a la fecha del servicio:** la liquidación toma la tarifa vigente en `fecha_hora_inicio`.
3. **Sin duplicados al sincronizar:** `uuid_dispositivo` es único.
4. **Las excepciones quedan registradas:** tarifas especiales con `id_cliente`; ajustes con `ajuste_manual` y `motivo_ajuste`.

## HISTORIAL_ESTADO_PARTE (agregado en la Entrega N.º 4)

Registro de cada cambio de estado de un parte. Surge del control: sin esta entidad no se pueden medir el tiempo de validación (IP2) ni la proporción de partes observados (IP3). Lo genera el sistema en cada transición; nadie lo modifica.

| Campo | Tipo | Restricciones / valores | Descripción |
|---|---|---|---|
| id_historial | Integer | Clave primaria; autoincremental | Identificador del registro. |
| id_parte | Integer | FK a PARTE_SERVICIO; obligatorio | Parte que cambió de estado. |
| estado_anterior | String(12) | Nulo en el primer registro | Estado de origen de la transición. |
| estado_nuevo | String(12) | BORRADOR, PENDIENTE, VALIDADO u OBSERVADO | Estado de destino. |
| fecha_hora | DateTime | Obligatorio; lo asigna el sistema | Momento del cambio. |
| legajo | Integer | Obligatorio | Persona que provocó el cambio (tripulante o validador). |
| motivo | String(150) | Obligatorio si estado_nuevo es OBSERVADO | Motivo de la observación. |

Relación: PARTE_SERVICIO – HISTORIAL_ESTADO_PARTE, 1 a N.

5. **Cada transición queda registrada:** ningún parte cambia de estado sin un registro en HISTORIAL_ESTADO_PARTE.
