---
title: Implementación de una máquina virtual a partir de discos duros virtuales para Azure Marketplace
description: Se explica cómo registrar una máquina virtual desde un disco duro virtual implementado en Azure.
author: qianw211
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 08/08/2019
ms.author: dsindona
ms.openlocfilehash: f13e4066137e0d76612040d9f6e5ff3d0aa399c8
ms.sourcegitcommit: 530e2d56fc3b91c520d3714a7fe4e8e0b75480c8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/14/2020
ms.locfileid: "81273909"
---
# <a name="deploy-a-vm-from-your-vhds"></a>Implementación de una máquina virtual a partir de VHD

> [!IMPORTANT]
> A partir del 13 de abril de 2020, comenzaremos el traslado de la administración de las ofertas de máquinas virtuales de Azure al Centro de partners. Después de la migración, las ofertas se crearán y administrarán en el Centro de partners. Siga las instrucciones que se indican en [Creación de una oferta de máquina virtual de Azure](https://aka.ms/AzureVMTechAsset) para administrar las ofertas migradas.

En esta sección se explica cómo implementar una máquina virtual (VM) a partir de un disco duro virtual (VHD) implementado en Azure.  Se enumeran las herramientas necesarias y cómo usarlas para crear una imagen de VM de usuario que luego se implementa en Azure mediante scripts de PowerShell.

Una vez que los discos duros virtuales (VHD) se han cargado (el VHD del sistema operativo generalizado y cero o más VHD de discos de datos) en la cuenta de almacenamiento de Azure, puede registrarlos como una imagen de máquina virtual de usuario. A continuación, puede probar esa imagen. Puesto que el VHD del sistema operativo está generalizado, no puede implementar la máquina virtual directamente al proporcionar la dirección URL del VHD.

Para obtener más información sobre las imágenes de máquina virtual, vea las siguientes entradas de blog:

- [Imagen de máquina virtual](https://azure.microsoft.com/blog/vm-image-blog-post/)
- [VM Image PowerShell 'How To'](https://azure.microsoft.com/blog/vm-image-powershell-how-to-blog-post/) (Procedimientos para trabajar con imágenes de máquina virtual en PowerShell)

[!INCLUDE [updated-for-az](../../../../includes/updated-for-az.md)]

## <a name="prerequisite-install-the-necessary-tools"></a>Requisito previo: instalar las herramientas necesarias

Si aún no lo ha hecho, instale Azure PowerShell y la CLI de Azure mediante las siguientes instrucciones:

- [Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-Az-ps)
- [Instalación de la CLI de Azure](https://docs.microsoft.com/cli/azure/install-azure-cli)


## <a name="deployment-steps"></a>Pasos de implementación

Seguirá los pasos a continuación para crear e implementar una imagen de VM de usuario:

1. Cree la imagen de VM de usuario, lo que implica la captura y generalización de la imagen. 
2. Cree certificados y almacénelos en una nueva instancia de Azure Key Vault. Es necesario un certificado para establecer una conexión WinRM segura a la VM.  Se proporcionan una plantilla de Azure Resource Manager y un script de Azure PowerShell. 
3. Implemente la VM desde una imagen de VM de usuario, mediante la plantilla y el script proporcionados.

Una vez implementada la VM, está listo para [certificar la imagen de VM](./cpp-certify-vm.md).

1. Haga clic en **Nuevo** y busque **Implementación de plantillas**, luego seleccione **Build your own template in Editor** (Compilar plantilla propia en el editor).  <br/>
   ![Compilación de plantilla de implementación de disco duro virtual en Azure Portal](./media/publishvm_021.png)

1. Copie y pegue esta [plantilla JSON](./cpp-deploy-json-template.md) en el editor y haga clic en **Guardar**. <br/>
   ![Guardado de plantilla de implementación de disco duro virtual en Azure Portal](./media/publishvm_022.png)

1. Proporcione los valores de parámetro de las páginas de propiedades **Implementación personalizada** que se muestran.

   <table> <tr> <td valign="top"> <img src="./media/publishvm_023.png" alt="Custom deployment property page 1"> </td> <td valign="top"> <img src="./media/publishvm_024.png" alt="Custom deployment property page 2"> </td> </tr> </table> <br/> 

   |  **Parámetro**              |   **Descripción**                                                            |
   |  -------------              |   ---------------                                                            |
   | User Storage Account Name (Nombre de usuario de la cuenta de almacenamiento)   | Nombre de la cuenta de almacenamiento donde se encuentra el disco duro virtual generalizado                    |
   | User Storage Container Name (Nombre de usuario del contenedor de almacenamiento) | Nombre del contenedor donde se encuentra el disco duro virtual generalizado                          |
   | DNS Name for Public IP (Nombre DNS para IP pública)      | Nombre DNS de IP pública. El nombre DNS es el de la máquina virtual, lo definirá en Azure Portal una vez implementada la oferta.  |
   | Nombre de usuario administrador             | Nombre de usuario de la cuenta de administrador de la nueva máquina virtual                                  |
   | Contraseña de administrador              | Contraseña de la cuenta de administrador de la nueva máquina virtual                                  |
   | Tipo de SO                     | Sistema operativo de la máquina virtual: `Windows` \| `Linux`                                    |
   | Id. de suscripción             | Identificador de la suscripción seleccionada                                      |
   | Location                    | Ubicación geográfica de la implementación                                        |
   | Tamaño de VM                     | [Tamaño de la máquina virtual de Azure](https://docs.microsoft.com/azure/virtual-machines/windows/sizes), por ejemplo `Standard_A2` |
   | Nombre de dirección IP pública      | Nombre de la dirección IP pública                                               |
   | Nombre de la máquina virtual                     | Nombre de la nueva máquina virtual                                                           |
   | El nombre de la red virtual        | Nombre de la red virtual que usa la máquina virtual                                   |
   | Nombre de NIC                    | Nombre de la tarjeta de interfaz de red que ejecuta la red virtual               |
   | URL del VHD                     | Dirección URL completa del VHD del disco del sistema operativo                                                     |
   |  |  |
            
1. Después de proporcionar estos valores, haga clic en **Adquirir**. 

Azure comienza la implementación: crea una nueva máquina virtual con el VHD no administrado especificado en la ruta de acceso de la cuenta de almacenamiento especificada.  Puede realizar un seguimiento del progreso en Azure Portal si hace clic en **Máquinas virtuales** en el lado izquierdo del portal.  Cuando se ha creado la máquina virtual, el estado cambia de `Starting` a `Running`. 


### <a name="deploy-a-vm-from-powershell"></a>Implementar una máquina virtual desde PowerShell

Para implementar una máquina virtual de gran tamaño a partir de la imagen de máquina virtual generalizada que acaba de crear, use los siguientes cmdlets.

``` powershell
    $img = Get-AzureVMImage -ImageName "myVMImage"
    $user = "user123"
    $pass = "adminPassword123"
    $myVM = New-AzureVMConfig -Name "VMImageVM" -InstanceSize "Large" -ImageName $img.ImageName | Add-AzureProvisioningConfig -Windows -AdminUsername $user -Password $pass
    New-AzureVM -ServiceName "VMImageCloudService" -VMs $myVM -Location "West US" -WaitForBoot
```


## <a name="next-steps"></a>Pasos siguientes

A continuación, [creará una imagen de VM de usuario](cpp-create-user-image.md) para su solución.

