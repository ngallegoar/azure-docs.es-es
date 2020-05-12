---
title: 'Regiones en las que está disponible Video Indexer: Azure'
titleSuffix: Azure Media Services
description: En este artículo se tratan las regiones de Azure donde está disponible Video Indexer de Azure Media Services.
services: media-services
author: anikaz
manager: johndeu
ms.service: media-services
ms.subservice: video-indexer
ms.topic: article
ms.date: 05/15/2019
ms.author: anzaman
ms.openlocfilehash: b71890323f6aca92fb9f847252feed57d8db234f
ms.sourcegitcommit: 3abadafcff7f28a83a3462b7630ee3d1e3189a0e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/30/2020
ms.locfileid: "82594957"
---
# <a name="azure-regions-in-which-video-indexer-exists"></a>Regiones de Azure en las que existe Video Indexer

Las API de Video Indexer incluyen un parámetro **location** que se debe establecer en la región de Azure a la que se debe enrutar la llamada. Este parámetro debe ser una [región de Azure en la que esté disponible Video Indexer](https://azure.microsoft.com/global-infrastructure/services/?products=cognitive-services&regions=all).

## <a name="locations"></a>Ubicaciones

El parámetro `location` debe tener como valor el nombre en código de la región de Azure. Si usa la versión preliminar de Video Indexer, debe poner `"trial"` como valor. `trial` es el valor predeterminado para el parámetro `location`. Si no, para obtener el nombre en código de la versión de Azure en la que está su cuenta y a la que se debe enrutar la llamada, puede ejecutar la siguiente línea en la [CLI de Azure](/cli/azure):

```azurecli-interactive
az account list-locations
```

Una vez que ejecuta la línea mostrada anteriormente, obtiene una lista de todas las regiones de Azure. Vaya a la región de Azure que tenga el elemento *displayName* que busca y use su valor *name* para el parámetro **location**.

Por ejemplo, para la región Oeste de EE. UU. 2 de Azure (se muestra a continuación), usará "westus2" para el parámetro **location**.

```json
   {
      "displayName": "West US 2",
      "id": "/subscriptions/35c2594a-23da-4fce-b59c-f6fb9513eeeb/locations/westus2",
      "latitude": "47.233",
      "longitude": "-119.852",
      "name": "westus2",
      "subscriptionId": null
    }
```

## <a name="next-steps"></a>Pasos siguientes

- [Personalización del modelo de lenguaje mediante las API](customize-language-model-with-api.md)
- [Personalización del modelo de marcas mediante las API](customize-brands-model-with-api.md)
- [Personalización del modelo de persona mediante las API](customize-person-model-with-api.md)
