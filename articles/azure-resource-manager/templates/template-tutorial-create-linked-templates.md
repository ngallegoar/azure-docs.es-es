---
title: Creación de plantillas vinculadas
description: Creación de plantillas vinculadas de Azure Resource Manager para crear máquinas virtuales
author: mumian
ms.date: 12/03/2019
ms.topic: tutorial
ms.author: jgao
ms.openlocfilehash: e1cce566fb7aab286c57f32d9348e51dd0a7c1ee
ms.sourcegitcommit: 253d4c7ab41e4eb11cd9995190cd5536fcec5a3c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/25/2020
ms.locfileid: "80239336"
---
# <a name="tutorial-create-linked-arm-templates"></a>Tutorial: Creación de plantillas de ARM vinculadas

Obtenga información sobre cómo crear plantillas vinculadas de Azure Resource Manager. Con las plantillas vinculadas, puede hacer que una plantilla llame a otra. Resulta muy útil para el diseño modular de las plantillas. En este tutorial, usará la misma plantilla que se usa en [Tutorial: Creación de plantillas de Azure Resource Manager con recursos dependientes](./template-tutorial-create-templates-with-dependent-resources.md), que crea una máquina virtual, una red virtual y otros recursos dependientes, incluida una cuenta de almacenamiento. Separe la creación de recursos de la cuenta de almacenamiento en una plantilla vinculada.

Llamar a una plantilla vinculada es como realizar una llamada de función.  También se aprende a pasar valores de parámetros a la plantilla vinculada y cómo obtener "valores devueltos" de ella.

En este tutorial se describen las tareas siguientes:

> [!div class="checklist"]
> * Apertura de una plantilla de inicio rápido
> * Creación de la plantilla vinculada
> * Carga de la plantilla vinculada
> * Vínculo a la plantilla vinculada
> * Configuración de la dependencia
> * Implementación de la plantilla
> * Procedimientos adicionales

Para más información, consulte [Uso de plantillas vinculadas y anidadas al implementar recursos de Azure](./linked-templates.md).

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Prerrequisitos

Para completar este artículo, necesitará lo siguiente:

* Visual Studio Code con la extensión Resource Manager Tools. Consulte [Uso de Visual Studio Code para la creación de plantillas de Resource Manager](use-vs-code-to-create-template.md).
* Para aumentar la seguridad, utilice una contraseña generada para la cuenta de administrador de máquina virtual. Este es un ejemplo para generar una contraseña:

    ```console
    openssl rand -base64 32
    ```

    Azure Key Vault está diseñado para proteger las claves criptográficas y otros secretos. Para más información, consulte el [Tutorial: Integración de Azure Key Vault en la implementación de la plantilla de Resource Manager](./template-tutorial-use-key-vault.md) También se recomienda actualizar la contraseña cada tres meses.

## <a name="open-a-quickstart-template"></a>Abra una plantilla de inicio rápido.

