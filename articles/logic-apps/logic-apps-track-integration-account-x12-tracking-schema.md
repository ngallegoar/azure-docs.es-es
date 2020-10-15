---
title: Esquemas de seguimiento de X12 para mensajes B2B
description: Creación de esquemas de seguimiento para supervisar mensajes X12 en Azure Logic Apps
services: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: jonfan, estfan, logicappspm
ms.topic: article
ms.date: 01/01/2020
ms.openlocfilehash: 5b2df194761ebc167e67498a985960a4fce35f19
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "76905303"
---
# <a name="create-schemas-for-tracking-x12-messages-in-azure-logic-apps"></a>Creación de esquemas para el seguimiento de mensajes X12 en Azure Logic Apps

Para ayudarle a supervisar éxitos, errores y propiedades de mensaje para transacciones de negocio a negocio (B2B), puede usar estos esquemas de seguimiento de X12 en su cuenta de integración de Azure:

* Esquema de seguimiento de conjuntos de transacciones de X12
* Esquema de seguimiento de confirmación de conjuntos de transacciones de X12
* Esquema de seguimiento de intercambio de X12
* Esquema de seguimiento de confirmación de intercambio de X12
* Esquema de seguimiento de grupos funcionales de X12
* Esquema de seguimiento de confirmación de grupos funcionales de X12

## <a name="x12-transaction-set-tracking-schema"></a>Esquema de seguimiento de conjuntos de transacciones de X12

```json
{
   "agreementProperties": {
      "senderPartnerName": "",
      "receiverPartnerName": "",
      "senderQualifier": "",
      "senderIdentifier": "",
      "receiverQualifier": "",
      "receiverIdentifier": "",
      "agreementName": ""
   },
   "messageProperties": {
      "direction": "",
      "interchangeControlNumber": "",
      "functionalGroupControlNumber": "",
      "transactionSetControlNumber": "",
      "CorrelationMessageId": "",
      "messageType": "",
      "isMessageFailed": "",
      "isTechnicalAcknowledgmentExpected": "",
      "isFunctionalAcknowledgmentExpected": "",
      "needAk2LoopForValidMessages": "",
      "segmentsCount": ""
   }
}
```

| Propiedad | Obligatorio | Tipo | Descripción |
|----------|----------|------|-------------|
| senderPartnerName | No | String | Nombre de asociado del remitente del mensaje X12. |
| receiverPartnerName | No | String | Nombre de asociado del destinatario del mensaje X12. |
| senderQualifier | Sí | String | Calificador del asociado remitente. |
| senderIdentifier | Sí | String | Identificador del asociado remitente. |
| receiverQualifier | Sí | String | Calificador del asociado destinatario. |
| receiverIdentifier | Sí | String | Identificador del asociado destinatario. |
| agreementName | No | String | Nombre del contrato X12 en el que se resuelven los mensajes |
| direction | Sí | Enum | Dirección del flujo de mensajes, que es `receive` o `send`. |
| interchangeControlNumber | No | String | Número de control de intercambio. |
| functionalGroupControlNumber | No | String | Número de control funcional. |
| transactionSetControlNumber | No | String | Número de control de conjunto de transacciones. |
| CorrelationMessageId | No | String | El identificador de mensaje de correlación es la combinación de {AgreementName} *{GroupControlNumber}* {TransactionSetControlNumber}. |
| messageType | No | String | Conjunto de transacciones o tipo de documento. |
| isMessageFailed | Sí | Boolean | Si el mensaje X12 genera error. |
| isTechnicalAcknowledgmentExpected | Sí | Boolean | Si la confirmación técnica se ha configurado en el contrato X12. |
| isFunctionalAcknowledgmentExpected | Sí | Boolean | Si la confirmación funcional se ha configurado en el contrato X12. |
| needAk2LoopForValidMessages | Sí | Boolean | Si el bucle AK2 es necesario para un mensaje válido. |
| segmentsCount | No | Entero | Número de segmentos del conjunto de transacciones de X12. |
|||||

## <a name="x12-transaction-set-acknowledgment-tracking-schema"></a>Esquema de seguimiento de confirmación de conjuntos de transacciones de X12

```json
{
   "agreementProperties": {
      "senderPartnerName": "",
      "receiverPartnerName": "",
      "senderQualifier": "",
      "senderIdentifier": "",
      "receiverQualifier": "",
      "receiverIdentifier": "",
      "agreementName": ""
   },
   "messageProperties": {
      "direction": "",
      "interchangeControlNumber": "",
      "functionalGroupControlNumber": "",
      "isaSegment": "",
      "gsSegment": "",
      "respondingfunctionalGroupControlNumber": "",
      "respondingFunctionalGroupId": "",
      "respondingtransactionSetControlNumber": "",
      "respondingTransactionSetId": "",
      "statusCode": "",
      "processingStatus": "",
      "CorrelationMessageId": "",
      "isMessageFailed": "",
      "ak2Segment": "",
      "ak3Segment": "",
      "ak5Segment": ""
   }
}
```

