---
title: API Cancelar operación | Azure Marketplace
description: Cancele las operaciones.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: reference
ms.date: 09/13/2018
ms.author: dsindona
ms.openlocfilehash: 6d4c1f52f0f3b1e05ec06f5a66a36323f346d4eb
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80280548"
---
# <a name="cancel-operation"></a>Cancelar operación 

Esta API cancela una operación en curso en la oferta. Use [Retrieve operations API](./cloud-partner-portal-api-retrieve-operations.md) para obtener `operationId` para pasar a esta API. La cancelación suele ser una operación sincrónica pero, en algunos escenarios complejos, es posible que se necesite una operación nueva para cancelar una existente. En este caso, el cuerpo de la respuesta HTTP contiene la ubicación de la operación que se debe usar para consultar el estado.

Puede proporcionar una lista de direcciones de correo electrónico delimitadas por coma con la solicitud y la API enviará una notificación a estas direcciones sobre el progreso de la operación.

  `POST https://cloudpartner.azure.com/api/publishers/<publisherId>/offers/<offerId>/cancel?api-version=2017-10-31`

<a name="uri-parameters"></a>Parámetros del identificador URI
--------------

|  **Nombre**    |      **Descripción**                                  |    **Tipo de datos**  |
| ------------ |     ----------------                                  |     -----------   |
| publisherId  |  Identificador del editor, por ejemplo, `contoso`         |   String          |
| offerId      |  Identificador de la oferta.                                     |   String          |
| api-version  |  Versión actual de la API                               |    Date           |
|  |  |  |


<a name="header"></a>Encabezado
------

|  **Nombre**              |  **Value**         |
|  ---------             |  ----------        |
|  Content-Type          |  application/json  |
|  Autorización         |  SU TOKEN de portador |
|  |  |


<a name="body-example"></a>Ejemplo de cuerpo
------------

### <a name="request"></a>Request

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

  `Operation-Location: https://cloudpartner.azure.com/api/publishers/contoso/offers/contoso-virtualmachineoffer/operations/56615b67-2185-49fe-80d2-c4ddf77bb2e8`


### <a name="response-header"></a>Encabezado de respuesta

|  **Nombre**             |    **Value**                       |
|  ---------            |    ----------                      |
| Operation-Location    | Dirección URL que se puede consultar para determinar el estado actual de la operación. |
|  |  |


### <a name="response-status-codes"></a>Códigos de estado de respuesta

| **(Código:**  |  **Descripción**                                                                       |
|  ------   |  ------------------------------------------------------------------------               |
|  200      | OK. La solicitud se procesó correctamente y la operación se canceló de manera sincrónica. |
|  202      | Accepted. La solicitud se procesó correctamente y la operación está en proceso de ser cancelada. La ubicación de la operación de cancelación se devuelve en el encabezado de la respuesta. |
|  400      | Bad/Malformed request. El cuerpo de la respuesta del error podría proporcionar más información.  |
|  403      | Access Forbidden. El cliente no tiene acceso al espacio de nombres especificado en la solicitud. |
|  404      | Not found. La entidad especificada no existe. |
|  |  |
