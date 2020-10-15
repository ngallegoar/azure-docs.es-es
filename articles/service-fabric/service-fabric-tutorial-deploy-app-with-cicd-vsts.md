---
title: Implementación de una aplicación con CI y Azure Pipelines
description: En este tutorial aprenderá a configurar la integración y la implementación continuas para una aplicación de Service Fabric mediante Azure Pipelines.
ms.topic: tutorial
ms.date: 07/22/2019
ms.custom: mvc
ms.openlocfilehash: a26cfaca466e01b154c65b27895f3004f6320e5d
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "91326344"
---
# <a name="tutorial-deploy-an-application-with-cicd-to-a-service-fabric-cluster"></a>Tutorial: Implementar una aplicación con CI/CD en un clúster de Service Fabric

Este tutorial es la cuarta parte de una serie y describe cómo configurar la integración y la implementación continuas de una aplicación de Azure Service Fabric mediante Azure Pipelines.  Se necesita una aplicación existente de Service Fabric; se usa como ejemplo la aplicación creada en [Compilación de una aplicación .NET](service-fabric-tutorial-create-dotnet-app.md).

En la tercera parte de la serie, se aprende a:

> [!div class="checklist"]
> * Agregar un control de código fuente al proyecto
> * Crear una canalización de compilación en Azure Pipelines
> * Crear una canalización de versión en Azure Pipelines
> * Implementar y actualizar una aplicación automáticamente

En esta serie de tutoriales, se aprende a:
> [!div class="checklist"]
> * [Crear una aplicación de .NET Service Fabric](service-fabric-tutorial-create-dotnet-app.md)
> * [Implementar la aplicación en un clúster remoto](service-fabric-tutorial-deploy-app-to-party-cluster.md)
> * [Agregar un punto de conexión HTTPS a un servicio de front-end de ASP.NET Core](service-fabric-tutorial-dotnet-app-enable-https-endpoint.md)
> * Configurar CI/CD con Azure Pipelines
> * [Configurar la supervisión y el diagnóstico para la aplicación](service-fabric-tutorial-monitoring-aspnet.md)

## <a name="prerequisites"></a>Prerrequisitos

Antes de empezar este tutorial:

