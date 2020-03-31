---
title: Recuperar el estado de la oferta | Azure Marketplace
description: La API recupera el estado actual de la oferta.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: reference
ms.date: 09/13/2018
ms.author: dsindona
ms.openlocfilehash: 2f5211716145d6c05bbfb0132c4a6ba2f9cceabe
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80280514"
---
<a name="retrieve-offer-status"></a>Recuperación del estado de la oferta 
=====================

Recupera el estado actual de la oferta.

  `GET  https://cloudpartner.azure.com/api/publishers/<publisherId>/offers/<offerId>/status?api-version=2017-10-31`

<a name="uri-parameters"></a>Parámetros del identificador URI
--------------

|  **Nombre**       |   **Descripción**                            |  **Tipo de datos** |
|  -------------  |  ------------------------------------------  |  ------------  |
|  publisherId    | Identificador del publicador, por ejemplo `Contoso`.  |     String     |
|  offerId        | GUID que identifica de manera única la oferta      |     String     |
|  api-version    | Última versión de la API.                        |     Date       |
|  |  |


<a name="header"></a>Encabezado
------

|  NOMBRE           |  Valor               |
|  -------------  | -------------------  |
|  Content-Type   |  `application/json`  |
|  Autorización  | `Bearer YOUR_TOKEN`  |
|  |  |


<a name="body-example"></a>Ejemplo de cuerpo
------------

### <a name="response"></a>Response

``` json
  {
      "status": "succeeded",
      "messages": [],
      "steps": [
      {
          "estimatedTimeFrame": "< 15 min",
          "id": "displaydummycertify",
          "stepName": "Validate Pre-Requisites",
          "description": "Offer settings provided are validated.",
          "status": "complete",
          "messages": [
              {
                  "messageHtml": "Step completed.",
                  "level": "information",
                  "timestamp": "2018-03-16T17:50:45.7215661Z"
              }
          ],       
          "progressPercentage": 100
      },
      {
          "estimatedTimeFrame": "~2-3 days",
          "id": "displaycertify",
          "stepName": "Certification",
          "description": "Your offer is analyzed by our certification systems for issues.",
          "status": "notStarted",
          "messages": [],
          "progressPercentage": 0
      },
      {
          "estimatedTimeFrame": "< 1 day",
          "id": "displayprovision",
          "stepName": "Provisioning",
          "description": "Your virtual machine is being replicated in our production systems.",
          "status": "notStarted",
          "messages": [],
          "progressPercentage": 0
      },
      {
          "estimatedTimeFrame": "< 1 hour",
          "id": "displaypackage",
          "stepName": "Packaging and Lead Generation Registration",
          "description": "Your virtual machine is being packaged for customers. Additionally, lead systems are being configured and set up.",
          "status": "notStarted",
          "messages": [],
          "progressPercentage": 0
      },
      {
          "estimatedTimeFrame": "< 1 hour",
          "id": "publisher-signoff",
          "stepName": "Publisher signoff",
          "description": "Offer is available to preview. Ensure that everything looks good before making your offer live.",
          "status": "complete",
          "messages": [],
          "progressPercentage": 0
      },
      {
          "estimatedTimeFrame": "~2-5 days",
          "id": "live",
          "stepName": "Live",
          "description": "Offer is publicly visible and is available for purchase.",
          "status": "complete",
          "messages": [],
          "progressPercentage": 0
      }
      ],
      "previewLinks": [],
      liveLinks": [],
      "notificationEmails": "jdoe@contoso.com"
  } 
```


### <a name="response-body-properties"></a>Propiedades del cuerpo de la respuesta

|  **Nombre**             |    **Descripción**                                                                             |
| --------------------  |   -------------------------------------------------------------------------------------------- |
|  status               | Estado de la oferta. Para obtener la lista de valores posibles, consulte la sección [Estado de la oferta](#offer-status) a continuación. |
|  messages             | Matriz de mensajes asociados con la oferta                                                    |
|  steps                | Matriz de los pasos por los que pasa la oferta durante una publicación de ofertas                      |
|  estimatedTimeFrame   | Estimación del tiempo que se tardaría en completar este paso, en formato descriptivo                       |
|  id                   | Identificador del paso                                                                         |
|  stepName             | Nombre del paso                                                                               |
|  description          | Descripción del paso                                                                        |
|  status               | Estado del paso. Para obtener la lista de valores posibles, vea la sección [Estado del paso](#step-status) a continuación.    |
|  messages             | Matriz de mensajes relacionados con el paso                                                          |
|  processPercentage    | Porcentaje de finalización del paso                                                              |
|  previewLinks         | *No implementado actualmente*                                                                    |
|  liveLinks            | *No implementado actualmente*                                                                    |
|  notificationEmails   | Lista de direcciones de correo electrónico separadas por comas para recibir una notificación del progreso de la operación        |
|  |  |


### <a name="response-status-codes"></a>Códigos de estado de respuesta

| **(Código:** |   **Descripción**                                                                                 |
| -------  |   ----------------------------------------------------------------------------------------------- |
|  200     |  `OK`: la solicitud se procesó correctamente y se devolvió el estado actual de la oferta. |
|  400     | `Bad/Malformed request`: El cuerpo de la respuesta del error puede contener más información.                 |
|  404     | `Not found`: La entidad especificada no existe.                                                |
|  |  |


### <a name="offer-status"></a>Estado de la oferta

|  **Nombre**                    |    **Descripción**                                       |
|  --------------------------  |  ------------------------------------------------------  |
|  NeverPublished              | La oferta nunca se ha publicado.                          |
|  NotStarted                  | La oferta es nueva y no se ha iniciado.                            |
|  WaitingForPublisherReview   | La oferta está esperando la aprobación del anunciante.                 |
|  En ejecución                     | Se está procesando el envío de la oferta.                     |
|  Correcto                   | Se terminó de procesar el envío de la oferta.               |
|  Canceled                    | Se canceló el envío de la oferta.                           |
|  Con error                      | Error en el envío de la oferta.                                 |
|  |  |


### <a name="step-status"></a>Estado del paso

|  **Nombre**                    |    **Descripción**                           |
|  -------------------------   |  ------------------------------------------  |
|  NotStarted                  | El paso no se inició.                        |
|  InProgress                  | El paso se está ejecutando.                             |
|  WaitingForPublisherReview   | El paso está esperando la aprobación del anunciante.      |
|  WaitingForApproval          | El paso está esperando la aprobación del proceso.        |
|  Bloqueado                     | El paso está bloqueado.                             |
|  Rechazada                    | El paso se rechazó                            |
|  Operación completada                    | El paso se completó.                            |
|  Canceled                    | El paso se canceló.                           |
|  |  |
