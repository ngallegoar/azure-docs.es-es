---
title: Implementación y llamada de API web y API REST desde Azure Logic Apps
description: Implementación y llamada a API web y API REST para flujos de trabajo de integración de sistemas en Azure Logic Apps
services: logic-apps
ms.suite: integration
ms.reviewer: klam, logicappspm
ms.topic: article
ms.date: 05/26/2017
ms.openlocfilehash: d1305be54a22b1460000a357074cbb1f67123bd6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "74790754"
---
# <a name="deploy-and-call-custom-apis-from-workflows-in-azure-logic-apps"></a>Implementación y llamada a API personalizadas desde flujos de trabajo de Azure Logic Apps

Después de [crear API personalizadas](./logic-apps-create-api-app.md) para usarlas en flujos de trabajo de aplicaciones lógicas, debe implementar las API para poder llamarlas. Puede implementar las API como [aplicaciones web](../app-service/overview.md), pero considere la posibilidad de implementarlas como [aplicaciones de API](../app-service/app-service-web-tutorial-rest-api.md), lo que puede facilitarle su trabajo al compilar, hospedar y consumir API en la nube y en el entorno local. No tiene que cambiar ningún código en las API; basta con implementar el código en una aplicación de API. Puede hospedar las API en [Azure App Service](../app-service/overview.md), una oferta de plataforma como servicio (PaaS) que proporciona hospedaje API fácil y altamente escalable.

Aunque se puede llamar a cualquier API desde una aplicación lógica, para lograr una experiencia óptima, agregue [metadatos de OpenAPI (anteriormente Swagger)](https://swagger.io/specification/) que describan las operaciones y los parámetros de la API. Este archivo de OpenAPI ayuda a que su API se integre con más facilidad y funcione mejor con aplicaciones lógicas.

## <a name="deploy-your-api-as-a-web-app-or-api-app"></a>Implementación de la API como aplicación web o aplicación de API

Para poder llamar a su API personalizada desde una aplicación lógica, impleméntela como aplicación web o aplicación de API en Azure App Service. Además, para mejorar la legibilidad del archivo de OpenAPI en el Diseñador de Logic Apps, establezca las propiedades de definición de la API y active [Uso compartido de recursos entre orígenes (CORS)](../app-service/overview.md) para su aplicación web o de API.

1. En [Azure Portal](https://portal.azure.com), seleccione la aplicación web o de API.

2. En el menú que se abre, en **API**, elija **Definición de la API**. Establezca **Ubicación de la definición de la API** en la dirección URL del archivo swagger.json de OpenAPI.

   Por lo general, la dirección URL aparece en este formato: `https://{name}.azurewebsites.net/swagger/docs/v1)`

   ![Vinculación al archivo de OpenAPI para la API personalizada](./media/logic-apps-custom-api-deploy-call/custom-api-swagger-url.png)

3. En **API**, elija **CORS**. Establezca la directiva ORS para **Orígenes permitido** en **"*"** (permitir todos).

   Esta configuración permite solicitudes del Diseñador de aplicación lógica.

   ![Permitir solicitudes del Diseñador de aplicación lógica a la API personalizada](./media/logic-apps-custom-api-deploy-call/custom-api-cors.png)

Para más información, consulte [Hospedaje de una API RESTful con CORS en Azure App Service](../app-service/app-service-web-tutorial-rest-api.md).

## <a name="call-your-custom-api-from-logic-app-workflows"></a>Llamada a la API personalizada desde flujos de trabajo de aplicación lógica

Después de configurar las propiedades de definición de la API y de CORS, los desencadenadores y las acciones de la API personalizada deberían estar disponibles para incluirlos en el flujo de trabajo de la aplicación lógica. 

*  Para ver los sitios web que tienen direcciones URL de OpenAPI, puede examinar los sitios web de su suscripción en el Diseñador de Logic Apps.

*  Para ver las acciones y entradas disponibles señalando a un documento de OpenAPI, use la [acción HTTP + Swagger](../connectors/connectors-native-http-swagger.md).

*  Para llamar a cualquier API, incluso aquellas que no tengan ni expongan un documento de OpenAPI, siempre puede crear una solicitud con la [acción HTTP](../connectors/connectors-native-http.md).

## <a name="next-steps"></a>Pasos siguientes

* [Información general de los conectores personalizados](../logic-apps/custom-connector-overview.md)