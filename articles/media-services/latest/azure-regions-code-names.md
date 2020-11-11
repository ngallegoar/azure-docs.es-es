---
title: Nubes y regiones en las que Azure Media Services v3 está disponible
description: En este artículo se tratan las direcciones URL que se usan para los puntos de conexión y el código de las regiones.
services: media-services
author: IngridAtMicrosoft
manager: femila
ms.service: media-services
ms.topic: reference
ms.date: 10/28/2020
ms.author: inhenkel
ms.openlocfilehash: c17eeda91af46c4159f7c4b1747416298f24c703
ms.sourcegitcommit: 96918333d87f4029d4d6af7ac44635c833abb3da
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/04/2020
ms.locfileid: "93319634"
---
# <a name="regional-code-names-and-endpoints"></a>Nombres de código y puntos de conexión regionales

### <a name="region-code-name"></a>Nombre de código de región

Al usar el parámetro **location** en un comando o una solicitud, deberá proporcionar el nombre de código de la región como valor de **location**. Para obtener el nombre de código de la región en la que está su cuenta y a la que se debe enrutar la llamada, puede ejecutar la siguiente línea en la CLI de Azure.

```azurecli-interactive
az account list-locations
```

Una vez que ejecuta la línea mostrada anteriormente, obtiene una lista de todas las regiones de Azure. Vaya a la región de Azure que tenga el elemento *displayName* que busca y use su valor *name* para el parámetro **location**.

Por ejemplo, para la región Oeste de EE. UU. 2 de Azure (se muestra a continuación), usará "westus2" para el parámetro **location**.

```json
   {
      "displayName": "West US 2",
      "id": "/subscriptions/00000000-23da-4fce-b59c-f6fb9513eeeb/locations/westus2",
      "latitude": "47.233",
      "longitude": "-119.852",
      "name": "westus2",
      "subscriptionId": null
    }
```

## <a name="endpoints"></a>Puntos de conexión  

Es importante conocer los siguientes puntos de conexión al conectarse a cuentas de Media Services desde diferentes nubes nacionales de Azure.

### <a name="global-azure"></a>Azure global

| Servicio | Punto de conexión |
| ------- | -------- |
| Azure Resource Manager |  `https://management.azure.com/` |
| Authentication | `https://login.microsoftonline.com/` |
| Público del token | `https://management.core.windows.net/` |

### <a name="azure-government"></a>Azure Government

| Servicio | Punto de conexión |
| ------- | -------- |
| Azure Resource Manager |  `https://management.usgovcloudapi.net/` |
| Authentication | `https://login.microsoftonline.us/` |
| Público del token | `https://management.core.usgovcloudapi.net/` |

[!INCLUDE [Widevine is not available in the GovCloud region.](./includes/widevine-not-available-govcloud.md)]

### <a name="azure-germany"></a>Azure Alemania

> [!NOTE]
> Los puntos de conexión de Azure Alemania solo se aplican a las nubes soberanas de Alemania.

| Servicio | Punto de conexión |
| ------- | -------- |
| Azure Resource Manager | `https://management.cloudapi.de/` |
| Authentication | `https://login.microsoftonline.de/` |
| Público del token | `https://management.core.cloudapi.de/`|

### <a name="azure-china-21vianet"></a>Azure China 21Vianet

| Servicio | Punto de conexión |
| ------- | -------- |
| Azure Resource Manager | `https://management.chinacloudapi.cn/` |
| Authentication | `https://login.chinacloudapi.cn/` |
| Público del token |  `https://management.core.chinacloudapi.cn/` |

## <a name="see-also"></a>Consulte también

* [Regiones de Azure](https://azure.microsoft.com/global-infrastructure/regions/)
* [Nombres de código y puntos de conexión regionales](azure-regions-code-names.md)
* [Zonas geográficas de Azure](https://azure.microsoft.com/global-infrastructure/geographies/)
* [Ubicaciones de Azure](https://azure.microsoft.com/global-infrastructure/locations/)

## <a name="next-steps"></a>Pasos siguientes

[Introducción a Media Services v3](media-services-overview.md)
