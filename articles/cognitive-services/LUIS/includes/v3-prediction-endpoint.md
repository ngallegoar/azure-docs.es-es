---
title: Obtención del punto de conexión de predicción de V3
titleSuffix: Azure Cognitive Services
services: cognitive-services
manager: nitinme
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: include
ms.date: 05/05/2020
ms.openlocfilehash: a607ad71915606d6046b4c71291a49b8641bdcc9
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/05/2020
ms.locfileid: "91544885"
---
1. En el portal de LUIS, en la sección **Administrar** (menú superior derecho), en la página **Recursos de Azure** (menú izquierdo), en la pestaña **Recursos de predicción**, copie la **Consulta de ejemplo** de la parte inferior de la página.

    Pegue la dirección URL en una pestaña nueva del explorador.

    La dirección URL tiene el identificador de la aplicación, la clave y el nombre de la ranura. La dirección URL del punto de conexión de predicción de V3 tiene el siguiente aspecto:

    `https://YOUR-RESOURCE-NAME.api.cognitive.microsoft.com/luis/prediction/v3.0/apps/APP-ID/slots/SLOT-NAME/predict?subscription-key=YOUR-PREDICTION-KEY&<optional-name-value-pairs>&query=YOUR_QUERY_HERE`

