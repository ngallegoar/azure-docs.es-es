---
title: Publicación de una oferta | Azure Marketplace
description: API para publicar la oferta especificada.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: reference
ms.date: 04/08/2020
ms.author: dsindona
ms.openlocfilehash: 960d5facb53f20719045c5fdbe2179f549aca3f2
ms.sourcegitcommit: 8dc84e8b04390f39a3c11e9b0eaf3264861fcafc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/13/2020
ms.locfileid: "81255947"
---
# <a name="publish-an-offer"></a>Publicación de una oferta

> [!NOTE]
> Las API de Cloud Partner Portal se integran con el Centro de partners y seguirán funcionando después de migrar las ofertas al Centro de partners. La integración presenta pequeños cambios. Revise los cambios que se muestran en la [referencia de las API de Cloud Partner Portal](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/cloud-partner-portal-api-overview) para asegurarse de que el código sigue funcionando después de la migración al Centro de partners.

Inicia el proceso de publicación de la oferta especificada. Esta llamada es una operación de larga duración.

  `POST  https://cloudpartner.azure.com/api/publishers/<publisherId>/offers/<offerId>/publish?api-version=2017-10-31`

## <a name="uri-parameters"></a>Parámetros del identificador URI
--------------

|  **Nombre**      |    **Descripción**                               |  **Tipo de datos** |
|  ------------- |  ------------------------------------            |   -----------  |
|  publisherId   | Identificador del publicador, por ejemplo `contoso`.      |   String       |
|  offerId       | Identificador de la oferta.                                 |   String       |
|  api-version   | La versión más reciente de la API                        |   Date         |
|  |  |

## <a name="header"></a>Encabezado
------

|  **Nombre**        |    **Valor**          |
|  --------        |    ---------          |
|  Content-Type    | `application/json`    |
|  Authorization   |  `Bearer YOUR_TOKEN`  |
|  |  |


## <a name="body-example"></a>Ejemplo de cuerpo
------------

### <a name="request"></a>Solicitud

``` json
  { 
      'metadata': 
          { 
              'notification-emails': 'jdoe@contoso.com'
          } 
  }
```

### <a name="request-body-properties"></a>Propiedades del cuerpo de la solicitud

|  **Nombre**               |   **Descripción**                                                                                 |
|  ---------------------  | ------------------------------------------------------------------------------------------------- |
|  notification-emails    | Lista de direcciones de correo electrónico separadas por comas para recibir una notificación del progreso de la operación de publicación. |
|  |  |


### <a name="response"></a>Response

#### <a name="migrated-offers"></a>Ofertas migradas

`Location: /api/publishers/contoso/offers/contoso-offer/operations/56615b67-2185-49fe-80d2-c4ddf77bb2e8?api-version=2017-10-31`

#### <a name="non-migrated-offers"></a>Ofertas no migradas

`Location: /api/operations/contoso$contoso-offer$2$preview?api-version=2017-10-31`


### <a name="response-header"></a>Encabezado de respuesta

|  **Nombre**             |    **Valor**                                                                 |
|  -------------------- | ---------------------------------------------------------------------------- |
| Location    | La ruta de acceso relativa para recuperar el estado de esta operación.     |
|  |  |


### <a name="response-status-codes"></a>Códigos de estado de respuesta

| **Código** |  **Descripción**                                                                                                                           |
| ------   |  ----------------------------------------------------------------------------------------------------------------------------------------- |
| 202   | `Accepted`: La solicitud se aceptó correctamente. La respuesta contiene una ubicación que se puede usar para realizar un seguimiento de la operación que se inicia. |
| 400   | `Bad/Malformed request`: el cuerpo de la respuesta del error puede proporcionar más información.                                                               |
| 422   | `Un-processable entity`: indica que la entidad que se va a publicar no se pudo validar.                                                        |
| 404   | `Not found`: la entidad especificada por el cliente no existe.                                                                              |
|  |  |
