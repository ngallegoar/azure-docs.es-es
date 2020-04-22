---
title: Inicio rápido para la implementación automatizada de máquina virtual con Azure App Configuration
description: En este inicio rápido se muestra cómo usar el módulo Azure PowerShell y las plantillas de Azure Resource Manager para implementar un almacén de Azure App Configuration. Y a continuación, utilizar los valores del almacén para implementar una máquina virtual.
author: lisaguthrie
ms.author: lcozzens
ms.date: 04/14/2020
ms.topic: quickstart
ms.service: azure-app-configuration
ms.custom:
- mvc
- subject-armqs
ms.openlocfilehash: 02afa2cb36323e0c3c38c2451b1924b636f7faed
ms.sourcegitcommit: 7e04a51363de29322de08d2c5024d97506937a60
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/14/2020
ms.locfileid: "81309108"
---
# <a name="quickstart-automated-vm-deployment-with-app-configuration-and-resource-manager-template"></a>Inicio rápido: Implementación automatizada de máquina virtual con App Configuration y la plantilla de Resource Manager

El módulo de Azure PowerShell se usa para crear y administrar recursos de Azure mediante cmdlets o scripts de PowerShell. Este inicio rápido le muestra cómo usar Azure PowerShell y las plantillas de Azure Resource Manager para implementar un almacén de Azure App Configuration. Acto seguido, aprenderá a usar los valores de clave en el almacén para implementar una máquina virtual.

Use la plantilla de requisitos previos para crear un almacén de App Configuration y después agregue valores de clave en el almacén mediante Azure Portal o la CLI de Azure. La plantilla principal hace referencia a las configuraciones de clave-valor existentes de un almacén de configuración existente. Los valores recuperados se usan para establecer las propiedades de los recursos creados por la plantilla, como una máquina virtual en este ejemplo.

[!INCLUDE [About Azure Resource Manager](../../includes/resource-manager-quickstart-introduction.md)]

## <a name="before-you-begin"></a>Antes de empezar

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

* Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/).

* Este inicio rápido requiere el módulo Azure PowerShell. Ejecute `Get-Module -ListAvailable Az` para buscar la versión que está instalada en la máquina local. Si necesita instalarla o actualizarla, consulte el artículo sobre [cómo instalar el módulo de Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-Az-ps).

## <a name="sign-in-to-azure"></a>Inicio de sesión en Azure

Inicie sesión en la suscripción de Azure con el comando `Connect-AzAccount` y escriba las credenciales de Azure en el explorador emergente:

```azurepowershell-interactive
# Connect to your Azure account
Connect-AzAccount
```

Si tiene más de una suscripción, seleccione la que desee utilizar para este inicio rápido mediante la ejecución de los siguientes cmdlets. No olvide reemplazar `<your subscription name>` por el nombre de su suscripción:

```azurepowershell-interactive
# List all available subscriptions.
Get-AzSubscription

# Select the Azure subscription you want to use to create the resource group and resources.
Get-AzSubscription -SubscriptionName "<your subscription name>" | Select-AzSubscription
```

## <a name="create-a-resource-group"></a>Crear un grupo de recursos

Cree un grupo de recursos de Azure con [New-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/new-azresourcegroup). Un grupo de recursos es un contenedor lógico en el que se implementan y se administran los recursos de Azure.

```azurepowershell-interactive
$resourceGroup = "StreamAnalyticsRG"
$location = "WestUS2"
New-AzResourceGroup `
    -Name $resourceGroup `
    -Location $location
```

## <a name="deploy-an-azure-app-configuration-store"></a>Implementación de un almacén de Azure App Configuration

Antes de poder aplicar valores clave a la máquina virtual, tiene que tener ya un almacén de Azure App Configuration. En esta sección se describe cómo implementar un almacén de Azure App Configuration mediante una plantilla de Azure Resource Manager. Si ya tiene un almacén de App Configuration, puede pasar a la siguiente sección de este artículo. 

