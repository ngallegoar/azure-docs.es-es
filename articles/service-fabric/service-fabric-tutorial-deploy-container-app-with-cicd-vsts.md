---
title: Implementación de una aplicación de contenedor con CI/CD
description: En este tutorial aprenderá a configurar la integración y la implementación continuas para una aplicación de contenedor de Azure Service Fabric mediante Visual Studio Azure DevOps.
ms.topic: tutorial
ms.date: 08/29/2018
ms.custom: mvc
ms.openlocfilehash: bb0eb9226a99f139ff10a8da12a1e22017536c67
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "91326310"
---
# <a name="tutorial-deploy-a-container-application-with-cicd-to-a-service-fabric-cluster"></a>Tutorial: Implementación de una aplicación de contenedor con CI/CD en un clúster de Service Fabric

Este tutorial es la segunda parte de una serie y describe cómo configurar la integración y la implementación continuas de una aplicación de contenedor de Azure Service Fabric mediante Visual Studio y Azure DevOps.  Se necesita una aplicación existente de Service Fabric; a modo de ejemplo se usa la aplicación creada en [Tutorial: Implementación de una aplicación .NET de un contenedor de Windows en Azure Service Fabric](service-fabric-host-app-in-a-container.md).

En la segunda parte de la serie, se aprende a:

> [!div class="checklist"]
> * Agregar un control de código fuente al proyecto
> * Crear una definición de compilación en Visual Studio Team Explorer
> * Crear una definición de versión en Visual Studio Team Explorer
> * Implementar y actualizar una aplicación automáticamente

## <a name="prerequisites"></a>Prerrequisitos

Antes de empezar este tutorial:

* Tener un clúster en Azure o [crear uno con este tutorial](service-fabric-tutorial-create-vnet-and-windows-cluster.md)
* [Implementar una aplicación en contenedores en él](service-fabric-host-app-in-a-container.md)

## <a name="prepare-a-publish-profile"></a>Preparar un perfil de publicación

Ahora que ha [implementado una aplicación de contenedor](service-fabric-host-app-in-a-container.md), está preparado para configurar la integración continua.  En primer lugar, prepare un perfil de publicación en la aplicación que se use durante el proceso de implementación que se ejecuta en Azure DevOps.  El perfil de publicación debe configurarse para que tenga como destino el clúster que ha creado antes.  Inicie Visual Studio y abra un proyecto de aplicación existente de Service Fabric.  En el **Explorador de soluciones**, haga clic con el botón derecho en la aplicación y seleccione **Publicar...** .

Elija un perfil de destino en el proyecto de la aplicación que vaya a usar para el flujo de trabajo de integración continua (por ejemplo, Cloud).  Especifique el punto de conexión de la conexión del clúster.  Marque la casilla **Actualizar la aplicación** para que la esta se actualice para cada implementación de Azure DevOps.  Haga clic en el hipervínculo **Guardar** para guardar la configuración en el perfil de publicación y, luego, haga clic en **Cancelar** para cerrar el cuadro de diálogo.

![Perfil de inserción][publish-app-profile]

## <a name="share-your-visual-studio-solution-to-a-new-azure-devops-git-repo"></a>Solución de Visual Studio compartida en un nuevo repositorio Git de Azure DevOps

Comparta los archivos de origen de su aplicación en un proyecto de equipo de Azure DevOps para poder generar compilaciones.

Cree un repositorio Git local para el proyecto seleccionando **Agregar al control de código fuente** -> **Git** en la barra de estado situada en la esquina inferior derecha de Visual Studio.

En la vista **Inserción** de **Team Explorer**, seleccione el botón **Publicar repositorio Git** en **Push to Azure DevOps** (Insertar en Azure DevOps).

![Captura de pantalla de la ventana Team Explorer - Sincronización en Visual Studio. En Insertar en Azure DevOps, el botón Publicar repositorio Git aparece resaltado.][push-git-repo]

Compruebe su correo electrónico y seleccione su organización en la lista desplegable **Cuenta**. Si no tiene alguna organización, es posible que deba configurar una. Escriba el nombre del repositorio y seleccione **Publicar repositorio**.

![Captura de pantalla de la ventana Insertar en Azure DevOps. Los valores de Correo electrónico, Cuenta, Nombre del repositorio y del botón Publicar repositorio aparecen resaltados.][publish-code]

La publicación del repositorio crea un proyecto de equipo en su cuenta con el mismo nombre que el repositorio local. Para crear el repositorio en un proyecto de equipo existente, haga clic en **Avanzado**, junto al nombre del **repositorio**, y seleccione un proyecto de equipo. Puede ver el código en la web seleccionando **Ver en la web**.

