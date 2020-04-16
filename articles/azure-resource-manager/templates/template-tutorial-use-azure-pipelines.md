---
title: Integración continua con Azure Pipelines
description: Aprende a compilar, probar e implementar plantillas de Azure Resource Manager de manera continua.
author: mumian
ms.date: 10/29/2019
ms.topic: tutorial
ms.author: jgao
ms.openlocfilehash: b117861a528b6983876d28d5b343ea88c2bcadc0
ms.sourcegitcommit: 8dc84e8b04390f39a3c11e9b0eaf3264861fcafc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/13/2020
ms.locfileid: "81260690"
---
# <a name="tutorial-continuous-integration-of-azure-resource-manager-templates-with-azure-pipelines"></a>Tutorial: Integración continua de plantillas de Azure Resource Manager en Azure Pipelines

Aprende a usar Azure Pipelines para compilar e implementar proyectos de plantillas de Azure Resource Manager de manera continua.

Azure DevOps proporciona servicios para desarrolladores para apoyar a los equipos de soporte técnico a planear el trabajo, colaborar en el desarrollo de código, y compilar e implementar aplicaciones. Los desarrolladores pueden trabajar en la nube con Azure DevOps Services. Azure DevOps proporciona un conjunto integrado de características a las que puedes acceder a través de tu explorador web o cliente IDE. Azure Pipelines es una de estas características. Azure Pipelines es un servicio completo de integración continua (CI) y entrega continua (CD). Funciona con tu proveedor de Git preferido y puede implementarse en la mayoría de los servicios en la nube principales. A continuación, puedes automatizar la compilación, las pruebas y la implementación del código en Microsoft Azure, Google Cloud Platform o Amazon Web Services.

