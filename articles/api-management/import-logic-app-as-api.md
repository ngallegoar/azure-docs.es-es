---
title: Importación de una aplicación lógica como una API desde Azure Portal | Microsoft Docs
description: En este tutorial se muestra cómo utilizar API Management (APIM) para importar una aplicación lógica como API.
services: api-management
documentationcenter: ''
author: vladvino
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.topic: tutorial
ms.date: 08/01/2019
ms.author: apimpm
ms.openlocfilehash: 4077187fe04e3be914a6f7fba84c03df1b79d06a
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/24/2020
ms.locfileid: "74108396"
---
# <a name="import-a-logic-app-as-an-api"></a>Importación de una aplicación lógica como API

Este artículo muestra cómo importar una aplicación lógica como una API y probar la API importada.

En este artículo aprenderá a:

> [!div class="checklist"]
>
> -   Importación de una aplicación lógica como API
> -   Prueba de la API en Azure Portal
> -   Pruebe la API en el Portal para desarrolladores

## <a name="prerequisites"></a>Prerequisites

-   Completar la guía de inicio rápido siguiente: [Creación de una instancia de Azure API Management](get-started-create-service-instance.md)
-   Asegúrese de que hay alguna aplicación lógica en su suscripción que expone un punto de conexión HTTP. Para más información, consulte el artículo acerca del [desencadenamiento de flujos de trabajo con puntos de conexión HTTP](../logic-apps/logic-apps-http-endpoint.md)

[!INCLUDE [api-management-navigate-to-instance.md](../../includes/api-management-navigate-to-instance.md)]

## <a name="import-and-publish-a-back-end-api"></a><a name="create-api"> </a>Importación y publicación de una API de back-end

1. Seleccione **API** en **API MANAGEMENT**.
2. Seleccione **Aplicación lógica** en la lista **Add a new API** (Agregar una nueva API).

    ![Aplicación lógica](./media/import-logic-app-as-api/logic-app-api.png)

3. Presione **Examinar** para ver la lista de aplicaciones Logic Apps con desencadenador HTTP en su suscripción. (Tenga en cuenta que las aplicaciones Logic Apps sin desencadenador HTTP no aparecerán en la lista).
4. Seleccione la aplicación. API Management busca el swagger asociado a la aplicación seleccionada, lo captura y lo importa.
5. Agregue un sufijo URL de API. El sufijo es un nombre que identifica esta API específica en esta instancia de API Management. Tiene que ser único en esta instancia de API Management.
6. Publique la API asociándola a un producto. En este caso, se usa el producto "_Unlimited_". Si desea que la API se publique y esté disponible para los desarrolladores, agréguela a un producto. Puede hacerlo durante la creación de la API o configurarla más adelante.

    Los productos son asociaciones de una o varias API. Puede incluir varias API y ofrecerlas a los desarrolladores mediante el portal para desarrolladores. En primer lugar, los desarrolladores deben suscribirse a un producto para acceder a la API. Al suscribirse, obtienen una clave de suscripción que funciona con cualquier API de ese producto. Si creó la instancia de API Management, ya es un administrador, así que de forma predeterminada está suscrito a todos los productos.

    De forma predeterminada, cada instancia de API Management incluye dos productos de ejemplo:

    - **Starter**
    - **Sin límite**

7. Seleccione **Crear**.

## <a name="test-the-api-in-the-azure-portal"></a>Prueba de la API en Azure Portal

Se puede llamar a las operaciones directamente desde Azure Portal, lo que proporciona una forma cómoda de ver y probar las operaciones de una API.

1. Seleccione la API que creó en los pasos anteriores.
2. Presione la pestaña **Prueba**.
3. Seleccione alguna operación.

    La página muestra los campos de parámetros de consulta y los campos para los encabezados. Uno de los encabezados es "Ocp-Apim-Suscripción-Key", para la clave de suscripción del producto que está asociado a esta API. Si ha creado la instancia de API Management, significa que ya es administrador, por lo que la clave se rellena automáticamente.

4. Presione **Enviar**.

    Back-end responde con **200 Aceptar** y algunos datos.

[!INCLUDE [api-management-navigate-to-instance.md](../../includes/api-management-append-apis.md)]

>[!NOTE]
>Cada aplicación lógica tiene una operación **manual-invoke**. Si desea que la API contenga varias aplicaciones lógicas, para que no tenga colisión, debe cambiar el nombre de la función.

[!INCLUDE [api-management-define-api-topics.md](../../includes/api-management-define-api-topics.md)]

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
>
> [Transformación y protección de una API publicada](transform-api.md)
