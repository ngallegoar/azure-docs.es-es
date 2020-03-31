---
title: Referencia de proxy de recurso personalizado
description: Referencia de proxy de recurso personalizado para los proveedores de recursos personalizados de Azure. En este artículo se analizarán los requerimientos de los puntos de conexión que implementan los recursos personalizados de proxy.
ms.topic: conceptual
ms.author: jobreen
author: jjbfour
ms.date: 06/20/2019
ms.openlocfilehash: 46b38686b39836f3d4bfb80686d514f932a79bf3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "75649200"
---
# <a name="custom-resource-proxy-reference"></a>Referencia de proxy de recurso personalizado

En este artículo se analizarán los requerimientos de los puntos de conexión que implementan los recursos personalizados de proxy. Si no está familiarizado con los proveedores de recursos personalizados de Azure, consulte [la introducción a los proveedores de recursos personalizados](overview.md).

## <a name="how-to-define-a-proxy-resource-endpoint"></a>Definición de un punto de conexión de recursos de proxy

Puede crear un recurso de proxy si especifica **routingType** en "Proxy".

Proveedor de recursos personalizados de ejemplo:

```JSON
{
  "properties": {
    "resourceTypes": [
      {
        "name": "myCustomResources",
        "routingType": "Proxy",
        "endpoint": "https://{endpointURL}/"
      }
    ]
  },
  "location": "eastus",
  "type": "Microsoft.CustomProviders/resourceProviders",
  "id": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.CustomProviders/resourceProviders/{resourceProviderName}",
  "name": "{resourceProviderName}"
}
```

## <a name="building-proxy-resource-endpoint"></a>Creación de un punto de conexión de recursos de proxy

Un **punto de conexión** que implementa un **punto de conexión** de recurso "Proxy" debe controlar la solicitud la respuesta de la API nueva en Azure. En este caso, **resourceType** generará una nueva API de recursos de Azure para que `PUT`, `GET` y `DELETE` ejecuten CRUD en un recurso único, así como para que `GET` recupere todos los recursos existentes.

> [!NOTE]
> Los campos `id`, `name` y `type` no son obligatorios, pero si son necesarios para integrar el recurso personalizado con el ecosistema existente de Azure.

Recurso de ejemplo:

``` JSON
{
    "name": "{myCustomResourceName}",
    "id": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.CustomProviders/resourceProviders/{resourceProviderName}/myCustomResources/{myCustomResourceName}",
    "type": "Microsoft.CustomProviders/resourceProviders/myCustomResources",
    "properties": {
        "myProperty1": "myPropertyValue1",
        "myProperty2": {
            "myProperty3" : "myPropertyValue3"
        }
    }
}
```

Referencia de parámetro:

Propiedad | Muestra | Descripción
---|---|---
name | "{myCustomResourceName}" | El nombre del recurso personalizado.
type | "Microsoft.CustomProviders/resourceProviders/{resourceTypeName}" | El espacio de nombres del tipo de recurso.
id | "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/<br>providers/Microsoft.CustomProviders/resourceProviders/{resourceProviderName}/<br>myCustomResources/{myCustomResourceName}" | El identificador del recurso.

### <a name="create-a-custom-resource"></a>Creación de un recurso personalizado

Solicitud entrante de Azure API:

``` HTTP
PUT https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.CustomProviders/resourceProviders/{resource-provider-name}/myCustomResources/{myCustomResourceName}?api-version=2018-09-01-preview
Authorization: Bearer eyJ0e...
Content-Type: application/json

{
    "properties": {
        "myProperty1": "myPropertyValue1",
        "myProperty2": {
            "myProperty3" : "myPropertyValue3"
        }
    }
}
```

Esta solicitud se reenviará al **punto de conexión** de esta forma:

