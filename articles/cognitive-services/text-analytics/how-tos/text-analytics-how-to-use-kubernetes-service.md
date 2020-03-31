---
title: 'Ejecución de Azure Kubernetes Service: Text Analytics'
titleSuffix: Azure Cognitive Services
description: Implemente la imagen de contenedor de Text Analytics en Azure Kubernetes Service y pruébelo en un explorador web.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: conceptual
ms.date: 11/21/2019
ms.author: dapine
ms.openlocfilehash: 3264ec5a83277e6bb4befad46cd1337175e911c5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "74383470"
---
# <a name="deploy-a-text-analytics-container-to-azure-kubernetes-service"></a>Implementación de un contenedor de Text Analytics en Azure Kubernetes Service

Aprenda cómo implementar la imagen de contenedor de [Text Analytics](https://docs.microsoft.com/azure/cognitive-services/text-analytics/how-tos/text-analytics-how-to-install-containers) de Azure Cognitive Services en Azure Kubernetes Service (AKS). Este procedimiento muestra cómo crear un recurso de Text Analytics, cómo crear una imagen de análisis de opiniones asociada y cómo efectuar la orquestación de ambos elementos desde un explorador. El uso de contenedores puede desviar la atención de la administración de la infraestructura y centrarla en el desarrollo de aplicaciones.

## <a name="prerequisites"></a>Prerequisites

Este procedimiento requiere varias herramientas que se deben instalar y ejecutar localmente. No use Azure Cloud Shell. Necesita lo siguiente:

* Suscripción a Azure. Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.
* Un editor de texto; por ejemplo, [Visual Studio Code](https://code.visualstudio.com/download).
* La [CLI de Azure](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) instalada.
* La [CLI de Kubernetes](https://kubernetes.io/docs/tasks/tools/install-kubectl/) instalada.
* Un recurso de Azure con el plan de tarifa correcto. No todos los planes de tarifa funcionan con este contenedor:
    * El recurso **Azure Text Analytics** solo con los planes de tarifas Estándar o F0.
    * El recurso **Azure Cognitive Services** con el plan de tarifa S0.

[!INCLUDE [Create a Cognitive Services Text Analytics resource](../includes/create-text-analytics-resource.md)]

[!INCLUDE [Create a Text Analytics container on Azure Kubernetes Service (AKS)](../../containers/includes/create-aks-resource.md)]

#### <a name="key-phrase-extraction"></a>[Extracción de frases clave](#tab/keyphrase)

[!INCLUDE [Key Phrase Extraction Kubernetes config and deploy steps](../includes/key-phrase-extraction-kubernetes-config-deploy.md)]

[!INCLUDE [Verify the Key Phrase Extraction container instance](../includes/verify-key-phrase-extraction-container.md)]

#### <a name="language-detection"></a>[Detección de idioma](#tab/language)

[!INCLUDE [Language Detection Kubernetes config and deploy steps](../includes/language-detection-kubernetes-config-deploy.md)]

[!INCLUDE [Verify the Language Detection container instance](../includes/verify-language-detection-container.md)]

#### <a name="sentiment-analysis"></a>[Análisis de sentimiento](#tab/sentiment)

[!INCLUDE [Sentiment Analysis Kubernetes config and deploy steps](../includes/sentiment-analysis-kubernetes-config-deploy.md)]

[!INCLUDE [Verify the Sentiment Analysis container instance](../includes/verify-sentiment-analysis-container.md)]

***

## <a name="next-steps"></a>Pasos siguientes

* Uso de otros [contenedores de Cognitive Services](../../cognitive-services-container-support.md)
* Uso del [servicio conectado de Text Analytics](../vs-text-connected-service.md)
