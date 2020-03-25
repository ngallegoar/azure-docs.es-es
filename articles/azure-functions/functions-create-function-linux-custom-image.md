---
title: Creación de funciones de Azure en Linux mediante una imagen personalizada
description: Aprenda a crear funciones de Azure que se ejecutan en una imagen de Linux personalizada.
ms.date: 01/15/2020
ms.topic: tutorial
ms.custom: mvc
zone_pivot_groups: programming-languages-set-functions
ms.openlocfilehash: 8c074c677c645dd03e3cf5288d82aa3e65720e8b
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/24/2020
ms.locfileid: "79223732"
---
# <a name="create-a-function-on-linux-using-a-custom-container"></a>Creación de una función en Linux con un contenedor personalizado

En este tutorial, va a crear e implementar código de Python en Azure Functions como un contenedor de Docker personalizado mediante una imagen base de Linux. Normalmente usa una imagen personalizada cuando las funciones necesitan una versión de lenguaje determinada o requieren una dependencia o configuración específica que no se proporciona en la imagen integrada.

También puede usar un contenedor de Azure App Service predeterminado como se indica en [Creación de su primera función hospedada en Linux](functions-create-first-azure-function-azure-cli-linux.md). Se pueden encontrar imágenes base admitidas para Azure Functions en el [repositorio de imágenes base de Azure Functions](https://hub.docker.com/_/microsoft-azure-functions-base).

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Crear una aplicación de funciones y un archivo Dockerfile mediante Azure Functions Core Tools.
> * Crear una imagen personalizada con Docker
> * Publicar una imagen personalizada en un registro de contenedor
> * Crear recursos auxiliares en Azure para la aplicación de funciones
> * Implementar una aplicación de función desde Docker Hub
> * Agregar la configuración de aplicación a la aplicación de función
> * Habilite la implementación continua.
> * Habilitar las conexiones SSH al contenedor.
> * Agregar un enlace de salida de Queue Storage. 

Puede seguir este tutorial en cualquier equipo que ejecute Windows, MacOS o Linux. Completar el tutorial hará que incurra en un pequeño costo en su cuenta de Azure.

## <a name="prerequisites"></a>Prerequisites

- Una cuenta de Azure con una suscripción activa. [Cree una cuenta gratuita](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio).
- [Azure Functions Core Tools](./functions-run-local.md#v2) versión 2.7.1846 u otra posterior.
- La [CLI de Azure](/cli/azure/install-azure-cli), versión 2.0.77 o cualquier versión posterior.
- El [runtime de Azure Functions 2.x](functions-versions.md).
- Los siguientes componentes de Language Runtime:
    ::: zone pivot="programming-language-csharp"
    - [.NET Core 2.2.x o versiones posteriores](https://dotnet.microsoft.com/download)
    ::: zone-end
    ::: zone pivot="programming-language-javascript"
    - [Node.js](https://nodejs.org/en/download/)
    ::: zone-end
    ::: zone pivot="programming-language-powershell"
    - [PowerShell](/powershell/scripting/install/installing-windows-powershell?view=powershell-7)
    ::: zone-end
    ::: zone pivot="programming-language-python"
    - [Python 3.6 de 64 bits](https://www.python.org/downloads/release/python-3610/) o [Python 3.7 de 64 bits](https://www.python.org/downloads/release/python-376/)
    ::: zone-end
    ::: zone pivot="programming-language-typescript"
    - [Node.js](https://nodejs.org/en/download/)
    - [TypeScript](http://www.typescriptlang.org/#download-links)
    ::: zone-end
- [Docker](https://docs.docker.com/install/)
- Un [identificador de Docker](https://hub.docker.com/signup)

### <a name="prerequisite-check"></a>Comprobación de requisitos previos

1. En una ventana de terminal o de comandos, ejecute `func --version` para comprobar que la versión de Azure Functions Core Tools es 2.7.1846 u otra posterior.
1. Ejecute `az --version` para comprobar que la versión de la CLI de Azure es 2.0.76 o posterior.
1. Ejecute `az login` para iniciar sesión en Azure y comprobar una suscripción activa.
1. Ejecute `docker login` para iniciar sesión en Docker. Este comando genera un error si no se está ejecutando Docker. Si este es el caso, debe iniciarlo y volver a intentar el comando.

## <a name="create-and-test-the-local-functions-project"></a>Creación y prueba de un proyecto local de Functions

1. En un símbolo del sistema o terminal, cree una carpeta para este tutorial en una ubicación adecuada y, a continuación, vaya a esa carpeta.

1. Siga las instrucciones de [Creación y activación de un entorno virtual](/azure/azure-functions/functions-create-first-azure-function-azure-cli?pivots=programming-language-python#create-venv) para crear un entorno virtual para usarlo en este tutorial.

1. Ejecute el siguiente comando para el lenguaje elegido para crear un proyecto de aplicación de funciones en una carpeta denominada `LocalFunctionsProject`. La opción `--docker` genera un archivo `Dockerfile` para el proyecto que permite definir un contenedor personalizado adecuado para su uso con Azure Functions y el entorno de ejecución seleccionado.

    ::: zone pivot="programming-language-csharp"
    ```
    func init LocalFunctionsProject --worker-runtime dotnet --docker
    ```
    ::: zone-end

    ::: zone pivot="programming-language-javascript"
    ```
    func init LocalFunctionsProject --worker-runtime node --language javascript --docker
    ```
    ::: zone-end

    ::: zone pivot="programming-language-powershell"
    ```
    func init LocalFunctionsProject --worker-runtime powershell --docker
    ```
    ::: zone-end

    ::: zone pivot="programming-language-python"
    ```
    func init LocalFunctionsProject --worker-runtime python --docker
    ```
    ::: zone-end

    ::: zone pivot="programming-language-typescript"
    ```
    func init LocalFunctionsProject --worker-runtime node --language typescript --docker
    ```
    ::: zone-end
    
1. Vaya a la carpeta del proyecto:

    ```
    cd LocalFunctionsProject
    ```
    
1. Agregue una función al proyecto mediante el comando siguiente, donde el argumento `--name` es el nombre único de la función y el argumento `--template` especifica el desencadenador de esta. `func new` crea una subcarpeta que coincide con el nombre de la función y que contiene un archivo de código apropiado para el lenguaje elegido del proyecto y un archivo de configuración denominado *function.json*.

    ```
    func new --name HttpExample --template "HTTP trigger"
    ```

1. Para probar la función localmente, inicie el host en tiempo de ejecución de Azure Functions local en la carpeta *LocalFunctionsProject*:
   
    ::: zone pivot="programming-language-csharp"
    ```
    func start --build
    ```
    ::: zone-end

    ::: zone pivot="programming-language-javascript"
    ```
    func start
    ```
    ::: zone-end

    ::: zone pivot="programming-language-powershell"
    ```
    func start
    ```
    ::: zone-end

    ::: zone pivot="programming-language-python"
    ```
    func start
    ```
    ::: zone-end    

    ::: zone pivot="programming-language-typescript"
    ```
    npm install
    ```

    ```
    npm start
    ```
    ::: zone-end

1. Una vez que vea el punto de conexión `HttpExample` aparecer en la salida, vaya a `http://localhost:7071/api/HttpExample?name=Functions`. El explorador debe mostrar un mensaje del tipo "Hola, Functions" (puede variar ligeramente según el lenguaje de programación elegido).

1. Use **Ctrl**-**C** para detener el host.

## <a name="build-the-container-image-and-test-locally"></a>Creación de la imagen de contenedor y prueba local

1. (Opcional) Examine el archivo *Dockerfile en la carpeta *LocalFunctionsProj*. Este archivo describe el entorno necesario para ejecutar la aplicación de funciones en Linux: 

    ::: zone pivot="programming-language-csharp"
    ```Dockerfile
    FROM microsoft/dotnet:2.2-sdk AS installer-env

    COPY . /src/dotnet-function-app
    RUN cd /src/dotnet-function-app && \
        mkdir -p /home/site/wwwroot && \
        dotnet publish *.csproj --output /home/site/wwwroot
    
    # To enable ssh & remote debugging on app service change the base image to the one below
    # FROM mcr.microsoft.com/azure-functions/dotnet:2.0-appservice 
    FROM mcr.microsoft.com/azure-functions/dotnet:2.0
    ENV AzureWebJobsScriptRoot=/home/site/wwwroot \
        AzureFunctionsJobHost__Logging__Console__IsEnabled=true
    
    COPY --from=installer-env ["/home/site/wwwroot", "/home/site/wwwroot"]
    ```
    ::: zone-end

    ::: zone pivot="programming-language-javascript"
    ```Dockerfile
    # To enable ssh & remote debugging on app service change the base image to the one below
    # FROM mcr.microsoft.com/azure-functions/node:2.0-appservice
    FROM mcr.microsoft.com/azure-functions/node:2.0
    
    ENV AzureWebJobsScriptRoot=/home/site/wwwroot \
        AzureFunctionsJobHost__Logging__Console__IsEnabled=true
    
    COPY . /home/site/wwwroot
    
    RUN cd /home/site/wwwroot && \
    npm install    
    ```
    ::: zone-end

    ::: zone pivot="programming-language-powershell"
    ```Dockerfile
    # To enable ssh & remote debugging on app service change the base image to the one below
    # FROM mcr.microsoft.com/azure-functions/powershell:2.0-appservice
    FROM mcr.microsoft.com/azure-functions/powershell:2.0
    ENV AzureWebJobsScriptRoot=/home/site/wwwroot \
        AzureFunctionsJobHost__Logging__Console__IsEnabled=true
    
    COPY . /home/site/wwwroot    
    ```
    ::: zone-end

    ::: zone pivot="programming-language-python"
    ```Dockerfile
    # To enable ssh & remote debugging on app service change the base image to the one below
    # FROM mcr.microsoft.com/azure-functions/python:2.0-python3.7-appservice
    FROM mcr.microsoft.com/azure-functions/python:2.0-python3.7
    
    ENV AzureWebJobsScriptRoot=/home/site/wwwroot \
        AzureFunctionsJobHost__Logging__Console__IsEnabled=true
    
    COPY requirements.txt /
    RUN pip install -r /requirements.txt
    
    COPY . /home/site/wwwroot    
    ```
    ::: zone-end

    ::: zone pivot="programming-language-typescript"
    ```Dockerfile
    # To enable ssh & remote debugging on app service change the base image to the one below
    # FROM mcr.microsoft.com/azure-functions/node:2.0-appservice
    FROM mcr.microsoft.com/azure-functions/node:2.0
    
    ENV AzureWebJobsScriptRoot=/home/site/wwwroot \
        AzureFunctionsJobHost__Logging__Console__IsEnabled=true
    
    COPY . /home/site/wwwroot
    
    RUN cd /home/site/wwwroot && \
    npm install    
    ```
    ::: zone-end

    > [!NOTE]
    > La lista completa de imágenes base admitidas para Azure Functions se puede encontrar en la [página de imágenes base de Azure Functions](https://hub.docker.com/_/microsoft-azure-functions-base).
    
1. En la carpeta *LocalFunctionsProject*, ejecute el comando [docker build](https://docs.docker.com/engine/reference/commandline/build/) y especifique un nombre, `azurefunctionsimage`, y una etiqueta, `v1.0.0`. Reemplace `<docker_id>` por el identificador de su cuenta de Docker Hub. Este comando compila la imagen de Docker del contenedor.

    ```
    docker build --tag <docker_id>/azurefunctionsimage:v1.0.0 .
    ```
    
    Cuando el comando se complete, podrá ejecutar el nuevo contenedor de forma local.
    
1. Para probar la compilación, ejecute la imagen en un contenedor local con el comando [docker run](https://docs.docker.com/engine/reference/commandline/run/). Para ello, reemplace de nuevo `<docker_id>` por el identificador de Docker y agregue el argumento de puertos, `-p 8080:80`:

    ```
    docker run -p 8080:80 -it <docker_id>/azurefunctionsimage:v1.0.0
    ```
    
1. Una vez que la imagen se esté ejecutando en un contenedor local, abra un explorador que vaya a `http://localhost:8080` y aquí debería aparecer la imagen del marcador de posición que se muestra a continuación. La imagen aparece en este momento porque la función se ejecuta en el contenedor local, como lo haría en Azure, lo que significa que está protegida por una clave de acceso, como se define en *function.json* con la propiedad `"authLevel": "function"`. No obstante, el contenedor no se ha publicado aún en una aplicación de funciones de Azure, por lo que la clave aún no está disponible. Si desea realizar una prueba localmente, detenga Docker, cambie la propiedad de autorización a `"authLevel": "anonymous"`, vuelva a compilar la imagen y reinicie Docker. Después, restablezca `"authLevel": "function"` en *function.json*. Para más información, consulte [Claves de autorización](functions-bindings-http-webhook-trigger.md#authorization-keys).

    ![Imagen de marcador de posición que indica que el contenedor se está ejecutando localmente](./media/functions-create-function-linux-custom-image/run-image-local-success.png)

1. Después de haber comprobado la aplicación de funciones en el contenedor, detenga Docker con **Ctrl**+**C**.

## <a name="push-the-image-to-docker-hub"></a>Inserción de la imagen en Docker Hub

Docker Hub es un registro de contenedor que hospeda imágenes y proporciona servicios de imágenes y contenedores. Para compartir la imagen, lo cual incluye la implementación en Azure, debe insertarla en un registro.

1. Si aún no ha iniciado sesión en Docker, hágalo con el comando [docker login](https://docs.docker.com/engine/reference/commandline/login/), reemplazando `<docker_id>` por el identificador de Docker. Este comando le solicita su nombre de usuario y contraseña. Un mensaje de inicio de sesión correcto confirmará que ha iniciado sesión.

    ```
    docker login
    ```
    
1. Cuando haya iniciado sesión, inserte la imagen en Docker Hub mediante el comando [docker push](https://docs.docker.com/engine/reference/commandline/push/) y reemplazando de nuevo `<docker_id>` por el identificador de Docker.

    ```
    docker push <docker_id>/azurefunctionsimage:v1.0.0
    ```

1. Según la velocidad de la red, la inserción de la imagen la primera vez puede tardar unos minutos (la inserción de los cambios posteriores es mucho más rápida). Mientras espera, puede continuar con la sección siguiente y crear recursos de Azure en otro terminal.

## <a name="create-supporting-azure-resources-for-your-function"></a>Creación de recursos auxiliares de Azure para la función

Para implementar el código de la función en Azure, debe crear tres recursos:

- Un grupo de recursos, que es un contenedor lógico de recursos relacionados.
- Una cuenta de almacenamiento, que mantiene el estado e información adicional sobre los proyectos.
- Una aplicación de funciones de Azure, que proporciona el entorno para ejecutar el código de las funciones. Una aplicación de funciones se asigna al proyecto de funciones y le permite agrupar funciones como una unidad lógica para facilitar la administración, la implementación y el uso compartido de recursos.

Puede utilizar comandos de la CLI de Azure para crear estos elementos. Cada comando proporciona una salida JSON al finalizar.

1. Inicie sesión en Azure con el comando [az login](/cli/azure/reference-index#az-login):

    ```azurecli
    az login
    ```
    
1. Para crear un grupo de recursos, use el comando [az group create](/cli/azure/group#az-group-create). En el ejemplo siguiente se crea un grupo de recursos denominado `AzureFunctionsContainers-rg` en la región `westeurope`. (Por lo general, tanto los grupos de recursos como los recursos se crean en una región cercana y se utiliza alguna de las regiones disponibles del comando `az account list-locations`).

    ```azurecli
    az group create --name AzureFunctionsContainers-rg --location westeurope
    ```
    
    > [!NOTE]
    > No se pueden hospedar aplicaciones de Windows y Linux en el mismo grupo de recursos. Si tiene un grupo de recursos existente llamado `AzureFunctionsContainers-rg` con una aplicación web o aplicación de función de Windows, debe usar un grupo de recursos diferente.
    
1. Cree una cuenta de almacenamiento de uso general en el grupo de recursos con el comando [az storage account create](/cli/azure/storage/account#az-storage-account-create). En el siguiente ejemplo, reemplace `<storage_name>` por un nombre único global que sea adecuado. Los nombres deben contener entre 3 y 24 caracteres y solo letras minúsculas. `Standard_LRS` especifica una cuenta típica de uso general.

    ```azurecli
    az storage account create --name <storage_name> --location westeurope --resource-group AzureFunctionsContainers-rg --sku Standard_LRS
    ```
    
    La cuenta de almacenamiento incurrirá solo en un costo insignificante para este tutorial.
    
1. Use el comando para crear un plan Premium para Azure Functions denominado `myPremiumPlan` en el plan de tarifa **Premium elástico 1** (`--sku EP1`), en la región Oeste de Europa (`-location westeurope`, o use una región adecuada cerca de usted) y en un contenedor de Linux (`--is-linux`).

    ```azurecli
    az functionapp plan create --resource-group AzureFunctionsContainers-rg --name myPremiumPlan --location westeurope --number-of-workers 1 --sku EP1 --is-linux
    ```   

    Se admite el hospedaje en Linux de contenedores de Functions personalizados en [planes dedicados (App Service)](functions-scale.md#app-service-plan) y [planes Premium](functions-premium-plan.md#features). Aquí se usa el plan Premium, que se puede escalar según sea necesario. Para obtener más información sobre el hospedaje, consulte [Comparación de los planes de hospedaje de Azure Functions](functions-scale.md). Para calcular los costos, consulte la [página de precios de Functions](https://azure.microsoft.com/pricing/details/functions/).

    El comando también aprovisiona una instancia asociada de Azure Application Insights en el mismo grupo de recursos con la que puede supervisar la aplicación de funciones y ver registros. Para más información, consulte [Supervisión de Azure Functions](functions-monitoring.md). La instancia no incurrirá en ningún costo hasta que se active.

## <a name="create-and-configure-a-function-app-on-azure-with-the-image"></a>Creación y configuración de una aplicación de funciones en Azure con la imagen

Una aplicación de funciones en Azure administra la ejecución de las funciones en el plan de hospedaje. En esta sección, usará los recursos de Azure de la sección anterior para crear una aplicación de funciones a partir de una imagen en Docker Hub y configurarla con una cadena de conexión a Azure Storage.

1. Cree la aplicación de funciones con el comando [az functionapp create](/cli/azure/functionapp#az-functionapp-create). En el ejemplo siguiente, reemplace `<storage_name>` por el nombre que utilizó en la sección anterior para la cuenta de almacenamiento. Reemplace también `<app_name>` por un nombre único global que sea adecuado y `<docker_id>` por el identificador de Docker.

    ```azurecli
    az functionapp create --name <app_name> --storage-account <storage_name> --resource-group AzureFunctionsContainers-rg --plan myPremiumPlan --deployment-container-image-name <docker_id>/azurefunctionsimage:v1.0.0
    ```
    
    El parámetro *deployment-container-image-name* especifica la imagen que se va a usar para la aplicación de funciones. Puede usar el comando [az functionapp config container show](/cli/azure/functionapp/config/container#az-functionapp-config-container-show) para ver información sobre la imagen que se utilizó en la implementación. También puede usar el comando [az functionapp config container set](/cli/azure/functionapp/config/container#az-functionapp-config-container-set) para realizar la implementación desde una imagen diferente.

1. Recupere la cadena de conexión para la cuenta de almacenamiento que creó mediante el comando [az storage account show-connection-string](/cli/azure/storage/account), asignándolo a una variable del shell `storageConnectionString`:

    ```azurecli
    az storage account show-connection-string --resource-group AzureFunctionsContainers-rg --name <storage_name> --query connectionString --output tsv
    ```
    
1. Agregue esta configuración a la aplicación de funciones mediante el comando [az functionapp config appsettings set](/cli/azure/functionapp/config/appsettings#az-functionapp-config-appsettings-set). En el siguiente comando, reemplace `<app_name>` por el nombre de la aplicación de funciones y reemplace `<connection_string>` por la cadena de conexión del paso anterior (una cadena larga codificada que comienza por "DefaultEndpointProtocol ="):
 
    ```azurecli
    az functionapp config appsettings set --name <app_name> --resource-group AzureFunctionsContainers-rg --settings AzureWebJobsStorage=<connection_string>
    ```

1. La función ahora puede usar esta cadena de conexión para acceder a la cuenta de almacenamiento.

> [!TIP]
> En Bash, puede usar una variable del shell para capturar la cadena de conexión en lugar de usar el portapapeles. En primer lugar, use el siguiente comando para crear una variable con la cadena de conexión:
> 
> ```bash
> storageConnectionString=$(az storage account show-connection-string --resource-group AzureFunctionsContainers-rg --name <storage_name> --query connectionString --output tsv)
> ```
> 
> Después, haga referencia a la variable en el segundo comando:
> 
> ```azurecli
> az functionapp config appsettings set --name <app_name> --resource-group AzureFunctionsContainers-rg --settings AzureWebJobsStorage=$storageConnectionString
> ```

> [!NOTE]    
> Si publica la imagen personalizada en una cuenta de contenedor privada, debe usar, en su lugar, variables de entorno en el archivo Dockerfile para la cadena de conexión. Para más información, consulte la [instrucción ENV](https://docs.docker.com/engine/reference/builder/#env). También debe establecer las variables `DOCKER_REGISTRY_SERVER_USERNAME` y `DOCKER_REGISTRY_SERVER_PASSWORD`. Para usar los valores, debe recompilar la imagen, insertarla en el registro y, a continuación, reiniciar la aplicación de funciones en Azure.

## <a name="verify-your-functions-on-azure"></a>Comprobación de las funciones en Azure

Con la imagen implementada en la aplicación de funciones en Azure, ya puede invocar la función mediante solicitudes HTTP. Dado que la definición de *function.json* incluye la propiedad `"authLevel": "function"`, primero debe obtener la clave de acceso (también denominada "clave de función") e incluirla como parámetro de dirección URL en las solicitudes al punto de conexión.

1. Recupere la dirección URL con la clave de acceso (función) mediante Azure Portal o mediante la CLI de Azure con el comando `az rest`.

    # <a name="portal"></a>[Portal](#tab/portal)

    1. Inicie sesión en Azure Portal y, a continuación, busque la aplicación de funciones. Para ello, escriba el nombre de esta en el cuadro de **búsqueda** en la parte superior de la página. En los resultados, seleccione el recurso **App Service**.

    1. En el panel de navegación izquierdo, en **Funciones (solo lectura)** , seleccione el nombre de la función.

    1. En el panel de detalles, seleccione **</> Obtener la dirección URL de la función**:
    
        ![Comando Obtener la dirección URL de la función en Azure Portal](./media/functions-create-function-linux-custom-image/functions-portal-get-url-key.png)   

    1. En el menú emergente, seleccione **valor predeterminado (clave de función)** y, a continuación, **Copiar**. La clave es la cadena de caracteres que sigue a `?code=`.

        ![Copia de la dirección URL de la función desde Azure Portal](./media/functions-create-function-linux-custom-image/functions-portal-get-url-key-popup.png)   

    > [!NOTE]  
    > Dado que la aplicación de función se implementa como un contenedor, no se pueden realizar cambios en el código de la función en el portal. En su lugar, debe actualizar el proyecto en la imagen local, insertar de nuevo la imagen en el registro y, a continuación, volver a implementarlo en Azure. Puede configurar la implementación continua en una sección posterior.
    
    # <a name="azure-cli"></a>[CLI de Azure](#tab/azurecli)

    1. Construya una cadena de dirección URL con el formato siguiente, reemplazando `<subscription_id>`, `<resource_group>` y `<app_name>` por el identificador de suscripción de Azure, el grupo de recursos de la aplicación de funciones y el nombre de esta, respectivamente:

        ```
        "/subscriptions/<subscription_id>/resourceGroups/<resource_group>/providers/Microsoft.Web/sites/<app_name>/host/default/listKeys?api-version=2018-11-01"
        ```

        Por ejemplo, la dirección URL podría ser parecida a la siguiente:

        ```
        "/subscriptions/1234aaf4-1234-abcd-a79a-245ed34eabcd/resourceGroups/AzureFunctionsContainers-rg/providers/Microsoft.Web/sites/msdocsfunctionscontainer/host/default/listKeys?api-version=2018-11-01"
        ```

        > [!TIP]
        > Para mayor comodidad, puede asignar la dirección URL a una variable de entorno y usarla en el comando `az rest`.
    
    1. Ejecute el siguiente comando `az rest` (disponible en la versión 2.0.77 y posteriores de la CLI de Azure), reemplazando `<uri>` por la cadena de URI del último paso, incluidas las comillas:

        ```azurecli
        az rest --method post --uri <uri> --query functionKeys.default --output tsv
        ```

    1. La salida del comando es la clave de función. La dirección URL completa de la función es `https://<app_name>.azurewebsites.net/api/<function_name>?code=<key>`, reemplazando `<app_name>`, `<function_name>` y `<key>` por sus valores específicos.
    
        > [!NOTE]
        > La clave recuperada aquí es la clave de *host* que funciona para todas las funciones de la aplicación. El método que se muestra para el portal recupera la clave solo para una función.

    ---

1. Pegue la dirección URL de la función en la barra de direcciones del explorador y agregue el parámetro `&name=Azure` al final de esta dirección URL. Ahora debería aparecer un texto del tipo "Hello Azure" en el explorador.

    ![Respuesta de la función en el explorador.](./media/functions-create-function-linux-custom-image/function-app-browser-testing.png)

1. Para probar la autorización, quite el parámetro code= de la dirección URL y compruebe que no obtiene ninguna respuesta de la función.


## <a name="enable-continuous-deployment-to-azure"></a>Habilitar implementación continua en Azure

Puede habilitar Azure Functions para que actualice automáticamente la implementación de una imagen cada vez que actualice la imagen en el registro.

1. Habilite la implementación continua mediante el comando [az functionapp deployment container config](/cli/azure/functionapp/deployment/container#az-functionapp-deployment-container-config), reemplazando `<app_name>` por el nombre de la aplicación de funciones:

    ```azurecli
    az functionapp deployment container config --enable-cd --query CI_CD_URL --output tsv --name <app_name> --resource-group AzureFunctionsContainers-rg
    ```
    
    Este comando permite habilitar la implementación continua y devuelve la dirección URL del webhook de implementación. (Puede recuperar esta dirección URL en cualquier momento posterior mediante el comando [az functionapp deployment container show-cd-url](/cli/azure/functionapp/deployment/container#az-functionapp-deployment-container-show-cd-url).)

1. Copie la dirección URL del webhook de implementación en el portapapeles.

1. Abra [Docker Hub](https://hub.docker.com/), inicie sesión y seleccione **Repositorios** en la barra de navegación. Busque y seleccione la imagen, seleccione la pestaña **Webhooks**, especifique un **nombre de webhook**, pegue la dirección URL en **Dirección URL de webhook** y, finalmente, seleccione **Crear**:

    ![Agregar el webhook al repositorio de DockerHub](./media/functions-create-function-linux-custom-image/dockerhub-set-continuous-webhook.png)  

1. Una vez establecido el webhook, Azure Functions volverá a implementar la imagen cada vez que la actualice en Docker Hub.

## <a name="enable-ssh-connections"></a>Habilitación de las conexiones SSH

SSH habilita la comunicación segura entre un contenedor y un cliente. Con SSH habilitado, puede conectarse al contenedor mediante las herramientas avanzadas de App Service (Kudu). Para facilitar la conexión al contenedor mediante SSH, Azure Functions proporciona una imagen base que ya tiene SSH habilitado. Solo necesita editar el archivo de Dockerfile y, después, recompilar y volver a implementar la imagen. Después, puede conectar el contenedor mediante Herramientas avanzadas (Kudu)

1. En el archivo de Dockerfile, anexe la cadena `-appservice` a la imagen base en la instrucción `FROM`:

    ::: zone pivot="programming-language-csharp"
    ```Dockerfile
    FROM microsoft/dotnet:2.2-sdk-appservice AS installer-env
    ```
    ::: zone-end

    ::: zone pivot="programming-language-javascript"
    ```Dockerfile
    FROM mcr.microsoft.com/azure-functions/node:2.0-appservice
    ```
    ::: zone-end

    ::: zone pivot="programming-language-powershell"
    ```Dockerfile
    FROM mcr.microsoft.com/azure-functions/powershell:2.0-appservice
    ```
    ::: zone-end

    ::: zone pivot="programming-language-python"
    ```Dockerfile
    FROM mcr.microsoft.com/azure-functions/python:2.0-python3.7-appservice
    ```
    ::: zone-end

    ::: zone pivot="programming-language-typescript"
    ```Dockerfile
    FROM mcr.microsoft.com/azure-functions/node:2.0-appservice
    ```
    ::: zone-end

    Las diferencias entre las imágenes base se describen en el [tutorial App Services: Imágenes personalizadas de Docker](../app-service/containers/tutorial-custom-docker-image.md#enable-ssh-connections).

1. Vuelva a recompilar la imagen mediante el comando `docker build`, sustituyendo `<docker_id>` por el identificador de Docker:

    ```
    docker build --tag <docker_id>/azurefunctionsimage:v1.0.0 .
    ```
    
1. Inserte la imagen actualizada en Docker Hub, lo cual ahora debe tardar mucho menos tiempo que la primera vez ya que solo se cargarán los segmentos actualizados de la imagen.

    ```
    docker push <docker_id>/azurefunctionsimage:v1.0.0
    ```
    
1. Azure Functions vuelve a implementar automáticamente la imagen en la aplicación de funciones. El proceso se realiza en menos de un minuto.

1. En un explorador, abra `https://<app_name>.scm.azurewebsites.net/`, sustituyendo `<app_name>` por su nombre único. Esta dirección URL es el punto de conexión de Herramientas avanzadas (KUDU) para el contenedor de la aplicación de funciones.

1. Inicie sesión en su cuenta de Azure y, después, seleccione **SSH** para establecer una conexión con el contenedor. La conexión puede tardar unos minutos si Azure todavía está en el proceso de actualización de la imagen de contenedor.

1. Una vez establecida la conexión con el contenedor, ejecute el comando `top` para ver los procesos que se están ejecutando actualmente. 

    ![Comando top de Linux en ejecución en una sesión SSH](media/functions-create-function-linux-custom-image/linux-custom-kudu-ssh-top.png)

## <a name="write-to-an-azure-storage-queue"></a>Escritura en una cola de Azure Storage

Azure Functions permite conectar funciones a otros servicios y recursos de Azure sin tener que escribir su propio código de integración. Estos *enlaces*, que representan la entrada y la salida, se declaran dentro de la definición de función. Los datos de los enlaces se proporcionan a la función como parámetros. Un *desencadenador* es un tipo especial de enlace de entrada. Si bien una función tiene un único desencadenador, puede tener varios enlaces de entrada y salida. Para más información, consulte [Conceptos básicos sobre los enlaces y desencadenadores de Azure Functions](functions-triggers-bindings.md).

En esta sección se muestra cómo integrar la función con una cola de Azure Storage. El enlace de salida que se agrega a esta función escribe datos de una solicitud HTTP en un mensaje de la cola.

## <a name="retrieve-the-azure-storage-connection-string"></a>Recuperación de la cadena de conexión de Azure Storage

Anteriormente, creó una cuenta de Azure Storage para que la aplicación de funciones la utilizara. La cadena de conexión de esta cuenta se almacena de forma segura en la configuración de la aplicación en Azure. Mediante la descarga de la configuración en el archivo *local.settings.json*, puede usar esa conexión para escribir en una cola de Storage de la misma cuenta cuando ejecute la función de forma local. 

1. En la raíz del proyecto, ejecute el comando siguiente, reemplazando `<app_name>` por el nombre de la aplicación de funciones del inicio rápido anterior. Este comando sobrescribirá los valores existentes en el archivo.

    ```
    func azure functionapp fetch-app-settings <app_name>
    ```
    
1. Abra *local.settings.json* y busque el valor denominado `AzureWebJobsStorage`, que es la cadena de conexión de la cuenta de almacenamiento. Usará el nombre `AzureWebJobsStorage` y la cadena de conexión en otras secciones de este artículo.

> [!IMPORTANT]
> Como *local.settings.json* contiene secretos descargados de Azure, excluya siempre este archivo del control de código fuente. El archivo *.gitignore* que se creó con un proyecto de Functions local excluye el archivo de forma predeterminada.

### <a name="add-an-output-binding-to-functionjson"></a>Adición de un enlace de salida a function.json

En Azure Functions, para cada tipo de enlace es necesario definir los elementos `direction`, `type` y un valor único de `name` en el archivo *function.json*. El archivo *function.json* ya incluye un enlace de entrada para el tipo "httpTrigger" y un enlace de salida para la respuesta HTTP. Para agregar un enlace a una cola de almacenamiento, modifique el archivo como se indica a continuación, con lo cual podrá agregar un enlace de salida para el tipo "queue", en el que la cola aparece en el código como un argumento de entrada denominado `msg`. El enlace de la cola también necesita el nombre de la cola que se va a usar, en este caso `outqueue`, y el nombre de la configuración que contiene la cadena de conexión, en este caso `AzureWebJobStorage`.

::: zone pivot="programming-language-csharp"

En un proyecto de biblioteca de clases de C#, los enlaces se definen como atributos de enlace en el método de función. Posteriormente, el archivo *function.json* se genera automáticamente en función de estos atributos.

1. Para el enlace de cola, ejecute el siguiente comando [dotnet add package](/dotnet/core/tools/dotnet-add-package) para agregar el paquete de extensión de Storage al proyecto.

    ```
    dotnet add package Microsoft.Azure.WebJobs.Extensions.Storage --version 3.0.4
    ```

1. Abra el archivo *HttpTrigger.cs* y agregue la siguiente instrucción `using`:

    ```cs
    using Microsoft.Azure.WebJobs.Extensions.Storage;
    ```
    
1. Agregue el siguiente parámetro a la definición de método `Run`:
    
    ```csharp
    [Queue("outqueue"), StorageAccount("AzureWebJobsStorage")] ICollector<string> msg
    ```
    
    La definición del método `Run` debe ahora parecerse a la del siguiente código:
    
    ```csharp
    [FunctionName("HttpTrigger")]
    public static async Task<IActionResult> Run(
        [HttpTrigger(AuthorizationLevel.Function, "get", "post", Route = null)] HttpRequest req, 
        [Queue("outqueue"), StorageAccount("AzureWebJobsStorage")] ICollector<string> msg, ILogger log)
    ```

El parámetro `msg` es de tipo `ICollector<T>`, que representa una colección de mensajes escritos en un enlace de salida cuando se completa la función. En este caso, la salida es una cola de almacenamiento denominada `outqueue`. La cadena de conexión de la cuenta de Storage la establece `StorageAccountAttribute`. Este atributo indica la configuración que contiene la cadena de conexión de la cuenta de Storage y se puede aplicar en el nivel de clase, método o parámetro. En este caso, puede omitir `StorageAccountAttribute`, puesto que ya está usando la cuenta de almacenamiento predeterminada.

::: zone-end

::: zone pivot="programming-language-javascript"

Actualice *function.json* para que coincida con lo siguiente mediante la adición del enlace de cola después del enlace HTTP:

```json
{
  "bindings": [
    {
      "authLevel": "function",
      "type": "httpTrigger",
      "direction": "in",
      "name": "req",
      "methods": [
        "get",
        "post"
      ]
    },
    {
      "type": "http",
      "direction": "out",
      "name": "res"
    },
    {
      "type": "queue",
      "direction": "out",
      "name": "msg",
      "queueName": "outqueue",
      "connection": "AzureWebJobsStorage"
    }
  ]
}
```
::: zone-end

::: zone pivot="programming-language-powershell"

Actualice *function.json* para que coincida con lo siguiente mediante la adición del enlace de cola después del enlace HTTP:

```json
{
  "bindings": [
    {
      "authLevel": "function",
      "type": "httpTrigger",
      "direction": "in",
      "name": "Request",
      "methods": [
        "get",
        "post"
      ]
    },
    {
      "type": "http",
      "direction": "out",
      "name": "Response"
    },
    {
      "type": "queue",
      "direction": "out",
      "name": "msg",
      "queueName": "outqueue",
      "connection": "AzureWebJobsStorage"
    }
  ]
}
```
::: zone-end

::: zone pivot="programming-language-python"

Actualice *function.json* para que coincida con lo siguiente mediante la adición del enlace de cola después del enlace HTTP:

```json
{
  "scriptFile": "__init__.py",
  "bindings": [
    {
      "authLevel": "function",
      "type": "httpTrigger",
      "direction": "in",
      "name": "req",
      "methods": [
        "get",
        "post"
      ]
    },
    {
      "type": "http",
      "direction": "out",
      "name": "$return"
    },
    {
      "type": "queue",
      "direction": "out",
      "name": "msg",
      "queueName": "outqueue",
      "connection": "AzureWebJobsStorage"
    }
  ]
}
```
::: zone-end

::: zone pivot="programming-language-typescript"

Actualice *function.json* para que coincida con lo siguiente mediante la adición del enlace de cola después del enlace HTTP:

```json
{
  "bindings": [
    {
      "authLevel": "function",
      "type": "httpTrigger",
      "direction": "in",
      "name": "Request",
      "methods": [
        "get",
        "post"
      ]
    },
    {
      "type": "http",
      "direction": "out",
      "name": "Response"
    },
    {
      "type": "queue",
      "direction": "out",
      "name": "msg",
      "queueName": "outqueue",
      "connection": "AzureWebJobsStorage"
    }
  ]
}
```
::: zone-end

## <a name="add-code-to-use-the-output-binding"></a>Adición de código para usar el enlace de salida

Una vez definido el enlace, el nombre de este, que en este caso es `msg`, aparece en el código de la función como un argumento (o en el objeto `context` en JavaScript y TypeScript). Posteriormente, puede usar esa variable para escribir mensajes en la cola. Debe escribir cualquier código para la autenticación, la obtención de una referencia a la cola o la escritura de datos. Todas estas tareas de integración se administran de manera adecuada en el entorno de ejecución de Azure Functions y en el enlace de salida de la cola.

::: zone pivot="programming-language-csharp"
```csharp
[FunctionName("HttpTrigger")]
public static async Task<IActionResult> Run(
    [HttpTrigger(AuthorizationLevel.Function, "get", "post", Route = null)] HttpRequest req, 
    [Queue("outqueue"), StorageAccount("AzureWebJobsStorage")] ICollector<string> msg, ILogger log)
{
    log.LogInformation("C# HTTP trigger function processed a request.");

    string name = req.Query["name"];

    string requestBody = await new StreamReader(req.Body).ReadToEndAsync();
    dynamic data = JsonConvert.DeserializeObject(requestBody);
    name = name ?? data?.name;

    if (!string.IsNullOrEmpty(name))
    {
        // Add a message to the output collection.
        msg.Add(string.Format("Name passed to the function: {0}", name));
    }
    
    return name != null
        ? (ActionResult)new OkObjectResult($"Hello, {name}")
        : new BadRequestObjectResult("Please pass a name on the query string or in the request body");
}
```
::: zone-end

::: zone pivot="programming-language-javascript"
```js
module.exports = async function (context, req) {
    context.log('JavaScript HTTP trigger function processed a request.');

    if (req.query.name || (req.body && req.body.name)) {
        // Add a message to the Storage queue.
        context.bindings.msg = "Name passed to the function: " +
            (req.query.name || req.body.name);

        context.res = {
            // status: 200, /* Defaults to 200 */
            body: "Hello " + (req.query.name || req.body.name)
        };
    }
    else {
        context.res = {
            status: 400,
            body: "Please pass a name on the query string or in the request body"
        };
    }
};
```
::: zone-end

::: zone pivot="programming-language-powershell"
```powershell
using namespace System.Net

# Input bindings are passed in via param block.
param($Request, $TriggerMetadata)

# Write to the Azure Functions log stream.
Write-Host "PowerShell HTTP trigger function processed a request."

# Interact with query parameters or the body of the request.
$name = $Request.Query.Name
if (-not $name) {
    $name = $Request.Body.Name
}

if ($name) {
    $outputMsg = "Name passed to the function: $name"
    Push-OutputBinding -name msg -Value $outputMsg

    $status = [HttpStatusCode]::OK
    $body = "Hello $name"
}
else {
    $status = [HttpStatusCode]::BadRequest
    $body = "Please pass a name on the query string or in the request body."
}

# Associate values to output bindings by calling 'Push-OutputBinding'.
Push-OutputBinding -Name Response -Value ([HttpResponseContext]@{
    StatusCode = $status
    Body = $body
})
```
::: zone-end

::: zone pivot="programming-language-python"
```python
import logging

import azure.functions as func


def main(req: func.HttpRequest, msg: func.Out[func.QueueMessage]) -> str:

    name = req.params.get('name')
    if not name:
        try:
            req_body = req.get_json()
        except ValueError:
            pass
        else:
            name = req_body.get('name')

    if name:
        msg.set(name)
        return func.HttpResponse(f"Hello {name}!")
    else:
        return func.HttpResponse(
            "Please pass a name on the query string or in the request body",
            status_code=400
        )
```
::: zone-end

::: zone pivot="programming-language-typescript"
```typescript
import { AzureFunction, Context, HttpRequest } from "@azure/functions"

const httpTrigger: AzureFunction = async function (context: Context, req: HttpRequest): Promise<void> {
    context.log('HTTP trigger function processed a request.');
    const name = (req.query.name || (req.body && req.body.name));

    if (name) {
        // Add a message to the Storage queue.
        context.bindings.msg = "Name passed to the function: " +
            (req.query.name || req.body.name);
        
        context.res = {
            // status: 200, /* Defaults to 200 */
            body: "Hello " + (req.query.name || req.body.name)
        };
    }
    else {
        context.res = {
            status: 400,
            body: "Please pass a name on the query string or in the request body"
        };
    }
};

export default httpTrigger;
```
::: zone-end

### <a name="update-the-image-in-the-registry"></a>Actualización de la imagen en el registro

1. En la carpeta raíz, ejecute de nuevo `docker build` y, en esta ocasión, actualice la versión que está en la etiqueta a `v1.0.1`. Igual que antes, reemplace `<docker_id>` por el identificador de la cuenta de Docker Hub:

    ```
    docker build --tag <docker_id>/azurefunctionsimage:v1.0.1
    ```
    
1. Inserte la imagen actualizada de nuevo en el repositorio con `docker push`:

    ```
    docker push <docker_id>/azurefunctionsimage:v1.0.1
    ```

1. Dado que configuró la entrega continua, si actualiza la imagen en el registro se actualizará automáticamente la aplicación de funciones de nuevo en Azure.

## <a name="view-the-message-in-the-azure-storage-queue"></a>Visualización del mensaje en la cola de Azure Storage

En un explorador, use la misma dirección URL de antes para invocar la función. El explorador debe mostrar la misma respuesta que antes, ya que no se modificó esa parte del código de la función. No obstante, el mensaje agregado escribió un mensaje mediante el parámetro `name` de URL en la cola de almacenamiento `outqueue`.

La cola se puede ver en [Azure Portal](../storage/queues/storage-quickstart-queues-portal.md) o en el [Explorador de Microsoft Azure Storage](https://storageexplorer.com/). También puede ver la cola en la CLI de Azure como se indica en los pasos siguientes:

1. Abra el archivo *local.setting.json* del proyecto de Functions y copie el valor de la cadena de conexión. En una ventana de terminal o de comandos, ejecute el siguiente comando para crear una variable de entorno denominada `AZURE_STORAGE_CONNECTION_STRING` y pegue la cadena de conexión concreta en lugar de `<connection_string>`. (Esta variable de entorno significa que no es necesario proporcionar la cadena de conexión a cada comando posterior mediante el argumento `--connection-string`).

    # <a name="bash"></a>[bash](#tab/bash)
    
    ```bash
    AZURE_STORAGE_CONNECTION_STRING="<connection_string>"
    ```
    
    # <a name="powershell"></a>[PowerShell](#tab/powershell)
    
    ```powershell
    $env:AZURE_STORAGE_CONNECTION_STRING = "<connection_string>"
    ```
    
    # <a name="cmd"></a>[Cmd](#tab/cmd)
    
    ```cmd
    set AZURE_STORAGE_CONNECTION_STRING="<connection_string>"
    ```
    
    ---
    
1. (Opcional) Puede usar el comando [`az storage queue list`](/cli/azure/storage/queue#az-storage-queue-list) para ver las colas de Storage de la cuenta. La salida de este comando debe incluir una cola denominada `outqueue`, la cual se creó cuando la función escribió su primer mensaje en esa cola.
    
    # <a name="bash"></a>[bash](#tab/bash)
    
    ```azurecli
    az storage queue list --output tsv
    ```
    
    # <a name="powershell"></a>[PowerShell](#tab/powershell)
    
    ```azurecli
    az storage queue list --output tsv
    ```
    
    # <a name="cmd"></a>[Cmd](#tab/cmd)
    
    ```azurecli
    az storage queue list --output tsv
    ```
    
    ---

1. Use el comando [`az storage message peek`](/cli/azure/storage/message#az-storage-message-peek) para ver los mensajes de esta cola, la cual debe ser el nombre que utilizó al probar la función anteriormente. El comando recupera el primer mensaje de la cola con [codificación Base64](functions-bindings-storage-queue-trigger.md#encoding), por lo que también deberá descodificar el mensaje para verlo como texto.

    # <a name="bash"></a>[bash](#tab/bash)
    
    ```bash
    echo `echo $(az storage message peek --queue-name outqueue -o tsv --query '[].{Message:content}') | base64 --decode`
    ```
    
    # <a name="powershell"></a>[PowerShell](#tab/powershell)
    
    ```powershell
    [System.Text.Encoding]::UTF8.GetString([System.Convert]::FromBase64String($(az storage message peek --queue-name outqueue -o tsv --query '[].{Message:content}')))
    ```
    
    # <a name="cmd"></a>[Cmd](#tab/cmd)
    
    Como debe desreferenciar la colección de mensajes y descodificar a partir de Base64, ejecute PowerShell y use el comando de PowerShell.

    ---

## <a name="clean-up-resources"></a>Limpieza de recursos

Si desea seguir trabajando con Azure Functions y con los recursos que creó en este tutorial, puede dejar todos esos recursos activos. Como ha creado un plan Premium para Azure Functions, incurrirá en un pequeño costo continuo por día.

Para evitar estos costos, elimine el grupo de recursos `AzureFunctionsContainer-rg` para limpiar todos los recursos de ese grupo: 

```azurecli
az group delete --name AzureFunctionsContainer-rg
```

## <a name="next-steps"></a>Pasos siguientes

+ [Supervisión de funciones](functions-monitoring.md)
+ [Opciones de escalado y hospedaje](functions-scale.md)
+ [Hospedaje sin servidor basado en Kubernetes](functions-kubernetes-keda.md)
