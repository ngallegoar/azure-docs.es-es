---
title: Comprobación del estado de cifrado para Linux
description: En este artículo se proporcionan instrucciones para comprobar el estado de cifrado del nivel de plataforma y del sistema operativo.
author: kailashmsft
ms.service: security
ms.topic: article
ms.author: kaib
ms.date: 03/11/2020
ms.custom: seodec18
ms.openlocfilehash: 0aaa32c46d915eafffcfac9d95cfdd3a24d4086d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80123422"
---
# <a name="how-to-verify-encryption-status-for-linux"></a>Comprobación del estado de cifrado para Linux 

**Este escenario se aplica a las extensiones ADE de pase único y de pase doble.**  
El ámbito de este documento es validar el estado de cifrado de una máquina virtual mediante distintos métodos.

### <a name="environment"></a>Entorno

- Distribuciones de Linux

### <a name="procedure"></a>Procedimiento

Una máquina virtual se ha cifrado mediante el pase único o el pase doble.

El estado de cifrado se puede validar durante o después del cifrado mediante métodos diferentes.

>[!NOTE] 
>A lo largo del documento se usarán variables, por lo que debe reemplazar los valores según corresponda.

### <a name="verification"></a>Comprobación

La comprobación se puede realizar desde el portal, PowerShell, la CLI de Azure y o desde el lado del sistema operativo de la máquina virtual. 

Esta verificación se puede realizar mediante la comprobación de los discos conectados a una máquina virtual determinada. 

O bien, consultando la configuración de cifrado en cada disco individual si el disco está conectado o no.

A continuación se indican los diferentes métodos de validaciones:

## <a name="using-the-portal"></a>Uso del portal

Valide el estado de cifrado mediante la comprobación de la sección de extensiones en Azure Portal.

Dentro de la sección **Extensiones**, verá la extensión ADE. 

Haga clic en ella y eche un vistazo al **mensaje de estado**; indicará el estado de cifrado actual:

![Comprobación del portal número 1](./media/disk-encryption/verify-encryption-linux/portal-check-001.png)

En la lista de extensiones, verá la versión de la extensión ADE correspondiente. La versión 0.x corresponde al pase doble de ADE y la versión 1.x corresponde al pase único de ADE.

Puede obtener más detalles haciendo clic en la extensión y, a continuación, en *Ver estado detallado*.

Verá un estado más detallado del proceso de cifrado en formato JSON:

![Comprobación del portal número 2](./media/disk-encryption/verify-encryption-linux/portal-check-002.png)

![Comprobación del portal número 3](./media/disk-encryption/verify-encryption-linux/portal-check-003.png)

Otra forma de validar el estado de cifrado es echar un vistazo a la sección **Discos**.

![Comprobación del portal número 4](./media/disk-encryption/verify-encryption-linux/portal-check-004.png)

>[!NOTE] 
> Este estado significa que los discos tienen una configuración de cifrado marcada pero no que se cifraron realmente en el nivel de sistema operativo. Por diseño, los discos se marcan primero y se cifran más adelante. Si se produce un error en el proceso de cifrado, los discos pueden acabar marcados pero no cifrados. Para confirmar que los discos están realmente cifrados, puede comprobar el cifrado de cada disco en el nivel de sistema operativo.

## <a name="using-powershell"></a>Usar PowerShell

Puede validar el estado de cifrado **general** de una máquina virtual cifrada con los siguientes comandos de PowerShell:

```azurepowershell
   $VMNAME="VMNAME"
   $RGNAME="RGNAME"
   Get-AzVmDiskEncryptionStatus -ResourceGroupName  ${RGNAME} -VMName ${VMNAME}
```
![comprobación de PowerShell 1](./media/disk-encryption/verify-encryption-linux/verify-status-ps-01.png)

Puede capturar la configuración de cifrado de cada disco individual mediante los siguientes comandos de PowerShell:

### <a name="single-pass"></a>Paso único
Si se trata de un paso único, la configuración de cifrado se marca en cada uno de los discos (sistema operativo y datos), puede capturar la configuración de cifrado del disco del sistema operativo en un paso único de la manera siguiente:

