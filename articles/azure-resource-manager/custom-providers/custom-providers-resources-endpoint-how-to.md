---
title: Adición de recursos personalizados a la API REST de Azure
description: Obtenga información sobre cómo agregar recursos personalizados a la API REST de Azure. En este artículo le guiaremos a través de los requisitos y procedimientos recomendados para los puntos de conexión que quieren implementar recursos personalizados.
ms.topic: conceptual
ms.author: jobreen
author: jjbfour
ms.date: 06/20/2019
ms.openlocfilehash: b6c5f5b8e437ad2dc2e8a3be3f3f2ed03a613b44
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "75649220"
---
# <a name="adding-custom-resources-to-azure-rest-api"></a>Adición de recursos personalizados a la API REST de Azure

Este artículo le guiará a través de los requisitos y procedimientos recomendados para crear puntos de conexión del proveedor de recursos personalizados de Azure que implementa recursos personalizados. Si no está familiarizado con los proveedores de recursos personalizados de Azure, consulte [la introducción a los proveedores de recursos personalizados](overview.md).

## <a name="how-to-define-a-resource-endpoint"></a>Cómo definir un punto de conexión de recursos

Un **punto de conexión** es una dirección URL que apunta a un servicio que implementa el contrato subyacente entre él y Azure. El punto de conexión se define en el proveedor de recursos personalizados y puede ser cualquier dirección URL accesible públicamente. El ejemplo siguiente tiene un **resourceType** denominado `myCustomResource` implementado por `endpointURL`.

**ResourceProvider** de ejemplo:

```JSON
{
  "properties": {
    "resourceTypes": [
      {
        "name": "myCustomResource",
        "routingType": "Proxy, Cache",
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

## <a name="building-a-resource-endpoint"></a>Compilación de un punto de conexión de recursos

Un **punto de conexión** que implementa un **resourceType** debe controlar la solicitud y la respuesta de la nueva API en Azure. Cuando se crea un proveedor de recursos personalizados con un **resourceType**, generará un nuevo conjunto de API en Azure. En este caso, **resourceType** generará una nueva API de recursos de Azure para que `PUT`, `GET` y `DELETE` ejecuten CRUD en un recurso único, así como para que `GET` recupere todos los recursos existentes:

Manipulación del recurso único (`PUT`, `GET` y `DELETE`):

``` JSON
/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.CustomProviders/resourceProviders/{resourceProviderName}/myCustomResource/{myCustomResourceName}
```

Recuperación de todos los recursos (`GET`):

``` JSON
/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.CustomProviders/resourceProviders/{resourceProviderName}/myCustomResource
```

Para los recursos personalizados, los proveedores de recursos personalizados ofrecen dos tipos de **routingTypes**: "`Proxy`" y "`Proxy, Cache`".

### <a name="proxy-routing-type"></a>Tipo de enrutamiento Proxy

**routingType** "`Proxy`" redirige mediante proxy todos los métodos de solicitud para el **punto de conexión** especificado en el proveedor de recursos personalizados. Casos en los que se debe usar "`Proxy`":

- Se necesita un control total sobre la respuesta.
- Sistemas de integración con los recursos existentes.

Para obtener más información acerca de los recursos "`Proxy`", consulte la [referencia de proxy de recurso personalizado](proxy-resource-endpoint-reference.md).

### <a name="proxy-cache-routing-type"></a>Tipo de enrutamiento Caché de proxy

**routingType** "`Proxy, Cache`" solo redirige mediante proxy los métodos de solicitud `PUT` y `DELETE` para el **punto de conexión** especificado en el proveedor de recursos personalizado. El proveedor de recursos personalizado devolverá automáticamente solicitudes `GET` en función de lo que ha almacenado en su caché. Si un recurso personalizado está marcado con la memoria caché, el proveedor de recursos personalizados también agregará o sobrescribirá los campos en la respuesta para hacerlos compatibles con las API de Azure. Casos en los que se debe usar "`Proxy, Cache`":

- Creación de un nuevo sistema que no tenga ningún recurso existente.
- Funcionamiento con el ecosistema de Azure existente.

Para obtener más información acerca de los recursos "`Proxy, Cache`", consulte la [referencia de caché de recurso personalizado](proxy-cache-resource-endpoint-reference.md).

## <a name="creating-a-custom-resource"></a>Creación de un recurso personalizado

Hay dos formas principales de crear un recurso personalizado fuera de un proveedor de recursos personalizados:

- Azure CLI
- Plantillas del Administrador de recursos de Azure

### <a name="azure-cli"></a>Azure CLI

Cree un recurso personalizado:

```azurecli-interactive
az resource create --is-full-object \
                   --id /subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.CustomProviders/resourceProviders/{resourceProviderName}/{resourceTypeName}/{customResourceName} \
                   --properties \
                    '{
                        "location": "eastus",
                        "properties": {
                            "myProperty1": "myPropertyValue1",
                            "myProperty2": {
                                "myProperty3": "myPropertyValue3"
                            }
                        }
                    }'
