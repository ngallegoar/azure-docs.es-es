---
title: Integración y entrega continuas en Azure Data Factory
description: Aprenda a usar la integración y la entrega continuas para mover canalizaciones de Data Factory de un entorno (desarrollo, prueba o producción) a otro.
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
author: djpmsft
ms.author: daperlov
ms.reviewer: maghan
manager: jroth
ms.topic: conceptual
ms.date: 02/12/2020
ms.openlocfilehash: 6aad01808ad155b745b614d8de6009386f0d2914
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/21/2020
ms.locfileid: "81687959"
---
# <a name="continuous-integration-and-delivery-in-azure-data-factory"></a>Integración y entrega continuas en Azure Data Factory

[!INCLUDE[appliesto-adf-xxx-md](includes/appliesto-adf-xxx-md.md)]

## <a name="overview"></a>Información general

La integración continua es el procedimiento de probar cada cambio realizado en el código base automáticamente y tan pronto como sea posible. La entrega continua sigue las pruebas realizadas durante la integración continua y envía los cambios a un sistema de ensayo o producción.

En Azure Data Factory, la integración y la entrega continuas (CI/CD) implican el traslado de canalizaciones de Data Factory de un entorno (desarrollo, prueba o producción) a otro. Puede usar la integración de la experiencia de la interfaz de usuario de Data Factory con plantillas de Azure Resource Manager para operaciones de CI/CD.

En la experiencia de la interfaz de usuario de Data Factory puede generar una plantilla de Resource Manager desde el menú desplegable **Plantilla de ARM**. Al seleccionar **Export ARM template** (Exportar plantilla de ARM), el portal genera la plantilla de Resource Manager de la factoría de datos y un archivo de configuración que incluye todas las cadenas de conexión y otros parámetros. Después, cree un archivo de configuración para cada entorno (desarrollo, prueba o producción). El archivo de plantilla de Resource Manager principal sigue siendo el mismo para todos los entornos.

Para ver una introducción de nueve minutos sobre esta característica y una demostración, vea este vídeo:

> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Continuous-integration-and-deployment-using-Azure-Data-Factory/player]

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="cicd-lifecycle"></a>Ciclo de vida de CI/CD

Aquí se muestra una descripción general de ejemplo del ciclo de vida de CI/CD en una factoría de datos de Azure configurada con Git de Azure Repos. Para más información sobre cómo configurar un repositorio Git, vea [Control de código fuente en Azure Data Factory](source-control.md).

1.  Se crea una factoría de datos de desarrollo y se configura con GIT de Azure Repos. Todos los desarrolladores deben tener permiso para crear recursos de Data Factory como canalizaciones y conjuntos de usuarios.

1.  A medida que los desarrolladores realizan cambios en sus ramas de características, depuran las ejecuciones de la canalización con los cambios más recientes. Para más información sobre cómo depurar una ejecución de canalización, consulte [Desarrollo y depuración iterativos con Azure Data Factory](iterative-development-debugging.md).

1.  Una vez que los desarrolladores estén satisfechos con los cambios, crean una solicitud de incorporación de cambios desde su rama de características a la rama maestra o rama de colaboración para que otros equipos del mismo nivel revisen sus cambios.

1.  Después de que la solicitud de incorporación de cambios se haya aprobado y los cambios se hayan combinado en la rama maestra, se pueden publicar en la factoría de desarrollo.

1.  Cuando el equipo está listo para implementar los cambios en la factoría de pruebas y después en la de producción, exporta la plantilla de Resource Manager desde la rama maestra.

1.  La plantilla de Resource Manager exportada se implementa con otros archivos de parámetros en la factoría de pruebas y en la de producción.

## <a name="create-a-resource-manager-template-for-each-environment"></a>Creación de una plantilla de Resource Manager para cada entorno

1. En la lista **Plantilla de ARM**, seleccione **Export ARM Template** (Exportar plantilla de ARM) para exportar la plantilla de Resource Manager de la factoría de datos en el entorno de desarrollo.

   ![Exportación de una plantilla de Resource Manager](media/continuous-integration-deployment/continuous-integration-image1.png)

1. En las factorías de datos de prueba y de producción, seleccione **Import ARM Template** (Importar plantilla de ARM). Esta acción abrirá Azure Portal, donde puede importar la plantilla exportada. Seleccione **Cree su propia plantilla en el editor** para abrir el editor de plantillas de Resource Manager.

   ![Creación de una plantilla propia](media/continuous-integration-deployment/custom-deployment-build-your-own-template.png) 

1. Seleccione **Cargar archivo** y después la plantilla de Resource Manager generada. Este es el archivo **arm_template.json** que se encuentra en el archivo ZIP exportado en el paso 1.

   ![Editar plantilla](media/continuous-integration-deployment/custom-deployment-edit-template.png)

1. En la sección de configuración, escriba los valores de configuración, como las credenciales del servicio vinculado. Cuando haya terminado, seleccione **Comprar** para implementar la plantilla de Resource Manager.

   ![Sección de configuración](media/continuous-integration-deployment/continuous-integration-image5.png)

### <a name="connection-strings"></a>Cadenas de conexión

Para obtener información sobre cómo configurar las cadenas de conexión, vea el artículo del conector. Por ejemplo, para Azure SQL Database, consulte [Copia de datos con una base de datos de Azure SQL como origen o destino mediante Azure Data Factory](connector-azure-sql-database.md). Para comprobar una cadena de conexión, puede abrir la vista de código para el recurso en la experiencia de la interfaz de usuario de Data Factory. En la vista de código, la parte de contraseña o clave de cuenta de la cadena de conexión se ha quitado. Para abrir la vista de código, seleccione el icono aquí resaltado:

![Apertura de la vista de código para ver la cadena de conexión](media/continuous-integration-deployment/continuous-integration-codeview.png)

## <a name="automate-continuous-integration-by-using-azure-pipelines-releases"></a>Automatización de la integración continua mediante versiones de Azure Pipelines

Aquí se ofrece una guía para configurar una versión de Azure Pipelines, que automatiza la implementación de una factoría de datos en varios entornos.

![Diagrama de integración continua con Azure Pipelines](media/continuous-integration-deployment/continuous-integration-image12.png)

### <a name="requirements"></a>Requisitos