``` powershell
$RGNAME = "RGNAME"
$VMNAME = "VMNAME"

$VM = Get-AzVM -Name ${VMNAME} -ResourceGroupName ${RGNAME}  
 $Sourcedisk = Get-AzDisk -ResourceGroupName ${RGNAME} -DiskName $VM.StorageProfile.OsDisk.Name
 Write-Host "============================================================================================================================================================="
 Write-Host "Encryption Settings:"
 Write-Host "============================================================================================================================================================="
 Write-Host "Enabled:" $Sourcedisk.EncryptionSettingsCollection.Enabled
 Write-Host "Version:" $Sourcedisk.EncryptionSettingsCollection.EncryptionSettingsVersion
 Write-Host "Source Vault:" $Sourcedisk.EncryptionSettingsCollection.EncryptionSettings.DiskEncryptionKey.SourceVault.Id
 Write-Host "Secret URL:" $Sourcedisk.EncryptionSettingsCollection.EncryptionSettings.DiskEncryptionKey.SecretUrl
 Write-Host "Key URL:" $Sourcedisk.EncryptionSettingsCollection.EncryptionSettings.KeyEncryptionKey.KeyUrl
 Write-Host "============================================================================================================================================================="
```
![Comprobación del pase único del sistema operativo 01](./media/disk-encryption/verify-encryption-linux/verify-os-single-ps-001.png)

Si el disco no tiene la configuración de cifrado marcada, la salida estará vacía como se muestra a continuación:

![Configuración de cifrado de sistema operativo 2](./media/disk-encryption/verify-encryption-linux/os-encryption-settings-2.png)

Captura de la configuración de cifrado de discos de datos:

```azurepowershell
$RGNAME = "RGNAME"
$VMNAME = "VMNAME"

$VM = Get-AzVM -Name ${VMNAME} -ResourceGroupName ${RGNAME}
 clear
 foreach ($i in $VM.StorageProfile.DataDisks|ForEach-Object{$_.Name})
 {
 Write-Host "============================================================================================================================================================="
 Write-Host "Encryption Settings:"
 Write-Host "============================================================================================================================================================="
 Write-Host "Checking Disk:" $i
 $Disk=(Get-AzDisk -ResourceGroupName ${RGNAME} -DiskName $i)
 Write-Host "Encryption Enable: " $Sourcedisk.EncryptionSettingsCollection.Enabled
 Write-Host "Encryption KeyEncryptionKey: " $Sourcedisk.EncryptionSettingsCollection.EncryptionSettings.KeyEncryptionKey.KeyUrl;
 Write-Host "Encryption DiskEncryptionKey: " $Sourcedisk.EncryptionSettingsCollection.EncryptionSettings.DiskEncryptionKey.SecretUrl;
 Write-Host "============================================================================================================================================================="
 }
```
![Comprobación de datos de pase único 001](./media/disk-encryption/verify-encryption-linux/verify-data-single-ps-001.png)

### <a name="dual-pass"></a>Pase dual
En el pase dual, la configuración de cifrado se marca en el modelo de máquina virtual y no en cada disco individual.

Para comprobar que la configuración de cifrado se ha marcado en el pase doble, puede usar los siguientes comandos:

```azurepowershell
$RGNAME = "RGNAME"
$VMNAME = "VMNAME"

$vm = Get-AzVm -ResourceGroupName ${RGNAME} -Name ${VMNAME};
$Sourcedisk = Get-AzDisk -ResourceGroupName ${RGNAME} -DiskName $VM.StorageProfile.OsDisk.Name
clear
Write-Host "============================================================================================================================================================="
Write-Host "Encryption Settings:"
Write-Host "============================================================================================================================================================="
Write-Host "Enabled:" $Sourcedisk.EncryptionSettingsCollection.Enabled
Write-Host "Version:" $Sourcedisk.EncryptionSettingsCollection.EncryptionSettingsVersion
Write-Host "Source Vault:" $Sourcedisk.EncryptionSettingsCollection.EncryptionSettings.DiskEncryptionKey.SourceVault.Id
Write-Host "Secret URL:" $Sourcedisk.EncryptionSettingsCollection.EncryptionSettings.DiskEncryptionKey.SecretUrl
Write-Host "Key URL:" $Sourcedisk.EncryptionSettingsCollection.EncryptionSettings.KeyEncryptionKey.KeyUrl
Write-Host "============================================================================================================================================================="
```
![Comprobación de PowerShell de pase doble 1](./media/disk-encryption/verify-encryption-linux/verify-dual-ps-001.png)