1. Copie y pegue el siguiente código JSON en un nuevo archivo denominado *prereq.azuredeploy.json*.

   ```json
   {
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
      "configStoreName": {
        "type": "string",
        "metadata": {
          "description": "Specifies the name of the app configuration store."
        }
      },
      "location": {
        "type": "string",
        "defaultValue": "[resourceGroup().location]",
        "metadata": {
          "description": "Specifies the Azure location where the app configuration store should be created."
        }
      },
      "skuName": {
        "type": "string",
        "defaultValue": "standard",
        "metadata": {
          "description": "Specifies the SKU of the app configuration store."
        }
      }
    },
    "resources": [
      {
        "type": "Microsoft.AppConfiguration/configurationStores",
        "name": "[parameters('configStoreName')]",
        "apiVersion": "2019-10-01",
        "location": "[parameters('location')]",
        "sku": {
          "name": "[parameters('skuName')]"
        }
      }
    ]
   }
   ```

1. Copie y pegue el siguiente código JSON en un nuevo archivo denominado *prereq.azuredeploy.parameters.json*. Reemplace **GET-UNIQUE** por un nombre único para el almacén de App Configuration.

   ```json
   {
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
      "configStoreName": {
        "value": "GET-UNIQUE"
      }
    }
   }
   ```

1. En la ventana de PowerShell, ejecute el siguiente comando para implementar el almacén de Azure App Configuration. No olvide reemplazar el nombre del grupo de recursos, la ruta de acceso del archivo de plantilla y la ruta de acceso del archivo de parámetros de plantilla.

   ```azurepowershell
   New-AzResourceGroupDeployment `
       -ResourceGroupName "<your resource group>" 
       -TemplateFile "<path to prereq.azuredeploy.json>" `
       -TemplateParameterFile "<path to prereq.azuredeploy.parameters.json>"
   ```

## <a name="add-vm-configuration-key-values"></a>Incorporación de pares clave-valor de configuración de máquina virtual

Puede crear un almacén de App Configuration mediante una plantilla de Azure Resource Manager plantilla, pero tiene que agregar pares clave-valor mediante Azure Portal o la CLI de Azure. En este inicio rápido se agregan los pares clave-valor mediante Azure Portal.