``` HTTP
PUT https://{endpointURL}/?api-version=2018-09-01-preview
Content-Type: application/json
X-MS-CustomProviders-RequestPath: /subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.CustomProviders/resourceProviders/{resourceProviderName}/myCustomResources/{myCustomResourceName}

{
    "properties": {
        "myProperty1": "myPropertyValue1",
        "myProperty2": {
            "myProperty3" : "myPropertyValue3"
        }
    }
}
```

De manera similar, la respuesta del **punto de conexión** se reenvía de vuelta al cliente. La respuesta del punto de conexión debería devolver:

- Un documento de objeto JSON válido. Todas las matrices y cadenas deben estar anidadas bajo un objeto superior.
- El encabezado `Content-Type` se debe establecer en "application/json; charset=utf-8".

Respuesta del **punto de conexión**:

``` HTTP
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8

{
    "name": "{myCustomResourceName}",
    "id": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.CustomProviders/resourceProviders/{resourceProviderName}/myCustomResources/{myCustomResourceName}",
    "type": "Microsoft.CustomProviders/resourceProviders/myCustomResources",
    "properties": {
        "myProperty1": "myPropertyValue1",
        "myProperty2": {
            "myProperty3" : "myPropertyValue3"
        }
    }
}
```

Respuesta del proveedor de recursos personalizado de Azure:

``` HTTP
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8

{
    "name": "{myCustomResourceName}",
    "id": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.CustomProviders/resourceProviders/{resourceProviderName}/myCustomResources/{myCustomResourceName}",
    "type": "Microsoft.CustomProviders/resourceProviders/myCustomResources",
    "properties": {
        "myProperty1": "myPropertyValue1",
        "myProperty2": {
            "myProperty3" : "myPropertyValue3"
        }
    }
}
```

### <a name="remove-a-custom-resource"></a>Eliminación de un recurso personalizado

Solicitud entrante de Azure API:

``` HTTP
Delete https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.CustomProviders/resourceProviders/{resourceProviderName}/myCustomResources/{myCustomResourceName}?api-version=2018-09-01-preview
Authorization: Bearer eyJ0e...
Content-Type: application/json
```

Esta solicitud se reenviará al **punto de conexión** de esta forma:

``` HTTP
Delete https://{endpointURL}/?api-version=2018-09-01-preview
Content-Type: application/json
X-MS-CustomProviders-RequestPath: /subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.CustomProviders/resourceProviders/{resourceProviderName}/myCustomResources/{myCustomResourceName}
```

De manera similar, la respuesta del **punto de conexión** se reenvía de vuelta al cliente. La respuesta del punto de conexión debería devolver:

- Un documento de objeto JSON válido. Todas las matrices y cadenas deben estar anidadas bajo un objeto superior.
- El encabezado `Content-Type` se debe establecer en "application/json; charset=utf-8".

Respuesta del **punto de conexión**:

``` HTTP
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
```

Respuesta del proveedor de recursos personalizado de Azure:

``` HTTP
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
```

### <a name="retrieve-a-custom-resource"></a>Recuperación de un recurso personalizado

Solicitud entrante de Azure API:

``` HTTP
GET https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.CustomProviders/resourceProviders/{resourceProviderName}/myCustomResources/{myCustomResourceName}?api-version=2018-09-01-preview
Authorization: Bearer eyJ0e...
Content-Type: application/json
```

Esta solicitud se reenviará al **punto de conexión** de esta forma:

``` HTTP
GET https://{endpointURL}/?api-version=2018-09-01-preview
Content-Type: application/json
X-MS-CustomProviders-RequestPath: /subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.CustomProviders/resourceProviders/{resourceProviderName}/myCustomResources/{myCustomResourceName}
```

De manera similar, la respuesta del **punto de conexión** se reenvía de vuelta al cliente. La respuesta del punto de conexión debería devolver:

- Un documento de objeto JSON válido. Todas las matrices y cadenas deben estar anidadas bajo un objeto superior.
- El encabezado `Content-Type` se debe establecer en "application/json; charset=utf-8".

