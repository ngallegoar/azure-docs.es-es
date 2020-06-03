---
title: Cambios en la documentación de SQL Server en Azure Virtual Machines | Microsoft Docs
description: Conozca las nuevas características y mejoras de SQL Server en una máquina virtual de Azure
services: virtual-machines-windows
author: MashaMSFT
ms.author: mathoma
manager: craigg
tags: azure-service-management
ms.assetid: 2fa5ee6b-51a6-4237-805f-518e6c57d11b
ms.service: virtual-machines-sql
ms.topic: conceptual
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 01/06/2020
ms.openlocfilehash: f87d72df977e98c60948389d794eb102ac08f8d2
ms.sourcegitcommit: 053e5e7103ab666454faf26ed51b0dfcd7661996
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/27/2020
ms.locfileid: "84032476"
---
# <a name="documentation-changes-for-sql-server-on-azure-virtual-machines"></a>Cambios en la documentación de SQL Server en Azure Virtual Machines
[!INCLUDE[appliesto-sqlvm](../../includes/appliesto-sqlvm.md)]

Azure permite implementar una máquina virtual (VM) con una imagen de SQL Server integrada. En este artículo se resumen los cambios en la documentación asociados a las nuevas características y mejoras de las versiones recientes de [SQL Server en Azure Virtual Machines](https://azure.microsoft.com/services/virtual-machines/sql-server/). 


## <a name="january-2020"></a>Enero de 2020

| Cambios | Detalles |
| --- | --- |
| **Soporte de Azure Government** | Ahora es posible registrar máquinas virtuales de SQL Server con el proveedor de recursos de SQL VM para máquinas virtuales alojadas en la nube [Azure Government](https://azure.microsoft.com/global-infrastructure/government/). | 
| &nbsp; | &nbsp; |

## <a name="2019"></a>2019

|Cambios | Detalles |
 --- | --- |
| **Réplica de DR gratuita en Azure** | Puede hospedar una [instancia pasiva gratuita](business-continuity-high-availability-disaster-recovery-hadr-overview.md#free-dr-replica-in-azure) para la recuperación ante desastres en Azure para su instancia de SQL Server local si tiene [Software Assurance](https://www.microsoft.com/licensing/licensing-programs/software-assurance-default?rtc=1&activetab=software-assurance-default-pivot:primaryr3). | 
| **Registro masivo del proveedor de recursos** | Ahora puede [registrar masivamente](sql-vm-resource-provider-bulk-register.md) máquinas virtuales con SQL con el proveedor de recursos. | 
|**Configuración del almacenamiento optimizado para el rendimiento** | Ya se puede [personalizar totalmente la configuración del almacenamiento](storage-configuration.md#new-vms) al crear una nueva máquina virtual con SQL Server. |
|**Recursos compartidos de archivos premium para FCI** | Ahora puede crear una instancia de clúster de conmutación por error mediante un [recurso compartido de archivos Premium](failover-cluster-instance-premium-file-share-manually-configure.md) en lugar del método original de [Espacios de almacenamiento directo](failover-cluster-instance-storage-spaces-direct-manually-configure.md). 
| **Host dedicado de Azure** | Puede ejecutar VM con SQL Server en un [host dedicado de Azure](dedicated-host.md). | 
| **Traslado de una máquina virtual de SQL a otra región** | Use Azure Site Recovery para [migrar las máquinas virtuales con SQL Server de una región de Azure a otra](move-sql-vm-different-region.md). |
|  **Nuevos modos de instalación de IaaS de SQL** | Ahora es posible instalar la extensión IaaS de SQL Server en [modo ligero](sql-server-iaas-agent-extension-automate-management.md) para evitar tener que reiniciar el servicio SQL Server.  |
| **Modificación de la edición de SQL Server** | Ahora puede cambiar la [propiedad edition](change-sql-server-edition.md) de la VM con SQL Server. |
| **Cambios con el proveedor de recursos de máquina virtual con SQL** | Puede [registrar la máquina virtual con SQL Server con el proveedor de recursos de máquina virtual con SQL](sql-vm-resource-provider-register.md) mediante los nuevos modos de IaaS de SQL. Esta funcionalidad incluye [imágenes de Windows Server 2008](sql-vm-resource-provider-register.md#management-modes).|
| **Imágenes del tipo "traiga su propia licencia" mediante la Ventaja híbrida de Azure** | Ahora es posible cambiar al [tipo de licencia de pago por uso](licensing-model-azure-hybrid-benefit-ahb-change.md#remarks) las imágenes del tipo "traiga su propia licencia" implementadas desde Azure Marketplace.| 
| **Nueva administración de máquinas virtuales con SQL Server en Azure Portal** | Ahora existe una nueva forma de administrar una máquina virtual con SQL Server en Azure Portal. Para más información, consulte [Administración de máquinas virtuales con SQL Server en Azure Portal](manage-sql-vm-portal.md).  | 
| **Compatibilidad ampliada con SQL Server 2008 y SQL Server 2008 R2** | [Amplíe la compatibilidad](sql-server-2008-extend-end-of-support.md) con SQL Server 2008 y SQL Server 2008 R2 mediante la migración *tal cual* a una máquina virtual de Azure. | 
| **Compatibilidad de imágenes personalizada** | Ahora puede instalar la [extensión IaaS de SQL Server](sql-server-iaas-agent-extension-automate-management.md#installation) en imágenes de SQL y sistemas operativos personalizadas, lo que ofrece una funcionalidad limitada de [licencias flexibles](licensing-model-azure-hybrid-benefit-ahb-change.md). Al registrar una imagen personalizada con el proveedor de recursos SQL, especifique el tipo de licencia como "AHUB". De lo contrario, el registro no podrá realizarse. | 
| **Compatibilidad de instancias con nombre** | Ahora puede usar la [extensión IaaS de SQL Server](sql-server-iaas-agent-extension-automate-management.md#installation) con una instancia con nombre, si la instancia predeterminada se ha desinstalado correctamente. | 
| **Renovación del portal** | La experiencia de Azure Portal para implementar una VM con SQL Server se ha renovado para mejorar la facilidad de uso. Para más información, vea el breve [inicio rápido](sql-vm-create-portal-quickstart.md) y la guía [paso a paso](create-sql-vm-portal.md) detallada para implementar una máquina virtual con SQL Server.|
| **Mejora del portal** | Ahora es posible cambiar el modelo de licencias para una máquina virtual con SQL Server del tipo "pago por uso" a "traiga su propia licencia" mediante [Azure Portal](licensing-model-azure-hybrid-benefit-ahb-change.md#vms-already-registered-with-the-resource-provider).|
| **Simplificación de la implementación del grupo de disponibilidad en máquinas virtuales con SQL Server con la CLI de Azure** | Ahora es más fácil que nunca implementar un grupo de disponibilidad en una máquina virtual con SQL Server en Azure. Puede usar la [CLI de Azure](/cli/azure/sql/vm?view=azure-cli-2018-03-01-hybrid) para crear el clúster de conmutación por error de Windows, el equilibrador de carga interno y los clientes de escucha de grupo de disponibilidad, todo ello desde la línea de comandos. Para más información, vea cómo [usar la CLI de Azure para configurar un grupo de disponibilidad Always On para SQL Server en una máquina virtual de Azure](availability-group-az-cli-configure.md). | 
| &nbsp; | &nbsp; |

## <a name="2018"></a>2018 

 Cambios | Detalles |
| --- | --- |
|  **Nuevo proveedor de recursos para un clúster de SQL Server** | Un nuevo proveedor de recursos (Microsoft.SqlVirtualMachine/SqlVirtualMachineGroups) que define los metadatos del clúster de conmutación por error de Windows. Al unir una máquina virtual con SQL Server a *SqlVirtualMachineGroups*, se arranca el servicio Clúster de conmutación por error de Windows Server (WSFC) y la máquina virtual se une al clúster.  |
| **Configuración automatizada de la implementación de un grupo de disponibilidad con plantillas de inicio rápido de Azure** |Ahora es posible crear el clúster de conmutación por error de Windows, unirlo a las máquinas virtuales con SQL Server, crear el cliente de escucha y configurar el equilibrador de carga interno con dos plantillas de inicio rápido de Azure. Para más información, consulte [Uso de plantillas de inicio rápido de Azure para configurar un grupo de disponibilidad Always On para SQL Server en una máquina virtual de Azure](availability-group-quickstart-template-configure.md). | 
| **Registro automático del proveedor de recursos de máquina virtual con SQL Server** | Las VM con SQL Server implementadas después de este mes se registran automáticamente con el nuevo proveedor de recursos de SQL Server. Las máquinas virtuales con SQL Server implementadas antes de este mes todavía deben registrarse manualmente. Para más información, consulte [Registro de una máquina virtual con SQL Server en Azure con el proveedor de recursos de máquina virtual con SQL](sql-vm-resource-provider-register.md).|
|**Nuevo proveedor de recursos de VM con SQL** |  Un nuevo proveedor de recursos (Microsoft.SqlVirtualMachine) proporciona una mejor administración de las máquinas virtuales con SQL Server. Para más información sobre cómo registrar las máquinas virtuales, consulte [Registro de una máquina virtual con SQL Server en Azure con el proveedor de recursos de máquina virtual con SQL](sql-vm-resource-provider-register.md). |
|**Cambiar el modelo de licencia** | Ahora puede cambiar entre los modelos de "pago por uso" y "traiga su propia licencia" para la máquina virtual con SQL Server mediante PowerShell o la CLI de Azure. Para más información, consulte [Modificación del modelo de licencia para una máquina virtual con SQL Server en Azure](licensing-model-azure-hybrid-benefit-ahb-change.md). | 
| &nbsp; | &nbsp; |

## <a name="additional-resources"></a>Recursos adicionales

**Máquinas virtuales Windows**:

* [Introducción a SQL Server en máquinas virtuales Windows](sql-server-on-azure-vm-iaas-what-is-overview.md)
* [Aprovisionamiento de una máquina virtual Windows con SQL Server](create-sql-vm-portal.md)
* [Migración de una base de datos a SQL Server en una máquina virtual de Azure](migrate-to-vm-from-sql-server.md)
* [Alta disponibilidad y recuperación ante desastres para SQL Server en Azure Virtual Machines](business-continuity-high-availability-disaster-recovery-hadr-overview.md)
* [Directrices de rendimiento para SQL Server en Azure Virtual Machines](performance-guidelines-best-practices.md)
* [Estrategias de desarrollo y patrones de aplicación de SQL Server en Azure Virtual Machines](application-patterns-development-strategies.md)

**Máquinas virtuales Linux**:

* [Introducción a SQL Server en máquinas virtuales Linux](../linux/sql-server-on-linux-vm-what-is-iaas-overview.md)
* [Aprovisionamiento de una máquina virtual Linux con SQL Server en Azure Portal](../linux/sql-vm-create-portal-quickstart.md)
* [Preguntas más frecuentes (Linux)](../linux/frequently-asked-questions-faq.md)
* [Documentación de SQL Server en Linux](https://docs.microsoft.com/sql/linux/sql-server-linux-overview)
