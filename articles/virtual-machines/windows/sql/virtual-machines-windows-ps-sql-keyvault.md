---
title: Integración de Key Vault con SQL Server en máquinas virtuales Windows en Azure (Resource manager) | Microsoft Docs
description: Aprenda a automatizar la configuración de cifrado de SQL Server para su uso con Azure Key Vault. Este tema explica cómo usar la integración de Azure Key Vault con máquinas virtuales de SQL Server creadas con Resource Manager.
services: virtual-machines-windows
documentationcenter: ''
author: MashaMSFT
manager: craigg
editor: ''
tags: azure-service-management
ms.assetid: cd66dfb1-0e9b-4fb0-a471-9deaf4ab4ab8
ms.service: virtual-machines-sql
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 04/30/2018
ms.author: mathoma
ms.reviewer: jroth
ms.openlocfilehash: cad70169e88e1fafa129c02f30d5288d39e30a9c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "70102145"
---
# <a name="configure-azure-key-vault-integration-for-sql-server-on-azure-virtual-machines-resource-manager"></a>Configurar la integración de Azure Key Vault para SQL Server en Azure Virtual Machines (Resource Manager)

> [!div class="op_single_selector"]
> * [Resource Manager](virtual-machines-windows-ps-sql-keyvault.md)
> * [Clásico](../sqlclassic/virtual-machines-windows-classic-ps-sql-keyvault.md)

## <a name="overview"></a>Información general
SQL Server tiene varias características de cifrado, como el [cifrado de datos transparente (TDE)](https://msdn.microsoft.com/library/bb934049.aspx), el [cifrado de nivel de columna (CLE)](https://msdn.microsoft.com/library/ms173744.aspx) y el [cifrado de copia de seguridad](https://msdn.microsoft.com/library/dn449489.aspx). Estas formas de cifrado requieren administrar y almacenar las claves criptográficas que se usan para el cifrado. El servicio Azure Key Vault (AKV) está diseñado para mejorar la seguridad y la administración de estas claves en una ubicación segura y con gran disponibilidad. El [conector de SQL Server](https://www.microsoft.com/download/details.aspx?id=45344) permite que SQL Server use estas claves de Azure Key Vault.

Si ejecuta SQL Server con máquinas locales, hay una serie de [pasos que puede seguir para acceder a Azure Key Vault desde la máquina de SQL Server local](https://msdn.microsoft.com/library/dn198405.aspx). Pero para SQL Server en las máquinas virtuales de Azure, puede ahorrar tiempo usando la característica *Integración de Azure Key Vault* .

Cuando se habilita esta característica, automáticamente se instala el conector de SQL Server, se configura el proveedor EKM para obtener acceso a Azure Key Vault y se crea la credencial para que pueda tener acceso a su almacén. Si examinamos los pasos descritos en la documentación local que se mencionó anteriormente, puede ver que esta característica automatiza los pasos 2 y 3. Lo único que aún tiene que hacer manualmente es crear el Almacén de claves y las claves. Desde allí, se automatiza toda la configuración de la máquina virtual de SQL. Cuando esta característica haya completado el programa de instalación, puede ejecutar instrucciones de T-SQL para empezar a cifrar sus bases de datos o copias de seguridad como lo haría normalmente.

[!INCLUDE [AKV Integration Prepare](../../../../includes/virtual-machines-sql-server-akv-prepare.md)]

  >[!NOTE]
  > La versión 1.0.4.0 del proveedor EKM está instalada en la VM con SQL Server a través de la [extensión IaaS de SQL](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-server-agent-extension). Al actualizar la extensión IaaS de SQL no se actualizará la versión del proveedor. Considere la posibilidad de actualizar manualmente la versión del proveedor EKM si es necesario (por ejemplo, al migrar a una instancia administrada de SQL).


## <a name="enabling-and-configuring-akv-integration"></a>Habilitación y configuración de la integración de Almacén de claves de Azure
Puede habilitar la integración de Almacén de claves de Azure durante el aprovisionamiento o configurarlo para las máquinas virtuales existentes.

### <a name="new-vms"></a>Nuevas máquinas virtuales
Si está aprovisionando una nueva máquina virtual de SQL Server con Resource Manager, Azure Portal permite habilitar la integración de Azure Key Vault. La característica Azure Key Vault está disponible solo para las ediciones Enterprise, Developer y Evaluation de SQL Server.

![Integración de Azure Key Vault en SQL](./media/virtual-machines-windows-ps-sql-keyvault/azure-sql-arm-akv.png)

Para ver un tutorial detallado del aprovisionamiento, consulte [Aprovisionamiento de una máquina virtual con SQL Server en Azure Portal](virtual-machines-windows-portal-sql-server-provision.md).

### <a name="existing-vms"></a>Máquinas virtuales existentes

[!INCLUDE [windows-virtual-machines-sql-use-new-management-blade](../../../../includes/windows-virtual-machines-sql-new-resource.md)]

En el caso de las máquinas de virtuales con SQL Server existentes, abra su [recurso máquinas virtuales SQL](virtual-machines-windows-sql-manage-portal.md#access-the-sql-virtual-machines-resource) y seleccione **Seguridad** en **Configuración**. Seleccione **Habilitar** para habilitar la integración de Azure Key Vault. 

![Integración de Almacén de claves de Azure de SQL para máquinas virtuales existentes](./media/virtual-machines-windows-ps-sql-keyvault/azure-sql-rm-akv-existing-vms.png)

Cuando termine, seleccione el botón **Aplicar** de la parte inferior de página **Seguridad** para guardar los cambios.

> [!NOTE]
> El nombre de la credencial que se ha creado aquí se asignará más tarde a un inicio de sesión de SQL. Esto permite que el inicio de sesión de SQL tenga acceso al almacén de claves. 


> [!NOTE]
> También puede usar una plantilla para configurar la integración de Almacén de claves de Azure. Para más información, consulte la [plantilla de inicio rápido de Azure para la integración de Azure Key Vault](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-sql-existing-keyvault-update).


[!INCLUDE [AKV Integration Next Steps](../../../../includes/virtual-machines-sql-server-akv-next-steps.md)]