Respuesta del **punto de conexión**:

``` HTTP
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8

{
    "name": "{myCustomResourceName}",
    "id": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.CustomProviders/resourceProviders/{resourceProviderName}/myCustomResources/{myCustomResourceName}",
    "type": "Microsoft.CustomProviders/resourceProviders/myCustomResources",
    "properties": {
        "myProperty1": "myPropertyValue1",
        "myProperty2": {
            "myProperty3" : "myPropertyValue3"
        }
    }
}
```

Respuesta del proveedor de recursos personalizado de Azure:

``` HTTP
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8

{
    "name": "{myCustomResourceName}",
    "id": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.CustomProviders/resourceProviders/{resourceProviderName}/myCustomResources/{myCustomResourceName}",
    "type": "Microsoft.CustomProviders/resourceProviders/myCustomResources",
    "properties": {
        "myProperty1": "myPropertyValue1",
        "myProperty2": {
            "myProperty3" : "myPropertyValue3"
        }
    }
}
```

### <a name="enumerate-all-custom-resources"></a>Enumeración de todos los recursos personalizados

Solicitud entrante de Azure API:

``` HTTP
GET https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.CustomProviders/resourceProviders/{resourceProviderName}/myCustomResources?api-version=2018-09-01-preview
Authorization: Bearer eyJ0e...
Content-Type: application/json
```

Esta solicitud se reenviará al **punto de conexión** de esta forma:

``` HTTP
GET https://{endpointURL}/?api-version=2018-09-01-preview
Content-Type: application/json
X-MS-CustomProviders-RequestPath: /subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.CustomProviders/resourceProviders/{resourceProviderName}/myCustomResources
```

De manera similar, la respuesta del **punto de conexión** se reenvía de vuelta al cliente. La respuesta del punto de conexión debería devolver:

- Un documento de objeto JSON válido. Todas las matrices y cadenas deben estar anidadas bajo un objeto superior.
- El encabezado `Content-Type` se debe establecer en "application/json; charset=utf-8".
- La lista de recursos se debe colocar bajo la propiedad `value` de nivel superior.

Respuesta del **punto de conexión**:

``` HTTP
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8

{
    "value" : [
        {
            "name": "{myCustomResourceName}",
            "id": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.CustomProviders/resourceProviders/{resourceProviderName}/myCustomResources/{myCustomResourceName}",
            "type": "Microsoft.CustomProviders/resourceProviders/myCustomResources",
            "properties": {
                "myProperty1": "myPropertyValue1",
                "myProperty2": {
                    "myProperty3" : "myPropertyValue3"
                }
            }
        }
    ]
}
```

Respuesta del proveedor de recursos personalizado de Azure:

``` HTTP
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8

{
    "value" : [
        {
            "name": "{myCustomResourceName}",
            "id": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.CustomProviders/resourceProviders/{resourceProviderName}/myCustomResources/{myCustomResourceName}",
            "type": "Microsoft.CustomProviders/resourceProviders/myCustomResources",
            "properties": {
                "myProperty1": "myPropertyValue1",
                "myProperty2": {
                    "myProperty3" : "myPropertyValue3"
                }
            }
        }
    ]
}
```

## <a name="next-steps"></a>Pasos siguientes

- [Introducción a los proveedores de recursos personalizados de Azure](overview.md)
- [Inicio rápido: Creación de un proveedor de recursos personalizados de Azure e implementación de recursos personalizados](./create-custom-provider.md)
- [Tutorial: Creación de acciones y recursos personalizados en Azure](./tutorial-get-started-with-custom-providers.md)
- [Cómo: Adding Custom Actions to Azure REST API](./custom-providers-action-endpoint-how-to.md) (Adición de acciones personalizadas a la API REST de Azure)
- [Referencia: Custom Resource Cache Reference](proxy-cache-resource-endpoint-reference.md) (Referencia: Referencia de caché de recurso personalizado)
