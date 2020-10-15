---
title: Llamar a API web y API REST personalizadas
description: Llamada a API web y API REST propias desde Azure Logic Apps
services: logic-apps
ms.suite: integration
ms.reviewer: jonfan, logicappspm
ms.topic: article
ms.date: 05/13/2020
ms.openlocfilehash: 7b4d00e8c0366d10fddafa66db699c1a59fd9ad7
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "83659771"
---
# <a name="deploy-and-call-custom-apis-from-workflows-in-azure-logic-apps"></a>Implementación y llamada a API personalizadas desde flujos de trabajo de Azure Logic Apps

Después de [crear sus propias API](./logic-apps-create-api-app.md) para usarlas en los flujos de trabajo de la aplicación lógica, debe implementarlas para poder invocarlas. Puede implementar las API como [aplicaciones web](../app-service/overview.md), pero considere la posibilidad de implementarlas como [aplicaciones de API](../app-service/app-service-web-tutorial-rest-api.md), lo que puede facilitarle su trabajo al compilar, hospedar y consumir API en la nube y en el entorno local. No tiene que cambiar ningún código en las API; basta con implementar el código en una aplicación de API. Puede hospedar las API en [Azure App Service](../app-service/overview.md), una oferta de plataforma como servicio (PaaS) que proporciona hospedaje API fácil y altamente escalable.

Aunque se puede llamar a cualquier API desde una aplicación lógica, para lograr una experiencia óptima, agregue [metadatos de Swagger](https://swagger.io/specification/) que describan las operaciones y los parámetros de la API. Este documento de Swagger facilita que la API se integre con más facilidad y funcione mejor con aplicaciones lógicas.

## <a name="deploy-your-api-as-a-web-app-or-api-app"></a>Implementación de la API como aplicación web o aplicación de API

Para poder llamar a su API personalizada desde una aplicación lógica, impleméntela como aplicación web o aplicación de API en Azure App Service. Para mejorar la legibilidad del documento de Swagger en el Diseñador de Logic Apps, establezca las propiedades de definición de la API y active el [uso compartido de recursos entre orígenes (CORS)](../app-service/overview.md) para la aplicación web o de API.

1. En [Azure Portal](https://portal.azure.com), seleccione la aplicación web o de API.

2. En el menú que se abre, en **API**, elija **Definición de la API**. Establezca **Ubicación de la definición de la API** en la dirección URL del archivo swagger.json.

   Por lo general, la dirección URL aparece en este formato: `https://{name}.azurewebsites.net/swagger/docs/v1)`

   ![Vínculo al documento de Swagger para la API personalizada](./media/logic-apps-custom-api-deploy-call/custom-api-swagger-url.png)

3. En **API**, elija **CORS**. Establezca la directiva ORS para **Orígenes permitido** en **"*"** (permitir todos).

   Esta configuración permite solicitudes del Diseñador de aplicación lógica.

   ![Permitir solicitudes del Diseñador de aplicación lógica a la API personalizada](./media/logic-apps-custom-api-deploy-call/custom-api-cors.png)

Para más información, consulte [Hospedaje de una API RESTful con CORS en Azure App Service](../app-service/app-service-web-tutorial-rest-api.md).

## <a name="call-your-custom-api-from-logic-app-workflows"></a>Llamada a la API personalizada desde flujos de trabajo de aplicación lógica

Después de configurar las propiedades de definición de la API y de CORS, los desencadenadores y las acciones de la API personalizada deberían estar disponibles para incluirlos en el flujo de trabajo de la aplicación lógica. 

*  Para ver los sitios web que tienen direcciones URL de OpenAPI, puede examinar los sitios web de su suscripción en el Diseñador de Logic Apps.

*  Para ver las acciones y entradas disponibles señalando a un documento de Swagger, use la [acción HTTP + Swagger](../connectors/connectors-native-http-swagger.md).

*  Para llamar a cualquier API, incluso aquellas que no tengan ni expongan un documento de Swagger, siempre puede crear una solicitud con la [acción HTTP](../connectors/connectors-native-http.md).

## <a name="next-steps"></a>Pasos siguientes

* [Información general de los conectores personalizados](../logic-apps/custom-connector-overview.md)
