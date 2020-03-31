---
title: Incorporación de recursos
description: Obtenga información sobre cómo realizar la incorporación de recursos mediante el uso de proveedores personalizados de Azure para aplicar la administración o la configuración a otros tipos de recursos de Azure.
author: jjbfour
ms.topic: conceptual
ms.date: 09/06/2019
ms.author: jobreen
ms.openlocfilehash: 1846b036f12fe7e691021ec0248782cad946d9b7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "75649184"
---
# <a name="azure-custom-providers-resource-onboarding-overview"></a>Información general sobre la incorporación de recursos de proveedores personalizados de Azure

La incorporación de recursos de proveedores personalizados de Azure es un modelo de extensibilidad para los tipos de recursos de Azure. Permite aplicar operaciones o administración entre recursos de Azure existentes a escala. Para obtener más información, vea [Cómo los proveedores personalizados de Azure pueden ampliar Azure](overview.md). En este artículo se describe:

- Qué puede hacer la incorporación de recursos.
- Aspectos básicos de la incorporación de recursos y cómo usarla.
- Dónde encontrar guías y ejemplos de código para empezar a trabajar.

> [!IMPORTANT]
> La función de proveedores personalizados está actualmente en versión preliminar pública.
> Esta versión preliminar se ofrece sin un Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan las capacidades limitadas.
> Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="what-can-resource-onboarding-do"></a>¿Qué puede hacer la incorporación de recursos?

De forma similar a los [recursos personalizados de proveedores personalizados de Azure](./custom-providers-resources-endpoint-how-to.md), la incorporación de recursos define un contrato que realizará el proxy de las solicitudes de "incorporación" a un punto de conexión. A diferencia de los recursos personalizados, la incorporación de recursos no crea un tipo de recurso. En su lugar, permite la extensión de los tipos de recursos existentes. Y la incorporación de recursos funciona con Azure Policy, por lo que la administración y la configuración de los recursos se pueden realizar a escala. Algunos ejemplos de flujos de trabajo de incorporación de recursos:

- Creación y administración de extensiones de máquinas virtuales.
- Carga y configuración de valores predeterminados en cuentas de Azure Storage.
- Habilitación de la configuración de diagnóstico de línea de base a escala.

## <a name="resource-onboarding-basics"></a>Aspectos básicos de la incorporación de recursos

Configure la incorporación de recursos a través de proveedores personalizados de Azure mediante los tipos de recursos Microsoft.CustomProviders/resourceProviders y Microsoft.CustomProviders/associations. Con el fin de habilitar la incorporación de recursos para un proveedor personalizado, durante el proceso de configuración, cree un **resourceType** denominado "associations" con un **routingType** que incluya "Extension". No es necesario que Microsoft.CustomProviders/associations y Microsoft.CustomProviders/resourceProviders pertenezcan al mismo grupo de recursos.

Este es un proveedor personalizado de Azure de ejemplo:

```JSON
{
  "properties": {
    "resourceTypes": [
      {
        "name": "associations",
        "routingType": "Proxy,Cache,Extension",
        "endpoint": "https://microsoft.com/"
      }
    ]
  },
  "location": "eastus"
}
```

Propiedad | ¿Necesario? | Descripción
---|---|---
name | Sí | El nombre de la definición del punto de conexión. Para la incorporación de recursos, el nombre debe ser "associations".
routingType | Sí | Determina el tipo de contrato con el punto de conexión. Para la incorporación de recursos, los valores **routingTypes** válidos son "Proxy,Cache,Extension" y "Webhook,Cache,Extension".
endpoint | Sí | El punto de conexión al que enrutar las solicitudes. Esto controlará la respuesta, así como los efectos secundarios de la solicitud.

Después de crear el proveedor personalizado con el tipo de recurso "associations", puede usar Microsoft. CustomProviders/associations como destino. Microsoft.CustomProviders/associations es un recurso de extensión que puede ampliar cualquier otro recurso de Azure. Cuando se crea una instancia de Microsoft.CustomProviders/associations, tomará una propiedad **targetResourceId**, que debe ser un identificador de recurso válido de Microsoft.CustomProviders/resourceProviders o Microsoft.Solutions/applications. En estos casos, la solicitud se reenviará al tipo de recurso "associations" en la instancia de Microsoft.CustomProviders/resourceProviders que se ha creado.

> [!NOTE]
> Si se proporciona un identificador de recurso de Microsoft.Solutions/applications como **targetResourceId**, se debe haber implementado Microsoft.CustomProviders/resourceProviders en el grupo de recursos administrados con el nombre "public".

Ejemplo de asociación de proveedores personalizados de Azure:

