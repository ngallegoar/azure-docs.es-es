---
title: Utilizar Azure Image Builder con una galería de imágenes para máquinas virtuales Windows (versión preliminar)
description: Cree versiones de imágenes de Azure Shared Gallery mediante Azure Image Builder and Azure PowerShell.
author: cynthn
ms.author: cynthn
ms.date: 01/14/2020
ms.topic: article
ms.service: virtual-machines-windows
manager: gwallace
ms.openlocfilehash: d5856780d0d9f1a1943bca1c2f076bb3ec914e1d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "76263360"
---
# <a name="preview-create-a-windows-image-and-distribute-it-to-a-shared-image-gallery"></a>Vista previa: Creación de una imagen de Windows y distribución en una galería de imágenes compartidas 

En este artículo se muestra cómo puede usar Azure Image Builder y Azure PowerShell para crear una versión de una imagen en una instancia de [Shared Image Gallery](shared-image-galleries.md) y después distribuirla globalmente. También puede hacerlo mediante la [CLI de Azure](../linux/image-builder-gallery.md).

Se usará una plantilla .json para configurar la imagen. El archivo .json que se usa aquí es: [armTemplateWinSIG.json](https://raw.githubusercontent.com/danielsollondon/azvmimagebuilder/master/quickquickstarts/1_Creating_a_Custom_Win_Shared_Image_Gallery_Image/armTemplateWinSIG.json). Descargaremos y editaremos una versión local de la plantilla, por lo que este artículo se escribe con la sesión local de PowerShell.

Para distribuir la imagen en una galería de imágenes compartidas, en la plantilla se usa [sharedImage](../linux/image-builder-json.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json#distribute-sharedimage) como valor de la sección `distribute` de la plantilla.

Azure Image Builder ejecuta automáticamente sysprep para generalizar la imagen; consiste en un comando sysprep genérico que puede [reemplazar](https://github.com/danielsollondon/azvmimagebuilder/blob/master/troubleshootingaib.md#vms-created-from-aib-images-do-not-create-successfully) si es necesario. 

Tenga en cuenta el número de veces que pone una capa en las personalizaciones. Puede ejecutar el comando Sysprep hasta ocho veces en una sola imagen de Windows. Después de ejecutar Sysprep ocho veces, debe volver a crear la imagen de Windows. Para más información, consulte [Límites en la cantidad de veces que se puede ejecutar Sysprep](https://docs.microsoft.com/windows-hardware/manufacture/desktop/sysprep--generalize--a-windows-installation#limits-on-how-many-times-you-can-run-sysprep). 

> [!IMPORTANT]
> Actualmente, el generador de imágenes de Azure se encuentra en versión preliminar pública.
> Esta versión preliminar se ofrece sin Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas. Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="register-the-features"></a>Registro de las características
Para usar el generador de imágenes de Azure durante la versión preliminar, tendrá que registrar la nueva característica.

```powershell
Register-AzProviderFeature -FeatureName VirtualMachineTemplatePreview -ProviderNamespace Microsoft.VirtualMachineImages
```

Compruebe el estado del registro de la característica.

```powershell
Get-AzProviderFeature -FeatureName VirtualMachineTemplatePreview -ProviderNamespace Microsoft.VirtualMachineImages
```

Espere a que el estado de `RegistrationState` sea `Registered` antes de continuar con el paso siguiente.

Compruebe los registros del proveedor. Asegúrese de que cada uno devuelve `Registered`.

```powershell
Get-AzResourceProvider -ProviderNamespace Microsoft.VirtualMachineImages | Format-table -Property ResourceTypes,RegistrationState
Get-AzResourceProvider -ProviderNamespace Microsoft.Storage | Format-table -Property ResourceTypes,RegistrationState 
Get-AzResourceProvider -ProviderNamespace Microsoft.Compute | Format-table -Property ResourceTypes,RegistrationState
Get-AzResourceProvider -ProviderNamespace Microsoft.KeyVault | Format-table -Property ResourceTypes,RegistrationState
```

Si no devuelven `Registered`, utilice lo siguiente para registrar los proveedores:

```powershell
Register-AzResourceProvider -ProviderNamespace Microsoft.VirtualMachineImages
Register-AzResourceProvider -ProviderNamespace Microsoft.Storage
Register-AzResourceProvider -ProviderNamespace Microsoft.Compute
Register-AzResourceProvider -ProviderNamespace Microsoft.KeyVault
```

## <a name="create-variables"></a>Creación de variables

Usaremos algunos datos de forma repetida, por lo que crearemos diversas variables para almacenar esa información. Reemplace los valores de las variables, como `username` y `vmpassword`, por información propia.

```powershell
# Get existing context
$currentAzContext = Get-AzContext

# Get your current subscription ID. 
$subscriptionID=$currentAzContext.Subscription.Id

# Destination image resource group
$imageResourceGroup="aibwinsig"

# Location
$location="westus"

# Image distribution metadata reference name
$runOutputName="aibCustWinManImg02ro"

# Image template name
$imageTemplateName="helloImageTemplateWin02ps"

# Distribution properties object name (runOutput).
# This gives you the properties of the managed image on completion.
$runOutputName="winclientR01"
```



## <a name="create-the-resource-group"></a>Creación del grupo de recursos

Cree un grupo de recursos y conceda a Azure Image Builder permiso para crear recursos en ese grupo.

```powershell
New-AzResourceGroup `
   -Name $imageResourceGroup `
   -Location $location
New-AzRoleAssignment `
   -ObjectId ef511139-6170-438e-a6e1-763dc31bdf74 `
   -Scope /subscriptions/$subscriptionID/resourceGroups/$imageResourceGroup `
   -RoleDefinitionName Contributor
```



## <a name="create-the-shared-image-gallery"></a>Creación de la instancia de Shared Image Gallery

Para usar Image Builder con una galería de imágenes compartidas, debe tener ya una galería de imágenes y una definición de imagen. Image Builder no creará la galería de imágenes ni la definición de imagen automáticamente.

Si aún no tiene una galería y una definición de imagen para usar, empiece por crearlas. En primer lugar, cree una galería de imágenes.

```powershell
# Image gallery name
$sigGalleryName= "myIBSIG"

# Image definition name
$imageDefName ="winSvrimage"

# additional replication region
$replRegion2="eastus"

# Create the gallery
New-AzGallery `
   -GalleryName $sigGalleryName `
   -ResourceGroupName $imageResourceGroup  `
   -Location $location

# Create the image definition
New-AzGalleryImageDefinition `
   -GalleryName $sigGalleryName `
   -ResourceGroupName $imageResourceGroup `
   -Location $location `
   -Name $imageDefName `
   -OsState generalized `
   -OsType Windows `
   -Publisher 'myCompany' `
   -Offer 'WindowsServer' `
   -Sku 'WinSrv2019'
```



## <a name="download-and-configure-the-template"></a>Descarga y configuración de la plantilla

Descargue la plantilla .json y configúrela con las variables.

```powershell

$templateFilePath = "armTemplateWinSIG.json"

Invoke-WebRequest `
   -Uri "https://raw.githubusercontent.com/danielsollondon/azvmimagebuilder/master/quickquickstarts/1_Creating_a_Custom_Win_Shared_Image_Gallery_Image/armTemplateWinSIG.json" `
   -OutFile $templateFilePath `
   -UseBasicParsing

(Get-Content -path $templateFilePath -Raw ) `
   -replace '<subscriptionID>',$subscriptionID | Set-Content -Path $templateFilePath
(Get-Content -path $templateFilePath -Raw ) `
   -replace '<rgName>',$imageResourceGroup | Set-Content -Path $templateFilePath
(Get-Content -path $templateFilePath -Raw ) `
   -replace '<runOutputName>',$runOutputName | Set-Content -Path $templateFilePath
(Get-Content -path $templateFilePath -Raw ) `
   -replace '<imageDefName>',$imageDefName | Set-Content -Path $templateFilePath
(Get-Content -path $templateFilePath -Raw ) `
   -replace '<sharedImageGalName>',$sigGalleryName | Set-Content -Path $templateFilePath
(Get-Content -path $templateFilePath -Raw ) `
   -replace '<region1>',$location | Set-Content -Path $templateFilePath
(Get-Content -path $templateFilePath -Raw ) `
   -replace '<region2>',$replRegion2 | Set-Content -Path $templateFilePath

```


## <a name="create-the-image-version"></a>Creación de la versión de la imagen

La plantilla debe enviarse al servicio, se descargarán todos los artefactos dependientes, como los scripts, y se almacenarán en el grupo de recursos de almacenamiento provisional, con el prefijo *IT_* .

```powershell
New-AzResourceGroupDeployment `
   -ResourceGroupName $imageResourceGroup `
   -TemplateFile $templateFilePath `
   -api-version "2019-05-01-preview" `
   -imageTemplateName $imageTemplateName `
   -svclocation $location
```

Para compilar la imagen, debe invocar "Run" (Ejecutar) en la plantilla.

```powershell
Invoke-AzResourceAction `
   -ResourceName $imageTemplateName `
   -ResourceGroupName $imageResourceGroup `
   -ResourceType Microsoft.VirtualMachineImages/imageTemplates `
   -ApiVersion "2019-05-01-preview" `
   -Action Run
```

La creación de la imagen y su replicación en las dos regiones puede llevar un tiempo. Espere a que termine esta parte antes de pasar a la creación de una máquina virtual.

Para información sobre las opciones para automatizar la obtención del estado de compilación de la imagen, consulte el documento [Léame](https://github.com/danielsollondon/azvmimagebuilder/blob/master/quickquickstarts/1_Creating_a_Custom_Win_Shared_Image_Gallery_Image/readme.md#get-status-of-the-image-build-and-query) de esta plantilla en GitHub.


## <a name="create-the-vm"></a>Creación de la máquina virtual

Cree una máquina virtual a partir la versión de la imagen creada por Azure Image Builder.

Obtenga la versión de la imagen que ha creado.
```powershell
$imageVersion = Get-AzGalleryImageVersion `
   -ResourceGroupName $imageResourceGroup `
   -GalleryName $sigGalleryName `
   -GalleryImageDefinitionName $imageDefName
```

Cree la máquina virtual en la segunda región en la que se ha replicado la imagen.

```powershell
$vmResourceGroup = "myResourceGroup"
$vmName = "myVMfromImage"

# Create user object
$cred = Get-Credential -Message "Enter a username and password for the virtual machine."

# Create a resource group
New-AzResourceGroup -Name $vmResourceGroup -Location $replRegion2

# Network pieces
$subnetConfig = New-AzVirtualNetworkSubnetConfig -Name mySubnet -AddressPrefix 192.168.1.0/24
$vnet = New-AzVirtualNetwork -ResourceGroupName $vmResourceGroup -Location $replRegion2 `
  -Name MYvNET -AddressPrefix 192.168.0.0/16 -Subnet $subnetConfig
$pip = New-AzPublicIpAddress -ResourceGroupName $vmResourceGroup -Location $replRegion2 `
  -Name "mypublicdns$(Get-Random)" -AllocationMethod Static -IdleTimeoutInMinutes 4
$nsgRuleRDP = New-AzNetworkSecurityRuleConfig -Name myNetworkSecurityGroupRuleRDP  -Protocol Tcp `
  -Direction Inbound -Priority 1000 -SourceAddressPrefix * -SourcePortRange * -DestinationAddressPrefix * `
  -DestinationPortRange 3389 -Access Allow
$nsg = New-AzNetworkSecurityGroup -ResourceGroupName $vmResourceGroup -Location $replRegion2 `
  -Name myNetworkSecurityGroup -SecurityRules $nsgRuleRDP
$nic = New-AzNetworkInterface -Name myNic -ResourceGroupName $vmResourceGroup -Location $replRegion2 `
  -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id -NetworkSecurityGroupId $nsg.Id

# Create a virtual machine configuration using $imageVersion.Id to specify the shared image
$vmConfig = New-AzVMConfig -VMName $vmName -VMSize Standard_D1_v2 | `
Set-AzVMOperatingSystem -Windows -ComputerName $vmName -Credential $cred | `
Set-AzVMSourceImage -Id $imageVersion.Id | `
Add-AzVMNetworkInterface -Id $nic.Id

# Create a virtual machine
New-AzVM -ResourceGroupName $vmResourceGroup -Location $replRegion2 -VM $vmConfig
```

## <a name="verify-the-customization"></a>Comprobación de la personalización
Cree una conexión de Escritorio remoto a la máquina virtual con el nombre de usuario y la contraseña que ha establecido al crear la máquina virtual. Dentro de la máquina virtual, abra un símbolo del sistema y escriba lo siguiente:

```console
dir c:\
```

Debería ver un directorio con el nombre `buildActions`, que se ha creado durante la personalización de la imagen.


## <a name="clean-up-resources"></a>Limpieza de recursos
Si ahora quiere volver a personalizar la versión de la imagen para crear una nueva de la misma imagen, **omita este paso** y vaya a [Uso de Azure Image Builder para crear otra versión de la imagen](image-builder-gallery-update-image-version.md).


Esta acción eliminará la imagen que se ha creado, junto con todos los demás archivos de recursos. Asegúrese de que haya terminado con esta implementación antes de eliminar los recursos.

Elimine primero la plantilla del grupo de recursos, ya que de lo contrario no se limpiará el grupo de recursos de almacenamiento provisional (*IT_* ) usado por AIB.

Obtenga el elemento ResourceID de la plantilla de la imagen. 

```powerShell
$resTemplateId = Get-AzResource -ResourceName $imageTemplateName -ResourceGroupName $imageResourceGroup -ResourceType Microsoft.VirtualMachineImages/imageTemplates -ApiVersion "2019-05-01-preview"
```

Elimine la plantilla de la imagen.

```powerShell
Remove-AzResource -ResourceId $resTemplateId.ResourceId -Force
```

Elimine el grupo de recursos.

```powerShell
Remove-AzResourceGroup $imageResourceGroup -Force
```

## <a name="next-steps"></a>Pasos siguientes

Para obtener información sobre cómo actualizar la versión de la imagen que ha creado, vea [Uso de Azure Image Builder para crear otra versión de la imagen](image-builder-gallery-update-image-version.md).
