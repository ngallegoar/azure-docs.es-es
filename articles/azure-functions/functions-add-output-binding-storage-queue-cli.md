---
title: Conexión de Azure Functions a Azure Storage mediante herramientas de línea de comandos
description: Aprenda a conectar Azure Functions a una cola de Azure Storage mediante la incorporación de un enlace de salida al proyecto de línea de comandos.
ms.date: 02/07/2020
ms.topic: quickstart
zone_pivot_groups: programming-languages-set-functions
ms.openlocfilehash: 9181caf516d5c2003cfe99b125d2921732cbbb9d
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/26/2020
ms.locfileid: "79473394"
---
# <a name="connect-azure-functions-to-azure-storage-using-command-line-tools"></a>Conexión de Azure Functions a Azure Storage mediante herramientas de línea de comandos

En este artículo, integrará una cola de Azure Storage con la función y la cuenta de almacenamiento que creó en [el inicio rápido anterior](functions-create-first-azure-function-azure-cli.md). Para lograr esta integración se usa un *enlace de salida* que escribe los datos de una solicitud HTTP en un mensaje de la cola. Completar este artículo no supone ningún costo adicional sobre el pequeño importe del inicio rápido anterior. Para más información acerca de los enlaces, consulte [Conceptos básicos sobre los enlaces y desencadenadores de Azure Functions](functions-triggers-bindings.md).

## <a name="configure-your-local-environment"></a>Configuración del entorno local

Antes de empezar, debe completar el artículo [Inicio rápido: Creación de un proyecto de Azure Functions desde la línea de comandos](functions-create-first-azure-function-azure-cli.md). Si ya ha limpiado los recursos al final de ese artículo, vuelva a recorrer los pasos para crear de nuevo la aplicación de función y los recursos relacionados en Azure.

## <a name="retrieve-the-azure-storage-connection-string"></a>Recuperación de la cadena de conexión de Azure Storage

Al crear una aplicación de funciones en Azure en el inicio rápido anterior, también creó una cuenta de Storage. La cadena de conexión de esta cuenta se almacena de forma segura en la configuración de la aplicación en Azure. Mediante la descarga de la configuración en el archivo *local.settings.json*, puede usar esa conexión para escribir en una cola de Storage de la misma cuenta cuando ejecute la función de forma local. 

1. En la raíz del proyecto, ejecute el comando siguiente, reemplazando `<APP_NAME>` por el nombre de la aplicación de funciones del inicio rápido anterior. Este comando sobrescribirá los valores existentes en el archivo.

    ```
    func azure functionapp fetch-app-settings <APP_NAME>
    ```
    
1. Abra *local.settings.json* y busque el valor denominado `AzureWebJobsStorage`, que es la cadena de conexión de la cuenta de almacenamiento. Usará el nombre `AzureWebJobsStorage` y la cadena de conexión en otras secciones de este artículo.

> [!IMPORTANT]
> Como *local.settings.json* contiene secretos descargados de Azure, excluya siempre este archivo del control de código fuente. El archivo *.gitignore* que se creó con un proyecto de Functions local excluye el archivo de forma predeterminada.

[!INCLUDE [functions-register-storage-binding-extension-csharp](../../includes/functions-register-storage-binding-extension-csharp.md)]

## <a name="add-an-output-binding-definition-to-the-function"></a>Incorporación de una definición de enlace de salida a la función

Aunque ninguna de las funciones puede tener más de un desencadenador, puede tener varios enlaces de entrada y salida que le permiten conectarse a otros servicios y recursos de Azure sin escribir código de integración personalizado. 

::: zone pivot="programming-language-python,programming-language-javascript,programming-language-powershell,programming-language-typescript"  
Estos enlaces se declaran en el archivo *function.json* en la carpeta de la función. En el inicio rápido anterior, el archivo *function.json* de la carpeta *HttpExample* contiene dos enlaces en la colección `bindings`:  
::: zone-end

::: zone pivot="programming-language-javascript,programming-language-typescript"  
:::code language="json" source="~/functions-quickstart-templates/Functions.Templates/Templates/HttpTrigger-JavaScript/function.json" range="2-18":::  
::: zone-end

