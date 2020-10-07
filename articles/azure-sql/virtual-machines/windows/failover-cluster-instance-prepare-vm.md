---
title: Preparación de máquinas virtuales para una FCI
description: Prepare las máquinas virtuales de Azure para utilizarlas con una instancia de clúster de conmutación por error (FCI) y SQL Server en máquinas virtuales de Azure.
services: virtual-machines
documentationCenter: na
author: MashaMSFT
editor: monicar
tags: azure-service-management
ms.service: virtual-machines-sql
ms.topic: how-to
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 06/02/2020
ms.author: mathoma
ms.openlocfilehash: f42d6c8015061406958bdc16473dc0f042d3143a
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/25/2020
ms.locfileid: "91272508"
---
# <a name="prepare-virtual-machines-for-an-fci-sql-server-on-azure-vms"></a>Preparación de máquinas virtuales para una FCI (SQL Server en máquinas virtuales de Azure)
[!INCLUDE[appliesto-sqlvm](../../includes/appliesto-sqlvm.md)]

En este artículo se describe cómo preparar máquinas virtuales (VM) de Azure para utilizarlas con una instancia de clúster de conmutación por error (FCI) de SQL Server. La configuración varía en función de la solución de almacenamiento de una FCI, por lo que debe asegurarse de que elige la configuración correcta para su entorno y su empresa. 

Para más información, consulte la información general de [FCI con SQL Server en VM de Azure](failover-cluster-instance-overview.md) y [Procedimientos recomendados de clúster](hadr-cluster-best-practices.md). 

## <a name="prerequisites"></a>Requisitos previos 