### <a name="unattached-disks"></a>Discos sin conectar

Compruebe la configuración de cifrado de los discos que no están conectados a una máquina virtual.

### <a name="managed-disks"></a>Discos administrados
```powershell
$Sourcedisk = Get-AzDisk -ResourceGroupName ${RGNAME} -DiskName ${TARGETDISKNAME}
Write-Host "============================================================================================================================================================="
Write-Host "Encryption Settings:"
Write-Host "============================================================================================================================================================="
Write-Host "Enabled:" $Sourcedisk.EncryptionSettingsCollection.Enabled
Write-Host "Version:" $Sourcedisk.EncryptionSettingsCollection.EncryptionSettingsVersion
Write-Host "Source Vault:" $Sourcedisk.EncryptionSettingsCollection.EncryptionSettings.DiskEncryptionKey.SourceVault.Id
Write-Host "Secret URL:" $Sourcedisk.EncryptionSettingsCollection.EncryptionSettings.DiskEncryptionKey.SecretUrl
Write-Host "Key URL:" $Sourcedisk.EncryptionSettingsCollection.EncryptionSettings.KeyEncryptionKey.KeyUrl
Write-Host "============================================================================================================================================================="
```
## <a name="using-az-cli"></a>Uso de la CLI de Azure

Puede validar el estado de cifrado **general** de una máquina virtual cifrada con los siguientes comandos de la CLI de Azure:

```bash
VMNAME="VMNAME"
RGNAME="RGNAME"
az vm encryption show --name ${VMNAME} --resource-group ${RGNAME} --query "substatus"
```
![Comprobación general mediante la CLI ](./media/disk-encryption/verify-encryption-linux/verify-gen-cli.png)

### <a name="single-pass"></a>Pase único
Puede validar la configuración de cifrado de cada disco individual mediante los siguientes comandos de la CLI de Azure:

```bash
az vm encryption show -g ${RGNAME} -n ${VMNAME} --query "disks[*].[name, statuses[*].displayStatus]"  -o table
```

![Configuración del cifrado de datos](./media/disk-encryption/verify-encryption-linux/data-encryption-settings-2.png)

>[!IMPORTANT]
> En caso de que el disco no tenga la configuración de cifrado marcada, se mostrará como "El disco no está cifrado".

Configuración detallada del estado y del cifrado:

Disco del sistema operativo:

```bash
RGNAME="RGNAME"
VMNAME="VNAME"

disk=`az vm show -g ${RGNAME} -n ${VMNAME} --query storageProfile.osDisk.name -o tsv`
for disk in $disk; do \
echo "============================================================================================================================================================="
echo -ne "Disk Name: "; az disk show -g ${RGNAME} -n ${disk} --query name -o tsv; \
echo -ne "Encryption Enabled: "; az disk show -g ${RGNAME} -n ${disk} --query encryptionSettingsCollection.enabled -o tsv; \
echo -ne "Version: "; az disk show -g ${RGNAME} -n ${TARGETDISKNAME} --query encryptionSettingsCollection.encryptionSettingsVersion -o tsv; \
echo -ne "Disk Encryption Key: "; az disk show -g ${RGNAME} -n ${disk} --query encryptionSettingsCollection.encryptionSettings[].diskEncryptionKey.secretUrl -o tsv; \
echo -ne "key Encryption Key: "; az disk show -g ${RGNAME} -n ${disk} --query encryptionSettingsCollection.encryptionSettings[].keyEncryptionKey.keyUrl -o tsv; \
echo "============================================================================================================================================================="
done
```

![OSSingleCLI](./media/disk-encryption/verify-encryption-linux/os-single-cli.png)

