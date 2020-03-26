---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 01/12/2020
ms.author: glenga
ms.openlocfilehash: f4af3c202d4f00c4ac3041921175c92226f0db7c
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/26/2020
ms.locfileid: "76964154"
---
## <a name="run-the-function-locally"></a>Ejecución local de la función

Visual Studio Code se integra con [Azure Functions Core Tools](../articles/azure-functions/functions-run-local.md) para que pueda ejecutar este proyecto en el equipo de desarrollo local antes de publicarlo en Azure.

1. Para llamar a la función, presione F5 para iniciar el proyecto de aplicación de funciones. La salida de Core Tools aparece en el panel **Terminal**.

1. Si aún no ha instalado Azure Functions Core Tools, seleccione **Instalar** en el símbolo del sistema. Cuando se instala Core Tools, la aplicación se inicia en el panel **Terminal**. Puede ver el punto de conexión de la dirección URL de la función desencadenada por HTTP que se ejecuta localmente. 

    ![Salida local de Azure](./media/functions-run-function-test-local-vs-code/functions-vscode-f5.png)

1. Con Core Tools en ejecución, vaya a la siguiente dirección URL para ejecutar una solicitud GET, que incluye la cadena de consulta `?name=Functions`.

    <http://localhost:7071/api/HttpExample?name=Functions>

1. Se devuelve una respuesta, que tiene un aspecto similar al siguiente en un explorador:

    ![Respuesta de localhost de la función en el explorador](./media/functions-run-function-test-local-vs-code/functions-test-local-browser.png)

1. La información sobre la solicitud se muestra en el panel **Terminal**.

    ![Ejecución de función en el panel Terminal](./media/functions-run-function-test-local-vs-code/function-execution-terminal.png)

1. Presione Ctrl + C para detener Core Tools y desconectar el depurador.
