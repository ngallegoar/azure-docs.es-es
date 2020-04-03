---
title: Creación de una función en Azure que responda a solicitudes HTTP
description: Aprenda a crear una función desde la línea de comandos y, luego, a publicar el proyecto local en el hospedaje sin servidor en Azure Functions.
ms.date: 01/28/2020
ms.topic: quickstart
zone_pivot_groups: programming-languages-set-functions
ms.openlocfilehash: 89b6a9f31414cbaa9cc92c1a0d881a1354180990
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80282739"
---
# <a name="quickstart-create-a-function-in-azure-that-responds-to-http-requests"></a>Inicio rápido: Creación de una función en Azure que responda a solicitudes HTTP

En este artículo se usan herramientas de línea de comandos para crear una función que responda a solicitudes HTTP. Después de probar el código localmente, se implementa en el entorno sin servidor de Azure Functions. Este inicio rápido supone un pequeño costo en su cuenta de Azure.

También hay una [versión basada en Visual Studio Code](functions-create-first-function-vs-code.md) de este artículo.

## <a name="configure-your-local-environment"></a>Configuración del entorno local

Antes de empezar, debe disponer de lo siguiente:

+ Una cuenta de Azure con una suscripción activa. [Cree una cuenta gratuita](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio).

::: zone pivot="programming-language-csharp,programming-language-javascript,programming-language-typescript,programming-language-powershell"  
+ [Azure Functions Core Tools](./functions-run-local.md#v2), versión 2.7.1846, o cualquier versión 2.x posterior.
::: zone-end  
::: zone pivot="programming-language-python"
+ Python 3.6 y 3.7 requieren la versión 2.7.1846 de [Azure Functions Core Tools](./functions-run-local.md#v2), o cualquier versión 2.x posterior. Python 3.8 requiere la [versión 3.x](./functions-run-local.md#v2) de Core Tools.
::: zone-end

+ La [CLI de Azure](/cli/azure/install-azure-cli), versión 2.0.76, o cualquier versión posterior. 
::: zone pivot="programming-language-javascript,programming-language-typescript"
+ Versiones de [Node.js](https://nodejs.org/), Active LTS y Maintenance LTS (se recomiendan 8.11.1 y 10.14.1).
::: zone-end

::: zone pivot="programming-language-python"
+ [Python 3.8](https://www.python.org/downloads/release/python-382/), [Python 3.7](https://www.python.org/downloads/release/python-375/) y [Python 3.6](https://www.python.org/downloads/release/python-368/), que son compatibles con Azure Functions (x64).
::: zone-end
::: zone pivot="programming-language-powershell"
+ [PowerShell Core](/powershell/scripting/install/installing-powershell-core-on-windows)

+ El [SDK de .NET Core 2.2+](https://www.microsoft.com/net/download)
::: zone-end

### <a name="check-your-environment"></a>Comprobación del entorno

+ En una ventana de terminal o de comandos, ejecute `func --version` para comprobar que la versión de Azure Functions Core Tools es 2.7.1846 o una versión 2.x posterior.

+ Ejecute `az --version` para comprobar que la versión de la CLI de Azure es 2.0.76 o posterior.

+ Ejecute `az login` para iniciar sesión en Azure y comprobar una suscripción activa.

::: zone pivot="programming-language-javascript,programming-language-typescript"
+ Ejecute `node --version` para comprobar que la versión de Node.js indica 8.x o 10.x.
::: zone-end
::: zone pivot="programming-language-python"
+ Ejecute `python --version` (Linux/MacOS) o `py --version` (Windows) para comprobar que la versión de Python es la 3.8.x, 3.7.x o 3.6.x.

## <a name="create-and-activate-a-virtual-environment"></a><a name="create-venv"></a>Creación y activación de un entorno virtual

En una carpeta adecuada, ejecute los comandos siguientes para crear y activar un entorno virtual denominado `.venv`. Asegúrese de usar Python 3.8, 3.7 o 3.6, que son compatibles con Azure Functions.


# <a name="bash"></a>[bash](#tab/bash)

```bash
python -m venv .venv
```

```bash
source .venv/bin/activate
```

Si Python no instaló el paquete venv en la distribución de Linux, ejecute el siguiente comando:

```bash
sudo apt-get install python3-venv
```

# <a name="powershell"></a>[PowerShell](#tab/powershell)

```powershell
py -m venv .venv
```

```powershell
.venv\scripts\activate
```

# <a name="cmd"></a>[Cmd](#tab/cmd)

```cmd
py -m venv .venv
```

```cmd
.venv\scripts\activate
```

---

Ejecute todos los comandos siguientes en este entorno virtual activado. (Para salir del entorno virtual, ejecute `deactivate`).

::: zone-end

## <a name="create-a-local-function-project"></a>Creación de un proyecto de función local

En Azure Functions, un proyecto de función es un contenedor para una o varias funciones individuales que responden a un desencadenador específico. Todas las funciones de un proyecto comparten las mismas configuraciones locales y de hospedaje. En esta sección, se crea un proyecto de función que contiene una sola función.

1. En el entorno virtual, ejecute el comando `func init` para crear un proyecto de funciones en una carpeta llamada *LocalFunctionProj* con el runtime especificado:

    ::: zone pivot="programming-language-python"
    ```
    func init LocalFunctionProj --python
    ```
    ::: zone-end
    ::: zone pivot="programming-language-csharp"
    ```
    func init LocalFunctionProj --dotnet
    ```
    ::: zone-end
    ::: zone pivot="programming-language-javascript"
    ```
    func init LocalFunctionProj --javascript
    ```
    ::: zone-end
    ::: zone pivot="programming-language-typescript"
    ```
    func init LocalFunctionProj --typescript
    ```
    ::: zone-end
    ::: zone pivot="programming-language-powershell"
    ```
    func init LocalFunctionProj --powershell
    ```
    ::: zone-end


    Esta carpeta contiene varios archivos del proyecto, incluidos los archivos de configuración [local.settings.json](functions-run-local.md#local-settings-file) y [host.json](functions-host-json.md). Como *local.settings.json* puede contener secretos descargados de Azure, el archivo se excluye del control de código fuente de forma predeterminada en el archivo *.gitignore*.

1. Vaya a la carpeta del proyecto:

    ```
    cd LocalFunctionProj
    ```
    
1. Agregue una función al proyecto mediante el comando siguiente, donde el argumento `--name` es el nombre único de la función y el argumento `--template` especifica el desencadenador de esta. 

    ```
    func new --name HttpExample --template "HTTP trigger"
    ```

    ::: zone pivot="programming-language-csharp"
    `func new` crea un archivo de código HttpExample.cs.
    ::: zone-end
    ::: zone pivot="programming-language-javascript,programming-language-typescript,programming-language-python,programming-language-powershell"
    `func new` crea una subcarpeta que coincide con el nombre de la función que contiene un archivo de código apropiado para el lenguaje elegido del proyecto y un archivo de configuración denominado *function.json*.
    ::: zone-end

### <a name="optional-examine-the-file-contents"></a>(Opcional) Examen del contenido del archivo

Si lo desea, puede ir a [Ejecución local de la función](#run-the-function-locally) y examine el contenido del archivo posteriormente.

::: zone pivot="programming-language-csharp"
#### <a name="httpexamplecs"></a>HttpExample.cs

*HttpExample.cs* contiene un método `Run` que recibe datos de solicitud en la variable `req` de tipo [HttpRequest](/dotnet/api/microsoft.aspnetcore.http.httprequest) que está decorado con **HttpTriggerAttribute**, que define el comportamiento del desencadenador. 

:::code language="csharp" source="~/functions-docs-csharp/http-trigger-template/HttpExample.cs":::

El objeto de devolución es un objeto [ActionResult](/dotnet/api/microsoft.aspnetcore.mvc.actionresult) que devuelve un mensaje de respuesta como [OkObjectResult](/dotnet/api/microsoft.aspnetcore.mvc.okobjectresult) (200) o [BadRequestObjectResult](/dotnet/api/microsoft.aspnetcore.mvc.badrequestobjectresult) (400). Para más información, vea [Enlaces y desencadenadores HTTP de Azure Functions](/azure/azure-functions/functions-bindings-http-webhook?tabs=csharp).
::: zone-end

::: zone pivot="programming-language-python"
#### <a name="__init__py"></a>\_\_init\_\_.py

*\_\_init\_\_.py* contiene una función `main()` de Python que se desencadena en función de la configuración de *function.json*.

:::code language="python" source="~/functions-quickstart-templates/Functions.Templates/Templates/HttpTrigger-Python/__init__.py":::

Con un desencadenador HTTP, la función recibe datos de solicitud en la variable `req`, como se define en *function.json*. `req` es una instancia de la [clase azure.functions.HttpRequest](/python/api/azure-functions/azure.functions.httprequest). El objeto devuelto, definido como `$return` en *function.json*, es una instancia de la [clase azure.functions.HttpResponse](/python/api/azure-functions/azure.functions.httpresponse). Para más información, vea [Enlaces y desencadenadores HTTP de Azure Functions](/azure/azure-functions/functions-bindings-http-webhook?tabs=python).
::: zone-end

::: zone pivot="programming-language-javascript"
#### <a name="indexjs"></a>index.js

*index.js* exporta una función que se desencadena según la configuración de *function.json*.

:::code language="javascript" source="~/functions-quickstart-templates/Functions.Templates/Templates/HttpTrigger-JavaScript/index.js":::

Con un desencadenador HTTP, la función recibe datos de solicitud en la variable `req`, como se define en *function.json*. El objeto devuelto, definido como `$return` en *function.json*, es la respuesta. Para más información, vea [Enlaces y desencadenadores HTTP de Azure Functions](/azure/azure-functions/functions-bindings-http-webhook?tabs=javascript).
::: zone-end

::: zone pivot="programming-language-typescript"
#### <a name="indexts"></a>index.ts

*index.ts* exporta una función que se desencadena de acuerdo con la configuración de *function.json*.

:::code language="typescript" source="~/functions-quickstart-templates/Functions.Templates/Templates/HttpTrigger-TypeScript/index.ts":::

Con un desencadenador HTTP, la función recibe datos de solicitud en la variable `req` de tipo **HttpRequest**, como se define en *function.json*. El objeto devuelto, definido como `$return` en *function.json*, es la respuesta. 
::: zone-end

::: zone pivot="programming-language-powershell"
#### <a name="runps1"></a>run.ps1

*run.ps1* define un script de función que se desencadena de acuerdo con la configuración de *function.json*.

:::code language="powershell" source="~/functions-quickstart-templates/Functions.Templates/Templates/HttpTrigger-PowerShell/run.ps1":::

Con un desencadenador HTTP, la función recibe los datos de solicitud pasados al parámetro `$Request` definido en *function.json*. El objeto devuelto, definido como `Response` en *function.json*, se pasa al cmdlet `Push-OutputBinding` como respuesta. 
::: zone-end

::: zone pivot="programming-language-javascript,programming-language-typescript,programming-language-python,programming-language-powershell"
#### <a name="functionjson"></a>function.json

*function.json* es un archivo de configuración que define la entrada y salida `bindings` de la función, lo que incluye el tipo de desencadenador. 
::: zone-end

::: zone pivot="programming-language-python"
Si lo desea, puede cambiar `scriptFile` para invocar otro un archivo de Python.

:::code language="json" source="~/functions-quickstart-templates/Functions.Templates/Templates/HttpTrigger-Python/function.json":::
::: zone-end

::: zone pivot="programming-language-javascript,programming-language-typescript"
:::code language="json" source="~/functions-quickstart-templates/Functions.Templates/Templates/HttpTrigger-JavaScript/function.json":::
::: zone-end

::: zone pivot="programming-language-powershell"
:::code language="json" source="~/functions-quickstart-templates/Functions.Templates/Templates/HttpTrigger-PowerShell/function.json":::
::: zone-end

::: zone pivot="programming-language-javascript,programming-language-typescript,programming-language-python,programming-language-powershell"  
Cada enlace requiere una dirección, un tipo y un nombre único. El desencadenador HTTP tiene un enlace de entrada de tipo [`httpTrigger`](functions-bindings-http-webhook-trigger.md) y un enlace de salida de tipo [`http`](functions-bindings-http-webhook-output.md).
::: zone-end  

[!INCLUDE [functions-run-function-test-local-cli](../../includes/functions-run-function-test-local-cli.md)]

## <a name="create-supporting-azure-resources-for-your-function"></a>Creación de recursos auxiliares de Azure para la función

Antes de poder implementar el código de la función en Azure, debe crear tres recursos:

- Un grupo de recursos, que es un contenedor lógico de recursos relacionados.
- Una cuenta de almacenamiento, que mantiene el estado y otra información sobre los proyectos.
- Una aplicación de funciones, que proporciona el entorno para ejecutar el código de función. Una aplicación de funciones se asigna al proyecto de funciones y le permite agrupar funciones como una unidad lógica para facilitar la administración, la implementación y el uso compartido de recursos.

Use los siguientes comandos de la CLI de Azure para crear estos elementos. Cada comando proporciona una salida JSON al finalizar.

1. Si aún no lo ha hecho, inicie sesión en Azure con el comando [az login](/cli/azure/reference-index#az-login):

    ```azurecli
    az login
    ```
    
1. Para crear un grupo de recursos, use el comando [az group create](/cli/azure/group#az-group-create). En el ejemplo siguiente se crea un grupo de recursos denominado `AzureFunctionsQuickstart-rg` en la región `westeurope`. (Por lo general, tanto los grupos de recursos como los recursos se crean en una región cercana y se utiliza alguna de las regiones disponibles del comando `az account list-locations`).

    ```azurecli
    az group create --name AzureFunctionsQuickstart-rg --location westeurope
    ```
    ::: zone pivot="programming-language-python"  
    > [!NOTE]
    > No se pueden hospedar aplicaciones de Windows y Linux en el mismo grupo de recursos. Si tiene un grupo de recursos existente llamado `AzureFunctionsQuickstart-rg` con una aplicación web o aplicación de función de Windows, debe usar un grupo de recursos diferente.
    ::: zone-end  
    
1. Cree una cuenta de almacenamiento de uso general en el grupo de recursos con el comando [az storage account create](/cli/azure/storage/account#az-storage-account-create). En el siguiente ejemplo, reemplace `<STORAGE_NAME>` por un nombre único global que sea adecuado. Los nombres deben contener entre 3 y 24 caracteres y solo letras minúsculas. `Standard_LRS` especifica una cuenta de uso general, que es [compatible con Functions](storage-considerations.md#storage-account-requirements).

    ```azurecli
    az storage account create --name <STORAGE_NAME> --location westeurope --resource-group AzureFunctionsQuickstart-rg --sku Standard_LRS
    ```
    
    Este inicio rápido solo le supondrá unos pequeños gastos a la cuenta de almacenamiento.
    
1. Cree la aplicación de funciones mediante el comando [az functionapp create](/cli/azure/functionapp#az-functionapp-create). En el ejemplo siguiente, reemplace `<STORAGE_NAME>` por el nombre de la cuenta que usó en el paso anterior y `<APP_NAME>` por un nombre único global que le resulte adecuado. `<APP_NAME>` también es el dominio DNS predeterminado de la aplicación de función. 

    ::: zone pivot="programming-language-python"  
    Si usa Python 3.8, cambie `--runtime-version` a `3.8` y `--functions_version` a `3`.
    
    Si usa Python 3.6, cambie `--runtime-version` a `3.6`.

    ```azurecli
    az functionapp create --resource-group AzureFunctionsQuickstart-rg --os-type Linux --consumption-plan-location westeurope --runtime python --runtime-version 3.7 --functions-version 2 --name <APP_NAME> --storage-account <STORAGE_NAME>
    ```
    ::: zone-end  

    ::: zone pivot="programming-language-javascript,programming-language-typescript"  
    Si usa Node.js 8, cambie también `--runtime-version` a `8`.

    
    ```azurecli
    az functionapp create --resource-group AzureFunctionsQuickstart-rg --consumption-plan-location westeurope --runtime node --runtime-version 10 --functions-version 2 --name <APP_NAME> --storage-account <STORAGE_NAME>
    ```
    ::: zone-end  

    ::: zone pivot="programming-language-csharp"  
    ```azurecli
    az functionapp create --resource-group AzureFunctionsQuickstart-rg --consumption-plan-location westeurope --runtime dotnet --functions-version 2 --name <APP_NAME> --storage-account <STORAGE_NAME>
    ```
    ::: zone-end  
    
    ::: zone pivot="programming-language-powershell"  
    ```azurecli
    az functionapp create --resource-group AzureFunctionsQuickstart-rg --consumption-plan-location westeurope --runtime powershell --functions-version 2 --name <APP_NAME> --storage-account <STORAGE_NAME>
    ```
    ::: zone-end  

    Este comando crea una aplicación de funciones que se ejecuta en el entorno de ejecución del lenguaje especificado en el [plan de consumo de Azure Functions](functions-scale.md#consumption-plan), que es gratuito para la cantidad de uso que se realiza aquí. El comando también aprovisiona una instancia asociada de Azure Application Insights en el mismo grupo de recursos con la que puede supervisar la aplicación de funciones y ver registros. Para más información, consulte [Supervisión de Azure Functions](functions-monitoring.md). La instancia no incurrirá en ningún costo hasta que se active.
    
## <a name="deploy-the-function-project-to-azure"></a>Implementación del proyecto de función en Azure

::: zone pivot="programming-language-typescript"  
Antes de usar Core Tools para implementar el proyecto en Azure, creará una compilación lista para producción de archivos de JavaScript a partir de los archivos de origen de TypeScript.

El siguiente comando prepara el proyecto de TypeScript para la implementación:

```
npm run build:production 
```
::: zone-end  

Con los recursos necesarios en vigor, ya está listo para implementar el proyecto de funciones local en la aplicación de funciones de Azure mediante el comando [func azure functionapp publish](functions-run-local.md#project-file-deployment). En el ejemplo siguiente, reemplace `<APP_NAME>` por el nombre de su aplicación.

```
func azure functionapp publish <APP_NAME>
```

Si aparece el error "Can't find app with name..." (No se encuentra la aplicación...), espere unos segundos e inténtelo de nuevo, ya que es posible que Azure no haya inicializado completamente la aplicación después del comando `az functionapp create` anterior.

El comando de publicación muestra un resultado similar a lo que se muestra a continuación (se ha truncado para que resulte más simple):

<pre>
...

Getting site publishing info...
Creating archive for current directory...
Performing remote build for functions project.

...

Deployment successful.
Remote build succeeded!
Syncing triggers...
Functions in msdocs-azurefunctions-qs:
    HttpExample - [httpTrigger]
        Invoke url: https://msdocs-azurefunctions-qs.azurewebsites.net/api/httpexample?code=KYHrydo4GFe9y0000000qRgRJ8NdLFKpkakGJQfC3izYVidzzDN4gQ==
</pre>

## <a name="invoke-the-function-on-azure"></a>Invocación de la función en Azure

Como la función usa un desencadenador HTTP, para invocarla es preciso realizar una solicitud HTTP a su dirección URL en el explorador o con una herramienta como CURL. En ambas instancias, el parámetro de dirección URL `code` es la [clave de función](functions-bindings-http-webhook-trigger.md#authorization-keys) exclusiva que autoriza la invocación del punto de conexión de la función.

# <a name="browser"></a>[Browser](#tab/browser)

Copie la **dirección URL de invocación** completa que se muestra en la salida del comando de publicación en una barra de direcciones del explorador, y anexe el parámetro de consulta `&name=Functions`. El explorador debe mostrar una salida similar a cuando ejecutó la función localmente.

![La salida de la función ejecutada en Azure en un explorador](./media/functions-create-first-azure-function-azure-cli/function-test-cloud-browser.png)


# <a name="curl"></a>[curl](#tab/curl)

Ejecute [`curl`](https://curl.haxx.se/) con la **dirección URL de invocación**, y anexe el parámetro `&name=Functions`. La salida del comando será el texto "Hello Functions".

![La salida de la función ejecutada en Azure en un mediante curl](./media/functions-create-first-azure-function-azure-cli/function-test-cloud-curl.png)

---

> [!TIP]
> Para ver los registros casi en tiempo real de una aplicación de funciones publicada, use [Live Metrics Stream de Application Insights](functions-monitoring.md#streaming-logs).

## <a name="clean-up-resources"></a>Limpieza de recursos

Si continúa con el paso siguiente, [Adición de un enlace de salida de la cola de Azure Storage](functions-add-output-binding-storage-queue-cli.md), conserve todos los recursos intactos, ya que va a crear a partir de lo que ya ha hecho.

De lo contrario, use el siguiente comando para eliminar el grupo de recursos y todos los recursos que contiene para evitar incurrir en costos adicionales.

```azurecli
az group delete --name AzureFunctionsQuickstart-rg
```

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Conexión a una cola de Azure Storage](functions-add-output-binding-storage-queue-cli.md)
