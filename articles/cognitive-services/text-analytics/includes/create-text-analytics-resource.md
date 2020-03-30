---
title: Crear un recurso de Text Analytics de Cognitive Services
titleSuffix: Azure Cognitive Services
description: Obtenga información sobre cómo crear un recurso de Text Analytics de Cognitive Services.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 11/21/2019
ms.author: dapine
ms.openlocfilehash: d3b65174f3d161e0b8780a890f297d9d1c812b44
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "74383446"
---
## <a name="create-a-cognitive-services-text-analytics-resource"></a>Crear un recurso de Text Analytics de Cognitive Services

1. Inicie sesión en [Azure Portal](https://portal.azure.com).
1. Seleccione **Crear un recurso** y vaya a **IA y Machine Learning** > **Text Analytics**.
   O bien, vaya a [Create Text Analytics](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesTextAnalytics) (Crear un recurso de Text Analytics).
1. Establezca todas las configuraciones necesarias:

    |Configuración|Value|
    |--|--|
    |Nombre|Escriba un nombre (de 2 a 64 caracteres).|
    |Subscription|Seleccione la suscripción adecuada.|
    |Location|Seleccione una ubicación cercana.|
    |Plan de tarifa| Escriba **S**, el plan de tarifa estándar.|
    |Resource group|Seleccione un grupo de recursos disponible.|

1. Seleccione **Crear** y espere a que el recurso se cree. El explorador se redirige automáticamente a la página de recursos recién creada.
1. Recopile el elemento `endpoint` configurado y una clave de API:

    |Pestaña de recursos en el portal|Configuración|Value|
    |--|--|--|
    |**Información general**|Punto de conexión|Copie el punto de conexión. Tiene un aspecto similar a `https://northeurope.api.cognitive.microsoft.com/text/analytics/v2.0`.|
    |**Claves**|Clave de API|Copie una de las dos claves. Es una cadena alfanumérica de 32 caracteres sin espacios ni guiones: <`xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx`>.|
