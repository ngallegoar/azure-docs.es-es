---
title: De SQL Server a SQL Server en VM de Azure (información general sobre la migración)
description: Obtenga información sobre las distintas estrategias de migración cuando quiera migrar su instancia de SQL Server a SQL Server en VM de Azure.
ms.custom: ''
ms.service: virtual-machines-sql
ms.subservice: ''
ms.devlang: ''
ms.topic: how-to
author: markjones-msft
ms.author: markjon
ms.reviewer: mathoma
ms.date: 11/06/2020
ms.openlocfilehash: 134685f56de569b39e28165d7ed25868300810f5
ms.sourcegitcommit: b4880683d23f5c91e9901eac22ea31f50a0f116f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/11/2020
ms.locfileid: "94496447"
---
# <a name="migration-overview-sql-server-to-sql-server-on-azure-vms"></a>Información general sobre la migración: De SQL Server a SQL Server en VM de Azure
[!INCLUDE[appliesto--sqlmi](../../includes/appliesto-sqlvm.md)]

Obtenga información sobre las diferentes estrategias de migración para migrar su instancia de SQL Server a SQL Server en Azure Virtual Machines (VM). 

Puede migrar las instancias de SQL Server que se ejecutan de forma local o en:

- SQL Server en Virtual Machines  
- Amazon Web Services (AWS) EC2 
- Amazon Relational Database Service (AWS RDS) 
- Compute Engine (Google Cloud Platform o GCP)

