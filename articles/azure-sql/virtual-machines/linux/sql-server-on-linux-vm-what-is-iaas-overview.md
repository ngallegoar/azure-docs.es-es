---
title: Información general de SQL Server en Azure Virtual Machines para Linux | Microsoft Docs
description: Aprenda a ejecutar las ediciones completas de SQL Server en Azure Virtual Machines para Linux. Obtenga vínculos directos a todas las imágenes de máquina virtual de SQL Server y al contenido relacionado.
services: virtual-machines-linux
documentationcenter: ''
author: MashaMSFT
tags: azure-service-management
ms.service: virtual-machines-sql
ms.topic: conceptual
ms.workload: iaas-sql-server
ms.date: 04/10/2018
ms.author: mathoma
ms.reviewer: jroth
ms.openlocfilehash: 721f30f5b17b078f3a3905204f6be56db25adead
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "84669484"
---
# <a name="overview-of-sql-server-on-azure-virtual-machines-linux"></a>Introducción a SQL Server en SQL Server en máquinas virtuales de Azure (Linux)
[!INCLUDE[appliesto-sqlvm](../../includes/appliesto-sqlvm.md)]

> [!div class="op_single_selector"]
> * [Windows](../windows/sql-server-on-azure-vm-iaas-what-is-overview.md)
> * [Linux](sql-server-on-linux-vm-what-is-iaas-overview.md)

SQL Server en Azure Virtual Machines le permite usar versiones completas de SQL Server en la nube sin tener que administrar ningún hardware local. Las máquinas virtuales con SQL Server también simplifican los costos de licencia cuando se paga por uso.

