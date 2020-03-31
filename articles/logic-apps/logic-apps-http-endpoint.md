---
title: Llamada, desencadenamiento o anidación de aplicaciones lógicas
description: Configuración de puntos de conexión HTTP para llamadas, desencadenamientos o anidamiento de flujos de trabajo de aplicaciones lógicas en Azure Logic Apps
services: logic-apps
ms.workload: integration
ms.reviewer: klam, jehollan, logicappspm
ms.topic: article
ms.date: 11/04/2019
ms.openlocfilehash: d5b5a69c7927d07c0ae6b3b56ec97b6551e5d46b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "77191330"
---
# <a name="call-trigger-or-nest-logic-apps-by-using-http-endpoints-in-azure-logic-apps"></a>Llamada, desencadenamiento o anidamiento de aplicaciones lógicas con puntos de conexión HTTP en Azure Logic Apps

Para que se pueda llamar a la aplicación lógica a través de una dirección URL con el fin de que pueda recibir solicitudes entrantes de otros servicios, puede exponer de forma nativa un punto de conexión HTTP sincrónico como un desencadenador en esa aplicación lógica. Cuando configura esta funcionalidad, también puede anidar la aplicación lógica dentro de otras aplicaciones lógicas, lo que le permite crear un patrón de puntos de conexión a los que se puede llamar.

Para configurar un punto de conexión HTTP, puede usar cualquiera de los siguientes tipos de desencadenadores que permiten que las aplicaciones lógicas reciban solicitudes entrantes:

* [Solicitud](../connectors/connectors-native-reqres.md)
* [Webhook HTTP](../connectors/connectors-native-webhook.md)
* Desencadenadores de conectores administrados del tipo [ApiConnectionWebhook](../logic-apps/logic-apps-workflow-actions-triggers.md#apiconnectionwebhook-trigger) que pueden recibir solicitudes HTTP entrantes

> [!NOTE]
> En estos ejemplos se usa el desencadenador de solicitud, pero puede usar cualquier desencadenador basado en solicitudes HTTP de la lista anterior. Todos los principios se aplican de manera idéntica a estos otros tipos de desencadenadores.

Si no está familiarizado con las aplicaciones lógicas, consulte [¿Qué es Azure Logic Apps?](../logic-apps/logic-apps-overview.md) e [Inicio rápido: Creación de la primera aplicación lógica](../logic-apps/quickstart-create-first-logic-app-workflow.md).

## <a name="prerequisites"></a>Prerrequisitos

* Suscripción a Azure. Si aún no tiene una, [regístrese para obtener una cuenta de Azure gratuita](https://azure.microsoft.com/free/).

* Aplicación lógica en la que quiera configurar el punto de conexión HTTP como desencadenador. Puede empezar con una aplicación lógica en blanco o con una aplicación lógica existente en la que quiera reemplazar el desencadenador actual. Este ejemplo comienza con una aplicación lógica en blanco.

## <a name="create-a-callable-endpoint"></a>Creación de un punto de conexión al que se puede llamar

1. Inicie sesión en [Azure Portal](https://portal.azure.com). Cree y abra una aplicación lógica en blanco en el Diseñador de aplicación lógica.

   En este ejemplo, se usa el desencadenador de solicitud, pero puede usar cualquier desencadenador que pueda recibir solicitudes HTTP entrantes. Todos los principios se aplican de manera idéntica a estos desencadenadores. Para obtener más información sobre el desencadenador de solicitud, consulte [Recepción de llamadas HTTPS entrantes y respuesta a ellas mediante Azure Logic Apps](../connectors/connectors-native-reqres.md).

1. En el cuadro de búsqueda, seleccione **Integrado**. En el cuadro de búsqueda, escriba `request` como filtro. En la lista de desencadenadores, seleccione **Cuando se recibe una solicitud HTTP**.

   ![Buscar y seleccionar el desencadenador de solicitud](./media/logic-apps-http-endpoint/find-and-select-request-trigger.png)

1. De manera opcional, en el cuadro **Esquema JSON de cuerpo de solicitud**, puede especificar un esquema JSON que describa la carga o los datos que espera que reciba el desencadenador.

   El diseñador usa este esquema para generar tokens que representan los resultados del desencadenador. Luego puede hacer referencia fácilmente a estos resultados en el flujo de trabajo de la aplicación lógica. Obtenga más información sobre [tokens generados a partir de esquemas JSON](#generated-tokens).

   Para este ejemplo, escriba este esquema:

   ```json
      {
      "type": "object",
      "properties": {
         "address": {
            "type": "object",
            "properties": {
               "streetNumber": {
                  "type": "string"
               },
               "streetName": {
                  "type": "string"
               },
               "town": {
                  "type": "string"
               },
               "postalCode": {
                  "type": "string"
               }
            }
         }
      }
   }
    ```

   ![Proporcionar el esquema JSON para la acción de solicitud](./media/logic-apps-http-endpoint/manual-request-trigger-schema.png)

   O bien, puede generar un esquema JSON si proporciona una carga de ejemplo:

   1. En el desencadenador **Solicitud**, seleccione **Usar una carga de ejemplo para generar el esquema**.

   1. En el cuadro **Especificar o pegar una carga de JSON de ejemplo**, proporcione una carga de muestra, por ejemplo:

      ```json
      {
         "address": {
            "streetNumber": "00000",
            "streetName": "AnyStreet",
            "town": "AnyTown",
            "postalCode": "11111-1111"
        }
      }
      ```

   1. Cuando lo tenga todo preparado, seleccione **Listo**.

      El cuadro **Esquema JSON de cuerpo de solicitud** muestra ahora el esquema generado.

1. Guarde la aplicación lógica.

   En el cuadro **Solicitud HTTP POST a esta dirección URL** ahora muestra la dirección URL de devolución de llamada generada que otros servicios pueden usar para llamar a la aplicación lógica y desencadenarla. Esta dirección URL incluye una clave de firma de acceso compartido (SAS), la cual se usa para la autenticación, en los parámetros de consulta, por ejemplo:

   ![Dirección URL de devolución de llamadas generada para el punto de conexión](./media/logic-apps-http-endpoint/generated-endpoint-url.png)

   También puede obtener la dirección URL del punto de conexión HTTP en el panel **Información general** de la aplicación lógica.

   1. En el menú de la aplicación lógica, seleccione **Introducción**.

   1. En la sección **Resumen**, seleccione **Ver el historial de desencadenadores**.

      ![Obtención de la dirección URL del punto de conexión HTTP en Azure Portal](./media/logic-apps-http-endpoint/find-manual-trigger-url.png)

   1. En **URL de devolución de llamadas [POST]** , copie la dirección URL:

      ![Copiar la dirección URL del punto de conexión HTTP en Azure Portal](./media/logic-apps-http-endpoint/copy-manual-trigger-callback-url.png)

      También puede obtener la dirección URL mediante la realización de esta llamada:

      ```http
      POST https://management.azure.com/{logic-app-resource-ID}/triggers/{endpoint-trigger-name}/listCallbackURL?api-version=2016-06-01
      ```

<a name="set-method"></a>

## <a name="set-expected-http-method"></a>Configuración del método HTTP esperado

De manera predeterminada, el desencadenador de tipo Solicitud espera una solicitud HTTP POST. Sin embargo, puede especificar un método esperado diferente, pero solo uno.

1. En el desencadenador de solicitud, abra la lista **Agregar nuevo parámetro** y seleccione **Método**, que agrega esta propiedad al desencadenador.

   ![Agregar la propiedad "Método" al desencadenador](./media/logic-apps-http-endpoint/select-add-new-parameter-for-method.png)

1. En la lista **Método**, seleccione otro método que espera el desencadenador en su lugar. O bien, puede especificar un método personalizado.

   Por ejemplo, seleccione el método **GET** para que pueda probar la dirección URL del punto de conexión HTTP más adelante.

   ![Seleccionar el método HTTP que se usará para el desencadenador](./media/logic-apps-http-endpoint/select-method-request-trigger.png)

## <a name="accept-parameters-in-endpoint-url"></a>Aceptación de parámetros en la dirección URL del punto de conexión

Si quiere que la dirección URL del punto de conexión acepte parámetros, especifique la ruta de acceso relativa en el desencadenador. También debe [establecer explícitamente el método](#set-method) que espera la solicitud HTTP.

1. En el desencadenador de solicitud, abra la lista **Agregar nuevo parámetro** y seleccione **Puerta de acceso relativa** para agregar esta propiedad al desencadenador.

   ![Agregar la propiedad "Ruta de acceso relativa" al desencadenador](./media/logic-apps-http-endpoint/select-add-new-parameter-for-relative-path.png)

1. En la propiedad **Ruta de acceso relativa**, especifique la ruta de acceso relativa para el parámetro en el esquema JSON que quiera que acepte la dirección URL, por ejemplo, `address/{postalCode}`.

   ![Especificar la ruta de acceso relativa para el parámetro](./media/logic-apps-http-endpoint/relative-path-url-value.png)

1. Para usar el parámetro, busque y agregue una acción de **respuesta** a la aplicación lógica.

   1. En el desencadenador de solicitud, seleccione **Nuevo paso** > **Agregar una acción**.

   1. En **Elegir una acción**, en el cuadro de búsqueda, escriba `response` como filtro.

   1. En la lista de acciones, seleccione la acción **Respuesta**.

1. En la propiedad **Cuerpo** de la acción de respuesta, incluya el token que representa el parámetro especificado en la ruta de acceso relativa del desencadenador.

   Por ejemplo, supongamos que quiere que la acción de respuesta devuelva `Postal Code: {postalCode}`.

   En la propiedad **Cuerpo**, escriba `Postal Code: ` con un espacio al final. En la lista de contenido dinámico que aparece, seleccione el token **postalCode**.

   ![Agregar el parámetro al cuerpo de la respuesta](./media/logic-apps-http-endpoint/relative-url-with-parameter-token.png)

   La propiedad **Cuerpo** ahora incluye el parámetro seleccionado:

   ![Ejemplo de cuerpo de respuesta con el parámetro](./media/logic-apps-http-endpoint/relative-url-with-parameter.png)

1. Guarde la aplicación lógica.

    La dirección URL del punto de conexión HTTP ahora incluye la ruta de acceso relativa, por ejemplo:

    ```http
    https://prod-25.westus.logic.azure.com/workflows/{logic-app-resource-ID}/triggers/manual/paths/invoke/address/{postalCode}?api-version=2016-10-01&sp=%2Ftriggers%2Fmanual%2Frun&sv=1.0&sig={shared-access-signature}
    ```

1. Para probar el punto de conexión HTTP, copie y pegue la dirección URL actualizada en otra ventana del explorador, pero reemplace `{postalCode}` con `123456`, y presione ENTRAR.

   El explorador muestra este texto: `Postal Code: 123456`

## <a name="call-logic-app-through-http-endpoint"></a>Llamada a la aplicación lógica a través de un punto de conexión HTTP

Después de crear el punto de conexión HTTP, puede desencadenar la aplicación lógica mediante el envío de una solicitud HTTP `POST` a la dirección URL completa del punto de conexión. Las aplicaciones lógicas tienen compatibilidad integrada para los puntos de conexión de acceso directo.

<a name="generated-tokens"></a>

## <a name="tokens-generated-from-schema"></a>Tokens generados a partir de un esquema

Cuando se proporciona un esquema JSON en el desencadenador de solicitud, el Diseñador de aplicación lógica genera tokens para las propiedades de ese esquema. Después puede usar esos tokens para pasar datos a través del flujo de trabajo de la aplicación lógica.

Por ejemplo, si agrega más propiedades, como `"suite"`, al esquema JSON, los tokens de esas propiedades estarán disponibles para su uso en los pasos posteriores de su aplicación lógica. Este es el esquema JSON completo:

```json
   {
   "type": "object",
   "properties": {
      "address": {
         "type": "object",
         "properties": {
            "streetNumber": {
               "type": "string"
            },
            "streetName": {
               "type": "string"
            },
            "suite": {
               "type": "string"
            },
            "town": {
               "type": "string"
            },
            "postalCode": {
               "type": "string"
            }
         }
      }
   }
}
```

## <a name="create-nested-logic-apps"></a>Creación de aplicaciones lógicas anidadas

Puede anidar los flujos de trabajo en la aplicación lógica mediante la adición de otras aplicaciones de lógica que pueden recibir solicitudes. Para incluir estas aplicaciones lógicas, siga los pasos siguientes:

1. En el paso en el que quiera llamar a otra aplicación lógica, seleccione **Nuevo paso** > **Agregar una acción**.

1. En **Elegir una acción**, seleccione **Integrado**. En el cuadro de búsqueda, escriba `logic apps` como filtro. En la lista de acciones, seleccione **Elegir un flujo de trabajo de Logic Apps**.

   ![Anidar una aplicación lógica dentro de la aplicación lógica actual](./media/logic-apps-http-endpoint/choose-logic-apps-workflow.png)

   El diseñador muestra las aplicaciones lógicas que se pueden seleccionar.

1. Seleccione la aplicación lógica a la que se llamará desde la aplicación lógica actual.

   ![Seleccionar la aplicación lógica a la que se llamará desde la aplicación lógica actual](./media/logic-apps-http-endpoint/select-logic-app-to-nest.png)

## <a name="reference-content-from-an-incoming-request"></a>Referencia al contenido de una solicitud entrante

Si el tipo de contenido de la solicitud entrante es `application/json`, puede hacer referencia a las propiedades en la solicitud entrante. En caso contrario, este contenido se trata como una sola unidad binaria que se puede pasar a otras API. Para hacer referencia a este contenido dentro del flujo de trabajo de la aplicación lógica, primero debe convertir el contenido.

Por ejemplo, si va a pasar contenido de tipo `application/xml`, puede usar la [expresión `@xpath()`](../logic-apps/workflow-definition-language-functions-reference.md#xpath) para realizar una extracción de XPath, o bien puede usar la [expresión `@json()`](../logic-apps/workflow-definition-language-functions-reference.md#json) para convertir XML a JSON. Obtenga más información sobre cómo trabajar con los [tipos de contenido](../logic-apps/logic-apps-content-type.md) admitidos.

Para obtener la salida de una solicitud entrante, puede usar la [expresión `@triggerOutputs`](../logic-apps/workflow-definition-language-functions-reference.md#triggerOutputs). Por ejemplo, supongamos que tiene un resultado similar al de este ejemplo:

```json
{
   "headers": {
      "content-type" : "application/json"
   },
   "body": {
      "myProperty" : "property value"
   }
}
```

Para acceder específicamente a la propiedad `body`, puede usar la [expresión `@triggerBody()`](../logic-apps/workflow-definition-language-functions-reference.md#triggerBody) como un acceso directo.

## <a name="respond-to-requests"></a>Respuesta a solicitudes

Es posible que a veces quiera responder a determinadas solicitudes que desencadenan una aplicación lógica mediante la devolución de contenido al autor de la llamada. Para construir el código de estado, el encabezado y el cuerpo de la respuesta, use la acción de respuesta. Esta acción puede aparecer en cualquier lugar de la aplicación lógica, no solo al final del flujo de trabajo. Si la aplicación lógica no incluye una acción de respuesta, el punto de conexión HTTP responde *inmediatamente* con un estado **202 - Aceptado**.

Para que el autor original de la llamada obtenga la respuesta de forma correcta, todos los pasos necesarios para la respuesta deben terminar dentro del [límite del tiempo de espera de la solicitud](./logic-apps-limits-and-config.md), a menos que se llame a la aplicación lógica desencadenada como una aplicación lógica anidada. Si no se devuelve ninguna respuesta dentro de este límite, la solicitud entrante agota el tiempo de espera y recibe la respuesta **408 - Tiempo de espera del cliente agotado**.

En cuanto a las aplicaciones lógicas anidadas, la aplicación lógica principal seguirá esperando una respuesta hasta que se completen todos los pasos, independientemente del tiempo que se necesite.

### <a name="construct-the-response"></a>Construcción de la respuesta

En el cuerpo de la respuesta, puede incluir varios encabezados y cualquier tipo de contenido. Por ejemplo, el encabezado de esta respuesta especifica que el tipo de contenido de la respuesta es `application/json` y que el cuerpo contiene valores de las propiedades `town` y `postalCode`, en función del esquema JSON descrito anteriormente en este tema para el desencadenador de solicitud.

![Proporcionar el contenido de la respuesta para la acción de respuesta HTTP](./media/logic-apps-http-endpoint/content-for-response-action.png)

Las respuestas tienen estas propiedades:

| Propiedad (nombre) | Propiedad (JSON) | Descripción |
|--------------------|-----------------|-------------|
| **Código de estado** | `statusCode` | Código de estado HTTP que se usará en la respuesta para la solicitud entrante. Este código puede ser cualquier código de estado válido que comience con 2xx, 4xx o 5xx. En cambio, no se permiten códigos de estado 3xx. |
| **Encabezados** | `headers` | Uno o más encabezados que se incluirán en la respuesta. |
| **Cuerpo** | `body` | Un objeto de cuerpo que puede ser una cadena, un objeto JSON o incluso contenido binario al que se hace referencia desde un paso anterior. |
||||

Para ver la definición de JSON de la acción de respuesta y la definición completa de JSON de la aplicación lógica, en la barra de herramientas del Diseñador de aplicación lógica, seleccione **Vista Código**.

``` json
"Response": {
   "type": "Response",
   "kind": "http",
   "inputs": {
      "body": {
         "postalCode": "@triggerBody()?['address']?['postalCode']",
         "town": "@triggerBody()?['address']?['town']"
      },
      "headers": {
         "content-type": "application/json"
      },
      "statusCode": 200
   },
   "runAfter": {}
}
```

## <a name="q--a"></a>Preguntas y respuestas

#### <a name="q-what-about-url-security"></a>P: ¿Qué se puede decir sobre la seguridad de las direcciones URL?

**R.** : Azure genera direcciones URL de devolución de llamada de la aplicación lógica de manera segura con una [firma de acceso compartido (SAS)](https://docs.microsoft.com/rest/api/storageservices/delegate-access-with-shared-access-signature). Esta firma pasa como un parámetro de consulta y debe validarse antes de que se pueda ejecutar la aplicación lógica. Azure genera la firma mediante una combinación única de una clave secreta por aplicación lógica, el nombre del desencadenador y la operación que se realiza. Por tanto, a menos que alguien tenga acceso a la clave de aplicación lógica secreta, no se puede generar una firma válida.

> [!IMPORTANT]
> En sistemas de más alta seguridad y de producción, se recomienda encarecidamente no llamar a la aplicación lógica directamente desde el explorador. Los motivos son estos:
>
> * La clave de acceso compartido aparece en la dirección URL.
> * No puede administrar directivas de contenido de seguridad debido a los dominios compartidos entre los clientes de Azure Logic Apps.

#### <a name="q-can-i-configure-http-endpoints-further"></a>P: ¿Puedo configurar más puntos de conexión HTTP?

**R.** : Sí, los puntos de conexión HTTP admiten una configuración más avanzada con [Azure API Management](../api-management/api-management-key-concepts.md). Este servicio también ofrece la funcionalidad de administrar de forma coherente todas las API, incluidas las aplicaciones lógicas, configurar nombres de dominio personalizados, usar varios métodos de autenticación y muchas más, como, por ejemplo:

* [Cambio del método de solicitud](../api-management/api-management-advanced-policies.md#SetRequestMethod)
* [Cambio de los segmentos de dirección URL de la solicitud](../api-management/api-management-transformation-policies.md#RewriteURL)
* Configuración de los dominios de API Management en [Azure Portal](https://portal.azure.com/)
* Configuración de la directiva para comprobar la autenticación básica

## <a name="next-steps"></a>Pasos siguientes

* [Recepción de llamadas HTTPS entrantes y respuesta e ellas mediante Azure Logic Apps](../connectors/connectors-native-reqres.md)