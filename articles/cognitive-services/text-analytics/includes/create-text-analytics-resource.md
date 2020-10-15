---
title: Crear un recurso de Text Analytics de Cognitive Services
titleSuffix: Azure Cognitive Services
description: Obtenga información sobre cómo crear un recurso de Text Analytics de Cognitive Services.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: include
ms.date: 04/01/2020
ms.author: aahi
ms.openlocfilehash: daafea59b96cc8da6b78f0733c9f54e0e4d3a8c2
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "91779727"
---
## <a name="create-a-cognitive-services-text-analytics-resource"></a>Crear un recurso de Text Analytics de Cognitive Services

1. Inicie sesión en [Azure Portal](https://portal.azure.com).
1. Seleccione **Crear un recurso** y vaya a **IA y Machine Learning** > **Text Analytics**.
   O bien, vaya a [Create Text Analytics](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesTextAnalytics) (Crear un recurso de Text Analytics).
1. Establezca todas las configuraciones necesarias:

    |Configuración|Value|
    |--|--|
    |Nombre|Escriba un nombre (de 2 a 64 caracteres).|
    |Suscripción|Seleccione la suscripción adecuada.|
    |Location|Seleccione una ubicación cercana.|
    |Plan de tarifa| Escriba **S**, el plan de tarifa estándar.|
    |Resource group|Seleccione un grupo de recursos disponible.|

1. Seleccione **Crear** y espere a que el recurso se cree. El explorador se redirige automáticamente a la página de recursos recién creada.
1. Recopile el elemento `endpoint` configurado y una clave de API:

    |Pestaña de recursos en el portal|Configuración|Value|
    |--|--|--|
    |**Información general**|Punto de conexión|Copie el punto de conexión. Tiene un aspecto similar a `https://northeurope.api.cognitive.microsoft.com/text/analytics/v2.0`.|
    |**Claves**|Clave de API|Copie una de las dos claves. Es una cadena alfanumérica de 32 caracteres sin espacios ni guiones: <`xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx`>.|
