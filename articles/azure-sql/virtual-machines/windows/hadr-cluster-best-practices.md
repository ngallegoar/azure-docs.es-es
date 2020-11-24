---
title: Procedimientos recomendados de configuración de clústeres
description: Obtenga información sobre las configuraciones de clúster admitidas al configurar la alta disponibilidad y la recuperación ante desastres (HADR) para SQL Server en Azure Virtual Machines, por ejemplo, los cuórum admitidos o las opciones de enrutamiento de conexión.
services: virtual-machines
documentationCenter: na
author: MashaMSFT
editor: monicar
tags: azure-service-management
ms.service: virtual-machines-sql
ms.topic: conceptual
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 06/02/2020
ms.author: mathoma
ms.openlocfilehash: 86db8c88fae7a5fd1ec4828d8936c6cb8172a61c
ms.sourcegitcommit: 04fb3a2b272d4bbc43de5b4dbceda9d4c9701310
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/12/2020
ms.locfileid: "94564572"
---
# <a name="cluster-configuration-best-practices-sql-server-on-azure-vms"></a>Procedimientos recomendados para la configuración de clústeres (SQL Server en máquinas virtuales de Azure)
[!INCLUDE[appliesto-sqlvm](../../includes/appliesto-sqlvm.md)]

Un clúster se usa para conseguir una alta disponibilidad y la recuperación ante desastres (HADR) con SQL Server en Azure Virtual Machines. 

En este artículo se proporcionan procedimientos recomendados de configuración de clústeres tanto para [instancias de clúster de conmutación por error (FCI)](failover-cluster-instance-overview.md) como para [grupos de disponibilidad](availability-group-overview.md) cuando se usan con SQL Server en máquinas virtuales de Azure. 


## <a name="networking"></a>Redes

Use una sola NIC por servidor (nodo de clúster) y una sola subred. La red de Azure tiene redundancia física, lo que hace que las NIC y subredes adicionales sean innecesarias en un clúster invitado de máquina virtual de Azure. El informe de validación de clúster le avisará de que solo se puede tener acceso a los nodos en una sola red. Puede omitir esta advertencia en los clústeres de conmutación por error invitados de máquinas virtuales de Azure.

### <a name="tuning-failover-cluster-network-thresholds"></a>Ajuste de los umbrales de red de clústeres de conmutación por error

Al ejecutar nodos de clúster de conmutación por error Windows en máquinas virtuales de Azure con AlwaysOn para SQL Server, se recomienda cambiar la configuración del clúster a un estado de supervisión más flexible.  Esto hará que el clúster sea mucho más estable y confiable.  Para más información sobre esto, consulte [IaaS con SQL AlwaysOn: Ajuste de los umbrales de red de clústeres de conmutación por error](/windows-server/troubleshoot/iaas-sql-failover-cluser).

## <a name="quorum"></a>Quorum

Aunque un clúster de dos nodos funcionará sin un [recurso de cuórum](/windows-server/storage/storage-spaces/understand-quorum), los clientes deben usar un recurso de quórum para tener soporte técnico de producción. La validación del clúster no aprobará ningún clúster sin un recurso de quórum. 

Técnicamente, un clúster de tres nodos puede sobrevivir a la pérdida de un solo nodo (hasta dos) sin un recurso de quórum. Pero una vez que el clúster se reduce a dos nodos, existe el riesgo de que los recursos de clúster se desconecten en caso de que se produzca una pérdida de nodo o un error de comunicación para evitar un escenario de cerebro dividido.

La configuración de un recurso de cuórum permitirá que el clúster continúe en línea con un solo nodo en línea.

En la tabla siguiente se enumeran las opciones de cuórum disponibles en el orden de recomendación para uso con una máquina virtual de Azure; el testigo de disco es la opción preferida: 


