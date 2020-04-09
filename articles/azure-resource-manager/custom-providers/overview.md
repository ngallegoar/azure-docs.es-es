---
title: Información general sobre proveedores personalizados
description: Obtenga información sobre los proveedores de recursos personalizados de Azure y cómo ampliar el plano de la API de Azure para que se ajuste a los flujos de trabajo.
author: jjbfour
ms.topic: conceptual
ms.date: 06/19/2019
ms.author: jobreen
ms.openlocfilehash: 68b8bd187d58cd71778b8a922684cc3817a0715d
ms.sourcegitcommit: 632e7ed5449f85ca502ad216be8ec5dd7cd093cb
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/30/2020
ms.locfileid: "80398476"
---
# <a name="azure-custom-resource-providers-overview"></a>Información general sobre los proveedores de recursos personalizados de Azure

Los proveedores de recursos personalizados de Azure son una plataforma de extensibilidad de Azure. Permite definir las API personalizadas que se pueden usar para enriquecer la experiencia predeterminada de Azure. En esta documentación se describe lo siguiente:

- Cómo compilar e implementar un proveedor de recursos personalizados de Azure.
- Cómo utilizar los proveedores de recursos personalizados de Azure para ampliar los flujos de trabajo existentes.
- Dónde encontrar guías y ejemplos de código para empezar a trabajar.

![Información general del proveedor personalizado](./media/overview/overview.png)

> [!IMPORTANT]
> La función de proveedores personalizados está actualmente en versión preliminar pública.
> Esta versión preliminar se ofrece sin Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas.
> Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="what-can-custom-resource-providers-do"></a>Lo que pueden hacer los proveedores de recursos personalizados de Azure

Estos son algunos ejemplos de lo que puede lograr con los proveedores de recursos de Azure personalizados:

- Ampliar la API de REST de Azure Resource Manager para que incluya servicios internos y externos.
- Habilitar escenarios personalizados sobre los flujos de trabajo de Azure existentes.
- Personalizar el control y el efecto de las plantillas de Azure Resource Manager.

## <a name="what-is-a-custom-resource-provider"></a>Qué es un proveedor de recursos personalizados

Los proveedores de recursos personalizados de Azure se realizan mediante la creación de un contrato entre Azure y un punto de conexión. Este contrato define una lista de recursos y acciones nuevos a través de un nuevo recurso, **Microsoft.CustomProviders/resourceProviders**. El proveedor de recursos personalizados después expondrá estas nuevas API en Azure. Los proveedores de recursos personalizados de Azure se componen de tres partes: el proveedor de recursos personalizados, los **puntos de conexión** y los recursos personalizados.

## <a name="how-to-build-custom-resource-providers"></a>Cómo crear proveedores de recursos personalizados de Azure

Los proveedores de recursos personalizados son una lista de contratos entre Azure y los puntos de conexión. Este contrato describe cómo debe interactuar Azure con un punto de conexión. El proveedor de recursos actúa como un proxy y reenvía las solicitudes y respuestas desde y hacia el **punto de conexión** especificado. Un proveedor de recursos puede especificar dos tipos de contratos: [**resourceTypes**](./custom-providers-resources-endpoint-how-to.md) y [**acciones**](./custom-providers-action-endpoint-how-to.md). Estos se habilitan a través de definiciones de punto de conexión. Una definición de punto de conexión está formada por tres campos: **nombre**, **routingType**, y **punto de conexión**.

Punto de conexión de ejemplo:

```JSON
{
  "name": "{endpointDefinitionName}",
  "routingType": "Proxy",
  "endpoint": "https://{endpointURL}/"
}
```

Propiedad | Obligatorio | Descripción
---|---|---
name | *sí* | El nombre de la definición del punto de conexión. Azure expondrá este nombre a través de su API en "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.CustomProviders/<br>resourceProviders/{resourceProviderName}/{endpointDefinitionName}"
routingType | *no* | Determina el tipo de contrato con el **punto de conexión**. Si no se especifica, el valor predeterminado será "Proxy".
endpoint | *sí* | El punto de conexión al que enrutar las solicitudes. Esto controlará la respuesta, así como los efectos secundarios de la solicitud.