- Una suscripción de Microsoft Azure. Comience a utilizarla [gratis](https://azure.microsoft.com/free/). 
- Un dominio de Windows en máquinas virtuales de Azure o un centro de datos local extendido a Azure con emparejamiento de red virtual.
- Una cuenta que tenga permisos para crear objetos en máquinas virtuales de Azure y en Active Directory.
- Una red virtual de Azure y una subred con espacio de direcciones IP suficiente para estos componentes:
   - Ambas máquinas virtuales
   - La dirección IP del clúster de conmutación por error de Windows
   - Una dirección IP para cada FCI
- DNS configurado en la red de Azure que señala a los controladores de dominio.

## <a name="choose-an-fci-storage-option"></a>Elección de una opción de almacenamiento de FCI

La configuración de la máquina virtual varía en función de la opción de almacenamiento que piensa utilizar para la instancia de clúster de conmutación por error de SQL Server. Antes de preparar la máquina virtual, revise las [opciones de almacenamiento de FCI disponibles](failover-cluster-instance-overview.md#storage), y elija la opción que mejor se adapte a sus necesidades empresariales y de entorno. Después, seleccione cuidadosamente las opciones de configuración de máquinas virtuales adecuadas en este artículo según la selección de almacenamiento. 

## <a name="configure-vm-availability"></a>Configuración de la disponibilidad de máquinas virtuales 

La característica de clúster de conmutación por error requiere que las máquinas virtuales se coloquen en un [conjunto de disponibilidad](../../../virtual-machines/linux/tutorial-availability-sets.md) o en una [zona de disponibilidad](../../../availability-zones/az-overview.md#availability-zones). Si elige conjuntos de disponibilidad, puede utilizar [grupos con ubicación por proximidad](../../../virtual-machines/windows/co-location.md#proximity-placement-groups) para ubicar las máquinas virtuales más cerca. De hecho, los grupos con ubicación por proximidad son un requisito previo para el uso de discos compartidos de Azure. 

Seleccione cuidadosamente la opción de disponibilidad de la máquina virtual que coincida con la configuración de clúster prevista: 

 - **Discos compartidos de Azure**: [Conjunto de disponibilidad](../../../virtual-machines/windows/tutorial-availability-sets.md#create-an-availability-set) configurado con el dominio de error y el dominio de actualización establecido en 1, y colocado dentro de un [grupo con ubicación por proximidad](../../../virtual-machines/windows/proximity-placement-groups-portal.md).
 - **Recursos compartidos de archivos Premium**: [Conjunto de disponibilidad](../../../virtual-machines/windows/tutorial-availability-sets.md#create-an-availability-set) o [zona de disponibilidad](../../../virtual-machines/windows/create-portal-availability-zone.md#confirm-zone-for-managed-disk-and-ip-address). Los recursos compartidos de archivos Premium son la única opción de almacenamiento compartido si elige las zonas de disponibilidad como configuración de disponibilidad para las máquinas virtuales. 
 - **Espacios de almacenamiento directo**: [Conjunto de disponibilidad](../../../virtual-machines/windows/tutorial-availability-sets.md#create-an-availability-set).

>[!IMPORTANT]
>Una vez creada una máquina virtual el conjunto de disponibilidad no se puede establecer o cambiar.

## <a name="create-the-virtual-machines"></a>Creación de las máquinas virtuales

Después de configurar la disponibilidad de la máquina virtual, está preparado para crear máquinas virtuales. Puede optar por utilizar una imagen de Azure Marketplace, ya tenga o no SQL Server instalado. Sin embargo, si elige una imagen para SQL Server en máquinas virtuales de Azure, deberá desinstalar SQL Server de la máquina virtual antes de configurar la instancia de clúster de conmutación por error. 


Coloque ambas máquinas virtuales:

- En el mismo grupo de recursos de Azure en el que se encuentra su conjunto de disponibilidad, si está utilizando este tipo de conjuntos.
- En la misma red virtual que el controlador de dominio.
- En una subred que tenga suficiente espacio de direcciones IP para ambas máquinas virtuales y todas las FCI que finalmente puede utilizar en este clúster.
- En el conjunto de disponibilidad o la zona de disponibilidad de Azure.

Puede crear una máquina virtual de Azure mediante una imagen [con](sql-vm-create-portal-quickstart.md) o [sin](../../../virtual-machines/windows/quick-create-portal.md) SQL Server preinstalado. Si elige la imagen con SQL Server, deberá desinstalar manualmente la instancia de SQL Server antes de instalar la instancia de clúster de conmutación por error. 


## <a name="uninstall-sql-server"></a>Desinstalar SQL Server

Como parte del proceso de creación de la FCI, instalará SQL Server como una instancia en clúster en el clúster de conmutación por error. *Si ha implementado una máquina virtual con una imagen de Azure Marketplace sin SQL Server, puede omitir este paso.* Si ha implementado una imagen con SQL Server preinstalado, tendrá que anular el registro de la máquina virtual con SQL Server en el proveedor de recursos de máquina virtual con SQL y, a continuación, desinstalar SQL Server. 

### <a name="unregister-from-the-sql-vm-resource-provider"></a>Anulación del registro en el proveedor de recursos de máquina virtual con SQL

Las imágenes de máquinas virtuales con SQL Server de Azure Marketplace se registran automáticamente con el proveedor de recursos de máquina virtual con SQL. Antes de desinstalar la instancia con SQL Server preinstalado, primero debe [anular el registro de cada máquina virtual con SQL Server en el proveedor de recursos de máquina virtual de SQL](sql-vm-resource-provider-register.md#unregister-from-rp). 

### <a name="uninstall-sql-server"></a>Desinstalar SQL Server

Después de anular el registro del proveedor de recursos, puede desinstalar SQL Server. Siga estos pasos en cada máquina virtual: 

1. Conéctese a la máquina virtual mediante RDP.

   La primera vez que se conecte a una máquina virtual con RDP, un mensaje le pregunta si desea permitir que el equipo se pueda detectar en la red. Seleccione **Sí**.

1. Si utiliza una de las imágenes de máquina virtual basadas en SQL Server, quite la instancia de SQL Server:

   1. En **Programas y características**, haga clic con el botón derecho en **Microsoft SQL Server 201_ (64 bits)** y seleccione **Desinstalar o cambiar**.
   1. Seleccione **Quitar**.
   1. Seleccione la instancia predeterminada.
   1. Quite todas las características en **Servicios de motor de base de datos**. No quite nada en **Características compartidas**. Verá algo parecido a la siguiente captura de pantalla:

      ![Selección de características](./media/failover-cluster-instance-prepare-vm/03-remove-features.png)

   1. Seleccione **Siguiente** y después **Quitar**.
   1. Una vez quitada correctamente la instancia, reinicie la máquina virtual. 

## <a name="open-the-firewall"></a>Apertura del firewall 

En cada máquina virtual, abra el puerto TCP de Firewall de Windows que SQL Server utiliza. De forma predeterminada, es el puerto 1433. Sin embargo, puede cambiar el puerto de SQL Server en una implementación de máquina virtual de Azure, por lo que debe abrir el puerto que utiliza SQL Server en su entorno. Este puerto se abre automáticamente en las imágenes SQL Server implementadas desde Azure Marketplace. 

Si utiliza un [equilibrador de carga](hadr-vnn-azure-load-balancer-configure.md), también deberá abrir el puerto que usa el sondeo de estado. De forma predeterminada, es el puerto 59999. Pero puede ser cualquier puerto TCP que especifique al crear el equilibrador de carga. 

En esta tabla se detallan los puertos que es posible que necesite abrir, en función de la configuración de la FCI: 

   | Propósito | Port | Notas
   | ------ | ------ | ------
   | SQL Server | TCP 1433 | Puerto normal para las instancias predeterminadas de SQL Server. Si usó una imagen de la galería, este puerto se abre automáticamente. </br> </br> **Utilizado por**: Todas las configuraciones de FCI. |
   | Sondeo de mantenimiento | TCP 59999 | Cualquier puerto TCP abierto. Configure el [sondeo de estado](hadr-vnn-azure-load-balancer-configure.md#configure-health-probe) del equilibrador de carga y el clúster para utilizar este puerto. </br> </br> **Utilizado por**: FCI con equilibrador de carga. |
   | Recurso compartido de archivos | UDP 445 | Puerto que el servicio del recurso compartido de archivos utiliza. </br> </br> **Utilizado por**: FCI con recurso compartido de archivos Premium. |

## <a name="join-the-domain"></a>Unir al dominio

También debe unir las máquinas virtuales al dominio. Puede hacerlo mediante una [plantilla de inicio rápido](../../../active-directory-domain-services/join-windows-vm-template.md#join-an-existing-windows-server-vm-to-a-managed-domain). 

## <a name="review-storage-configuration"></a>Revisión de la configuración de almacenamiento

Las máquinas virtuales creadas en Azure Marketplace vienen con un almacenamiento asociado. Si planea configurar el almacenamiento de FCI mediante recursos compartidos de archivos Premium o discos compartidos de Azure, puede quitar el almacenamiento asociado para ahorrar en los costos porque el almacenamiento local no se utiliza para la instancia de clúster de conmutación por error. Sin embargo, es posible utilizar el almacenamiento asociado para las soluciones de FCI de espacios de almacenamiento directo, así que eliminarlos podría ser inútil en este caso. Revise la solución de almacenamiento de FCI para determinar si la eliminación del almacenamiento asociado ayuda a ahorrar costos. 


## <a name="next-steps"></a>Pasos siguientes

Ahora que ha preparado el entorno de la máquina virtual, está preparado para configurar la instancia de clúster de conmutación por error. 

Elija una de las siguientes guías para configurar el entorno de FCI que sea adecuado para su empresa: 
- [Configuración de una FCI con discos compartidos de Azure](failover-cluster-instance-azure-shared-disks-manually-configure.md)
- [Configuración de una FCI con un recurso compartido Premium](failover-cluster-instance-premium-file-share-manually-configure.md)
- [Configuración de una FCI con espacios de almacenamiento directo](failover-cluster-instance-storage-spaces-direct-manually-configure.md)

Para más información, consulte la información general de [FCI con SQL Server en VM de Azure](failover-cluster-instance-overview.md) y las [configuraciones de HADR admitidas](hadr-cluster-best-practices.md). 

Para información adicional, consulte: 
- [Tecnologías de clúster de Windows](/windows-server/failover-clustering/failover-clustering-overview)   
- [Instancias del clúster de conmutación por error de SQL Server](/sql/sql-server/failover-clusters/windows/always-on-failover-cluster-instances-sql-server)