| Propiedad | Obligatorio | Tipo | Descripción |
|----------|----------|------|-------------|
| senderPartnerName | No | String | Nombre de asociado del remitente del mensaje X12. |
| receiverPartnerName | No | String | Nombre de asociado del destinatario del mensaje X12. |
| senderQualifier | Sí | String | Calificador del asociado remitente. |
| senderIdentifier | Sí | String | Identificador del asociado remitente. |
| receiverQualifier | Sí | String | Calificador del asociado destinatario. |
| receiverIdentifier | Sí | String | Identificador del asociado destinatario. |
| agreementName | No | String | Nombre del contrato X12 en el que se resuelven los mensajes |
| direction | Sí | Enum | Dirección del flujo de mensajes, que es `receive` o `send`. |
| interchangeControlNumber | No | String | Número de control de intercambio de la confirmación funcional. El valor se rellena solo en el lado de envío en el que se ha recibido confirmación funcional de los mensajes enviados al asociado. |
| functionalGroupControlNumber | No | String | Número de control del grupo funcional de la confirmación funcional. El valor se rellena solo en el lado de envío en el que se ha recibido confirmación funcional de los mensajes enviados al asociado. |
| isaSegment | No | String | Segmento ISA del mensaje. El valor se rellena solo en el lado de envío en el que se ha recibido confirmación funcional de los mensajes enviados al asociado. |
| gsSegment | No | String | Segmento GS del mensaje. El valor se rellena solo en el lado de envío en el que se ha recibido confirmación funcional de los mensajes enviados al asociado. |
| respondingfunctionalGroupControlNumber | No | String | Número de control de intercambio de respuesta. |
| respondingFunctionalGroupId | No | String | Identificador del grupo funcional de respuesta, que se asigna a AK101 en la confirmación. |
| respondingtransactionSetControlNumber | No | String | Número de control del conjunto de transacciones de respuesta. |
| respondingTransactionSetId | No | String | Identificador del conjunto de transacciones de respuesta, que se asigna a AK201 en la confirmación. |
| statusCode | Sí | Boolean | Código de estado de la confirmación del conjunto de transacciones. |
| segmentsCount | Sí | Enum | Código de estado de confirmación con estos valores permitidos: `Accepted`, `Rejected` y `AcceptedWithErrors` |
| processingStatus | Sí | Enum | Estado de procesamiento de la confirmación con estos valores permitidos: `Received`, `Generated` y `Sent` |
| CorrelationMessageId | No | String | El identificador de mensaje de correlación es la combinación de {AgreementName} *{GroupControlNumber}* {TransactionSetControlNumber}. |
| isMessageFailed | Sí | Boolean | Si el mensaje X12 genera error. |
| ak2Segment | No | String | Confirmación para un conjunto de transacciones del grupo funcional recibido. |
| ak3Segment | No | String | Indica que hay errores en un segmento de datos. |
| ak5Segment | No | String | Indica si el conjunto de transacciones identificado en el segmento de AK2 se acepta o se rechaza y por qué. |
|||||

## <a name="x12-interchange-tracking-schema"></a>Esquema de seguimiento de intercambio de X12

```json
{
   "agreementProperties": {
      "senderPartnerName": "",
      "receiverPartnerName": "",
      "senderQualifier": "",
      "senderIdentifier": "",
      "receiverQualifier": "",
      "receiverIdentifier": "",
      "agreementName": ""
   },
   "messageProperties": {
      "direction": "",
      "interchangeControlNumber": "",
      "isaSegment": "",
      "isTechnicalAcknowledgmentExpected": "",
      "isMessageFailed": "",
      "isa09": "",
      "isa10": "",
      "isa11": "",
      "isa12": "",
      "isa14": "",
      "isa15": "",
      "isa16": ""
   }
}
```