```JSON
{
  "properties": {
    "targetResourceId": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.CustomProviders/resourceProviders/{resourceProviderName}",
    ...
  }
}
```

Propiedad | ¿Necesario? | Descripción
---|---|---
targetResourceId | Sí | El identificador de recurso de Microsoft.CustomProviders/resourceProviders o Microsoft.Solutions/applications.

## <a name="how-to-use-resource-onboarding"></a>Cómo se usa la incorporación de recursos

La incorporación de recursos funciona con la ampliación de otros recursos con el recurso de extensión Microsoft.CustomProviders/associations. En el ejemplo siguiente, la solicitud se realiza para una máquina virtual, pero se puede ampliar cualquier recurso.

En primer lugar, debe crear un recurso de proveedor personalizado con un tipo de recurso "associations". Esto declarará la URL de devolución de llamada que se usará cuando se cree un recurso de Microsoft.CustomProviders/associations correspondiente, que tiene como destino el proveedor personalizado.

Ejemplo de solicitud de creación de Microsoft.CustomProviders/resourceProviders:

``` HTTP
PUT https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.CustomProviders/resourceProviders/{resourceProviderName}?api-version=2018-09-01-preview
Authorization: Bearer eyJ0e...
Content-Type: application/json

{
  "properties": {
    "resourceTypes": [
      {
        "name": "associations",
        "routingType": "Proxy,Cache,Extension",
        "endpoint": "https://{myCustomEndpoint}/"
      }
    ]
  },
  "location": "{location}"
}
```

Después de crear el proveedor personalizado, puede tener como destino otros recursos y aplicarles los efectos secundarios del proveedor personalizado.

Ejemplo de solicitud de creación de Microsoft.CustomProviders/associations:

``` HTTP
PUT https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Compute/virtualMachines/{virtualMachineName}/providers/Microsoft.CustomProviders/associations/{associationName}?api-version=2018-09-01-preview
Authorization: Bearer eyJ0e...
Content-Type: application/json

{
  "properties": {
    "targetResourceId": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.CustomProviders/resourceProviders/{resourceProviderName}",
    "myProperty1": "myPropertyValue1",
    "myProperty2": {
        "myProperty3" : "myPropertyValue3"
    }
  }
}
```

Esta solicitud se reenviará al punto de conexión especificado en el proveedor personalizado que se ha creado, al que **targetResourceId** hace referencia de esta forma:

``` HTTP
PUT https://{endpointURL}/?api-version=2018-09-01-preview
Content-Type: application/json
X-MS-CustomProviders-RequestPath: /subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.CustomProviders/resourceProviders/{resourceProviderName}/associations/{associationName}
X-MS-CustomProviders-ExtensionPath: /subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Compute/virtualMachines/{virtualMachineName}/providers/Microsoft.CustomProviders/associations/{associationName}
X-MS-CustomProviders-ExtendedResource: /subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Compute/virtualMachines/{virtualMachineName}

{
  "properties": {
    "myProperty1": "myPropertyValue1",
    "myProperty2": {
        "myProperty3" : "myPropertyValue3"
    }
  }
}
```

El punto de conexión debe responder con un valor `Content-Type` de aplicación o JSON, y un cuerpo de respuesta JSON válido. Los campos que se devuelven en el objeto **properties** del código JSON se agregarán a la respuesta de devolución de la asociación.

## <a name="getting-help"></a>Ayuda

Si tiene preguntas sobre el desarrollo de proveedores de recursos personalizados de Azure, haga su consulta en [Stack Overflow](https://stackoverflow.com/questions/tagged/azure-custom-providers). Puede que ya se haya respondido a una pregunta similar, así que compruébelo antes de publicar la suya. Agregue la etiqueta ```azure-custom-providers``` para obtener una respuesta rápida.

## <a name="next-steps"></a>Pasos siguientes

En este artículo, ha obtenido información sobre los proveedores personalizados. Para obtener más información, vea estos artículos:

- [Tutorial: Incorporación de recursos con proveedores personalizados](./tutorial-resource-onboarding.md)
- [Tutorial: Creación de acciones y recursos personalizados en Azure](./tutorial-get-started-with-custom-providers.md)
- [Inicio rápido: Creación de un proveedor de recursos personalizados e implementación de recursos personalizados](./create-custom-provider.md)
- [Cómo: Incorporación de acciones personalizadas a una API REST de Azure](./custom-providers-action-endpoint-how-to.md)
- [Cómo: Incorporación de recursos personalizados a una API REST de Azure](./custom-providers-resources-endpoint-how-to.md)
