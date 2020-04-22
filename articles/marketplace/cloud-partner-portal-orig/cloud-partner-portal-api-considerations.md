---
title: Consideraciones sobre las API | Azure Marketplace
description: Versiones, control de errores y problemas de autorización con las API de Marketplace.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 04/08/2020
ms.author: dsindona
ms.openlocfilehash: e4d4d5cb16e1037458d09f8c7681ab2d2ecf8676
ms.sourcegitcommit: 8dc84e8b04390f39a3c11e9b0eaf3264861fcafc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/13/2020
ms.locfileid: "81256338"
---
# <a name="api-considerations"></a>Consideraciones sobre las API

<a name="api-versioning"></a>Control de versiones de la API
--------------

> [!NOTE]
> Las API de Cloud Partner Portal se integran con el Centro de partners y seguirán funcionando después de migrar a él las ofertas. La integración presenta pequeños cambios. Revise los cambios que se muestran en [Referencia de API del Cloud Partner Portal](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/cloud-partner-portal-api-overview) para asegurarse de que el código sigue funcionando después de la migración al Centro de partners.

Puede haber varias versiones de la API disponibles al mismo tiempo. Los clientes deben indicar qué versión quieren invocar para usarla proporcionando el parámetro `api-version` como parte de la cadena de consulta.

   `GET https://cloudpartner.azure.com/api/offerTypes?api-version=2017-10-31`

La respuesta a una solicitud con una versión de API desconocida o no válida es un código HTTP 400. Este error devuelve la colección de versiones de API conocidas en el cuerpo de la respuesta.

``` json
    {
        "error": { 
            "code":"InvalidAPIVersion",
            "message":"Invalid api version. Allowed values are [2016-08-01-preview]"
        }
    }
```            

<a name="errors"></a>Errors
------

La API responde a los errores con los códigos de estado HTTP correspondientes y, opcionalmente, información adicional en la respuesta serializada como JSON.
Cuando reciba un error, especialmente un error de clase 400, no vuelva a intentar la solicitud antes de corregir la causa subyacente. Por ejemplo, en la respuesta de ejemplo anterior, corrija el parámetro de la versión de la API antes de volver a enviar la solicitud.

<a name="authorization-header"></a>Encabezado de autorización
--------------------

Para todas las API de esta referencia, debe pasar el encabezado de autorización junto con el token de portador obtenido de Azure Active Directory (Azure AD). Este encabezado se requiere para recibir una respuesta válida. Si no está presente, se devuelve un error `401 Unauthorized`. 

``` HTTP
  GET https://cloudpartner.azure.com/api/offerTypes?api-version=2016-08-01-preview

    Accept: application/json 
    Authorization: Bearer <YOUR_TOKEN> 
```
