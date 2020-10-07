---
title: Creación de un recurso de LUIS
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 04/07/2020
ms.author: aahi
ms.openlocfilehash: ebfe7e80577b163218a7bc501fa4e3e9b206fd62
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/05/2020
ms.locfileid: "80879225"
---
<a name="create-luis-resources"></a>

## <a name="create-luis-resources-in-azure-portal"></a>Creación de recursos de LUIS en Azure Portal

1. Use [este vínculo](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesLUISAllInOne) para empezar a crear recursos de LUIS en Azure Portal.

1. Establezca todas las opciones de configuración necesarias:

    |Nombre|Propósito|
    |--|--|
    |Nombre de suscripción| Es la suscripción que se facturará por el recurso.|
    |Resource group| Es el nombre del grupo de recursos personalizado que elija o cree. Los grupos de recursos le permiten agrupar los recursos de Azure para el acceso y la administración.|
    |Nombre| Es el nombre personalizado que elija, que se usa como parte del subdominio personalizado para las consultas de punto de conexión de creación y predicción.|
    |Creación de la ubicación|Es la región asociada al modelo.|
    |Creación del plan de tarifa|El plan de tarifa determina el número máximo de transacciones por segundo y mes.|
    |Ubicación del tiempo de ejecución|Es la región asociada al tiempo de ejecución del punto de conexión de predicción publicado.|
    |Plan de tarifa del tiempo de ejecución|El plan de tarifa determina el número máximo de transacciones por segundo y mes.|

    > [!div class="mx-imgBorder"]
    > [![Creación del recurso de reconocimiento del lenguaje](../media/luis-how-to-azure-subscription/create-resource-in-azure-small.png)](../media/luis-how-to-azure-subscription/create-resource-in-azure-small.png#lightbox)

1. Haga clic en  **Revisar y crear** y espere a que el recurso se cree.
1. Una vez creados ambos recursos, en Azure Portal, seleccione el nuevo recurso de creación y, a continuación, **Inicios rápidos** para obtener la **dirección URL del punto de conexión** de creación y la **clave** para la creación mediante programación.

> [!TIP]
> Para usar los recursos, [asígnelos](../luis-how-to-azure-subscription.md#assign-an-authoring-resource-in-the-luis-portal-for-all-apps) en el portal de LUIS.