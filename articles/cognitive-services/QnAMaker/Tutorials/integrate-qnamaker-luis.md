---
title: 'LUIS y QnA Maker: integración en bots'
titleSuffix: Azure Cognitive Services
description: A medida que la base de conocimiento de QnA Maker va creciendo, es difícil mantenerla como un solo conjunto monolítico. Divida la base de conocimiento en fragmentos lógicos más pequeños.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: article
ms.date: 09/26/2019
ms.author: diberry
ms.custom: seodec18
ms.openlocfilehash: b0d28c77966668f919cdf1265f8cc63b4931d5fd
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/16/2020
ms.locfileid: "81402713"
---
# <a name="use-a-bot-with-qna-maker-and-luis-to-distribute-your-knowledge-base"></a>Uso de un bot con QnA Maker y LUIS para distribuir la base de conocimiento
A medida que la base de conocimiento de QnA Maker va creciendo, es difícil mantenerla como un solo conjunto monolítico. Divida la base de conocimiento en fragmentos lógicos más pequeños.

Aunque es fácil crear varias bases de conocimiento en QnA Maker, necesitará alguna lógica para enrutar la pregunta entrante a la base de conocimiento apropiada. Puede hacerlo si utiliza LUIS.

En este artículo se usa el SDK de Bot Framework v3. Si está interesado en la versión del SDK de Bot Framework v4 de esta información, consulte [Uso de varios modelos de LUIS y QnA](https://docs.microsoft.com/azure/bot-service/bot-builder-tutorial-dispatch?view=azure-bot-service-4.0&tabs=csharp).

## <a name="architecture"></a>Architecture

![Gráfico que muestra la arquitectura de QnA Maker con Language Understanding](../media/qnamaker-tutorials-qna-luis/qnamaker-luis-architecture.PNG)

En el gráfico anterior se muestra que QnA Maker primero obtiene la intención de la pregunta entrante de un modelo LUIS. A continuación, QnA Maker utiliza esa intención para enrutar la pregunta a la base de conocimiento de QnA Maker correcta.

## <a name="create-a-luis-app"></a>Creación de una aplicación de LUIS

1. Inicie sesión en el portal de [LUIS](https://www.luis.ai/).
1. [Cree una aplicación](https://docs.microsoft.com/azure/cognitive-services/luis/create-new-app).
1. [Agregue una intención](https://docs.microsoft.com/azure/cognitive-services/luis/add-intents) para cada base de conocimiento de QnA Maker. Las declaraciones de ejemplo se deben corresponder a las preguntas de las bases de conocimiento de QnA Maker.
1. [Entrene la aplicación de LUIS](https://docs.microsoft.com/azure/cognitive-services/luis/luis-how-to-train) y [publíquela](https://docs.microsoft.com/azure/cognitive-services/luis/publishapp).
1. En la sección **Administrar**, tome nota del identificador de la aplicación de LUIS, la clave de punto de conexión de LUIS y el [nombre de dominio personalizado](../../cognitive-services-custom-subdomains.md). Necesitará estos valores más adelante.

## <a name="create-qna-maker-knowledge-bases"></a>Creación de bases de conocimiento de QnA Maker

1. Inicie sesión en [QnA Maker](https://qnamaker.ai).
1. [Cree](https://www.qnamaker.ai/Create) una base de conocimiento para cada intención de la aplicación de LUIS.
1. Pruebe y publique las bases de conocimiento. Al publicar cada una, anote el id., el nombre de recurso (el subdominio personalizado antes de _.azurewebsites.net/qnamaker_) y la clave de punto de conexión de autorización. Necesitará estos valores más adelante.

    En este artículo se da por supuesto que las bases de conocimiento se crean en la misma suscripción de Azure QnA Maker.

    ![Captura de pantalla de la solicitud de QnA Maker](../media/qnamaker-tutorials-qna-luis/qnamaker-http-request.png)

## <a name="web-app-bot"></a>Bot de aplicación web

1. [Cree un bot "Básico" con Azure Bot Service](https://docs.microsoft.com/azure/bot-service/bot-service-quickstart?view=azure-bot-service-4.0) que incluya automáticamente una aplicación de LUIS. Seleccione el lenguaje de programación C#.

1. Después de crear el bot de aplicación web, selecciónelo en Azure Portal.
1. En la navegación del servicio de bot de aplicación web, seleccione **Configuración de aplicaciones**. A continuación, desplácese hacia abajo hasta la sección **Configuración de aplicaciones** de la configuración disponible.
1. Cambie el valor **LuisAppId** por el valor de la aplicación de LUIS creada en la sección anterior. Después, seleccione **Guardar**.


## <a name="change-the-code-in-the-basicluisdialogcs-file"></a>Cambio del código en el archivo BasicLuisDialog.cs
1. En la sección **Administración de bots** de la navegación del bot de aplicación web en Azure Portal, haga clic en **Compilar**.
2. Haga clic en **Open online code editor** (Abrir el editor de código en línea). Se abre una nueva pestaña del explorador con el entorno de edición en línea.
3. En la sección **WWWROOT**, seleccione el directorio **Dialogs** y después abra **BasicLuisDialog.cs**.
4. Agregue dependencias a la parte superior del archivo **BasicLuisDialog.cs**:

    ```csharp
    using System;
    using System.Net.Http;
    using System.Threading.Tasks;
    using System.Collections.Generic;
    using Microsoft.Bot.Builder.Dialogs;
    using Microsoft.Bot.Builder.Luis;
    using Microsoft.Bot.Builder.Luis.Models;
    using Newtonsoft.Json;
    using System.Text;
    ```

5. Agregue las clases siguientes para deserializar la respuesta de QnA Maker:

    ```csharp
    public class Metadata
    {
        public string name { get; set; }
        public string value { get; set; }
    }

    public class Answer
    {
        public IList<string> questions { get; set; }
        public string answer { get; set; }
        public double score { get; set; }
        public int id { get; set; }
        public string source { get; set; }
        public IList<object> keywords { get; set; }
        public IList<Metadata> metadata { get; set; }
    }

    public class QnAAnswer
    {
        public IList<Answer> answers { get; set; }
    }
    ```


6. Agregue la clase siguiente para realizar una solicitud HTTP al servicio QnA Maker. Observe que el valor del encabezado **Autorización** incluye la palabra `EndpointKey`, con un espacio detrás de la palabra. El resultado JSON se deserializa en las clases anteriores y se devuelve la primera respuesta.

    ```csharp
    [Serializable]
    public class QnAMakerService
    {
        private string qnaServiceResourceName;
        private string knowledgeBaseId;
        private string endpointKey;

        public QnAMakerService(string resourceName, string kbId, string endpointkey)
        {
            qnaServiceResourceName = resourceName;
            knowledgeBaseId = kbId;
            endpointKey = endpointkey;

        }
        async Task<string> Post(string uri, string body)
        {
            using (var client = new HttpClient())
            using (var request = new HttpRequestMessage())
            {
                request.Method = HttpMethod.Post;
                request.RequestUri = new Uri(uri);
                request.Content = new StringContent(body, Encoding.UTF8, "application/json");
                request.Headers.Add("Authorization", "EndpointKey " + endpointKey);

                var response = await client.SendAsync(request);
                return  await response.Content.ReadAsStringAsync();
            }
        }
        public async Task<string> GetAnswer(string question)
        {
            string uri = qnaServiceResourceName + "/qnamaker/knowledgebases/" + knowledgeBaseId + "/generateAnswer";
            string questionJSON = "{\"question\": \"" + question.Replace("\"","'") +  "\"}";

            var response = await Post(uri, questionJSON);

            var answers = JsonConvert.DeserializeObject<QnAAnswer>(response);
            if (answers.answers.Count > 0)
            {
                return answers.answers[0].answer;
            }
            else
            {
                return "No good match found.";
            }
        }
    }
    ```


7. Modifique la clase `BasicLuisDialog`. Cada intención de LUIS debe tener un método decorado con **LuisIntent**. El parámetro para la decoración es el nombre real de la intención de LUIS. El nombre del método decorado _debe_ ser el nombre de la intención de LUIS para mejorar la legibilidad y el mantenimiento, pero no tiene que ser el mismo en tiempo de diseño o ejecución.

    ```csharp
    [Serializable]
    public class BasicLuisDialog : LuisDialog<object>
    {
        // LUIS Settings
        static string LUIS_appId = "<LUIS APP ID>";
        static string LUIS_apiKey = "<LUIS API KEY>";
        static string LUIS_hostRegion = "westus.api.cognitive.microsoft.com";

        // QnA Maker global settings
        // assumes all knowledge bases are created with same Azure service
        static string qnamaker_endpointKey = "<QnA Maker endpoint KEY>";
        static string qnamaker_resourceName = "my-qnamaker-s0-s";

        // QnA Maker Human Resources Knowledge base
        static string HR_kbID = "<QnA Maker KNOWLEDGE BASE ID>";

        // QnA Maker Finance Knowledge base
        static string Finance_kbID = "<QnA Maker KNOWLEDGE BASE ID>";

        // Instantiate the knowledge bases
        public QnAMakerService hrQnAService = new QnAMakerService("https://" + qnamaker_resourceName + ".azurewebsites.net", HR_kbID, qnamaker_endpointKey);
        public QnAMakerService financeQnAService = new QnAMakerService("https://" + qnamaker_resourceName + ".azurewebsites.net", Finance_kbID, qnamaker_endpointKey);

        public BasicLuisDialog() : base(new LuisService(new LuisModelAttribute(
            LUIS_appId,
            LUIS_apiKey,
            domain: LUIS_hostRegion)))
        {
        }

        [LuisIntent("None")]
        public async Task NoneIntent(IDialogContext context, LuisResult result)
        {
            HttpClient client = new HttpClient();
            await this.ShowLuisResult(context, result);
        }

        // HR Intent
        [LuisIntent("HR")]
        public async Task HumanResourcesIntent(IDialogContext context, LuisResult result)
        {
            // Ask the HR knowledge base
            var qnaMakerAnswer = await hrQnAService.GetAnswer(result.Query);
            await context.PostAsync($"{qnaMakerAnswer}");
            context.Wait(MessageReceived);
        }

        // Finance intent
        [LuisIntent("Finance")]
        public async Task FinanceIntent(IDialogContext context, LuisResult result)
        {
            // Ask the finance knowledge base
            var qnaMakerAnswer = await financeQnAService.GetAnswer(result.Query);
            await context.PostAsync($"{qnaMakerAnswer}");
            context.Wait(MessageReceived);
        }
        private async Task ShowLuisResult(IDialogContext context, LuisResult result)
        {
            await context.PostAsync($"You have reached {result.Intents[0].Intent}. You said: {result.Query}");
            context.Wait(MessageReceived);
        }
    }
    ```


## <a name="build-the-bot"></a>Compilar el bot
1. En el editor de código, haga clic con el botón derecho en **build.cmd** y seleccione **Run from Console** (Ejecutar desde la consola).

    ![Captura de pantalla de la opción de ejecución desde la consola en el editor de código](../media/qnamaker-tutorials-qna-luis/run-from-console.png)

2. La vista de código se reemplaza por una ventana de terminal en la que se muestran el progreso y los resultados de la compilación.

    ![Captura de pantalla de la compilación de la consola](../media/qnamaker-tutorials-qna-luis/console-build.png)

## <a name="test-the-bot"></a>Probar el bot
En Azure Portal, haga clic en **Test in Web Chat** (Probar en Chat en web) para probar el bot. Escriba mensajes a partir de intenciones diferentes para obtener la respuesta de la base de conocimiento correspondiente.

![Captura de pantalla de la prueba del chat en web](../media/qnamaker-tutorials-qna-luis/qnamaker-web-chat.png)

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Integración de una base de conocimiento en un agente de Power Virtual Agents](integrate-with-power-virtual-assistant-fallback-topic.md)