* Si no tiene ninguna suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)
* [Instale Visual Studio 2019](https://www.visualstudio.com/) y las cargas de trabajo de **desarrollo de Azure** y de **desarrollo web y de ASP.NET**.
* [Instale el SDK de Service Fabric](service-fabric-get-started.md)
* Cree un clúster de Windows Service Fabric en Azure (por ejemplo, [siguiendo este tutorial](service-fabric-tutorial-create-vnet-and-windows-cluster.md))
* Cree una [organización de Azure DevOps](/azure/devops/organizations/accounts/create-organization-msa-or-work-student). Esto le permite crear un proyecto de Azure DevOps y usar Azure Pipelines.

## <a name="download-the-voting-sample-application"></a>Descarga de la aplicación de ejemplo de votación

Si no compiló la aplicación de ejemplo de votación en la [primera parte de esta serie de tutoriales](service-fabric-tutorial-create-dotnet-app.md), puede descargarla. En una ventana Comandos, ejecute el comando siguiente para clonar el repositorio de la aplicación de ejemplo en la máquina local.

```git
git clone https://github.com/Azure-Samples/service-fabric-dotnet-quickstart
```

## <a name="prepare-a-publish-profile"></a>Preparar un perfil de publicación

Ahora que ha [creado una aplicación](service-fabric-tutorial-create-dotnet-app.md) y ha [implementado la aplicación en Azure](service-fabric-tutorial-deploy-app-to-party-cluster.md), ya puede configurar la integración continua.  En primer lugar, prepare un perfil de publicación en la aplicación que se use durante el proceso de implementación que se ejecuta en Azure Pipelines.  El perfil de publicación debe configurarse para que tenga como destino el clúster que ha creado antes.  Inicie Visual Studio y abra un proyecto de aplicación existente de Service Fabric.  En el **Explorador de soluciones**, haga clic con el botón derecho en la aplicación y seleccione **Publicar...** .

Elija un perfil de destino en el proyecto de la aplicación que vaya a usar para el flujo de trabajo de integración continua (por ejemplo, Cloud).  Especifique el punto de conexión de la conexión del clúster.  Marque la casilla **Actualizar la aplicación** para que la esta se actualice para cada implementación de Azure DevOps.  Haga clic en el hipervínculo **Guardar** para guardar la configuración en el perfil de publicación y, luego, haga clic en **Cancelar** para cerrar el cuadro de diálogo.

![Perfil de inserción][publish-app-profile]

## <a name="share-your-visual-studio-solution-to-a-new-azure-devops-git-repo"></a>Solución de Visual Studio compartida en un nuevo repositorio Git de Azure DevOps

Comparta sus archivos de origen de la aplicación en un proyecto de Azure DevOps para generar compilaciones.

Cree un repositorio Git local para el proyecto seleccionando **Agregar al control de código fuente** -> **Git** en la barra de estado situada en la esquina inferior derecha de Visual Studio.

En la vista **Inserción** de **Team Explorer**, seleccione el botón **Publicar repositorio Git** en **Push to Azure DevOps** (Insertar en Azure DevOps).

![Captura de pantalla de la ventana Team Explorer - Sincronización en Visual Studio. El botón Publicar repositorio de Git aparece resaltado en Insertar en Azure DevOps.][push-git-repo]

Verifique su correo electrónico y seleccione su cuenta en la lista desplegable **Dominio de Azure DevOps**. Escriba el nombre del repositorio y seleccione **Publicar repositorio**.

![Captura de pantalla de la configuración de Insertar en Azure DevOps con los botones Correo electrónico, Cuenta, Nombre del repositorio y Publicar repositorio resaltados.][publish-code]

La publicación del repositorio crea un proyecto en la cuenta con el mismo nombre que el repositorio local. Para crear el repositorio en un proyecto existente, haga clic en **Avanzado**, junto al nombre del **repositorio**, y seleccione un proyecto. Puede ver el código en la web seleccionando **Ver en la web**.

## <a name="configure-continuous-delivery-with-azure-pipelines"></a>Configuración de la entrega continua con Azure Pipelines

Una canalización de compilación de Azure Pipelines describe un flujo de trabajo compuesto por una serie de pasos de compilación que se ejecutan de manera secuencial. Cree una canalización de compilación que genere un paquete de aplicación de Service Fabric y otros artefactos para implementarlos en un clúster de Service Fabric. Más información sobre las [canalizaciones de compilación de Azure Pipelines](https://www.visualstudio.com/docs/build/define/create). 

Una canalización de versión de Azure Pipelines describe un flujo de trabajo que implementa un paquete de aplicación en un clúster. Cuando se usan juntas, la canalización de compilación y la de versión ejecutan el flujo de trabajo completo empezando por los archivos de origen y terminando por una aplicación en el clúster. Más información sobre las [canalizaciones de versión de Azure Pipelines](https://www.visualstudio.com/docs/release/author-release-definition/more-release-definition).

### <a name="create-a-build-pipeline"></a>Creación de una canalización de compilación

Abra un explorador web y vaya hasta el nuevo proyecto en: [https://&lt;micuenta&gt;.visualstudio.com/Voting/Voting%20Team/_git/Voting](https://myaccount.visualstudio.com/Voting/Voting%20Team/_git/Voting).

Seleccione la pestaña **Canalizaciones**, después **Compilaciones** y, finalmente, **Nueva canalización**.

![Nueva canalización][new-pipeline]

Seleccione **Azure Repos Git** como origen, el proyecto de equipo **Voting**, el repositorio **Voting** y la rama predeterminada **maestra** o compilaciones manuales y programadas.  A continuación, haga clic en **Continue** (Continuar).

![Seleccionar repositorio][select-repo]

En **Seleccionar una plantilla**, seleccione la plantilla de la **aplicación de Azure Service Fabric** y haga clic en **Apply** (Aplicar).

![Elegir la plantilla de compilación][select-build-template]

En **Tareas**, escriba "VS2017 hospedado" como **Grupo de agentes**.

![Seleccionar tareas][save-and-queue]

En **Desencadenadores**, habilite la integración continua, para lo que debe seleccionar **Habilitar la integración continua**. En **Filtros de rama**, la **especificación de rama** es **maestro** de forma predeterminada. Seleccione **Guardar y poner en cola** para iniciar manualmente una compilación.

![Seleccionar desencadenadores][save-and-queue2]

Las compilaciones también se desencadenan después de una inserción o una protección. Para comprobar el progreso de la compilación, vaya a la pestaña **Compilaciones**.  Cuando haya verificado que la compilación se ejecuta correctamente, defina una canalización de versión que implemente la aplicación en un clúster.

### <a name="create-a-release-pipeline"></a>Creación de una canalización de versión

Seleccione la pestaña **Canalizaciones**, luego, **Versiones** y, finalmente, **+ Nueva canalización**.  En **Seleccionar una plantilla**, seleccione la plantilla **Implementación de Azure Service Fabric** de la lista y haga clic en **Aplicar**.

![Elegir la versión de la plantilla][select-release-template]

Seleccione **Tareas**->**Entorno 1** y, a continuación, **+Nuevo** para agregar una nueva conexión de clúster.

![Agregar conexión del clúster][add-cluster-connection]

En la vista **Agregar nueva conexión de Service Fabric**, seleccione la autenticación **Basada en certificado** o **Azure Active Directory**.  Especifique como nombre de conexión "mysftestcluster" y como punto de conexión del clúster "tcp://mysftestcluster.southcentralus.cloudapp.azure.com:19000" (o el punto de conexión del clúster en el que se va a implementar).

Para la autenticación basada en certificados, agregue la **huella digital del certificado de servidor** del certificado de servidor utilizado para crear el clúster.  En **Certificado de cliente**, agregue la codificación en base 64 del archivo del certificado de cliente. Consulte la ventana de ayuda emergente de ese campo para obtener información sobre cómo obtener esa representación codificada en base 64 del certificado. Agregue también un valor de **Contraseña** del certificado.  Puede utilizar el certificado del clúster o del servidor si no tiene un certificado de cliente independiente.

Para las credenciales de Azure Active Directory, agregue la **Huella digital del certificado de servidor** del certificado de servidor utilizado para crear el clúster y las credenciales que desea usar para conectarse al clúster en los campos **Nombre de usuario** y **Contraseña**.

Haga clic en **Agregar** para guardar la conexión del clúster.

A continuación, agregue un artefacto de compilación a la canalización para que la canalización de versión pueda encontrar la salida de la compilación. Seleccione **Canalización** y **Artefactos**-> **+Agregar**.  En **Origen (definición de compilación)** , seleccione la canalización de compilación que creó anteriormente.  Haga clic en **Agregar** para guardar el artefacto de compilación.

![Agregar artefacto][add-artifact]

Habilite un desencadenador de implementación continua para que se cree automáticamente una versión cuando finalice la compilación. Haga clic en el icono de rayo del artefacto, habilite el desencadenador y haga clic en **Guardar** para guardar la canalización de versión.

![Habilitar el desencadenador][enable-trigger]

Seleccione **+Versión** -> **Crear versión** -> **Crear** para crear manualmente una versión. El progreso de la versión se puede supervisar en la pestaña **Versiones**.

Compruebe que la implementación se haya efectuado correctamente y que la aplicación se está ejecutando en el clúster.  Abra un explorador web y vaya a `http://mysftestcluster.southcentralus.cloudapp.azure.com:19080/Explorer/`.  Observe que la versión de la aplicación de este ejemplo es "1.0.0.20170616.3".

## <a name="commit-and-push-changes-trigger-a-release"></a>Confirmar e insertar cambios y desencadenar una versión

Para verificar que la canalización de integración continua funciona, inserte en el repositorio algunos cambios de código para Azure DevOps.

A medida que escribe el código, los cambios se controlan automáticamente con Visual Studio. Confirme los cambios en el repositorio Git local seleccionando el icono de cambios pendientes (![El icono de cambios pendientes muestra un lápiz y un número.][pending]) en la barra de estado situada en la parte inferior derecha.

En la vista **Cambios** de Team Explorer, agregue un mensaje que describa la actualización y confirme los cambios.

![Confirmar todo][changes]

Seleccione el icono de la barra de estado de los cambios no publicados (![Cambios no publicados][unpublished-changes]) o la vista Sincronización de Team Explorer. Seleccione **Insertar** para actualizar el código en Azure Pipelines.

![Inserción de los cambios][push]

La inserción de los cambios en Azure Pipelines desencadena automáticamente una compilación.  Cuando la canalización de compilación se haya completado correctamente, se creará automáticamente una versión y se empezará a actualizar la aplicación en el clúster.

Para comprobar el progreso de la compilación, vaya a la pestaña **Compilaciones** de **Team Explorer** en Visual Studio.  Cuando haya verificado que la compilación se ejecuta correctamente, defina una canalización de versión que implemente la aplicación en un clúster.

Compruebe que la implementación se haya efectuado correctamente y que la aplicación se está ejecutando en el clúster.  Abra un explorador web y vaya a `http://mysftestcluster.southcentralus.cloudapp.azure.com:19080/Explorer/`.  Observe que la versión de la aplicación de este ejemplo es "1.0.0.20170815.3".

![Captura de pantalla de la aplicación Voting de Service Fabric Explorer que se ejecuta en una ventana del explorador. La versión de la aplicación "1.0.0.20170815.3" aparece resaltada.][sfx1]

## <a name="update-the-application"></a>Actualizar la aplicación

Efectúe cambios de código en la aplicación.  Guarde y confirme los cambios siguiendo los pasos anteriores.

Cuando haya comenzado la actualización de la aplicación, podrá ver el progreso de la actualización en Service Fabric Explorer:

![Captura de pantalla de la aplicación Voting de Service Fabric Explorer. El mensaje de estado "Actualizando" y un mensaje de "Actualización en curso" aparecen resaltados.][sfx2]

La actualización de la aplicación puede tardar varios minutos. Una vez concluida la actualización, la aplicación ejecutará la próxima versión.  En este ejemplo "1.0.0.20170815.4".

![Captura de pantalla de la aplicación Voting de Service Fabric Explorer que se ejecuta en una ventana del explorador. La versión de la aplicación actualizada "1.0.0.20170815.4" aparece resaltada.][sfx3]

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, ha aprendido a:

> [!div class="checklist"]
> * Agregar un control de código fuente al proyecto
> * Creación de una canalización de compilación
> * Creación de una canalización de versión
> * Implementar y actualizar una aplicación automáticamente

Avance hasta el siguiente tutorial:
> [!div class="nextstepaction"]
> [Configurar la supervisión y el diagnóstico para la aplicación](service-fabric-tutorial-monitoring-aspnet.md)

<!-- Image References -->
[publish-app-profile]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/PublishAppProfile.png
[push-git-repo]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/PublishGitRepo.png
[publish-code]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/PublishCode.png
[new-pipeline]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/NewPipeline.png
[select-repo]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/SelectRepo.png
[select-build-template]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/SelectBuildTemplate.png
[save-and-queue]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/SaveAndQueue.png
[save-and-queue2]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/SaveAndQueue2.png
[select-release-template]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/SelectReleaseTemplate.png
[set-continuous-integration]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/SetContinuousIntegration.png
[add-cluster-connection]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/AddClusterConnection.png
[add-artifact]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/AddArtifact.png
[enable-trigger]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/EnableTrigger.png
[sfx1]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/SFX1.png
[sfx2]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/SFX2.png
[sfx3]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/SFX3.png
[pending]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/Pending.png
[changes]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/Changes.png
[unpublished-changes]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/UnpublishedChanges.png
[push]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/Push.png
[continuous-delivery-with-AzureDevOpsServices]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/VSTS-Dialog.png
[new-service-endpoint]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/NewServiceEndpoint.png
[new-service-endpoint-dialog]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/NewServiceEndpointDialog.png
