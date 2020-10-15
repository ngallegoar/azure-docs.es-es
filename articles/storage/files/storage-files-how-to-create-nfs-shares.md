---
title: 'Creación de un recurso compartido NFS: Azure Files'
description: Obtenga información sobre cómo crear un recurso compartido de archivos de Azure que se pueda montar con el protocolo Network File System.
author: roygara
ms.service: storage
ms.topic: how-to
ms.date: 09/15/2020
ms.author: rogarana
ms.subservice: files
ms.custom: references_regions
ms.openlocfilehash: 2a848cb77336fc89172d55a6204d66b9e5be5976
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "90705238"
---
# <a name="how-to-create-an-nfs-share"></a>Procedimiento para crear un recurso compartido NFS

Los recursos compartidos de archivos de Azure son recursos compartidos de archivos totalmente administrados en la nube. Se pueden acceder mediante el protocolo de Bloque de mensajes del servidor o el protocolo Network File System (NFS). En este artículo se describe la creación de un recurso compartido de archivos que usa el protocolo NFS. Para obtener más información sobre ambos protocolos, consulte [Protocolos de recurso compartido de archivos de Azure](storage-files-compare-protocols.md).

## <a name="limitations"></a>Limitaciones

[!INCLUDE [files-nfs-limitations](../../../includes/files-nfs-limitations.md)]

### <a name="regional-availability"></a>Disponibilidad regional

[!INCLUDE [files-nfs-regional-availability](../../../includes/files-nfs-regional-availability.md)]

## <a name="prerequisites"></a>Requisitos previos

