---
title: ¿Qué es la extensión del Agente de IaaS de SQL Server?
description: En este artículo se describe cómo ayuda la extensión del Agente de IaaS de SQL Server a automatizar las tareas de administración específicas de SQL Server en las VM de Azure. Entre ellas se incluyen características como la copia de seguridad automatizada, la aplicación automatizada de revisiones, la integración de Azure Key Vault, la administración de licencias, la configuración de almacenamiento y la administración central de todas las instancias de VM con SQL Server.
services: virtual-machines-windows
documentationcenter: ''
author: MashaMSFT
editor: ''
tags: azure-resource-manager
ms.assetid: effe4e2f-35b5-490a-b5ef-b06746083da4
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 10/30/2020
ms.author: mathoma
ms.reviewer: jroth
ms.custom: seo-lt-2019
ms.openlocfilehash: 070058eae36bf4f8546cfcf4beb85ac5023e9c79
ms.sourcegitcommit: 7863fcea618b0342b7c91ae345aa099114205b03
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/03/2020
ms.locfileid: "93286180"
---
# <a name="what-is-the-sql-server-iaas-agent-extension"></a>¿Qué es la extensión del Agente de IaaS de SQL Server?
[!INCLUDE[appliesto-sqlvm](../../includes/appliesto-sqlvm.md)]


La extensión del Agente de IaaS de SQL Server (SqlIaasExtension) se ejecuta en SQL Server en Azure Virtual Machines (VM) para automatizar las tareas de administración. 

En este artículo se proporciona información general de la extensión. Para instalar la extensión de IaaS de SQL Server en SQL Server en las VM de Azure, consulte los artículos sobre [instalación automática](sql-vm-resource-provider-automatic-registration.md), [VM únicas](sql-vm-resource-provider-register.md) o [VM en masa](sql-vm-resource-provider-bulk-register.md). 

## <a name="overview"></a>Introducción

La extensión del Agente de IaaS de SQL Server ofrece una serie de ventajas para SQL Server en las VM de Azure: 

