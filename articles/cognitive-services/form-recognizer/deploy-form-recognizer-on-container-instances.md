---
title: Ejecución de un contenedor de Form Recognizer en Azure Container Instances
titleSuffix: Azure Cognitive Services
description: Implemente el contenedor de Form Recognizer en una instancia de Azure Container Instances y pruébelo en un explorador web.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: forms-recognizer
ms.topic: conceptual
ms.date: 07/14/2020
ms.author: aahi
ms.openlocfilehash: fb837dfc72a148d20d382a5dc356c99306aa400f
ms.sourcegitcommit: d76108b476259fe3f5f20a91ed2c237c1577df14
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/29/2020
ms.locfileid: "92913179"
---
# <a name="deploy-the-form-recognizer-container-to-azure-container-instances"></a>Implementar un contenedor de Form Recognizer en Azure Container Instances

[!INCLUDE [Form Recognizer containers limit](includes/container-limit.md)]

Aprenda a implementar el contenedor de [Form Recognizer](form-recognizer-container-howto.md) de Cognitive Services en una instancia de [Azure Container Instances](../../container-instances/index.yml). Este procedimiento muestra la creación de un recurso de Azure Form Recognizer. Luego se trata la extracción de la imagen de contenedor asociada. Por último, se resalta la posibilidad de aprovechar la orquestación de los dos desde un explorador. El uso de contenedores puede desviar la atención de los desarrolladores de la administración de la infraestructura y centrarla en el desarrollo de aplicaciones.

[!INCLUDE [Prerequisites](../containers/includes/container-preview-prerequisites.md)]

[!INCLUDE [Create a Cognitive Services Form Recognizer resource](includes/create-resource.md)]

[!INCLUDE [Create an Form Recognizer container on Azure Container Instances](../containers/includes/create-container-instances-resource-from-azure-cli.md)]

[!INCLUDE [API documentation](../../../includes/cognitive-services-containers-api-documentation.md)]

[!INCLUDE [Containers Next Steps](../containers/includes/containers-next-steps.md)]