- Cree una [cuenta FileStorage](storage-how-to-create-premium-fileshare.md).

    > [!IMPORTANT]
    > Solo se puede tener acceso a los recursos compartidos de NFS desde redes de confianza. Las conexiones a su recurso compartido de NFS deben originarse en uno de los orígenes siguientes:

    - [Cree un punto de conexión privado](storage-files-networking-endpoints.md#create-a-private-endpoint) (recomendado) o [restrinja el acceso al punto de conexión público](storage-files-networking-endpoints.md#restrict-public-endpoint-access).
    - [Configure una VPN de punto a sitio (P2S) en Linux para su uso con Azure Files](storage-files-configure-p2s-vpn-linux.md).
    - [Configure una VPN de sitio a sitio para su uso con Azure Files](storage-files-configure-s2s-vpn.md).
    - Configure [ExpressRoute](../../expressroute/expressroute-introduction.md).
- Si planea usar la CLI de Azure, [instale la versión más reciente](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest).

## <a name="register-the-nfs-41-protocol"></a>Registro del protocolo NFS 4.1

Si usa el módulo Azure PowerShell o la CLI de Azure, registre la característica con los siguientes comandos:

### <a name="powershell"></a>PowerShell

```azurepowershell
Connect-AzAccount
$context = Get-AzSubscription -SubscriptionId <yourSubscriptionIDHere>
Set-AzContext $context
Register-AzProviderFeature -FeatureName AllowNfsFileShares -ProviderNamespace Microsoft.Storage
Register-AzResourceProvider -ProviderNamespace Microsoft.Storage
```

### <a name="azure-cli"></a>Azure CLI

```azurecli
az login
az feature register --name AllowNfsFileShares
                    --namespace Microsoft.Storage
                    --subscription <yourSubscriptionIDHere>
az provider register --namespace Microsoft.Storage
```

## <a name="verify-that-the-feature-is-registered"></a>Comprobar si la característica está registrada

La aprobación del registro puede tardar hasta una hora en completarse. Para comprobar si el registro se ha completado, ejecute los siguientes comandos:

### <a name="powershell"></a>PowerShell

```azurepowershell
Get-AzProviderFeature -ProviderNamespace Microsoft.Storage -FeatureName AllowNfsFileShares
```

### <a name="azure-cli"></a>Azure CLI

```azurecli
az feature show --name AllowNfsFileShares --namespace Microsoft.Storage --subscription <yourSubscriptionIDHere>
```

## <a name="create-an-nfs-share"></a>Creación de un recurso compartido NFS

# <a name="portal"></a>[Portal](#tab/azure-portal)

Ahora que ha creado una cuenta de FileStorage y ha configurado la red, puede crear un recurso compartido de archivos NFS. El proceso es similar a la creación de un recurso compartido SMB. Seleccione **NFS** en lugar de **SMB** al crear el recurso compartido.

1. Vaya a la cuenta de almacenamiento y seleccione **Recursos compartidos de archivos**.
1. Seleccione **+ Recurso compartido de archivos** para crear un recurso compartido de archivos.
1. Asigne un nombre al recurso compartido de archivos y seleccione una capacidad aprovisionada.
1. En **Protocolo**, seleccione **NFS (versión preliminar)** .
1. Para **Squash raíz** realice una selección.

    - Squash raíz (predeterminado): el acceso al superusuario remoto (raíz) se asigna a UID (65534) y GID (65534).
    - Sin squash raíz: el superusuario remoto (raíz) recibe acceso como raíz.
    - Todos con squash: todo el acceso de usuario se asigna a UID (65534) y GID (65534).
    
1. Seleccione **Crear**.

    :::image type="content" source="media/storage-files-how-to-create-mount-nfs-shares/create-nfs-file-share.png" alt-text="Captura de pantalla de la hoja de creación de recurso compartido de archivos":::

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

1. Asegúrese de que .NET Framework está instalado. Vea [Descargar .NET Framework](https://dotnet.microsoft.com/download/dotnet-framework).
 
1. Use el siguiente comando para comprobar que la versión de PowerShell que ha instalado es `5.1` o posterior.    

   ```powershell
   echo $PSVersionTable.PSVersion.ToString() 
   ```
    
   Para actualizar la versión de PowerShell, vea [Actualización de Windows PowerShell existente](https://docs.microsoft.com/powershell/scripting/install/installing-windows-powershell?view=powershell-6#upgrading-existing-windows-powershell).
    
1. Instale la versión más reciente del módulo PowerShellGet.

   ```powershell
   install-Module PowerShellGet –Repository PSGallery –Force  
   ```

1. Cierre la consola de PowerShell y vuelva a abrirla.

1. Instale la versión preliminar del módulo **Az.Storage** **2.5.2-preview**.

   ```powershell
   Install-Module Az.Storage -Repository PsGallery -RequiredVersion 2.5.2-preview -AllowClobber -AllowPrerelease -Force  
   ```

   Para más información sobre cómo instalar módulos de PowerShell, vea [Instalación del módulo de Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azps-3.0.0).
   
1. Para crear un recurso compartido de archivos Premium con el módulo de Azure PowerShell, use el cmdlet [New-AzRmStorageShare](/powershell/module/az.storage/new-azrmstorageshare).

> [!NOTE]
> Los tamaños de recurso compartido aprovisionados se especifican mediante la cuota del recurso compartido y los recursos compartidos de archivos se facturan en función del tamaño aprovisionado. Consulte la [página de precios](https://azure.microsoft.com/pricing/details/storage/files/)para obtener más información.

  ```powershell
  New-AzRmStorageShare `
   -ResourceGroupName $resourceGroupName `
   -StorageAccountName $storageAccountName `
   -Name myshare `
   -EnabledProtocol NFS `
   -RootSquash RootSquash `
   -Context $storageAcct.Context
  ```

# <a name="azure-cli"></a>[CLI de Azure](#tab/azure-cli)

Para crear un recurso compartido de archivos Premium con la CLI de Azure, utilice el comando [az storage share create](/cli/azure/storage/share-rm).

> [!NOTE]
> Los tamaños de recurso compartido aprovisionados se especifican mediante la cuota del recurso compartido y los recursos compartidos de archivos se facturan en función del tamaño aprovisionado. Consulte la [página de precios](https://azure.microsoft.com/pricing/details/storage/files/)para obtener más información.

```azurecli-interactive
az storage share-rm create \
    --account-name $STORAGEACCT \
    --account-key $STORAGEKEY \
    --enabled-protocol NFS \
    --root-access RootSquash \
    --name "myshare" 
```
---

## <a name="next-steps"></a>Pasos siguientes

Ahora que ha creado un recurso compartido de NFS, para usarlo debe montarlo en el cliente Linux. Para obtener más información, consulte [Procedimiento para montar un recurso compartido NFS](storage-files-how-to-mount-nfs-shares.md).

Si tiene algún problema, consulte [Solución de problemas de los recursos compartidos de archivos NFS de Azure](storage-troubleshooting-files-nfs.md).