```

Parámetro | Obligatorio | Descripción
---|---|---
is-full-object | *sí* | Indica que el objeto properties incluye otras opciones, como ubicación, etiquetas, sku o plan.
id | *sí* | Id. de recurso del recurso personalizado. Debería existir fuera de **ResourceProvider**.
properties | *sí* | El cuerpo de la solicitud que se enviará al **punto de conexión**.

Elimine un recurso personalizado de Azure:

```azurecli-interactive
az resource delete --id /subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.CustomProviders/resourceProviders/{resourceProviderName}/{resourceTypeName}/{customResourceName}
```

Parámetro | Obligatorio | Descripción
---|---|---
id | *sí* | Id. de recurso del recurso personalizado. Debería existir fuera de **ResourceProvider**.

Recupere un recurso personalizado de Azure:

```azurecli-interactive
az resource show --id /subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.CustomProviders/resourceProviders/{resourceProviderName}/{resourceTypeName}/{customResourceName}
```

Parámetro | Obligatorio | Descripción
---|---|---
id | *sí* | Id. de recurso del recurso personalizado. Debería existir fuera de **ResourceProvider**.

### <a name="azure-resource-manager-template"></a>Plantilla de Azure Resource Manager

> [!NOTE]
> Los recursos requieren que la respuesta contenga una instancia adecuada de `id`, `name` y `type` del **punto de conexión**.

Las plantillas de Azure Resource Manager requieren que `id`, `name` y `type` se devuelvan correctamente desde el punto de conexión de bajada. Una respuesta del recurso devuelto debería tener el siguiente formato:

Ejemplo de respuesta del **punto de conexión**:

``` JSON
{
  "properties": {
    "myProperty1": "myPropertyValue1",
    "myProperty2": {
        "myProperty3": "myPropertyValue3"
    }
  },
  "id": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.CustomProviders/resourceProviders/{customResourceName}",
  "name": "{customResourceName}",
  "type": "Microsoft.CustomProviders/resourceProviders/{resourceTypeName}"
}
```

Plantilla de Azure Resource Manager de ejemplo:

```JSON
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [
        {
            "type": "Microsoft.CustomProviders/resourceProviders/{resourceTypeName}",
            "name": "{resourceProviderName}/{customResourceName}",
            "apiVersion": "2018-09-01-preview",
            "location": "eastus",
            "properties": {
                "myProperty1": "myPropertyValue1",
                "myProperty2": {
                    "myProperty3": "myPropertyValue3"
                }
            }
        }
    ]
}
```

Parámetro | Obligatorio | Descripción
---|---|---
resourceTypeName | *sí* | **Nombre** del parámetro **resourceType** definido en el proveedor personalizado.
resourceProviderName | *sí* | Nombre de la instancia del proveedor de recursos personalizados.
customResourceName | *sí* | Nombre del recurso personalizado.

## <a name="next-steps"></a>Pasos siguientes

- [Introducción a los proveedores de recursos personalizados de Azure](overview.md)
- [Inicio rápido: Creación de un proveedor de recursos personalizados de Azure e implementación de recursos personalizados](./create-custom-provider.md)
- [Tutorial: Creación de acciones y recursos personalizados en Azure](./tutorial-get-started-with-custom-providers.md)
- [Cómo: Adding Custom Actions to Azure REST API](./custom-providers-action-endpoint-how-to.md) (Adición de acciones personalizadas a la API REST de Azure)
- [Referencia: Custom Resource Proxy Reference](proxy-resource-endpoint-reference.md) (Referencia: Referencia de proxy de recurso personalizado)
- [Referencia: Custom Resource Cache Reference](proxy-cache-resource-endpoint-reference.md) (Referencia: Referencia de caché de recurso personalizado)
