---
title: Publicación de la aplicación administrada del catálogo de servicios
description: Se explica cómo crear una aplicación administrada de Azure diseñada para los miembros de su organización.
author: tfitzmac
ms.topic: tutorial
ms.date: 10/04/2018
ms.author: tomfitz
ms.openlocfilehash: 13c45bc6e67d9d3d06a70b7cf3326cc112cd7829
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/24/2020
ms.locfileid: "79473020"
---
# <a name="tutorial-create-and-publish-a-managed-application-definition"></a>Tutorial: Creación y publicación de una definición de aplicación administrada

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Puede crear y publicar [aplicaciones administradas](overview.md) de Azure que están diseñadas para los miembros de su organización. Por ejemplo, un departamento de TI puede publicar aplicaciones administradas que cumplen los estándares de la organización. Estas aplicaciones administradas están disponibles a través del catálogo de servicios y no en Azure Marketplace.

Para publicar una aplicación administrada en el catálogo de servicios de Azure, debe hacer lo siguiente:

* Cree una plantilla que defina los recursos que va a implementar con la aplicación administrada.
* Defina los elementos de la interfaz de usuario del portal cuando implemente la aplicación administrada.
* Cree un paquete .zip que contenga los archivos de plantilla necesarios.
* Decida qué usuario, grupo o aplicación necesita acceder al grupo de recursos en la suscripción del usuario.
* Cree la definición de aplicación administrada que apunta al paquete .zip y solicita acceso para la identidad.

Para este artículo, la aplicación administrada solo tiene una cuenta de almacenamiento. Está pensado para ilustrar los pasos necesarios para publicar una aplicación administrada. Para ver ejemplos completos, consulte [Sample projects for Azure managed applications](sample-projects.md) (Ejemplos de proyectos para aplicaciones administradas de Azure).

Los ejemplos de PowerShell de este artículo requieren Azure PowerShell 6.2 o posterior. Si es necesario, [actualice la versión](/powershell/azure/install-Az-ps).

## <a name="create-the-resource-template"></a>Creación de la plantilla de recursos

Todas las definiciones de aplicaciones administradas incluyen un archivo denominado **mainTemplate.json**. En él, se definen los recursos de Azure que desea implementar. La plantilla no difiere de una plantilla habitual de Resource Manager.

Cree un archivo denominado **mainTemplate.json**. El nombre distingue mayúsculas de minúsculas.

Agregue el siguiente JSON al archivo. Define los parámetros para crear una cuenta de almacenamiento y especifica las propiedades de la cuenta de almacenamiento.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "storageAccountNamePrefix": {
            "type": "string"
        },
        "storageAccountType": {
            "type": "string"
        },
        "location": {
            "type": "string",
            "defaultValue": "[resourceGroup().location]"
        }
    },
    "variables": {
        "storageAccountName": "[concat(parameters('storageAccountNamePrefix'), uniqueString(resourceGroup().id))]"
    },
    "resources": [
        {
            "type": "Microsoft.Storage/storageAccounts",
            "name": "[variables('storageAccountName')]",
            "apiVersion": "2016-01-01",
            "location": "[parameters('location')]",
            "sku": {
                "name": "[parameters('storageAccountType')]"
            },
            "kind": "Storage",
            "properties": {}
        }
    ],
    "outputs": {
        "storageEndpoint": {
            "type": "string",
            "value": "[reference(resourceId('Microsoft.Storage/storageAccounts/', variables('storageAccountName')), '2016-01-01').primaryEndpoints.blob]"
        }
    }
}
```

Guarde el archivo mainTemplate.json.

## <a name="defining-your-create-experience-using-createuidefinitionjson"></a>Definición de la experiencia de creación con CreateUiDefinition.json

Como publicador, defina la experiencia de creación mediante el archivo **createUiDefinition.json** que genera la interfaz para los usuarios que crean aplicaciones administradas. Defina cómo los usuarios proporcionan la entrada para cada parámetro mediante [elementos de control](create-uidefinition-elements.md), incluidas las listas desplegables, los cuadros de texto y los cuadros de contraseña.

Cree un archivo denominado **createUiDefinition.json** (distingue mayúsculas de minúsculas).

Agregue el siguiente código de inicio JSON y guárdelo.

```json
{
   "$schema": "https://schema.management.azure.com/schemas/0.1.2-preview/CreateUIDefinition.MultiVm.json#",
   "handler": "Microsoft.Azure.CreateUIDef",
   "version": "0.1.2-preview",
   "parameters": {
        "basics": [
            {}
        ],
        "steps": [
            {
                "name": "storageConfig",
                "label": "Storage settings",
                "subLabel": {
                    "preValidation": "Configure the infrastructure settings",
                    "postValidation": "Done"
                },
                "bladeTitle": "Storage settings",
                "elements": [
                    {
                        "name": "storageAccounts",
                        "type": "Microsoft.Storage.MultiStorageAccountCombo",
                        "label": {
                            "prefix": "Storage account name prefix",
                            "type": "Storage account type"
                        },
                        "defaultValue": {
                            "type": "Standard_LRS"
                        },
                        "constraints": {
                            "allowedTypes": [
                                "Premium_LRS",
                                "Standard_LRS",
                                "Standard_GRS"
                            ]
                        }
                    }
                ]
            }
        ],
        "outputs": {
            "storageAccountNamePrefix": "[steps('storageConfig').storageAccounts.prefix]",
            "storageAccountType": "[steps('storageConfig').storageAccounts.type]",
            "location": "[location()]"
        }
    }
}
```

Para más información, consulte [Introducción a CreateUiDefinition](create-uidefinition-overview.md).

## <a name="package-the-files"></a>Empaquetado de los archivos

Agregue los dos archivos a un archivo .zip denominado app.zip. Los dos archivos tienen que estar en el nivel raíz del archivo ZIP. Si los coloca en una carpeta, recibe un error al crear la definición de aplicación administrada que indica que los archivos requeridos no están presentes. 

Cargue el paquete en una ubicación accesible desde donde pueda consumirse. 

```powershell
New-AzResourceGroup -Name storageGroup -Location eastus
$storageAccount = New-AzStorageAccount -ResourceGroupName storageGroup `
  -Name "mystorageaccount" `
  -Location eastus `
  -SkuName Standard_LRS `
  -Kind Storage

