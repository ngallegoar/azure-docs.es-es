---
title: Implementación de recursos con una plantilla y PowerShell
description: Use Azure Resource Manager y Azure PowerShell para implementar recursos para Azure. Los recursos se definen en una plantilla de Resource Manager.
ms.topic: conceptual
ms.date: 03/16/2020
ms.openlocfilehash: e595aa8f86a24e59c8e00d24ea8e9dcb0875a8f4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80153274"
---
# <a name="deploy-resources-with-arm-templates-and-azure-powershell"></a>Implementación de recursos con las plantillas de Resource Manager y Azure PowerShell

Aprenda a utilizar Azure PowerShell con plantillas de Azure Resource Manager (ARM) para implementar recursos en Azure. Para más información sobre los conceptos asociados a la implementación y administración de las soluciones de Azure, consulte [Introducción a la implementación de plantillas](overview.md).

## <a name="deployment-scope"></a>Ámbito de la implementación

La implementación puede tener como destino un grupo de recursos, una suscripción, un grupo de administración o un inquilino. En la mayoría de los casos, la implementación tendrá como destino un grupo de recursos. Para aplicar directivas y asignaciones de roles en un ámbito mayor, use implementaciones de suscripción, de grupo de administración o de inquilino. Al implementar en una suscripción, puede crear un grupo de recursos e implementar recursos en él.

Según el ámbito de la implementación, usará comandos diferentes.

Para implementar en un **grupo de recursos**, utilice [New-AzResourceGroupDeployment](/powershell/module/az.resources/new-azresourcegroupdeployment):

```azurepowershell
New-AzResourceGroupDeployment -ResourceGroupName <resource-group-name> -TemplateFile <path-to-template>
```

Para hacer la implementación en una **suscripción**, use New-AzSubscriptionDeployment:

```azurepowershell
New-AzSubscriptionDeployment -Location <location> -TemplateFile <path-to-template>
```

Para más información sobre las implementaciones en el nivel de suscripción, consulte [Creación de grupos de recursos y otros recursos en el nivel de suscripción](deploy-to-subscription.md).

Para hacer la implementación en un **grupo de administración**, use [New-AzManagementGroupDeployment](/powershell/module/az.resources/New-AzManagementGroupDeployment).

```azurepowershell
New-AzManagementGroupDeployment -Location <location> -TemplateFile <path-to-template>
```

Para obtener más información sobre las implementaciones de nivel de grupo de administración, consulte [Creación de recursos en el nivel de grupo de administración](deploy-to-management-group.md).

Para hacer la implementación en un **inquilino**, use [New-AzTenantDeployment](/powershell/module/az.resources/new-aztenantdeployment).

```azurepowershell
New-AzTenantDeployment -Location <location> -TemplateFile <path-to-template>
```

Para más información sobre las implementaciones a nivel de inquilino, consulte [Creación de recursos en el nivel de inquilino](deploy-to-tenant.md).

Los ejemplos de este artículo usan las implementaciones del grupo de recursos.

## <a name="prerequisites"></a>Prerrequisitos

Necesita una plantilla para implementar. Si aún no tiene ninguna, descargue una [plantilla de ejemplo](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.json) desde el repositorio Azure Quickstart Templates y guárdela. El nombre de archivo local que se utiliza en este artículo es **c:\MyTemplates\azuredeploy.json**.

A menos que use Azure Cloud Shell para implementar las plantillas, deberá instalar Azure PowerShell y conectarse a Azure:

- **Instale los cmdlets de Azure PowerShell en el equipo local.** Para más información, consulte el artículo de [introducción a Azure PowerShell](/powershell/azure/get-started-azureps).
- **Conéctese a Azure utilizando [Connect-AZAccount](/powershell/module/az.accounts/connect-azaccount)** . Si tiene varias suscripciones de Azure, es posible que también tenga que ejecutar [Set-AzContext](/powershell/module/Az.Accounts/Set-AzContext). Para más información, consulte [Use multiple Azure subscriptions](/powershell/azure/manage-subscriptions-azureps) (Uso de varias suscripciones de Azure).

## <a name="deploy-local-template"></a>Implementar una plantilla local

