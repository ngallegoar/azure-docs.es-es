---
title: archivo de inclusión
description: archivo
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: include
author: mingshen-ms
ms.author: krsh
ms.date: 10/20/2020
ms.openlocfilehash: 24adbfe38a3d43a83307fb8726849f7c73def3f3
ms.sourcegitcommit: b6f3ccaadf2f7eba4254a402e954adf430a90003
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/20/2020
ms.locfileid: "92283378"
---
## <a name="generalize-the-image"></a>Generalizar la imagen

Todas las imágenes de Azure Marketplace deben ser reutilizables de forma genérica. Para lograrlo, el VHD del sistema operativo debe estar generalizado, una operación que quita todos los controladores de software y los identificadores específicos de la instancia de una máquina virtual.

### <a name="for-windows"></a>Para Windows

Los discos de sistema operativo Windows se generalizan con la herramienta [sysprep](https://docs.microsoft.com/windows-hardware/manufacture/desktop/sysprep--system-preparation--overview). Si más adelante actualiza o vuelve a configurar el sistema operativo, debe ejecutar sysprep de nuevo.

> [!WARNING]
> Después de ejecutar sysprep, desactive la máquina virtual hasta que se implemente, ya que las actualizaciones pueden ejecutarse automáticamente. Este apagado evita que las actualizaciones posteriores realicen cambios específicos de la instancia en el sistema operativo o los servicios instalados. Para más información sobre la ejecución de sysprep, vea [Pasos para generalizar un disco duro virtual](../../virtual-machines/windows/capture-image-resource.md#generalize-the-windows-vm-using-sysprep).

### <a name="for-linux"></a>Para Linux

El siguiente proceso generaliza una máquina virtual Linux y la vuelve a implementar como una máquina virtual independiente. Para obtener información detallada, vea [Creación de una imagen de una máquina virtual o un disco duro virtual](../../virtual-machines/linux/capture-image.md). Puede detenerse cuando llegue a la sección denominada "Crear una máquina virtual a partir de la imagen capturada".

1. Quite el agente Linux de Azure.
    1. Conexión a una máquina virtual Linux de Azure mediante un cliente SSH.
    2. En la ventana de SSH, escriba este comando: `sudo waagent –deprovision+user`.
    3. Escriba Y para continuar (puede agregar el parámetro -force al comando anterior para evitar el paso de confirmación).
    4. Una vez finalizado el comando, escriba **Exit** para cerrar el cliente de SSH.
2. Detenga la máquina virtual.
    1. En Azure Portal, seleccione el grupo de recursos (RG) y anule la asignación de la máquina virtual.
    2. La máquina virtual ahora se ha generalizado y puede crear una nueva mediante este disco de máquina virtual.

### <a name="take-a-snapshot-of-the-vm-disk"></a>Toma de una instantánea del disco de máquina virtual

1. Inicie sesión en [Azure Portal](https://ms.portal.azure.com/).
2. En el menú superior izquierdo, seleccione **Crear un recurso** y, a continuación, busque y seleccione **Instantánea** .
3. En la hoja Instantánea, seleccione **Crear** .
4. Escriba un **nombre** para la instantánea.
5. Seleccione un grupo de recursos existente o escriba el nombre de uno nuevo.
6. Como **disco de origen** , seleccione el disco administrado para la instantánea.
7. Seleccione el **tipo de cuenta** que se usará para almacenar la instantánea. Use **HDD estándar** , a menos que necesite almacenarla en un disco SSD de alto rendimiento.
8. Seleccione **Crear** .

#### <a name="extract-the-vhd"></a>Extracción del disco duro

Use el siguiente script para exportar la instantánea a un disco duro virtual de la cuenta de almacenamiento.

```JSON
#Provide the subscription Id where the snapshot is created
subscriptionId=yourSubscriptionId

#Provide the name of your resource group where the snapshot is created
resourceGroupName=myResourceGroupName

#Provide the snapshot name
snapshotName=mySnapshot

#Provide Shared Access Signature (SAS) expiry duration in seconds (such as 3600)
#Know more about SAS here: https://docs.microsoft.com/en-us/azure/storage/storage-dotnet-shared-access-signature-part-1
sasExpiryDuration=3600

#Provide storage account name where you want to copy the underlying VHD file. 
storageAccountName=mystorageaccountname

#Name of the storage container where the downloaded VHD will be stored.
storageContainerName=mystoragecontainername

#Provide the key of the storage account where you want to copy the VHD 
storageAccountKey=mystorageaccountkey

#Give a name to the destination VHD file to which the VHD will be copied.
destinationVHDFileName=myvhdfilename.vhd

az account set --subscription $subscriptionId

sas=$(az snapshot grant-access --resource-group $resourceGroupName --name $ snapshotName --duration-in-seconds $sasExpiryDuration --query [accessSas] -o tsv)

az storage blob copy start --destination-blob $destinationVHDFileName --destination-container $storageContainerName --account-name $storageAccountName --account-key $storageAccountKey --source-uri $sas
```

#### <a name="script-explanation"></a>Explicación del script

En este script se usan los siguientes comandos para generar el URI de SAS de una instantánea y se copia el disco duro virtual (VHD) subyacente en una cuenta de almacenamiento mediante dicho URI. Cada comando de la tabla crea un vínculo a documentación específica del comando.

| Get-Help | Notas |
| --- | --- |
| az disk grant-access | Genera la SAS de solo lectura que se usa para copiar el archivo de VHD subyacente en una cuenta de almacenamiento o descargarlo localmente.
| az storage blob copy start | Copia un blob de forma asincrónica desde una cuenta de almacenamiento a otra. Use `az storage blob show` para comprobar el estado del nuevo blob. |
|