$ctx = $storageAccount.Context

New-AzStorageContainer -Name appcontainer -Context $ctx -Permission blob

Set-AzStorageBlobContent -File "D:\myapplications\app.zip" `
  -Container appcontainer `
  -Blob "app.zip" `
  -Context $ctx 
```

## <a name="create-the-managed-application-definition"></a>Creación de la definición de aplicación administrada

### <a name="create-an-azure-active-directory-user-group-or-application"></a>Creación de una aplicación o grupo de usuarios de Azure Active Directory

El siguiente paso consiste en seleccionar una aplicación o un grupo de usuarios para administrar los recursos en nombre del cliente. Este grupo de usuarios o esta aplicación tienen permisos en el grupo de recursos administrado, según el rol asignado. El rol puede ser cualquier rol de Control de acceso basado en roles (RBAC) integrado como propietario o colaborador. También puede conceder a un usuario individual permisos para administrar los recursos, pero este permiso se suele asignar a un grupo de usuarios. Para crear un grupo de usuarios de Active Directory, consulte [Creación de un grupo y adición de miembros en Azure Active Directory](../../active-directory/fundamentals/active-directory-groups-create-azure-portal.md).

Necesita el identificador de objeto del grupo de usuarios que se usará para administrar los recursos. 

```powershell
$groupID=(Get-AzADGroup -DisplayName mygroup).Id
```

### <a name="get-the-role-definition-id"></a>Obtención del identificador de definición de rol

A continuación, necesita el identificador de definición de rol para el rol RBAC integrado que desea para conceder acceso al usuario, al grupo de usuarios o a la aplicación. Por lo general, se usa el rol Propietario, Colaborador o Lector. El comando siguiente muestra cómo obtener el identificador de definición de rol para el rol Propietario:

```powershell
$ownerID=(Get-AzRoleDefinition -Name Owner).Id
```

### <a name="create-the-managed-application-definition"></a>Creación de la definición de aplicación administrada

Si ya no dispone de un grupo de recursos para almacenar la definición de aplicación administrada, créelo ahora:

```powershell
New-AzResourceGroup -Name appDefinitionGroup -Location westcentralus
```

Ahora, cree el recurso de la definición de aplicación administrada.

```powershell
$blob = Get-AzStorageBlob -Container appcontainer -Blob app.zip -Context $ctx

New-AzManagedApplicationDefinition `
  -Name "ManagedStorage" `
  -Location "westcentralus" `
  -ResourceGroupName appDefinitionGroup `
  -LockLevel ReadOnly `
  -DisplayName "Managed Storage Account" `
  -Description "Managed Azure Storage Account" `
  -Authorization "${groupID}:$ownerID" `
  -PackageFileUri $blob.ICloudBlob.StorageUri.PrimaryUri.AbsoluteUri
```

## <a name="bring-your-own-storage-for-the-managed-application-definition"></a>Traiga su propio almacenamiento para la definición de aplicación administrada
Puede optar por almacenar la definición de aplicación administrada en la cuenta de almacenamiento que proporcionara durante la creación para poder administrar totalmente su ubicación y acceso en función de las necesidades regulatorias aplicables.

> [!NOTE]
> Traiga su propio almacenamiento solo es compatible con las implementaciones de la plantilla de ARM o la API REST de la definición de aplicación administrada.

### <a name="select-your-storage-account"></a>Selección de la cuenta de almacenamiento
Debe [crear una cuenta de almacenamiento](../../storage/common/storage-account-create.md) para que contenga la definición de aplicación administrada para su uso con el catálogo de servicios.

Copie el identificador de recurso de la cuenta de almacenamiento. Se usará más adelante al implementar la definición.

