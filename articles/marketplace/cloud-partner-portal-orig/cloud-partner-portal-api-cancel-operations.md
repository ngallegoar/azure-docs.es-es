---
title: API Cancelar operación | Azure Marketplace
description: Cancele las operaciones.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: reference
ms.date: 04/08/2020
ms.author: dsindona
ms.openlocfilehash: f9e55ff2c581f9392a125f6dc3ec8d903e9876a4
ms.sourcegitcommit: 8dc84e8b04390f39a3c11e9b0eaf3264861fcafc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/13/2020
ms.locfileid: "81256440"
---
# <a name="cancel-operation"></a>Cancelar operación

> [!NOTE]
> Las API de Cloud Partner Portal se integran con el Centro de partners y seguirán funcionando después de migrar a él las ofertas. La integración presenta pequeños cambios. Revise los cambios que se muestran en [Referencia de API de Cloud Partner Portal](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/cloud-partner-portal-api-overview) para asegurarse de que el código sigue funcionando después de la migración al Centro de partners.

Esta API cancela una operación en curso en la oferta. Use [Retrieve operations API](./cloud-partner-portal-api-retrieve-operations.md) para obtener `operationId` para pasar a esta API. La cancelación suele ser una operación sincrónica pero, en algunos escenarios complejos, es posible que se necesite una operación nueva para cancelar una existente. En este caso, el cuerpo de la respuesta HTTP contiene la ubicación de la operación que se debe usar para consultar el estado.

  `POST https://cloudpartner.azure.com/api/publishers/<publisherId>/offers/<offerId>/cancel?api-version=2017-10-31`

## <a name="uri-parameters"></a>Parámetros del identificador URI

--------------

|  **Nombre**    |      **Descripción**                                  |    **Tipo de datos**  |
| ------------ |     ----------------                                  |     -----------   |
| publisherId  |  Identificador del editor, por ejemplo, `contoso`         |   String          |
| offerId      |  Identificador de la oferta.                                     |   String          |
| api-version  |  Versión actual de la API                               |    Date           |
|  |  |  |

## <a name="header"></a>Encabezado
------

|  **Nombre**              |  **Valor**         |
|  ---------             |  ----------        |
|  Content-Type          |  application/json  |
|  Authorization         |  SU TOKEN de portador |
|  |  |

## <a name="body-example"></a>Ejemplo de cuerpo
------------

### <a name="request"></a>Solicitud

``` json
{
   "metadata": {
     "notification-emails": "jondoe@contoso.com"
    }
}     
```

### <a name="request-body-properties"></a>Propiedades del cuerpo de la solicitud

|  **Nombre**                |  **Descripción**                                               |
|  --------                |  ---------------                                               |
|  notification-emails     | Lista de identificadores de correo electrónico delimitados por coma para recibir una notificación del progreso de la operación de publicación. |
|  |  |

### <a name="response"></a>Response

#### <a name="migrated-offers"></a>Ofertas migradas

`Location: /api/publishers/contoso/offers/contoso-offer/operations/56615b67-2185-49fe-80d2-c4ddf77bb2e8?api-version=2017-10-31`

#### <a name="non-migrated-offers"></a>Ofertas no migradas

`Location: /api/operations/contoso$contoso-offer$2$preview?api-version=2017-10-31`

### <a name="response-header"></a>Encabezado de respuesta

|  **Nombre**             |    **Valor**                       |
|  ---------            |    ----------                      |
| Location    | La ruta de acceso relativa para recuperar el estado de esta operación. |
|  |  |

### <a name="response-status-codes"></a>Códigos de estado de respuesta

| **Código**  |  **Descripción**                                                                       |
|  ------   |  ------------------------------------------------------------------------               |
|  200      | OK. La solicitud se procesó correctamente y la operación se canceló de manera sincrónica. |
|  202      | Accepted. La solicitud se procesó correctamente y la operación está en proceso de ser cancelada. La ubicación de la operación de cancelación se devuelve en el encabezado de la respuesta. |
|  400      | Bad/Malformed request. El cuerpo de la respuesta del error podría proporcionar más información.  |
|  403      | Access Forbidden. El cliente no tiene acceso al espacio de nombres especificado en la solicitud. |
|  404      | Not found. La entidad especificada no existe. |
|  |  |
