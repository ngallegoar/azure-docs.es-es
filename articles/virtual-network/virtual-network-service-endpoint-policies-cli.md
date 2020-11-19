---
title: 'Restricción de la filtración de datos a Azure Storage: CLI de Azure'
description: En este artículo aprenderá a limitar y restringir la filtración de datos de red virtual a los recursos de Azure Storage con directivas de punto de conexión de servicio de red virtual mediante la CLI de Azure.
services: virtual-network
documentationcenter: virtual-network
author: rdhillon
manager: ''
editor: ''
tags: azure-resource-manager
Customer intent: I want only specific Azure Storage account to be allowed access from a virtual network subnet.
ms.assetid: ''
ms.service: virtual-network
ms.devlang: azurecli
ms.topic: how-to
ms.tgt_pltfrm: virtual-network
ms.workload: infrastructure-services
ms.date: 02/03/2020
ms.author: rdhillon
ms.custom: ''
ms.openlocfilehash: a080c3953c48227301052a0bb151c8cba96e515a
ms.sourcegitcommit: c2dd51aeaec24cd18f2e4e77d268de5bcc89e4a7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/18/2020
ms.locfileid: "94737394"
---
# <a name="manage-data-exfiltration-to-azure-storage-accounts-with-virtual-network-service-endpoint-policies-using-the-azure-cli"></a>Administración de la filtración de datos a cuentas de Azure Storage con directivas de punto de conexión de servicio de red virtual mediante la CLI de Azure

Las directivas de punto de conexión de servicio de red virtual permiten aplicar el control de acceso en cuentas de Azure Storage desde una red virtual a través de puntos de conexión de servicio. Se trata de una clave para proteger las cargas de trabajo, administrar qué cuentas de almacenamiento se permiten y dónde se permite la filtración de datos.
En este artículo aprenderá a:

* Crear una red virtual y agregar una subred.
* Habilitar un punto de conexión de servicio para Azure Storage.
* Crear dos cuentas de Azure Storage y permita el acceso de red a estas desde la subred creada anteriormente.
* Crear una directiva de punto de conexión de servicio para permitir el acceso solo a una de las cuentas de almacenamiento.
* Implementar una máquina virtual (VM) en la subred.
* Confirmar el acceso a la cuenta de almacenamiento permitida desde la subred.
* Confirmar que se ha denegado el acceso a la cuenta de almacenamiento no permitida desde la subred.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [azure-cli-prepare-your-environment.md](../../includes/azure-cli-prepare-your-environment.md)]

- This article requires version 2.0.28 or later of the Azure CLI. Si usa Azure Cloud Shell, ya está instalada la versión más reciente.

## <a name="create-a-virtual-network"></a>Creación de una red virtual

Antes de crear una red virtual, cree un grupo de recursos para ella y los demás recursos que se crearon en este artículo. Cree un grupo de recursos con [az group create](/cli/azure/group). En el ejemplo siguiente, se crea un grupo de recursos denominado *myResourceGroup* en la ubicación *eastus*.

```azurecli-interactive
az group create \
  --name myResourceGroup \
  --location eastus
```

Cree una red virtual con una subred con [az network vnet create](/cli/azure/network/vnet).

```azurecli-interactive
az network vnet create \
  --name myVirtualNetwork \
  --resource-group myResourceGroup \
  --address-prefix 10.0.0.0/16 \
  --subnet-name Private \
  --subnet-prefix 10.0.0.0/24
```

## <a name="enable-a-service-endpoint"></a>Habilitación de un punto de conexión de servicio 

En este ejemplo, se crea un punto de conexión de servicio para *Microsoft.Storage* para la subred *Private*: 

```azurecli-interactive
az network vnet subnet create \
  --vnet-name myVirtualNetwork \
  --resource-group myResourceGroup \
  --name Private \
  --address-prefix 10.0.0.0/24 \
  --service-endpoints Microsoft.Storage
```

## <a name="restrict-network-access-for-a-subnet"></a>Restricción del acceso de la red para una subred

Cree un grupo de seguridad de red con [az network nsg create](/cli/azure/network/nsg). En el ejemplo siguiente se crea un grupo de seguridad de red denominado *myNsgPrivate*.

```azurecli-interactive
az network nsg create \
  --resource-group myResourceGroup \
  --name myNsgPrivate
```

