---
title: 'Depuración e iteración en Kubernetes: Visual Studio y .NET Core'
services: azure-dev-spaces
ms.date: 11/13/2019
ms.topic: quickstart
description: En este inicio rápido se muestra cómo usar Azure Dev Spaces y Visual Studio para depurar e iterar rápidamente una aplicación .NET Core en Azure Kubernetes Service
keywords: Docker, Kubernetes, Azure, AKS, Azure Kubernetes Service, contenedores, Helm, service mesh, enrutamiento de service mesh, kubectl, k8s
manager: gwallace
ms.custom: vs-azure
ms.workload: azure-vs
ms.openlocfilehash: 909e4638b3b0919919320a09cbfa0e8d9ac92f2e
ms.sourcegitcommit: fc718cc1078594819e8ed640b6ee4bef39e91f7f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/27/2020
ms.locfileid: "83995945"
---
# <a name="quickstart-debug-and-iterate-on-kubernetes-visual-studio--net-core---azure-dev-spaces"></a>Inicio rápido: Depuración e iteración en Kubernetes: Visual Studio y .NET Core: Azure Dev Spaces

En esta guía, aprenderá a:

- Configurar Azure Dev Spaces con un clúster de Kubernetes administrado en Azure.
- Desarrollar código de forma iterativa en contenedores con Visual Studio
- Depurar el código que se ejecuta en un clúster con Visual Studio

Azure Dev Spaces también permite depurar e iterar mediante:
- [Java y Visual Studio Code](quickstart-java.md)
- [Node.js y Visual Studio Code](quickstart-nodejs.md)
- [.NET Core y Visual Studio Code](quickstart-netcore.md)

## <a name="prerequisites"></a>Requisitos previos

