---
title: 'Tutorial: Bot de Language Understanding Node.js v4'
description: Use Node.js para crear un bot de chat que se integre con Language Understanding (LUIS) en este tutorial. Este bot de chat utiliza la aplicación de recursos humanos para implementar con rapidez una solución de bot. El bot se compila con la versión 4 de Bot Framework y el bot de aplicación web de Azure.
ms.topic: tutorial
ms.date: 02/03/2020
ms.openlocfilehash: 3ce12176957412a5599ced8b043f553969194efb
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "76987854"
---
# <a name="tutorial-use-a-web-app-bot-enabled-with-language-understanding-in-nodejs"></a>Tutorial: Uso de un bot de aplicación web habilitado con Language Understanding en Node.js

Use Node.js para crear un bot de chat que se integre con Language Understanding (LUIS). El bot se ha compilado con el recurso [bot de aplicación web](https://docs.microsoft.com/azure/bot-service/) de Azure y [Bot Framework versión](https://github.com/Microsoft/botbuilder-dotnet) V4.

**En este tutorial, aprenderá a:**

> [!div class="checklist"]
> * Crear un bot de aplicación web. Este proceso crea una nueva aplicación de LUIS.
> * Descargar el proyecto de bot creado por el servicio de bot web
> * Iniciar el emulador y el bot localmente en el equipo.
> * Ver los resultados de expresiones del bot.

## <a name="prerequisites"></a>Prerrequisitos

* [Bot Emulator](https://aka.ms/abs/build/emulatordownload)
* [Visual Studio Code](https://code.visualstudio.com/Download)


## <a name="create-a-web-app-bot-resource"></a>Crear un nuevo recurso de bot de aplicación web

1. En [Azure Portal](https://portal.azure.com), haga clic en **Crear un recurso**.

1. En el cuadro de búsqueda, busque y seleccione **Bot de aplicación web**. Seleccione **Crear**.

1. En **Bot Service**, proporcione la información necesaria:

    |Configuración|Propósito|Configuración sugerida|
    |--|--|--|
    |Nombre del bot|Nombre del recurso|`luis-nodejs-bot-` + `<your-name>`, por ejemplo, `luis-nodejs-bot-johnsmith`|
    |Subscription|Suscripción donde se creará el bot.|Suscripción principal.
    |Resource group|Grupo lógico de recursos de Azure|Crear un nuevo grupo para almacenar todos los recursos utilizados con este bot, denominando el grupo `luis-nodejs-bot-resource-group`.|
    |Location|Región de Azure: no tiene que ser la misma que la región de creación o publicación de LUIS.|`westus`|
    |Plan de tarifa|Se usa para los límites de solicitud de servicio y facturación.|`F0` es el nivel Gratis.
    |Nombre de la aplicación|El nombre se usa como subdominio cuando el bot se implementa en la nube (por ejemplo, humanresourcesbot.azurewebsites.net).|`luis-nodejs-bot-` + `<your-name>`, por ejemplo, `luis-nodejs-bot-johnsmith`|
    |Plantilla de bot|Configuración de Bot Framework: ver la tabla siguiente|
    |Ubicación de la aplicación de LUIS|Debe ser la misma que la región de recursos de LUIS.|`westus`|
    |Plan de App Service/ubicación|No cambie el valor predeterminado proporcionado.|
    |Application Insights|No cambie el valor predeterminado proporcionado.|
    |Id. y contraseña de aplicación de Microsoft|No cambie el valor predeterminado proporcionado.|

1. En **Bot template** (Plantilla del bot), seleccione lo siguiente y luego elija el botón **Seleccionar** en esta configuración:

    |Configuración|Propósito|Número de selección|
    |--|--|--|
    |Versión del SDK|Versión de Bot Framework|**SDK v4**|
    |Lenguaje de SDK|Lenguaje de programación del bot|**Node.js**|
    |Bot|Tipo de bot|**Bot básico**|

1. Seleccione **Crear**. Esto crea e implementa el servicio de bots en Azure. Parte de este proceso crea una aplicación de LUIS denominada `luis-nodejs-bot-XXXX`. Este nombre se basa en el nombre de la aplicación /Azure Bot Service.

    > [!div class="mx-imgBorder"]
    > [![Creación de un bot de aplicación web](./media/bfv4-nodejs/create-web-app-service.png)](./media/bfv4-nodejs/create-web-app-service.png#lightbox)

    Antes de continuar, espere hasta que se crea el servicio bot.

## <a name="the-bot-has-a-language-understanding-model"></a>El bot tiene un modelo de Language Understanding.

El proceso de creación del servicio de bot también crea una nueva aplicación LUIS con intenciones y expresiones de ejemplo. El bot proporciona asignación de intenciones a la nueva aplicación de LUIS para las intenciones siguientes:

|Intenciones de LUIS del bot básico|Expresión de ejemplo|
|--|--|
|Reservar vuelo|`Travel to Paris`|
|Cancelar|`bye`|
|GetWeather|`what's the weather like?`|
|None|Nada fuera del dominio de la aplicación.|

## <a name="test-the-bot-in-web-chat"></a>Probar el bot en Chat en web

1. Mientras sigue en Azure Portal para el nuevo bot, seleccione **Test in Web Chat** (Probar en chat web).
1. En el cuadro de texto **Type your message** (Escriba su mensaje), escriba el texto `Book a flight from Seattle to Berlin tomorrow`. Al responder, el bot comprueba que desea reservar un vuelo.

    ![Captura de pantalla de Azure Portal, escribir el texto "hello".](./media/bfv4-nodejs/ask-bot-question-in-portal-test-in-web-chat.png)

    Puede usar la funcionalidad de prueba para probar rápidamente su bot. Para pruebas más completas, como la depuración, descargue el código de bot y use Visual Studio.

## <a name="download-the-web-app-bot-source-code"></a>Descarga del código fuente del bot de aplicación web
Con el fin de desarrollar el código del bot de aplicación web, descargue el código y úselo en el equipo local.

1. En Azure Portal, seleccione **Compilar** en la sección **Bot Management** (Administración del bot).

1. Seleccione **Download Bot source code** (Descargar el código fuente del bot).

    [![Descargar el código fuente del bot de aplicación web para el bot básico](../../../includes/media/cognitive-services-luis/bfv4/download-code.png)](../../../includes/media/cognitive-services-luis/bfv4/download-code.png#lightbox)

1. Cuando el cuadro de diálogo emergente le pregunte **Include app settings in the downloaded zip file?** (¿Incluir la configuración de la aplicación en el archivo zip descargado?), seleccione **Sí**. Así se proporciona la configuración de LUIS.

1. Cuando el código fuente se haya comprimido, un mensaje proporcionará un vínculo para descargar el código. Seleccione el vínculo.

1. Guarde el archivo ZIP en el equipo local y extraiga los ficheros. Abra el proyecto con Visual Studio.

## <a name="review-code-to-send-utterance-to-luis-and-get-response"></a>Revisión del código para enviar la expresión a LUIS y obtener respuesta

1. Para enviar la expresión del usuario al punto de conexión de predicción LUIS, abra el archivo **dialogs -> flightBookingRecognizer.cs**. Aquí es donde la expresión del usuario especificada en el bot se envía a LUIS. La respuesta de LUIS se devuelve desde el método **executeLuisQuery**.

    ````javascript
    class FlightBookingRecognizer {

        ...

        /**
         * Returns an object with preformatted LUIS results for the bot's dialogs to consume.
         * @param {TurnContext} context
         */
        async executeLuisQuery(context) {
            return await this.recognizer.recognize(context);
        }

        ...

    }
    ````

1. El archivo **dialogs -> MainDialog** captura la expresión y la envía a executeLuisQuery en el método actStep.


    ````javascript
    // Copyright (c) Microsoft Corporation. All rights reserved.
    // Licensed under the MIT License.

    const { LuisRecognizer } = require('botbuilder-ai');

    class FlightBookingRecognizer {
        constructor(config) {
            const luisIsConfigured = config && config.applicationId && config.endpointKey && config.endpoint;
            if (luisIsConfigured) {
                this.recognizer = new LuisRecognizer(config, {}, true);
            }
        }

        get isConfigured() {
            return (this.recognizer !== undefined);
        }

        /**
         * Returns an object with preformatted LUIS results for the bot's dialogs to consume.
         * @param {TurnContext} context
         */
        async executeLuisQuery(context) {
            return await this.recognizer.recognize(context);
        }

        getFromEntities(result) {
            let fromValue, fromAirportValue;
            if (result.entities.$instance.From) {
                fromValue = result.entities.$instance.From[0].text;
            }
            if (fromValue && result.entities.From[0].Airport) {
                fromAirportValue = result.entities.From[0].Airport[0][0];
            }

            return { from: fromValue, airport: fromAirportValue };
        }

        getToEntities(result) {
            let toValue, toAirportValue;
            if (result.entities.$instance.To) {
                toValue = result.entities.$instance.To[0].text;
            }
            if (toValue && result.entities.To[0].Airport) {
                toAirportValue = result.entities.To[0].Airport[0][0];
            }

            return { to: toValue, airport: toAirportValue };
        }

        /**
         * This value will be a TIMEX. And we are only interested in a Date so grab the first result and drop the Time part.
         * TIMEX is a format that represents DateTime expressions that include some ambiguity. e.g. missing a Year.
         */
        getTravelDate(result) {
            const datetimeEntity = result.entities.datetime;
            if (!datetimeEntity || !datetimeEntity[0]) return undefined;

            const timex = datetimeEntity[0].timex;
            if (!timex || !timex[0]) return undefined;

            const datetime = timex[0].split('T')[0];
            return datetime;
        }
    }

    module.exports.FlightBookingRecognizer = FlightBookingRecognizer;
    ````
<a name="ask-bot-a-question-for-the-book-flight-intent"></a>

## <a name="use-the-bot-emulator-to-test-the-bot"></a>Uso del emulador de bot para probar el bot

Formule una pregunta al bot para la intención de reservar vuelo.

1. Inicie el emulador de bots y seleccione **Open Bot** (Abrir bot).
1. En el cuadro de diálogo emergente **Open a bot** (Abrir un bot), escriba la dirección URL del bot, por ejemplo, `http://localhost:3978/api/messages`. La ruta `/api/messages` es la dirección web para el bot.
1. Escriba valores en **Microsoft App ID** (Id. de aplicación de Microsoft) y **Microsoft App password** (Contraseña de aplicación de Microsoft), que se encuentran en el archivo **.env** en la raíz del código de bot que descargó.

1. En el emulador de bot, escriba `Book a flight from Seattle to Berlin tomorrow` y obtenga la misma respuesta para el bot básico que la que recibió en el **Test in Web Chat** (Probar en chat web).

    [![Respuesta de bot básico en el emulador](./media/bfv4-nodejs/ask-bot-emulator-a-question-and-get-response.png)](./media/bfv4-nodejs/ask-bot-emulator-a-question-and-get-response.png#lightbox)

1. Seleccione **Sí**. El bot responde con un resumen de sus acciones.
1. En el registro del emulador bot, seleccione la línea que incluye `Luis Trace`. Esto muestra la respuesta JSON de LUIS para la intención y las entidades de la expresión.

    [![Respuesta de bot básico en el emulador](./media/bfv4-nodejs/ask-luis-book-flight-question-get-json-response-in-bot-emulator.png)](./media/bfv4-nodejs/ask-luis-book-flight-question-get-json-response-in-bot-emulator.png#lightbox)


[!INCLUDE [Bot Information](../../../includes/cognitive-services-qnamaker-luis-bot-info.md)]

## <a name="next-steps"></a>Pasos siguientes

Consulte más [ejemplos](https://github.com/microsoft/botframework-solutions) con bots de conversación.

> [!div class="nextstepaction"]
> [Build a Language Understanding app with a custom subject domain](luis-quickstart-intents-only.md) (Compilación de una aplicación Language Understanding con un dominio de sujeto personalizado)