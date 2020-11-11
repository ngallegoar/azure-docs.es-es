---
title: Instalación de la extensión del Agente de IaaS de SQL Server
description: Registre la máquina virtual de Azure SQL Server con el proveedor de recursos de máquina virtual con SQL para habilitar características para máquinas virtuales con SQL Server implementadas fuera de Azure Marketplace, además de para garantizar el cumplimiento y mejorar la administración.
services: virtual-machines-windows
documentationcenter: na
author: MashaMSFT
tags: azure-resource-manager
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 10/30/2020
ms.author: mathoma
ms.reviewer: jroth
ms.custom: devx-track-azurecli, devx-track-azurepowershell
ms.openlocfilehash: 8099bf87410e8359894d8b40d7637451891ad664
ms.sourcegitcommit: 7863fcea618b0342b7c91ae345aa099114205b03
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/03/2020
ms.locfileid: "93286143"
---
# <a name="install-sql-server-iaas-agent-extension"></a>Instalación de la extensión del Agente de IaaS de SQL Server
[!INCLUDE[appliesto-sqlvm](../../includes/appliesto-sqlvm.md)]

Al registrar la VM de SQL Server con el proveedor de recursos de VM con SQL, se instala la [extensión del Agente de IaaS de SQL Server](sql-server-iaas-agent-extension-automate-management.md). 

En este artículo se explica cómo registrar una única VM con SQL Server con el proveedor de recursos de la máquina virtual con SQL. Como alternativa, puede registrar todas las VM con SQL Server [automáticamente](sql-vm-resource-provider-automatic-registration.md) o [cifradas en masa](sql-vm-resource-provider-bulk-register.md).


## <a name="overview"></a>Introducción

Al registrarse con el proveedor de recursos, se instala la [extensión del Agente de IaaS de SQL Server](sql-server-iaas-agent-extension-automate-management.md) y también se crea el _recurso_ de la **máquina virtual con SQL** dentro de la suscripción, que es un recurso _independiente_ del recurso de máquina virtual. Al anular el registro de la máquina virtual SQL Server del proveedor de recursos, se eliminará el **recurso** de la _máquina virtual SQL_ pero no se eliminará la máquina virtual real.

La implementación de una imagen de Azure Marketplace de una máquina virtual con SQL Server mediante Azure Portal registra la máquina virtual con SQL Server con el proveedor de recursos. Sin embargo, si elige instalar automáticamente SQL Server en una máquina virtual de Azure, o aprovisionar una máquina virtual de Azure desde un disco duro virtual personalizado, debe registrar la VM con SQL Server con el proveedor de recursos de VM con SQL para instalar la extensión del Agente de IaaS de SQL. 

