---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 02/09/2020
ms.author: glenga
ms.openlocfilehash: d53c41752d57a27ebea9bd60f7e723dab1e7308a
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/26/2020
ms.locfileid: "78190890"
---
## <a name="run-the-function-locally"></a>Ejecución local de la función

Para ejecutar una función, inicie el host en tiempo de ejecución local de Azure Functions desde la carpeta *LocalFunctionProj*:

::: zone pivot="programming-language-csharp,programming-language-powershell,programming-language-javascript,programming-language-python"
```
func start
```
::: zone-end

::: zone pivot="programming-language-typescript"
```
npm install
npm start
```
::: zone-end

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
> Si HttpExample no aparece como se muestra a continuación, es probable que haya iniciado el host desde la carpeta *HttpExample*. En ese caso, use **Ctrl**+**C** para detener el host, vaya a la carpeta *LocalFunctionProj* primaria y vuelva a ejecutar el comando anterior.

Copie la dirección URL de la función `HttpExample` de esta salida en un explorador y anexe la cadena de consulta `?name=<your-name>`, lo que hará que la dirección URL completa sea `http://localhost:7071/api/HttpExample?name=Functions`. El explorador debe mostrar un mensaje como este `Hello Functions`:

![Resultado de la ejecución de la función localmente en el explorador](./media/functions-run-function-test-local-cli/function-test-local-browser.png)

El terminal en el que ejecutó `func start` también muestra la salida del registro cuando realiza solicitudes.

Cuando esté listo, presione **Ctrl**+**C** y elija `y` para detener el host de Functions.