## <a name="configure-continuous-delivery-with-azure-pipelines"></a>Configuración de la entrega continua con Azure Pipelines

Una definición de compilación de Azure DevOps describe un flujo de trabajo que consta de una serie de pasos de compilación que se ejecutan de manera secuencial. Cree una definición de compilación que genere un paquete de aplicación de Service Fabric y otros artefactos para implementarlos en un clúster de Service Fabric. Más información sobre las [definiciones de compilación](https://www.visualstudio.com/docs/build/define/create) de Azure DevOps. 

Las definiciones de versión de Azure DevOps describen un flujo de trabajo que implementa un paquete de aplicación en un clúster. Cuando se usan juntas, la definición de compilación y la de versión ejecutan el flujo de trabajo completo empezando por los archivos de origen y terminando por la ejecución de una aplicación en el clúster. Más información sobre las [definiciones de versión](https://www.visualstudio.com/docs/release/author-release-definition/more-release-definition) de Azure DevOps.

### <a name="create-a-build-definition"></a>Creación de una definición de compilación

Abra el nuevo proyecto de equipo, para lo que debe ir a https://dev.azure.com en un explorador web y seleccionar su organización, seguida del nuevo proyecto. 

Seleccione la opción **Canalizaciones** en el panel izquierdo y, después, haga clic en **Nueva canalización**.

>[!NOTE]
>Si no ve la plantilla de definición de compilación, asegúrese de que la característica **Nueva experiencia de creación de canalizaciones de YAML** está desactivada. Esta característica se configura en la sección **Características en vista previa** de la cuenta de DevOps.

![Nueva canalización][new-pipeline]

Seleccione **GIT de Azure Repos** como origen, el nombre de proyecto del equipo, el repositorio del proyecto y la rama predeterminada **maestra** o las compilaciones manuales y programadas.  A continuación, haga clic en **Continue** (Continuar).

En **Seleccionar una plantilla**, seleccione la plantilla de la **aplicación de Azure Service Fabric con compatibilidad para Docker** y haga clic en **Aplicar**.

![Elegir la plantilla de compilación][select-build-template]

En **Tareas**, seleccione **Hosted VS2017** como **Grupo de agentes**.

![Seleccionar tareas][task-agent-pool]

Haga clic en **Tag images** (Etiquetar imágenes).

En **Tipo de Container Registry**, seleccione **Azure Container Registry**. Seleccione una **suscripción de Azure** y haga clic en **Autorizar**. Seleccione una instancia de **Azure Container Registry**.

![Selección de Tag images (Etiquetar imágenes) en Docker][select-tag-images]

Haga clic en **Push images** (Insertar imágenes).

En **Tipo de Container Registry**, seleccione **Azure Container Registry**. Seleccione una **suscripción de Azure** y haga clic en **Autorizar**. Seleccione una instancia de **Azure Container Registry**.

![Selección de Push images (Insertar imágenes) en Docker][select-push-images]

En la pestaña **Desencadenadores**, habilite la integración continua, para lo que debe seleccionar **Habilitar la integración continua**. En **Filtros de rama**, haga clic en **+ Agregar**y el valor predeterminado de **Especificación de rama** será **maestro**.

En el **cuadro de diálogo Guardar y poner en cola la canalización de compilación**, haga clic en **Guardar y poner en cola** para empezar manualmente una compilación.

![Seleccionar desencadenadores][save-and-queue]

Las compilaciones también se desencadenan después de una inserción o una protección. Para comprobar el progreso de la compilación, vaya a la pestaña **Compilaciones**.  Cuando haya comprobado que la compilación se ejecuta correctamente, defina una definición de versión que implemente la aplicación en un clúster.

### <a name="create-a-release-definition"></a>Creación de una definición de versión

Seleccione la opción **Canalizaciones** en el panel izquierdo, después **Versiones** y, finalmente, **+Nueva canalización**.  En **Seleccionar una plantilla**, seleccione la plantilla **Implementación de Azure Service Fabric** de la lista y haga clic en **Aplicar**.

![Elegir la versión de la plantilla][select-release-template]

Seleccione **Tareas**, **Entorno 1** y **+Nuevo** para agregar una nueva conexión de clúster.

![Agregar conexión del clúster][add-cluster-connection]

En la vista **Agregar nueva conexión de Service Fabric**, seleccione la autenticación **Basada en certificado** o **Azure Active Directory**.  Especifique como nombre de conexión "mysftestcluster" y como punto de conexión del clúster "tcp://mysftestcluster.southcentralus.cloudapp.azure.com:19000" (o el punto de conexión del clúster en el que se va a implementar).

Para la autenticación basada en certificados, agregue la **Huella digital del certificado de servidor** del certificado de servidor utilizado para crear el clúster.  En **Certificado de cliente**, agregue la codificación en base 64 del archivo del certificado de cliente. Consulte la ventana de ayuda emergente de ese campo para obtener información sobre cómo obtener esa representación codificada en base 64 del certificado. Agregue también un valor de **Contraseña** del certificado.  Puede utilizar el certificado del clúster o del servidor si no tiene un certificado de cliente independiente.

Para las credenciales de Azure Active Directory, agregue la **Huella digital del certificado de servidor** del certificado de servidor utilizado para crear el clúster y las credenciales que desea usar para conectarse al clúster en los campos **Nombre de usuario** y **Contraseña**.

Haga clic en **Agregar** para guardar la conexión del clúster.



En la fase del agente, haga clic en **Implementar aplicación de Service Fabric**.
Haga clic en **Configuración de Docker** y en **Configurar las opciones de Docker**. En **Origen de credenciales del Registro**, seleccione **Azure Resource Manager Service Connection** (Conexión de servicio de Azure Resource Manager). Después, seleccione la **suscripción a Azure**.

![Agente de canalización de versión][release-pipeline-agent]

A continuación, agregue un artefacto de compilación a la canalización para que la definición de la versión pueda encontrar la salida de la compilación. Seleccione **Canalización** y **Artefactos**-> **+Agregar**.  En **Origen (Definición de compilación)** , seleccione la definición de compilación que creó anteriormente.  Haga clic en **Agregar** para guardar el artefacto de compilación.

![Agregar artefacto][add-artifact]

Habilite un desencadenador de implementación continua para que se cree automáticamente una versión cuando finalice la compilación. Haga clic en el icono de rayo en el artefacto, habilite el desencadenador y haga clic en **Guardar** para guardar la definición de la versión.

![Habilitar el desencadenador][enable-trigger]

Seleccione **+Versión** -> **Crear versión** -> **Crear** para crear manualmente una versión. El progreso de la versión se puede supervisar en la pestaña **Versiones**.

Compruebe que la implementación se haya efectuado correctamente y que la aplicación se está ejecutando en el clúster.  Abra un explorador web y vaya a `http://mysftestcluster.southcentralus.cloudapp.azure.com:19080/Explorer/`.  Observe que la versión de la aplicación de este ejemplo es "1.0.0.20170616.3".

## <a name="commit-and-push-changes-trigger-a-release"></a>Confirmar e insertar cambios y desencadenar una versión

Para verificar que la canalización de integración continua funciona, inserte en el repositorio algunos cambios de código para Azure DevOps.

A medida que escribe el código, los cambios se controlan automáticamente con Visual Studio. Confirme los cambios en el repositorio Git local seleccionando el icono de cambios pendientes (![El icono de cambios pendientes muestra un lápiz y un número.][pending]) en la barra de estado situada en la parte inferior derecha.

En la vista **Cambios** de Team Explorer, agregue un mensaje que describa la actualización y confirme los cambios.

![Confirmar todo][changes]

Seleccione el icono de la barra de estado de los cambios no publicados (![Cambios no publicados][unpublished-changes]) o la vista Sincronización de Team Explorer. Seleccione **Insertar** para actualizar el código en Azure DevOps.

![Inserción de los cambios][push]

La inserción de los cambios en Azure DevOps desencadena automáticamente una compilación.  Cuando la definición de compilación se haya completado correctamente, se creará automáticamente una versión y se empezará a actualizar la aplicación en el clúster.

Para comprobar el progreso de la compilación, vaya a la pestaña **Compilaciones** de **Team Explorer** en Visual Studio.  Cuando haya comprobado que la compilación se ejecuta correctamente, defina una definición de versión que implemente la aplicación en un clúster.

Compruebe que la implementación se haya efectuado correctamente y que la aplicación se está ejecutando en el clúster.  Abra un explorador web y vaya a `http://mysftestcluster.southcentralus.cloudapp.azure.com:19080/Explorer/`.  Observe que la versión de la aplicación de este ejemplo es "1.0.0.20170815.3".

![Captura de pantalla de la aplicación Voting de Service Fabric Explorer. En la pestaña Información esencial, la versión de la aplicación "1.0.0.20170815.3" aparece resaltada.][sfx1]

## <a name="update-the-application"></a>Actualizar la aplicación

Efectúe cambios de código en la aplicación.  Guarde y confirme los cambios siguiendo los pasos anteriores.

Cuando haya comenzado la actualización de la aplicación, podrá ver el progreso de la actualización en Service Fabric Explorer:

![Captura de pantalla de la aplicación Voting de Service Fabric Explorer. El mensaje "Actualización en curso" aparece resaltado y el estado de la aplicación es "Actualizando".][sfx2]

La actualización de la aplicación puede tardar varios minutos. Una vez concluida la actualización, la aplicación ejecutará la próxima versión.  En este ejemplo "1.0.0.20170815.4".

![Captura de pantalla de la aplicación Voting de Service Fabric Explorer. En la pestaña Información esencial, la versión de la aplicación actualizada "1.0.0.20170815.4" aparece resaltada.][sfx3]

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, ha aprendido a:

> [!div class="checklist"]
> * Agregar un control de código fuente al proyecto
> * Creación de una definición de compilación
> * Creación de una definición de versión
> * Implementar y actualizar una aplicación automáticamente

En la siguiente parte del tutorial, puede aprender a configurar la [supervisión del contenedor](service-fabric-tutorial-monitoring-wincontainers.md).

<!-- Image References -->
[publish-app-profile]: ./media/service-fabric-tutorial-deploy-container-app-with-cicd-vsts/PublishAppProfile.png
[push-git-repo]: ./media/service-fabric-tutorial-deploy-container-app-with-cicd-vsts/PublishGitRepo.png
[publish-code]: ./media/service-fabric-tutorial-deploy-container-app-with-cicd-vsts/PublishCode.png
[new-pipeline]: ./media/service-fabric-tutorial-deploy-container-app-with-cicd-vsts/NewPipeline.png
[select-build-template]: ./media/service-fabric-tutorial-deploy-container-app-with-cicd-vsts/SelectBuildTemplate.png
[task-agent-pool]: ./media/service-fabric-tutorial-deploy-container-app-with-cicd-vsts/TaskAgentPool.png
[save-and-queue]: ./media/service-fabric-tutorial-deploy-container-app-with-cicd-vsts/SaveAndQueue.png
[select-tag-images]: ./media/service-fabric-tutorial-deploy-container-app-with-cicd-vsts/DockerTagImages.png
[select-push-images]: ./media/service-fabric-tutorial-deploy-container-app-with-cicd-vsts/DockerPushImages.png
[select-release-template]: ./media/service-fabric-tutorial-deploy-container-app-with-cicd-vsts/SelectReleaseTemplate.png
[set-continuous-integration]: ./media/service-fabric-tutorial-deploy-container-app-with-cicd-vsts/SetContinuousIntegration.png
[add-cluster-connection]: ./media/service-fabric-tutorial-deploy-container-app-with-cicd-vsts/AddClusterConnection.png
[release-pipeline-agent]: ./media/service-fabric-tutorial-deploy-container-app-with-cicd-vsts/ReleasePipelineAgent.png
[add-artifact]: ./media/service-fabric-tutorial-deploy-container-app-with-cicd-vsts/AddArtifact.png
[enable-trigger]: ./media/service-fabric-tutorial-deploy-container-app-with-cicd-vsts/EnableTrigger.png
[sfx1]: ./media/service-fabric-tutorial-deploy-container-app-with-cicd-vsts/SFX1.png
[sfx2]: ./media/service-fabric-tutorial-deploy-container-app-with-cicd-vsts/SFX2.png
[sfx3]: ./media/service-fabric-tutorial-deploy-container-app-with-cicd-vsts/SFX3.png
[pending]: ./media/service-fabric-tutorial-deploy-container-app-with-cicd-vsts/Pending.png
[changes]: ./media/service-fabric-tutorial-deploy-container-app-with-cicd-vsts/Changes.png
[unpublished-changes]: ./media/service-fabric-tutorial-deploy-container-app-with-cicd-vsts/UnpublishedChanges.png
[push]: ./media/service-fabric-tutorial-deploy-container-app-with-cicd-vsts/Push.png
[continuous-delivery-with-VSTS]: ./media/service-fabric-tutorial-deploy-container-app-with-cicd-vsts/VSTS-Dialog.png
[new-service-endpoint]: ./media/service-fabric-tutorial-deploy-container-app-with-cicd-vsts/NewServiceEndpoint.png
[new-service-endpoint-dialog]: ./media/service-fabric-tutorial-deploy-container-app-with-cicd-vsts/NewServiceEndpointDialog.png
