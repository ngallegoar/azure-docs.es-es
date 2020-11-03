---
title: Prueba de una imagen de máquina virtual de Azure para Azure Marketplace
description: Obtenga información sobre cómo probar y enviar una oferta de máquina virtual de Azure en Azure Marketplace.
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: how-to
author: github-2407
ms.author: krsh
ms.date: 10/15/2020
ms.openlocfilehash: 36eebb218ed2b2d9a48cf7d970896115af5cf6f8
ms.sourcegitcommit: 6906980890a8321dec78dd174e6a7eb5f5fcc029
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/22/2020
ms.locfileid: "92424858"
---
# <a name="test-a-virtual-machine-image"></a>Prueba de una imagen de máquina virtual

En este artículo se describe cómo probar y enviar una imagen de máquina virtual (VM) en Marketplace comercial para asegurarse de que cumple los requisitos de publicación más recientes de Azure Marketplace.

Siga estos pasos antes de enviar la oferta de máquina virtual:

- Implemente una máquina virtual de Azure con la imagen generalizada; consulte [Creación de una máquina virtual con una base aprobada](azure-vm-create-using-approved-base.md) o [Creación de una máquina virtual con su propia imagen](azure-vm-create-using-own-image.md).
- Ejecute las validaciones.

## <a name="deploy-an-azure-vm-using-your-generalized-image"></a>Implementación de una máquina virtual de Azure con la imagen generalizada

En esta sección se describe cómo implementar una imagen de disco duro virtual (VHD) generalizada para crear un recurso de máquina virtual de Azure. En este proceso, usaremos la plantilla de Azure Resource Manager proporcionada y el script de Azure PowerShell.

### <a name="prepare-an-azure-resource-manager-template"></a>Preparación de una plantilla de Azure Resource Manager

En esta sección se describe cómo crear e implementar una imagen de máquina virtual (VM) proporcionada por el usuario. Para ello, proporcione imágenes del VHD del sistema operativo y del disco de datos desde un disco duro virtual implementado en Azure. En estos pasos se implementa la máquina virtual mediante un VHD generalizado.

