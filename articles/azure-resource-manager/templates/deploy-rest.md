---
title: Implementación de recursos con la API de REST y plantilla
description: Use Azure Resource Manager y la API REST de Resource Manager para implementar recursos en Azure. Los recursos se definen en una plantilla de Resource Manager.
ms.topic: conceptual
ms.date: 05/20/2020
ms.openlocfilehash: d7865ac6f9b2bb176ea5308e326dec0741a80962
ms.sourcegitcommit: 6fd8dbeee587fd7633571dfea46424f3c7e65169
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/21/2020
ms.locfileid: "83723126"
---
# <a name="deploy-resources-with-arm-templates-and-resource-manager-rest-api"></a>Implementación de recursos con las plantillas de Azure Resource Manager y la API REST de Resource Manager

En este artículo se explica cómo usar la API REST de Resource Manager con plantillas de Azure Resource Manager (ARM) para implementar sus recursos en Azure.

Puede incluir la plantilla en el cuerpo de solicitud o vincularla a un archivo. Al usar un archivo, este puede ser un archivo local o un archivo externo disponible a través de un identificador URI. Cuando la plantilla se encuentra en una cuenta de almacenamiento, puede restringir el acceso a ella y proporcionar un token de firma de acceso compartido (SAS) durante la implementación.

## <a name="deployment-scope"></a>Ámbito de la implementación

La implementación puede tener como destino un grupo de administración, una suscripción de Azure o un grupo de recursos. En la mayoría de los casos, la implementación tendrá como destino un grupo de recursos. Use las implementaciones en un grupo de recursos o en una suscripción para aplicar directivas y asignaciones de roles en el ámbito especificado. También puede usar las implementaciones de la suscripción para crear un grupo de recursos e implementar recursos en él. Según el ámbito de la implementación, usará comandos diferentes.

* Para implementar en un **grupo de recursos**, use [Deployments - Create](/rest/api/resources/deployments/createorupdate). La solicitud se envía a:

  ```HTTP
  PUT https://management.azure.com/subscriptions/{subscriptionId}/resourcegroups/{resourceGroupName}/providers/Microsoft.Resources/deployments/{deploymentName}?api-version=2019-10-01
  ```

* Para implementar en una **suscripción**, use [Deployments - Create At Subscription Scope](/rest/api/resources/deployments/createorupdateatsubscriptionscope). La solicitud se envía a:

  ```HTTP
  PUT https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.Resources/deployments/{deploymentName}?api-version=2019-10-01
  ```

  Para más información sobre las implementaciones en el nivel de suscripción, consulte [Creación de grupos de recursos y otros recursos en el nivel de suscripción](deploy-to-subscription.md).

* Para implementar en un **grupo de administración**, use [Deployments - Create At Management Group Scope](/rest/api/resources/deployments/createorupdateatmanagementgroupscope). La solicitud se envía a:

  ```HTTP
  PUT https://management.azure.com/providers/Microsoft.Management/managementGroups/{groupId}/providers/Microsoft.Resources/deployments/{deploymentName}?api-version=2019-10-01
  ```

  Para obtener más información sobre las implementaciones de nivel de grupo de administración, consulte [Creación de recursos en el nivel de grupo de administración](deploy-to-management-group.md).

* Para implementar en un **inquilino**, use [Implementaciones: Crear o actualizar en el ámbito del inquilino](/rest/api/resources/deployments/createorupdateattenantscope). La solicitud se envía a:

  ```HTTP
  PUT https://management.azure.com/providers/Microsoft.Resources/deployments/{deploymentName}?api-version=2019-10-01
  ```

  Para más información sobre las implementaciones a nivel de inquilino, consulte [Creación de recursos en el nivel de inquilino](deploy-to-tenant.md).

Los ejemplos de este artículo usan las implementaciones del grupo de recursos.

## <a name="deploy-with-the-rest-api"></a>Implementación con la API de REST

1. Establezca los [encabezados y parámetros comunes](/rest/api/azure/), incluidos los tokens de autenticación.

1. Si no tiene un grupo de recursos existente, puede crear uno. Especifique el identificador de la suscripción, el nombre del nuevo grupo de recursos y la ubicación que necesita para la solución. Para obtener más información, consulte [Crear un grupo de recursos](/rest/api/resources/resourcegroups/createorupdate).

   ```HTTP
   PUT https://management.azure.com/subscriptions/<YourSubscriptionId>/resourcegroups/<YourResourceGroupName>?api-version=2019-10-01
   ```

   Con un cuerpo de la solicitud como:

   ```json
   {
    "location": "West US",
    "tags": {
      "tagname1": "tagvalue1"
    }
   }
   ```

1. Valide la implementación antes de ejecutarla. Para ello, ejecute la operación de [validación de una implementación de plantilla](/rest/api/resources/deployments/validate). Al probar la implementación, proporcione los parámetros exactamente como lo haría al ejecutar la implementación (como se muestra en el paso siguiente).

