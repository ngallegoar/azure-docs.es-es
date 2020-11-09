---
title: Obtención del punto de conexión de predicción de V3
titleSuffix: Azure Cognitive Services
services: cognitive-services
manager: nitinme
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: include
ms.date: 05/05/2020
ms.openlocfilehash: 85fcd0308083350a02afd4eadac2b361337f3b33
ms.sourcegitcommit: 857859267e0820d0c555f5438dc415fc861d9a6b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2020
ms.locfileid: "93128225"
---
1. En el portal de LUIS, en la sección **Administrar** (menú superior derecho), en la página **Recursos de Azure** (menú izquierdo), en la pestaña **Recursos de predicción** , copie la **Consulta de ejemplo** de la parte inferior de la página. La dirección URL tiene el identificador de la aplicación, la clave y el nombre de la ranura. La dirección URL del punto de conexión de predicción de V3 tiene el siguiente formato: `https://YOUR-RESOURCE-NAME.api.cognitive.microsoft.com/luis/prediction/v3.0/apps/APP-ID/slots/SLOT-NAME/predict?subscription-key=YOUR-PREDICTION-KEY&<optional-name-value-pairs>&query=YOUR_QUERY_HERE`

    :::image type="content" source="../media/prediction-resources-example-query.png" alt-text="consulta de ejemplo en la sección de recursos de predicción" lightbox="../media/prediction-resources-example-query.png":::
    
    Pegue la dirección URL en una pestaña nueva del explorador. Si no ve la dirección URL, significa que no tiene ningún recurso de predicción y tendrá que crearlo. 

    

    

