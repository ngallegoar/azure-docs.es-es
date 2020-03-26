---
title: 'Tutorial: Desencadenamiento de un grupo de contenedores mediante una función de Azure'
description: Creación de una función de PowerShell sin servidor desencadenada por HTTP para automatizar la creación de instancias de contenedor de Azure
ms.topic: tutorial
ms.date: 09/20/2019
ms.custom: ''
ms.openlocfilehash: 9dbb22a2449e4c41bff802ab827da4489fc7ffeb
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/24/2020
ms.locfileid: "78331032"
---
# <a name="tutorial-use-an-http-triggered-azure-function-to-create-a-container-group"></a>Tutorial: Uso de una función de Azure desencadenada por HTTP para crear un grupo de contenedores

[Azure Functions](../azure-functions/functions-overview.md) es un servicio de proceso sin servidor que puede ejecutar scripts o código como respuesta a diversos eventos, como una solicitud HTTP, un temporizador o un mensaje en una cola de Azure Storage.

En este tutorial se crea una función de Azure que toma una solicitud HTTP y desencadena la implementación de un [grupo de contenedores](container-instances-container-groups.md). En este ejemplo se muestran los fundamentos del uso de Azure Functions para crear automáticamente recursos en Azure Container Instances. Modifique o extienda el ejemplo a escenarios más complejos o a otros desencadenadores de eventos. 

Aprenderá a:

> [!div class="checklist"]
> * Usar Visual Studio Code con la [extensión de Azure Functions](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurefunctions) para crear una función básica de PowerShell desencadenada por HTTP.
> * Habilitar una identidad en la aplicación de funciones y a darle permisos para crear recursos de Azure.
> * Modificar y volver a publicar la función de PowerShell para automatizar la implementación de un grupo de contenedores con un solo contenedor.
> * Comprobar la implementación desencadenada por HTTP del contenedor.

> [!IMPORTANT]
> PowerShell para Azure Functions actualmente se encuentra disponible en versión preliminar. Las versiones preliminares están a su disposición con la condición de que acepte los [términos de uso adicionales][terms-of-use]. Es posible que algunos de los aspectos de esta característica cambien antes de ofrecer disponibilidad general.

## <a name="prerequisites"></a>Prerrequisitos

