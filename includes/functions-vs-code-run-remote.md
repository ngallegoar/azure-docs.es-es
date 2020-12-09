---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 10/01/2020
ms.author: glenga
ms.openlocfilehash: 55a75651b724a4fe975f655958e36fbd40e35db7
ms.sourcegitcommit: ad83be10e9e910fd4853965661c5edc7bb7b1f7c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/06/2020
ms.locfileid: "96748235"
---
## <a name="run-the-function-in-azure"></a>Ejecución de la función en Azure

1. Vuelva al área **Azure: Functions** en la barra lateral y expanda la aplicación de funciones nueva en la suscripción. Expanda **Functions**, haga clic con el botón derecho (Windows) o presione <kbd>Ctrl</kbd> y haga clic (macOS) en **HttpExample** y, luego, elija **Copy function URL** (Copiar dirección URL de función).

    ![Copia de la dirección URL de la función para el nuevo desencadenador HTTP](./media/functions-vs-code-run-remote/function-copy-endpoint-url.png)

1. Pegue esta dirección URL de la solicitud HTTP en la barra de dirección del navegador, agregue la `name` cadena de consulta como `?name=Functions` al final de la dirección URL y, después, ejecute la solicitud. La dirección URL que llama a la función desencadenada por HTTP debería tener el formato siguiente:

    ```http
    http://<FUNCTION_APP_NAME>.azurewebsites.net/api/HttpExample?name=Functions
    ```

    En el siguiente ejemplo se muestra la respuesta en el explorador para la solicitud GET remota devuelta por la función:

    ![Respuesta de la función en el explorador](./media/functions-vs-code-run-remote/functions-test-remote-browser.png)