-   Una suscripción a Azure vinculada a Visual Studio Team Foundation Server o Azure Repos que use el  [punto de conexión de servicio de Azure Resource Manager](https://docs.microsoft.com/azure/devops/pipelines/library/service-endpoints#sep-azure-resource-manager).

-   Una factoría de datos configurada con la integración de GIT de Azure Repos.

-   Un  [almacén de claves de Azure](https://azure.microsoft.com/services/key-vault/) que contenga los secretos para cada entorno.

### <a name="set-up-an-azure-pipelines-release"></a>Configuración de una versión de Azure Pipelines

1.  En [Azure DevOps](https://dev.azure.com/), abra el proyecto configurado con la factoría de datos.

1.  En el lado izquierdo de la página, seleccione **Canalizaciones** y después **Versiones**.

    ![Selección de Canalizaciones, Versiones](media/continuous-integration-deployment/continuous-integration-image6.png)

1.  Seleccione **Nueva canalización** o, si tiene canalizaciones existentes, seleccione **Nueva** y, luego **Nueva canalización de versión**.

1.  Seleccione la plantilla **Fase vacía**.

    ![Selección de Trabajo vacío](media/continuous-integration-deployment/continuous-integration-image13.png)

1.  En el cuadro **Nombre de la fase**, escriba el nombre del entorno.

1.  Seleccione **Agregar artefacto** y después el mismo repositorio configurado con la factoría de datos. Seleccione **adf_publish** para la **Rama predeterminada**. En **Versión predeterminada**, seleccione **Más reciente de la rama predeterminada**.

    ![Agregar un artefacto](media/continuous-integration-deployment/continuous-integration-image7.png)

1.  Añada una tarea de implementación de Azure Resource Manager:

    a.  En la vista de fase, seleccione **Ver tareas de la fase**.

    ![Vista de fase](media/continuous-integration-deployment/continuous-integration-image14.png)

    b.  Cree una nueva tarea. Busque **Implementación de un grupo de recursos de Azure** y, después, seleccione **Agregar**.

    c.  En la tarea de implementación, seleccione la suscripción, el grupo de recursos y la ubicación de la factoría de datos de destino. Proporcione las credenciales si es necesario.

    d.  En la lista **Acción**, seleccione **Create or update resource group** (Crear o actualizar grupo de recursos).

    e.  Seleccione el botón de puntos suspensivos ( **...** ) situado junto al cuadro **Plantilla**. Busque la plantilla de Azure Resource Manager creada mediante **Import ARM Template** (Importar plantilla de ARM) en la sección [Creación de una plantilla de Resource Manager para cada entorno](continuous-integration-deployment.md#create-a-resource-manager-template-for-each-environment) de este artículo. Busque este archivo en la carpeta <FactoryName> de la rama adf_publish.

    f.  Seleccione **…** junto al cuadro **Parámetros de plantilla** para elegir el archivo de parámetros. El archivo que elija dependerá de si ha creado una copia o si usa el archivo predeterminado, ARMTemplateParametersForFactory.json.

    g.  Seleccione **…** junto al cuadro **Reemplazar parámetros de plantilla** y escriba la información de la factoría de datos de destino. En el caso de las credenciales que proceden de Azure Key Vault, escriba el nombre del secreto entre comillas dobles. Por ejemplo, si el nombre del secreto es cred1, escriba **"$(cred1)"** para este valor.

    h. Seleccione **Incremental** para el **Modo de implementación**.

    > [!WARNING]
    > Si selecciona **Completo** para **Modo de implementación**, es posible que se eliminen los recursos existentes, incluidos todos los del grupo de recursos de destino que no están definidos en la plantilla de Resource Manager.

    ![Implementación de producción de Data Factory](media/continuous-integration-deployment/continuous-integration-image9.png)

1.  Guarde la canalización de versión.

1. Para desencadenar una versión, seleccione **Crear versión**.

   ![Selección de Crear versión](media/continuous-integration-deployment/continuous-integration-image10.png)

> [!IMPORTANT]
> En escenarios de CI/CD, el tipo de entorno de ejecución de integración (IR) en otros entornos debe ser el mismo. Por ejemplo, si tiene un IR autohospedado en el entorno de desarrollo, el mismo IR también debe ser de tipo autohospedado en otros entornos, como los de prueba y producción. Del mismo modo, si va a compartir los entornos de ejecución de integración entre varias fases, tendrá que configurarlos como autohospedados vinculados en todos los entornos: desarrollo, prueba y producción.

### <a name="get-secrets-from-azure-key-vault"></a>Obtención de secretos de Azure Key Vault

Si tiene secretos para pasar en una plantilla de Azure Resource Manager, se recomienda usar Azure Key Vault con la versión de Azure Pipelines.

Hay dos formas de administrar los secretos:

1.  Agregue los secretos al archivo de parámetros. Para más información, consulte [Uso de Azure Key Vault para pasar el valor de parámetro seguro durante la implementación](../azure-resource-manager/templates/key-vault-parameter.md).

    Cree una copia del archivo de parámetros que se ha cargado en la rama de publicación. Establezca los valores de los parámetros que quiera obtener del almacén de claves con este formato:

    ```json
    {
        "parameters": {
            "azureSqlReportingDbPassword": {
                "reference": {
                    "keyVault": {
                        "id": "/subscriptions/<subId>/resourceGroups/<resourcegroupId> /providers/Microsoft.KeyVault/vaults/<vault-name> "
                    },
                    "secretName": " < secret - name > "
                }
            }
        }
    }
    ```

    Al utilizar este método, el secreto se extrae automáticamente del almacén de claves.

    El archivo de parámetros también debe estar en la rama de publicación.

1. Agregue una [tarea de Azure Key Vault](https://docs.microsoft.com/azure/devops/pipelines/tasks/deploy/azure-key-vault) antes de la tarea de implementación de Azure Resource Manager que se ha descrito en la sección anterior:

    1.  En la pestaña **Tareas**, cree una tarea. Busque **Azure Key Vault** y agréguelo.

    1.  En la tarea de Key Vault, seleccione la suscripción en la que haya creado el almacén de claves. Proporcione credenciales si es necesario y, después, seleccione el almacén de claves.

    ![Adición de una tarea de Key Vault](media/continuous-integration-deployment/continuous-integration-image8.png)

#### <a name="grant-permissions-to-the-azure-pipelines-agent"></a>Concesión de permisos al agente de Azure Pipelines

Es posible que se produzca un error de acceso denegado en la tarea Azure Key Vault si no se han establecido los permisos correctos. Descargue los registros de la versión y busque el archivo .ps1 que contiene el comando para conceder permisos al agente de Azure Pipelines. Puede ejecutar el comando directamente. O bien, puede copiar el identificador de entidad de seguridad del archivo y añadir la directiva de acceso manualmente en Azure Portal. `Get` y `List` son los permisos mínimos necesarios.

### <a name="update-active-triggers"></a>Actualización de desencadenadores activos

Puede producirse un error en la implementación si intenta actualizar desencadenadores activos. Para actualizar desencadenadores activos, debe detenerlos manualmente e iniciarlos después de la implementación. Puede hacerlo mediante una tarea de Azure PowerShell:

1.  En la pestaña **Tareas** de la versión, agregue una tarea **Azure PowerShell**. Elija la versión de la tarea 4.*. 

1.  Seleccione la suscripción en la que se encuentra la factoría.

1.  Seleccione **Ruta de acceso del archivo de script** como tipo de script. Esto requiere guardar el script de PowerShell en el repositorio. El siguiente script de PowerShell sirve para detener desencadenadores:

    ```powershell
    $triggersADF = Get-AzDataFactoryV2Trigger -DataFactoryName $DataFactoryName -ResourceGroupName $ResourceGroupName

    $triggersADF | ForEach-Object { Stop-AzDataFactoryV2Trigger -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Name $_.name -Force }
    ```

Puede completar pasos similares (con la función `Start-AzDataFactoryV2Trigger`) para reiniciar los desencadenadores después de la implementación.

### <a name="sample-pre--and-post-deployment-script"></a>Script anterior y posterior a la implementación

Se puede usar este script de ejemplo para detener los desencadenadores antes de la implementación y reiniciarlos más adelante. El script también incluye código para eliminar recursos que se han quitado. Guarde el script en un repositorio de Git de Azure DevOps y haga referencia a él mediante una tarea de Azure PowerShell de la versión 4.*.

Al ejecutar un script anterior a la implementación, tendrá que especificar una variante de los siguientes parámetros en el campo **Argumentos de script**.

`-armTemplate "$(System.DefaultWorkingDirectory)/<your-arm-template-location>" -ResourceGroupName <your-resource-group-name> -DataFactoryName <your-data-factory-name>  -predeployment $true -deleteDeployment $false`


Al ejecutar un script posterior a la implementación, tendrá que especificar una variante de los siguientes parámetros en el campo **Argumentos de script**.

`-armTemplate "$(System.DefaultWorkingDirectory)/<your-arm-template-location>" -ResourceGroupName <your-resource-group-name> -DataFactoryName <your-data-factory-name>  -predeployment $false -deleteDeployment $true`

![Tarea de Azure PowerShell](media/continuous-integration-deployment/continuous-integration-image11.png)

Este es el script que se puede usar antes y después de la implementación. Contabiliza los recursos eliminados y las referencias a recursos.

```powershell
param
(
    [parameter(Mandatory = $false)] [String] $armTemplate,
    [parameter(Mandatory = $false)] [String] $ResourceGroupName,
    [parameter(Mandatory = $false)] [String] $DataFactoryName,
    [parameter(Mandatory = $false)] [Bool] $predeployment=$true,
    [parameter(Mandatory = $false)] [Bool] $deleteDeployment=$false
)

function getPipelineDependencies {
    param([System.Object] $activity)
    if ($activity.Pipeline) {
        return @($activity.Pipeline.ReferenceName)
    } elseif ($activity.Activities) {
        $result = @()
        $activity.Activities | ForEach-Object{ $result += getPipelineDependencies -activity $_ }
        return $result
    } elseif ($activity.ifFalseActivities -or $activity.ifTrueActivities) {
        $result = @()
        $activity.ifFalseActivities | Where-Object {$_ -ne $null} | ForEach-Object{ $result += getPipelineDependencies -activity $_ }
        $activity.ifTrueActivities | Where-Object {$_ -ne $null} | ForEach-Object{ $result += getPipelineDependencies -activity $_ }
        return $result
    } elseif ($activity.defaultActivities) {
        $result = @()
        $activity.defaultActivities | ForEach-Object{ $result += getPipelineDependencies -activity $_ }
        if ($activity.cases) {
            $activity.cases | ForEach-Object{ $_.activities } | ForEach-Object{$result += getPipelineDependencies -activity $_ }
        }
        return $result
    } else {
        return @()
    }
}

function pipelineSortUtil {
    param([Microsoft.Azure.Commands.DataFactoryV2.Models.PSPipeline]$pipeline,
    [Hashtable] $pipelineNameResourceDict,
    [Hashtable] $visited,
    [System.Collections.Stack] $sortedList)
    if ($visited[$pipeline.Name] -eq $true) {
        return;
    }
    $visited[$pipeline.Name] = $true;
    $pipeline.Activities | ForEach-Object{ getPipelineDependencies -activity $_ -pipelineNameResourceDict $pipelineNameResourceDict}  | ForEach-Object{
        pipelineSortUtil -pipeline $pipelineNameResourceDict[$_] -pipelineNameResourceDict $pipelineNameResourceDict -visited $visited -sortedList $sortedList
    }
    $sortedList.Push($pipeline)

}

function Get-SortedPipelines {
    param(
        [string] $DataFactoryName,
        [string] $ResourceGroupName
    )
    $pipelines = Get-AzDataFactoryV2Pipeline -DataFactoryName $DataFactoryName -ResourceGroupName $ResourceGroupName
    $ppDict = @{}
    $visited = @{}
    $stack = new-object System.Collections.Stack
    $pipelines | ForEach-Object{ $ppDict[$_.Name] = $_ }
    $pipelines | ForEach-Object{ pipelineSortUtil -pipeline $_ -pipelineNameResourceDict $ppDict -visited $visited -sortedList $stack }
    $sortedList = new-object Collections.Generic.List[Microsoft.Azure.Commands.DataFactoryV2.Models.PSPipeline]
    
    while ($stack.Count -gt 0) {
        $sortedList.Add($stack.Pop())
    }
    $sortedList
}

function triggerSortUtil {
    param([Microsoft.Azure.Commands.DataFactoryV2.Models.PSTrigger]$trigger,
    [Hashtable] $triggerNameResourceDict,
    [Hashtable] $visited,
    [System.Collections.Stack] $sortedList)
    if ($visited[$trigger.Name] -eq $true) {
        return;
    }
    $visited[$trigger.Name] = $true;
    $trigger.Properties.DependsOn | Where-Object {$_ -and $_.ReferenceTrigger} | ForEach-Object{
        triggerSortUtil -trigger $triggerNameResourceDict[$_.ReferenceTrigger.ReferenceName] -triggerNameResourceDict $triggerNameResourceDict -visited $visited -sortedList $sortedList
    }
    $sortedList.Push($trigger)
}

function Get-SortedTriggers {
    param(
        [string] $DataFactoryName,
        [string] $ResourceGroupName
    )
    $triggers = Get-AzDataFactoryV2Trigger -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName
    $triggerDict = @{}
    $visited = @{}
    $stack = new-object System.Collections.Stack
    $triggers | ForEach-Object{ $triggerDict[$_.Name] = $_ }
    $triggers | ForEach-Object{ triggerSortUtil -trigger $_ -triggerNameResourceDict $triggerDict -visited $visited -sortedList $stack }
    $sortedList = new-object Collections.Generic.List[Microsoft.Azure.Commands.DataFactoryV2.Models.PSTrigger]
    
    while ($stack.Count -gt 0) {
        $sortedList.Add($stack.Pop())
    }
    $sortedList
}

function Get-SortedLinkedServices {
    param(
        [string] $DataFactoryName,
        [string] $ResourceGroupName
    )
    $linkedServices = Get-AzDataFactoryV2LinkedService -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName
    $LinkedServiceHasDependencies = @('HDInsightLinkedService', 'HDInsightOnDemandLinkedService', 'AzureBatchLinkedService')
    $Akv = 'AzureKeyVaultLinkedService'
    $HighOrderList = New-Object Collections.Generic.List[Microsoft.Azure.Commands.DataFactoryV2.Models.PSLinkedService]
    $RegularList = New-Object Collections.Generic.List[Microsoft.Azure.Commands.DataFactoryV2.Models.PSLinkedService]
    $AkvList = New-Object Collections.Generic.List[Microsoft.Azure.Commands.DataFactoryV2.Models.PSLinkedService]

    $linkedServices | ForEach-Object {
        if ($_.Properties.GetType().Name -in $LinkedServiceHasDependencies) {
            $HighOrderList.Add($_)
        }
        elseif ($_.Properties.GetType().Name -eq $Akv) {
            $AkvList.Add($_)
        }
        else {
            $RegularList.Add($_)
        }
    }

    $SortedList = New-Object Collections.Generic.List[Microsoft.Azure.Commands.DataFactoryV2.Models.PSLinkedService]($HighOrderList.Count + $RegularList.Count + $AkvList.Count)
    $SortedList.AddRange($HighOrderList)
    $SortedList.AddRange($RegularList)
    $SortedList.AddRange($AkvList)
    $SortedList
}

$templateJson = Get-Content $armTemplate | ConvertFrom-Json
$resources = $templateJson.resources

#Triggers 
Write-Host "Getting triggers"
$triggersADF = Get-SortedTriggers -DataFactoryName $DataFactoryName -ResourceGroupName $ResourceGroupName
$triggersTemplate = $resources | Where-Object { $_.type -eq "Microsoft.DataFactory/factories/triggers" }
$triggerNames = $triggersTemplate | ForEach-Object {$_.name.Substring(37, $_.name.Length-40)}
$activeTriggerNames = $triggersTemplate | Where-Object { $_.properties.runtimeState -eq "Started" -and ($_.properties.pipelines.Count -gt 0 -or $_.properties.pipeline.pipelineReference -ne $null)} | ForEach-Object {$_.name.Substring(37, $_.name.Length-40)}
$deletedtriggers = $triggersADF | Where-Object { $triggerNames -notcontains $_.Name }
$triggerstostop = $triggerNames | where { ($triggersADF | Select-Object name).name -contains $_ }

if ($predeployment -eq $true) {
    #Stop all triggers
    Write-Host "Stopping deployed triggers"
    $triggerstostop | ForEach-Object { 
        Write-host "Disabling trigger " $_
        Remove-AzDataFactoryV2TriggerSubscription -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Name $_ -Force
    $status = Get-AzDataFactoryV2TriggerSubscriptionStatus -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Name $_
    while ($status.Status -ne "Disabled"){
            Start-Sleep -s 15
            $status = Get-AzDataFactoryV2TriggerSubscriptionStatus -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Name $_
    }
    Stop-AzDataFactoryV2Trigger -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Name $_ -Force 
    }
}
else {
    #Deleted resources
    #pipelines
    Write-Host "Getting pipelines"
    $pipelinesADF = Get-SortedPipelines -DataFactoryName $DataFactoryName -ResourceGroupName $ResourceGroupName
    $pipelinesTemplate = $resources | Where-Object { $_.type -eq "Microsoft.DataFactory/factories/pipelines" }
    $pipelinesNames = $pipelinesTemplate | ForEach-Object {$_.name.Substring(37, $_.name.Length-40)}
    $deletedpipelines = $pipelinesADF | Where-Object { $pipelinesNames -notcontains $_.Name }
    #dataflows
    $dataflowsADF = Get-AzDataFactoryV2DataFlow -DataFactoryName $DataFactoryName -ResourceGroupName $ResourceGroupName
    $dataflowsTemplate = $resources | Where-Object { $_.type -eq "Microsoft.DataFactory/factories/dataflows" }
    $dataflowsNames = $dataflowsTemplate | ForEach-Object {$_.name.Substring(37, $_.name.Length-40) }
    $deleteddataflow = $dataflowsADF | Where-Object { $dataflowsNames -notcontains $_.Name }
    #datasets
    Write-Host "Getting datasets"
    $datasetsADF = Get-AzDataFactoryV2Dataset -DataFactoryName $DataFactoryName -ResourceGroupName $ResourceGroupName
    $datasetsTemplate = $resources | Where-Object { $_.type -eq "Microsoft.DataFactory/factories/datasets" }
    $datasetsNames = $datasetsTemplate | ForEach-Object {$_.name.Substring(37, $_.name.Length-40) }
    $deleteddataset = $datasetsADF | Where-Object { $datasetsNames -notcontains $_.Name }
    #linkedservices
    Write-Host "Getting linked services"
    $linkedservicesADF = Get-SortedLinkedServices -DataFactoryName $DataFactoryName -ResourceGroupName $ResourceGroupName
    $linkedservicesTemplate = $resources | Where-Object { $_.type -eq "Microsoft.DataFactory/factories/linkedservices" }
    $linkedservicesNames = $linkedservicesTemplate | ForEach-Object {$_.name.Substring(37, $_.name.Length-40)}
    $deletedlinkedservices = $linkedservicesADF | Where-Object { $linkedservicesNames -notcontains $_.Name }
    #Integrationruntimes
    Write-Host "Getting integration runtimes"
    $integrationruntimesADF = Get-AzDataFactoryV2IntegrationRuntime -DataFactoryName $DataFactoryName -ResourceGroupName $ResourceGroupName
    $integrationruntimesTemplate = $resources | Where-Object { $_.type -eq "Microsoft.DataFactory/factories/integrationruntimes" }
    $integrationruntimesNames = $integrationruntimesTemplate | ForEach-Object {$_.name.Substring(37, $_.name.Length-40)}
    $deletedintegrationruntimes = $integrationruntimesADF | Where-Object { $integrationruntimesNames -notcontains $_.Name }

    #Delete resources
    Write-Host "Deleting triggers"
    $deletedtriggers | ForEach-Object { 
        Write-Host "Deleting trigger "  $_.Name
        $trig = Get-AzDataFactoryV2Trigger -name $_.Name -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName
        if ($trig.RuntimeState -eq "Started") {
            Stop-AzDataFactoryV2Trigger -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Name $_.Name -Force 
        }
        Remove-AzDataFactoryV2Trigger -Name $_.Name -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Force 
    }
    Write-Host "Deleting pipelines"
    $deletedpipelines | ForEach-Object { 
        Write-Host "Deleting pipeline " $_.Name
        Remove-AzDataFactoryV2Pipeline -Name $_.Name -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Force 
    }
    Write-Host "Deleting dataflows"
    $deleteddataflow | ForEach-Object { 
        Write-Host "Deleting dataflow " $_.Name
        Remove-AzDataFactoryV2DataFlow -Name $_.Name -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Force 
    }
    Write-Host "Deleting datasets"
    $deleteddataset | ForEach-Object { 
        Write-Host "Deleting dataset " $_.Name
        Remove-AzDataFactoryV2Dataset -Name $_.Name -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Force 
    }
    Write-Host "Deleting linked services"
    $deletedlinkedservices | ForEach-Object { 
        Write-Host "Deleting Linked Service " $_.Name
        Remove-AzDataFactoryV2LinkedService -Name $_.Name -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Force 
    }
    Write-Host "Deleting integration runtimes"
    $deletedintegrationruntimes | ForEach-Object { 
        Write-Host "Deleting integration runtime " $_.Name
        Remove-AzDataFactoryV2IntegrationRuntime -Name $_.Name -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Force 
    }

    if ($deleteDeployment -eq $true) {
        Write-Host "Deleting ARM deployment ... under resource group: " $ResourceGroupName
        $deployments = Get-AzResourceGroupDeployment -ResourceGroupName $ResourceGroupName
        $deploymentsToConsider = $deployments | Where { $_.DeploymentName -like "ArmTemplate_master*" -or $_.DeploymentName -like "ArmTemplateForFactory*" } | Sort-Object -Property Timestamp -Descending
        $deploymentName = $deploymentsToConsider[0].DeploymentName

       Write-Host "Deployment to be deleted: " $deploymentName
        $deploymentOperations = Get-AzResourceGroupDeploymentOperation -DeploymentName $deploymentName -ResourceGroupName $ResourceGroupName
        $deploymentsToDelete = $deploymentOperations | Where { $_.properties.targetResource.id -like "*Microsoft.Resources/deployments*" }

        $deploymentsToDelete | ForEach-Object { 
            Write-host "Deleting inner deployment: " $_.properties.targetResource.id
            Remove-AzResourceGroupDeployment -Id $_.properties.targetResource.id
        }
        Write-Host "Deleting deployment: " $deploymentName
        Remove-AzResourceGroupDeployment -ResourceGroupName $ResourceGroupName -Name $deploymentName
    }

    #Start active triggers - after cleanup efforts
    Write-Host "Starting active triggers"
    $activeTriggerNames | ForEach-Object { 
        Write-host "Enabling trigger " $_
        Add-AzDataFactoryV2TriggerSubscription -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Name $_ -Force
    $status = Get-AzDataFactoryV2TriggerSubscriptionStatus -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Name $_
    while ($status.Status -ne "Enabled"){
            Start-Sleep -s 15
            $status = Get-AzDataFactoryV2TriggerSubscriptionStatus -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Name $_
    }
    Start-AzDataFactoryV2Trigger -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Name $_ -Force 
    }
}
```

## <a name="use-custom-parameters-with-the-resource-manager-template"></a>Usar parámetros personalizados con la plantilla de Resource Manager

Si está en modo GIT, puede reemplazar las propiedades predeterminadas en la plantilla de Resource Manager para establecer propiedades que están parametrizadas en la plantilla y propiedades que están codificadas de forma rígida. Es posible que desee reemplazar la plantilla de parametrización predeterminada en estos escenarios:

* Se usa CI/CD automatizada y se quieren cambiar algunas propiedades durante la implementación de Resource Manager, pero las propiedades no están parametrizadas de forma predeterminada.
* La fábrica es tan grande que la plantilla de Resource Manager predeterminada no es válida porque contiene más parámetros que el número máximo permitido (256).

En estas condiciones, para reemplazar la plantilla de parametrización predeterminada, cree un archivo denominado **arm-template-parameters-definition.json** en la carpeta especificada como carpeta raíz para la integración de Git de los datos de la factoría de datos. Debe usar ese nombre de archivo exacto. Data Factory lee el archivo de la rama en la que está actualmente en el portal de Azure Data Factory, no solo de la rama de colaboración. Puede crear o editar el archivo desde una rama privada, donde pueda probar los cambios si selecciona **Export ARM Template** (Exportar plantilla de ARM) en la interfaz de usuario. Después, puede combinar el archivo en la rama de colaboración. Si no se encuentra ningún archivo, se usa la plantilla predeterminada.

> [!NOTE]
> Una plantilla de parametrización personalizada no cambia el límite de 256 parámetros de plantilla de Resource Manager. Le permite elegir y reducir el número de propiedades parametrizadas.

### <a name="syntax-of-a-custom-parameters-file"></a>Sintaxis de un archivo de parámetros personalizados

Puede usar las instrucciones siguientes durante la creación del archivo de parámetros personalizado. El archivo consta de una sección para cada tipo de entidad: desencadenador, canalización, servicio vinculado, conjunto de datos, entorno de ejecución de integración, etc.
* Escriba la ruta de acceso de la propiedad en el tipo de entidad correspondiente.
* El establecimiento de un nombre de propiedad en  `*` indica que quiere parametrizar todas las propiedades que incluye (solo en el primer nivel, no de forma recursiva). También puede proporcionar excepciones a esta configuración.
* El establecimiento del valor de una propiedad como una cadena indica que quiere parametrizar la propiedad. Use el formato `<action>:<name>:<stype>`.
   *  `<action>` puede ser uno de estos caracteres:
      * `=` significa que el valor actual debe conservarse como el valor predeterminado para el parámetro.
      * `-` significa que no se debe conservar el valor predeterminado para el parámetro.
      * `|` es un caso especial para los secretos de Azure Key Vault para una cadena de conexión o claves.
   * `<name>` es el nombre del parámetro. Si está en blanco, toma el nombre de la propiedad. Si el valor empieza por un carácter `-`, el nombre está abreviado. Por ejemplo, `AzureStorage1_properties_typeProperties_connectionString` se abreviará a `AzureStorage1_connectionString`.
   * `<stype>` es el tipo del parámetro. Si `<stype>` está en blanco, el tipo predeterminado es `string`. Los valores admitidos son: `string`, `bool`, `number`, `object` y `securestring`.
* Al especificar una matriz en el archivo de definición indica que la propiedad coincidente en la plantilla es una matriz. Data Factory recorre en iteración todos los objetos de la matriz mediante la definición especificada en el objeto del entorno de ejecución de integración de la matriz. El segundo objeto, una cadena, se convierte en el nombre de la propiedad, que se utiliza como el nombre del parámetro para cada iteración.
* Una definición no puede ser específica de una instancia de recurso. Cualquier definición se aplica a todos los recursos de ese tipo.
* De forma predeterminada, todas las cadenas seguras, como los secretos de Key Vault, las cadenas de conexión, las claves y los tokens, están parametrizadas.
 
### <a name="sample-parameterization-template"></a>Plantilla de parametrización de ejemplo

En el ejemplo siguiente se muestra el aspecto que podría tener una plantilla de parametrización:

```json
{
    "Microsoft.DataFactory/factories/pipelines": {
        "properties": {
            "activities": [{
                "typeProperties": {
                    "waitTimeInSeconds": "-::number",
                    "headers": "=::object"
                }
            }]
        }
    },
    "Microsoft.DataFactory/factories/integrationRuntimes": {
        "properties": {
            "typeProperties": {
                "*": "="
            }
        }
    },
    "Microsoft.DataFactory/factories/triggers": {
        "properties": {
            "typeProperties": {
                "recurrence": {
                    "*": "=",
                    "interval": "=:triggerSuffix:number",
                    "frequency": "=:-freq"
                },
                "maxConcurrency": "="
            }
        }
    },
    "Microsoft.DataFactory/factories/linkedServices": {
        "*": {
            "properties": {
                "typeProperties": {
                    "accountName": "=",
                    "username": "=",
                    "connectionString": "|:-connectionString:secureString",
                    "secretAccessKey": "|"
                }
            }
        },
        "AzureDataLakeStore": {
            "properties": {
                "typeProperties": {
                    "dataLakeStoreUri": "="
                }
            }
        }
    },
    "Microsoft.DataFactory/factories/datasets": {
        "properties": {
            "typeProperties": {
                "*": "="
            }
        }
    }
}
```
Esta es una explicación de cómo se construye la plantilla anterior, desglosada por tipo de recurso.

#### <a name="pipelines"></a>Procesos
    
* Cualquier propiedad de la ruta de acceso `activities/typeProperties/waitTimeInSeconds` está parametrizada. Cualquier actividad en una canalización que tiene una propiedad de nivel de código denominada `waitTimeInSeconds` (por ejemplo, la actividad `Wait`) está parametrizada como un número, con un nombre predeterminado. Sin embargo, no tendrá un valor predeterminado en la plantilla de Resource Manager. Será una entrada obligatoria durante la implementación de Resource Manager.
* De forma similar, una propiedad denominada `headers` (por ejemplo, en una actividad `Web`) está parametrizada con el tipo `object` (JObject). Tiene un valor predeterminado, que es el mismo que el de la factoría de origen.

#### <a name="integrationruntimes"></a>IntegrationRuntimes

* Todas las propiedades bajo la ruta de acceso `typeProperties` están parametrizadas con sus valores predeterminados correspondientes. Por ejemplo, hay dos propiedades en las propiedades de tipo `IntegrationRuntimes`: `computeProperties` y `ssisProperties`. Ambos tipos de propiedades se crean con sus valores predeterminados y tipos (objeto) respectivos.

#### <a name="triggers"></a>Desencadenadores

* En `typeProperties`, hay dos propiedades parametrizadas. La primera de ellas es `maxConcurrency`, que tiene especificado un valor predeterminado y es de tipo `string`. Tiene el nombre de parámetro predeterminado `<entityName>_properties_typeProperties_maxConcurrency`.
* La propiedad `recurrence` también está parametrizada. En ella, se especifica que todas las propiedades de ese nivel están parametrizadas como cadenas, con valores predeterminados y los nombres de parámetro. Una excepción es la propiedad `interval`, que está parametrizada como el tipo `number`. El sufijo del nombre del parámetro es `<entityName>_properties_typeProperties_recurrence_triggerSuffix`. De forma similar, la propiedad `freq` es una cadena y está parametrizada como una cadena. Sin embargo, la propiedad `freq` está parametrizada sin un valor predeterminado. El nombre está abreviado y con un sufijo. Por ejemplo, `<entityName>_freq`.

#### <a name="linkedservices"></a>LinkedServices

* Los servicios vinculados son únicos. Dado que los servicios vinculados y los conjuntos de datos tienen una gran variedad de tipos, puede proporcionar una personalización específica de tipo. En este ejemplo, para todos los servicios vinculados de tipo `AzureDataLakeStore`, se aplicará una plantilla específica. Para todos los demás (a través de `*`), se aplicará otra plantilla.
* La propiedad `connectionString` se parametrizará como un valor `securestring`. No tendrá un valor predeterminado. Tendrá un nombre de parámetro abreviado con el sufijo `connectionString`.
* La propiedad `secretAccessKey` resulta ser `AzureKeyVaultSecret` (por ejemplo, en un servicio vinculado de Amazon S3). Se parametriza automáticamente como un secreto de Azure Key Vault y se captura desde el almacén de claves configurado. También puede parametrizar el propio almacén de claves.

#### <a name="datasets"></a>Conjuntos de datos

* Aunque la personalización específica de tipos está disponible para conjuntos de datos, puede proporcionar configuración sin necesidad de una configuración explícita de nivel \*. En el ejemplo anterior, todas las propiedades del conjunto de datos de `typeProperties` están parametrizadas.

### <a name="default-parameterization-template"></a>Plantilla de parametrización predeterminada

Aquí se muestra la plantilla de parametrización predeterminada actual. Si solo tiene que agregar algunos parámetros, la modificación directa de esta plantilla puede ser una buena idea porque no perderá la estructura de parametrización existente.

```json
{
    "Microsoft.DataFactory/factories/pipelines": {
    },
    "Microsoft.DataFactory/factories/dataflows": {
    },
    "Microsoft.DataFactory/factories/integrationRuntimes":{
        "properties": {
            "typeProperties": {
                "ssisProperties": {
                    "catalogInfo": {
                        "catalogServerEndpoint": "=",
                        "catalogAdminUserName": "=",
                        "catalogAdminPassword": {
                            "value": "-::secureString"
                        }
                    },
                    "customSetupScriptProperties": {
                        "sasToken": {
                            "value": "-::secureString"
                        }
                    }
                },
                "linkedInfo": {
                    "key": {
                        "value": "-::secureString"
                    },
                    "resourceId": "="
                }
            }
        }
    },
    "Microsoft.DataFactory/factories/triggers": {
        "properties": {
            "pipelines": [{
                    "parameters": {
                        "*": "="
                    }
                },  
                "pipelineReference.referenceName"
            ],
            "pipeline": {
                "parameters": {
                    "*": "="
                }
            },
            "typeProperties": {
                "scope": "="
            }

        }
    },
    "Microsoft.DataFactory/factories/linkedServices": {
        "*": {
            "properties": {
                "typeProperties": {
                    "accountName": "=",
                    "username": "=",
                    "userName": "=",
                    "accessKeyId": "=",
                    "servicePrincipalId": "=",
                    "userId": "=",
                    "clientId": "=",
                    "clusterUserName": "=",
                    "clusterSshUserName": "=",
                    "hostSubscriptionId": "=",
                    "clusterResourceGroup": "=",
                    "subscriptionId": "=",
                    "resourceGroupName": "=",
                    "tenant": "=",
                    "dataLakeStoreUri": "=",
                    "baseUrl": "=",
                    "database": "=",
                    "serviceEndpoint": "=",
                    "batchUri": "=",
                    "poolName": "=",
                    "databaseName": "=",
                    "systemNumber": "=",
                    "server": "=",
                    "url":"=",
                    "aadResourceId": "=",
                    "connectionString": "|:-connectionString:secureString"
                }
            }
        },
        "Odbc": {
            "properties": {
                "typeProperties": {
                    "userName": "=",
                    "connectionString": {
                        "secretName": "="
                    }
                }
            }
        }
    },
    "Microsoft.DataFactory/factories/datasets": {
        "*": {
            "properties": {
                "typeProperties": {
                    "folderPath": "=",
                    "fileName": "="
                }
            }
        }}
}
```

En el ejemplo siguiente se muestra cómo agregar un único valor a la plantilla de parametrización predeterminada. Solamente se quiere agregar al archivo de parámetros un identificador de clúster interactivo de Azure Databricks existente para un servicio vinculado de Databricks. Tenga en cuenta que este archivo es el mismo que el anterior, salvo por la adición de `existingClusterId` en el campo de propiedades de `Microsoft.DataFactory/factories/linkedServices`.

```json
{
    "Microsoft.DataFactory/factories/pipelines": {
    },
    "Microsoft.DataFactory/factories/dataflows": {
    },
    "Microsoft.DataFactory/factories/integrationRuntimes":{
        "properties": {
            "typeProperties": {
                "ssisProperties": {
                    "catalogInfo": {
                        "catalogServerEndpoint": "=",
                        "catalogAdminUserName": "=",
                        "catalogAdminPassword": {
                            "value": "-::secureString"
                        }
                    },
                    "customSetupScriptProperties": {
                        "sasToken": {
                            "value": "-::secureString"
                        }
                    }
                },
                "linkedInfo": {
                    "key": {
                        "value": "-::secureString"
                    },
                    "resourceId": "="
                }
            }
        }
    },
    "Microsoft.DataFactory/factories/triggers": {
        "properties": {
            "pipelines": [{
                    "parameters": {
                        "*": "="
                    }
                },  
                "pipelineReference.referenceName"
            ],
            "pipeline": {
                "parameters": {
                    "*": "="
                }
            },
            "typeProperties": {
                "scope": "="
            }
 
        }
    },
    "Microsoft.DataFactory/factories/linkedServices": {
        "*": {
            "properties": {
                "typeProperties": {
                    "accountName": "=",
                    "username": "=",
                    "userName": "=",
                    "accessKeyId": "=",
                    "servicePrincipalId": "=",
                    "userId": "=",
                    "clientId": "=",
                    "clusterUserName": "=",
                    "clusterSshUserName": "=",
                    "hostSubscriptionId": "=",
                    "clusterResourceGroup": "=",
                    "subscriptionId": "=",
                    "resourceGroupName": "=",
                    "tenant": "=",
                    "dataLakeStoreUri": "=",
                    "baseUrl": "=",
                    "database": "=",
                    "serviceEndpoint": "=",
                    "batchUri": "=",
            "poolName": "=",
                    "databaseName": "=",
                    "systemNumber": "=",
                    "server": "=",
                    "url":"=",
                    "aadResourceId": "=",
                    "connectionString": "|:-connectionString:secureString",
                    "existingClusterId": "-"
                }
            }
        },
        "Odbc": {
            "properties": {
                "typeProperties": {
                    "userName": "=",
                    "connectionString": {
                        "secretName": "="
                    }
                }
            }
        }
    },
    "Microsoft.DataFactory/factories/datasets": {
        "*": {
            "properties": {
                "typeProperties": {
                    "folderPath": "=",
                    "fileName": "="
                }
            }
        }}
}
```

## <a name="linked-resource-manager-templates"></a>Plantillas vinculadas de Resource Manager

Si ha configurado CI/CD para las factorías de datos, es posible que supere los límites de plantilla de Azure Resource Manager a medida que la fábrica aumente de tamaño. Por ejemplo, un límite es el número máximo de recursos de una plantilla de Resource Manager. Para acomodar factorías más grandes mientras se genera la plantilla completa de Resource Manager para una factoría, ahora Data Factory genera plantillas de Resource Manager vinculadas. Con esta característica, la carga de la factoría completa se divide en varios archivos, para que no esté restringido por los límites.

Si ha configurado Git, se generan las plantillas vinculadas y se guardan junto con las plantillas completas de Resource Manager en la rama adf_publish, en una carpeta nueva denominada linkedTemplates:

![Carpeta de plantillas vinculadas de Resource Manager](media/continuous-integration-deployment/linked-resource-manager-templates.png)

Normalmente, las plantillas vinculadas de Resource Manager tienen una plantilla principal y un conjunto de plantillas secundarias vinculadas a la principal. La plantilla principal se denomina ArmTemplate_master.json y las plantillas secundarias se denominan con el patrón ArmTemplate_0.json, ArmTemplate_1.json, etc. 

Para usar plantillas vinculadas en lugar de la plantilla de Resource Manager completa, actualice la tarea de CI/CD para que apunte a ArmTemplate_master.json en lugar de ArmTemplateForFactory.json (la plantilla de Resource Manager completa). Resource Manager también necesita que cargue las plantillas vinculadas en una cuenta de almacenamiento para que Azure pueda acceder a ellas durante la implementación. Para más información, vea [Implementación de plantillas vinculadas de Resource Manager con VSTS](https://blogs.msdn.microsoft.com/najib/2018/04/22/deploying-linked-arm-templates-with-vsts/).

No se olvide de agregar los scripts de Data Factory en la canalización de CI/CD antes y después de la tarea de implementación.

Si no ha configurado Git, puede acceder a las plantillas vinculadas a través de **Export ARM Template** en la lista **Plantilla de ARM**.

## <a name="hotfix-production-branch"></a>Rama de producción de revisión

Si implementa una factoría en producción y se da cuenta de que hay un error que se debe corregir de inmediato, pero no puede implementar la rama de colaboración actual, es posible que deba implementar una revisión. Este enfoque se conoce como ingeniería de corrección rápida o QFE.

1.    En Azure DevOps, vaya a la versión que se ha implementado en producción. Busque la última confirmación que se ha implementado.

2.    En el mensaje de confirmación, obtenga el identificador de confirmación de la rama de colaboración.

3.    Cree una rama de revisión a partir de esa confirmación.

4.    Vaya a la experiencia de la interfaz de usuario de Azure Data Factory y cambie a la rama de revisión.

5.    Mediante la experiencia de la interfaz de usuario de Azure Data Factory, corrija el error. Guarde los cambios.

6.    Una vez comprobada la corrección, seleccione **Export ARM Template** (Exportar plantilla de ARM) para obtener la plantilla de Resource Manager de revisión.

7.    Inserte manualmente esta compilación en la rama adf_publish.

8.    Si ha configurado la canalización de versión para que se desencadene automáticamente en función de las inserciones en el repositorio de adf_publish, se iniciará una versión nueva de forma automática. De lo contrario, ponga en cola manualmente una versión.

9.    Implemente la versión de revisión en las factorías de prueba y producción. Esta versión contiene la carga de producción anterior más la revisión realizada en el paso 5.

10.    Agregue los cambios de la revisión a la rama de desarrollo para que las versiones posteriores no incluyan el mismo error.

## <a name="best-practices-for-cicd"></a>Procedimientos recomendados para CI/CD

Si usa la integración de Git con la factoría de datos y tiene una canalización de CI/CD que mueve los cambios desde el entorno de desarrollo al de prueba y, luego, al de producción, los procedimientos recomendados son los siguientes:

-   **Integración de Git**. Solo tiene que configurar la factoría de datos de desarrollo con la integración de Git. Los cambios en los entornos de prueba y producción se implementan a través de CI/CD y no se necesita la integración Git.

-   **Script de CI/CD de Data Factory**. Antes del paso de implementación de Resource Manager en CI/CD, debe completar ciertas tareas, como detener y reiniciar los desencadenadores y realizar la limpieza. Se recomienda usar scripts de PowerShell antes y después de la implementación. Para más información, vea [Actualización de desencadenadores activos](#update-active-triggers).

-   **Entornos de ejecución de integración y uso compartido**. Los entornos de ejecución de integración no cambian a menudo y son similares en todas las fases de CI/CD. Por tanto, Data Factory espera que el usuario tenga el mismo nombre y tipo de entorno de ejecución de integración en todas las etapas de CI/CD. Si quiere compartir entornos de ejecución de integración en todas las fases, considere la posibilidad de usar una factoría ternaria solo para contener los entornos de ejecución de integración compartidos. Puede usar esta factoría compartida en todos los entornos como un tipo de entorno de ejecución de integración vinculado.

-   **Key Vault**. Cuando usa servicios vinculados basados en Azure Key Vault, puede aprovecharlos todavía más si mantiene almacenes de claves independientes para otros entornos. También puede configurar niveles de permisos independientes para cada almacén de claves. Por ejemplo, es posible que no quiera que los miembros del equipo tengan permisos para ver los secretos de producción. Si sigue este enfoque, se recomienda mantener los mismos nombres de secreto en todas las fases. Si conserva los mismos nombres, no tendrá que cambiar las plantillas de Resource Manager en los entornos de CI/CD, porque lo único que cambia es el nombre del almacén de claves, que es uno de los parámetros de la plantilla de Resource Manager.

## <a name="unsupported-features"></a>Características no admitidas

- Por diseño, Data Factory no permite la selección exclusiva de confirmaciones ni la publicación selectiva de recursos. Las publicaciones incluirán todos los cambios realizados en la factoría de datos.

    - Las entidades de Data Factory dependen unas de otras. Por ejemplo, los desencadenadores dependen de las canalizaciones y las canalizaciones dependen de los conjuntos de datos y otras canalizaciones. La publicación selectiva de un subconjunto de recursos podría provocar comportamientos y errores inesperados.
    - En casos excepcionales, cuando necesite la publicación selectiva, considere la posibilidad de usar una revisión. Para más información, vea [Rama de producción de revisión](#hotfix-production-branch).

-   No es posible publicar desde ramas privadas.

-   En la actualidad no se pueden hospedar proyectos en Bitbucket.