En el ejemplo siguiente se crea un grupo de recursos y se implementa una plantilla desde la máquina local. El nombre del grupo de recursos solo puede incluir caracteres alfanuméricos, puntos, guiones bajos, guiones y paréntesis. Puede tener hasta 90 caracteres. No puede terminar en punto.

```azurepowershell
$resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"
$location = Read-Host -Prompt "Enter the location (i.e. centralus)"

New-AzResourceGroup -Name $resourceGroupName -Location $location
New-AzResourceGroupDeployment -ResourceGroupName $resourceGroupName `
  -TemplateFile c:\MyTemplates\azuredeploy.json
```

La implementación puede demorar unos minutos en completarse.

## <a name="deploy-remote-template"></a>Implementación de una plantilla remota

En lugar de almacenar las plantillas de ARM en el equipo local, quizás prefiera almacenarlas en una ubicación externa. Puede almacenar plantillas en un repositorio de control de código fuente (por ejemplo, GitHub). O bien, puede almacenarlas en una cuenta de Azure Storage para el acceso compartido en su organización.

Para implementar una plantilla externa, use el parámetro **TemplateUri**. Use el identificador URI en el ejemplo para implementar la plantilla de ejemplo de GitHub.

```azurepowershell
$resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"
$location = Read-Host -Prompt "Enter the location (i.e. centralus)"

New-AzResourceGroup -Name $resourceGroupName -Location $location
New-AzResourceGroupDeployment -ResourceGroupName $resourceGroupName `
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.json
```

En el ejemplo anterior, se requiere un identificador URI accesible públicamente para la plantilla, que funciona con la mayoría de los escenarios porque la plantilla no debe incluir datos confidenciales. Si tiene que especificar datos confidenciales (por ejemplo, una contraseña de administrador), pase ese valor como un parámetro seguro. Sin embargo, si no desea que la plantilla sea accesible públicamente, puede almacenarla en un contenedor de almacenamiento privado para protegerla. Para más información sobre la implementación de una plantilla que requiere un token de Firma de acceso compartido (SAS), consulte [Implementación de una plantilla privada con el token de SAS](secure-template-with-sas-token.md). Para realizar un tutorial, consulte [Tutorial: Integración de Azure Key Vault en la implementación de la plantilla de Resource Manager](template-tutorial-use-key-vault.md).

## <a name="deploy-from-azure-cloud-shell"></a>Implementación desde Azure Cloud Shell