- **Ventajas de las características** : la extensión desbloquea varias ventajas de las características de automatización, como la administración del portal, la flexibilidad de las licencia, la copia de seguridad automatizada, la aplicación automatizada de revisiones, etc. Consulte [Ventajas de las características](#feature-benefits) más adelante en este artículo para obtener más información. 

- **Cumplimiento** : la extensión ofrece un método simplificado para satisfacer el requisito de notificar a Microsoft que se ha habilitado Ventaja híbrida de Azure, según se especifica en los términos del producto. Con este proceso no es necesario administrar formularios de registro de licencias para cada recurso.  

- **Gratis** : la extensión es totalmente gratuita en los tres modos de administración. No hay costos adicionales asociados al proveedor de recursos o al cambio de modo de administración. 

- **Administración de licencias simplificada** : La extensión simplifica la administración de licencias de SQL Server y permite identificar rápidamente las VM con SQL Server mediante Ventaja híbrida de Azure en [Azure Portal](manage-sql-vm-portal.md), la CLI de Azure o PowerShell: 

   # <a name="azure-cli"></a>[CLI de Azure](#tab/azure-cli)

   ```azurecli-interactive
   $vms = az sql vm list | ConvertFrom-Json
   $vms | Where-Object {$_.sqlServerLicenseType -eq "AHUB"}
   ```

   # <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

   ```powershell-interactive
   Get-AzSqlVM | Where-Object {$_.LicenseType -eq 'AHUB'}
   ```

   ---


> [!IMPORTANT]
> La extensión del Agente de IaaS de SQL recopila datos con el fin de ofrecer a los clientes ventajas opcionales al usar SQL Server en Azure Virtual Machines. Microsoft no usará estos datos para auditorías de licencias sin el consentimiento previo del cliente. Para obtener más información, consulte [Complemento de privacidad de SQL Server](/sql/sql-server/sql-server-privacy#non-personal-data).


## <a name="feature-benefits"></a>Ventajas de las características 

La extensión del Agente de IaaS de SQL Server desbloquea una serie de ventajas de características para administrar la VM con SQL Server. 

En la siguiente tabla se detallan estas ventajas: 


| Característica | Descripción |
| --- | --- |
| **Administración de portal** | Desbloquea la [administración en el portal](manage-sql-vm-portal.md), lo que permite ver todas las VM con SQL Server en un solo lugar, así como habilitar y deshabilitar características específicas de SQL directamente desde el portal. 
| **Copia de seguridad automatizada** |Automatiza la programación de copias de seguridad de todas las bases de datos para la instancia predeterminada o una instancia con nombre [instalada de forma correcta](frequently-asked-questions-faq.md#administration) de SQL Server en la máquina virtual. Para más información, consulte [Automated Backup para SQL Server en Azure Virtual Machines (Resource Manager)](automated-backup-sql-2014.md). |
| **Aplicación de revisión automatizada** |Configura una ventana de mantenimiento durante la cual pueden tener lugar actualizaciones de seguridad importantes de Windows y SQL Server en la VM, de forma que pueda evitar las actualizaciones durante las horas punta de la carga de trabajo. Para más información, consulte [Automated Patching para SQL Server en Azure Virtual Machines (Resource Manager)](automated-patching.md). |
| **Integración de Azure Key Vault** |Permite instalar y configurar automáticamente Azure Key Vault en la máquina virtual SQL Server. Para más información, consulte [Configurar la integración de Azure Key Vault para SQL Server en Azure Virtual Machines (Resource Manager)](azure-key-vault-integration-configure.md). |
| **Licencias flexibles** | Ahorre en el costo mediante la [transición sin problemas](licensing-model-azure-hybrid-benefit-ahb-change.md) de la característica traiga su propia licencia (también conocida como Ventaja híbrida de Azure) al modelo de licencias de pago por uso y viceversa. | 
| **Versión o edición flexibles** | Si decide cambiar la [versión ](change-sql-server-version.md) o la [edición](change-sql-server-edition.md) de SQL Server, puede actualizar los metadatos en Azure Portal sin tener que volver a implementar toda la VM con SQL Server.  | 


## <a name="management-modes"></a>Modos de administración

La extensión IaaS de SQL tiene tres modos de administración:

- El modo **Lightweight** no requiere reiniciar SQL Server, pero solo permite cambiar la edición y el tipo de licencia de SQL Server. Use esta opción para máquinas virtuales con SQL Server con varias instancias o para participar en una instancia de clúster de conmutación por error (FCI). Este modo de administración mantiene los archivos binarios de la extensión del Agente de IaaS de SQL en el equipo, pero no instala el Agente. El modo ligero es el modo de administración predeterminado cuando se usa la característica [registro automático](sql-vm-resource-provider-automatic-registration.md), o cuando no se especifica un tipo de administración durante el registro manual. Si se usa el modo ligero, no se produce ningún impacto en la memoria ni en la CPU, ni conlleva costos asociados. Se recomienda registrar la máquina virtual con SQL Server en el modo ligero primero y, posteriormente, actualizar al modo completo durante una ventana de mantenimiento programado.

- El modo **Full** ofrece todas las funcionalidades, pero requiere reiniciar los permisos de administrador del sistema y SQL Server. Úselo para administrar una VM con SQL Server con una sola instancia. El modo completo instala dos servicios de Windows que tienen un impacto mínimo en la memoria y la CPU (se pueden supervisar mediante el administrador de tareas). No hay ningún costo asociado al uso del modo de administración completa. 

- El modo **NoAgent** está dedicado a SQL Server 2008 y SQL Server 2008 R2 instalados en Windows Server 2008. Si se usa el modo NoAgent, ni la memoria ni la CPU resultan afectadas. No hay ningún costo asociado al uso del modo de administración NoAgent. 

Puede ver el modo actual del Agente de IaaS de SQL Server mediante Azure PowerShell: 

  ```powershell-interactive
  # Get the SqlVirtualMachine
  $sqlvm = Get-AzSqlVM -Name $vm.Name  -ResourceGroupName $vm.ResourceGroupName
  $sqlvm.SqlManagementType
  ```

## <a name="installation"></a>Instalación

La extensión del Agente de IaaS de SQL Server se instala al registrar las VM de SQL Server con el proveedor de recursos de VM de SQL. Al registrarse con el proveedor de recursos, se crea el _recurso_ de **máquina virtual de SQL** en su suscripción, que es un recurso _independiente_ del recurso de máquina virtual. Al anular el registro de la máquina virtual SQL Server del proveedor de recursos, se eliminará el **recurso** de la _máquina virtual SQL_ pero no se eliminará la máquina virtual real.

La implementación de una imagen de Azure Marketplace de una máquina virtual con SQL Server mediante Azure Portal registra la máquina virtual con SQL Server con el proveedor de recursos. Sin embargo, si elige instalar automáticamente SQL Server en una máquina virtual de Azure, o aprovisionar una máquina virtual de Azure desde un disco duro virtual personalizado, debe registrar la VM con SQL Server con el proveedor de recursos de VM con SQL para instalar la extensión del Agente de IaaS de SQL. 

Para instalar la extensión, registre la VM con SQL Server con el proveedor de recursos:
- [Automáticamente para todas las VM actuales y futuras de una suscripción](sql-vm-resource-provider-automatic-registration.md)
- [Para una sola VM](sql-vm-resource-provider-register.md)
- [Para varias VM en masa](sql-vm-resource-provider-bulk-register.md)

### <a name="named-instance-support"></a>Compatibilidad de las instancias con nombre

La extensión de IaaS SQL Server funcionará con una instancia con nombre de SQL Server si esa es la única instancia de SQL Server disponible en la máquina virtual. La extensión no se instalará en las VM que tengan varias instancias de SQL Server. 

Para usar una instancia con nombre de SQL Server, implemente una máquina virtual de Azure, instale una única instancia con nombre de SQL Server y, a continuación, regístrela en el [proveedor de recursos de VM con SQL](sql-vm-resource-provider-register.md) para instalar la extensión.

Como alternativa, para usar una instancia con nombre con una imagen de SQL Server de Azure Marketplace, siga estos pasos: 

   1. Implemente una VM SQL Server desde Azure Marketplace. 
   1. [Anule el registro](sql-vm-resource-provider-register.md#unregister-from-rp) de la VM con SQL Server del proveedor de recursos de VM con SQL. 
   1. Desinstale SQL Server por completo de la máquina virtual SQL Server.
   1. Instale SQL Server con una instancia con nombre en la máquina virtual SQL Server. 
   1. Instale la extensión del Agente de IaaS de SQL. Para ello, [registre la VM con SQL Server mediante el proveedor de recursos de VM con SQL](sql-vm-resource-provider-register.md#register-with-rp). 

## <a name="verify-status-of-extension"></a>Comprobación del estado de la extensión

Use Azure Portal o Azure PowerShell para comprobar el estado de la extensión. 


### <a name="azure-portal"></a>Azure portal

Compruebe que la extensión esté instalada en Azure Portal. 

Seleccione **Todas las configuraciones** en el panel de la máquina virtual y seleccione **Extensiones**. Debería aparecer la extensión **SqlIaasExtension**.

![Estado de la extensión del Agente de IaaS de SQL Server en Azure Portal](./media/sql-server-iaas-agent-extension-automate-management/azure-rm-sql-server-iaas-agent-portal.png)


### <a name="azure-powershell"></a>Azure PowerShell

También puede usar el cmdlet de Azure PowerShell **Get-AzVMSqlServerExtension** :

   ```powershell-interactive
   Get-AzVMSqlServerExtension -VMName "vmname" -ResourceGroupName "resourcegroupname"
   ```

El comando anterior confirma que el agente está instalado y proporciona información de estado general. Puede obtener información de estado específica sobre Automated Backup y Automated Patching con los siguientes comandos:

   ```powershell-interactive
    $sqlext = Get-AzVMSqlServerExtension -VMName "vmname" -ResourceGroupName "resourcegroupname"
    $sqlext.AutoPatchingSettings
    $sqlext.AutoBackupSettings
   ```


## <a name="limitations"></a>Limitaciones

La extensión del Agente de IaaS de SQL solo admite: 

- Máquinas virtuales con SQL Server implementadas mediante Azure Resource Manager. No se admiten las máquinas virtuales con SQL Server implementadas con el modelo clásico. 
- Las máquinas virtuales SQL Server implementadas en la nube pública o Azure Government. No se admiten las implementaciones en otras nubes públicas o privadas. 


## <a name="frequently-asked-questions"></a>Preguntas más frecuentes 

**¿Debo registrar mi máquina virtual con SQL Server aprovisionada a partir de una imagen de SQL Server en Azure Marketplace?**

No. Microsoft registra automáticamente las máquinas virtuales aprovisionadas a partir de imágenes de SQL Server en Azure Marketplace. El registro con el proveedor de recursos de máquina virtual con SQL solo es necesario si la máquina virtual *no* se aprovisionó a partir de las imágenes de SQL Server en Azure Marketplace y SQL Server se instaló automáticamente.

**¿El proveedor de recursos de VM de SQL está disponible para todos los clientes?** 

Sí. Los clientes deben registrar sus máquinas virtuales con SQL Server con el proveedor de recursos de máquina virtual con SQL si no usaron una imagen de SQL Server de Azure Marketplace y, en su lugar, se instaló automáticamente SQL Server o si usan su propio VHD personalizado. Las máquinas virtuales propiedad de todos los tipos de suscripciones (directa, Contrato Enterprise y proveedor de soluciones en la nube) se pueden registrar con el proveedor de recursos de máquina virtual con SQL.

**¿Cuál es el modo de administración predeterminado al registrarse con el proveedor de recursos de máquina virtual con SQL?**

El modo de administración predeterminado al registrarse con el proveedor de recursos de VM con SQL es *ligero*. Si no se establece la propiedad de administración de SQL Server al registrarse con el proveedor de recursos de VM con SQL, se establecerá el modo de administración ligero y el servicio SQL Server no se reiniciará. Se recomienda registrar la máquina virtual con el proveedor de recursos de máquina virtual con SQL en el modo ligero primero y, posteriormente, actualizar al modo completo durante una ventana de mantenimiento. Del mismo modo, la administración predeterminada también es ligera cuando se usa la [característica de registro automático](sql-vm-resource-provider-automatic-registration.md).

**¿Cuáles son los requisitos previos para registrarse con el proveedor de recursos de máquina virtual con SQL?**

No existe ningún requisito previo para el registro con el proveedor de recursos de VM con SQL a parte de tener SQL Server instalado en la VM. Tenga en cuenta que si la extensión del Agente de IaaS de SQL se instala en modo completo, el servicio SQL Server se reiniciará, por lo que se recomienda hacerlo durante una ventana de mantenimiento.

**Durante el registro con el proveedor de recursos de máquina virtual con SQL, ¿se instalará un agente en mi máquina virtual?**

Sí, el registro con el proveedor de recursos de VM con SQL en el modo de administración completa instala un agente en la VM. No si se realiza el registro en modo ligero o NoAgent. 

El registro con el proveedor de recursos de VM con SQL en modo ligero solo copia los *archivos binarios* de la extensión del Agente de IaaS de SQL en la VM, pero no instala el agente. Estos archivos binarios se utilizan posteriormente para instalar el agente cuando el modo de administración se actualiza a completo.


**Durante el registro con el proveedor de recursos de VM con SQL, ¿se reiniciará SQL Server en mi VM?**

Depende del modo especificado durante el registro. Si se especifica el modo ligero o NoAgent, el servicio SQL Server no se reiniciará. Sin embargo, si se especifica el modo de administración como completo, el servicio SQL Server se reiniciará. La característica de registro automático registra las VM con SQL Server en modo ligero, a menos que la versión de Windows Server sea 2008, en cuyo caso la VM con SQL Server se registrará en el modo NoAgent. 

**¿Cuál es la diferencia entre los modos de administración ligero y NoAgent al registrarse con el proveedor de recursos de VM con SQL?** 

El modo de administración NoAgent es el único modo de administración disponible para SQL Server 2008 y SQL Server 2008 R2 en Windows Server 2008. En todas las versiones posteriores de Windows Server, los dos modos de administración disponibles son ligero y completo. 

El modo NoAgent requiere que el cliente establezca las propiedades de la versión y edición de SQL Server. El modo ligero consulta a la máquina virtual para encontrar la versión y la edición de la instancia de SQL Server.

**¿Puedo registrarme con el proveedor de recursos de máquina virtual con SQL sin especificar el tipo de licencia de SQL Server?**

No. El tipo de licencia de SQL Server no es una propiedad opcional cuando se registra con el proveedor de recursos de máquina virtual con SQL. Tiene que establecer el tipo de licencia de SQL Server como pago por uso o Ventaja híbrida de Azure al registrarse con el proveedor de recursos de VM con SQL en todos los modos de administración (NoAgent, ligero y completo). Si tiene alguna de las versiones gratuitas de SQL Server instaladas, como Developer o Evaluation Edition, debe registrar las licencias de pago por uso. Ventaja híbrida de Azure solo está disponible para las versiones de pago de SQL Server, como Enterprise y Standard Edition.

**¿Se puede actualizar la extensión de IaaS de SQL Server del modo NoAgent al modo completo?**

No. La actualización del modo de administración a completo o ligero no está disponible para el modo NoAgent. Se trata de una limitación técnica de Windows Server 2008. Tendrá que actualizar primero el sistema operativo a Windows Server 2008 R2 o posterior y, a continuación, podrá actualizar al modo de administración completa. 

**¿Se puede actualizar la extensión IaaS de SQL Server del modo ligero al modo completo?**

Sí. La actualización del modo de administración de ligero a completo se admite a través de Azure PowerShell o Azure Portal. Esto desencadenará un reinicio del servicio SQL Server.

**¿Se puede cambiar la extensión de IaaS de SQL Server del modo completo a los modos inferiores de administración NoAgent o ligero?**

No. No se admite cambiar a una versión inferior del modo de administración de la extensión IaaS de SQL Server. El modo de administración no se puede cambiar a una versión inferior, del modo completo al modo ligero o NoAgent, ni del modo ligero al modo NoAgent. 

Para cambiar el modo de administración de completo a otro, [anule el registro](sql-vm-resource-provider-register.md#unregister-from-rp) de la VM con SQL Server del proveedor de recursos de VM con SQL. Para ello, quite el _recurso_ de la máquina virtual con SQL y vuelva a registrar la VM con SQL Server con el proveedor de recursos de VM con SQL de nuevo en un modo de administración diferente.

**¿Puedo registrarme en un proveedor de recursos de máquina virtual con SQL desde Azure Portal?**

No. El registro en un proveedor de recursos de máquina virtual con SQL no está disponible en Azure Portal. El registro con el proveedor de recursos de VM con SQL solo se admite con la CLI de Azure o Azure PowerShell. 

**¿Puedo registrar una máquina virtual con el proveedor de recursos de máquina virtual con SQL antes de instalar SQL Server?**

No. Una VM debe tener al menos una instancia de SQL Server (Motor de base de datos) para registrarse correctamente con el proveedor de recursos de VM con SQL. Si no hay ninguna instancia de SQL Server en la máquina virtual, el nuevo recurso Microsoft.SqlVirtualMachine tendrá un estado de error.

**¿Puedo registrar una máquina virtual en un proveedor de recursos de máquina virtual con SQL si hay varias instancias de SQL Server?**

Sí. El proveedor de recursos de máquina virtual con SQL solo registrará una instancia de SQL Server (Motor de base de datos). El proveedor de recursos de máquina virtual con SQL registrará la instancia de SQL Server predeterminada en caso de varias instancias. Si no hay ninguna instancia predeterminada, solo se admite el registro en modo ligero. Para actualizar del modo ligero al modo de administración completa, debe existir la instancia de SQL Server predeterminada o la máquina virtual debe tener solo una instancia de SQL Server con nombre.

**¿Se puede registrar una instancia de clúster de conmutación por error de SQL Server con el proveedor de recursos de máquina virtual con SQL?**

Sí. Las instancias de clúster de conmutación por error de SQL Server en una máquina virtual de Azure se pueden registrar con el proveedor de recursos de máquina virtual con SQL en modo ligero. Sin embargo, las instancias de clúster de conmutación por error de SQL Server no se pueden actualizar al modo de administración completa.

**¿Puedo registrar mi máquina virtual con el proveedor de recursos de máquina virtual con SQL si el grupo de disponibilidad AlwaysOn está configurado?**

Sí. No hay ninguna restricción para registrar una instancia de SQL Server instalada en una máquina virtual de Azure con el proveedor de recursos de máquina virtual con SQL si participa en una configuración de grupo de disponibilidad AlwaysOn.

**¿Cuál es el costo del registro en el proveedor de recursos de máquina virtual de SQL o de la actualización al modo de administración completa?**

Ninguno. No hay ningún precio asociado al registro en el proveedor de recursos de máquina virtual de SQL ni al uso de ninguno de los tres modos de manejabilidad. La administración de una VM con SQL Server con el proveedor de recursos es totalmente gratuita. 

**¿Como afecta el uso de los distintos modos de administración al rendimiento?**

El uso de los modos de manejabilidad *NoAgent* y *ligero* no tiene ningún impacto en el rendimiento. Si se usa el modo de manejabilidad *completa* desde dos servicios instalados en el sistema operativo, el impacto es mínimo. Estos se pueden supervisar mediante el administrador de tareas y se muestran en la consola de Servicios de Windows integrada. 

Los dos nombres de servicio son:
- `SqlIaaSExtensionQuery` (nombre para mostrar - `Microsoft SQL Server IaaS Query Service`)
- `SQLIaaSExtension` (nombre para mostrar - `Microsoft SQL Server IaaS Agent`)

**¿Cómo se quita la extensión?**

Para quitar la extensión, [anule el registro](sql-vm-resource-provider-register.md#unregister-from-rp) de la VM con SQL Server del proveedor de recursos de VM con SQL. 

## <a name="next-steps"></a>Pasos siguientes

Para instalar la extensión de IaaS de SQL Server en SQL Server en las VM de Azure, consulte los artículos sobre [instalación automática](sql-vm-resource-provider-automatic-registration.md), [VM únicas](sql-vm-resource-provider-register.md) o [VM en masa](sql-vm-resource-provider-bulk-register.md).

Para obtener más información sobre cómo ejecutar SQL Server en Azure Virtual Machines, consulte [¿Qué es SQL Server en Azure Virtual Machines?](sql-server-on-azure-vm-iaas-what-is-overview.md).