1. Una vez finalizada la implementación, vaya al recién creado almacén de App Configuration en [Azure Portal](https://portal.azure.com).

1. Seleccione **Configuración** > **Claves de acceso**. Tome nota de la cadena de conexión de la clave de solo lectura principal. Usará esta cadena de conexión más adelante para configurar la aplicación y que se comunique con el almacén de Azure App Configuration que ha creado.

1. Seleccione **Explorador de configuración** > **Crear** para agregar los siguientes pares clave-valor:

   |Clave|Value|
   |-|-|
   |windowsOsVersion|2019-Datacenter|
   |diskSizeGB|1023|
  
   Escriba *plantilla* para **Etiqueta**, pero mantenga **Tipo de contenido** vacío.

## <a name="deploy-vm-using-stored-key-values"></a>Implementación de máquina virtual con pares clave-valor almacenados

Ahora que ha agregado pares clave-valor al almacén, ya está preparado para implementar una máquina virtual mediante una plantilla de Azure Resource Manager. La plantilla hace referencia a las claves **windowsOsVersion** y **diskSizeGB** que creó.

> [!WARNING]
> Las plantillas de Resource Manager no pueden hacer referencia a claves de un almacén de App Configuration que tiene habilitado Private Link.

1. Copie y pegue el siguiente código JSON en un nuevo archivo denominado *azuredeploy.json*, o bien descargue el archivo desde [plantillas de Inicio rápido de Azure](https://github.com/Azure/azure-quickstart-templates/blob/master/101-app-configuration/azuredeploy.json).

   ```json
   {
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "adminUsername": {
            "type": "string",
            "metadata": {
                "description": "Admin user name."
            }
        },
        "adminPassword": {
            "type": "securestring",
            "metadata": {
                "description": "Password for the Virtual Machine."
            }
        },
        "appConfigStoreName": {
            "type": "string",
            "metadata": {
                "description": "App configuration store name."
            }
        },
        "appConfigStoreResourceGroup": {
            "type": "string",
            "metadata": {
                "description": "Name of the resource group for the app config store."
            }
        },
        "domainNameLabel": {
            "type": "string",
            "metadata": {
                "description": "The DNS label for the public IP address. It must be lowercase. It should match the following regular expression, or it will raise an error: ^[a-z][a-z0-9-]{1,61}[a-z0-9]$."
            }
        },
        "location": {
            "type": "string",
            "defaultValue": "[resourceGroup().location]",
            "metadata": {
                "description": "Location for all resources."
            }
        },
        "vmSize": {
            "type": "string",
            "defaultValue": "Standard_D2_v3",
            "metadata": {
                "description": "Size of the VM"
            }
        },
        "vmSkuKey": {
            "type": "string",
            "metadata": {
                "description": "Name of the key in the app config store for the VM windows sku"
            }
        },
        "diskSizeKey": {
            "type": "string",
            "metadata": {
                "description": "Name of the key in the app config store for the VM disk size"
            }
        },
        "storageAccountName": {
            "type": "string",
            "metadata": {
                "description": "The name of the storage account."
            }
        }
    },
    "variables": {
        "nicName": "myVMNic",
        "addressPrefix": "10.0.0.0/16",
        "subnetName": "Subnet",
        "subnetPrefix": "10.0.0.0/24",
        "publicIPAddressName": "myPublicIP",
        "vmName": "SimpleWinVM",
        "virtualNetworkName": "MyVNET",
        "subnetRef": "[resourceId('Microsoft.Network/virtualNetworks/subnets', variables('virtualNetworkName'), variables('subnetName'))]",
        "appConfigRef": "[resourceId(parameters('appConfigStoreResourceGroup'), 'Microsoft.AppConfiguration/configurationStores', parameters('appConfigStoreName'))]",
        "windowsOSVersionParameters": {
            "key": "[parameters('vmSkuKey')]",
            "label": "template"
        },
        "diskSizeGBParameters": {
            "key": "[parameters('diskSizeKey')]",
            "label": "template"
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
            "properties": {
            }
        },
        {
            "type": "Microsoft.Network/publicIPAddresses",
            "apiVersion": "2018-11-01",
            "name": "[variables('publicIPAddressName')]",
            "location": "[parameters('location')]",
            "properties": {
                "publicIPAllocationMethod": "Dynamic",
                "dnsSettings": {
                    "domainNameLabel": "[parameters('domainNameLabel')]"
                }
            }
        },
        {
            "type": "Microsoft.Network/virtualNetworks",
            "apiVersion": "2018-11-01",
            "name": "[variables('virtualNetworkName')]",
            "location": "[parameters('location')]",
            "properties": {
                "addressSpace": {
                    "addressPrefixes": [
                        "[variables('addressPrefix')]"
                    ]
                },
                "subnets": [
                    {
                        "name": "[variables('subnetName')]",
                        "properties": {
                            "addressPrefix": "[variables('subnetPrefix')]"
                        }
                    }
                ]
            }
        },
        {
            "type": "Microsoft.Network/networkInterfaces",
            "apiVersion": "2018-11-01",
            "name": "[variables('nicName')]",
            "location": "[parameters('location')]",
            "dependsOn": [
                "[resourceId('Microsoft.Network/publicIPAddresses/', variables('publicIPAddressName'))]",
                "[resourceId('Microsoft.Network/virtualNetworks/', variables('virtualNetworkName'))]"
            ],
            "properties": {
                "ipConfigurations": [
                    {
                        "name": "ipconfig1",
                        "properties": {
                            "privateIPAllocationMethod": "Dynamic",
                            "publicIPAddress": {
                                "id": "[resourceId('Microsoft.Network/publicIPAddresses',variables('publicIPAddressName'))]"
                            },
                            "subnet": {
                                "id": "[variables('subnetRef')]"
                            }
                        }
                    }
                ]
            }
        },
        {
            "type": "Microsoft.Compute/virtualMachines",
            "apiVersion": "2018-10-01",
            "name": "[variables('vmName')]",
            "location": "[parameters('location')]",
            "dependsOn": [
                "[resourceId('Microsoft.Storage/storageAccounts/', parameters('storageAccountName'))]",
                "[resourceId('Microsoft.Network/networkInterfaces/', variables('nicName'))]"
            ],
            "properties": {
                "hardwareProfile": {
                    "vmSize": "[parameters('vmSize')]"
                },
                "osProfile": {
                    "computerName": "[variables('vmName')]",
                    "adminUsername": "[parameters('adminUsername')]",
                    "adminPassword": "[parameters('adminPassword')]"
                },
                "storageProfile": {
                    "imageReference": {
                        "publisher": "MicrosoftWindowsServer",
                        "offer": "WindowsServer",
                        "sku": "[listKeyValue(variables('appConfigRef'), '2019-10-01', variables('windowsOSVersionParameters')).value]",
                        "version": "latest"
                    },
                    "osDisk": {
                        "createOption": "FromImage"
                    },
                    "dataDisks": [
                        {
                            "diskSizeGB": "[listKeyValue(variables('appConfigRef'), '2019-10-01', variables('diskSizeGBParameters')).value]",
                            "lun": 0,
                            "createOption": "Empty"
                        }
                    ]
                },
                "networkProfile": {
                    "networkInterfaces": [
                        {
                            "id": "[resourceId('Microsoft.Network/networkInterfaces',variables('nicName'))]"
                        }
                    ]
                },
                "diagnosticsProfile": {
                    "bootDiagnostics": {
                        "enabled": true,
                        "storageUri": "[reference(resourceId('Microsoft.Storage/storageAccounts/', parameters('storageAccountName'))).primaryEndpoints.blob]"
                    }
                }
            }
        }
    ],
    "outputs": {
        "hostname": {
            "type": "string",
            "value": "[reference(variables('publicIPAddressName')).dnsSettings.fqdn]"
        }
    }
   }
   ```

1. Copie y pegue el siguiente código JSON en un nuevo archivo denominado *azuredeploy.parameters.json*, o bien descargue el archivo desde [plantillas de Inicio rápido de Azure](https://github.com/Azure/azure-quickstart-templates/blob/master/101-app-configuration/azuredeploy.parameters.json).

   ```json
   {
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
      "adminPassword": {
        "value": "GEN-PASSWORD"
      },
      "appConfigStoreName":{
        "value": "GEN-APPCONFIGSTORE-NAME"
      },
      "appConfigStoreResourceGroup": {
         "value": "GEN-APPCONFIGSTORE-RESOURCEGROUP-NAME"
      },
      "vmSkuKey":{
        "value": "GEN-APPCONFIGSTORE-WINDOWSOSVERSION"
      },
      "diskSizeKey" :{
         "value": "GEN-APPCONFIGSTORE-DISKSIZEGB"
      },
      "adminUsername":{
        "value": "GEN-UNIQUE"
      },
      "storageAccountName":{
        "value": "GEN-UNIQUE"
      },
      "domainNameLabel":{
        "value": "GEN-UNIQUE"
      }
    }
   }
   ```

   Reemplace los valores de parámetro de la plantilla por los valores siguientes:

   |Parámetro|Value|
   |-|-|
   |adminPassword|Una contraseña del administrador para la máquina virtual.|
   |appConfigStoreName|El nombre del almacén de Azure App Configuration.|
   |appConfigStoreResourceGroup|El grupo de recursos que contiene el almacén de App Configuration.|
   |vmSkuKey|*windowsOSVersion*|
   |diskSizeKey|*diskSizeGB*|
   |adminUsername|Un nombre de usuario del administrador para la máquina virtual.|
   |storageAccountName|Un nombre único para una cuenta de almacenamiento asociada a la máquina virtual.|
   |domainNameLabel|Un nombre de dominio único.|

1. En la ventana de PowerShell, ejecute el siguiente comando para implementar la máquina virtual. No olvide reemplazar el nombre del grupo de recursos, la ruta de acceso del archivo de plantilla y la ruta de acceso del archivo de parámetros de plantilla.

   ```azurepowershell
   New-AzResourceGroupDeployment `
       -ResourceGroupName "<your resource group>"
       -TemplateFile "<path to azuredeploy.json>" `
       -TemplateParameterFile "<path to azuredeploy.parameters.json>"
   ```

Felicidades. Ha implementado una máquina virtual mediante configuraciones almacenadas en Azure App Configuration.

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando ya no los necesite, elimine el grupo de recursos, el almacén de App Configuration y todos los recursos relacionados. Si tiene planes para utilizar el almacén de App Configuration o la máquina virtual en el futuro, puede omitir su eliminación. Si no va a seguir usando este trabajo, ejecute el siguiente cmdlet para eliminar todos los recursos creados en este inicio rápido:

```azurepowershell-interactive
Remove-AzResourceGroup `
  -Name $resourceGroup
```

## <a name="next-steps"></a>Pasos siguientes

En este inicio rápido, ha implementado una máquina virtual con una plantilla de Azure Resource Manager y pares de clave-valor de Azure App Configuration.

Para obtener información acerca de cómo crear otras aplicaciones con Azure App Configuration, continúe con el siguiente artículo:

> [!div class="nextstepaction"]
> [Inicio rápido: Creación de una aplicación ASP.NET Core con Azure App Configuration](quickstart-aspnet-core-app.md)