1. Inicie sesión en [Azure Portal](https://portal.azure.com/).
2. Cargue el VHD del sistema operativo generalizado y los VHD del disco de datos en su cuenta de Azure Storage.
3. En la página principal, seleccione **Crear un recurso** , busque "Template Deployment" y seleccione **Crear**.
4. Seleccione **Cree su propia plantilla en el editor**.

    :::image type="content" source="media/vm/template-deployment.png" alt-text="Muestra la selección de una plantilla.":::

5. Pegue la siguiente plantilla JSON en el editor y seleccione **Guardar**.

```JSON
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "userStorageAccountName": {
            "type": "String"
        },
        "userStorageContainerName": {
            "defaultValue": "vhds",
            "type": "String"
        },
        "dnsNameForPublicIP": {
            "type": "String"
        },
        "adminUserName": {
            "defaultValue": "isv",
            "type": "String"
        },
        "adminPassword": {
            "defaultValue": "",
            "type": "SecureString"
        },
        "osType": {
            "defaultValue": "windows",
            "allowedValues": [
                "windows",
                "linux"
            ],
            "type": "String"
        },
        "subscriptionId": {
            "type": "String"
        },
        "location": {
            "type": "String"
        },
        "vmSize": {
            "type": "String"
        },
        "publicIPAddressName": {
            "type": "String"
        },
        "vmName": {
            "type": "String"
        },
        "virtualNetworkName": {
            "type": "String"
        },
        "nicName": {
            "type": "String"
        },
        "vhdUrl": {
            "type": "String",
            "metadata": {
                "description": "VHD Url..."
            }
        }
    },
    "variables": {
        "addressPrefix": "10.0.0.0/16",
        "subnet1Name": "Subnet-1",
        "subnet2Name": "Subnet-2",
        "subnet1Prefix": "10.0.0.0/24",
        "subnet2Prefix": "10.0.1.0/24",
        "publicIPAddressType": "Dynamic",
        "vnetID": "[resourceId('Microsoft.Network/virtualNetworks',parameters('virtualNetworkName'))]",
        "subnet1Ref": "[concat(variables('vnetID'),'/subnets/',variables('subnet1Name'))]",
        "hostDNSNameScriptArgument": "[concat(parameters('dnsNameForPublicIP'),'.',parameters('location'),'.cloudapp.azure.com')]",
        "osDiskVhdName": "[concat('http://',parameters('userStorageAccountName'),'.blob.core.windows.net/',parameters('userStorageContainerName'),'/',parameters('vmName'),'osDisk.vhd')]"
    },
    "resources": [
        {
            "type": "Microsoft.Network/publicIPAddresses",
            "apiVersion": "2015-06-15",
            "name": "[parameters('publicIPAddressName')]",
            "location": "[parameters('location')]",
            "properties": {
                "publicIPAllocationMethod": "[variables('publicIPAddressType')]",
                "dnsSettings": {
                    "domainNameLabel": "[parameters('dnsNameForPublicIP')]"
                }
            }
        },
        {
            "type": "Microsoft.Network/virtualNetworks",
            "apiVersion": "2015-06-15",
            "name": "[parameters('virtualNetworkName')]",
            "location": "[parameters('location')]",
            "properties": {
                "addressSpace": {
                    "addressPrefixes": [
                        "[variables('addressPrefix')]"
                    ]
                },
                "subnets": [
                    {
                        "name": "[variables('subnet1Name')]",
                        "properties": {
                            "addressPrefix": "[variables('subnet1Prefix')]"
                        }
                    },
                    {
                        "name": "[variables('subnet2Name')]",
                        "properties": {
                            "addressPrefix": "[variables('subnet2Prefix')]"
                        }
                    }
                ]
            }
        },
        {
            "type": "Microsoft.Network/networkInterfaces",
            "apiVersion": "2015-06-15",
            "name": "[parameters('nicName')]",
            "location": "[parameters('location')]",
            "dependsOn": [
                "[concat('Microsoft.Network/publicIPAddresses/', parameters('publicIPAddressName'))]",
                "[concat('Microsoft.Network/virtualNetworks/', parameters('virtualNetworkName'))]"
            ],
            "properties": {
                "ipConfigurations": [
                    {
                        "name": "ipconfig1",
                        "properties": {
                            "privateIPAllocationMethod": "Dynamic",
                            "publicIPAddress": {
                                "id": "[resourceId('Microsoft.Network/publicIPAddresses',parameters('publicIPAddressName'))]"
                            },
                            "subnet": {
                                "id": "[variables('subnet1Ref')]"
                            }
                        }
                    }
                ]
            }
        },
        {
            "type": "Microsoft.Compute/virtualMachines",
            "apiVersion": "2015-06-15",
            "name": "[parameters('vmName')]",
            "location": "[parameters('location')]",
            "dependsOn": [
                "[concat('Microsoft.Network/networkInterfaces/', parameters('nicName'))]"
            ],
            "properties": {
                "hardwareProfile": {
                    "vmSize": "[parameters('vmSize')]"
                },
                "osProfile": {
                    "computername": "[parameters('vmName')]",
                    "adminUsername": "[parameters('adminUsername')]",
                    "adminPassword": "[parameters('adminPassword')]"
                },
                "storageProfile": {
                    "osDisk": {
                        "name": "[concat(parameters('vmName'),'-osDisk')]",
                        "osType": "[parameters('osType')]",
                        "caching": "ReadWrite",
                        "image": {
                            "uri": "[parameters('vhdUrl')]"
                        },
                        "vhd": {
                            "uri": "[variables('osDiskVhdName')]"
                        },
                        "createOption": "FromImage"
                    }
                },
                "networkProfile": {
                    "networkInterfaces": [
                        {
                            "id": "[resourceId('Microsoft.Network/networkInterfaces',parameters('nicName'))]"
                        }
                    ]
                }
            }
        },
        {
            "type": "Microsoft.Compute/virtualMachines/extensions",
            "apiVersion": "2015-06-15",
            "name": "[concat(parameters('vmName'),'/WinRMCustomScriptExtension')]",
            "location": "[parameters('location')]",
            "dependsOn": [
                "[concat('Microsoft.Compute/virtualMachines/', parameters('vmName'))]"
            ],
            "properties": {
                "publisher": "Microsoft.Compute",
                "type": "CustomScriptExtension",
                "typeHandlerVersion": "1.4",
                "settings": {
                    "fileUris": [
                        "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-vm-winrm-windows/ConfigureWinRM.ps1",
                        "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-vm-winrm-windows/makecert.exe",
                        "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-vm-winrm-windows/winrmconf.cmd"
                    ],
                    "commandToExecute": "[concat('powershell -ExecutionPolicy Unrestricted -file ConfigureWinRM.ps1 ',variables('hostDNSNameScriptArgument'))]"
                }
            }
        }
    ]
}
```

<br>

6. Proporcione los valores de parámetro de las páginas de propiedades Implementación personalizada que se muestran.

    | ResourceGroupName | El nombre del grupo de recursos de Azure existente. Normalmente, use el mismo grupo de recursos que el almacén de claves. |
    | --- | --- |
    | TemplateFile | Ruta de acceso completa al archivo VHDtoImage.json. |
    | userStorageAccountName | Nombre de la cuenta de almacenamiento. |
    | dnsNameForPublicIP | Nombre DNS para la dirección IP pública; debe estar en minúscula. |
    | subscriptionId | Identificador de la suscripción de Azure. |
    | Location | Ubicación geográfica de Azure estándar del grupo de recursos. |
    | vmName | Nombre de la máquina virtual. |
    | vhdUrl | Dirección web del disco duro virtual. |
    | vmSize | Tamaño de la instancia de máquina virtual. |
    | publicIPAddressName | Nombre de la dirección IP pública. |
    | virtualNetworkName | Nombre de la red virtual. |
    | nicName | Nombre de la tarjeta de interfaz de red de la red virtual. |
    | adminUserName | Nombre de usuario de la cuenta de administrador. |
    | adminPassword | Contraseña de administrador. |


7. Después de proporcionar estos valores, seleccione **Comprar**.

Azure comenzará la implementación. Crea una máquina virtual con el VHD no administrado especificado en la ruta de acceso de la cuenta de almacenamiento especificada. Puede realizar un seguimiento del progreso en Azure Portal si selecciona **Máquinas virtuales** en el lado izquierdo del portal. Una vez creada la máquina virtual, el estado cambiará de Iniciando a En ejecución.

#### <a name="for-generation-2-vm-deployment-use-this-template"></a>Para la implementación de máquinas virtuales de generación 2, use esta plantilla:

```JSON
{
   "$schema":"https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
   "contentVersion":"1.0.0.0",
   "parameters":{
      "userStorageAccountName":{
         "type":"String"
      },
      "userStorageContainerName":{
         "type":"String",
         "defaultValue":"vhds-86350-720-f4efbbb2-611b-4cd7-ad1e-afdgfuadfluad"
      },
      "dnsNameForPublicIP":{
         "type":"String"
      },
      "adminUserName":{
         "defaultValue":"isv",
         "type":"String"
      },
      "adminPassword":{
         "type":"securestring",
         "defaultValue":"Certcare@86350"
      },
      "osType":{
         "type":"string",
         "defaultValue":"linux",
         "allowedValues":[
            "windows",
            "linux"
         ]
      },
      "subscriptionId":{
         "type":"string"
      },
      "location":{
         "type":"string"
      },
      "vmSize":{
         "type":"string"
      },
      "publicIPAddressName":{
         "type":"string"
      },
      "vmName":{
         "type":"string"
      },
      "vNetNewOrExisting":{
         "defaultValue":"existing",
         "allowedValues":[
            "new",
            "existing"
         ],
         "type":"String",
         "metadata":{
            "description":"Specify whether to create a new or existing virtual network for the VM."
         }
      },
      "virtualNetworkName":{
         "type":"String",
         "defaultValue":""
      },
      "SubnetName":{
         "defaultValue":"subnet-5",
         "type":"String"
      },
      "SubnetPrefix":{
         "defaultValue":"10.0.5.0/24",
         "type":"String"
      },
      "nicName":{
         "type":"string"
      },
      "vhdUrl":{
         "type":"string",
         "metadata":{
            "description":"VHD Url..."
         }
      },
      "Datadisk1":{
         "type":"string",
         "metadata":{
            "description":"datadisk1 Url..."
         }
      },
      "Datadisk2":{
         "type":"string",
         "metadata":{
            "description":"datadisk2 Url..."
         }
      },
      "Datadisk3":{
         "type":"string",
         "metadata":{
            "description":"datadisk2 Url..."
         }
      }
   },
   "variables":{
      "addressPrefix":"10.0.0.0/16",
      "subnet1Name":"[parameters('SubnetName')]",
      "subnet1Prefix":"[parameters('SubnetPrefix')]",
      "publicIPAddressType":"Static",
      "vnetID":"[resourceId('Microsoft.Network/virtualNetworks',parameters('virtualNetworkName'))]",
      "subnet1Ref":"[concat(variables('vnetID'),'/subnets/',variables('subnet1Name'))]",
      "osDiskVhdName":"[concat('http://',parameters('userStorageAccountName'),'.blob.core.windows.net/',parameters('userStorageContainerName'),'/',parameters('vmName'),'osDisk.vhd')]",
      "dataDiskVhdName":"[concat('http://',parameters('userStorageAccountName'),'.blob.core.windows.net/',parameters('userStorageContainerName'),'/',parameters('vmName'),'datadisk')]",
      "imageName":"[concat(parameters('vmName'), '-image')]"
   },
   "resources":[
      {
         "apiVersion":"2015-05-01-preview",
         "type":"Microsoft.Network/publicIPAddresses",
         "name":"[parameters('publicIPAddressName')]",
         "location":"[parameters('location')]",
         "properties":{
            "publicIPAllocationMethod":"[variables('publicIPAddressType')]",
            "dnsSettings":{
               "domainNameLabel":"[parameters('dnsNameForPublicIP')]"
            }
         }
      },
      {
         "type":"Microsoft.Compute/images",
         "apiVersion":"2019-12-01",
         "name":"[variables('imageName')]",
         "location":"[parameters('location')]",
         "properties":{
            "storageProfile":{
               "osDisk":{
                  "osType":"[parameters('osType')]",
                  "osState":"Generalized",
                  "blobUri":"[parameters('vhdUrl')]",
                  "storageAccountType":"Standard_LRS"
               },
               "dataDisks":[
                  {
                     "lun":0,
                     "blobUri":"[parameters('Datadisk1')]",
                     "storageAccountType":"Standard_LRS"
                  },
                  {
                     "lun":1,
                     "blobUri":"[parameters('Datadisk2')]",
                     "storageAccountType":"Standard_LRS"
                  },
                  {
                     "lun":2,
                     "blobUri":"[parameters('Datadisk3')]",
                     "storageAccountType":"Standard_LRS"
                  }
               ]
            },
            "hyperVGeneration":"V2"
         }
      },
      {
         "apiVersion":"2015-05-01-preview",
         "type":"Microsoft.Network/virtualNetworks",
         "name":"[parameters('virtualNetworkName')]",
         "location":"[parameters('location')]",
         "properties":{
            "addressSpace":{
               "addressPrefixes":[
                  "[variables('addressPrefix')]"
               ]
            },
            "subnets":[
               {
                  "name":"[variables('subnet1Name')]",
                  "properties":{
                     "addressPrefix":"[variables('subnet1Prefix')]"
                  }
               }
            ]
         },
         "condition":"[equals(parameters('vNetNewOrExisting'), 'new')]"
      },
      {
         "apiVersion":"2016-09-01",
         "type":"Microsoft.Network/networkInterfaces",
         "name":"[parameters('nicName')]",
         "location":"[parameters('location')]",
         "dependsOn":[
            "[concat('Microsoft.Network/publicIPAddresses/', parameters('publicIPAddressName'))]",
            "[concat('Microsoft.Network/virtualNetworks/', parameters('virtualNetworkName'))]"
         ],
         "properties":{
            "ipConfigurations":[
               {
                  "name":"ipconfig1",
                  "properties":{
                     "privateIPAllocationMethod":"Dynamic",
                     "publicIPAddress":{
                        "id":"[resourceId('Microsoft.Network/publicIPAddresses',parameters('publicIPAddressName'))]"
                     },
                     "subnet":{
                        "id":"[variables('subnet1Ref')]"
                     }
                  }
               }
            ]
         }
      },
      {
         "apiVersion":"2019-12-01",
         "type":"Microsoft.Compute/virtualMachines",
         "name":"[parameters('vmName')]",
         "location":"[parameters('location')]",
         "dependsOn":[
            "[concat('Microsoft.Network/networkInterfaces/', parameters('nicName'))]",
            "[variables('imageName')]"
         ],
         "properties":{
            "hardwareProfile":{
               "vmSize":"[parameters('vmSize')]"
            },
            "osProfile":{
               "computername":"[parameters('vmName')]",
               "adminUsername":"[parameters('adminUsername')]",
               "adminPassword":"[parameters('adminPassword')]"
            },
            "storageProfile":{
               "imageReference":{
                  "id":"[resourceId('Microsoft.Compute/images', variables('imageName'))]"
               },
               "dataDisks":[
                  {
                     "name":"[concat(parameters('vmName'),'_DataDisk0')]",
                     "lun":0,
                     "createOption":"FromImage",
                     "managedDisk":{
                        "storageAccountType":"Standard_LRS"
                     }
                  },
                  {
                     "name":"[concat(parameters('vmName'),'_DataDisk1')]",
                     "lun":1,
                     "createOption":"FromImage",
                     "managedDisk":{
                        "storageAccountType":"Standard_LRS"
                     }
                  },
                  {
                     "name":"[concat(parameters('vmName'),'_DataDisk2')]",
                     "lun":2,
                     "createOption":"FromImage",
                     "managedDisk":{
                        "storageAccountType":"Standard_LRS"
                     }
                  }
               ],
               "osDisk":{
                  "name":"[concat(parameters('vmName'),'-OSDisk')]",
                  "createOption":"FromImage",
                  "managedDisk":{
                     "storageAccountType":"Standard_LRS"
                  }
               }
            },
            "networkProfile":{
               "networkInterfaces":[
                  {
                     "id":"[resourceId('Microsoft.Network/networkInterfaces',parameters('nicName'))]"
                  }
               ]
            }
         }
      }
   ]
}
```

### <a name="deploy-an-azure-vm-using-powershell"></a>Implementación de una máquina virtual de Azure mediante PowerShell

Copie y edite el script siguiente para proporcionar valores para las variables `$storageaccount` y `$vhdUrl`. Ejecútelo para crear un recurso de máquina virtual de Azure a partir de su disco duro virtual generalizado existente.

```PowerShell
# storage account of existing generalized VHD
$storageaccount = "testwinrm11815"
# generalized VHD URL
$vhdUrl = "https://testwinrm11815.blob.core.windows.net/vhds/testvm1234562016651857.vhd"

echo "New-AzResourceGroupDeployment -Name "dplisvvm$postfix" -ResourceGroupName "$rgName" -TemplateFile "C:\certLocation\VHDtoImage.json" -userStorageAccountName "$storageaccount" -dnsNameForPublicIP "$vmName" -subscriptionId "$mysubid" -location "$location" -vmName "$vmName" -vaultName "$kvname" -vaultResourceGroup "$rgName" -certificateUrl
$objAzureKeyVaultSecret.Id -vhdUrl "$vhdUrl" -vmSize "Standard\_A2" -publicIPAddressName "myPublicIP1" -virtualNetworkName "myVNET1" -nicName "myNIC1" -adminUserName "isv" -adminPassword $pwd"

# deploying VM with existing VHD
New-AzResourceGroupDeployment -Name "dplisvvm$postfix" -ResourceGroupName "$rgName" -TemplateFile "C:\certLocation\VHDtoImage.json" -userStorageAccountName "$storageaccount" -dnsNameForPublicIP "$vmName" -subscriptionId "$mysubid" -location "$location" -vmName "$vmName" -vaultName "$kvname" -vaultResourceGroup "$rgName" -certificateUrl $objAzureKeyVaultSecret.Id -vhdUrl "$vhdUrl" -vmSize "Standard\_A2" -publicIPAddressName "myPublicIP1" -virtualNetworkName "myVNET1" -nicName "myNIC1" -adminUserName "isv" -adminPassword $pwd
```

## <a name="run-validations"></a>Ejecución de validaciones

Hay dos maneras de ejecutar validaciones en la imagen implementada.

### <a name="use-certification-test-tool-for-azure-certified"></a>Usar Certification Test Tool for Azure Certified

#### <a name="download-and-run-the-certification-test-tool"></a>Descargar y ejecutar Certification Test Tool

Certification Test Tool for Azure Certified se ejecuta en un equipo Windows local, pero prueba una máquina virtual Linux o Windows basada en Azure. Verifica que la imagen de máquina virtual de usuario se puede usar con Microsoft Azure y que se han cumplido las instrucciones y los requisitos relacionados con la preparación del VHD.

1. Descargue e instale la versión más reciente de [Certification Test Tool for Azure Certified](https://www.microsoft.com/download/details.aspx?id=44299).
2. Abra la herramienta de certificación y seleccione **Iniciar nueva prueba**.
3. En la pantalla Información de la prueba, escriba un **Nombre de prueba** para la serie de pruebas.
4. Seleccione la plataforma de la máquina virtual, ya sea **Windows Server** o **Linux**. La elección de plataforma afecta a las opciones restantes.
5. Si la máquina virtual usa este servicio de base de datos, seleccione la casilla **Prueba de Azure SQL Database**.

#### <a name="connect-the-certification-tool-to-a-vm-image"></a>Conectar la herramienta de certificación a una imagen de máquina virtual

1. Seleccione el modo Autenticación SSH: Autenticación de contraseña o autenticación de archivo de clave.
2. Si usa la autenticación de contraseña, escriba los valores para **Nombre DNS de máquina virtual** , **Nombre de usuario** y **Contraseña**. También puede cambiar el número predeterminado de Puerto SSH.

    :::image type="content" source="media/vm/azure-vm-cert-2.png" alt-text="Muestra la selección de la información de la prueba de la máquina virtual.":::

3. Si usa autenticación con archivo de clave, escriba valores para Nombre DNS de máquina virtual, Nombre de usuario y Ubicación de clave privada. También puede incluir una Frase de contraseña o cambiar el número predeterminado de Puerto SSH.
4. Escriba el nombre DNS completo de la máquina virtual (por ejemplo, MyVMName.ClOudapp.net).
5. Escriba el **Nombre de usuario** y la **Contraseña**.

    :::image type="content" source="media/vm/azure-vm-cert-4.png" alt-text="Muestra la selección del nombre de usuario y contraseña de la máquina virtual.":::

6. Seleccione **Next** (Siguiente).

#### <a name="run-a-certification-test"></a>Ejecutar una prueba de certificación

Una vez proporcionados los valores de parámetro para la imagen de máquina virtual en la herramienta de certificación, seleccione Probar conexión para crear una conexión válida a la máquina virtual. Una vez verificada la conexión, seleccione **Siguiente** para iniciar la prueba. Una vez completada la prueba, los resultados se muestran en una tabla. La columna Estado se muestra (Correcto/Error/Advertencia) para cada prueba. Si se produce un error en alguna de las pruebas, la imagen no se certifica. En ese caso, revise los requisitos y los mensajes de error, realice los cambios sugeridos y vuelva a ejecutar la prueba.

Una vez completada la prueba automatizada, proporcione información adicional sobre la imagen de máquina virtual en las dos pestañas de la pantalla Cuestionario, es decir, Evaluación general y Personalización del kernel, y, después, seleccione Siguiente.

La última pantalla permite proporcionar más información, como datos de acceso SSH para una imagen de máquina virtual Linux y una explicación sobre las evaluaciones con errores, en caso de que busque excepciones.

Por último, seleccione Generar informe para descargar los resultados de las pruebas y los archivos de registro de los casos de prueba ejecutados, junto con las respuestas al cuestionario. 
> [!Note]
> algunos publicadores tienen escenarios donde es necesario bloquear las máquinas virtuales, ya que tienen software como firewalls instalados en la máquina virtual. En este caso, descargue la [herramienta de prueba certificada](https://aka.ms/AzureCertificationTestTool) aquí y envíe el informe al [soporte técnico](https://aka.ms/marketplacepublishersupport) del Centro de partners.

## <a name="how-to-use-powershell-to-consume-the-self-test-api"></a>Cómo usar PowerShell para consumir la API de prueba automática

### <a name="on-linux-os"></a>En el SO Linux

Llame a la API en PowerShell:

1. Use el comando Invoke-WebRequest para llamar a la API.
2. El método es Post y el tipo de contenido es JSON, tal como se muestra en la siguiente captura de pantalla y el ejemplo de código.
3. Especifique los parámetros del cuerpo en formato JSON.

El ejemplo siguiente muestra una llamada de PowerShell a la API:

```POWERSHELL
$accesstoken = "token"
$headers = @{ "Authorization" = "Bearer $accesstoken" }
$DNSName = "<Machine DNS Name>"
$UserName = "<User ID>"
$Password = "<Password>"
$OS = "Linux"
$PortNo = "22"
$CompanyName = "ABCD"
$AppID = "<Application ID>"
$TenantId = "<Tenant ID>"

$body = @{
   "DNSName" = $DNSName
   "UserName" = $UserName
   "Password" = $Password
   "OS" = $OS
   "PortNo" = $PortNo
   "CompanyName" = $CompanyName
   "AppID" = $AppID
   "TenantId" = $TenantId
} | ConvertTo-Json

$body

$uri = "URL"

$res = (Invoke-WebRequest -Method "Post" -Uri $uri -Body $body -ContentType "application/json" -Headers $headers).Content
```

<br>Este es un ejemplo de llamada a la API en PowerShell:

[![Ejemplo de pantalla de una llamada a la API en PowerShell.](media/vm/call-api-in-powershell.png)](media/vm/call-api-in-powershell.png#lightbox)

<br>Mediante el ejemplo anterior, puede recuperar el archivo JSON y analizarlo para obtener los detalles siguientes:

```PowerShell
$resVar = $res | ConvertFrom-Json
$actualresult = $resVar.Response | ConvertFrom-Json

Write-Host "OSName: $($actualresult.OSName)"
Write-Host "OSVersion: $($actualresult.OSVersion)"
Write-Host "Overall Test Result: $($actualresult.TestResult)"

For ($i = 0; $i -lt $actualresult.Tests.Length; $i++) {
   Write-Host "TestID: $($actualresult.Tests[$i].TestID)"
   Write-Host "TestCaseName: $($actualresult.Tests[$i].TestCaseName)"
   Write-Host "Description: $($actualresult.Tests[$i].Description)"
   Write-Host "Result: $($actualresult.Tests[$i].Result)"
   Write-Host "ActualValue: $($actualresult.Tests[$i].ActualValue)"
}
```

<br>En esta pantalla de ejemplo, que muestra `$res.Content`, se pueden ver los detalles de los resultados de las pruebas en formato JSON:

[![Ejemplo de pantalla de una llamada a la API en PowerShell con los detalles de los resultados de la prueba.](media/vm/call-api-in-powershell-details.png)](media/vm/call-api-in-powershell-details.png#lightbox)

<br>Este es un ejemplo de los resultados JSON de las pruebas que se ven en un visor JSON en línea (por ejemplo, [Code Beautify](https://codebeautify.org/jsonviewer) o [JSON Viewer](https://jsonformatter.org/json-viewer)).

![Más resultados de pruebas en un visor JSON en línea.](media/vm/test-results-json-viewer-1.png)

### <a name="on-windows-os"></a>En el SO Windows

Llame a la API en PowerShell:

1. Use el comando Invoke-WebRequest para llamar a la API.
2. El método es Post y el tipo de contenido es JSON, tal como se muestra en la siguiente pantalla de ejemplo y en el ejemplo de código.
3. Cree los parámetros del cuerpo en formato JSON.

El siguiente ejemplo de código muestra una llamada de PowerShell a la API:

```PowerShell
$accesstoken = "Get token for your Client AAD App"
$headers = @{ "Authorization" = "Bearer $accesstoken" }
$Body = @{ 
   "DNSName" = "XXXX.westus.cloudapp.azure.com"
   "UserName" = "XXX"
   "Password" = "XXX@123456"
   "OS" = "Windows"
   "PortNo" = "5986"
   "CompanyName" = "ABCD"
   "AppID" = "XXXX-XXXX-XXXX"
   "TenantId" = "XXXX-XXXX-XXXX"
} | ConvertTo-Json

$res = Invoke-WebRequest -Method "Post" -Uri $uri -Body $Body -ContentType "application/json" –Headers $headers;
$Content = $res | ConvertFrom-Json
```

Estos ejemplos de pantalla muestra una llamada a la API en PowerShell:

**Con la clave SSH** :

 :::image type="content" source="media/vm/call-api-with-ssh-key.png" alt-text="Llamar a la API en PowerShell con una clave SSH.":::

**Con contraseña** :

 :::image type="content" source="media/vm/call-api-with-password.png" alt-text="Llamar a la API en PowerShell con una contraseña.":::

<br>Mediante el ejemplo anterior, puede recuperar el archivo JSON y analizarlo para obtener los detalles siguientes:

```PowerShell
$resVar = $res | ConvertFrom-Json
$actualresult = $resVar.Response | ConvertFrom-Json

Write-Host "OSName: $($actualresult.OSName)"
Write-Host "OSVersion: $($actualresult.OSVersion)"
Write-Host "Overall Test Result: $($actualresult.TestResult)"

For ($i = 0; $i -lt $actualresult.Tests.Length; $i++) {
   Write-Host "TestID: $($actualresult.Tests[$i].TestID)"
   Write-Host "TestCaseName: $($actualresult.Tests[$i].TestCaseName)"
   Write-Host "Description: $($actualresult.Tests[$i].Description)"
   Write-Host "Result: $($actualresult.Tests[$i].Result)"
   Write-Host "ActualValue: $($actualresult.Tests[$i].ActualValue)"
}
```

<br>En esta pantalla, que muestra `$res.Content`, se pueden ver los detalles de los resultados de las pruebas en formato JSON:

 :::image type="content" source="media/vm/test-results-json-format.png" alt-text="Detalles de los resultados de la prueba en formato JSON.":::

<br>Este es un ejemplo de los resultados de las pruebas que se ven en un visor JSON en línea (por ejemplo, [Code Beautify](https://codebeautify.org/jsonviewer) o [JSON Viewer](https://jsonformatter.org/json-viewer)).

![Resultados de pruebas en un visor JSON en línea.](media/vm/test-results-json-viewer-2.png)

## <a name="how-to-use-curl-to-consume-the-self-test-api-on-linux-os"></a>Cómo usar cURL para consumir la API de prueba automática en el SO Linux

En este ejemplo, se usará curl para realizar una llamada de API POST a Azure Active Directory y la máquina virtual autohospedada.

1. Solicitar un token de Azure AD para autenticarse en una máquina virtual autohospedada

   Asegúrese de que se sustituyen los valores correctos en la solicitud de curl.

   ```
   curl --location --request POST 'https://login.microsoftonline.com/{TENANT_ID}/oauth2/token' \
   --header 'Content-Type: application/x-www-form-urlencoded' \
   --data-urlencode 'grant_type=client_credentials' \
   --data-urlencode 'client_id={CLIENT_ID} ' \
   --data-urlencode 'client_secret={CLIENT_SECRET}' \
   --data-urlencode 'resource=https://management.core.windows.net'
   ```
   Este es un ejemplo de la respuesta de la solicitud:
   ```JSON
   {
       "token_type": "Bearer",
       "expires_in": "86399",
       "ext_expires_in": "86399",
       "expires_on": "1599663998",
       "not_before": "1599577298",
       "resource": "https://management.core.windows.net",
       "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJS…"
   }
   ```

2. Enviar una solicitud para la máquina virtual de prueba automática

   Asegúrese de que el token de portador y los parámetros se sustituyen por los valores correctos.

   ```
   curl --location --request POST 'https://isvapp.azurewebsites.net/selftest-vm' \
   --header 'Content-Type: application/json' \
   --header 'Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJS…' \
   --data-raw '{
       "DNSName": "avvm1.eastus.cloudapp.azure.com",
       "UserName": "azureuser",
       "Password": "SECURE_PASSWORD_FOR_THE_SSH_INTO_VM",
       "OS": "Linux",
       "PortNo": "22",
       "CompanyName": "COMPANY_NAME",
       "AppId": "CLIENT_ID_SAME_AS_USED_FOR_AAD_TOKEN ",
       "TenantId": "TENANT_ID_SAME_AS_USED_FOR_AAD_TOKEN"
   }'
   ```

   Respuesta de ejemplo de la llamada API de máquina virtual de prueba automática
   ```JSON
   {
       "TrackingId": "9bffc887-dd1d-40dd-a8a2-34cee4f4c4c3",
       "Response": "{\"SchemaVersion\":1,\"AppCertificationCategory\":\"Microsoft Single VM Certification\",\"ProviderID\":\"050DE427-2A99-46D4-817C-5354D3BF2AE8\",\"OSName\":\"Ubuntu 18.04\",\"OSDistro\":\"Ubuntu 18.04.5 LTS\",\"KernelVersion\":\"5.4.0-1023-azure\",\"KernelFullVersion\":\"Linux version 5.4.0-1023-azure (buildd@lgw01-amd64-053) (gcc version 7.5.0 (Ubuntu 7.5.0-3ubuntu1~18.04)) #23~18.04.1-Ubuntu SMP Thu Aug 20 14:46:48 UTC 2020\\n\",\"OSVersion\":\"18.04\",\"CreatedDate\":\"09/08/2020 01:13:47\",\"TestResult\":\"Pass\",\"APIVersion\":\"1.5\",\"Tests\":[{\"TestID\":\"48\",\"TestCaseName\":\"Bash History\",\"Description\":\"Bash history files should be cleared before creating the VM image.\",\"Result\":\"Passed\",\"ActualValue\":\"No file Exist\",\"RequiredValue\":\"1024\"},{\"TestID\":\"39\",\"TestCaseName\":\"Linux Agent Version\",\"Description\":\"Azure Linux Agent Version 2.2.41 and above should be installed.\",\"Result\":\"Passed\",\"ActualValue\":\"2.2.49\",\"RequiredValue\":\"2.2.41\"},{\"TestID\":\"40\",\"TestCaseName\":\"Required Kernel Parameters\",\"Description\":\"Verifies the following kernel parameters are set console=ttyS0, earlyprintk=ttyS0, rootdelay=300\",\"Result\":\"Warning\",\"ActualValue\":\"Missing Parameter: rootdelay=300\\r\\nMatched Parameter: console=ttyS0,earlyprintk=ttyS0\",\"RequiredValue\":\"console=ttyS0#earlyprintk=ttyS0#rootdelay=300\"},{\"TestID\":\"41\",\"TestCaseName\":\"Swap Partition on OS Disk\",\"Description\":\"Verifies that no Swap partitions are created on the OS disk.\",\"Result\":\"Passed\",\"ActualValue\":\"No. of Swap Partitions: 0\",\"RequiredValue\":\"swap\"},{\"TestID\":\"42\",\"TestCaseName\":\"Root Partition on OS Disk\",\"Description\":\"It is recommended that a single root partition is created for the OS disk.\",\"Result\":\"Passed\",\"ActualValue\":\"Root Partition: 1\",\"RequiredValue\":\"1\"},{\"TestID\":\"44\",\"TestCaseName\":\"OpenSSL Version\",\"Description\":\"OpenSSL Version should be >=0.9.8.\",\"Result\":\"Passed\",\"ActualValue\":\"1.1.1\",\"RequiredValue\":\"0.9.8\"},{\"TestID\":\"45\",\"TestCaseName\":\"Python Version\",\"Description\":\"Python version 2.6+ is highly recommended. \",\"Result\":\"Passed\",\"ActualValue\":\"2.7.17\",\"RequiredValue\":\"2.6\"},{\"TestID\":\"46\",\"TestCaseName\":\"Client Alive Interval\",\"Description\":\"It is recommended to set ClientAliveInterval to 180. On the application need, it can be set between 30 to 235. \\nIf you are enabling the SSH for your end users this value must be set as explained.\",\"Result\":\"Warning\",\"ActualValue\":\"120\",\"RequiredValue\":\"ClientAliveInterval 180\"},{\"TestID\":\"49\",\"TestCaseName\":\"OS Architecture\",\"Description\":\"Only 64-bit operating system should be supported.\",\"Result\":\"Passed\",\"ActualValue\":\"x86_64\\n\",\"RequiredValue\":\"x86_64,amd64\"},{\"TestID\":\"50\",\"TestCaseName\":\"Security threats\",\"Description\":\"Identifies OS with recent high profile vulnerability that may need patching.  Ignore warning if system was patched as appropriate.\",\"Result\":\"Passed\",\"ActualValue\":\"Ubuntu 18.04\",\"RequiredValue\":\"OS impacted by GHOSTS\"},{\"TestID\":\"51\",\"TestCaseName\":\"Auto Update\",\"Description\":\"Identifies if Linux Agent Auto Update is enabled or not.\",\"Result\":\"Passed\",\"ActualValue\":\"# AutoUpdate.Enabled=y\\n\",\"RequiredValue\":\"Yes\"},{\"TestID\":\"52\",\"TestCaseName\":\"SACK Vulnerability patch verification\",\"Description\":\"Checks if the running Kernel Version has SACK vulnerability patch.\",\"Result\":\"Passed\",\"ActualValue\":\"Ubuntu 18.04.5 LTS,Linux version 5.4.0-1023-azure (buildd@lgw01-amd64-053) (gcc version 7.5.0 (Ubuntu 7.5.0-3ubuntu1~18.04)) #23~18.04.1-Ubuntu SMP Thu Aug 20 14:46:48 UTC 2020\",\"RequiredValue\":\"Yes\"}]}"
   }
   ```

## <a name="next-steps"></a>Pasos siguientes

- [Obtención del identificador URI de SAS](azure-vm-get-sas-uri.md)
