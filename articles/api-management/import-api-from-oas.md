---
title: Importación de una especificación OpenAPI mediante Azure Portal | Microsoft Docs
description: Obtenga información sobre cómo importar una especificación OpenAPI con API Management.
services: api-management
documentationcenter: ''
author: vladvino
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.topic: tutorial
ms.date: 07/12/2019
ms.author: apimpm
ms.openlocfilehash: 57803ec9889cb6a19dae6d6d1070d8381577aff0
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/24/2020
ms.locfileid: "77468395"
---
# <a name="import-an-openapi-specification"></a>Importación de una especificación OpenAPI

En este artículo se explica cómo importar una API de back-end de Especificación OpenAPI que reside en https://conferenceapi.azurewebsites.net?format=json. Esta API de back-end la proporciona Microsoft y se hospeda en Azure. El artículo también muestra cómo probar la API de APIM.

> [!IMPORTANT]
> Consulte este [documento](https://azure.microsoft.com/blog/announcing-the-preview-of-openapi-specification-v3-support-in-azure-api-management/) para obtener información importante y consejos relacionados con la importación de OpenAPI.

En este artículo aprenderá a:

> [!div class="checklist"]
> * Importación de una API de back-end de la especificación OpenAPI
> * Prueba de la API en Azure Portal
> * Pruebe la API en el Portal para desarrolladores

## <a name="prerequisites"></a>Prerequisites

Completar la guía de inicio rápido siguiente: [Creación de una instancia de Azure API Management](get-started-create-service-instance.md)

[!INCLUDE [api-management-navigate-to-instance.md](../../includes/api-management-navigate-to-instance.md)]

## <a name="import-and-publish-a-back-end-api"></a><a name="create-api"> </a>Importación y publicación de una API de back-end

1. Seleccione **API** en **API MANAGEMENT**.
2. Seleccione **Especificación OpenAPI** de la lista **Add a new API** (Agregar una nueva API).

    ![Especificación de OpenAPI](./media/import-api-from-oas/oas-api.png)
3. Especifique la configuración adecuada. Puede establecer los valores de API durante la creación. Como alternativa, puede establecer algunos de ellos más adelante accediendo a la pestaña **Configuración**. <br/> Si presiona **Tab**, algunos de los campos, o todos, se rellenan con la información del servicio de back-end especificado.

    ![Creación de una API](./media/api-management-get-started/create-api.png)

    |Configuración|Value|Descripción|
    |---|---|---|
    |**Especificación OpenAPI**|https://conferenceapi.azurewebsites.net?format=json|Hace referencia al servicio que implementa la API. API Management envía las solicitudes a esta dirección.|
    |**Nombre para mostrar**|*API de conferencia de demostración* (API de conferencia de demostración)|Si presiona la tecla Tab después de escribir la dirección URL del servicio, APIM rellenará este campo en función de lo que aparece en el JSON. <br/>El nombre se muestra en el Portal para desarrolladores.|
    |**Nombre**|*demo-conference-api*|Proporciona un nombre único para la API. <br/>Si presiona la tecla Tab después de escribir la dirección URL del servicio, APIM rellenará este campo en función de lo que aparece en el JSON.|
    |**Descripción**|Proporcione una descripción opcional de la API.|Si presiona la tecla Tab después de escribir la dirección URL del servicio, APIM rellenará este campo en función de lo que aparece en el JSON.|
    |**Sufijo de dirección URL de API**|*conference*|El sufijo se anexa a la dirección URL base del servicio API Management. API Management distingue las API por su sufijo, por lo que el sufijo debe ser único para cada API de un publicador determinado.|
    |**Esquema URL**|*HTTPS*|Determina los protocolos que se pueden usar para acceder a la API. |
    |**Productos**|*Sin límite*| Publique la API asociándola a un producto. Para agregar, opcionalmente, esta nueva API a un producto, escriba el nombre del producto. Este paso se puede repetir varias veces para agregar la API a varios productos.<br/>Los productos son asociaciones de una o varias API. Puede incluir varias API y ofrecerlas a los desarrolladores mediante el portal para desarrolladores. En primer lugar, los desarrolladores deben suscribirse a un producto para acceder a la API. Al suscribirse, obtienen una clave de suscripción que funciona con cualquier API de ese producto. Si creó la instancia de APIM, ya es un administrador, así que, de forma predeterminada, está suscrito a todos los productos.<br/> De forma predeterminada, cada instancia de API Management incluye dos productos de ejemplo: **Starter** y **Unlimited**. |

4. Seleccione **Crear**.

> [!NOTE]
> Las limitaciones de la importación de API se documentan en [otro artículo](api-management-api-import-restrictions.md).

## <a name="test-the-new-api-in-the-azure-portal"></a>Prueba de la nueva API en Azure Portal

![Prueba del mapa de API](./media/api-management-get-started/01-import-first-api-01.png)

Se puede llamar a las operaciones directamente desde Azure Portal, lo que proporciona una forma cómoda de ver y probar las operaciones de una API.

1. Seleccione la API que ha creado en los pasos anteriores (desde la pestaña **API**).
2. Presione la pestaña **Prueba**.
3. Haga clic en **GetSpeakers**. En la página se muestran los campos de los parámetros de consulta, que, en este caso, no tiene ninguno, y los encabezados. Uno de los encabezados es "Ocp-Apim-Suscripción-Key", para la clave de suscripción del producto que está asociado a esta API. La clave se rellena automáticamente.
4. Presione **Enviar**.

    Back-end responde con **200 Aceptar** y algunos datos.

[!INCLUDE [api-management-navigate-to-instance.md](../../includes/api-management-append-apis.md)]

[!INCLUDE [api-management-define-api-topics.md](../../includes/api-management-define-api-topics.md)]

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Transformación y protección de una API publicada](transform-api.md)