Discos de datos:

```bash
RGNAME="RGNAME"
VMNAME="VMNAME"
az vm encryption show --name ${VMNAME} --resource-group ${RGNAME} --query "substatus"

for disk in `az vm show -g ${RGNAME} -n ${VMNAME} --query storageProfile.dataDisks[].name -o tsv`; do \
echo "============================================================================================================================================================="; \
echo -ne "Disk Name: "; az disk show -g ${RGNAME} -n ${disk} --query name -o tsv; \
echo -ne "Encryption Enabled: "; az disk show -g ${RGNAME} -n ${disk} --query encryptionSettingsCollection.enabled -o tsv; \
echo -ne "Version: "; az disk show -g ${RGNAME} -n ${TARGETDISKNAME} --query encryptionSettingsCollection.encryptionSettingsVersion -o tsv; \
echo -ne "Disk Encryption Key: "; az disk show -g ${RGNAME} -n ${disk} --query encryptionSettingsCollection.encryptionSettings[].diskEncryptionKey.secretUrl -o tsv; \
echo -ne "key Encryption Key: "; az disk show -g ${RGNAME} -n ${disk} --query encryptionSettingsCollection.encryptionSettings[].keyEncryptionKey.keyUrl -o tsv; \
echo "============================================================================================================================================================="
done
```

![CLI de datos única ](./media/disk-encryption/verify-encryption-linux/data-single-cli.png)

### <a name="dual-pass"></a>Pase doble de ADE

``` bash
az vm encryption show --name ${VMNAME} --resource-group ${RGNAME} -o table
```

![Compruebe la configuración general dual mediante la CLI ](./media/disk-encryption/verify-encryption-linux/verify-gen-dual-cli.png). También puede comprobar la configuración de cifrado en el perfil de almacenamiento del modelo de máquina virtual del disco del sistema operativo:

```bash
disk=`az vm show -g ${RGNAME} -n ${VMNAME} --query storageProfile.osDisk.name -o tsv`
for disk in $disk; do \
echo "============================================================================================================================================================="; \
echo -ne "Disk Name: "; az disk show -g ${RGNAME} -n ${disk} --query name -o tsv; \
echo -ne "Encryption Enabled: "; az disk show -g ${RGNAME} -n ${disk} --query encryptionSettingsCollection.enabled -o tsv; \
echo -ne "Version: "; az disk show -g ${RGNAME} -n ${TARGETDISKNAME} --query encryptionSettingsCollection.encryptionSettingsVersion -o tsv; \
echo -ne "Disk Encryption Key: "; az disk show -g ${RGNAME} -n ${disk} --query encryptionSettingsCollection.encryptionSettings[].diskEncryptionKey.secretUrl -o tsv; \
echo -ne "key Encryption Key: "; az disk show -g ${RGNAME} -n ${disk} --query encryptionSettingsCollection.encryptionSettings[].keyEncryptionKey.keyUrl -o tsv; \
echo "============================================================================================================================================================="
done
```

![Comprobación del perfil dual de máquina virtual mediante la CLI ](./media/disk-encryption/verify-encryption-linux/verify-vm-profile-dual-cli.png)

### <a name="unattached-disks"></a>Discos sin conectar

Compruebe la configuración de cifrado de los discos que no están conectados a una máquina virtual.

### <a name="managed-disks"></a>Discos administrados

```bash
RGNAME="RGNAME"
TARGETDISKNAME="DISKNAME"
echo "============================================================================================================================================================="
echo -ne "Disk Name: "; az disk show -g ${RGNAME} -n ${TARGETDISKNAME} --query name -o tsv; \
echo -ne "Encryption Enabled: "; az disk show -g ${RGNAME} -n ${TARGETDISKNAME} --query encryptionSettingsCollection.enabled -o tsv; \
echo -ne "Version: "; az disk show -g ${RGNAME} -n ${TARGETDISKNAME} --query encryptionSettingsCollection.encryptionSettingsVersion -o tsv; \
echo -ne "Disk Encryption Key: "; az disk show -g ${RGNAME} -n ${TARGETDISKNAME} --query encryptionSettingsCollection.encryptionSettings[].diskEncryptionKey.secretUrl -o tsv; \
echo -ne "key Encryption Key: "; az disk show -g ${RGNAME} -n ${TARGETDISKNAME} --query encryptionSettingsCollection.encryptionSettings[].keyEncryptionKey.keyUrl -o tsv; \
echo "============================================================================================================================================================="
```
### <a name="unmanaged-disks"></a>Discos no administrados

