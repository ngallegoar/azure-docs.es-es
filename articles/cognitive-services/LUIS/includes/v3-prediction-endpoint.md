---
title: Obtención del punto de conexión de predicción de V3
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 10/14/2019
ms.author: diberry
ms.openlocfilehash: 23e7f88b23ac7af7470183877eeb1be89e79779c
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "80287754"
---
1. En el portal de LUIS, en la sección **Manage** (Administrar) del menú superior derecho, en la página **Azure Resources** (Recursos de Azure) (menú izquierdo), en la pestaña **Prediction Resources** (Recursos de predicción), copie la **consulta de ejemplo** de la parte inferior de la página.

    Pegue la dirección URL en una pestaña nueva del explorador.

    La dirección URL tiene el identificador de la aplicación, la clave y el nombre de la ranura. La dirección URL del punto de conexión de predicción de V3 tiene el siguiente aspecto:

    `https://REGION.api.cognitive.microsoft.com/luis/prediction/v3.0/apps/APP-ID/slots/SLOT-NAME/predict?subscription-key=YOUR-KEY&<optional-name-value-pairs>&query=YOUR_QUERY_HERE`