### <a name="building-custom-resources"></a>Creación de recursos personalizados

**ResourceTypes** describe los nuevos recursos personalizados que se agregan a Azure. Estos exponen métodos CRUD de RESTful básicos. Obtenga [más información sobre la creación de recursos personalizados](./custom-providers-resources-endpoint-how-to.md).

Ejemplo de proveedor de recursos personalizados con **resourceTypes**:

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
  "location": "eastus"
}
```

API agregadas a Azure para el ejemplo anterior:

HttpMethod | URI de ejemplo | Descripción
---|---|---
PUT | /subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/<br>providers/Microsoft.CustomProviders/resourceProviders/{resourceProviderName}/<br>myCustomResources/{customResourceName}?api-version=2018-09-01-preview | La llamada de API de REST de Azure para crear un nuevo recurso.
Delete | /subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/<br>providers/Microsoft.CustomProviders/resourceProviders/{resourceProviderName}/<br>myCustomResources/{customResourceName}?api-version=2018-09-01-preview | La llamada de API de REST de Azure para eliminar un recurso existente.
GET | /subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/<br>providers/Microsoft.CustomProviders/resourceProviders/{resourceProviderName}/<br>myCustomResources/{customResourceName}?api-version=2018-09-01-preview | La llamada de API de REST de Azure para recuperar un recurso existente.
GET | /subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/<br>providers/Microsoft.CustomProviders/resourceProviders/{resourceProviderName}/<br>myCustomResources?api-version=2018-09-01-preview | La llamada de API de REST de Azure para recuperar la lista de recursos existentes.

### <a name="building-custom-actions"></a>Creación de acciones personalizadas

**Actions** describe las nuevas acciones que se agregan a Azure. Estas se pueden exponer sobre el proveedor de recursos o anidadas bajo un **resourceType**. Obtenga [más información sobre la creación de acciones personalizadas](./custom-providers-action-endpoint-how-to.md).

Ejemplo de proveedor de recursos personalizados con **actions**:

```JSON
{
  "properties": {
    "actions": [
      {
        "name": "myCustomAction",
        "routingType": "Proxy",
        "endpoint": "https://{endpointURL}/"
      }
    ]
  },
  "location": "eastus"
}
```

API agregadas a Azure para el ejemplo anterior:

HttpMethod | URI de ejemplo | Descripción
---|---|---
POST | /subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/<br>providers/Microsoft.CustomProviders/resourceProviders/{resourceProviderName}/<br>myCustomAction?api-version=2018-09-01-preview | La llamada de API de REST de Azure para activar la acción.

## <a name="looking-for-help"></a>¿Busca ayuda?

Si tiene preguntas sobre el desarrollo del proveedor de recursos personalizados de Azure, haga su consulta en [Stack Overflow](https://stackoverflow.com/questions/tagged/azure-custom-providers). Puede que ya se haya formulado y respondido una pregunta similar, así que compruébelo antes de publicar la suya. Agregue la etiqueta ```azure-custom-providers``` para obtener una respuesta rápida.

## <a name="next-steps"></a>Pasos siguientes

En este artículo, ha obtenido información sobre los proveedores personalizados. Vaya al artículo siguiente para crear un proveedor personalizado.

- [Inicio rápido: Creación de un proveedor de recursos personalizados de Azure e implementación de recursos personalizados](./create-custom-provider.md)
- [Tutorial: Creación de acciones y recursos personalizados en Azure](./tutorial-get-started-with-custom-providers.md)
- [Cómo: Adding Custom Actions to Azure REST API](./custom-providers-action-endpoint-how-to.md) (Adición de acciones personalizadas a la API REST de Azure)
- [Cómo: Incorporación de recursos personalizados a la API REST de Azure](./custom-providers-resources-endpoint-how-to.md)