Para utilizar el proveedor de recursos de máquina virtual con SQL, primero debe [registrar la suscripción con el proveedor de recursos](#register-subscription-with-rp), lo que proporciona al proveedor de recursos la capacidad de crear recursos dentro de esa suscripción específica.

> [!IMPORTANT]
> La extensión del Agente de IaaS de SQL recopila datos con el fin de ofrecer a los clientes ventajas opcionales al usar SQL Server en Azure Virtual Machines. Microsoft no usará estos datos para auditorías de licencias sin el consentimiento previo del cliente. Para obtener más información, consulte [Complemento de privacidad de SQL Server](/sql/sql-server/sql-server-privacy#non-personal-data).

## <a name="prerequisites"></a>Requisitos previos

Para registrar una máquina virtual con SQL Server con el proveedor de recursos, necesita lo siguiente: 

- Una [suscripción de Azure](https://azure.microsoft.com/free/).
- Una [máquina virtual Windows Server 2008 (o superior)](../../../virtual-machines/windows/quick-create-portal.md) del modelo de recursos de Azure con [SQL Server 2008 (o superior)](https://www.microsoft.com/sql-server/sql-server-downloads) implementado en la nube pública o de Azure Government. 
- La versión más reciente de la [CLI de Azure](/cli/azure/install-azure-cli) o [Azure PowerShell (5.0 como mínimo)](/powershell/azure/install-az-ps). 


## <a name="register-subscription-with-rp"></a>Registro de la suscripción con el proveedor de recursos

Para registrar la máquina virtual de SQL Server con el proveedor de recursos de la máquina virtual de SQL, debe registrar primero su suscripción con el proveedor de recursos. Esto permite al proveedor de recursos de la máquina virtual con SQL crear recursos en la suscripción.  Para ello, puede usar Azure Portal, la CLI de Azure o Azure PowerShell.

### <a name="azure-portal"></a>Azure portal

1. Abra Azure Portal y vaya a **Todos los servicios**. 
1. Vaya a **Suscripciones** y seleccione la suscripción que le interese.  
1. En la página **Suscripciones** , vaya a **Proveedores de recursos**. 
1. Escriba **sql** en el filtro para mostrar los proveedores de recursos relacionados con SQL. 
1. Seleccione **Registrar** , **Volver a registrar** o **Anular registro** para el proveedor **Microsoft.SqlVirtualMachine** , según la acción que quiera realizar. 

   ![Modificación del proveedor](./media/sql-vm-resource-provider-register/select-resource-provider-sql.png)


### <a name="command-line"></a>Línea de comandos

Registre el proveedor de recursos de VM con SQL en su suscripción a Azure mediante la CLI de Azure o Azure PowerShell. 

# <a name="azure-cli"></a>[CLI de Azure](#tab/bash)

```azurecli-interactive
# Register the SQL VM resource provider to your subscription 
az provider register --namespace Microsoft.SqlVirtualMachine 
```

# <a name="azure-powershell"></a>[Azure PowerShell](#tab/powershell)

```powershell-interactive
# Register the SQL VM resource provider to your subscription
Register-AzResourceProvider -ProviderNamespace Microsoft.SqlVirtualMachine
```

---

## <a name="register-with-rp"></a>Registro con proveedor de recursos

Hay tres modos de administración para la [extensión del Agente de IaaS de SQL Server](sql-server-iaas-agent-extension-automate-management.md#management-modes). 

Al instalar la extensión en el modo de administración completo, se reinicia el servicio SQL Server, por lo que se recomienda instalar la extensión en modo ligero primero y, a continuación, [actualizar al modo completo](#upgrade-to-full) durante una ventana de mantenimiento. 

### <a name="lightweight-management-mode"></a>Modo de administración ligera

Use la CLI de Azure o Azure PowerShell para registrar su VM con SQL Server con el proveedor de recursos e instale la extensión IaaS de SQL en el modo ligero. Esto no reiniciará el servicio SQL Server. Después, puede actualizar al modo completo en cualquier momento, pero si lo hace, se reiniciará el servicio SQL Server, por lo que se recomienda esperar a una ventana de mantenimiento programado. 

Proporcione un tipo de licencia de SQL Server como pago por uso (`PAYG`) para pagar por uso, Ventaja híbrida de Azure (`AHUB`) para usar su propia licencia o recuperación ante desastres (`DR`) para activar [la licencia de réplica de recuperación ante desastres gratuita](business-continuity-high-availability-disaster-recovery-hadr-overview.md#free-dr-replica-in-azure).

Tanto las instancias del clúster de conmutación por error como las implementaciones de instancias múltiples solo se pueden registrar en el proveedor de recursos de máquina virtual con SQL en el modo ligero. 

# <a name="azure-cli"></a>[CLI de Azure](#tab/bash)

Registre una VM con SQL Server en modo ligero con la CLI de Azure: 

  ```azurecli-interactive
  # Register Enterprise or Standard self-installed VM in Lightweight mode
  az sql vm create --name <vm_name> --resource-group <resource_group_name> --location <vm_location> --license-type PAYG 
  ```


# <a name="azure-powershell"></a>[Azure PowerShell](#tab/powershell)

Registre una VM con SQL Server en modo ligero con Azure PowerShell:  


  ```powershell-interactive
  # Get the existing compute VM
  $vm = Get-AzVM -Name <vm_name> -ResourceGroupName <resource_group_name>
          
  # Register SQL VM with 'Lightweight' SQL IaaS agent
  New-AzSqlVM -Name $vm.Name -ResourceGroupName $vm.ResourceGroupName -Location $vm.Location `
    -LicenseType PAYG -SqlManagementType LightWeight  
  ```

---

### <a name="full-management-mode"></a>Modo de administración completa

Al registrar la VM con SQL Server en modo completo, se reiniciará el servicio SQL Server. Proceda con precaución. 

Para registrar la VM con SQL Server directamente en modo completo (y posiblemente reiniciar el servicio SQL Server), use el siguiente comando de Azure PowerShell: 

  ```powershell-interactive
  # Get the existing  Compute VM
  $vm = Get-AzVM -Name <vm_name> -ResourceGroupName <resource_group_name>
        
  # Register with SQL VM resource provider in full mode
  New-AzSqlVM -Name $vm.Name -ResourceGroupName $vm.ResourceGroupName -SqlManagementType Full
  ```

### <a name="noagent-management-mode"></a>Modo de administración NoAgent 

SQL Server 2008 y 2008 R2 instalados en Windows Server 2008 ( _no R2_ ) se pueden registrar con el proveedor de recursos de VM con SQL en [modo NoAgent](sql-server-iaas-agent-extension-automate-management.md#management-modes). Esta opción garantiza el cumplimiento de normas y permite que la VM con SQL Server se supervise en Azure Portal con una funcionalidad limitada.

Especifique `AHUB`, `PAYG` o `DR` como valores de **sqlLicenseType** y `SQL2008-WS2008` o `SQL2008R2-WS2008` como valores de **sqlImageOffer**. 

Para registrar SQL Server 2008 o 2008 R2 en una instancia de Windows Server 2008, use el siguiente fragmento de código de la CLI de Azure o Azure PowerShell: 


# <a name="azure-cli"></a>[CLI de Azure](#tab/bash)

Registre la máquina virtual con SQL Server 2008 en modo NoAgent con la CLI de Azure: 

  ```azurecli-interactive
   az sql vm create -n sqlvm -g myresourcegroup -l eastus |
   --license-type PAYG --sql-mgmt-type NoAgent 
   --image-sku Enterprise --image-offer SQL2008-WS2008
 ```
 
 
Registre la máquina virtual con SQL Server 2008 R2 en modo NoAgent con la CLI de Azure: 

  ```azurecli-interactive
   az sql vm create -n sqlvm -g myresourcegroup -l eastus |
   --license-type PAYG --sql-mgmt-type NoAgent 
   --image-sku Enterprise --image-offer SQL2008R2-WS2008
 ```

# <a name="azure-powershell"></a>[Azure PowerShell](#tab/powershell)

Registre la máquina virtual con SQL Server 2008 en modo NoAgent con Azure PowerShell: 

  ```powershell-interactive
  # Get the existing compute VM
  $vm = Get-AzVM -Name <vm_name> -ResourceGroupName <resource_group_name>
          
  New-AzSqlVM -Name $vm.Name -ResourceGroupName $vm.ResourceGroupName -Location $vm.Location `
    -LicenseType PAYG -SqlManagementType NoAgent -Sku Standard -Offer SQL2008-WS2008
  ```
  

Registre la máquina virtual con SQL Server 2008 R2 en modo NoAgent con Azure PowerShell: 

  ```powershell-interactive
  # Get the existing compute VM
  $vm = Get-AzVM -Name <vm_name> -ResourceGroupName <resource_group_name>
          
  New-AzSqlVM -Name $vm.Name -ResourceGroupName $vm.ResourceGroupName -Location $vm.Location `
    -LicenseType PAYG -SqlManagementType NoAgent -Sku Standard -Offer SQL2008R2-WS2008
  ```

---

## <a name="verify-mode"></a>Comprobación del modo

Puede ver el modo actual del Agente de IaaS de SQL Server mediante Azure PowerShell: 

```powershell-interactive
# Get the SqlVirtualMachine
$sqlvm = Get-AzSqlVM -Name $vm.Name  -ResourceGroupName $vm.ResourceGroupName
$sqlvm.SqlManagementType
```

## <a name="upgrade-to-full"></a>Actualizar a completo  

Las VM con SQL Server que tienen instalada la extensión de IaaS *ligera* pueden actualizarse al modo _completo_ mediante Azure Portal, la CLI de Azure o Azure PowerShell. Las máquinas virtuales con SQL Server en el modo _NoAgent_ pueden actualizar al modo _completo_ tras actualizar el sistema operativo a Windows 2008 R2 y versiones posteriores. No es posible cambiar a un modo inferior: para ello, deberá [anular el registro](#unregister-from-rp) de la máquina virtual con SQL Server del proveedor de recursos de máquina virtual con SQL. Al hacerlo, se quitará el **recurso** de la _máquina virtual SQL_ , pero no se eliminará la máquina virtual real. 


### <a name="azure-portal"></a>Azure portal

1. Inicie sesión en [Azure Portal](https://portal.azure.com).
1. Vaya al recurso [Máquinas virtuales SQL](manage-sql-vm-portal.md#access-the-sql-virtual-machines-resource). 
1. Seleccione la VM con SQL Server y, luego, **Información general**. 
1. En el caso de las VM con SQL Server con los modos de IaaS NoAgent o Lightweight, seleccione el mensaje **Only license type and edition updates are available with the SQL IaaS extension** (Solo las actualizaciones de la edición y el tipo de licencia están disponibles con la extensión IaaS de SQL).

   ![Selecciones para cambiar el modo desde el portal](./media/sql-vm-resource-provider-register/change-sql-iaas-mode-portal.png)

1. Seleccione la casilla **Acepto reiniciar el servicio SQL Server en la máquina virtual** y, a continuación, seleccione **Confirmar** para actualizar el modo de IaaS a Full. 

    ![Casilla para aceptar el reinicio del servicio SQL Server en la máquina virtual](./media/sql-vm-resource-provider-register/enable-full-mode-iaas.png)

### <a name="command-line"></a>Línea de comandos

# <a name="azure-cli"></a>[CLI de Azure](#tab/bash)

Ejecute el siguiente fragmento de código de la CLI de Azure:

  ```azurecli-interactive
  # Update to full mode
  az sql vm update --name <vm_name> --resource-group <resource_group_name> --sql-mgmt-type full  
  ```

# <a name="azure-powershell"></a>[Azure PowerShell](#tab/powershell)

Ejecute el siguiente fragmento de código de Azure PowerShell:

  ```powershell-interactive
  # Get the existing  Compute VM
  $vm = Get-AzVM -Name <vm_name> -ResourceGroupName <resource_group_name>
        
  # Register with SQL VM resource provider in full mode
  Update-AzSqlVM -Name $vm.Name -ResourceGroupName $vm.ResourceGroupName -SqlManagementType Full
  ```

---

## <a name="verify-registration-status"></a>Comprobación del estado de registro
Puede comprobar si su VM con SQL Server ya se ha registrado con el proveedor de recursos de VM con SQL mediante Azure Portal, la CLI de Azure o Azure PowerShell. 

### <a name="azure-portal"></a>Azure portal 

1. Inicie sesión en [Azure Portal](https://portal.azure.com). 
1. Vaya a las [VM con SQL Server](manage-sql-vm-portal.md).
1. Seleccione la máquina con SQL Server en la lista. Si la máquina virtual con SQL Server no aparece en la lista, es probable que no se haya registrado con el proveedor de recursos de máquina virtual con SQL. 
1. Vea el valor de **Estado**. Si el valor de **Estado** es **Correcto** , la máquina virtual con SQL Server se ha registrado con el proveedor de recursos de máquina virtual con SQL correctamente. 

   ![Comprobación del estado con el registro en un proveedor de recursos de SQL](./media/sql-vm-resource-provider-register/verify-registration-status.png)

### <a name="command-line"></a>Línea de comandos

Compruebe el estado actual de registro de la VM con SQL Server mediante la CLI de Azure o Azure PowerShell. `ProvisioningState` mostrará `Succeeded` si el registro fue correcto. 

# <a name="azure-cli"></a>[CLI de Azure](#tab/bash)


  ```azurecli-interactive
  az sql vm show -n <vm_name> -g <resource_group>
 ```

# <a name="azure-powershell"></a>[Azure PowerShell](#tab/powershell)

  ```powershell-interactive
  Get-AzSqlVM -Name <vm_name> -ResourceGroupName <resource_group>
  ```

---

Un error indica que la VM con SQL Server no se ha registrado en el proveedor de recursos. 


## <a name="unregister-from-rp"></a>Anular el registro con el proveedor de recursos

Para anular el registro de VM con SQL Server con el proveedor de recursos de VM con SQL, elimine el *recurso* de la máquina virtual de SQL mediante Azure Portal o la CLI de Azure. Al eliminar el *recurso* de la máquina virtual con SQL no se elimina la VM con SQL Server. Sin embargo, tenga cuidado y siga los pasos detenidamente, ya que es posible eliminar la máquina virtual involuntariamente al intentar quitar el *recurso*. 

Es necesario anular el registro de máquina virtual con SQL con el proveedor de recursos de VM con SQL para cambiar el modo de administración completa. 

### <a name="azure-portal"></a>Azure portal

Para anular el registro de MV con SQL Server con el proveedor de recursos mediante Azure Portal, siga estos pasos:

1. Inicie sesión en el [Portal de Azure](https://portal.azure.com).
1. Vaya al recurso de VM con SQL. 
  
   ![Recurso máquinas virtuales SQL](./media/sql-vm-resource-provider-register/sql-vm-manage.png)

1. Seleccione **Eliminar**. 

   ![Seleccionar Eliminar en el panel de navegación superior](./media/sql-vm-resource-provider-register/delete-sql-vm-resource-provider.png)

1. Escriba el nombre de la máquina virtual con SQL y **desactive la casilla situada junto a la máquina virtual**.

   ![Desactivar la máquina virtual para evitar la eliminación de la máquina virtual real y seleccionar Eliminar para continuar con la eliminación del recurso de máquina virtual con SQL](./media/sql-vm-resource-provider-register/confirm-delete-of-resource-uncheck-box.png)

   >[!WARNING]
   > Si no se borra la casilla situada junto al nombre de la máquina virtual, se *eliminará* la máquina virtual por completo. Borre la casilla para anular el registro de MV con SQL Server del proveedor de recursos, pero *no eliminar la máquina virtual real*. 

1. Seleccione **Eliminar** para confirmar la eliminación del *recurso* de máquina virtual con SQL y no la VM con SQL Server. 

### <a name="command-line"></a>Línea de comandos

# <a name="azure-cli"></a>[CLI de Azure](#tab/azure-cli)
Para anular el registro de la VM con SQL Server del proveedor de recursos con la CLI de Azure, use el comando [az sql vm delete](/cli/azure/sql/vm?view=azure-cli-latest&preserve-view=true#az-sql-vm-delete). Esto quitará el *recurso* de la VM con SQL Server, pero no eliminará la máquina virtual. 


```azurecli-interactive
az sql vm delete 
  --name <SQL VM resource name> |
  --resource-group <Resource group name> |
  --yes 
```

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)
Para anular el registro de la VM con SQL Server del proveedor de recursos con Azure PowerShell, use el comando [Remove-AzSqlVM](/powershell/module/az.sqlvirtualmachine/remove-azsqlvm). Esto quitará el *recurso* de la VM con SQL Server, pero no eliminará la máquina virtual. 

```powershell-interactive
Remove-AzSqlVM -ResourceGroupName <resource_group_name> -Name <VM_name>
```

---



## <a name="next-steps"></a>Pasos siguientes

Para más información, consulte los siguientes artículos. 

* [Introducción a SQL Server en máquinas virtuales Windows](sql-server-on-azure-vm-iaas-what-is-overview.md)
* [Preguntas más frecuentes de SQL Server en máquinas virtuales Windows](frequently-asked-questions-faq.md)  
* [Orientación de precios de SQL Server para máquinas virtuales de Azure](pricing-guidance.md)
* [Notas de la versión de SQL Server en Azure Virtual Machines](../../database/doc-changes-updates-release-notes.md)