Asocie el grupo de seguridad de red a la subred *Private* con [az network vnet subnet update](/cli/azure/network/vnet/subnet). En el ejemplo siguiente se asocia el grupo de seguridad de red *myNsgPrivate* a la subred *Private*:

```azurecli-interactive
az network vnet subnet update \
  --vnet-name myVirtualNetwork \
  --name Private \
  --resource-group myResourceGroup \
  --network-security-group myNsgPrivate
```

Cree reglas de seguridad con [az network nsg rule create](/cli/azure/network/nsg/rule). La regla siguiente permite el acceso de salida a las direcciones IP públicas asignadas al servicio Azure Storage: 

```azurecli-interactive
az network nsg rule create \
  --resource-group myResourceGroup \
  --nsg-name myNsgPrivate \
  --name Allow-Storage-All \
  --access Allow \
  --protocol "*" \
  --direction Outbound \
  --priority 100 \
  --source-address-prefix "VirtualNetwork" \
  --source-port-range "*" \
  --destination-address-prefix "Storage" \
  --destination-port-range "*"
```

Cada grupo de seguridad de red contiene varias [reglas de seguridad predeterminadas](security-overview.md#default-security-rules). La regla siguiente invalida una regla de seguridad predeterminada que permite el acceso de salida a todas las direcciones IP públicas. La opción `destination-address-prefix "Internet"` deniega el acceso de salida a todas las direcciones IP públicas. La regla anterior invalida esta regla porque su prioridad es más alta, lo que permite el acceso a las direcciones IP públicas de Azure Storage.

```azurecli-interactive
az network nsg rule create \
  --resource-group myResourceGroup \
  --nsg-name myNsgPrivate \
  --name Deny-Internet-All \
  --access Deny \
  --protocol "*" \
  --direction Outbound \
  --priority 110 \
  --source-address-prefix "VirtualNetwork" \
  --source-port-range "*" \
  --destination-address-prefix "Internet" \
  --destination-port-range "*"
```

La siguiente regla permite la entrada de tráfico SSH en la subred desde cualquier lugar. La regla invalidará cualquier regla de seguridad predeterminada que deniegue todo el tráfico de entrada procedente de Internet. SSH se admite en la subred, por lo que dicha conectividad podrá probarse en un paso posterior.

```azurecli-interactive
az network nsg rule create \
  --resource-group myResourceGroup \
  --nsg-name myNsgPrivate \
  --name Allow-SSH-All \
  --access Allow \
  --protocol Tcp \
  --direction Inbound \
  --priority 120 \
  --source-address-prefix "*" \
  --source-port-range "*" \
  --destination-address-prefix "VirtualNetwork" \
  --destination-port-range "22"
```

## <a name="restrict-network-access-to-azure-storage-accounts"></a>Restricción del acceso de red a las cuentas de Azure Storage

En esta sección se enumeran los pasos para restringir el acceso de red a una cuenta de Azure Storage desde la subred especificada en una red virtual a través de un punto de conexión de servicio.

### <a name="create-a-storage-account"></a>Crear una cuenta de almacenamiento

Cree dos cuentas de almacenamiento de Azure con [az storage account create](/cli/azure/storage/account).

```azurecli-interactive
storageAcctName1="allowedstorageacc"

az storage account create \
  --name $storageAcctName1 \
  --resource-group myResourceGroup \
  --sku Standard_LRS \
  --kind StorageV2

storageAcctName2="notallowedstorageacc"

az storage account create \
  --name $storageAcctName2 \
  --resource-group myResourceGroup \
  --sku Standard_LRS \
  --kind StorageV2
```

Después de crear las cuentas de almacenamiento, recupere la cadena de conexión para dichas cuentas en una variable con [az storage account show-connection-string](/cli/azure/storage/account). La cadena de conexión se utiliza para crear un recurso compartido de archivos en un paso posterior.

```azurecli-interactive
saConnectionString1=$(az storage account show-connection-string \
  --name $storageAcctName1 \
  --resource-group myResourceGroup \
  --query 'connectionString' \
  --out tsv)

saConnectionString2=$(az storage account show-connection-string \
  --name $storageAcctName2 \
  --resource-group myResourceGroup \
  --query 'connectionString' \
  --out tsv)
```

<a name="account-key"></a>Vea el contenido de la variable y anote el valor de **AccountKey** devuelto en la salida, porque se utilizará en un paso posterior.

```azurecli-interactive
echo $saConnectionString1

echo $saConnectionString2
```

### <a name="create-a-file-share-in-the-storage-account"></a>Creación de un recurso compartido de archivos en la cuenta de almacenamiento

Cree un recurso compartido de archivos en la cuenta de almacenamiento con [az storage share create](/cli/azure/storage/share). En un paso posterior, este recurso compartido de archivos está montado para confirmar el acceso de red a él.

```azurecli-interactive
az storage share create \
  --name my-file-share \
  --quota 2048 \
  --connection-string $saConnectionString1 > /dev/null

az storage share create \
  --name my-file-share \
  --quota 2048 \
  --connection-string $saConnectionString2 > /dev/null
```

### <a name="deny-all-network-access-to-the-storage-account"></a>Denegación de todo el acceso de red a la cuenta de almacenamiento

De forma predeterminada, las cuentas de almacenamiento aceptan conexiones de red procedentes de clientes de cualquier red. Para limitar el acceso a redes seleccionadas, cambie la acción predeterminada a *Deny* con [az storage account update](/cli/azure/storage/account). Una vez que se deniega el acceso a la red, no se puede acceder a la cuenta de almacenamiento desde ninguna red.

```azurecli-interactive
az storage account update \
  --name $storageAcctName1 \
  --resource-group myResourceGroup \
  --default-action Deny

az storage account update \
  --name $storageAcctName2 \
  --resource-group myResourceGroup \
  --default-action Deny
```

### <a name="enable-network-access-from-virtual-network-subnet"></a>Habilitación del acceso de red desde la subred de la red virtual

Permita el acceso a la red a la cuenta de almacenamiento desde la subred *Private* con [az storage account network-rule add](/cli/azure/storage/account/network-rule).

```azurecli-interactive
az storage account network-rule add \
  --resource-group myResourceGroup \
  --account-name $storageAcctName1 \
  --vnet-name myVirtualNetwork \
  --subnet Private

az storage account network-rule add \
  --resource-group myResourceGroup \
  --account-name $storageAcctName2 \
  --vnet-name myVirtualNetwork \
  --subnet Private
```

## <a name="apply-policy-to-allow-access-to-valid-storage-account"></a>Aplicación de la directiva para permitir el acceso a una cuenta de almacenamiento válida

Las directivas de punto de conexión de servicio de Azure solo están disponibles para Azure Storage. Por lo tanto, habilitaremos el punto de conexión de servicio para *Microsoft.Storage* en esta subred para este ejemplo de configuración.

Las directivas de punto de conexión de servicio se aplican a los puntos de conexión de servicio. Comenzaremos por crear una directiva de punto de conexión de servicio. A continuación, crearemos las definiciones de directiva como parte de esta directiva para las cuentas de Azure Storage que se aprobarán para la subred.

Creación de una directiva de punto de conexión de servicio

```azurecli-interactive
az network service-endpoint policy create \
  --resource-group myResourceGroup \
  --name mysepolicy \
  --location eastus
```

Guarde el URI de recurso para la cuenta de almacenamiento permitida en una variable. Antes de ejecutar el comando siguiente, reemplace *\<your-subscription-id>* por el valor real de su identificador de suscripción.

```azurecli-interactive
$serviceResourceId="/subscriptions/<your-subscription-id>/resourceGroups/myResourceGroup/providers/Microsoft.Storage/storageAccounts/allowedstorageacc"
```

Cree y agregue una definición de directiva para permitir la cuenta de Azure Storage anterior en la directiva de punto de conexión de servicio.

```azurecli-interactive
az network service-endpoint policy-definition create \
  --resource-group myResourceGroup \
  --policy-name mysepolicy \
  --name mypolicydefinition \
  --service "Microsoft.Storage" \
  --service-resources $serviceResourceId
```

Y actualice la subred de la red virtual para asociarla a la directiva de punto de conexión de servicio creada en el paso anterior.

```azurecli-interactive
az network vnet subnet update \
  --vnet-name myVirtualNetwork \
  --resource-group myResourceGroup \
  --name Private \
  --service-endpoints Microsoft.Storage \
  --service-endpoint-policy mysepolicy
```

## <a name="validate-access-restriction-to-azure-storage-accounts"></a>Validación de la restricción de acceso a las cuentas de Azure Storage

### <a name="create-the-virtual-machine"></a>Creación de la máquina virtual

Para probar el acceso de red a una cuenta de almacenamiento, implemente una máquina virtual en la subred.

Cree una máquina virtual en la subred *Private* con [az vm create](/cli/azure/vm). Si las claves SSH no existen en la ubicación de claves predeterminada, el comando las crea. Para utilizar un conjunto específico de claves, utilice la opción `--ssh-key-value`.

```azurecli-interactive
az vm create \
  --resource-group myResourceGroup \
  --name myVmPrivate \
  --image UbuntuLTS \
  --vnet-name myVirtualNetwork \
  --subnet Private \
  --generate-ssh-keys
```

La máquina virtual tarda en crearse unos minutos. Después de la creación, tome nota de **publicIpAddress** en la salida devuelta. En un paso posterior, usaremos esta dirección para obtener acceso a la máquina virtual desde Internet.

### <a name="confirm-access-to-storage-account"></a>Confirmación del acceso a la cuenta de almacenamiento

SSH en la máquina virtual *myVmPrivate*. Reemplace *\<publicIpAddress>* por la dirección IP pública de la máquina virtual *myVmPrivate*.

```bash 
ssh <publicIpAddress>
```

Cree una carpeta para un punto de montaje:

```bash
sudo mkdir /mnt/MyAzureFileShare1
```

Monte el recurso compartido de archivos de Azure en el directorio que ha creado. Antes de ejecutar el comando siguiente, reemplace *\<storage-account-key>* por el valor de *AccountKey* de **$saConnectionString1**.

```bash
sudo mount --types cifs //allowedstorageacc.file.core.windows.net/my-file-share /mnt/MyAzureFileShare1 --options vers=3.0,username=allowedstorageacc,password=<storage-account-key>,dir_mode=0777,file_mode=0777,serverino
```

Recibirá el símbolo del sistema `user@myVmPrivate:~$`. El recurso compartido de archivos de Azure se montó correctamente en */mnt/MyAzureFileShare*.

### <a name="confirm-access-is-denied-to-storage-account"></a>Confirmación de la denegación del acceso a la cuenta de almacenamiento

En la misma máquina virtual *myVmPrivate*, cree un directorio para un punto de montaje:

```bash
sudo mkdir /mnt/MyAzureFileShare2
```

Intente montar el recurso compartido de archivos de Azure desde la cuenta de almacenamiento *notallowedstorageacc* en el directorio creado. En este artículo se asume que ha implementado la versión más reciente de Ubuntu. Si está utilizando versiones anteriores de Ubuntu, consulte [Montaje en Linux](../storage/files/storage-how-to-use-files-linux.md?toc=%2fazure%2fvirtual-network%2ftoc.json) para instrucciones adicionales sobre el montaje de recursos compartidos de archivos. 

Antes de ejecutar el comando siguiente, reemplace *\<storage-account-key>* por el valor de *AccountKey* de **$saConnectionString2**.

```bash
sudo mount --types cifs //notallowedstorageacc.file.core.windows.net/my-file-share /mnt/MyAzureFileShare2 --options vers=3.0,username=notallowedstorageacc,password=<storage-account-key>,dir_mode=0777,file_mode=0777,serverino
```

Se ha denegado el acceso y recibe un error de `mount error(13): Permission denied`, porque esta cuenta de almacenamiento no está en la lista de permitidos de la directiva de punto de conexión de servicio que se aplica a la subred. 

Salga de la sesión de SSH en la máquina virtual *myVmPublic*.

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando ya no se necesiten, use [az group delete](/cli/azure) para quitar el grupo de recursos y todos los recursos que contenga.

```azurecli-interactive 
az group delete --name myResourceGroup --yes
```

## <a name="next-steps"></a>Pasos siguientes

En este artículo, ha aplicado una directiva de punto de conexión de servicio a través de un punto de conexión de servicio de Azure Virtual Network para Azure Storage. Ha creado cuentas de Azure Storage y limitado el acceso de red solo a determinadas cuentas de almacenamiento (por tanto, denegado a otras) desde una subred de la red virtual. Para más información sobre las directivas de punto de conexión de servicio, consulte [Información general sobre las directivas de punto de conexión de servicio](virtual-network-service-endpoint-policies-overview.md).
