---
title: Compatibilidad con los contenedores
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 04/01/2020
ms.author: aahi
ms.openlocfilehash: 2647c604bebc2885f5c8516eb997792b65751e88
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/29/2020
ms.locfileid: "80876105"
---
## <a name="create-an-computer-vision-resource"></a>Creación de un recurso de Computer Vision

1. Inicie sesión en el [Portal de Azure](https://portal.azure.com).
1. Haga clic en [Crear un recurso de **Computer Vision**](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesComputerVision).
1. Establezca todas las opciones de configuración necesarias:

    |Configuración|Value|
    |--|--|
    |Nombre|Nombre que quiera (2-64 caracteres).|
    |Subscription|Seleccione una suscripción adecuada.|
    |Location|Seleccione cualquier ubicación disponible cercana.|
    |Nivel de precios|`F0`: el plan de tarifa mínimo.|
    |Grupo de recursos|Seleccione un grupo de recursos disponible.|

1. Haga clic en **Crear** y espere a que el recurso se cree. Una vez creado, vaya a la página de recursos.
1. Recopile los elementos `{ENDPOINT_URI}` y `{API_KEY}` configurados. Consulte [recopilación los parámetros obligatorios](../computer-vision-how-to-install-containers.md#gathering-required-parameters) para obtener más información.
