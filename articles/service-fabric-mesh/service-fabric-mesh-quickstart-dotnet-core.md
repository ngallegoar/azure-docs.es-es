---
title: 'Inicio rápido: Creación e implementación de una aplicación web en Azure Service Fabric Mesh'
description: En esta guía de inicio rápido se muestra cómo crear un sitio web de ASP.NET Core y publicarlo en Azure Service Fabric Mesh mediante Visual Studio.
author: georgewallace
ms.topic: quickstart
ms.date: 07/17/2018
ms.author: gwallace
ms.custom: mvc, devcenter
ms.openlocfilehash: 15e0c3bcbc19eae344ca3965b2b866ca9a15f28d
ms.sourcegitcommit: b87c7796c66ded500df42f707bdccf468519943c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/08/2020
ms.locfileid: "91840326"
---
# <a name="quickstart-create-and-deploy-a-web-app-to-azure-service-fabric-mesh"></a>Inicio rápido: Creación e implementación de una aplicación web en Azure Service Fabric mesh

Azure Service Fabric mesh es un servicio totalmente administrado que permite a los desarrolladores implementar aplicaciones de microservicios sin la administración de máquinas virtuales, almacenamiento o redes.

En esta guía de inicio rápido creará una nueva aplicación de Service Fabric mesh que constará de una aplicación web ASP.NET Core, la ejecutará en el clúster de desarrollo local y, a continuación, la publicará para ejecutarla en Azure.

