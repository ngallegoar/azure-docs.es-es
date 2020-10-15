---
title: Esquemas de seguimiento personalizados para mensajes B2B
description: Creación de esquemas de seguimiento personalizados para supervisar mensajes B2B en Azure Logic Apps
services: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: jonfan, estfan, logicappspm
ms.topic: article
ms.date: 01/01/2020
ms.openlocfilehash: c82f9cbfaf2e23ddaa5e4b05f4aac4795d3e16a9
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "76903058"
---
# <a name="create-custom-tracking-schemas-that-monitor-end-to-end-workflows-in-azure-logic-a"></a>Creación de esquemas de seguimiento personalizado que supervisen los flujos de trabajo de un extremo a otro en Azure Logic Apps

Azure Logic Apps tiene seguimiento integrado que se puede habilitar para las partes del flujo de trabajo. Sin embargo, puede configurar un seguimiento personalizado que registre los eventos desde el principio hasta el fin del flujo de trabajo, por ejemplo, flujos de trabajo que incluyan una aplicación lógica, BizTalk Server, SQL Server y cualquier otra capa. En este artículo se proporciona un código personalizado que puede usar en las capas externas de la aplicación lógica.

## <a name="custom-tracking-schema"></a>Esquema de seguimiento personalizado

```json
{
   "sourceType": "",
   "source": {
      "workflow": {
         "systemId": ""
      },
      "runInstance": {
         "runId": ""
      },
      "operation": {
         "operationName": "",
         "repeatItemScopeName": "",
         "repeatItemIndex": ,
         "trackingId": "",
         "correlationId": "",
         "clientRequestId": ""
      }
   },
   "events": [
      {
         "eventLevel": "",
         "eventTime": "",
         "recordType": "",
         "record": {}
      }
   ]
}
```

| Propiedad | Obligatorio | Tipo | Descripción |
|----------|----------|------|-------------|
| sourceType | Sí | String | Tipo del origen de ejecución con estos valores permitidos: `Microsoft.Logic/workflows`, `custom` |
| source | Sí | String o JToken | Si el tipo de origen es `Microsoft.Logic/workflows`, la información de origen debe seguir este esquema. Si el tipo de origen es `custom`, el esquema es un JToken. |
| systemId | Sí | String | Identificador del sistema de la aplicación lógica |
| runId | Sí | String | Identificador de ejecución de la aplicación lógica |
| operationName | Sí | String | Nombre de la operación (por ejemplo, acción o desencadenador) |
| repeatItemScopeName | Sí | String | Repita el nombre del elemento si la acción se encuentra dentro de un bucle `foreach` o `until`. |
| repeatItemIndex | Sí | Entero | Indica que la acción está dentro de un bucle `foreach` o `until` y es el número de índice del elemento repetido. |
| trackingId | No | String | Identificador de seguimiento para correlacionar los mensajes |
| correlationId | No | String | Identificador de correlación para correlacionar los mensajes |
| clientRequestId | No | String | El cliente puede rellenar esta propiedad para correlacionar los mensajes |
| eventLevel | Sí | String | Nivel del evento |
| eventTime | Sí | DateTime | Hora del evento en formato UTC: *AAAA-MM-DDTHH:MM:SS.00000Z* |
| recordType | Sí | String | Tipo de registro de seguimiento con solo este valor permitido: `custom` |
| registro | Sí | JToken | Tipo de registro personalizado solo con formato JToken |
|||||

## <a name="b2b-protocol-tracking-schemas"></a>Esquemas de seguimiento de protocolos B2B

Para obtener información sobre los esquemas de seguimiento de protocolos B2B, consulte:

* [Esquemas de seguimiento de AS2](../logic-apps/logic-apps-track-integration-account-as2-tracking-schemas.md)
* [Esquemas de seguimiento de X12](logic-apps-track-integration-account-x12-tracking-schema.md)

## <a name="next-steps"></a>Pasos siguientes

* Más información sobre la [supervisión de mensajes B2B con registros de Azure Monitor](../logic-apps/monitor-b2b-messages-log-analytics.md)