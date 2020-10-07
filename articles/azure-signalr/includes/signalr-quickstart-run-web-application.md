---
title: archivo de inclusión
description: archivo de inclusión
author: anthonychu
ms.service: signalr
ms.topic: include
ms.date: 03/04/2019
ms.author: antchu
ms.custom: include file
ms.openlocfilehash: 2af9c1dfd13d3aeafa7cf1ac76537117ecc15aff
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/05/2020
ms.locfileid: "84317429"
---
## <a name="run-the-web-application"></a>Ejecución de la aplicación web

1. Para simplificar las pruebas del cliente, abra el explorador en nuestra aplicación web de la página individual de ejemplo [https://azure-samples.github.io/signalr-service-quickstart-serverless-chat/demo/chat-v2/](https://azure-samples.github.io/signalr-service-quickstart-serverless-chat/demo/chat-v2/). 

    > [!NOTE]
    > El origen del archivo HTML se encuentra en [/docs/demo/chat-v2/index.html](https://github.com/Azure-Samples/signalr-service-quickstart-serverless-chat/blob/master/docs/demo/chat-v2/index.html). Y si desea hospedar el HTML usted mismo, inicie un servidor HTTP local, como [http-server](https://www.npmjs.com/package/http-server) en el directorio */docs/demo/chat-v2*. Asegúrese de que el origen se agrega al valor `CORS` en *local.settings.json* de forma similar al ejemplo.
    > 
    > ```javascript
    > "Host": {
    >  "LocalHttpPort": 7071,
    >  "CORS": "http://localhost:8080,https://azure-samples.github.io",
    >  "CORSCredentials": true
    > }
    >
    > ```

1. Cuando se le pida la dirección URL base de la aplicación de función, escriba `http://localhost:7071`.

1. Escriba un nombre de usuario cuando se le solicite.

1. La aplicación web llama a la función *GetSignalRInfo* de la aplicación de función para recuperar la información de conexión y conectarse al servicio de Azure SignalR. Cuando se completa la conexión, aparece el cuadro de entrada de mensajes del chat.

1. Escriba un mensaje y presione ENTRAR. La aplicación envía el mensaje a la función *SendMessage* de la aplicación Azure Function que, a continuación, usa el enlace saliente de SignalR para difundir el mensaje a todos los clientes conectados. Si todo funciona correctamente, debería aparecer el mensaje en la aplicación.

    ![Ejecución de la aplicación](../media/signalr-quickstart-azure-functions-csharp/signalr-quickstart-run-application.png)

1. Abra otra instancia de la aplicación web en otra ventana del explorador. Verá que los mensajes enviados aparecerán en todas las instancias de la aplicación.