| Propiedad | Obligatorio | Tipo | Descripción |
|----------|----------|------|-------------|
| senderPartnerName | No | String | Nombre de asociado del remitente del mensaje X12. |
| receiverPartnerName | No | String | Nombre de asociado del destinatario del mensaje X12. |
| senderQualifier | Sí | String | Calificador del asociado remitente. |
| senderIdentifier | Sí | String | Identificador del asociado remitente. |
| receiverQualifier | Sí | String | Calificador del asociado destinatario. |
| receiverIdentifier | Sí | String | Identificador del asociado destinatario. |
| agreementName | No | String | Nombre del contrato X12 en el que se resuelven los mensajes |
| direction | Sí | Enum | Dirección del flujo de mensajes, que es `receive` o `send`. |
| interchangeControlNumber | No | String | Número de control de intercambio. |
| isaSegment | No | String | Segmento ISA del mensaje. |
| isTechnicalAcknowledgmentExpected | Boolean | Si la confirmación técnica se ha configurado en el contrato X12.  |
| isMessageFailed | Sí | Boolean | Si el mensaje X12 genera error. |
| isa09 | No | String | Fecha de intercambio del documento X12. |
| isa10 | No | String | Hora de intercambio del documento X12. |
| isa11 | No | String | Identificador de los estándares de control del intercambio de X12. |
| isa12 | No | String | Número de versión de control del intercambio de X12. |
| isa14 | No | String | Se solicita la confirmación de X12. |
| isa15 | No | String | Indicador de prueba o producción. |
| isa16 | No | String | Separador de elementos. |
|||||

## <a name="x12-interchange-acknowledgment-tracking-schema"></a>Esquema de seguimiento de confirmación de intercambio de X12

```json
{
   "agreementProperties": {
      "senderPartnerName": "",
      "receiverPartnerName": "",
      "senderQualifier": "",
      "senderIdentifier": "",
      "receiverQualifier": "",
      "receiverIdentifier": "",
      "agreementName": ""
   },
   "messageProperties": {
      "direction": "",
      "interchangeControlNumber": "",
      "isaSegment": "",
      "respondingInterchangeControlNumber": "",
      "isMessageFailed": "",
      "statusCode": "",
      "processingStatus": "",
      "ta102": "",
      "ta103": "",
      "ta105": ""
   }
}
```

| Propiedad | Obligatorio | Tipo | Descripción |
|----------|----------|------|-------------|
| senderPartnerName | No | String | Nombre de asociado del remitente del mensaje X12. |
| receiverPartnerName | No | String | Nombre de asociado del destinatario del mensaje X12. |
| senderQualifier | Sí | String | Calificador del asociado remitente. |
| senderIdentifier | Sí | String | Identificador del asociado remitente. |
| receiverQualifier | Sí | String | Calificador del asociado destinatario. |
| receiverIdentifier | Sí | String | Identificador del asociado destinatario. |
| agreementName | No | String | Nombre del contrato X12 en el que se resuelven los mensajes |
| direction | Sí | Enum | Dirección del flujo de mensajes, que es `receive` o `send`. |
| interchangeControlNumber | No | String | Número de control de intercambio de la confirmación técnica que se recibe de los asociados. |
| isaSegment | No | String | Segmento ISA de la confirmación técnica que se recibe de los asociados. |
| respondingInterchangeControlNumber | No | String | Número de control de intercambio de la confirmación técnica que se recibe de los asociados. |
| isMessageFailed | Sí | Boolean | Si el mensaje X12 genera error. |
| statusCode | Sí | Enum | Código de estado de confirmación de intercambio con estos valores permitidos: `Accepted`, `Rejected` y `AcceptedWithErrors` |
| processingStatus | Sí | Enum | Estado de confirmación con estos valores permitidos: `Received`, `Generated` y `Sent` |
| ta102 | No | String | Fecha de intercambio. |
| ta103 | No | String | Hora de intercambio. |
| ta105 | No | String | Código de nota de intercambio. |
|||||

## <a name="x12-functional-group-tracking-schema"></a>Esquema de seguimiento de grupos funcionales de X12

```json
{
   "agreementProperties": {
      "senderPartnerName": "",
      "receiverPartnerName": "",
      "senderQualifier": "",
      "senderIdentifier": "",
      "receiverQualifier": "",
      "receiverIdentifier": "",
      "agreementName": ""
   },
   "messageProperties": {
      "direction": "",
      "interchangeControlNumber": "",
      "functionalGroupControlNumber": "",
      "gsSegment": "",
      "isTechnicalAcknowledgmentExpected": "",
      "isFunctionalAcknowledgmentExpected": "",
      "isMessageFailed": "",
      "gs01": "",
      "gs02": "",
      "gs03": "",
      "gs04": "",
      "gs05": "",
      "gs07": "",
      "gs08": ""
   }
}
```