::: zone pivot="programming-language-python"  
:::code language="json" source="~/functions-quickstart-templates/Functions.Templates/Templates/HttpTrigger-Python/function.json" range="2-18":::  
::: zone-end

::: zone pivot="programming-language-powershell"  
:::code language="json" source="~/functions-quickstart-templates/Functions.Templates/Templates/HttpTrigger-PowerShell/function.json" range="2-18":::
::: zone-end  

::: zone pivot="programming-language-python,programming-language-javascript, programming-language-powershell, programming-language-typescript"  
Cada enlace tiene al menos un tipo, una dirección y un nombre. En el ejemplo anterior, el primer enlace es del tipo `httpTrigger` con la dirección `in`. En el caso de la dirección `in`, `name` especifica el nombre de un parámetro de entrada que se envía a la función cuando la invoca el desencadenador.  
::: zone-end

::: zone pivot="programming-language-javascript,programming-language-typescript"  
El segundo enlace de la colección se denomina `res`. Este enlace `http` es un enlace de salida (`out`) que se usa para escribir la respuesta HTTP. 

Para escribir en una cola de Azure Storage desde esta función, agregue un enlace `out` del tipo `queue` con el nombre `msg`, como se muestra en el código siguiente:

:::code language="json" source="~/functions-docs-javascript/functions-add-output-binding-storage-queue-cli/HttpExample/function.json" range="3-26":::
::: zone-end  

::: zone pivot="programming-language-python"  
El segundo enlace de la colección es de tipo `http` con la dirección `out`, en cuyo caso la variable `name` especial de `$return` indica que este enlace usa el valor devuelto de la función, en lugar de proporcionar un parámetro de entrada.

Para escribir en una cola de Azure Storage desde esta función, agregue un enlace `out` del tipo `queue` con el nombre `msg`, como se muestra en el código siguiente:

:::code language="json" source="~/functions-docs-python/functions-add-output-binding-storage-queue-cli/HttpExample/function.json" range="3-26":::
::: zone-end  

::: zone pivot="programming-language-powershell"  
El segundo enlace de la colección se denomina `res`. Este enlace `http` es un enlace de salida (`out`) que se usa para escribir la respuesta HTTP. 

Para escribir en una cola de Azure Storage desde esta función, agregue un enlace `out` del tipo `queue` con el nombre `msg`, como se muestra en el código siguiente:

:::code language="json" source="~/functions-docs-powershell/functions-add-output-binding-storage-queue-cli/HttpExample/function.json" range="3-26":::
::: zone-end  

::: zone pivot="programming-language-python,programming-language-javascript,programming-language-powershell,programming-language-typescript"  
En este caso, `msg` se asigna a la función como argumento de salida. En el caso de un tipo `queue`, también debe especificar el nombre de la cola en `queueName` y proporcionar el *nombre* de la conexión Azure Storage (desde *local.settings.json*) en `connection`. 
::: zone-end  

::: zone pivot="programming-language-csharp"  
[!INCLUDE [functions-add-storage-binding-csharp-library](../../includes/functions-add-storage-binding-csharp-library.md)]  
::: zone-end  

