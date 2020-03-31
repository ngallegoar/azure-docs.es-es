---
title: Implementación de un trabajo de Azure Stream Analytics mediante el paquete npm de CI/CD
description: En este artículo se describe cómo usar el paquete npm de CI/CD de Azure Stream Analytics para configurar un proceso de integración e implementación continuas.
services: stream-analytics
author: mamccrea
ms.author: mamccrea
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 01/28/2020
ms.openlocfilehash: deb6c2439cc84f196b7f42fd9f49d3ebfd057cbb
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "76962228"
---
# <a name="deploy-an-azure-stream-analytics-job-using-cicd-npm-package"></a>Implementación de un trabajo de Azure Stream Analytics mediante el paquete npm de CI/CD 

Puede usar el paquete npm de CI/CD de Azure Stream Analytics para configurar un proceso de implementación e integración continuas para los trabajos de Stream Analytics. En este artículo se describe cómo usar el paquete npm en general con cualquier sistema CI/CD, así como instrucciones específicas para la implementación con Azure Pipelines.

Para más información sobre la implementación con PowerShell, consulte [Implementación de recursos con las plantillas de Resource Manager y Azure PowerShell](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-template-deploy). También puede obtener más información acerca de cómo [utilizar un objeto como un parámetro en una plantilla de Resource Manager](https://docs.microsoft.com/azure/architecture/building-blocks/extending-templates/objects-as-parameters).

## <a name="build-the-vs-code-project"></a>Compilación del proyecto de VS Code

Puede habilitar la integración e implementación continuas para los trabajos de Azure Stream Analytics mediante el paquete npm **asa-streamanalytics-cicd**. El paquete npm proporciona las herramientas para generar plantillas de Azure Resource Manager de los [proyectos de Visual Studio Code de Stream Analytics](quick-create-vs-code.md). Se puede usar en Windows, macOS y Linux sin instalar Visual Studio Code.

Puede [descargar el paquete](https://www.npmjs.com/package/azure-streamanalytics-cicd) directamente, o bien instalarlo [globalmente](https://docs.npmjs.com/downloading-and-installing-packages-globally) a través del `npm install -g azure-streamanalytics-cicd` comando. Este es el enfoque recomendado, que también se puede usar en una tarea de un script de PowerShell o de la CLI de Azure de una canalización de compilación en **Azure Pipelines**.

Una vez que haya instalado el paquete, use el siguiente comando para generar las plantillas de Azure Resource Manager. El argumento **scriptPath** es la ruta de acceso absoluta al archivo **asaql** en el proyecto. Asegúrese de que los archivos asaproj.json y JobConfig.json están en la misma carpeta con el archivo de script. Si no se especifica la **outputPath**, las plantillas se colocarán en la carpeta **Deploy** (Implementación) en la carpeta **bin** del proyecto.

```powershell
azure-streamanalytics-cicd build -scriptPath <scriptFullPath> -outputPath <outputPath>
```
Ejemplo (en macOS)
```powershell
azure-streamanalytics-cicd build -scriptPath "/Users/roger/projects/samplejob/script.asaql" 
```

Cuando un proyecto de Stream Analytics para Visual Studio Code se compila correctamente, genera los dos siguientes archivos de plantilla de Azure Resource Manager en la carpeta **bin/[Debug/Retail]/Deploy**: 

*  Archivo de plantilla de Resource Manager

       [ProjectName].JobTemplate.json 

*  Archivo de parámetros de Resource Manager

       [ProjectName].JobTemplate.parameters.json   

Los parámetros predeterminados en el archivo parameters.json provienen de la configuración del proyecto de Visual Studio Code. Si quiere implementar en otro entorno, reemplace los parámetros según corresponda.

> [!NOTE]
> En todas las credenciales, los valores predeterminados están establecidos en null. Es **imperativo** establecer los valores antes de realizar la implementación en la nube.

```json
"Input_EntryStream_sharedAccessPolicyKey": {
      "value": null
    },
```

## <a name="deploy-with-azure-pipelines"></a>Implementación con Azure Pipelines

En esta sección se detalla cómo crear las canalizaciones de [compilación](https://docs.microsoft.com/azure/devops/pipelines/get-started-designer?view=vsts&tabs=new-nav) y [versión](https://docs.microsoft.com/azure/devops/pipelines/release/define-multistage-release-process?view=vsts) de Azure Pipelines mediante npm.

Abra un explorador web y vaya al proyecto de Visual Studio Code de Azure Stream Analytics.

1. En **Pipelines**, en el menú de navegación izquierdo, seleccione **Compilaciones**. Luego seleccione **Nueva canalización**

   ![Creación de una instancia de Azure Pipeline](./media/setup-cicd-vs-code/new-pipeline.png)

2. Seleccione **Usar el editor clásico** para crear una canalización sin YAML.

3. Seleccione el tipo de origen, el proyecto del equipo y el repositorio. Después, seleccione **Continuar**.

   ![Selección del proyecto de Azure Stream Analytics](./media/setup-cicd-vs-code/select-repo.png)

4. En la página **Elegir una plantilla**, seleccione **Trabajo vacío**.

### <a name="add-npm-task"></a>Incorporación de la tarea npm

1. En la página **Tareas**, seleccione el signo más que aparece al lado de **Agent job 1**. Escriba "npm" en la búsqueda de tarea y seleccione **npm**.

   ![Selección de la tarea npm](./media/setup-cicd-vs-code/search-npm.png)

2. Asigne a la tarea un **nombre para mostrar**. Cambie la opción **Comando** a *personalizar* y escriba el siguiente comando en **Comando y argumentos**. Deje las restantes opciones predeterminadas.

   ```cmd
   install -g azure-streamanalytics-cicd
   ```

   ![Especificación de configuraciones para la tarea npm](./media/setup-cicd-vs-code/npm-config.png)

### <a name="add-command-line-task"></a>Incorporación de una tarea de línea de comandos

1. En la página **Tareas**, seleccione el signo más que aparece al lado de **Agent job 1**. Busque **Línea de comandos**.

2. Asigne a la tarea un **nombre para mostrar** y escriba el siguiente script. Modifique el script con el nombre del repositorio y el nombre del proyecto.

   ```cmd
   azure-streamanalytics-cicd build -scriptPath $(Build.SourcesDirectory)/myASAProject/myASAProj.asaql
   ```

   ![Especificación de configuraciones para la tarea de línea de comandos](./media/setup-cicd-vs-code/commandline-config.png)

### <a name="add-copy-files-task"></a>Incorporación de la tarea de copiar archivos

1. En la página **Tareas**, seleccione el signo más que aparece al lado de **Agent job 1**. Busque **Copiar archivos**. Luego, escriba las siguientes configuraciones.

   |Parámetro|Entrada|
   |-|-|
   |Nombre para mostrar|Copiar archivos a: $(build.artifactstagingdirectory)|
   |Carpeta de origen|`$(system.defaultworkingdirectory)`| 
   |Contenido| `**\Deploy\**` |
   |Carpeta de destino| `$(build.artifactstagingdirectory)`|

   ![Especificación de configuraciones para la tarea de copia](./media/setup-cicd-vs-code/copy-config.png)

### <a name="add-publish-build-artifacts-task"></a>Adición de la tarea Publicar artefactos de compilación

1. En la página **Tareas**, seleccione el signo más que aparece al lado de **Agent job 1**. Busque **Publicar artefactos de compilación** y seleccione la opción con el icono de flecha negra. 

2. No cambie ninguna de las configuraciones predeterminadas.

### <a name="save-and-run"></a>Guardar y ejecutar

Una vez que haya terminado de agregar las tareas npm, línea de comandos, copiar archivos y publicar artefactos de compilación, seleccione **Guardar y poner en cola**. Cuando se le pida, escriba un comentario de guardado y seleccione **Guardar y ejecutar**.

## <a name="release-with-azure-pipelines"></a>Publicación con Azure Pipelines

Abra un explorador web y vaya al proyecto de Visual Studio Code de Azure Stream Analytics.

1. En **Pipelines**, en el menú de navegación izquierdo, seleccione **Versiones**. Luego seleccione **Nueva canalización**.

2. Seleccione **Empezar con un trabajo vacío**.

3. En el cuadro de **Artefactos**, seleccione **+ Agregar un artefacto**. En **Origen**, seleccione la canalización de compilación que acaba de crear y seleccione **Agregar**.

   ![Especificar artefacto de canalización de compilación](./media/setup-cicd-vs-code/build-artifact.png)

4. Cambie el nombre de **Stage 1** a **Deploy job to test environment** (Implementar trabajo en entorno de prueba).

5. Agregue una nueva fase y asígnele el nombre **Implementar trabajo en entorno de producción**.

### <a name="add-tasks"></a>Adición de tareas

1. En la lista desplegable de tareas, seleccione **Deploy job to test environment** (Implementar trabajo en entorno de prueba). 

2. Seleccione el signo **+** junto a **Trabajo de agente** y busque *Implementación de grupo de recursos de Azure*. Escriba los siguientes parámetros:

   |Configuración|Value|
   |-|-|
   |Nombre para mostrar| *Implementar myASAJob*|
   |Suscripción de Azure| Elija su suscripción.|
   |Acción| *Crear o actualizar grupo de recursos*|
   |Resource group| Elija un nombre para el grupo de recursos de prueba que contendrá su trabajo de Stream Analytics.|
   |Location|Elija la ubicación de su grupo de recursos de prueba.|
   |Ubicación de la plantilla| *Artefacto vinculado*|
   |Plantilla| $(Build.ArtifactStagingDirectory)\drop\myASAJob.JobTemplate.json |
   |Parámetros de plantilla|($(Build.ArtifactStagingDirectory)\drop\myASAJob.JobTemplate.parameters.json|
   |Reemplazo de los parámetros de plantilla|-Input_IoTHub1_iotHubNamespace $(test_eventhubname)|
   |Modo de implementación|Incremental|

3. En la lista desplegable de tareas, seleccione **Implementar trabajo en entorno de producción**.

4. Seleccione el signo **+** junto a **Trabajo de agente** y busque *Implementación de grupo de recursos de Azure*. Escriba los siguientes parámetros:

   |Configuración|Value|
   |-|-|
   |Nombre para mostrar| *Implementar myASAJob*|
   |Suscripción de Azure| Elija su suscripción.|
   |Acción| *Crear o actualizar grupo de recursos*|
   |Resource group| Elija un nombre para el grupo de recursos de producción que contendrá su trabajo de Stream Analytics.|
   |Location|Elija la ubicación del grupo de recursos de producción.|
   |Ubicación de la plantilla| *Artefacto vinculado*|
   |Plantilla| $(Build.ArtifactStagingDirectory)\drop\myASAJob.JobTemplate.json |
   |Parámetros de plantilla|($(Build.ArtifactStagingDirectory)\drop\myASAJob.JobTemplate.parameters.json|
   |Reemplazo de los parámetros de plantilla|-Input_IoTHub1_iotHubNamespace $(eventhubname)|
   |Modo de implementación|Incremental|

### <a name="create-release"></a>Crear versión

Para crear una versión, seleccione **Crear versión** en la esquina superior derecha.

![Crear una versión mediante Azure Pipelines](./media/setup-cicd-vs-code/create-release.png)

## <a name="additional-resources"></a>Recursos adicionales

Para usar la identidad administrada para Azure Data Lake Store Gen1 como receptor de salida, deberá proporcionar acceso a la entidad de servicio con PowerShell antes de la implementación en Azure. Más información acerca de cómo [implementar ADLS Gen1 con identidad administrada con la plantilla de Resource Manager](stream-analytics-managed-identities-adls.md#resource-manager-template-deployment).


## <a name="next-steps"></a>Pasos siguientes

* [Inicio rápido: Creación de un trabajo en la nube de Azure Stream Analytics en Visual Studio Code (versión preliminar)](quick-create-vs-code.md)
* [Prueba de las consultas de Stream Analytics localmente con Visual Studio Code (versión preliminar)](visual-studio-code-local-run.md)
* [Exploración de Azure Stream Analytics con Visual Studio Code (versión preliminar)](visual-studio-code-explore-jobs.md)
