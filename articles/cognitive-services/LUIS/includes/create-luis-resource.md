---
title: Creación de un recurso de LUIS
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 10/23/2019
ms.author: dapine
ms.openlocfilehash: a765ac27936da9da5a2f41464c17491e3561f44b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "73465934"
---
## <a name="create-a-luis-resource"></a>Creación de un recurso de LUIS

1. Inicie sesión en [Azure Portal](https://portal.azure.com).
1. Haga clic en [Crear **Language Understanding**](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesLUISAllInOne).
1. Establezca todas las opciones de configuración necesarias:

    |Configuración|Value|
    |--|--|
    |Nombre|Nombre que quiera (2-64 caracteres).|
    |Subscription|Seleccione una suscripción adecuada.|
    |Location|Seleccione cualquier ubicación disponible cercana.|
    |Nivel de precios|`F0`: el plan de tarifa mínimo.|
    |Grupo de recursos|Seleccione un grupo de recursos disponible.|

1. Haga clic en **Crear** y espere a que el recurso se cree. Una vez creado, vaya a la página de recursos.
1. Recopile el valor de `endpoint` y una clave de API; consulte [Recopilación de los parámetros obligatorios](#gathering-required-parameters).

[!INCLUDE [Gathering required parameters](../../containers/includes/container-gathering-required-parameters.md)]
