---
title: Ejecución de un contenedor de Anomaly Detector en Azure Container Instances
titleSuffix: Azure Cognitive Services
description: Implemente el contenedor de Anomaly Detector en una instancia de Azure Container Instances y pruébelo en un explorador web.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.subservice: anomaly-detector
ms.topic: conceptual
ms.date: 01/23/2020
ms.author: dapine
ms.openlocfilehash: 2fba0a0d64502a30b6dfbc9f4f109bca65cca8b9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "76716357"
---
# <a name="deploy-an-anomaly-detector-container-to-azure-container-instances"></a>Implementar un contenedor de Anomaly Detector en Azure Container Instances

Aprenda a implementar el contenedor de [Anomaly Detector](../anomaly-detector-container-howto.md) de Cognitive Services en una instancia de [Azure Container Instances](https://docs.microsoft.com/azure/container-instances/). Este procedimiento muestra la creación de un recurso de Anomaly Detector. Luego se trata la extracción de la imagen de contenedor asociada. Por último, se resalta la posibilidad de aprovechar la orquestación de los dos desde un explorador. El uso de contenedores puede desviar la atención de los desarrolladores de la administración de la infraestructura y centrarla en el desarrollo de aplicaciones.

[!INCLUDE [Prerequisites](../../containers/includes/container-preview-prerequisites.md)]

## <a name="request-access-to-the-private-container-registry"></a>Solicitud de acceso al registro de contenedor privado

Primero debe completar y enviar el [formulario de solicitud de contenedores de Anomaly Detector](https://aka.ms/adcontainer) para solicitar acceso al contenedor.

[!INCLUDE [Request access](../../../../includes/cognitive-services-containers-request-access-only.md)]

[!INCLUDE [Create a Cognitive Services Anomaly Detector resource](../includes/create-anomaly-detector-resource.md)]

[!INCLUDE [Create an Anomaly Detector container on Azure Container Instances](../../containers/includes/create-container-instances-resource-from-azure-cli.md)]

[!INCLUDE [API documentation](../../../../includes/cognitive-services-containers-api-documentation.md)]

## <a name="next-steps"></a>Pasos siguientes

* Revise [Instalación y ejecución de contenedores](../anomaly-detector-container-configuration.md) para extraer la imagen del contenedor y ejecutarlo.
* Revise [Configure containers](../anomaly-detector-container-configuration.md) (Configuración de contenedores) para ver las opciones de configuración.
* [Más información sobre el servicio de API Anomaly Detector](https://go.microsoft.com/fwlink/?linkid=2080698&clcid=0x409)