||[Testigo de disco](/windows-server/failover-clustering/manage-cluster-quorum#configure-the-cluster-quorum)  |[Testigo en la nube](/windows-server/failover-clustering/deploy-cloud-witness)  |[Testigo de recurso compartido de archivos](/windows-server/failover-clustering/manage-cluster-quorum#configure-the-cluster-quorum)  |
|---------|---------|---------|---------|
|**Sistema operativo admitido**| All |Windows Server 2016+| All|


### <a name="disk-witness"></a>Testigo de disco

Se trata de un pequeño disco agrupado en el grupo de almacenamiento disponible del clúster. Este disco presenta una alta disponibilidad y puede conmutar por error entre nodos. Contiene una copia de la base de datos del clúster, con un tamaño predeterminado que suele ser inferior a 1 GB. El testigo de disco es la opción de cuórum preferida para cualquier clúster que use discos compartidos de Azure (o cualquier solución de disco compartido como SCSI compartido, iSCSI o SAN de canal de fibra).  Un volumen compartido de clúster no se puede usar como testigo de disco.

Configure un disco compartido de Azure como el testigo de disco. 

Para empezar, consulte [Configuración de un testigo de disco](/windows-server/failover-clustering/manage-cluster-quorum#configure-the-cluster-quorum).


**Sistema operativo compatible**: All   


### <a name="cloud-witness"></a>Testigo en la nube

Se trata de un tipo de testigo de cuórum de clúster de conmutación por error que usa Microsoft Azure para proporcionar un voto en el quórum de clúster. El tamaño predeterminado es de aproximadamente 1 MB y solo contiene la marca de tiempo. Un testigo en la nube resulta ideal para implementaciones en varios sitios, en varias zonas y en varias regiones.

Para empezar, consulte [Configuración de un testigo en la nube](/windows-server/failover-clustering/deploy-cloud-witness#CloudWitnessSetUp).


**Sistema operativo compatible**: Windows Server 2016 y posteriores   


### <a name="file-share-witness"></a>Testigo de recurso compartido de archivos

Se trata de un recurso compartido de archivos SMB que se suele configurar en un servidor de archivos que ejecuta Windows Server. Este testigo mantiene la información de la agrupación en clústeres en un archivo witness.log, pero no almacena una copia de la base de datos del clúster. En Azure, puede configurar un [recurso compartido de archivos de Azure](../../../storage/files/storage-how-to-create-file-share.md) para usarlo como testigo de recurso compartido de archivos o en una máquina virtual independiente.

Si va a usar un recurso compartido de archivos de Azure, puede montarlo con el mismo proceso que usó para el [recurso compartido de archivos Premium](failover-cluster-instance-premium-file-share-manually-configure.md#mount-premium-file-share). 

Para empezar, consulte [Configuración de un testigo de recurso compartido de archivos](/windows-server/failover-clustering/manage-cluster-quorum#configure-the-cluster-quorum).


**Sistema operativo compatible**: Windows Server 2012 y posteriores   

## <a name="connectivity"></a>Conectividad

En un entorno de red local tradicional, una instancia de clúster de conmutación por error de SQL Server parece ser una instancia única de SQL Server que se ejecuta en un único equipo. Dado que la instancia de clúster de conmutación por error conmuta por error de nodo a nodo, el nombre de red virtual (VNN) de la instancia proporciona un punto de conexión unificado y permite que las aplicaciones se conecten a la instancia de SQL Server sin conocer qué nodo está activo actualmente. Cuando se produce una conmutación por error, el nombre de red virtual se registra en el nuevo nodo activo después de iniciarse. Este proceso pasa desapercibido para el cliente o aplicación que se conecta a SQL Server y reduce al mínimo el tiempo de inactividad que la aplicación o el cliente experimenta durante un error. Del mismo modo, el cliente de escucha del grupo de disponibilidad usa un VNN para enrutar el tráfico a la réplica adecuada. 

Use un VNN con Azure Load Balancer o un nombre de red distribuida (DNN) para enrutar el tráfico al VNN de la instancia de clúster de conmutación por error con VM con SQL Server en Azure o para reemplazar el cliente de escucha de VNN existente en un grupo de disponibilidad. 


En la tabla siguiente se compara la compatibilidad de la conexión de HADR: 

| |**Nombre de red virtual (VNN)**  |**Nombre de red distribuida (DNN)**  |
|---------|---------|---------|
|**Versión mínima de sistema operativo**| All | Windows Server 2016 |
|**Versión de SQL Server mínima** |All |SQL Server 2019 CU2 (para FCI)<br/> SQL Server 2019 CU8 (para AG)|
|**Solución HADR compatible** | Instancia de clúster de conmutación por error <br/> grupo de disponibilidad | Instancia de clúster de conmutación por error <br/> grupo de disponibilidad|


### <a name="virtual-network-name-vnn"></a>Nombre de red virtual (VNN)

Dado que el punto de acceso a la dirección IP virtual funciona de forma diferente en Azure, debe configurar [Azure Load Balancer](../../../load-balancer/index.yml) para enrutar el tráfico a la dirección IP de los nodos de FCI o al cliente de escucha del grupo de disponibilidad. En máquinas virtuales de Azure, un equilibrador de carga hospeda la dirección IP del nombre de red virtual en el que se basan los recursos de SQL Server en clúster. El equilibrador de carga distribuye los flujos de entrada que llegan al front-end y, a continuación, enruta ese tráfico a las instancias definidas por el grupo de back-end. El flujo de tráfico se configura mediante reglas de equilibrio de carga y sondeos de estado. Con FCI de SQL Server, las instancias del grupo de back-end son las máquinas virtuales de Azure que ejecutan SQL Server. 

Hay un ligero retraso en la conmutación por error cuando se usa el equilibrador de carga, ya que el sondeo de estado realiza comprobaciones activas cada 10 segundos de forma predeterminada. 

Para empezar, aprenda a configurar Azure Load Balancer para una [instancia de clúster de conmutación por error](failover-cluster-instance-vnn-azure-load-balancer-configure.md) o un [grupo de disponibilidad](availability-group-vnn-azure-load-balancer-configure.md).

**Sistema operativo compatible**: All   
**Versión de SQL compatible**: All   
**Solución HADR admitida**: Instancia del clúster de conmutación por error y grupo de disponibilidad   


### <a name="distributed-network-name-dnn"></a>Nombre de red distribuida (DNN)

El nombre de red distribuida es una nueva característica de Azure para SQL Server 2019. El DNN proporciona una manera alternativa para que los clientes de SQL Server se conecten al grupo de disponibilidad o a la instancia de clúster de conmutación por error de SQL Server sin usar un equilibrador de carga. 

Cuando se crea un recurso DNN, el clúster enlaza el nombre DNS con las direcciones IP de todos los nodos del clúster. El cliente de SQL intentará conectarse a cada dirección IP de esta lista para averiguar a qué recurso conectarse.  Puede acelerar este proceso si especifica `MultiSubnetFailover=True` en la cadena de conexión. Esta configuración indica al proveedor que pruebe todas las direcciones IP en paralelo, por lo que el cliente puede conectarse a la FCI o al cliente de escucha al instante. 

Se recomienda un nombre de red distribuida a través de un equilibrador de carga cuando sea posible porque: 
- La solución de un extremo a otro es más sólida, dado que ya no tiene que mantener el recurso del equilibrador de carga. 
- La eliminación de los sondeos del equilibrador de carga reduce al mínimo la duración de la conmutación por error. 
- El nombre de red distribuida simplifica el aprovisionamiento y la administración de la instancia de clúster de conmutación por error o del cliente de escucha del grupo de disponibilidad con VM con SQL Server en Azure. 

La mayoría de las características de SQL Server funcionan de manera transparente con FCI y grupos de disponibilidad cuando se usa el DNN, pero hay determinadas características que pueden exigir una consideración especial. Para obtener más información, consulte [Interoperabilidad con FCI y DNN](failover-cluster-instance-dnn-interoperability.md) e [Interoperabilidad con AG y DNN](availability-group-dnn-interoperability.md). 

Para empezar, aprenda a configurar un recurso de nombre de red distribuida para una [instancia de clúster de conmutación por error](failover-cluster-instance-distributed-network-name-dnn-configure.md) o un [grupo de disponibilidad](availability-group-distributed-network-name-dnn-listener-configure.md)

**Sistema operativo compatible**: Windows Server 2016 y posteriores   
**Versión de SQL compatible**: SQL Server 2019 CU2 (FCI) y SQL Server 2019 CU8 (AG)   
**Solución HADR admitida**: Instancia del clúster de conmutación por error y grupo de disponibilidad   


## <a name="limitations"></a>Limitaciones

Tenga en cuenta las siguientes limitaciones cuando vaya a trabajar con FCI o con grupos de disponibilidad y SQL Server en Azure Virtual Machines. 

### <a name="msdtc"></a>MSDTC 

Azure Virtual Machines admite el Coordinador de transacciones distribuidas de Microsoft (MSDTC) en Windows Server 2019 con almacenamiento en Volúmenes compartidos en clúster (CSV) y [Azure Standard Load Balancer](../../../load-balancer/load-balancer-overview.md) o en VM con SQL Server que usan discos compartidos de Azure. 

En Azure Virtual Machines, MSDTC no se admite para Windows Server 2016 ni versiones anteriores con Volúmenes compartidos en clúster porque:

- El recurso MSDTC en clúster no puede configurarse para usar almacenamiento compartido. En Windows Server 2016, si crea un recurso MSDTC, no mostrará ningún almacenamiento compartido disponible para su uso, incluso si el almacenamiento está disponible. Este problema se ha corregido en Windows Server 2019.
- El equilibrador de carga básico no controla los puertos RPC.


## <a name="next-steps"></a>Pasos siguientes

Una vez que haya determinado las prácticas recomendadas adecuadas para su solución, empiece por [preparar la VM con SQL Server para FCI](failover-cluster-instance-prepare-vm.md) o por crear el grupo de disponibilidad mediante [Azure Portal](availability-group-azure-portal-configure.md), la [CLI de Azure o PowerShell](./availability-group-az-commandline-configure.md), o [plantillas de inicio rápido de Azure](availability-group-quickstart-template-configure.md).