- Suscripción a Azure. En caso de no tener ninguna, puede crear una [cuenta gratuita](https://azure.microsoft.com/free).
- Visual Studio 2019 en Windows con la carga de trabajo de desarrollo de Azure instalada. Si no tiene instalado Visual Studio, descárguelo [aquí](https://aka.ms/vsdownload?utm_source=mscom&utm_campaign=msdocs).
- [La CLI de Azure instalada](/cli/azure/install-azure-cli?view=azure-cli-latest).

## <a name="create-an-azure-kubernetes-service-cluster"></a>Creación de un clúster de Azure Kubernetes Service

Debe crear un clúster de AKS en una [región admitida][supported-regions]. Los siguientes comandos permiten crear un grupo de recursos llamado *MyResourceGroup* y un clúster de AKS denominado *MyAKS*.

```azurecli
az group create --name MyResourceGroup --location eastus
az aks create -g MyResourceGroup -n MyAKS --location eastus --generate-ssh-keys
```

## <a name="enable-azure-dev-spaces-on-your-aks-cluster"></a>Habilitar Azure Dev Spaces en el clúster de AKS

Use el comando `use-dev-spaces` para habilitar Dev Spaces en el clúster de AKS y siga las indicaciones. El siguiente comando permite habilitar Dev Spaces en el clúster *MyAKS* del grupo *MyResourceGroup* y crea un espacio de desarrollo *predeterminado*.

> [!NOTE]
> El comando `use-dev-spaces` también instalará la CLI de Azure Dev Spaces si aún no se ha instalado. La CLI de Azure Dev Spaces no se puede instalar en Azure Cloud Shell.

```azurecli
az aks use-dev-spaces -g MyResourceGroup -n MyAKS
```

```output
'An Azure Dev Spaces Controller' will be created that targets resource 'MyAKS' in resource group 'MyResourceGroup'. Continue? (y/N): y

Creating and selecting Azure Dev Spaces Controller 'MyAKS' in resource group 'MyResourceGroup' that targets resource 'MyAKS' in resource group 'MyResourceGroup'...2m 24s

Select a dev space or Kubernetes namespace to use as a dev space.
 [1] default
Type a number or a new name: 1

Kubernetes namespace 'default' will be configured as a dev space. This will enable Azure Dev Spaces instrumentation for new workloads in the namespace. Continue? (Y/n): Y

Configuring and selecting dev space 'default'...3s

Managed Kubernetes cluster 'MyAKS' in resource group 'MyResourceGroup' is ready for development in dev space 'default'. Type `azds prep` to prepare a source directory for use with Azure Dev Spaces and `azds up` to run.
```

## <a name="create-a-new-aspnet-web-app"></a>Crear una aplicación web ASP.NET

1. Abra Visual Studio.
1. Cree un nuevo proyecto.
1. Elija *Aplicación Web ASP.NET Core* y haga clic en *Siguiente*.
1. Asigne el nombre *webfrontend* al proyecto y haga clic en *Crear*.
1. Cuando se le solicite, elija *Aplicación web (Modelo-Vista-Controlador)* para la plantilla.
1. Seleccione *.NET Core* y *ASP.NET Core 2.1* en la parte superior.
1. Haga clic en *Crear*.

## <a name="connect-your-project-to-your-dev-space"></a>Conexión del proyecto al espacio de desarrollo

En el proyecto, seleccione **Azure Dev Spaces**  en el menú desplegable de configuración de inicio, como se muestra a continuación.

![](media/get-started-netcore-visualstudio/LaunchSettings.png)

En el cuadro de diálogo de Azure Dev Spaces, seleccione su *suscripción* y *un clúster de Azure Kubernetes*. Deje *Espacio* establecido en *predeterminado* y active la casilla *Accesible públicamente*. Haga clic en *OK*.

![](media/get-started-netcore-visualstudio/Azure-Dev-Spaces-Dialog.png)

Este proceso implementa el servicio en el espacio de desarrollo *predeterminado* con una dirección URL accesible públicamente. Si elige un clúster que no ha sido configurado para trabajar con Azure Dev Spaces, verá un mensaje preguntando si desea configurarlo. Haga clic en *OK*.

![](media/get-started-netcore-visualstudio/Add-Azure-Dev-Spaces-Resource.png)

La dirección URL pública para el servicio que se ejecuta en el espacio de desarrollo *predeterminado* aparece en la ventana *Salida*:

```cmd
Starting warmup for project 'webfrontend'.
Waiting for namespace to be provisioned.
Using dev space 'default' with target 'MyAKS'
...
Successfully built 1234567890ab
Successfully tagged webfrontend:devspaces-11122233344455566
Built container image in 39s
Waiting for container...
36s

Service 'webfrontend' port 'http' is available at `http://default.webfrontend.1234567890abcdef1234.eus.azds.io/`
Service 'webfrontend' port 80 (http) is available at http://localhost:62266
Completed warmup for project 'webfrontend' in 125 seconds.
```

En el ejemplo anterior, la dirección URL pública es `http://default.webfrontend.1234567890abcdef1234.eus.azds.io/`. 

Seleccione **Depurar** y, después, **Iniciar depuración**. Después de unos segundos, se iniciará el servicio y Visual Studio abrirá un explorador con la dirección URL pública del servicio. Si no se abre automáticamente el explorador, vaya a la dirección URL pública del servicio en un explorador e interactúe con él mientras se ejecuta en el espacio de desarrollo.

Es posible que este proceso haya deshabilitado el acceso público a su servicio. Para permitir el acceso público, puede actualizar el [valor de entrada en *values.yaml*][ingress-update].

## <a name="update-code"></a>Actualización del código

Si Visual Studio está aún conectado a su espacio de desarrollo, haga clic en el botón Detener. Cambie la línea 20 en `Controllers/HomeController.cs` a:
    
```csharp
ViewData["Message"] = "Your application description page in Azure.";
```

Guarde los cambios, seleccione **Depurar** y, después, **Iniciar depuración**. Después de unos segundos, se iniciará el servicio y Visual Studio abrirá un explorador con la dirección URL pública del servicio. Si no se abre automáticamente el explorador, vaya a la dirección URL pública de su servicio en un explorador y haga clic en *Acerca de*. Observe que aparece el mensaje actualizado.

En lugar de volver a crear e implementar una nueva imagen de contenedor cada vez que se realizan modificaciones en el código, Azure Dev Spaces vuelve a compilar el código de manera incremental dentro del contenedor existente para proporcionar un bucle de modificación y depuración más rápido.

## <a name="setting-and-using-breakpoints-for-debugging"></a>Establecimiento y uso de puntos de interrupción para la depuración

Si Visual Studio está aún conectado a su espacio de desarrollo, haga clic en el botón Detener. Abra `Controllers/HomeController.cs` y haga clic en algún lugar de la línea 20 para colocar el cursor allí. Para establecer un punto de interrupción, pulse *F9* o haga clic en *Depurar* y en *Alternar punto de interrupción*. Para iniciar el servicio en modo de depuración en el espacio de desarrollo, pulse *F5* o haga clic en *Depurar* y en *Iniciar depuración*.

Abra el servicio en un explorador y observe que no se muestra ningún mensaje. Vuelva a Visual Studio y observe que la línea 20 está resaltada. El punto de interrupción que estableció ha detenido el servicio en la línea 20. Para reanudar el servicio, pulse *F5* o haga clic en *Depurar* y en *Continuar*. Regrese al explorador y observe que ahora se muestra el mensaje.

Mientras se ejecuta el servicio en Kubernetes con un depurador asociado, tiene acceso completo a la información de depuración como, por ejemplo, la de la pila de llamadas, las variables locales y la información de excepciones.

Quite el punto de interrupción colocando el cursor en la línea 20 en `Controllers/HomeController.cs` y pulsando *F9*.

## <a name="clean-up-your-azure-resources"></a>Limpieza de los recursos de Azure

Vaya al grupo de recursos en Azure Portal y haga clic en *Eliminar grupo de recursos*. Como alternativa, puede usar el comando [az aks delete](/cli/azure/aks#az-aks-delete):

```azurecli
az group delete --name MyResourceGroup --yes --no-wait
```

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Trabajo con varios contenedores y desarrollo en equipo](multi-service-netcore-visualstudio.md)

[ingress-update]: how-dev-spaces-works-up.md#how-running-your-code-is-configured
[supported-regions]: https://azure.microsoft.com/global-infrastructure/services/?products=kubernetes-service