Para más información sobre los enlaces, consulte [Conceptos básicos sobre los enlaces y desencadenadores de Azure Functions](functions-triggers-bindings.md) y [configuración de la cola de salida](functions-bindings-storage-queue-output.md#configuration).

## <a name="add-code-to-use-the-output-binding"></a>Adición de código para usar el enlace de salida

Con el enlace de cola especificado en *function.json*, puede actualizar la función para que reciba el parámetro de salida `msg` y escribir mensajes en la cola.

::: zone pivot="programming-language-python"     
[!INCLUDE [functions-add-output-binding-python](../../includes/functions-add-output-binding-python.md)]
::: zone-end  

::: zone pivot="programming-language-javascript"  
[!INCLUDE [functions-add-output-binding-js](../../includes/functions-add-output-binding-js.md)]
::: zone-end  

::: zone pivot="programming-language-typescript"  
[!INCLUDE [functions-add-output-binding-ts](../../includes/functions-add-output-binding-ts.md)]
::: zone-end  

::: zone pivot="programming-language-powershell"  
[!INCLUDE [functions-add-output-binding-powershell](../../includes/functions-add-output-binding-powershell.md)]  
::: zone-end

::: zone pivot="programming-language-csharp"  
[!INCLUDE [functions-add-storage-binding-csharp-library-code](../../includes/functions-add-storage-binding-csharp-library-code.md)]
::: zone-end 

Observe que *no* necesita escribir código para la autenticación, para obtener una referencia de la cola ni para escribir datos. Todas estas tareas de integración se administran de manera adecuada en el entorno de ejecución de Azure Functions y en el enlace de salida de la cola.

[!INCLUDE [functions-run-function-test-local-cli](../../includes/functions-run-function-test-local-cli.md)]

[!INCLUDE [functions-extension-bundles-info](../../includes/functions-extension-bundles-info.md)]

## <a name="view-the-message-in-the-azure-storage-queue"></a>Visualización del mensaje en la cola de Azure Storage

La cola se puede ver en [Azure Portal](../storage/queues/storage-quickstart-queues-portal.md) o en el [Explorador de Microsoft Azure Storage](https://storageexplorer.com/). También puede ver la cola en la CLI de Azure, como se describe en los pasos siguientes:

1. Abra el archivo *local.setting.json* del proyecto de Functions y copie el valor de la cadena de conexión. En una ventana de terminal o de comandos, ejecute el siguiente comando para crear una variable de entorno denominada `AZURE_STORAGE_CONNECTION_STRING` y pegue la cadena de conexión concreta en lugar de `<MY_CONNECTION_STRING>`. (Esta variable de entorno significa que no es necesario proporcionar la cadena de conexión a cada comando posterior mediante el argumento `--connection-string`).

    # <a name="bash"></a>[bash](#tab/bash)
    
    ```bash
    AZURE_STORAGE_CONNECTION_STRING="<MY_CONNECTION_STRING>"
    ```
    
    # <a name="powershell"></a>[PowerShell](#tab/powershell)
    
    ```powershell
    $env:AZURE_STORAGE_CONNECTION_STRING = "<MY_CONNECTION_STRING>"
    ```
    
    # <a name="azure-cli"></a>[CLI de Azure](#tab/cmd)
    
    ```azurecli
    set AZURE_STORAGE_CONNECTION_STRING="<MY_CONNECTION_STRING>"
    ```
    
    ---
    
1. (Opcional) Puede usar el comando [`az storage queue list`](/cli/azure/storage/queue#az-storage-queue-list) para ver las colas de Storage de la cuenta. La salida de este comando debe incluir una cola denominada `outqueue`, la cual se creó cuando la función escribió su primer mensaje en esa cola.
    
    ```azurecli
    az storage queue list --output tsv
    ```

1. Use el comando [`az storage message get`](/cli/azure/storage/message#az-storage-message-get) para leer el mensaje de esta cola, que será el primer nombre que usó al probar la función anteriormente. El comando lee y quita el primer mensaje de la cola. 

    # <a name="bash"></a>[bash](#tab/bash)
    
    ```bash
    echo `echo $(az storage message get --queue-name outqueue -o tsv --query '[].{Message:content}') | base64 --decode`
    ```
    
    # <a name="powershell"></a>[PowerShell](#tab/powershell)
    
    ```powershell
    [System.Text.Encoding]::UTF8.GetString([System.Convert]::FromBase64String($(az storage message get --queue-name outqueue -o tsv --query '[].{Message:content}')))
    ```
    
    # <a name="azure-cli"></a>[CLI de Azure](#tab/cmd)
    
    ```azurecli
    az storage message get --queue-name outqueue -o tsv --query [].{Message:content} > %TEMP%out.b64 && certutil -decode -f %TEMP%out.b64 %TEMP%out.txt > NUL && type %TEMP%out.txt && del %TEMP%out.b64 %TEMP%out.txt /q
    ```

    Este script usa certutil para descodificar la colección de mensajes codificados en Base64 desde un archivo temporal local. Si no hay ninguna salida, intente quitar `> NUL` del script para dejar de suprimir la salida de certutil, en caso de que se produzca un error. 
    
    ---
    
    Dado que el cuerpo del mensaje se almacena [codificado en Base64](functions-bindings-storage-queue-trigger.md#encoding), el mensaje se debe descodificar antes de visualizarse. Después de ejecutar `az storage message get`, el mensaje se quita de la cola. Si solo había un mensaje en `outqueue`, no se recuperará al ejecutar este comando una segunda vez y, en su lugar, recibirá un error.

## <a name="redeploy-the-project-to-azure"></a>Nueva implementación del proyecto en Azure

Ahora que ha comprobado localmente que la función escribió un mensaje en la cola de Azure Storage, puede volver a implementar el proyecto para actualizar el punto de conexión que se ejecuta en Azure.

1. En la carpeta *LocalFunctionsProj*, use el comando [`func azure functionapp publish`](functions-run-local.md#project-file-deployment) para volver a implementar el proyecto y reemplace`<APP_NAME>` el nombre de la aplicación.

    ```
    func azure functionapp publish <APP_NAME>
    ```
    
1. Como en el inicio rápido anterior, use un explorador o CURL para probar la función que ha vuelto a implementar.

    # <a name="browser"></a>[Browser](#tab/browser)
    
    Copie la **dirección URL de invocación** completa que se muestra en la salida del comando de publicación en una barra de direcciones del explorador, y anexe el parámetro de consulta `&name=Functions`. El explorador debe mostrar una salida similar a cuando ejecutó la función localmente.

    ![La salida de la función se ejecuta en Azure en un explorador](./media/functions-add-output-binding-storage-queue-cli/function-test-cloud-browser.png)

    # <a name="curl"></a>[curl](#tab/curl)
    
    Ejecute [`curl`](https://curl.haxx.se/) con la **dirección URL de invocación**, y anexe el parámetro `&name=Functions`. La salida del comando será el texto "Hello Functions".
    
    ![La salida de la función se ejecuta en Azure mediante CURL.](./media/functions-add-output-binding-storage-queue-cli/function-test-cloud-curl.png)

    --- 

1. Vuelva a examinar la cola de Storage, como se describe en la sección anterior, para comprobar que contiene el nuevo mensaje escrito en la cola.

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando haya terminado, use el siguiente comando para eliminar el grupo de recursos y todos los recursos que contiene para evitar incurrir en costos adicionales.

```azurecli
az group delete --name AzureFunctionsQuickstart-rg
```

## <a name="next-steps"></a>Pasos siguientes

Ha actualizado la función desencadenada por HTTP para escribir datos en una cola de almacenamiento. Ahora puede aprender más sobre el desarrollo de Functions desde la línea de comandos mediante Core Tools y la CLI de Azure:

+ [Uso de Azure Functions Core Tools](functions-run-local.md)  

::: zone pivot="programming-language-csharp"  
+ [Ejemplos de proyectos de Function completos en C#](/samples/browse/?products=azure-functions&languages=csharp).

+ [Referencia para desarrolladores de C# de Azure Functions](functions-dotnet-class-library.md)  
::: zone-end 
::: zone pivot="programming-language-javascript"  
+ [Ejemplos de proyectos de Function completos en JavaScript](/samples/browse/?products=azure-functions&languages=javascript).

+ [Guía para el desarrollador de JavaScript para Azure Functions](functions-reference-node.md)  
::: zone-end  
::: zone pivot="programming-language-typescript"  
+ [Ejemplos de proyectos de Function completos en TypeScript](/samples/browse/?products=azure-functions&languages=typescript).

+ [Guía para desarrolladores de TypeScript para Azure Functions](functions-reference-node.md#typescript)  
::: zone-end  
::: zone pivot="programming-language-python"  
+ [Ejemplos de proyectos de Function completos en Python](/samples/browse/?products=azure-functions&languages=python).

+ [Guía de Azure Functions para desarrolladores de Python](functions-reference-python.md)  
::: zone-end  
::: zone pivot="programming-language-powershell"  
+ [Ejemplos de proyectos de Function completos en PowerShell](/samples/browse/?products=azure-functions&languages=azurepowershell).

+ [Guía para desarrolladores de PowerShell para Azure Functions](functions-reference-powershell.md) 
::: zone-end
+ [Enlaces y desencadenadores de Azure Functions](functions-triggers-bindings.md)

+ [Página de precios de Functions](https://azure.microsoft.com/pricing/details/functions/)

+ [Estimación de los costos según el plan de consumo](functions-consumption-costs.md) 