Necesitará una suscripción a Azure. Si no la tiene, puede crear fácilmente una suscripción a Azure gratuita mediante la opción [Crear una cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar. También deberá [configurar su entorno de desarrollo](service-fabric-mesh-howto-setup-developer-environment-sdk.md)

[!INCLUDE [preview note](./includes/include-preview-note.md)]

## <a name="create-a-service-fabric-mesh-project"></a>Creación de un proyecto de Service Fabric mesh

Abra Visual Studio y seleccione **Archivo** > **Nuevo** > **Proyecto...** .

En el cuadro **Buscar** del cuadro de diálogo **Nuevo proyecto** de la parte superior, escriba `mesh`. Seleccione la plantilla **Service Fabric Mesh Application** (Aplicación de Service Fabric mesh). (Si no la ve, asegúrese de que ha instalado el SDK de Mesh y la versión preliminar de las herramientas de VS según se describe en [Configuración del entorno de desarrollo](service-fabric-mesh-howto-setup-developer-environment-sdk.md). 

En el cuadro **Nombre**, escriba **ServiceFabricMesh1** y, en el cuadro **Ubicación**, establezca la ruta de acceso a la carpeta donde se almacenarán los archivos del proyecto.

Asegúrese de que la opción **Create directory for solution** (Crear directorio para la solución) esté activada y haga clic en **Aceptar** para crear el proyecto de Service Fabric mesh.

![Captura de pantalla que muestra cómo crear un proyecto de Service Fabric Mesh.](media/service-fabric-mesh-quickstart-dotnet-core/visual-studio-new-project.png)

### <a name="create-a-service"></a>Crear un servicio

Tras hacer clic en **Aceptar**, aparece el cuadro de diálogo **New Service Fabric Service** (Nuevo servicio de Service Fabric). Seleccione el tipo de proyecto **ASP.NET Core**, asegúrese de que **Container OS** (SO contenedor) esté establecido en **Windows** y haga clic en **Aceptar** para crear el proyecto de ASP.NET Core. 

![Cuadro de diálogo de proyecto nuevo de Service Fabric mesh en Visual Studio](media/service-fabric-mesh-quickstart-dotnet-core/visual-studio-new-service-fabric-service.png)

Aparece el cuadro de diálogo **Nueva aplicación web ASP.NET Core**. Seleccione **Aplicación web** y, después, haga clic en **Aceptar**.

![Nueva aplicación web ASP.NET Core de Visual Studio](media/service-fabric-mesh-quickstart-dotnet-core/visual-studio-new-aspnetcore-app.png)

Visual Studio crea el proyecto de aplicación de Service Fabric mesh y el proyecto de ASP.NET Core.

## <a name="build-and-publish-to-your-local-cluster"></a>Creación y publicación en el clúster local

Tan pronto como se carga el proyecto, se crea y publica una imagen de Docker en el clúster local. Este proceso puede tardar un tiempo. Para supervisar el progreso de las herramientas de Service Fabric en la ventana **Salida**, seleccione el elemento **Herramientas de Service Fabric** en la ventana desplegable **Salida**. Puede seguir trabajando mientras se implementa la imagen de Docker.

Tras crear el proyecto, haga clic en **F5** para depurar el servicio localmente. Cuando finalice la implementación local y Visual Studio ejecute el proyecto, se abrirá una ventana del explorador con una página web de ejemplo.

Al terminar de examinar el servicio implementado, presione **Mayús + F5** en Visual Studio para detener la depuración del proyecto.

## <a name="publish-to-azure"></a>Publicación en Azure

Para publicar el proyecto de Service Fabric mesh en Azure, haga clic con el botón derecho en el **proyecto de Service Fabric mesh** en Visual Studio y seleccione **Publicar...**

![Hacer clic con el botón derecho en proyecto de Service Fabric mesh en Visual Studio](media/service-fabric-mesh-quickstart-dotnet-core/visual-studio-right-click-publish.png)

Verá el cuadro de diálogo **Publicación de la aplicación de Service Fabric**.

![Cuadro de diálogo de publicación de Service Fabric mesh en Visual Studio](media/service-fabric-mesh-quickstart-dotnet-core/visual-studio-publish-dialog.png)

Seleccione la cuenta y la suscripción de Azure. Elija una **ubicación**. En este artículo se utiliza **Este de EE. UU**.

En **Grupo de recursos**, seleccione **\<Create New Resource Group...>** . Aparece el cuadro de diálogo **Crear grupo de recursos**. Establezca el **nombre del grupo de recursos** y la **ubicación**.  En esta guía de inicio rápido se utiliza la ubicación de **Este de EE. UU.** y se denomina al grupo **sfmeshTutorial1RG** (si la organización tiene varias personas con la misma suscripción, elija un nombre de grupo de recursos único).  Haga clic en **Crear** para crear el grupo de recursos y volver al cuadro de diálogo Publicar.

![Captura de pantalla que muestra cómo crear un grupo de recursos.](media/service-fabric-mesh-quickstart-dotnet-core/visual-studio-publish-new-resource-group-dialog.png)

De nuevo en el cuadro de diálogo **Publicación de la aplicación de Service Fabric**, en **Azure Container Registry**, seleccione **\<Create New Container Registry...>** (Crear una instancia de Azure Container Registry...>). En el cuadro de diálogo **Crear Registro de contenedor**, utilice un nombre único para el **nombre de la instancia de Container Registry**. Especifique una **ubicación** (en esta guía de inicio rápido se utiliza **Este de EE. UU.** ). Seleccione el **grupo de recursos** que creó en el paso anterior en la lista desplegable, por ejemplo, **sfmeshTutorial1RG**. Establezca la **SKU** en **Básico** y, después, haga clic en **Crear** para volver al cuadro de diálogo Publicar.

![Cuadro de diálogo del nuevo grupo de recursos de Service Fabric mesh en Visual Studio](media/service-fabric-mesh-quickstart-dotnet-core/visual-studio-publish-new-container-registry-dialog.png)

En el cuadro de diálogo Publicar, haga clic en el botón **Publicar** para implementar la aplicación de Service Fabric mesh en Azure.

Al publicar en Azure por primera vez, la imagen de Docker se inserta en Azure Container Registry (ACR), que tarda según el tamaño de la imagen. Las publicaciones posteriores del mismo proyecto serán más rápidas. Para supervisar el progreso de la implementación, seleccione el panel **Herramientas de Service Fabric** en la ventana desplegable **Salida** de Visual Studio. Una vez finalizada la implementación, la salida de **Herramientas de Service Fabric** mostrará la dirección IP y el puerto de la aplicación en forma de dirección URL.

```
Packaging Application...
Building Images...
Web1 -> C:\Code\ServiceFabricMesh1\Web1\bin\Any CPU\Release\netcoreapp2.0\Web1.dll
Uploading the images to Azure Container Registry...
Deploying application to remote endpoint...
The application was deployed successfully and it can be accessed at http://...
```

Abra un explorador web y vaya a la dirección URL para ver el sitio web que se ejecuta en Azure:

![Ejecución de una aplicación web de Service Fabric mesh](media/service-fabric-mesh-tutorial-deploy-dotnetcore/deployed-web-project.png)

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando ya no los necesite, elimine todos los recursos que haya creado para esta guía de inicio rápido. Como ha creado un nuevo grupo de recursos para hospedar los recursos tanto de ACR como del servicio Service Fabric mesh, puede eliminar este grupo de recursos con seguridad, lo que supone una forma fácil de eliminar todos los recursos asociados a él.

```azurecli
az group delete --resource-group sfmeshTutorial1RG
```

```powershell
Connect-AzureRmAccount
Remove-AzureRmResourceGroup -Name sfmeshTutorial1RG
```

Además, puede eliminar el grupo de recursos [desde Azure Portal](https://portal.azure.com).

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información sobre cómo crear e implementar aplicaciones de Service Fabric mesh, continúe con el tutorial.
> [!div class="nextstepaction"]
> [Creación, depuración e implementación de una aplicación web multiservicio en Service Fabric mesh](service-fabric-mesh-tutorial-create-dotnetcore.md)
