---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 10/18/2020
ms.author: glenga
ms.openlocfilehash: 0bb0c22227946cba6790b536b3cb8db24af3ccbc
ms.sourcegitcommit: 7cc10b9c3c12c97a2903d01293e42e442f8ac751
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/06/2020
ms.locfileid: "93422870"
---
## <a name="run-the-function-locally"></a>Ejecución local de la función

1. Para ejecutar una función, inicie el host en tiempo de ejecución local de Azure Functions desde la carpeta *LocalFunctionProj*:

    ```
    func start
    ```

    Hacia el final de la salida, deberían aparecer las líneas siguientes: 
    
    <pre>
    ...
    
    Now listening on: http://0.0.0.0:7071
    Application started. Press Ctrl+C to shut down.
    
    Http Functions:
    
            HttpExample: [GET,POST] http://localhost:7071/api/HttpExample
    ...
    
    </pre>
    
    >[!NOTE]  
    > Si HttpExample no aparece como se muestra a continuación, es probable que haya iniciado el host desde fuera de la carpeta raíz del proyecto. En ese caso, use **Ctrl**+**C** para detener el host, vaya a la carpeta raíz del proyecto y vuelva a ejecutar el comando anterior.

1. Copie la dirección URL de la función `HttpExample` de esta salida en un explorador y anexe la cadena de consulta `?name=<YOUR_NAME>`, lo que hará que la dirección URL completa sea `http://localhost:7071/api/HttpExample?name=Functions`. El explorador debe mostrar un mensaje como este `Hello Functions`:

    ![Resultado de la ejecución de la función localmente en el explorador](./media/functions-run-function-test-local-cli/function-test-local-browser.png)

1. El terminal en el que inició el proyecto también muestra la salida del registro cuando realiza solicitudes.

1. Cuando termine, presione **Ctrl**+**C** y seleccione `y` para detener el host de Functions.
