---
title: API Recuperación de ofertas| Azure Marketplace
description: La API recupera una lista resumida de ofertas de un espacio de nombres del anunciante.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: reference
ms.date: 04/08/2020
ms.author: dsindona
ms.openlocfilehash: 0413bc71c113bf1eef9f761dbeed4c0d0afe735c
ms.sourcegitcommit: 8dc84e8b04390f39a3c11e9b0eaf3264861fcafc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/13/2020
ms.locfileid: "81255964"
---
<a name="retrieve-offers"></a>Recuperación de ofertas
===============

> [!NOTE]
> Las API de Cloud Partner Portal se integran con el Centro de partners y seguirán funcionando después de migrar las ofertas al Centro de partners. La integración presenta pequeños cambios. Revise los cambios que se muestran en la [referencia de las API de Cloud Partner Portal](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/cloud-partner-portal-api-overview) para asegurarse de que el código sigue funcionando después de la migración al Centro de partners.

Recupera una lista resumida de ofertas de un espacio de nombres del anunciante.

 `GET https://cloudpartner.azure.com/api/publishers/<publisherId>/offers?api-version=2017-10-31`

<a name="uri-parameters"></a>Parámetros del identificador URI
--------------

| **Nombre**         |  **Descripción**                         |  **Tipo de datos** |
| -------------    |  ------------------------------------    |  -----------   |
|  publisherId     | Identificador del publicador, por ejemplo `contoso`. |   String    |
|  api-version     | Última versión de la API.                    |    Date        |
|  |  |


<a name="header"></a>Encabezado
------

|  **Nombre**        |         **Valor**       |
|  --------------- |       ----------------  |
|  Content-Type    | `application/json`      |
|  Authorization   | `Bearer YOUR_TOKEN`     |
|  |  |


<a name="body-example"></a>Ejemplo de cuerpo
------------

### <a name="response"></a>Response

``` json
  200 OK 
  [ 
      {  
          "offerTypeId": "microsoft-azure-virtualmachines",
          "publisherId": "contoso",
          "status": "published",
          "id": "059afc24-07de-4126-b004-4e42a51816fe",
          "version": 1,
          "definition": {
              "displayText": "Contoso Virtual Machine"
          },
          "changedTime":"2017-05-23T23:33:47.8802283Z"
      }
  ]
```

### <a name="response-body-properties"></a>Propiedades del cuerpo de la respuesta

|  **Nombre**       |       **Descripción**                                                                                                  |
|  -------------  |      --------------------------------------------------------------------------------------------------------------    |
|  offerTypeId    | Identifica el tipo de oferta                                                                                           |
|  publisherId    | El identificador que distingue de manera exclusiva al anunciante                                                                      |
|  status         | Estado de la oferta. Para obtener la lista de valores posibles, consulte la sección [Estado de la oferta](#offer-status) a continuación.                         |
|  id             | GUID que identifica de forma exclusiva a la oferta del espacio de nombres del anunciante.                                                    |
|  version        | Versión actual de la oferta. El cliente no puede modificar la propiedad de versión. Se incrementa después de cada publicación. |
|  definición     | Contiene una vista resumida de la definición real de la carga de trabajo. Para obtener una definición detallada, use la API de [Recuperación de una oferta específica](./cloud-partner-portal-api-retrieve-specific-offer.md). |
|  changedTime    | Hora UTC en la que se modificó por última vez la oferta                                                                              |
|  |  |


### <a name="response-status-codes"></a>Códigos de estado de respuesta

| **Código**  |  **Descripción**                                                                                                   |
| -------   |  ----------------------------------------------------------------------------------------------------------------- |
|  200      | `OK`: La solicitud se ha procesado correctamente y todas las ofertas del anunciante se han devuelto al cliente.  |
|  400      | `Bad/Malformed request`: El cuerpo de la respuesta del error puede contener más información.                                    |
|  403      | `Forbidden`: El cliente no tiene acceso al espacio de nombres especificado.                                          |
|  404      | `Not found`: La entidad especificada no existe.                                                                 |
|  |  |


### <a name="offer-status"></a>Estado de la oferta

|  **Nombre**                    | **Descripción**                                  |
|  ------------------------    | -----------------------------------------------  |
|  NeverPublished              | La oferta nunca se ha publicado.                  |
|  NotStarted                  | La oferta es nueva y no se ha iniciado.                 |
|  WaitingForPublisherReview   | La oferta está esperando la aprobación del anunciante.         |
|  En ejecución                     | Se está procesando el envío de la oferta.             |
|  Correcto                   | Se terminó de procesar el envío de la oferta.       |
|  Canceled                    | Se canceló el envío de la oferta.                   |
|  Con error                      | Error en el envío de la oferta.                         |
|  |  |
