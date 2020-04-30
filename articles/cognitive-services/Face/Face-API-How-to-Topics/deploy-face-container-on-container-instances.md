---
title: Ejecución de un contenedor de Face en Azure Container Instances
titleSuffix: Azure Cognitive Services
description: Implemente el contenedor de Face en una instancia de Azure Container Instances y pruébelo en un explorador web.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: face-api
ms.topic: conceptual
ms.date: 04/01/2020
ms.author: aahi
ms.openlocfilehash: 4df5949e24e3fa59d37379c058a777c93dda2c44
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/29/2020
ms.locfileid: "80878378"
---
# <a name="deploy-the-face-container-to-azure-container-instances"></a>Implementar un contenedor de Face en Azure Container Instances

Aprenda a implementar el contenedor de [Face](../face-how-to-install-containers.md) de Cognitive Services en una instancia de [Azure Container Instances](https://docs.microsoft.com/azure/container-instances/). Este procedimiento muestra la creación de un recurso de Azure Face. Luego se trata la extracción de la imagen de contenedor asociada. Por último, se resalta la posibilidad de aprovechar la orquestación de los dos desde un explorador. El uso de contenedores puede desviar la atención de los desarrolladores de la administración de la infraestructura y centrarla en el desarrollo de aplicaciones.

[!INCLUDE [Prerequisites](../../containers/includes/container-preview-prerequisites.md)]

## <a name="request-access-to-the-private-container-registry"></a>Solicitud de acceso al registro de contenedor privado

[!INCLUDE [Request access to private container registry](../../../../includes/cognitive-services-containers-request-access.md)]

[!INCLUDE [Create a Cognitive Services Face resource](../includes/create-face-resource.md)]

[!INCLUDE [Create an Face container on Azure Container Instances](../../containers/includes/create-container-instances-resource-from-azure-cli.md)]

[!INCLUDE [API documentation](../../../../includes/cognitive-services-containers-api-documentation.md)]

[!INCLUDE [Containers Next Steps](../../containers/includes/containers-next-steps.md)]