| Propiedad | Obligatorio | Tipo | Descripción |
|----------|----------|------|-------------|
| senderPartnerName | No | String | Nombre de asociado del remitente del mensaje X12. |
| receiverPartnerName | No | String | Nombre de asociado del destinatario del mensaje X12. |
| senderQualifier | Sí | String | Calificador del asociado remitente. |
| senderIdentifier | Sí | String | Identificador del asociado remitente. |
| receiverQualifier | Sí | String | Calificador del asociado destinatario. |
| receiverIdentifier | Sí | String | Identificador del asociado destinatario. |
| agreementName | No | String | Nombre del contrato X12 en el que se resuelven los mensajes. |
| direction | Sí | Enum | Dirección del flujo de mensajes, recibidos o enviados. |
| interchangeControlNumber | No | String | Número de control de intercambio. |
| functionalGroupControlNumber | No | String | Número de control funcional. |
| gsSegment | No | String | Segmento GS del mensaje. |
| isTechnicalAcknowledgmentExpected | Sí | Boolean | Si la confirmación técnica se ha configurado en el contrato X12. |
| isFunctionalAcknowledgmentExpected | Sí | Boolean | Si la confirmación funcional se ha configurado en el contrato X12. |
| isMessageFailed | Sí | Boolean | Si el mensaje X12 genera error. |
| gs01 | No | String | Código del identificador funcional. |
| gs02 | No | String | Código del remitente de la aplicación. |
| gs03 | No | String | Código del destinatario de la aplicación. |
| gs04 | No | String | Fecha del grupo funcional. |
| gs05 | No | String | Hora del grupo funcional. |
| gs07 | No | String | Código de la agencia responsable. |
| gs08 | No | String | Código identificador de la versión, el lanzamiento o el sector. |
|||||

## <a name="x12-functional-group-acknowledgment-tracking-schema"></a>Esquema de seguimiento de confirmación de grupos funcionales de X12

```json
{
   "agreementProperties": {
      "senderPartnerName": "",
      "receiverPartnerName": "",
      "senderQualifier": "",
      "senderIdentifier": "",
      "receiverQualifier": "",
      "receiverIdentifier": "",
      "agreementName": ""
   },
   "messageProperties": {
      "direction": "",
      "interchangeControlNumber": "",
      "functionalGroupControlNumber": "",
      "isaSegment": "",
      "gsSegment": "",
      "respondingfunctionalGroupControlNumber": "",
      "respondingFunctionalGroupId": "",
      "isMessageFailed": "",
      "statusCode": "",
      "processingStatus": "",
      "ak903": "",
      "ak904": "",
      "ak9Segment": ""
   }
}
```

| Propiedad | Obligatorio | Tipo | Descripción |
|----------|----------|------|-------------|
| senderPartnerName | No | String | Nombre de asociado del remitente del mensaje X12. |
| receiverPartnerName | No | String | Nombre de asociado del destinatario del mensaje X12. |
| senderQualifier | Sí | String | Calificador del asociado remitente. |
| senderIdentifier | Sí | String | Identificador del asociado remitente. |
| receiverQualifier | Sí | String | Calificador del asociado destinatario. |
| receiverIdentifier | Sí | String | Identificador del asociado destinatario. |
| agreementName | No | String | Nombre del contrato X12 en el que se resuelven los mensajes |
| direction | Sí | Enum | Dirección del flujo de mensajes, que es `receive` o `send`. |
| interchangeControlNumber | No | String | Número de control de intercambio, que se rellena en el lado del envío cuando se recibe una confirmación técnica de los asociados. |
| functionalGroupControlNumber | No | String | Número de control del grupo funcional de la confirmación técnica, que se rellena en el lado de envío cuando se recibe una confirmación técnica de los asociados. |
| isaSegment | No | String | Igual que el número de control de intercambio, pero se rellena solo en casos específicos. |
| gsSegment | No | String | Igual que el número de control del grupo funcional, pero se rellena solo en casos específicos. |
| respondingfunctionalGroupControlNumber | No | String | Número de control del grupo funcional original. |
| respondingFunctionalGroupId | No | String | Se asigna a AK101 en el identificador del grupo funcional de confirmación. |
| isMessageFailed | Boolean | Si el mensaje X12 genera error. |
| statusCode | Sí | Enum | Código de estado de confirmación con estos valores permitidos: `Accepted`, `Rejected` y `AcceptedWithErrors` |
| processingStatus | Sí | Enum | Estado de procesamiento de la confirmación con estos valores permitidos: `Received`, `Generated` y `Sent` |
| ak903 | No | String | Número de conjuntos de transacciones recibidos. |
| ak904 | No | String | Número de conjuntos de transacciones aceptados en el grupo funcional identificado. |
| ak9Segment | No | String | Si el grupo funcional identificado en el segmento AK1 se acepta o se rechaza y por qué. |
|||||

## <a name="b2b-protocol-tracking-schemas"></a>Esquemas de seguimiento de protocolos B2B

Para obtener información sobre los esquemas de seguimiento de protocolos B2B, consulte:

* [Esquemas de seguimiento de AS2](../logic-apps/logic-apps-track-integration-account-as2-tracking-schemas.md)
* [Esquemas de seguimiento personalizado de B2B](logic-apps-track-integration-account-custom-tracking-schema.md)

## <a name="next-steps"></a>Pasos siguientes

* [Supervisión de mensajes B2B con los registros de Azure Monitor](../logic-apps/monitor-b2b-messages-log-analytics.md)