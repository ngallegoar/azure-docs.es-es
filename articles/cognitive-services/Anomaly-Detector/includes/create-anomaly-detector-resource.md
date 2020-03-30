---
title: Compatibilidad con los contenedores
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 7/5/2019
ms.author: dapine
ms.openlocfilehash: b40f1833f08074cb0a8d45fe3afc6bac7cbac7f0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "73523827"
---
## <a name="create-an-anomaly-detector-resource"></a>Creación de un recurso de Anomaly Detector

1. Inicie sesión en [Azure Portal](https://portal.azure.com).
1. Haga clic en [Creación de un recurso de **Anomaly Detector**](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesAnomalyDetector)
1. Establezca todas las opciones de configuración necesarias:

    |Configuración|Value|
    |--|--|
    |Nombre|Nombre que quiera (2-64 caracteres).|
    |Subscription|Seleccione una suscripción adecuada.|
    |Location|Seleccione cualquier ubicación disponible cercana.|
    |Nivel de precios|`F0`: el plan de tarifa mínimo.|
    |Grupo de recursos|Seleccione un grupo de recursos disponible.|
    |Preview Confirmation Checkbox (required)|Indica si ha leído el aviso sobre la **versión preliminar**.|

1. Haga clic en **Crear** y espere a que el recurso se cree. Una vez creado, vaya a la página de recursos.
1. Recopile el elemento `endpoint` configurado y una clave de API:

    |Pestaña de recursos en el portal|Configuración|Value|
    |--|--|--|
    |**Información general**|Punto de conexión|Copie el punto de conexión. Tiene un aspecto similar a `https://westus2.api.cognitive.microsoft.com/`.|
    |**Claves**|Clave de API|Copie una de las dos claves. Es una cadena de 32 caracteres alfanuméricos sin espacios ni guiones, `xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx`.|



