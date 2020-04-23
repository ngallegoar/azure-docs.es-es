---
title: Introducción a SQL Server en máquinas virtuales Windows de Azure | Microsoft Docs
description: Obtenga información sobre cómo ejecutar las ediciones completas de SQL Server en máquinas virtuales de Azure.
services: virtual-machines-windows
documentationcenter: ''
author: MashaMSFT
manager: craigg
tags: azure-service-management
ms.assetid: c505089e-6bbf-4d14-af0e-dd39a1872767
ms.service: virtual-machines-sql
ms.topic: conceptual
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 11/27/2019
ms.author: mathoma
ms.reviewer: jroth
ms.openlocfilehash: 4186a4ed369eb289360b74c0758e5ce3f7eca929
ms.sourcegitcommit: b55d7c87dc645d8e5eb1e8f05f5afa38d7574846
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/16/2020
ms.locfileid: "81482168"
---
# <a name="what-is-sql-server-on-azure-virtual-machines-windows"></a>¿Qué es SQL Server en máquinas virtuales de Azure? (Windows)

> [!div class="op_single_selector"]
> * [Windows](virtual-machines-windows-sql-server-iaas-overview.md)
> * [Linux](../../linux/sql/sql-server-linux-virtual-machines-overview.md)

[SQL Server en máquinas virtuales de Azure](https://azure.microsoft.com/services/virtual-machines/sql-server/) le permite usar versiones completas de SQL Server en la nube sin tener que administrar todo el hardware local. Las máquinas virtuales con SQL Server también simplifican los costos de licencia cuando se paga por uso.

Las máquinas virtuales de Azure se ejecutan en distintas [regiones geográficas](https://azure.microsoft.com/regions/) en todo el mundo. También ofrecen diversos [tamaños de máquina](../sizes.md). La galería de imágenes de máquina virtual le permite crear una máquina virtual con SQL Server con la versión, la edición y el sistema operativo correctos. Esto hace que las máquinas virtuales sean una buena opción para muchas cargas de trabajo de SQL Server diferentes.

## <a name="automated-updates"></a>Actualizaciones automatizadas

Las máquinas virtuales de Azure con SQL Server pueden usar [Automated Patching](virtual-machines-windows-sql-automated-patching.md) para programar el intervalo de tiempo de mantenimiento para la instalación automática de actualizaciones de SQL Server y Windows.

## <a name="automated-backups"></a>Copias de seguridad automatizadas

Las máquinas virtuales de Azure con SQL Server pueden aprovechar [Automated Backup](virtual-machines-windows-sql-automated-backup-v2.md), que crea de forma regular copias de seguridad de la base de datos en el almacenamiento de blobs. También puede utilizar esta técnica manualmente. Para más información, consulte [Uso de Azure Storage para la copia de seguridad y la restauración de SQL Server](virtual-machines-windows-use-storage-sql-server-backup-restore.md).

Azure ofrece también una solución de copia de seguridad de clase empresarial para las instancias de SQL Server que se ejecutan en máquinas virtuales de Azure. Una solución de copia de seguridad totalmente administrada admite Grupos de disponibilidad Always On, retención a largo plazo, recuperación a un momento dado y administración y supervisión centrales. Para obtener más información, vea [Azure Backup para SQL Server en máquina virtual de Azure](https://docs.microsoft.com/azure/backup/backup-azure-sql-database).
  

## <a name="high-availability"></a>Alta disponibilidad

Si requiere alta disponibilidad, considere la posibilidad de configurar grupos de disponibilidad de SQL Server. Esto implica varias máquinas virtuales de Azure con SQL Server en una red virtual. Puede configurar manualmente la solución de alta disponibilidad, o puede usar plantillas en Azure Portal para configurarla automáticamente. Para una introducción a las opciones de alta disponibilidad, consulte [Alta disponibilidad y recuperación ante desastres para SQL Server en máquinas virtuales de Azure](virtual-machines-windows-sql-high-availability-dr.md).

## <a name="performance"></a>Rendimiento

Las máquinas virtuales de Azure ofrecen tamaños de máquina diferentes para satisfacer las distintas demandas de carga de trabajo. Las máquinas virtuales de SQL también proporcionan la configuración automatizada de almacenamiento, que está optimizada para cumplir con los requisitos de rendimiento. Para más información acerca de cómo configurar el almacenamiento para las máquinas virtuales de SQL, consulte [Configuración del almacenamiento para máquinas virtuales de SQL Server](virtual-machines-windows-sql-server-storage-configuration.md). Para ajustar el rendimiento, consulte [Procedimientos recomendados para mejorar el rendimiento de SQL Server en Azure Virtual Machines](virtual-machines-windows-sql-performance.md).

## <a name="get-started-with-sql-vms"></a>Introducción a máquinas virtuales con SQL

Para empezar, elija una imagen de máquina virtual con SQL Server con la versión, la edición y el sistema operativo requeridos. Las secciones siguientes proporcionan vínculos directos a Azure Portal para las imágenes de la galería de máquinas virtuales de SQL Server.

> [!TIP]
> Para conocer los precios de las imágenes de SQL, consulte la [Guía de precios para máquinas virtuales de Azure con SQL Server](virtual-machines-windows-sql-server-pricing-guidance.md). 

### <a name="pay-as-you-go"></a><a id="payasyougo"></a> Pago por uso
En la tabla siguiente se proporciona la matriz de imágenes de SQL SErver de pago por uso.

| Versión | Sistema operativo | Edición |
| --- | --- | --- |
| **SQL Server 2019** | Windows Server 2019 | [Enterprise](https://ms.portal.azure.com/#create/microsoftsqlserver.sql2019-ws2019enterprise), [Standard](https://ms.portal.azure.com/#create/microsoftsqlserver.sql2019-ws2019standard), [Web](https://ms.portal.azure.com/#create/microsoftsqlserver.sql2019-ws2019web), [Developer](https://ms.portal.azure.com/#create/microsoftsqlserver.sql2019-ws2019sqldev) | 
| **SQL Server 2017** |Windows Server 2016 |[Enterprise](https://portal.azure.com/#create/Microsoft.SQLServer2017EnterpriseWindowsServer2016), [Standard](https://portal.azure.com/#create/Microsoft.SQLServer2017StandardonWindowsServer2016), [Web](https://portal.azure.com/#create/Microsoft.SQLServer2017WebonWindowsServer2016), [Express](https://portal.azure.com/#create/Microsoft.FreeSQLServerLicenseSQLServer2017ExpressonWindowsServer2016), [Developer](https://portal.azure.com/#create/Microsoft.FreeSQLServerLicenseSQLServer2017DeveloperonWindowsServer2016) |
| **SQL Server 2016 SP2** |Windows Server 2016 |[Enterprise](https://portal.azure.com/#create/Microsoft.SQLServer2016SP2EnterpriseWindowsServer2016), [Standard](https://portal.azure.com/#create/Microsoft.SQLServer2016SP2StandardWindowsServer2016), [Web](https://portal.azure.com/#create/Microsoft.SQLServer2016SP2WebWindowsServer2016), [Express](https://portal.azure.com/#create/Microsoft.FreeLicenseSQLServer2016SP2ExpressWindowsServer2016), [Developer](https://portal.azure.com/#create/Microsoft.FreeLicenseSQLServer2016SP2DeveloperWindowsServer2016) |
| **SQL Server 2014 SP2** |Windows Server 2012 R2 |[Enterprise](https://portal.azure.com/#create/Microsoft.SQLServer2014SP2EnterpriseWindowsServer2012R2), [Standard](https://portal.azure.com/#create/Microsoft.SQLServer2014SP2StandardWindowsServer2012R2), [Web](https://portal.azure.com/#create/Microsoft.SQLServer2014SP2WebWindowsServer2012R2), [Express](https://portal.azure.com/#create/Microsoft.SQLServer2014SP2ExpressWindowsServer2012R2) |
| **SQL Server 2012 SP4** |Windows Server 2012 R2 |[Enterprise](https://portal.azure.com/#create/Microsoft.SQLServer2012SP4EnterpriseWindowsServer2012R2), [Standard](https://portal.azure.com/#create/Microsoft.SQLServer2012SP4StandardWindowsServer2012R2), [Web](https://portal.azure.com/#create/Microsoft.SQLServer2012SP4WebWindowsServer2012R2), [Express](https://portal.azure.com/#create/Microsoft.SQLServer2012SP4ExpressWindowsServer2012R2) |
| **SQL Server 2008 R2 SP3** |Windows Server 2008 R2|[Enterprise](https://portal.azure.com/#create/Microsoft.SQLServer2008R2SP3EnterpriseWindowsServer2008R2), [Standard](https://portal.azure.com/#create/Microsoft.SQLServer2008R2SP3StandardWindowsServer2008R2), [Web](https://portal.azure.com/#create/Microsoft.SQLServer2008R2SP3WebWindowsServer2008R2), [Express](https://portal.azure.com/#create/Microsoft.SQLServer2008R2SP3ExpressWindowsServer2008R2) |

Para ver las imágenes de máquinas virtuales Linux con SQL Server disponibles, consulte [Overview of SQL Server on Azure Virtual Machines (Linux)](../../linux/sql/sql-server-linux-virtual-machines-overview.md) [Introducción a SQL Server en máquinas virtuales de Azure (Linux)].

> [!NOTE]
> Ya puede cambiar el modelo de licencias de una máquina virtual con SQL Server de pago por uso para usar su propia licencia. Para más información, consulte [Modificación del modelo de licencia para una máquina virtual de SQL en Azure](virtual-machines-windows-sql-ahb.md). 

### <a name="bring-your-own-license"></a><a id="BYOL"></a> Traiga su propia licencia
También puede traer su propia licencia (BYOL). En este escenario, solo paga por la máquina virtual sin ningún cargo adicional de licencia de SQL Server.  Aportar su propia licencia puede ahorrar dinero con el tiempo en cargas de trabajo de producción continuas. Para más información, consulte [Pricing guidance for SQL Server Azure VMs](virtual-machines-windows-sql-server-pricing-guidance.md#byol) (Guía de precios de máquinas virtuales de Azure con SQL Server).

Para usar su propia licencia, puede convertir una VM con SQL de pago por uso existente, o bien implementar una imagen con **{BYOL}** con prefijo. Para más información acerca de cómo cambiar el modelo de licencia entre pago por uso y BYOL, consulte [How to change the licensing model for a SQL VM](virtual-machines-windows-sql-ahb.md) (Cambio del modelo de licencia de una máquina virtual con SQL). 

| Versión | Sistema operativo | Edición |
| --- | --- | --- |
| **SQL Server 2019** | Windows Server 2019 | [Enterprise BYOL](https://ms.portal.azure.com/#create/microsoftsqlserver.sql2019-ws2019-byolenterprise), [Standard BYOL](https://ms.portal.azure.com/#create/microsoftsqlserver.sql2019-ws2019-byolstandard)| 
| **SQL Server 2017** |Windows Server 2016 |[Enterprise BYOL](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2017EnterpriseWindowsServer2016), [Standard BYOL](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2017StandardonWindowsServer2016) |
| **SQL Server 2016 SP2** |Windows Server 2016 |[Enterprise BYOL](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2016SP2EnterpriseWindowsServer2016), [Standard BYOL](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2016SP2StandardWindowsServer2016) |
| **SQL Server 2014 SP2** |Windows Server 2012 R2 |[Enterprise BYOL](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2014SP2EnterpriseWindowsServer2012R2), [Standard BYOL](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2014SP2StandardWindowsServer2012R2) |
| **SQL Server 2012 SP4** |Windows Server 2012 R2 |[Enterprise BYOL](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2012SP4EnterpriseWindowsServer2012R2), [Standard BYOL](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2012SP4StandardWindowsServer2012R2) |

Es posible implementar una imagen anterior de SQL Server que no esté disponible en Azure Portal mediante PowerShell. Para ver todas las imágenes disponibles con PowerShell, utilice el comando siguiente:

  ```powershell
  Get-AzVMImageOffer -Location $Location -Publisher 'MicrosoftSQLServer'
  ```

Para más información acerca de cómo implementar máquinas virtuales con SQL Server mediante PowerShell, consulte [Aprovisionamiento de máquinas virtuales de SQL Server con Azure PowerShell](virtual-machines-windows-ps-sql-create.md).


### <a name="connect-to-the-vm"></a>Conexión a la máquina virtual
Después de crear la máquina virtual con SQL Server, conéctese a ella desde aplicaciones o herramientas tales como SQL Server Management Studio (SSMS). Consulte las instrucciones en [Conexión a una máquina virtual de Azure con SQL Server (Resource Manager)](virtual-machines-windows-sql-connect.md).

### <a name="migrate-your-data"></a>Migración de los datos
Si tiene una base de datos existente, debe moverla a la máquina virtual de SQL recién aprovisionada. Para ver una lista de las opciones de migración e instrucciones, consulte [Migración de una Base de datos SQL Server a SQL Server en una máquina virtual de Azure](virtual-machines-windows-migrate-sql.md).

## <a name="create-and-manage-azure-sql-resources-with-the-azure-portal"></a>Creación y administración de recursos de Azure SQL con Azure Portal

Azure Portal proporciona una sola página donde puede administrar [todos los recursos de Azure SQL](https://portal.azure.com/#blade/HubsExtension/BrowseResource/resourceType/Microsoft.Sql%2Fazuresql), incluidas las máquinas virtuales de SQL.

Para acceder a la página **Recursos SQL de Azure**, en el menú de Azure Portal, seleccione **Azure SQL** o busque y seleccione **Azure SQL** en cualquier página.

![Búsqueda de Azure SQL](./media/quickstart-sql-vm-create-portal/search-for-azure-sql.png)

> [!NOTE]
> **Azure SQL** proporciona una manera rápida y sencilla de acceder a todas las bases de datos SQL, grupos elásticos, servidores de bases de datos, instancias administradas de SQL y máquinas virtuales SQL. SQL de Azure no es un servicio ni un recurso. 

Para administrar los recursos existentes, seleccione el elemento deseado en la lista. Para crear nuevos recursos de Azure SQL, seleccione **+ Agregar**. 

![Creación de un recurso de Azure SQL](./media/quickstart-sql-vm-create-portal/create-azure-sql-resource.png)

Después de seleccionar **+ Agregar**, vea información adicional sobre las diferentes opciones al seleccionar **Mostrar detalles** en cualquier icono.

![detalles del icono de bases de datos](./media/quickstart-sql-vm-create-portal/sql-vm-details.png)

Para obtener detalles, consulte:

- [Creación de una base de datos única](../../../sql-database/sql-database-single-database-get-started.md)
- [Creación de un grupo elástico](../../../sql-database/sql-database-elastic-pool.md#creating-a-new-sql-database-elastic-pool-using-the-azure-portal)
- [Creación de una instancia administrada](../../../sql-database/sql-database-managed-instance-get-started.md)
- [Creación de una máquina virtual SQL](quickstart-sql-vm-create-portal.md)

## <a name="sql-vm-image-refresh-policy"></a><a id="lifecycle"></a> Directiva de actualización de imagen de máquina virtual de SQL
Azure solo mantiene una imagen de máquina virtual para cada combinación admitida de sistema operativo, versión y edición. Esto significa que a lo largo del tiempo se actualizan las imágenes y las más antiguas se eliminan. Para más información, consulte la sección **Imágenes** de las [preguntas más frecuentes sobre máquinas virtuales de SQL Server](virtual-machines-windows-sql-server-iaas-faq.md#images).

## <a name="customer-experience-improvement-program-ceip"></a>Programa para la mejora de la experiencia del usuario (CEIP)
De manera predeterminada, el Programa para la mejora de la experiencia del cliente (CEIP) está habilitado. Esto permitirá enviar periódicamente informes a Microsoft para ayudar a mejorar SQL Server. No hay ninguna tarea de administración requerida relacionada con el CEIP, a menos que desee deshabilitarlo después del aprovisionamiento. Puede personalizar o deshabilitar el CEIP mediante la conexión a la máquina virtual a través del escritorio remoto. A continuación, ejecute la utilidad **Informes de uso y errores de SQL Server** . Siga las instrucciones para deshabilitar los informes. Para más información acerca de la recopilación de datos, vea la [declaración de privacidad de SQL Server](https://docs.microsoft.com/sql/getting-started/microsoft-sql-server-privacy-statement).

## <a name="related-products-and-services"></a>Productos y servicios relacionados
### <a name="windows-virtual-machines"></a>Máquinas virtuales Windows
* [Información general de Virtual Machines](../overview.md)

### <a name="storage"></a>Storage
* [Introducción a Microsoft Azure Storage](../../../storage/common/storage-introduction.md)

### <a name="networking"></a>Redes
* [Información general sobre Virtual Network](../../../virtual-network/virtual-networks-overview.md)
* [Direcciones IP en Azure](../../../virtual-network/virtual-network-ip-addresses-overview-arm.md)
* [Crear un nombre de dominio completo en el Portal de Azure](../portal-create-fqdn.md)

### <a name="sql"></a>SQL
* [Documentación de SQL Server](https://docs.microsoft.com/sql/index)
* [Comparación de Azure SQL Database](../../../sql-database/sql-database-paas-vs-sql-server-iaas.md)

## <a name="next-steps"></a>Pasos siguientes

Introducción a SQL Server en máquinas virtuales de Azure:

* [Creación de una máquina virtual con SQL Server en Azure Portal](quickstart-sql-vm-create-portal.md)

Obtenga respuestas a las preguntas más habituales acerca de las máquinas virtuales con SQL:

* [Preguntas más frecuentes sobre SQL Server en Azure Virtual Machines](virtual-machines-windows-sql-server-iaas-faq.md)

Vista de arquitecturas de referencia para ejecutar aplicaciones de n niveles en SQL Server en IaaS

* [Aplicación Windows de N niveles en Azure con SQL Server](https://docs.microsoft.com/azure/architecture/reference-architectures/n-tier/n-tier-sql-server)
* [Ejecución de una aplicación de N niveles en varias regiones de Azure para lograr alta disponibilidad](https://docs.microsoft.com/azure/architecture/reference-architectures/n-tier/multi-region-sql-server)