### <a name="set-the-role-assignment-for-appliance-resource-provider-in-your-storage-account"></a>Establezca la asignación de roles para "Appliance Resource Provider" en la cuenta de almacenamiento.
Para que la definición de aplicación administrada pueda implementarse en la cuenta de almacenamiento, debe conceder primero permisos de colaborador al rol **Appliance Resource Provider** para que pueda escribir los archivos de definición en el contenedor de la cuenta de almacenamiento.

1. En [Azure Portal](https://portal.azure.com), vaya a la cuenta de almacenamiento.
1. Seleccione **Control de acceso (IAM)** para mostrar la configuración del control de acceso de la cuenta de almacenamiento. Seleccione la pestaña **Asignaciones de roles** para ver la lista de asignaciones de roles.
1. En la ventana **Agregar asignación de roles**, seleccione el rol **Colaborador**. 
1. En el campo **Asignar acceso a**, seleccione **Usuario, grupo o entidad de servicio de Azure AD**.
1. En **Seleccionar**, busque el rol **Appliance Resource Provider** y selecciónelo.
1. Guarde la asignación de roles.

### <a name="deploy-the-managed-application-definition-with-an-arm-template"></a>Implementación de la definición de aplicación administrada con una plantilla de ARM 

Use la plantilla de ARM siguiente para implementar la aplicación administrada empaquetada como una definición de aplicación administrada nueva en el catálogo de servicios cuyos archivos de definición se guardan y mantienen en la cuenta de almacenamiento del usuario:
   
```json
    {
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "location": {
            "type": "string",
            "defaultValue": "[resourceGroup().location]"
        },
        "applicationName": {
            "type": "string",
            "metadata": {
                "description": "Managed Application name"
            }
        },
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
        "definitionStorageResourceID": {
            "type": "string",
            "metadata": {
                "description": "Storage account resource ID for where you're storing your definition"
            }
        },
        "_artifactsLocation": {
            "type": "string",
            "metadata": {
                "description": "The base URI where artifacts required by this template are located."
            }
        }
    },
    "variables": {
        "lockLevel": "None",
        "description": "Sample Managed application definition",
        "displayName": "Sample Managed application definition",
        "managedApplicationDefinitionName": "[parameters('applicationName')]",
        "packageFileUri": "[parameters('_artifactsLocation')]",
        "defLocation": "[parameters('definitionStorageResourceID')]",
        "managedResourceGroupId": "[concat(subscription().id,'/resourceGroups/', concat(parameters('applicationName'),'_managed'))]",
        "applicationDefinitionResourceId": "[resourceId('Microsoft.Solutions/applicationDefinitions',variables('managedApplicationDefinitionName'))]"
    },
    "resources": [
        {
            "type": "Microsoft.Solutions/applicationDefinitions",
            "apiVersion": "2019-07-01",
            "name": "[variables('managedApplicationDefinitionName')]",
            "location": "[parameters('location')]",
            "properties": {
                "lockLevel": "[variables('lockLevel')]",
                "description": "[variables('description')]",
                "displayName": "[variables('displayName')]",
                "packageFileUri": "[variables('packageFileUri')]",
                "storageAccountId": "[variables('defLocation')]"
            }
        }
    ],
    "outputs": {}
}
```

Se ha agregado una nueva propiedad llamada **storageAccountId** a las propiedades de applicationDefintion y se proporciona el identificador de la cuenta de almacenamiento en donde almacenar la definición como valor:

Puede comprobar que los archivos de definición de aplicación se guardan en la cuenta de almacenamiento proporcionada en un contenedor titulado **applicationdefinitions**.

> [!NOTE]
> Para mayor seguridad, puede crear una definición de aplicación administrada y almacenarla en un [blob de cuenta de almacenamiento de Azure con el cifrado habilitado](../../storage/common/storage-service-encryption.md). El contenido de la definición se cifra mediante las opciones de cifrado de la cuenta de almacenamiento. Solo los usuarios con permisos para el archivo pueden ver la definición en el catálogo de servicios.

### <a name="make-sure-users-can-see-your-definition"></a>Asegurarse de que los usuarios pueden ver la definición

Tiene acceso a la definición de la aplicación administrada, pero desea asegurarse de que otros usuarios de su organización puedan acceder a ella. Concédales al menos el rol de lector en la definición. Puede haber heredado este nivel de acceso de la suscripción o del grupo de recursos. Para comprobar quién tiene acceso a la definición y agregar usuarios o grupos, consulte [Uso del control de acceso basado en rol para administrar el acceso a los recursos de la suscripción de Azure](../../role-based-access-control/role-assignments-portal.md).

## <a name="next-steps"></a>Pasos siguientes

* Para publicar la aplicación administrada en Azure Marketplace, consulte [Aplicaciones administradas de Azure en Marketplace](publish-marketplace-app.md).
* Para implementar una instancia de la aplicación administrada, consulte [Implementación de la aplicación de catálogo de servicio mediante Azure Portal](deploy-service-catalog-quickstart.md).