Las máquinas virtuales de Azure se ejecutan en distintas [regiones geográficas](https://azure.microsoft.com/regions/) en todo el mundo. También ofrecen diversos [tamaños de máquina](../../../virtual-machines/windows/sizes.md). La galería de imágenes de máquina virtual le permite crear una máquina virtual con SQL Server con la versión, la edición y el sistema operativo correctos. Esto hace que las máquinas virtuales sean una buena opción para muchas cargas de trabajo de SQL Server diferentes. 

## <a name="get-started-with-sql-server-vms"></a><a id="create"></a> Introducción a máquinas virtuales con SQL Server

Para empezar, elija una imagen de máquina virtual con SQL Server con la versión, la edición y el sistema operativo requeridos. Las secciones siguientes proporcionan vínculos directos a Azure Portal para las imágenes de la galería de máquinas virtuales de SQL Server.

> [!TIP]
> Para más información sobre cómo entender los precios de imágenes de SQL Server; consulte la [página de precios de máquinas virtuales Linux que ejecutan SQL Server](https://azure.microsoft.com/pricing/details/virtual-machines/linux/).

| Versión | Sistema operativo | Edición |
| --- | --- | --- |
| **SQL Server 2017** | Red Hat Enterprise Linux (RHEL) 7.4 |[Enterprise](https://portal.azure.com/#create/Microsoft.SQLServer2017EnterpriseonRedHatEnterpriseLinux74), [Standard](https://portal.azure.com/#create/Microsoft.SQLServer2017StandardonRedHatEnterpriseLinux74), [Web](https://portal.azure.com/#create/Microsoft.SQLServer2017WebonRedHatEnterpriseLinux74), [Express](https://portal.azure.com/#create/Microsoft.FreeSQLServerLicenseSQLServer2017ExpressonRedHatEnterpriseLinux74), [Developer](https://portal.azure.com/#create/Microsoft.FreeSQLServerLicenseSQLServer2017DeveloperonRedHatEnterpriseLinux74) |
| **SQL Server 2017** | SUSE Linux Enterprise Server (SLES) v12 SP2 |[Enterprise](https://portal.azure.com/#create/Microsoft.SQLServer2017EnterpriseonSLES12SP2), [Standard](https://portal.azure.com/#create/Microsoft.SQLServer2017StandardonSLES12SP2), [Web](https://portal.azure.com/#create/Microsoft.SQLServer2017WebonSLES12SP2), [Express](https://portal.azure.com/#create/Microsoft.FreeSQLServerLicenseSQLServer2017ExpressonSLES12SP2), [Developer](https://portal.azure.com/#create/Microsoft.FreeSQLServerLicenseSQLServer2017DeveloperonSLES12SP2) |
| **SQL Server 2017** | Ubuntu 16.04 LTS |[Enterprise](https://portal.azure.com/#create/Microsoft.SQLServer2017EnterpriseonUbuntuServer1604LTS), [Standard](https://portal.azure.com/#create/Microsoft.SQLServer2017StandardonUbuntuServer1604LTS), [Web](https://portal.azure.com/#create/Microsoft.SQLServer2017WebonUbuntuServer1604LTS), [Express](https://portal.azure.com/#create/Microsoft.FreeSQLServerLicenseSQLServer2017ExpressonUbuntuServer1604LTS), [Developer](https://portal.azure.com/#create/Microsoft.FreeSQLServerLicenseSQLServer2017DeveloperonUbuntuServer1604LTS) |

> [!NOTE]
> Para ver las imágenes de máquinas virtuales con SQL Server para Windows disponibles, consulte [Introducción a SQL Server en máquinas virtuales Windows de Azure](../windows/sql-server-on-azure-vm-iaas-what-is-overview.md).

## <a name="installed-packages"></a><a id="packages"></a> Paquetes instalados

Al configurar SQL Server en Linux, instale el paquete del motor de base de datos y, después, varios paquetes opcionales en función de los requisitos. Las imágenes de máquina virtual Linux para SQL Server instalan automáticamente la mayoría de los paquetes. La siguiente tabla muestra qué paquetes se instalan para cada distribución.

| Distribución | [Motor de base de datos](https://docs.microsoft.com/sql/linux/sql-server-linux-setup) | [Herramientas](https://docs.microsoft.com/sql/linux/sql-server-linux-setup-tools) | [Agente SQL Server](https://docs.microsoft.com/sql/linux/sql-server-linux-setup-sql-agent) | [Búsqueda de texto completo](https://docs.microsoft.com/sql/linux/sql-server-linux-setup-full-text-search) | [SSIS](https://docs.microsoft.com/sql/linux/sql-server-linux-setup-ssis) | [Complemento de alta disponibilidad](https://docs.microsoft.com/sql/linux/sql-server-linux-business-continuity-dr) |
|---|---|---|---|---|---|---|
| RHEL | ![sí](./media/sql-server-on-linux-vm-what-is-iaas-overview/yes.png) | ![sí](./media/sql-server-on-linux-vm-what-is-iaas-overview/yes.png) | ![sí](./media/sql-server-on-linux-vm-what-is-iaas-overview/yes.png) | ![sí](./media/sql-server-on-linux-vm-what-is-iaas-overview/yes.png) | ![sí](./media/sql-server-on-linux-vm-what-is-iaas-overview/yes.png) | ![no](./media/sql-server-on-linux-vm-what-is-iaas-overview/no.png) |
| SLES | ![sí](./media/sql-server-on-linux-vm-what-is-iaas-overview/yes.png) | ![sí](./media/sql-server-on-linux-vm-what-is-iaas-overview/yes.png) | ![sí](./media/sql-server-on-linux-vm-what-is-iaas-overview/yes.png) | ![sí](./media/sql-server-on-linux-vm-what-is-iaas-overview/yes.png) | ![no](./media/sql-server-on-linux-vm-what-is-iaas-overview/no.png) | ![no](./media/sql-server-on-linux-vm-what-is-iaas-overview/no.png) |
| Ubuntu | ![sí](./media/sql-server-on-linux-vm-what-is-iaas-overview/yes.png) | ![sí](./media/sql-server-on-linux-vm-what-is-iaas-overview/yes.png) | ![sí](./media/sql-server-on-linux-vm-what-is-iaas-overview/yes.png) | ![sí](./media/sql-server-on-linux-vm-what-is-iaas-overview/yes.png) | ![sí](./media/sql-server-on-linux-vm-what-is-iaas-overview/yes.png) | ![sí](./media/sql-server-on-linux-vm-what-is-iaas-overview/yes.png) |

## <a name="related-products-and-services"></a>Productos y servicios relacionados

### <a name="linux-virtual-machines"></a>Máquinas virtuales Linux

* [Introducción a las máquinas virtuales de Azure Virtual Machines](../../../virtual-machines/linux/overview.md)

### <a name="storage"></a>Storage

* [Introducción a Microsoft Azure Storage](../../../storage/common/storage-introduction.md)

### <a name="networking"></a>Redes

* [Información general sobre Virtual Network](../../../virtual-network/virtual-networks-overview.md)
* [Direcciones IP en Azure](../../../virtual-network/public-ip-addresses.md)
* [Crear un nombre de dominio completo en el Portal de Azure](../../../virtual-machines/windows/portal-create-fqdn.md)

### <a name="sql"></a>SQL

* [Documentación de SQL Server en Linux](https://docs.microsoft.com/sql/linux)
* [Comparación de Azure SQL Database](../../azure-sql-iaas-vs-paas-what-is-overview.md)

## <a name="next-steps"></a>Pasos siguientes

Introducción a SQL Server en máquinas virtuales Linux:

* [Creación de una máquina virtual con SQL Server en Azure Portal](sql-vm-create-portal-quickstart.md)

Obtenga respuestas a las preguntas más habituales acerca de las máquinas virtuales con SQL Server en Linux:

* [Preguntas más frecuentes sobre SQL Server en Azure Virtual Machines](frequently-asked-questions-faq.md)