Para ver otros escenarios, consulte la [Guía de migración de bases de datos](https://datamigration.microsoft.com/). 

## <a name="overview"></a>Información general

Realice la migración a [SQL Server en Azure Virtual Machines (VM)](https://docs.microsoft.com/azure/azure-sql/virtual-machines/windows/sql-server-on-azure-vm-iaas-what-is-overview) cuando desee usar el entorno de SQL Server conocido con el control del sistema operativo y quiera aprovechar las características proporcionadas por la nube, como la alta disponibilidad de la VM integrada, las [copias de seguridad automatizadas](../../virtual-machines/windows/automated-backup.md) y la [aplicación automatizada de revisiones](../../virtual-machines/windows/automated-patching.md). 

Ahorre costos al traer su propia licencia con el [modelo de licencias de Ventaja híbrida de Azure](../../virtual-machines/windows/licensing-model-azure-hybrid-benefit-ahb-change.md) o amplíe el soporte técnico de SQL Server 2008 y SQL Server 2008 R2 mediante la obtención de [actualizaciones de seguridad gratuitas](../../virtual-machines/windows/sql-server-2008-extend-end-of-support.md). 


## <a name="choosing-appropriate-target"></a>Elección del destino adecuado

Azure Virtual Machines se ejecuta en muchas regiones diferentes de Azure y también ofrece varios [tamaños de máquina](https://docs.microsoft.com/azure/virtual-machines/windows/sizes) y distintas [opciones de almacenamiento](https://docs.microsoft.com/azure/virtual-machines/disks-types). A la hora de determinar el tamaño correcto de la VM y el almacenamiento de la carga de trabajo de SQL Server, consulte [Directrices de rendimiento para SQL Server en Azure Virtual Machines](https://docs.microsoft.com/azure/azure-sql/virtual-machines/windows/performance-guidelines-best-practices#vm-size-guidance). Para determinar los requisitos de almacenamiento y tamaño de VM de la carga de trabajo, se recomienda cambiar establecer su tamaño a través de una [evaluación de Azure Migrate](https://docs.microsoft.com/azure/migrate/concepts-assessment-calculation#types-of-assessments) basada en el rendimiento. Si esta opción no está disponible, consulte el siguiente artículo sobre la creación de su propia [base de referencia de rendimiento](https://azure.microsoft.com/services/virtual-machines/sql-server/).

También debe tener en cuenta la instalación y configuración correctas de SQL Server en una VM. Se recomienda usar la [galería de imágenes de máquina virtual de Azure SQL](https://docs.microsoft.com/azure/azure-sql/virtual-machines/windows/create-sql-vm-portal), ya que permite crear una VM con SQL Server con la versión, la edición y el sistema operativo correctos. También se registrará la VM de Azure con el [proveedor de recursos](https://docs.microsoft.com/azure/azure-sql/virtual-machines/windows/create-sql-vm-portal) de SQL Server automáticamente, y se habilitarán características como las copias de seguridad automatizadas y la aplicación automatizada de revisiones.

## <a name="migration-strategies"></a>Estrategias de migración

Hay dos estrategias de migración para migrar las bases de datos de usuario a una instancia de SQL Server en las VM de Azure: **migración** y **migración mediante lift-and-shift**. 

El enfoque adecuado para su empresa normalmente depende de los siguientes factores: 

- Tamaño y escala de la migración
- Velocidad de la migración
- Soporte técnico de la aplicación para el cambio de código
- Debe cambiar la versión de SQL Server, el sistema operativo, o ambas cosas.
- Ciclo de vida de compatibilidad de los productos existentes
- Ventana de tiempo de inactividad de la aplicación durante la migración

En la tabla siguiente se describen las diferencias entre las dos estrategias de migración:
<br />

| **Estrategia de migración** | **Descripción** | **Cuándo se deben usar** |
| --- | --- | --- |
| **Migración mediante lift-and-shift** | Use la estrategia de migración mediante lift-and-shift para mover toda la instancia de SQL Server física o virtual de su ubicación actual a una instancia de SQL Server en una VM de Azure sin realizar ningún cambio en el sistema operativo o en la versión de SQL Server. Para completar una migración mediante lift-and-shift, consulte [Azure Migrate](../../../migrate/migrate-services-overview.md). <br /><br /> El servidor de origen permanece en línea y atiende solicitudes mientras el servidor de origen y el de destino sincronizan los datos, lo que permite una migración prácticamente fluida. | Se usa para migraciones desde simples hasta a gran escala, e incluso es aplicable a escenarios como la salida del centro de datos. <br /><br /> No se requiere ningún cambio o solo cambios mínimos para las aplicaciones o bases de datos SQL de usuario, lo que permite migraciones generales más rápidas. <br /><br />No se requieren pasos adicionales para migrar los servicios de Business Intelligence, como [SSIS](/sql/integration-services/sql-server-integration-services), [SSRS](/sql/reporting-services/create-deploy-and-manage-mobile-and-paginated-reports) y [SSAS](/analysis-services/analysis-services-overview). |
|**Migrar** | Use una estrategia de migración cuando desee actualizar la instancia de SQL Server de destino o la versión del sistema operativo. <br /> <br /> Seleccione una VM de Azure desde Azure Marketplace o una imagen de SQL Server preparada que coincida con la versión de SQL Server de origen. | Úsela cuando sea necesario o cuando desee usar las características disponibles en las versiones más recientes de SQL Server, o bien, si existe una necesidad de actualizar las versiones heredadas de SQL Server o del sistema operativo para las que ya no se ofrece soporte técnico.  <br /> <br /> Puede requerir algunos cambios en la base de datos de usuario o la aplicación para admitir la actualización de SQL Server. <br /><br />Puede haber consideraciones adicionales para migrar servicios de [Business Intelligence](#business-intelligence) si están en el ámbito de la migración. |


## <a name="lift-and-shift"></a>migración mediante lift-and-shift  

En la tabla siguiente se detallan los métodos disponibles para la estrategia de migración **lift-and-shift** para migrar la base de datos de SQL Server a SQL Server en VM de Azure:
<br />

|**Método** | **Versión de origen mínima** | **Versión de destino mínima** | **Restricción del tamaño de copia de seguridad de origen** |  **Notas** |
| --- | --- | --- | --- | --- |
| [Azure Migrate](../../../migrate/index.yml) | SQL Server 2008 SP4| SQL Server 2008 SP4| [Límite de almacenamiento de máquina virtual de Azure](https://azure.microsoft.com/documentation/articles/azure-resource-manager/management/azure-subscription-service-limits/) |  Instancia de SQL Server existente que se va a migrar tal cual a la instancia de SQL Server en una VM de Azure. Puede escalar cargas de trabajo de migración de hasta 35 000 VM. <br /><br /> Los servidores de origen permanecen en línea y atienden las solicitudes durante la sincronización de datos del servidor, lo que minimiza el tiempo de inactividad. <br /><br /> **Automatización y scripts**: [Scripts de Azure Site Recovery](../../../migrate/how-to-migrate-at-scale.md) y [Ejemplo de migración y planeación escaladas de Azure](/cloud-adoption-framework/migrate/azure-best-practices/contoso-migration-scale)|

## <a name="migrate"></a>Migrar  

Debido a la facilidad de instalación, el enfoque de migración recomendado es tomar una [copia de seguridad](/sql/t-sql/statements/backup-transact-sql) de SQL Server nativa localmente y, a continuación, copiar el archivo en Azure. Este método admite bases de datos de mayor tamaño (> 1 TB) para todas las versiones de SQL Server a partir de 2008 y copias de seguridad de bases de datos más grandes (> 1 TB). Sin embargo, en el caso de las bases de datos a partir de SQL Server 2014, con un tamaño menor que 1 TB y que tienen una buena conectividad con Azure, la [copia de seguridad de SQL Server en URL](/sql/relational-databases/backup-restore/sql-server-backup-to-url) es el mejor enfoque. 

Al migrar bases de datos de SQL Server a una instancia de SQL Server en VM de Azure, es importante elegir un enfoque que se adapte cuando sea necesario realizar la transición al servidor de destino, ya que esto afecta al período de tiempo de inactividad de la aplicación.

En la tabla siguiente se detallan todos los métodos disponibles para migrar la base de datos de SQL Server a SQL Server en VM de Azure:
<br />

|**Método** | **Versión de origen mínima** | **Versión de destino mínima** | **Restricción del tamaño de copia de seguridad de origen** | **Notas** |
| --- | --- | --- | --- | --- |
| **[Copia de seguridad en un archivo](/windows/migrate-to-vm-from-sql-server.md#back-up-and-restore)** | SQL Server 2008 SP4 | SQL Server 2008 SP4| [Límite de almacenamiento de máquina virtual de Azure](https://azure.microsoft.com/documentation/articles/azure-resource-manager/management/azure-subscription-service-limits/) |  Se trata de una técnica muy sencilla y probada para mover bases de datos entre máquinas. Use la compresión para minimizar el tamaño de la copia de seguridad para la transferencia. <br /><br /> **Automatización y scripts**: [Transact-SQL (T-SQL)](/sql/t-sql/statements/backup-transact-sql) y [AzCopy para Blob Storage](../../../storage/common/storage-use-azcopy-v10.md)  |
| **[Copia de seguridad en URL](/windows/migrate-to-vm-from-sql-server.md#backup-to-url-and-restore-from-url)** | SQL Server 2012 SP1 CU2 | SQL Server 2012 SP1 CU2| 12,8 TB para SQL Server 2016, de lo contrario 1 TB | Método alternativo para mover el archivo de copia de seguridad a la VM con Azure Storage. Use la compresión para minimizar el tamaño de la copia de seguridad para la transferencia. <br /><br /> **Automatización y scripts**:  [T-SQL o plan de mantenimiento](/sql/relational-databases/backup-restore/sql-server-backup-to-url) |
| **[Database Migration Assistant (DMA)](/sql/dma/dma-overview)** | SQL Server 2005| SQL Server 2008 SP4| [Límite de almacenamiento de máquina virtual de Azure](https://azure.microsoft.com/documentation/articles/azure-resource-manager/management/azure-subscription-service-limits/) |  [DMA](/sql/dma/dma-overview) evalúa la instancia de SQL Server local y, a continuación, actualiza sin problemas a versiones posteriores de SQL Server o migra a SQL Server en VM de Azure, Azure SQL Database o Azure SQL Managed Instance. <br /><br /> No se debe usar en bases de datos de usuario habilitadas para secuencias de archivos.<br /><br /> DMA también incluye la capacidad de migrar los [inicios de sesión de SQL y Windows](/sql/dma/dma-migrateserverlogins), así como de evaluar [paquetes SSIS](/sql/dma/dma-assess-ssis). <br /><br /> **Automatización y scripts**: [Interfaz de línea de comandos](/sql/dma/dma-commandline) |
| **[Separar y adjuntar](/windows/migrate-to-vm-from-sql-server.md#detach-and-attach-from-a-url)** | SQL Server 2008 SP4 | SQL Server 2014 | [Límite de almacenamiento de máquina virtual de Azure](https://azure.microsoft.com/documentation/articles/azure-resource-manager/management/azure-subscription-service-limits/) | Use este método cuando planee [almacenar estos archivos mediante el servicio Azure Blob Storage](/sql/relational-databases/databases/sql-server-data-files-in-microsoft-azure) y adjuntarlos a una instancia de SQL Server en una VM de Azure. Resulta especialmente útil con bases de datos muy grandes o cuando el tiempo de copia de seguridad o restauración es prolongado. <br /><br /> **Automatización y scripts**:  [T-SQL](/sql/relational-databases/databases/detach-a-database#TsqlProcedure) y [AzCopy para Blob Storage](../../../storage/common/storage-use-azcopy-v10.md)|
|**[Trasvase de registros](/sql/database-engine/log-shipping/about-log-shipping-sql-server)** | SQL Server 2008 SP4 (solo Windows) | SQL Server 2008 SP4 (solo Windows) | [Límite de almacenamiento de máquina virtual de Azure](https://azure.microsoft.com/documentation/articles/azure-resource-manager/management/azure-subscription-service-limits/) | El trasvase de registros replica los archivos de registro transaccionales desde el entorno local a una instancia de SQL Server en una VM de Azure. <br /><br /> Esto proporciona un tiempo de inactividad mínimo durante la conmutación por error y presenta menos sobrecarga de configuración que la configuración de un grupo de disponibilidad Always On. <br /><br /> **Automatización y scripts**: [T-SQL](/sql/database-engine/log-shipping/log-shipping-tables-and-stored-procedures)  |
| **[Grupo de disponibilidad distribuido](/windows/business-continuity-high-availability-disaster-recovery-hadr-overview.md#hybrid-it-disaster-recovery-solutions)** | SQL Server 2016| SQL Server 2016 | [Límite de almacenamiento de máquina virtual de Azure](https://azure.microsoft.com/documentation/articles/azure-resource-manager/management/azure-subscription-service-limits/) |  Un [grupo de disponibilidad distribuido](/sql/database-engine/availability-groups/windows/distributed-availability-groups) es un tipo especial de grupo de disponibilidad que abarca dos tipos distintos de grupo de disponibilidad. No es necesario que los grupos de disponibilidad que participan en un grupo de disponibilidad distribuido estén en la misma ubicación e incluyan compatibilidad entre dominios. <br /><br /> Este método minimiza el tiempo de inactividad y se usa cuando se tiene un grupo de disponibilidad configurado en el en el entorno local. <br /><br /> **Automatización y scripts**: [T-SQL](/sql/t-sql/statements/alter-availability-group-transact-sql)  |
| | | | | |

&nbsp; &nbsp; &nbsp;&nbsp; &nbsp; &nbsp;
 
> [!TIP]
> Para las transferencias de datos de gran tamaño con opciones de red limitadas o nulas, consulte [Transferencias de datos de gran tamaño con conectividad limitada](../../../storage/common/storage-solution-large-dataset-low-network.md).
> 

### <a name="considerations"></a>Consideraciones

A continuación se muestra una lista de puntos clave que se deben tener en cuenta al revisar métodos de migración:

- Para obtener un rendimiento de transferencia de datos óptimo, migre las bases de datos y los archivos a una instancia de SQL Server en VM de Azure con un archivo de copia de seguridad comprimido. En el caso de las bases de datos de mayor tamaño, además de la compresión, [divida el archivo de copia de seguridad en archivos más pequeños](/sql/relational-databases/backup-restore/back-up-files-and-filegroups-sql-server) para mejorar el rendimiento durante la copia de seguridad y la transferencia. 
- Si realiza la migración desde SQL Server 2014 o una versión posterior, considere la posibilidad de [cifrar las copias de seguridad](/sql/relational-databases/backup-restore/backup-encryption) para proteger los datos durante la transferencia de red.
- Para minimizar el tiempo de inactividad durante la migración de la base de datos, use la opción de grupo de disponibilidad Always On. 
- Para minimizar el tiempo de inactividad sin la sobrecarga de configurar un grupo de disponibilidad, use la opción de trasvase de registros. 
- En el caso de las opciones de red limitadas o nulas, use métodos de migración sin conexión, como la copia de seguridad y la restauración, o los [servicios de transferencia de discos](../../../storage/common/storage-solution-large-dataset-low-network.md) disponibles en Azure.
- Para cambiar también la versión de SQL Server en una instancia de SQL Server en VM de Azure, consulte [Cambio de la edición de SQL Server](../../virtual-machines/windows/change-sql-server-edition.md).

## <a name="business-intelligence"></a>Inteligencia empresarial 

Puede haber consideraciones adicionales al migrar servicios de SQL Server Business intelligence fuera del ámbito de las migraciones de base de datos de usuario. 

Estos servicios incluyen:

- [**SQL Server Integration Services (SSIS)**](/sql/integration-services/install-windows/upgrade-integration-services)
- [**SQL Server Reporting Services (SSRS)** ](/sql/reporting-services/install-windows/upgrade-and-migrate-reporting-services)
- [**SQL Server Analysis Services (SSAS)**](/sql/database-engine/install-windows/upgrade-analysis-services)

## <a name="supported-versions"></a>Versiones compatibles

Al preparar la migración de bases de datos de SQL Server a SQL Server en VM de Azure, asegúrese de tener en cuenta las versiones de SQL Server que se admiten. Para obtener una lista de las versiones de SQL Server compatibles actualmente en las VM de Azure, consulte [SQL Server en VM de Azure](../../virtual-machines/windows/sql-server-on-azure-vm-iaas-what-is-overview.md#get-started-with-sql-server-vms).


## <a name="next-steps"></a>Pasos siguientes

Para empezar a migrar las bases de datos de SQL Server a SQL Server en VM de Azure, consulte la [Guía de migración de bases de datos individuales](sql-server-to-sql-on-azure-vm-individual-databases-guide.md). 

- Para obtener una matriz de los servicios y las herramientas de Microsoft y de otros fabricantes que están disponibles para ayudarlo en diversos escenarios de migración de datos y bases de datos, así como con las tareas especializadas, consulte el artículo [Servicios y herramientas de migración de datos](../../../dms/dms-tools-matrix.md).

- Para obtener más información acerca de Azure SQL, consulte:
   - [Opciones de implementación](../../azure-sql-iaas-vs-paas-what-is-overview.md)
   - [SQL Server en máquinas virtuales de Azure](../../virtual-machines/windows/sql-server-on-azure-vm-iaas-what-is-overview.md)
   - [Calculadora del costo total de propiedad de Azure](https://azure.microsoft.com/pricing/tco/calculator/) 


- Para obtener más información sobre el marco y el ciclo de adopción de las migraciones en la nube, consulte:
   -  [Cloud Adoption Framework para Azure](/azure/cloud-adoption-framework/migrate/azure-best-practices/contoso-migration-scale)
   -  [Procedimientos recomendados para la gestión de los costos y los ajustes de tamaño de las cargas de trabajo migradas a Azure](/azure/cloud-adoption-framework/migrate/azure-best-practices/migrate-best-practices-costs) 

- Para más información acerca de las licencias, consulte:
   - [Enfoque "traiga su propia licencia" con la Ventaja híbrida de Azure](../../virtual-machines/windows/licensing-model-azure-hybrid-benefit-ahb-change.md)
   - [Obtención de soporte extendido gratuito de SQL Server 2008 y SQL Server 2008 R2](../../virtual-machines/windows/sql-server-2008-extend-end-of-support.md)


- Para evaluar el nivel de acceso de la aplicación, consulte [Data Access Migration Toolkit (versión preliminar)](https://marketplace.visualstudio.com/items?itemName=ms-databasemigration.data-access-migration-toolkit)
- Para obtener más información sobre cómo realizar pruebas A/B de nivel de acceso a datos, consulte [Asistente para experimentación con bases de datos](/sql/dea/database-experimentation-assistant-overview).