Este tutorial está diseñado para los desarrolladores de plantillas de Azure Resource Manager que son nuevos en Azure DevOps Services y Azure Pipelines. Si ya estás familiarizado con GitHub y DevOps, puedes ir a [Crear una canalización](#create-a-pipeline).

> [!NOTE]
> Elige un nombre de proyecto. Al seguir el tutorial, reemplaza las instancias de **AzureRmPipeline** con el nombre de tu proyecto.

En este tutorial se describen las tareas siguientes:

> [!div class="checklist"]
> * Preparación de un repositorio de GitHub
> * Crear un proyecto de Azure DevOps Projects
> * Creación de una canalización de Azure
> * Comprobación de la implementación de la canalización
> * Actualización de la plantilla y reimplementación
> * Limpieza de recursos

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.

## <a name="prerequisites"></a>Prerrequisitos

Para completar este artículo, necesitará lo siguiente:

* **Una cuenta de GitHub**, que usarás para crear un repositorio para las plantillas. Si no tiene una, puedes [crear una gratis](https://github.com). Para más información sobre el uso de los repositorios de GitHub, consulta [Build GitHub repositories](/azure/devops/pipelines/repos/github) (Compilar repositorios de GitHub).
* **Instale Git**. En las instrucciones de este tutorial se usa *Git Bash* o *Git Shell*. Para instrucciones, consulta [Install Git]( https://www.atlassian.com/git/tutorials/install-git) (Instalar Git).
* **Una organización de Azure DevOps**. Si no tienes una, puedes crear una gratis. Consulte [Create an organization or project collection]( https://docs.microsoft.com/azure/devops/organizations/accounts/create-organization?view=azure-devops) (Crear una organización o colección de proyectos).
* Visual Studio Code con la extensión Resource Manager Tools. Consulte [Uso de Visual Studio Code para crear plantillas de Azure Resource Manager](use-vs-code-to-create-template.md).

## <a name="prepare-a-github-repository"></a>Preparación de un repositorio de GitHub

GitHub sirve para almacenar el código fuente del proyecto, incluidas plantillas de Resource Manager. Para otros repositorios admitidos, consulta los [repositorios compatibles con Azure DevOps](/azure/devops/pipelines/repos/?view=azure-devops).

### <a name="create-a-github-repository"></a>Crear un repositorio de GitHub

Si no tienes una cuenta de GitHub, consulta [Requisitos previos](#prerequisites).

1. Inicia sesión en [GitHub](https://github.com).
2. Selecciona la imagen de tu cuenta en la esquina superior derecha y selecciona **Los repositorios**.

    ![Creación de repositorio de GitHub para Azure Resource Manager Azure DevOps Azure Pipelines](./media/template-tutorial-use-azure-pipelines/azure-resource-manager-devops-pipelines-github-repository.png)

1. Selecciona **Nuevo**, un botón verde.
1. En **Nombre del repositorio**, escribe el nombre del repositorio.  Por ejemplo, **AzureRmPipeline-repo**. No olvides reemplazar las instancias de **AzureRmPipeline** por el nombre de tu proyecto. Puedes seleccionar **Público** o **Privado** para completar este tutorial. Luego selecciona **Crear repositorio**.
1. Anota la dirección URL. La dirección URL del repositorio tiene el formato siguiente:

    ```url
    https://github.com/[YourAccountName]/[YourRepositoryName]
    ```

Este repositorio se conoce como *repositorio remoto*. Cada uno de los desarrolladores del mismo proyecto puede clonar su propio *repositorio local* y combinar los cambios en el repositorio remoto.

### <a name="clone-the-remote-repository"></a>Clonar el repositorio remoto

1. Abre Git Shell o Git Bash.  Consulte [Requisitos previos](#prerequisites).
1. Verifica que la carpeta actual sea **github**.
1. Ejecute el siguiente comando:

    ```bash
    git clone https://github.com/[YourAccountName]/[YourGitHubRepositoryName]
    cd [YourGitHubRepositoryName]
    mkdir CreateAzureStorage
    cd CreateAzureStorage
    pwd
    ```

    Reemplaza **[YourAccountName]** por el nombre de tu cuenta de GitHub y reemplaza **[YourGitHubRepositoryName]** por el nombre del repositorio que creaste en el procedimiento anterior.

    La siguiente captura de pantalla muestra un ejemplo.

    ![Creación de bash de GitHub para Azure Resource Manager Azure DevOps Azure Pipelines](./media/template-tutorial-use-azure-pipelines/azure-resource-manager-devops-pipelines-github-bash.png)

La carpeta **CreateAzureStorage** es la carpeta donde se almacena la plantilla. El comando **pwd** muestra la ruta de acceso de la carpeta. La ruta de acceso es donde guardas la plantilla en el siguiente procedimiento.

### <a name="download-a-quickstart-template"></a>Descargar una plantilla de inicio rápido

En lugar de crear una plantilla, puedes descargar una [plantilla de inicio rápido]( https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.json). Esta plantilla crea una cuenta de Azure Storage.

1. Abre Visual Studio Code. Consulte [Requisitos previos](#prerequisites).
2. Abre la plantilla con la dirección URL siguiente:

    ```URL
    https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.json
    ```

3. Guarda el archivo como **azuredeploy.json** en la carpeta **CreateAzureStorage**. Tanto el nombre de carpeta como el nombre de archivo se usan tal como están en la canalización.  Si cambias estos nombres, debes actualizar los nombres usados en la canalización.

### <a name="push-the-template-to-the-remote-repository"></a>Insertar la plantilla en el repositorio remoto

El archivo azuredeploy.json se agregó al repositorio local. A continuación, inserta la plantilla en el repositorio remoto.

1. Abre *Git Shell* o *Git Bash*, si no está abierto.
1. Cambia el directorio a la carpeta CreateAzureStorage en el repositorio local.
1. Verifica que el archivo **azuredeploy.json** se encuentre en la carpeta.
1. Ejecute el siguiente comando:

    ```bash
    git add .
    git commit -m “Add a new create storage account template.”
    git push origin master
    ```

    Puedes recibir una advertencia sobre LF. Puedes omitir la advertencia. **master** la rama maestra.  Normalmente, creas una rama para cada actualización. Para simplificar el tutorial, usa directamente la rama maestra.
1. Ve al repositorio de GitHub desde un explorador.  La dirección URL es **https://github.com/ [YourAccountName]/[YourGitHubRepository]** . Verás la carpeta **CreateAzureStorage** y **Azuredeploy.json** dentro de la carpeta.

Hasta ahora, has creado un repositorio de GitHub y cargado una plantilla en el repositorio.

## <a name="create-a-devops-project"></a>Crear un proyecto de DevOps

Se necesita una organización de DevOps antes de poder avanzar al procedimiento siguiente.  Si no tienes una, consulta [Requisitos previos](#prerequisites).

1. Inicie sesión en [Azure DevOps](https://dev.azure.com).
1. Selecciona una organización de DevOps de la izquierda.

    ![Creación de un proyecto de Azure DevOps para Azure Resource Manager Azure DevOps Azure Pipelines](./media/template-tutorial-use-azure-pipelines/azure-resource-manager-devops-pipelines-create-devops-project.png)

1. Seleccione **Create project** (Crear proyecto). Si no tienes ningún proyecto, la página de creación de proyectos se abre automáticamente.
1. Escriba los siguientes valores:

    * **Nombre del proyecto**: Escriba un nombre de proyecto. Puedes usar el nombre del proyecto elegido al principio del tutorial.
    * **Control de versiones**: Selecciona **Git**. Es posible que tengas que expandir **Avanzado** para ver **Control de versiones**.

    Usa el valor predeterminado para las demás propiedades.
1. Seleccione **Create project** (Crear proyecto).

Crea una conexión de servicio que se use para implementar proyectos en Azure.

1. Selecciona **Configuración del proyecto** de la parte inferior del menú izquierdo.
1. Selecciona **Conexiones de servicio** en **Canalizaciones**.
1. Selecciona **Nueva conexión de servicio** y, luego, selecciona **AzureResourceManager**.
1. Escriba los siguientes valores:

    * **Nombre de conexión**: Escribe un nombre de conexión. Por ejemplo, **AzureRmPipeline-conn**. Anota este nombre, necesitarás el nombre al crear la canalización.
    * **Nivel de ámbito**: Selecciona **Suscripción**.
    * **Suscripción**: Selecciona tu suscripción.
    * **Grupo de recursos**: Deje este parámetro en blanco.
    * **Permita que todas las canalizaciones usen esta conexión.** (seleccionado)
1. Seleccione **Aceptar**.

## <a name="create-a-pipeline"></a>Crear una canalización

Hasta ahora, hemos completado las siguientes tareas.  Si omites las secciones anteriores porque ya estás familiarizado con GitHub y DevOps, debes completar las tareas antes de continuar.

- Crea un repositorio de GitHub y guarda [esta plantilla](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.json) en la carpeta **CreateAzureStorage** en el repositorio.
- Crea un proyecto de DevOps y crea una conexión de servicio de Azure Resource Manager.

Para crear una canalización con un paso para implementar una plantilla:

1. Selecciona **Canalizaciones** en el menú izquierdo.
1. Selecciona **Nueva canalización**.
1. Desde la pestaña **Conectar**, selecciona **GitHub**. Si se te piden, escribe las credenciales de GitHub y luego sigue las instrucciones. Si ves la siguiente pantalla, selecciona **Only select repositories** (Solo ciertos repositorios) y verifica que el repositorio esté en la lista antes de seleccionar **Approve & Install** (Aprobar e instalar).

    ![Solo ciertos repositorios para Azure Resource Manager Azure DevOps Azure Pipelines](./media/template-tutorial-use-azure-pipelines/azure-resource-manager-devops-pipelines-only-select-repositories.png)

1. Desde la pestaña **Seleccionar**, selecciona el repositorio.  El nombre predeterminado es **[YourAccountName] / [YourGitHubRepositoryName]** .
1. Desde la pestaña **Configurar**, selecciona **Canalización inicial**. Muestra el archivo de canalización **azure-pipelines.yml** con dos pasos de script.
1. Reemplaza la sección **steps** con el siguiente código YAML:

    ```yaml
    steps:
    - task: AzureResourceManagerTemplateDeployment@3
      inputs:
        deploymentScope: 'Resource Group'
        ConnectedServiceName: '[EnterYourServiceConnectionName]'
        subscriptionName: '[EnterTheTargetSubscriptionID]'
        action: 'Create Or Update Resource Group'
        resourceGroupName: '[EnterANewResourceGroupName]'
        location: 'Central US'
        templateLocation: 'Linked artifact'
        csmFile: 'CreateAzureStorage/azuredeploy.json'
        deploymentMode: 'Incremental'
    ```

    Debe parecerse a esto:

    ![YAML para Azure Resource Manager Azure DevOps Azure Pipelines](./media/template-tutorial-use-azure-pipelines/azure-resource-manager-devops-pipelines-yml.png)

    Se han realizado los siguientes cambios:

    * **DeploymentScope**: (opcional) seleccione el ámbito de implementación entre las opciones: `Management Group`, `Subscription` y `Resource Group`. En este tutorial, use **Grupo de recursos**. Para más información sobre los ámbitos, consulte [Ámbitos de implementación](deploy-rest.md#deployment-scope).
    * **ConnectedServiceName**: especifique el nombre de la conexión de servicio que creó anteriormente.
    * **SubscriptionName**:  especifique el identificador de la suscripción de destino.
    * **action**: La acción **Create Or Update Resource Group** se encarga de 2 acciones: 1. crear un grupo de recursos si se proporciona un nuevo nombre de grupo de recursos; 2. implementar la plantilla especificada.
    * **resourceGroupName**: Especifica un nuevo nombre de grupo de recursos. Por ejemplo, **AzureRmPipeline-rg**.
    * **location**: Especifica la ubicación del grupo de recursos.
    * **templateLocation**: Cuando se especifica **Linked artifact**, la tarea busca el archivo de plantilla directamente en el repositorio conectado.
    * **csmFile** es la ruta de acceso al archivo de plantilla. No es necesario especificar un archivo de parámetros de plantilla, ya que todos los parámetros definidos en la plantilla tienen valores predeterminados.

    Para más información sobre la tarea, consulte [Tarea de implementación del grupo de recursos de Azure](/azure/devops/pipelines/tasks/deploy/azure-resource-group-deployment) y [Tarea de implementación de plantillas de Azure Resource Manager](https://github.com/microsoft/azure-pipelines-tasks/blob/master/Tasks/AzureResourceManagerTemplateDeploymentV3/README.md).
1. Seleccione **Guardar y ejecutar**.
1. Selecciona **Guardar y ejecutar** nuevamente. En el repositorio conectado, se guarda una copia del archivo YAML. Para ver el archivo YAML, ve al repositorio.
1. Verifica que la canalización se haya ejecutado correctamente.

    ![YAML para Azure Resource Manager Azure DevOps Azure Pipelines](./media/template-tutorial-use-azure-pipelines/azure-resource-manager-devops-pipelines-status.png)

## <a name="verify-the-deployment"></a>Comprobar la implementación

1. Inicie sesión en [Azure Portal](https://portal.azure.com).
1. Abra el grupo de recursos. El nombre es lo que especificaste en el archivo YAML de la canalización.  Verás que se ha creado una cuenta de almacenamiento.  El nombre de la cuenta de almacenamiento comienza con **store**.
1. Selecciona el nombre de la cuenta de almacenamiento para abrirla.
1. Seleccione **Propiedades**. Observe que el valor de **Replicación** es **Almacenamiento con redundancia local (LRS)** .

    ![Verificación del portal para Azure Resource Manager Azure DevOps Azure Pipelines](./media/template-tutorial-use-azure-pipelines/azure-resource-manager-devops-pipelines-portal-verification.png)

## <a name="update-and-redeploy"></a>Actualizar y reimplementar

Al actualizar la plantilla e insertar los cambios en el repositorio remoto, la canalización actualiza automáticamente los recursos, en este caso, la cuenta de almacenamiento.

1. Abre **azuredeploy.json** desde el repositorio local en Visual Studio Code.
1. Actualiza **defaultValue** de **storageAccountType** a **Standard_GRS**. Vea la siguiente captura de pantalla:

    ![Actualizar YAML para Azure Resource Manager Azure DevOps Azure Pipelines](./media/template-tutorial-use-azure-pipelines/azure-resource-manager-devops-pipelines-update-yml.png)

1. Guarde los cambios.
1. Ejecuta los siguientes comandos de Git Bash o Shell para insertar los cambios en el repositorio remoto.

    ```bash
    git pull origin master
    git add .
    git commit -m “Add a new create storage account template.”
    git push origin master
    ```

    El primer comando sincroniza el repositorio local con el repositorio remoto. Recuerda que el archivo YAML de la canalización se agregó al repositorio remoto.

    Con la actualización de la rama maestra del repositorio remoto, la canalización se activa de nuevo.

Para verificar los cambios, puede comprobar la propiedad de replicación de la cuenta de almacenamiento.  Consulta [Verify the deployment](#verify-the-deployment) (Verificar la implementación).

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando los recursos de Azure ya no sean necesarios, limpie los recursos que implementó eliminando el grupo de recursos.

1. En Azure Portal, seleccione **Grupos de recursos** en el menú de la izquierda.
2. Escriba el nombre del grupo de recursos en el campo **Filtrar por nombre**.
3. Seleccione el nombre del grupo de recursos.
4. Seleccione **Eliminar grupo de recursos** del menú superior.

Puede que también quieras eliminar el repositorio de GitHub y el proyecto de Azure DevOps.

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, creas una canalización de Azure DevOps para implementar una plantilla de Azure Resource Manager. Para aprender a implementar los recursos de Azure en varias regiones y a usar prácticas de implementación seguras, consulte

> [!div class="nextstepaction"]
> [Uso de procedimientos de implementación seguros](./deployment-manager-tutorial.md)
