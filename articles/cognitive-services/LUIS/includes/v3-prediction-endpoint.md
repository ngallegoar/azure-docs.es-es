---
title: Obtención del punto de conexión de predicción de V3
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 05/05/2020
ms.author: diberry
ms.openlocfilehash: 18f5422202cf972d49349cc04b845c623cabffa3
ms.sourcegitcommit: bb0afd0df5563cc53f76a642fd8fc709e366568b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/19/2020
ms.locfileid: "83589171"
---
1. En el portal de LUIS, en la sección **Administrar** (menú superior derecho), en la página **Recursos de Azure** (menú izquierdo), en la pestaña **Recursos de predicción**, copie la **Consulta de ejemplo** de la parte inferior de la página.

    Pegue la dirección URL en una pestaña nueva del explorador.

    La dirección URL tiene el identificador de la aplicación, la clave y el nombre de la ranura. La dirección URL del punto de conexión de predicción de V3 tiene el siguiente aspecto:

    `https://YOUR-RESOURCE-NAME.api.cognitive.microsoft.com/luis/prediction/v3.0/apps/APP-ID/slots/SLOT-NAME/predict?subscription-key=YOUR-PREDICTION-KEY&<optional-name-value-pairs>&query=YOUR_QUERY_HERE`

