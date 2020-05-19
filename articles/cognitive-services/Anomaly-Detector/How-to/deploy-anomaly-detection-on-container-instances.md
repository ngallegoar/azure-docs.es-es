---
title: Ejecución de un contenedor de Anomaly Detector en Azure Container Instances
titleSuffix: Azure Cognitive Services
description: Implemente el contenedor de Anomaly Detector en una instancia de Azure Container Instances y pruébelo en un explorador web.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: anomaly-detector
ms.topic: conceptual
ms.date: 04/01/2020
ms.author: aahi
ms.openlocfilehash: 975efa9fdaff9175317794d8b6df7afbfc73cba8
ms.sourcegitcommit: bb0afd0df5563cc53f76a642fd8fc709e366568b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/19/2020
ms.locfileid: "83585696"
---
# <a name="deploy-an-anomaly-detector-container-to-azure-container-instances"></a>Implementar un contenedor de Anomaly Detector en Azure Container Instances

Aprenda a implementar el contenedor de [Anomaly Detector](../anomaly-detector-container-howto.md) de Cognitive Services en una instancia de [Azure Container Instances](https://docs.microsoft.com/azure/container-instances/). Este procedimiento muestra la creación de un recurso de Anomaly Detector. Luego se trata la extracción de la imagen de contenedor asociada. Por último, se resalta la posibilidad de aprovechar la orquestación de los dos desde un explorador. El uso de contenedores puede desviar la atención de los desarrolladores de la administración de la infraestructura y centrarla en el desarrollo de aplicaciones.

[!INCLUDE [Prerequisites](../../containers/includes/container-preview-prerequisites.md)]

[!INCLUDE [Create a Cognitive Services Anomaly Detector resource](../includes/create-anomaly-detector-resource.md)]

[!INCLUDE [Create an Anomaly Detector container on Azure Container Instances](../../containers/includes/create-container-instances-resource-from-azure-cli.md)]

[!INCLUDE [API documentation](../../../../includes/cognitive-services-containers-api-documentation.md)]

## <a name="next-steps"></a>Pasos siguientes

* Revise [Instalación y ejecución de contenedores](../anomaly-detector-container-configuration.md) para extraer la imagen del contenedor y ejecutarlo.
* Revise [Configure containers](../anomaly-detector-container-configuration.md) (Configuración de contenedores) para ver las opciones de configuración.
* [Más información sobre el servicio de API Anomaly Detector](https://go.microsoft.com/fwlink/?linkid=2080698&clcid=0x409)
