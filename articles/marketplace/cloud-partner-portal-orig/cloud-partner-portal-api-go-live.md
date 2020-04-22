---
title: Go Live | Azure Marketplace
description: Go Live API inicia el proceso de publicación de la oferta.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: reference
ms.date: 04/08/2020
ms.author: dsindona
ms.openlocfilehash: ef22f7720a4af2239c55d1a01f9d3f11c878d66e
ms.sourcegitcommit: 8dc84e8b04390f39a3c11e9b0eaf3264861fcafc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/13/2020
ms.locfileid: "81256321"
---
# <a name="go-live"></a>Go Live

> [!NOTE]
> Las API de Cloud Partner Portal se integran con el Centro de partners y seguirán funcionando después de migrar las ofertas al Centro de partners. La integración presenta pequeños cambios. Revise los cambios que se muestran en la [referencia de las API de Cloud Partner Portal](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/cloud-partner-portal-api-overview) para asegurarse de que el código sigue funcionando después de la migración al Centro de partners.

Esta API inicia el proceso para insertar una aplicación en producción. Esta operación es normalmente larga. Esta llamada usa la lista de correos electrónicos de notificación de la operación [Publicar](./cloud-partner-portal-api-publish-offer.md) de la API.

 `POST  https://cloudpartner.azure.com/api/publishers/<publisherId>/offers/<offerId>/golive?api-version=2017-10-31` 

## <a name="uri-parameters"></a>Parámetros del identificador URI
--------------

|  **Nombre**      |   **Descripción**                                                           | **Tipo de datos** |
|  --------      |   ---------------                                                           | ------------- |
| publisherId    | Identificador del anunciante de la oferta que se va a recuperar, por ejemplo `contoso`       |  String       |
| offerId        | Identificador de la oferta que se va a recuperar                                   |  String       |
| api-version    | La versión más reciente de la API                                                   |  Date         |
|  |  |  |

## <a name="header"></a>Encabezado
------

|  **Nombre**       |     **Valor**       |
|  ---------      |     ----------      |
| Content-Type    | `application/json`  |
| Authorization   | `Bearer YOUR_TOKEN` |
|  |  |

## <a name="body-example"></a>Ejemplo de cuerpo

### <a name="response"></a>Response

#### <a name="migrated-offers"></a>Ofertas migradas

`Location: /api/publishers/contoso/offers/contoso-offer/operations/56615b67-2185-49fe-80d2-c4ddf77bb2e8?api-version=2017-10-31`

#### <a name="non-migrated-offers"></a>Ofertas no migradas

`Location: /api/operations/contoso$contoso-offer$2$preview?api-version=2017-10-31`

### <a name="response-header"></a>Encabezado de respuesta

|  **Nombre**             |      **Valor**                                                            |
|  --------             |      ----------                                                           |
| Location    |  La ruta de acceso relativa para recuperar el estado de esta operación.            |
|  |  |

### <a name="response-status-codes"></a>Códigos de estado de respuesta

| **Código** |  **Descripción**                                                                        |
| -------- |  ----------------                                                                        |
|  202     | `Accepted`: La solicitud se aceptó correctamente. La respuesta contiene una ubicación para realizar un seguimiento del estado de la operación. |
|  400     | `Bad/Malformed request`: Puede encontrar información adicional sobre el error en el cuerpo de la respuesta. |
|  404     |  `Not found`: La entidad especificada no existe.                                       |
|  |  |
