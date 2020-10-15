---
title: Esquemas de seguimiento de AS2 para mensajes B2B
description: Creación de esquemas de seguimiento para supervisar mensajes AS2 en Azure Logic Apps
services: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: jonfan, estfan, logicappspm
ms.topic: article
ms.date: 01/01/2020
ms.openlocfilehash: bccf69362279afd9e8148b20b61ff3ea9b472a03
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "76906958"
---
# <a name="create-schemas-for-tracking-as2-messages-in-azure-logic-apps"></a>Creación de esquemas para el seguimiento de mensajes AS2 en Azure Logic Apps

Para ayudarle a supervisar éxitos, errores y propiedades de mensaje para transacciones de negocio a negocio (B2B), puede usar estos esquemas de seguimiento de AS2 en su cuenta de integración:

* Esquema de seguimiento de mensaje AS2
* Esquema de seguimiento de notificaciones de disposición de mensajes (MDN) AS2

## <a name="as2-message-tracking-schema"></a>Esquema de seguimiento de mensaje AS2

```json
{
   "agreementProperties": {
      "senderPartnerName": "",
      "receiverPartnerName": "",
      "as2To": "",
      "as2From": "",
      "agreementName": ""
   },
   "messageProperties": {
      "direction": "",
      "messageId": "",
      "dispositionType": "",
      "fileName": "",
      "isMessageFailed": "",
      "isMessageSigned": "",
      "isMessageEncrypted": "",
      "isMessageCompressed": "",
      "correlationMessageId": "",
      "incomingHeaders": {},
      "outgoingHeaders": {},
      "isNrrEnabled": "",
      "isMdnExpected": "",
      "mdnType": ""
    }
}
```

| Propiedad | Obligatorio | Tipo | Descripción |
|----------|----------|------|-------------|
| senderPartnerName | No | String | Nombre de asociado del remitente del mensaje AS2. |
| receiverPartnerName | No | String | Nombre de asociado del receptor del mensaje AS2. |
| as2To | Sí | String | Nombre del receptor del mensaje AS2 de los encabezados del mensaje AS2. |
| as2From | Sí | String | Nombre del remitente del mensaje AS2 de los encabezados del mensaje AS2. |
| agreementName | No | String | Nombre del contrato de AS2 en el que se resuelven los mensajes |
| direction | Sí | String | Dirección del flujo de mensajes, que es `receive` o `send`. |
| messageId | No | String | Identificador del mensaje AS2 de los encabezados del mensaje AS2. |
| dispositionType | No | String | Valor del tipo de disposición de notificaciones de disposición del mensaje (MDN). |
| fileName | No | String | Nombre de archivo del encabezado del mensaje AS2. |
| isMessageFailed | Sí | Boolean | Si el mensaje AS2 genera un error. |
| isMessageSigned | Sí | Boolean | Si el mensaje AS2 está firmado. |
| isMessageEncrypted | Sí | Boolean | Si el mensaje AS2 está cifrado. |
| isMessageCompressed | Sí | Boolean | Si el mensaje AS2 está comprimido. |
| correlationMessageId | No | String | Identificador de mensaje AS2, para correlacionar mensajes con MDN. |
| incomingHeaders | No | Diccionario de JToken | Detalles del encabezado del mensaje AS2 entrante. |
| outgoingHeaders | No | Diccionario de JToken | Detalles del encabezado del mensaje AS2 saliente. |
| isNrrEnabled | Sí | Boolean | Si se usará el valor predeterminado cuando se desconoce el valor. |
| isMdnExpected | Sí | Boolean | Si se usará el valor predeterminado cuando se desconoce el valor. |
| mdnType | Sí | Enum | Valores permitidos: `NotConfigured`, `Sync` y `Async`. |
|||||

## <a name="as2-mdn-tracking-schema"></a>Esquema de seguimiento de MDN AS2

```json
{
   "agreementProperties": {
      "senderPartnerName": "",
      "receiverPartnerName": "",
      "as2To": "",
      "as2From": "",
      "agreementName": ""
   },
   "messageProperties": {
      "direction": "",
      "messageId": "",
      "originalMessageId": "",
      "dispositionType": "",
      "isMessageFailed": "",
      "isMessageSigned": "",
      "isNrrEnabled": "",
      "statusCode": "",
      "micVerificationStatus": "",
      "correlationMessageId": "",
      "incomingHeaders": {
      },
      "outgoingHeaders": {
      }
   }
}
```

| Propiedad | Obligatorio | Tipo | Descripción |
|----------|----------|------|-------------|
| senderPartnerName | No | String | Nombre de asociado del remitente del mensaje AS2. |
| receiverPartnerName | No | String | Nombre de asociado del receptor del mensaje AS2. |
| as2To | Sí | String | Nombre del asociado que recibe el mensaje AS2. |
| as2From | Sí | String | Nombre del asociado que envía el mensaje AS2. |
| agreementName | No | String | Nombre del contrato de AS2 en el que se resuelven los mensajes |
| direction | Sí | String | Dirección del flujo de mensajes, que es `receive` o `send`. |
| messageId | No | String | Identificador del mensaje AS2. |
| originalMessageId | No | String | Identificador del mensaje original AS2. |
| dispositionType | No | String | Valor de tipo de disposición de MDN. |
| isMessageFailed | Sí | Boolean | Si el mensaje AS2 genera un error. |
| isMessageSigned | Sí | Boolean | Si el mensaje AS2 está firmado. |
| isNrrEnabled | Sí | Boolean | Si se usará el valor predeterminado cuando se desconoce el valor. |
| statusCode | Sí | Enum | Valores permitidos: `Accepted`, `Rejected` y `AcceptedWithErrors`. |
| micVerificationStatus | Sí | Enum | Valores permitidos:`NotApplicable`, `Succeeded` y `Failed`. |
| correlationMessageId | No | String | Identificador de correlación, que es el identificador del mensaje original con el MDN configurado. |
| incomingHeaders | No | Diccionario de JToken | Detalles del encabezado del mensaje entrante. |
| outgoingHeaders | No | Diccionario de JToken | Detalles del encabezado del mensaje saliente. |
|||||

## <a name="b2b-protocol-tracking-schemas"></a>Esquemas de seguimiento de protocolos B2B

Para obtener información sobre los esquemas de seguimiento de protocolos B2B, consulte:

* [Esquemas de seguimiento de X12](logic-apps-track-integration-account-x12-tracking-schema.md)
* [Esquemas de seguimiento personalizado de B2B](logic-apps-track-integration-account-custom-tracking-schema.md)

## <a name="next-steps"></a>Pasos siguientes

* [Supervisión de mensajes B2B con los registros de Azure Monitor](../logic-apps/monitor-b2b-messages-log-analytics.md)