1. Para implementar una plantilla, especifique el identificador de suscripción, el nombre del grupo de recursos y el nombre de la implementación en el URI de solicitud.

   ```HTTP
   PUT https://management.azure.com/subscriptions/<YourSubscriptionId>/resourcegroups/<YourResourceGroupName>/providers/Microsoft.Resources/deployments/<YourDeploymentName>?api-version=2019-10-01
   ```

   En el cuerpo de solicitud, proporcione un vínculo al archivo de plantilla y parámetros. Para más información sobre el archivo de parámetro, consulte [Creación de un archivo de parámetros de Resource Manager](parameter-files.md).

   Observe que el **modo** se establece en **Incremental**. Para ejecutar una implementación completa, establezca el **modo** en **Completo**. Tenga cuidado al usar este modo, ya que puede eliminar accidentalmente los recursos que no estén en la plantilla.

   ```json
   {
    "properties": {
      "templateLink": {
        "uri": "http://mystorageaccount.blob.core.windows.net/templates/template.json",
        "contentVersion": "1.0.0.0"
      },
      "parametersLink": {
        "uri": "http://mystorageaccount.blob.core.windows.net/templates/parameters.json",
        "contentVersion": "1.0.0.0"
      },
      "mode": "Incremental"
    }
   }
   ```

    Si desea registrar el contenido de la respuesta y el de la solicitud, o ambos, incluya **debugSetting** en la solicitud.

   ```json
   {
    "properties": {
      "templateLink": {
        "uri": "http://mystorageaccount.blob.core.windows.net/templates/template.json",
        "contentVersion": "1.0.0.0"
      },
      "parametersLink": {
        "uri": "http://mystorageaccount.blob.core.windows.net/templates/parameters.json",
        "contentVersion": "1.0.0.0"
      },
      "mode": "Incremental",
      "debugSetting": {
        "detailLevel": "requestContent, responseContent"
      }
    }
   }
   ```

    Puede configurar la cuenta de almacenamiento para utilizar un token de firma de acceso compartido (SAS). Para obtener más información, consulte [Delegating Access with a Shared Access Signature](/rest/api/storageservices/delegating-access-with-a-shared-access-signature)(Delegación del acceso con una firma de acceso compartido).

    Si necesita proporcionar un valor confidencial para un parámetro (por ejemplo, una contraseña), agregue ese valor a un almacén de claves. Recupere el almacén de claves durante la implementación, como se muestra en el ejemplo anterior. Para más información, consulte [Paso de valores seguros durante la implementación](key-vault-parameter.md).

1. En lugar de crear vínculos a archivos para la plantilla y los parámetros, puede incluirlos en el cuerpo de la solicitud. El ejemplo siguiente muestra el cuerpo de la solicitud con la plantilla y el parámetro en línea:

   ```json
   {
      "properties": {
      "mode": "Incremental",
      "template": {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
          "storageAccountType": {
            "type": "string",
            "defaultValue": "Standard_LRS",
            "allowedValues": [
              "Standard_LRS",
              "Standard_GRS",
              "Standard_ZRS",
              "Premium_LRS"
            ],
            "metadata": {
              "description": "Storage Account type"
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
        "variables": {
          "storageAccountName": "[concat(uniquestring(resourceGroup().id), 'standardsa')]"
        },
        "resources": [
          {
            "type": "Microsoft.Storage/storageAccounts",
            "apiVersion": "2018-02-01",
            "name": "[variables('storageAccountName')]",
            "location": "[parameters('location')]",
            "sku": {
              "name": "[parameters('storageAccountType')]"
            },
            "kind": "StorageV2",
            "properties": {}
          }
        ],
        "outputs": {
          "storageAccountName": {
            "type": "string",
            "value": "[variables('storageAccountName')]"
          }
        }
      },
      "parameters": {
        "location": {
          "value": "eastus2"
        }
      }
    }
   }
   ```

1. Para obtener el estado de la implementación de la plantilla, use [Deployments - Get](/rest/api/resources/deployments/get).

   ```HTTP
   GET https://management.azure.com/subscriptions/{subscriptionId}/resourcegroups/{resourceGroupName}/providers/Microsoft.Resources/deployments/{deploymentName}?api-version=2019-10-01
   ```

## <a name="next-steps"></a>Pasos siguientes

- Para revertir a una implementación correcta cuando se produce un error, consulte [Revertir en caso de error a una implementación correcta](rollback-on-error.md).
- Para especificar cómo controlar los recursos que existen en el grupo de recursos, pero que no están definidos en la plantilla, consulte [Modos de implementación de Azure Resource Manager](deployment-modes.md).
- Para obtener información sobre el control de operaciones asincrónicas de REST, vea [Seguimiento de las operaciones asincrónicas de Azure](../management/async-operations.md).
- Para información sobre las plantillas, consulte [Nociones sobre la estructura y la sintaxis de las plantillas de ARM](template-syntax.md).

