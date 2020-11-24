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
ms.openlocfilehash: a077d255648ff07cc88b43dece889a221c46c11f
ms.sourcegitcommit: 04fb3a2b272d4bbc43de5b4dbceda9d4c9701310
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/12/2020
ms.locfileid: "94561517"
---
<a name="create-luis-resources"></a>

## <a name="create-luis-resources-in-the-azure-portal"></a>Creación de recursos de LUIS en Azure Portal

1. Use [este vínculo](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesLUISAllInOne) para empezar a crear recursos de LUIS en Azure Portal.

1. Establezca todas las opciones de configuración necesarias:

    |Nombre|Propósito|
    |--|--|
    |Suscripción | La suscripción que se facturará para el recurso.|
    |Resource group| El nombre del grupo de recursos personalizado que elija o cree. Los grupos de recursos le permiten agrupar los recursos de Azure para el acceso y la administración.|
    |Nombre| el nombre personalizado que elija. Se usa como parte del subdominio personalizado para las consultas de punto de conexión de creación y predicción.|
    |Creación de la ubicación|Es la región asociada al modelo.|
    |Creación del plan de tarifa|Determina el número máximo de transacciones por segundo y mes.|
    |Ubicación de predicción|Es la región asociada al tiempo de ejecución del punto de conexión de predicción publicado.|
    |Plan de tarifa de predicción|Determina el número máximo de transacciones por segundo y mes.|

    > [!div class="mx-imgBorder"]
    > [![Captura de pantalla que muestra la ficha Datos básicos en Crear.](../media/luis-how-to-azure-subscription/create-resource-in-azure-small.png)](../media/luis-how-to-azure-subscription/create-resource-in-azure-small.png#lightbox)

1. Seleccione  **Revisar y crear** y espere a que el recurso se cree.
1. Después de crear ambos recursos, y sin salir de Azure Portal, seleccione el nuevo recurso de creación. Luego, seleccione **Inicios rápidos** para obtener la **dirección URL del punto de conexión** de creación y la **clave** para crear mediante programación.

> [!TIP]
> Para usar los recursos, [asígnelos](../luis-how-to-azure-subscription.md#assign-an-authoring-resource-in-the-luis-portal-for-all-apps) en el portal de LUIS.
