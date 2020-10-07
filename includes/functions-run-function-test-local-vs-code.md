---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 09/28/2020
ms.author: glenga
ms.openlocfilehash: 43da0ea4ddfc5410425465d436522523739218fe
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/05/2020
ms.locfileid: "91408584"
---
## <a name="run-the-function-locally"></a>Ejecución local de la función

Visual Studio Code se integra con [Azure Functions Core Tools](../articles/azure-functions/functions-run-local.md) para que pueda ejecutar este proyecto en un equipo de desarrollo local antes de publicarlo en Azure.

1. Para llamar a la función, presione <kbd>F5</kbd> para iniciar el proyecto de aplicación de funciones. La salida de Core Tools aparece en el panel **Terminal**.

1. Si aún no ha instalado Azure Functions Core Tools, seleccione **Instalar** en el símbolo del sistema. Cuando se instala Core Tools, la aplicación se inicia en el panel **Terminal**. Puede ver el punto de conexión de la dirección URL de la función desencadenada por HTTP que se ejecuta localmente.

    ![Salida de la función local de VS Code](./media/functions-run-function-test-local-vs-code/functions-vscode-f5.png)

1. Con Core Tools en ejecución, vaya a la siguiente dirección URL para ejecutar una solicitud GET, que incluye la cadena de consulta `?name=Functions`.

    `http://localhost:7071/api/HttpExample?name=Functions`

1. Se devuelve una respuesta, que tiene un aspecto similar al siguiente en un explorador:

    ![Explorador: salida de ejemplo de localhost](./media/functions-run-function-test-local-vs-code/functions-test-local-browser.png)

1. La información sobre la solicitud se muestra en el panel **Terminal**.

    ![Inicio del host de la tarea: salida del terminal de VS Code](./media/functions-run-function-test-local-vs-code/function-execution-terminal.png)

1. Presione <kbd>Ctrl + C</kbd> para detener Core Tools y desconectar el depurador.