Los discos no administrados son archivos VHD almacenados como blobs en páginas en cuentas de Azure Storage.

Para obtener los detalles de un disco específico, debe proporcionar:

El identificador de la cuenta de almacenamiento que contenga el disco.
Una cadena de conexión para esa cuenta de almacenamiento particular.
El nombre del contenedor que almacena el disco.
El nombre del disco.

Este comando muestra todos los identificadores de todas las cuentas de almacenamiento:

```bash
az storage account list --query [].[id] -o tsv
```
Los identificadores de la cuenta de almacenamiento se muestran en este formato:

/subscriptions/\<IDDeSuscripción>/resourceGroups/\<NombreGrupoDeRecursos>/providers/Microsoft.Storage/storageAccounts/\<NombreCuentaDeAlmacenamiento>

Seleccione el identificador adecuado y almacénelo en una variable:
```bash
id="/subscriptions/<subscription id>/resourceGroups/<resource group name>/providers/Microsoft.Storage/storageAccounts/<storage account name>"
```
La cadena de conexión.

Este comando obtiene la cadena de conexión para una cuenta de almacenamiento concreta y la almacena en una variable:

```bash
ConnectionString=$(az storage account show-connection-string --ids $id --query connectionString -o tsv)
```

El nombre del contenedor.

El siguiente comando muestra todos los contenedores en una cuenta de almacenamiento:
```bash
az storage container list --connection-string $ConnectionString --query [].[name] -o tsv
```
El contenedor que se usa para los discos se denomina normalmente "vhds".

Almacene el nombre del contenedor en una variable. 
```bash
ContainerName="name of the container"
```

El nombre del disco.

Use este comando para enumerar todos los blobs de un contenedor determinado.
```bash 
az storage blob list -c ${ContainerName} --connection-string $ConnectionString --query [].[name] -o tsv
```
Elija el disco que desea consultar y almacene su nombre en una variable.
```bash
DiskName="diskname.vhd"
```
Consulte la configuración de cifrado de disco.
```bash
az storage blob show -c ${ContainerName} --connection-string ${ConnectionString} -n ${DiskName} --query metadata.DiskEncryptionSettings
```

## <a name="from-the-os"></a>Desde el sistema operativo
Valide si las particiones de disco de datos están cifradas (y si el disco del sistema operativo no lo está)

Cuando se cifra una partición o un disco, se muestra como tipo **crypt** y, cuando no está cifrado, se muestra como tipo **part/disk**.

``` bash
lsblk
```

![Capa crypt de SO ](./media/disk-encryption/verify-encryption-linux/verify-os-crypt-layer.png)

Puede obtener más información con la variante "lsblk" siguiente. 

Verá una capa de tipo **crypt** que está montada por la extensión.

En el ejemplo siguiente se muestran los volúmenes lógicos y los discos normales que tienen un "**crypto\_LUKS FSTYPE**".

```bash
lsblk -o NAME,TYPE,FSTYPE,LABEL,SIZE,RO,MOUNTPOINT
```
![Capa crypt de SO 2](./media/disk-encryption/verify-encryption-linux/verify-os-crypt-layer-2.png)

Como paso adicional, también puede validar si el disco de datos tiene claves cargadas.

``` bash
cryptsetup luksDump /dev/VGNAME/LVNAME
```

``` bash
cryptsetup luksDump /dev/sdd1
```

Y qué dispositivos DM se muestran como crypt.

```bash
dmsetup ls --target crypt
```

## <a name="next-steps"></a>Pasos siguientes

- [Solución de problemas de Azure Disk Encryption](disk-encryption-troubleshooting.md)
