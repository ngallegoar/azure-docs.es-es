---
title: Creación de un área de trabajo con una plantilla de Azure Resource Manager
titleSuffix: Azure Machine Learning
description: Aprenda a usar una plantilla de Azure Resource Manager para crear una nueva área de trabajo de Azure Machine Learning.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: larryfr
author: Blackmist
ms.date: 05/19/2020
ms.custom: seoapril2019
ms.openlocfilehash: eae10b7ae8cd14fd120e969c39c05a8ba2525003
ms.sourcegitcommit: 50673ecc5bf8b443491b763b5f287dde046fdd31
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/20/2020
ms.locfileid: "83681551"
---
# <a name="use-an-azure-resource-manager-template-to-create-a-workspace-for-azure-machine-learning"></a>Uso de una plantilla de Azure Resource Manager para crear un área de trabajo para Azure Machine Learning

[!INCLUDE [aml-applies-to-basic-enterprise-sku](../../includes/aml-applies-to-basic-enterprise-sku.md)]
<br>

En este artículo aprenderá varias formas de crear un área de trabajo de Azure Machine Learning mediante plantillas de Azure Resource Manager. Una plantilla de Resource Manager facilita la creación de recursos en una única operación coordinada. Una plantilla es un documento JSON que define los recursos que son necesarios para una implementación. También puede especificar los parámetros de implementación. Los parámetros se emplean para proporcionar valores de entrada cuando se usa la plantilla.

Para obtener más información, consulte [Implementación de una aplicación con la plantilla de Azure Resource Manager](../azure-resource-manager/templates/deploy-powershell.md).

## <a name="prerequisites"></a>Prerrequisitos

