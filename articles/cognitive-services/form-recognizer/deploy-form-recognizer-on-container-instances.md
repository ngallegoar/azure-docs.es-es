---
title: Ejecución de un contenedor de Form Recognizer en Azure Container Instances
titleSuffix: Azure Cognitive Services
description: Implemente el contenedor de Form Recognizer en una instancia de Azure Container Instances y pruébelo en un explorador web.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.subservice: forms-recognizer
ms.topic: conceptual
ms.date: 02/25/2020
ms.author: dapine
ms.openlocfilehash: 9a3456eb4d30aa8d163488f558b571dd97c73bf4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "77605120"
---
# <a name="deploy-the-form-recognizer-container-to-azure-container-instances"></a>Implementar un contenedor de Form Recognizer en Azure Container Instances

Aprenda a implementar el contenedor de [Form Recognizer](form-recognizer-container-howto.md) de Cognitive Services en una instancia de [Azure Container Instances](https://docs.microsoft.com/azure/container-instances/). Este procedimiento muestra la creación de un recurso de Azure Form Recognizer. Luego se trata la extracción de la imagen de contenedor asociada. Por último, se resalta la posibilidad de aprovechar la orquestación de los dos desde un explorador. El uso de contenedores puede desviar la atención de los desarrolladores de la administración de la infraestructura y centrarla en el desarrollo de aplicaciones.

> [!IMPORTANT]
> Los contenedores de Form Recognizer usan actualmente la versión 1.0 de la API de Form Recognizer. En su lugar, puede acceder a la versión más reciente de la API mediante el servicio administrado.

[!INCLUDE [Prerequisites](../containers/includes/container-preview-prerequisites.md)]

## <a name="request-access-to-the-container-registry"></a>Solicitud de acceso al registro de contenedor

Primero debe completar y enviar el [formulario de solicitud de acceso a contenedores de Form Recognizer de Cognitive Services](https://aka.ms/FormRecognizerContainerRequestAccess) para solicitar acceso al contenedor. Esto también le registra en Computer Vision. No es necesario registrarse mediante el formulario de solicitud de Computer Vision por separado. 

[!INCLUDE [Request access](../../../includes/cognitive-services-containers-request-access-only.md)]

[!INCLUDE [Create a Cognitive Services Form Recognizer resource](includes/create-resource.md)]

[!INCLUDE [Create an Form Recognizer container on Azure Container Instances](../containers/includes/create-container-instances-resource-from-azure-cli.md)]

[!INCLUDE [API documentation](../../../includes/cognitive-services-containers-api-documentation.md)]

[!INCLUDE [Containers Next Steps](../containers/includes/containers-next-steps.md)]