Puede usar [Azure Cloud Shell](https://shell.azure.com) para implementar la plantilla. Para implementar una plantilla externa, use el identificador URI de la plantilla. Para implementar una plantilla local, primero debe cargar la plantilla en la cuenta de almacenamiento de Cloud Shell. Para cargar archivos en el shell, seleccione el icono de menú **Cargar/Descargar archivos** en la ventana del shell.

Para abrir Cloud Shell, vaya a [https://shell.azure.com](https://shell.azure.com) o seleccione **Try-it** en la siguiente sección de código:

```azurepowershell-interactive
$resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"
$location = Read-Host -Prompt "Enter the location (i.e. centralus)"

New-AzResourceGroup -Name $resourceGroupName -Location $location
New-AzResourceGroupDeployment -ResourceGroupName $resourceGroupName `
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.json
```

Para pegar el código en el shell, haga clic dentro del shell y seleccione **Pegar**.

## <a name="pass-parameter-values"></a>Pase de valores de parámetros

Para pasar valores de parámetros, puede usar parámetros en línea o un archivo de parámetros.

### <a name="inline-parameters"></a>Parámetros en línea

Para pasar parámetros en línea, proporcione los nombres de parámetro con el comando `New-AzResourceGroupDeployment`. Por ejemplo, para pasar una cadena y una matriz a una plantilla, use:

```powershell
$arrayParam = "value1", "value2"
New-AzResourceGroupDeployment -ResourceGroupName testgroup `
  -TemplateFile c:\MyTemplates\demotemplate.json `
  -exampleString "inline string" `
  -exampleArray $arrayParam
```

También puede obtener el contenido del archivo y proporcionar ese contenido como un parámetro en línea.

```powershell
$arrayParam = "value1", "value2"
New-AzResourceGroupDeployment -ResourceGroupName testgroup `
  -TemplateFile c:\MyTemplates\demotemplate.json `
  -exampleString $(Get-Content -Path c:\MyTemplates\stringcontent.txt -Raw) `
  -exampleArray $arrayParam
```

Obtener un valor de parámetro de un archivo es útil cuando se necesita proporcionar valores de configuración. Por ejemplo, puede proporcionar [valores de cloud-init para una máquina virtual Linux](../../virtual-machines/linux/using-cloud-init.md).

Si necesita pasar una matriz de objetos, cree las tablas hash en PowerShell y agréguelas a una matriz. Pase esa matriz como un parámetro durante la implementación.

```powershell
$hash1 = @{ Name = "firstSubnet"; AddressPrefix = "10.0.0.0/24"}
$hash2 = @{ Name = "secondSubnet"; AddressPrefix = "10.0.1.0/24"}
$subnetArray = $hash1, $hash2
New-AzResourceGroupDeployment -ResourceGroupName testgroup `
  -TemplateFile c:\MyTemplates\demotemplate.json `
  -exampleArray $subnetArray
```

### <a name="parameter-files"></a>Archivos de parámetros

En lugar de pasar parámetros como valores en línea en el script, quizá le resulte más fácil usar un archivo JSON que contiene los valores de parámetro. El archivo de parámetros puede ser un archivo local o un archivo externo con un identificador URI accesible.

Para más información sobre el archivo de parámetro, consulte [Creación de un archivo de parámetros de Resource Manager](parameter-files.md).

Para pasar un archivo de parámetros local, use el parámetro **TemplateParameterFile**:

```powershell
New-AzResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup `
  -TemplateFile c:\MyTemplates\azuredeploy.json `
  -TemplateParameterFile c:\MyTemplates\storage.parameters.json
```

Para pasar un archivo de parámetros externo, use el parámetro **TemplateParameterUri**:

```powershell
New-AzResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup `
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.json `
  -TemplateParameterUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.parameters.json
```

## <a name="test-template-deployments"></a>Prueba de implementaciones de plantillas

Para probar los valores de parámetro y de plantilla sin implementar realmente ningún recurso, use [Test-AzResourceGroupDeployment](/powershell/module/az.resources/test-azresourcegroupdeployment). 

```powershell
Test-AzResourceGroupDeployment -ResourceGroupName ExampleResourceGroup `
  -TemplateFile c:\MyTemplates\azuredeploy.json -storageAccountType Standard_GRS
```

Si no se detectan errores, el comando finaliza sin una respuesta. Si se detecta un error, el comando devuelve un mensaje de error. Por ejemplo, si se pasa un valor incorrecto para la SKU de la cuenta de almacenamiento, se devuelve el error siguiente:

```powershell
Test-AzResourceGroupDeployment -ResourceGroupName testgroup `
  -TemplateFile c:\MyTemplates\azuredeploy.json -storageAccountType badSku

Code    : InvalidTemplate
Message : Deployment template validation failed: 'The provided value 'badSku' for the template parameter 'storageAccountType'
          at line '15' and column '24' is not valid. The parameter value is not part of the allowed value(s):
          'Standard_LRS,Standard_ZRS,Standard_GRS,Standard_RAGRS,Premium_LRS'.'.
Details :
```

Si la plantilla tiene un error de sintaxis, el comando devuelve un error que indica que la plantilla no se pudo analizar. El mensaje indica el número de línea y la posición del error de análisis.

```powershell
Test-AzResourceGroupDeployment : After parsing a value an unexpected character was encountered: 
  ". Path 'variables', line 31, position 3.
```

## <a name="next-steps"></a>Pasos siguientes

- Para revertir a una implementación correcta cuando se produce un error, consulte [Revertir en caso de error a una implementación correcta](rollback-on-error.md).
- Para especificar cómo controlar los recursos que existen en el grupo de recursos, pero que no están definidos en la plantilla, consulte [Modos de implementación de Azure Resource Manager](deployment-modes.md).
- Para entender cómo definir parámetros en la plantilla, consulte [Nociones sobre la estructura y la sintaxis de las plantillas de Azure Resource Manager](template-syntax.md).
- Para más información sobre la implementación de una plantilla que requiere un token de SAS, vea [Implementación de una plantilla privada con el token de SAS](secure-template-with-sas-token.md).