Plantillas de inicio rápido de Azure es un repositorio de plantillas de Azure Resource Manager. En lugar de crear una plantilla desde cero, puede buscar una plantilla de ejemplo y personalizarla. La plantilla que se usa en este tutorial se denomina [Deploy a simple Windows VM](https://azure.microsoft.com/resources/templates/101-vm-simple-windows/). Se trata de la misma plantilla que se usa en [Tutorial: Creación de plantillas de Resource Manager con recursos dependientes](./template-tutorial-create-templates-with-dependent-resources.md). Se guardan dos copias de la misma plantilla para usarlas de esta manera:

* **La plantilla principal**: cree todos los recursos, excepto la cuenta de almacenamiento.
* **La plantilla vinculada**: cree la cuenta de almacenamiento.

1. En Visual Studio Code, seleccione **Archivo**>**Abrir archivo**.
1. En **Nombre de archivo**, pegue el código URL siguiente:

    ```url
    https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-simple-windows/azuredeploy.json
    ```

1. Seleccione **Abrir** para abrir el archivo.
1. La plantilla define seis recursos:

   * [`Microsoft.Storage/storageAccounts`](https://docs.microsoft.com/azure/templates/Microsoft.Storage/storageAccounts)
   * [`Microsoft.Network/publicIPAddresses`](https://docs.microsoft.com/azure/templates/microsoft.network/publicipaddresses)
   * [`Microsoft.Network/networkSecurityGroups`](https://docs.microsoft.com/azure/templates/microsoft.network/networksecuritygroups)
   * [`Microsoft.Network/virtualNetworks`](https://docs.microsoft.com/azure/templates/microsoft.network/virtualnetworks)
   * [`Microsoft.Network/networkInterfaces`](https://docs.microsoft.com/azure/templates/microsoft.network/networkinterfaces)
   * [`Microsoft.Compute/virtualMachines`](https://docs.microsoft.com/azure/templates/microsoft.compute/virtualmachines)

     Resulta útil obtener cierta información básica del esquema de la plantilla antes de personalizar esta.
1. Seleccione **Archivo**>**Guardar como** para guardar una copia del archivo en la máquina local con el nombre **azuredeploy.json**.
1. Seleccione **Archivo**>**Guardar como** para crear otra copia del archivo con el nombre **linkedTemplate.json**.

## <a name="create-the-linked-template"></a>Creación de la plantilla vinculada

La plantilla vinculada crea una cuenta de almacenamiento. La plantilla vinculada se puede usar como plantilla independiente para crear una cuenta de almacenamiento. En este tutorial, la plantilla vinculada toma dos parámetros y pasa un valor de vuelta a la plantilla principal. Este valor "devuelto" se define en el elemento `outputs`.

1. Abra el archivo **linkedTemplate.json** en Visual Studio Code si no está abierto.
1. Se han realizado los siguientes cambios:

    * Quite todos los parámetros excepto **location**.
    * Agregue un parámetro llamado **storageAccountName**.

      ```json
      "storageAccountName":{
        "type": "string",
        "metadata": {
            "description": "Azure Storage account name."
        }
      },
      ```

      El nombre y la ubicación de la cuenta de almacenamiento se pasan como parámetros de la plantilla principal a la plantilla vinculada.

    * Quite el elemento **variables** y todas las definiciones de variable.
    * Quite todos los recursos, excepto la cuenta de almacenamiento. Puede quitar un total de cuatro recursos.
    * Actualice el valor del elemento **name** del recurso de la cuenta de almacenamiento con:

        ```json
          "name": "[parameters('storageAccountName')]",
        ```

    * Actualice el elemento **outputs** para que tenga este aspecto:

        ```json
        "outputs": {
          "storageUri": {
              "type": "string",
              "value": "[reference(parameters('storageAccountName')).primaryEndpoints.blob]"
            }
        }
        ```

       La definición del recurso de máquina virtual necesita **storageUri** en la plantilla principal.  El valor se pasa de vuelta a la plantilla principal como un valor de salida.

        Cuando termine, la plantilla deberá verse así:

        ```json
        {
          "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
          "contentVersion": "1.0.0.0",
          "parameters": {
            "storageAccountName": {
              "type": "string",
              "metadata": {
                "description": "Azure Storage account name."
              }
            },
            "location": {
              "type": "string",
              "defaultValue": "[resourceGroup().location]",
              "metadata": {
                "description": "Location for all resources."
              }
            }
          },
          "resources": [
            {
              "type": "Microsoft.Storage/storageAccounts",
              "apiVersion": "2018-11-01",
              "name": "[parameters('storageAccountName')]",
              "location": "[parameters('location')]",
              "sku": {
                "name": "Standard_LRS"
              },
              "kind": "Storage",
              "properties": {}
            }
          ],
          "outputs": {
            "storageUri": {
              "type": "string",
              "value": "[reference(parameters('storageAccountName')).primaryEndpoints.blob]"
            }
          }
        }
        ```

1. Guarde los cambios.

## <a name="upload-the-linked-template"></a>Carga de la plantilla vinculada

La plantilla principal y la plantilla vinculada deben ser accesibles desde donde se ejecuta la implementación. En este tutorial, use el método de implementación de Cloud Shell que utilizó en [Tutorial: Creación de plantillas de Resource Manager con recursos dependientes](./template-tutorial-create-templates-with-dependent-resources.md). La plantilla principal (azuredeploy.json) se carga en el shell. La plantilla vinculada (linkedTemplate.json) se debe compartir en algún lugar seguro. El siguiente script de PowerShell crea una cuenta de Azure Storage, carga la plantilla en dicha cuenta y genera un token de SAS para conceder acceso limitado al archivo de plantilla. Para simplificar el tutorial, el script descarga una plantilla vinculada completada de un repositorio de GitHub. Si desea usar la plantilla vinculada que creó, puede usar la instancia de [Cloud Shell](https://shell.azure.com) para cargar la plantilla vinculada y luego modificar el script para utilizar su propia plantilla vinculada.

> [!NOTE]
> El script limita el uso del token SAS a un plazo de ocho horas. Si necesita más tiempo para completar este tutorial, aumente la hora de expiración.

```azurepowershell-interactive
$projectNamePrefix = Read-Host -Prompt "Enter a project name:"   # This name is used to generate names for Azure resources, such as storage account name.
$location = Read-Host -Prompt "Enter a location (i.e. centralus)"

$resourceGroupName = $projectNamePrefix + "rg"
$storageAccountName = $projectNamePrefix + "store"
$containerName = "linkedtemplates" # The name of the Blob container to be created.

$linkedTemplateURL = "https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/tutorial-linked-templates/linkedStorageAccount.json" # A completed linked template used in this tutorial.
$fileName = "linkedStorageAccount.json" # A file name used for downloading and uploading the linked template.

# Download the tutorial linked template
Invoke-WebRequest -Uri $linkedTemplateURL -OutFile "$home/$fileName"

# Create a resource group
New-AzResourceGroup -Name $resourceGroupName -Location $location

# Create a storage account
$storageAccount = New-AzStorageAccount `
    -ResourceGroupName $resourceGroupName `
    -Name $storageAccountName `
    -Location $location `
    -SkuName "Standard_LRS"

$context = $storageAccount.Context

# Create a container
New-AzStorageContainer -Name $containerName -Context $context

# Upload the linked template
Set-AzStorageBlobContent `
    -Container $containerName `
    -File "$home/$fileName" `
    -Blob $fileName `
    -Context $context

# Generate a SAS token
$templateURI = New-AzStorageBlobSASToken `
    -Context $context `
    -Container $containerName `
    -Blob $fileName `
    -Permission r `
    -ExpiryTime (Get-Date).AddHours(8.0) `
    -FullUri

Write-Host "You need the following values later in the tutorial:"
Write-Host "Resource Group Name: $resourceGroupName"
Write-Host "Linked template URI with SAS token: $templateURI"
Write-Host "Press [ENTER] to continue ..."
```

1. Seleccione el botón verde **Probarlo** para abrir el panel de Azure Cloud Shell.
2. Seleccione **Copiar** para copiar el script de PowerShell.
3. Haga clic en cualquier lugar del panel de Shell (la parte de color azul marino) y luego seleccione **Pegar**.
4. Tome nota de los dos valores (Nombre del grupo de recursos y Linked template URI [Uri de la plantilla vinculada]) al final del panel de Shell. Necesitará estos valores más adelante en el tutorial.
5. Seleccione **Salir del modo de enfoque** para cerrar el panel de Shell.

En la práctica, se genera un token de SAS cuando se implementa la plantilla principal y se asigna a la expiración del token de SAS una ventana más pequeña para que sea más seguro. Para más información, consulte [Provisión del token de SAS durante la implementación](./secure-template-with-sas-token.md#provide-sas-token-during-deployment).

## <a name="call-the-linked-template"></a>Llamada a la plantilla vinculada

La plantilla principal se llama azuredeploy.json.

1. Abra **azuredeploy.json** en Visual Studio Code en caso de que no esté abierto.
1. Reemplace la definición de recursos de la cuenta de almacenamiento por el siguiente fragmento de código JSON:

    ```json
    {
      "name": "linkedTemplate",
      "type": "Microsoft.Resources/deployments",
      "apiVersion": "2018-05-01",
      "properties": {
          "mode": "Incremental",
          "templateLink": {
              "uri":""
          },
          "parameters": {
              "storageAccountName":{"value": "[variables('storageAccountName')]"},
              "location":{"value": "[parameters('location')]"}
          }
      }
    },
    ```

    Ponga atención a estos detalles:

    * Un recurso `Microsoft.Resources/deployments` en la plantilla principal se usa para vincular a otra plantilla.
    * El recurso `deployments` tiene un nombre llamado `linkedTemplate`. Este nombre de usa para [configurar la dependencia](#configure-dependency).
    * Solo puede usar el modo de implementación [incremental](./deployment-modes.md) al llamar a las plantillas vinculadas.
    * `templateLink/uri` contiene el URI de la plantilla vinculada. Actualice el valor con el URI que obtiene al cargar la plantilla vinculada (la que tiene un token de SAS).
    * Use `parameters` para pasar valores desde la plantilla principal a la plantilla vinculada.
1. Asegúrese de que ha actualizado el valor del elemento `uri` con el valor que obtuvo al cargar la plantilla vinculada (la que tiene un token de SAS). En la práctica, se quiere proporcionar un parámetro al URI.
1. Guarde la plantilla modificada.

## <a name="configure-dependency"></a>Configuración de la dependencia

Recuerde que en [Tutorial: Creación de plantillas de Azure Resource Manager con recursos dependientes](./template-tutorial-create-templates-with-dependent-resources.md), el recurso de máquina virtual depende de la cuenta de almacenamiento:

![Diagrama de dependencia de las plantillas de Azure Resource Manager](./media/template-tutorial-create-linked-templates/resource-manager-template-visual-studio-code-dependency-diagram.png)

Como la cuenta de almacenamiento ahora se define en la plantilla vinculada, debe actualizar estos dos elementos del recurso `Microsoft.Compute/virtualMachines`.

* Vuelva a configurar el elemento `dependsOn`. La definición de la cuenta de almacenamiento se traslada a la plantilla vinculada.
* Vuelva a configurar el elemento `properties/diagnosticsProfile/bootDiagnostics/storageUri`. En [Creación de la plantilla vinculada](#create-the-linked-template), agregó un valor de salida:

    ```json
    "outputs": {
        "storageUri": {
            "type": "string",
            "value": "[reference(parameters('storageAccountName')).primaryEndpoints.blob]"
            }
    }
    ```

    La plantilla principal requiere este valor.

1. Abra azuredeploy.json en Visual Studio Code en caso de que no esté abierto.
2. Expanda la definición del recurso de máquina virtual, actualice **dependsOn** como se muestra en esta captura de pantalla:

    ![Las plantillas vinculadas de Azure Resource Manager configuran la dependencia](./media/template-tutorial-create-linked-templates/resource-manager-template-linked-templates-configure-dependency.png)

    *linkedTemplate* es el nombre del recurso de implementaciones.
3. Actualice **properties/diagnosticsProfile/bootDiagnostics/storageUri** tal y como se muestra en la captura de pantalla anterior.
4. Guarde la plantilla modificada.

## <a name="deploy-the-template"></a>Implementación de la plantilla

Consulte la sección [Implementación de la plantilla](./template-tutorial-create-templates-with-dependent-resources.md#deploy-the-template) para conocer el procedimiento de implementación. Use el mismo nombre del grupo de recursos que usa en la cuenta de almacenamiento para almacenar la plantilla vinculada. Simplifica la limpieza de recursos en la sección siguiente. Para aumentar la seguridad, utilice una contraseña generada para la cuenta de administrador de máquina virtual. Consulte [Requisitos previos](#prerequisites).

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando los recursos de Azure ya no sean necesarios, limpie los recursos que implementó eliminando el grupo de recursos.

1. En Azure Portal, seleccione **Grupos de recursos** en el menú de la izquierda.
2. Escriba el nombre del grupo de recursos en el campo **Filtrar por nombre**.
3. Seleccione el nombre del grupo de recursos.  Verá un total de seis recursos en el grupo de recursos.
4. Seleccione **Eliminar grupo de recursos** del menú superior.

## <a name="additional-practice"></a>Procedimiento adicional

Para mejorar el proyecto, efectúe los siguientes cambios adicionales en el proyecto completado:

1. Modifique la plantilla principal (azuredeploy.json) para que tome el valor del URI de la plantilla vinculada a través de un parámetro.
2. En lugar de generar un token de SAS al cargar la plantilla vinculada, genere el token al implementar la plantilla principal. Para más información, consulte [Provisión del token de SAS durante la implementación](./secure-template-with-sas-token.md#provide-sas-token-during-deployment).

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, ha dividido una plantilla en una plantilla principal y una plantilla vinculada. Para aprender a usar extensiones de máquina virtual para realizar tareas posteriores a la implementación, consulte:

> [!div class="nextstepaction"]
> [Implementación de extensiones de máquina virtual](./template-tutorial-deploy-vm-extensions.md)