Para saber cuáles son los requisitos previos para instalar y usar Visual Studio Code con Azure Functions en su sistema operativo, consulte [Creación de la primera función en Azure](/azure/azure-functions/functions-create-first-function-vs-code?pivots=programming-language-powershell#configure-your-environment).

En algunos de los pasos de este artículo se usa la CLI de Azure. Para realizar estos pasos puede usar Azure Cloud Shell o una instalación local de la CLI de Azure. Si necesita instalarla o actualizarla, vea [Instalación de la CLI de Azure][azure-cli-install].

## <a name="create-a-basic-powershell-function"></a>Creación de una función básica de PowerShell

Siga los pasos de [Creación de la primera función de PowerShell en Azure](../azure-functions/functions-create-first-function-powershell.md) para crear una función de PowerShell mediante la plantilla Desencadenador de HTTP. Use el nombre predeterminado de la función de Azure, **HttpTrigger**. Como se muestra en el inicio rápido, pruebe la función localmente y publique el proyecto en una aplicación de funciones en Azure. Este ejemplo es una función básica desencadenada por HTTP que devuelve una cadena de texto. En pasos posteriores de este artículo, se modificará la función para crear un grupo de contenedores.

En este artículo se supone que publica el proyecto con el nombre *myfunctionapp* en un grupo de recursos de Azure al que se asigna nombre automáticamente según el nombre de la aplicación de funciones (también *myfunctionapp*). Más adelante tendrá que sustituir tanto el nombre único de la aplicación de funciones como el nombre del grupo de recursos.

## <a name="enable-an-azure-managed-identity-in-the-function-app"></a>Habilitación de una identidad administrada por Azure en la aplicación de funciones

Ahora, habilite una [identidad administrada](../app-service/overview-managed-identity.md?toc=/azure/azure-functions/toc.json#add-a-system-assigned-identity) asignada por el sistema en la aplicación de funciones. El host de PowerShell que ejecuta la aplicación se puede autenticar automáticamente mediante esta identidad, lo que permite que las funciones realicen acciones en los servicios de Azure a los que se ha concedido acceso a la identidad. En este tutorial, concederá permisos de identidad administrada para crear recursos en el grupo de recursos de la aplicación de funciones. 

En primer lugar, use el comando [az group show][az-group-show] para obtener el identificador del grupo de recursos de la aplicación de funciones y almacenarlo en una variable de entorno. En este ejemplo se da por supuesto que el comando se ejecuta en un shell de Bash.

```azurecli
rgID=$(az group show --name myfunctionapp --query id --output tsv)
```

Ejecute [az functionapp identity app assign][az-functionapp-identity-app-assign] para asignar una identidad local a la aplicación de funciones y asignar un rol de colaborador al grupo de recursos. Este rol permite a la identidad crear recursos adicionales, como grupos de contenedores, en el grupo de recursos.

```azurecli
az functionapp identity assign \
  --name myfunctionapp \
  --resource-group myfunctionapp \
  --role contributor --scope $rgID
```

## <a name="modify-httptrigger-function"></a>Modificación de la función HttpTrigger

Modifique el código de PowerShell de la función **HttpTrigger** para crear un grupo de contenedores. En el archivo `run.ps1` de la función, busque el siguiente bloque de código. Este código muestra el valor de un nombre, si se ha usado alguno como cadena de consulta en la dirección URL de la función:

```powershell
[...]
if ($name) {
    $status = [HttpStatusCode]::OK
    $body = "Hello $name"
}
[...]
```

Reemplace este código por el siguiente bloque de ejemplo. Aquí, si se pasa el valor de un nombre en la cadena de consulta, se usa para asignar un nombre y crear un grupo de contenedores mediante el cmdlet [New-AzContainerGroup][new-azcontainergroup]. Asegúrese de reemplazar el nombre del grupo de recursos *myfunctionapp* por el nombre del grupo de recursos de la aplicación de funciones:

```powershell
[...]
if ($name) {
    $status = [HttpStatusCode]::OK
    New-AzContainerGroup -ResourceGroupName myfunctionapp -Name $name `
        -Image alpine -OsType Linux `
        -Command "echo 'Hello from an Azure container instance triggered by an Azure function'" `
        -RestartPolicy Never
    $body = "Started container group $name"
}
[...]
```

En este ejemplo se crea un grupo de contenedores que consta de una única instancia de contenedor que ejecuta la imagen `alpine`. El contenedor ejecuta un único comando `echo` y, después, finaliza. En un ejemplo real, podría desencadenar la creación de uno o varios grupos de contenedores para ejecutar un trabajo por lotes.
 
## <a name="test-function-app-locally"></a>Prueba de una aplicación de funciones en un entorno local

Asegúrese de que la función se ejecuta correctamente en un entorno local antes de volver a publicar el proyecto de la aplicación de funciones en Azure. Como se muestra en el [inicio rápido de PowerShell](../azure-functions/functions-create-first-function-powershell.md), inserte un punto de interrupción local en el script de PowerShell y una llamada a `Wait-Debugger` encima. Para obtener instrucciones para realizar la depuración, consulte [Depuración local de Azure Functions de PowerShell](../azure-functions/functions-debug-powershell-local.md).


## <a name="republish-azure-function-app"></a>Republicación de una aplicación de funciones de Azure

Una vez que haya comprobado que la función se ejecuta correctamente en un equipo local es el momento de volver a publicar el proyecto en la aplicación de funciones de Azure.

> [!NOTE]
> No olvide quitar todas las llamadas a `Wait-Debugger` antes de publicar sus funciones en Azure.

1. En Visual Studio Code, abra la paleta de comandos. Busque y seleccione `Azure Functions: Deploy to function app...`.
1. Seleccione la carpeta de trabajo actual para comprimirla y realizar la implementación.
1. Seleccione la suscripción y, después, el nombre de la aplicación de funciones existente (*myfunctionapp*). Confirme que desea sobrescribir la implementación anterior.

Una vez que se haya creado la aplicación de función se mostrará una notificación y se aplicará el paquete de implementación. Seleccione **View Output** (Ver salida) en esta notificación para ver la creación y los resultados de la implementación, incluidos los recursos de Azure que ha actualizado.

## <a name="run-the-function-in-azure"></a>Ejecución de la función en Azure

Una vez que la implementación se complete correctamente, obtenga la dirección URL de la función. Por ejemplo, use el área **Azure: Functions** de Visual Studio Code para copiar la dirección URL de la función **HttpTrigger** u obtenga la dirección URL de la función en [Azure Portal](../azure-functions/functions-create-first-azure-function.md#test-the-function).

La dirección URL de la función incluye un código único y tiene el siguiente formato:

```
https://myfunctionapp.azurewebsites.net/api/HttpTrigger?code=bmF/GljyfFWISqO0GngDPCtCQF4meRcBiHEoaQGeRv/Srx6dRcrk2M==
```

### <a name="run-function-without-passing-a-name"></a>Ejecución de la función sin pasar ningún nombre

Como primera prueba, ejecute el comando `curl` y pase la dirección URL de la función sin anexar una cadena de consulta `name`. Asegúrese de incluir el código único de la función.

```bash
curl --verbose "https://myfunctionapp.azurewebsites.net/api/HttpTrigger?code=bmF/GljyfFWISqO0GngDPCtCQF4meRcBiHEoaQGeRv/Srx6dRcrk2M=="
```

La función devuelve el código de estado 400 y el texto `Please pass a name on the query string or in the request body`:

```
[...]
> GET /api/HttpTrigger?code=bmF/GljyfFWISqO0GngDPCtCQF4meRcBiHEoaQGeRv/Srx6dRcrk2M== HTTP/2
> Host: myfunctionapp.azurewebsites.net
> User-Agent: curl/7.54.0
> Accept: */*
> 
* Connection state changed (MAX_CONCURRENT_STREAMS updated)!
< HTTP/2 400 
< content-length: 62
< content-type: text/plain; charset=utf-8
< date: Mon, 05 Aug 2019 22:08:15 GMT
< 
* Connection #0 to host myfunctionapp.azurewebsites.net left intact
Please pass a name on the query string or in the request body.
```

### <a name="run-function-and-pass-the-name-of-a-container-group"></a>Ejecución de la función y uso del nombre de un grupo de contenedores

Ahora, ejecute el comando `curl`, para lo que debe anexar el nombre de un grupo de contenedores (*mycontainergroup*) como una cadena de consulta `&name=mycontainergroup`:

```bash
curl --verbose "https://myfunctionapp.azurewebsites.net/api/HttpTrigger?code=bmF/GljyfFWISqO0GngDPCtCQF4meRcBiHEoaQGeRv/Srx6dRcrk2M==&name=mycontainergroup"
```

La función devuelve el código de estado 200 y desencadena la creación del grupo de contenedores:

```
[...]
> GET /api/HttpTrigger?ode=bmF/GljyfFWISqO0GngDPCtCQF4meRcBiHEoaQGeRv/Srx6dRcrk2M==&name=mycontainergroup HTTP/2
> Host: myfunctionapp.azurewebsites.net
> User-Agent: curl/7.54.0
> Accept: */*
> 
* Connection state changed (MAX_CONCURRENT_STREAMS updated)!
< HTTP/2 200 
< content-length: 28
< content-type: text/plain; charset=utf-8
< date: Mon, 05 Aug 2019 22:15:30 GMT
< 
* Connection #0 to host myfunctionapp.azurewebsites.net left intact
Started container group mycontainergroup
```

Compruebe que el contenedor se ha ejecutado con el comando [az container logs][az-container-logs]:

```azurecli
az container logs --resource-group myfunctionapp --name mycontainergroup
```

Salida del ejemplo:

```console
Hello from an Azure container instance triggered by an Azure function
```

## <a name="clean-up-resources"></a>Limpieza de recursos

Si ya no necesita ninguno de los recursos creados en este tutorial, puede ejecutar el comando [az group delete][az-group-delete] para quitar el grupo de recursos y todos los recursos que contiene. Este comando elimina el registro de contenedor que se ha creado, así como el contenedor en ejecución y todos los recursos relacionados.

```azurecli-interactive
az group delete --name myfunctionapp
```

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, ha creado una función de Azure que toma una solicitud HTTP y desencadena la implementación de un grupo de contenedores. Ha aprendido a:

> [!div class="checklist"]
> * Usar Visual Studio Code con la extensión de Azure Functions para crear una función básica de PowerShell desencadenada por HTTP.
> * Habilitar una identidad en la aplicación de funciones y a darle permisos para crear recursos de Azure.
> * Modificar el código de la función de PowerShell para automatizar la implementación de un grupo de contenedores con un solo contenedor.
> * Comprobar la implementación desencadenada por HTTP del contenedor.

Para obtener un ejemplo detallado de cómo iniciar y supervisar un trabajo con contenedores, consulte la entrada de blog sobre los [contenedores sin servidor basados en eventos con PowerShell Azure Functions y Azure Container Instances](https://dev.to/azure/event-driven-serverless-containers-with-powershell-azure-functions-and-azure-container-instances-e9b) y el [código de ejemplo](https://github.com/anthonychu/functions-powershell-run-aci) que lo acompaña.

Consulte la [documentación de Azure Functions](/azure/azure-functions/) para obtener instrucciones detalladas sobre la creación de funciones de Azure y la publicación de un proyecto de Functions. 

<!-- IMAGES -->


<!-- LINKS - external -->
[terms-of-use]: https://azure.microsoft.com/support/legal/preview-supplemental-terms/

<!-- LINKS - internal -->

[azure-cli-install]: /cli/azure/install-azure-cli
[az-group-show]: /cli/azure/group#az-group-show
[az-group-delete]: /cli/azure/group#az-group-delete
[az-functionapp-identity-app-assign]: /cli/azure/functionapp/identity#az-functionapp-identity-assign
[new-azcontainergroup]: /powershell/module/az.containerinstance/new-azcontainergroup
[az-container-logs]: /cli/azure/container#az-container-logs