* Una **suscripción de Azure**. Si no tiene una ya, pruebe la [versión gratuita o de pago de Azure Machine Learning](https://aka.ms/AMLFree).

* Para usar una plantilla desde una CLI, necesita [Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview?view=azps-1.2.0) o la [CLI de Azure](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest).

## <a name="resource-manager-template"></a>Plantilla de Resource Manager

La siguiente plantilla de Resource Manager se puede usar para crear un área de trabajo de Azure Machine Learning y los recursos de Azure asociados:

[!code-json[create-azure-machine-learning-service-workspace](~/quickstart-templates/101-machine-learning-create/azuredeploy.json)]

Esta plantilla crea los siguientes servicios de Azure:

* Grupo de recursos de Azure
* Cuenta de Azure Storage
* Azure Key Vault
* Azure Application Insights
* Azure Container Registry
* Área de trabajo de Azure Machine Learning

El grupo de recursos es el contenedor que contiene los servicios. El área de trabajo de Azure Machine Learning requiere los diversos servicios.

La plantilla de ejemplo tiene dos parámetros:

* La **ubicación** en la que se creará el grupo de recursos y de servicios.

    La plantilla utilizará la ubicación que seleccione para la mayoría de los recursos. La excepción es el servicio Application Insights que no está disponible en todas las ubicaciones en las que están los demás servicios. Si selecciona una ubicación donde no está disponible, el servicio se creará en la ubicación Centro-sur de EE. UU.

* El **nombre del área de trabajo**, que es el nombre descriptivo del área de trabajo de Azure Machine Learning.

    > [!NOTE]
    > El nombre del área de trabajo no distingue mayúsculas de minúsculas.

    Los nombres de los demás servicios se generan aleatoriamente.

> [!TIP]
> Aunque la plantilla asociada a este documento crea un nuevo registro de contenedor de Azure, también puede crear una nueva área de trabajo sin necesidad de generar un registro de contenedor. Se creará uno cuando se realice una operación que lo requiera. Por ejemplo, al entrenar o implementar un modelo.
>
> También se puede hacer referencia a un registro de contenedor o una cuenta de almacenamiento existentes en la plantilla de Azure Resource Manager, en lugar de crear otros nuevos.

[!INCLUDE [machine-learning-delete-acr](../../includes/machine-learning-delete-acr.md)]

Para más información sobre las plantillas, consulte los siguientes artículos:

* [Creación de plantillas del Administrador de recursos de Azure](../azure-resource-manager/templates/template-syntax.md)
* [Implementación de una aplicación con las plantillas de Azure Resource Manager](../azure-resource-manager/templates/deploy-powershell.md)
* [Tipos de recurso de Microsoft.MachineLearningServices](https://docs.microsoft.com/azure/templates/microsoft.machinelearningservices/allversions)

### <a name="advanced-template"></a>Plantilla avanzada

En la plantilla de ejemplo siguiente se muestra cómo crear un área de trabajo con tres configuraciones:

* Habilitar la configuración de alta confidencialidad para el área de trabajo
* Habilitar el cifrado para el área de trabajo
* Usar una instancia de Azure Key Vault existente para recuperar las claves administradas por el cliente

Para obtener más información, consulte [Cifrado en reposo](concept-enterprise-security.md#encryption-at-rest).

> [!IMPORTANT]
> Hay algunos requisitos específicos que la suscripción debe cumplir antes de usar esta plantilla:
> * La aplicación __Azure Machine Learning__ debe ser un __colaborador__ para su suscripción de Azure.
> * Debe tener un almacén Azure Key Vault existente que contenga una clave de cifrado.
> * Debe tener una directiva de acceso en el almacén Azure Key Vault que conceda el acceso __obtener__, __ajustar__ y __desajustar__ a la aplicación __Azure Cosmos DB__.
> * El almacén Azure Key Vault debe estar en la misma región en la que se planea crear el área de trabajo de Azure Machine Learning.

__Para agregar la aplicación de Azure Machine Learning como un colaborador__, use los comandos siguientes:

1. Para autenticarse en Azure desde la CLI, use el comando siguiente:

    ```azurecli-interactive
    az login
    ```
    
    [!INCLUDE [subscription-login](../../includes/machine-learning-cli-subscription.md)]

1. Para obtener el id. de objeto de la aplicación de Azure Machine Learning, use el comando siguiente. El valor puede ser distinto para cada una de las suscripciones de Azure:

    ```azurecli-interactive
    az ad sp list --display-name "Azure Machine Learning" --query '[].[appDisplayName,objectId]' --output tsv
    ```

    Este comando devuelve el id. de objeto, que es un GUID.

1. Para agregar el id. de objeto como colaborador a la suscripción, use el comando siguiente. Reemplace `<object-ID>` por el GUID del paso anterior. Reemplace `<subscription-ID>` por el nombre o id. de la suscripción de Azure:

    ```azurecli-interactive
    az role assignment create --role 'Contributor' --assignee-object-id <object-ID> --subscription <subscription-ID>
    ```

__Para agregar una clave al almacén Azure Key Vault__, use la información de la sección [Agregar una clave, un secreto o un certificado al almacén de claves](../key-vault/general/manage-with-cli2.md#adding-a-key-secret-or-certificate-to-the-key-vault) del artículo __Administración de Key Vault mediante la CLI de Azure__.

__Para agregar una directiva de acceso al almacén de claves, use los comandos siguientes__:

1. Para obtener el id. de objeto de la aplicación de Azure Cosmos DB, use el comando siguiente. El valor puede ser distinto para cada una de las suscripciones de Azure:

    ```azurecli-interactive
    az ad sp list --display-name "Azure Cosmos DB" --query '[].[appDisplayName,objectId]' --output tsv
    ```
    
    Este comando devuelve el id. de objeto, que es un GUID.

1. Para establecer una directiva, use el comando siguiente. Reemplace `<keyvault-name>` por el nombre del almacén Azure Key Vault existente. Reemplace `<object-ID>` por el GUID del paso anterior:

    ```azurecli-interactive
    az keyvault set-policy --name <keyvault-name> --object-id <object-ID> --key-permissions get unwrapKey wrapKey
    ```

__A fin de obtener los valores__ para `cmk_keyvault` (id. de Key Vault) y los parámetros de `resource_cmk_uri` (URI de clave) necesarios para esta plantilla, siga los pasos siguientes:

1. Para obtener el id. de Key Vault, use el comando siguiente:

    ```azurecli-interactive
    az keyvault show --name mykeyvault --resource-group myresourcegroup --query "id"
    ```

    Este comando devuelve un valor similar a `/subscriptions/{subscription-guid}/resourceGroups/myresourcegroup/providers/Microsoft.KeyVault/vaults/mykeyvault`.

1. Para obtener el valor del URI de la clave administrada por el cliente, use el comando siguiente:

    ```azurecli-interactive
    az keyvault key show --vault-name mykeyvault --name mykey --query "key.kid"
    ```

    Este comando devuelve un valor similar a `https://mykeyvault.vault.azure.net/keys/mykey/{guid}`.

__Plantilla de ejemplo__

:::code language="json" source="~/quickstart-templates/201-machine-learning-encrypted-workspace/azuredeploy.json":::

> [!IMPORTANT]
> Una vez que se ha creado un área de trabajo, no se puede cambiar la configuración de datos confidenciales, cifrado, identificador de almacén de claves o identificadores de claves. Para cambiar estos valores, debe crear una nueva área de trabajo con los nuevos valores.

## <a name="use-the-azure-portal"></a>Uso de Azure Portal

1. Siga los pasos que se indican en [Implementación de recursos desde plantilla personalizada](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-template-deploy-portal#deploy-resources-from-custom-template). Cuando llegue a la pantalla __Editar plantilla__, pegue la plantilla desde este documento.
1. Seleccione __Guardar__ para usar la plantilla. Proporcione la siguiente información y acepte los términos y condiciones que aparecen:

   * Suscripción: Seleccione la suscripción de Azure que va a usar para estos recursos.
   * Grupo de recursos: Seleccione o cree un grupo de recursos que contenga los servicios.
   * Nombre del área de trabajo: El nombre que se usará para el área de trabajo de Azure Machine Learning que se va a crear. El nombre del área de trabajo debe tener entre 3 y 33 caracteres. Solo puede contener caracteres alfanuméricos y "-".
   * Ubicación: Seleccione la ubicación en la que se crearán los recursos.

Para más información, vea [Implementación de recursos desde plantilla personalizada](../azure-resource-manager/templates/deploy-portal.md#deploy-resources-from-custom-template).

## <a name="use-azure-powershell"></a>Uso de Azure PowerShell

En este ejemplo se da por supuesto que ha guardado la plantilla en un archivo denominado `azuredeploy.json` en el directorio actual:

```powershell
New-AzResourceGroup -Name examplegroup -Location "East US"
new-azresourcegroupdeployment -name exampledeployment `
  -resourcegroupname examplegroup -location "East US" `
  -templatefile .\azuredeploy.json -workspaceName "exampleworkspace" -sku "basic"
```

Para más información, vea [Implementación de recursos con las plantillas de Resource Manager y Azure PowerShell](../azure-resource-manager/templates/deploy-powershell.md) e [Implementar la plantilla de Resource Manager privada con el token de SAS y Azure PowerShell](../azure-resource-manager/templates/secure-template-with-sas-token.md).

## <a name="use-the-azure-cli"></a>Uso de la CLI de Azure

En este ejemplo se da por supuesto que ha guardado la plantilla en un archivo denominado `azuredeploy.json` en el directorio actual:

```azurecli-interactive
az group create --name examplegroup --location "East US"
az group deployment create \
  --name exampledeployment \
  --resource-group examplegroup \
  --template-file azuredeploy.json \
  --parameters workspaceName=exampleworkspace location=eastus sku=basic
```

Para más información, vea [Implementación de recursos con plantillas de Resource Manager y la CLI de Azure](../azure-resource-manager/templates/deploy-cli.md) e [Implementar la plantilla de Resource Manager privada con el token de SAS y la CLI de Azure](../azure-resource-manager/templates/secure-template-with-sas-token.md).

## <a name="troubleshooting"></a>Solución de problemas

### <a name="resource-provider-errors"></a>Errores del proveedor de recursos

[!INCLUDE [machine-learning-resource-provider](../../includes/machine-learning-resource-provider.md)]

### <a name="azure-key-vault-access-policy-and-azure-resource-manager-templates"></a>Directiva de acceso de Azure Key Vault y plantillas de Azure Resource Manager

Una plantilla de Azure Resource Manager se usa para crear el área de trabajo y los recursos asociados (incluido Azure Key Vault) varias veces. Por ejemplo, con el uso de la plantilla varias veces con los mismos parámetros como parte de una canalización de implementación e integración continuas.

La mayoría de las operaciones de creación de recursos mediante plantillas son idempotentes, pero Key Vault borra las directivas de acceso cada vez que se usa la plantilla. Al borrar las directivas de acceso, se interrumpe el acceso al almacén de claves para las áreas de trabajo existentes que lo estén usando. Por ejemplo, las funcionalidades de detección y creación de Azure Notebooks pueden producir un error.  

Para evitar este problema, se recomienda uno de los siguientes enfoques:

* No implemente la plantilla de más de una vez con los mismos parámetros. O bien, elimine los recursos existentes antes de usar la plantilla para volver a crearlos.

* Examine las directivas de acceso de Key Vault y, luego, use estas directivas para establecer la propiedad `accessPolicies` de la plantilla. Para ver las directivas de acceso, use el siguiente comando de CLI de Azure:

    ```azurecli-interactive
    az keyvault show --name mykeyvault --resource-group myresourcegroup --query properties.accessPolicies
    ```

    Para más información sobre el uso de la `accessPolicies` sección de la plantilla, consulte la referencia del objeto [AccessPolicyEntry](https://docs.microsoft.com/azure/templates/Microsoft.KeyVault/2018-02-14/vaults#AccessPolicyEntry).

* Compruebe si ya existe el recurso de Key Vault. Si es así, no lo vuelva a crear con la plantilla. Por ejemplo, para usar el Key Vault existente en lugar de crear uno nuevo, realice los siguientes cambios en la plantilla:

    * **Agregue** un parámetro que acepte el identificador de un recurso de Key Vault existente:

        ```json
        "keyVaultId":{
          "type": "string",
          "metadata": {
            "description": "Specify the existing Key Vault ID."
          }
        }
      ```

    * **Elimine** la sección que crea un recurso de Key Vault:

        ```json
        {
          "type": "Microsoft.KeyVault/vaults",
          "apiVersion": "2018-02-14",
          "name": "[variables('keyVaultName')]",
          "location": "[parameters('location')]",
          "properties": {
            "tenantId": "[variables('tenantId')]",
            "sku": {
              "name": "standard",
              "family": "A"
            },
            "accessPolicies": [
            ]
          }
        },
        ```

    * **Elimine** la línea de la `"[resourceId('Microsoft.KeyVault/vaults', variables('keyVaultName'))]",` sección `dependsOn` del área de trabajo. También **cambie** la `keyVault` entrada en la sección `properties` del área de trabajo para hacer referencia al `keyVaultId` parámetro:

        ```json
        {
          "type": "Microsoft.MachineLearningServices/workspaces",
          "apiVersion": "2019-11-01",
          "name": "[parameters('workspaceName')]",
          "location": "[parameters('location')]",
          "dependsOn": [
            "[resourceId('Microsoft.Storage/storageAccounts', variables('storageAccountName'))]",
            "[resourceId('Microsoft.Insights/components', variables('applicationInsightsName'))]"
          ],
          "identity": {
            "type": "systemAssigned"
          },
          "sku": {
            "tier": "[parameters('sku')]",
            "name": "[parameters('sku')]"
          },
          "properties": {
            "friendlyName": "[parameters('workspaceName')]",
            "keyVault": "[parameters('keyVaultId')]",
            "applicationInsights": "[resourceId('Microsoft.Insights/components',variables('applicationInsightsName'))]",
            "storageAccount": "[resourceId('Microsoft.Storage/storageAccounts/',variables('storageAccountName'))]"
          }
        }
        ```

    Después de estos cambios, puede especificar el identificador del recurso de Key Vault existente al ejecutar la plantilla. La plantilla establecerá entonces la propiedad `keyVault` del área de trabajo en su identificador para reutilizar el almacén de claves.

    Para obtener el identificador de la Key Vault, puede hacer referencia a la salida de la ejecución de la plantilla original o usar el CLI de Azure. El siguiente comando es un ejemplo de uso de la CLI de Azure para obtener el identificador de recurso de Key Vault:

    ```azurecli-interactive
    az keyvault show --name mykeyvault --resource-group myresourcegroup --query id
    ```

    Este comando devuelve un valor similar al siguiente texto:

    ```text
    /subscriptions/{subscription-guid}/resourceGroups/myresourcegroup/providers/Microsoft.KeyVault/vaults/mykeyvault
    ```

## <a name="next-steps"></a>Pasos siguientes

* [Implementación de recursos con las plantillas de Resource Manager y la API REST de Resource Manager](../azure-resource-manager/templates/deploy-rest.md).
* [Creación e implementación de grupos de recursos de Azure mediante Visual Studio](../azure-resource-manager/templates/create-visual-studio-deployment-project.md).
