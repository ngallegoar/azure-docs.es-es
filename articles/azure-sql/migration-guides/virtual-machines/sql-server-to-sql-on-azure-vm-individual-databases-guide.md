---
title: De SQL Server a SQL Server en VM de Azure (guía de migración)
description: Siga esta guía para migrar las bases de datos de SQL Server individuales a SQL Server en Azure Virtual Machines (VM).
ms.custom: ''
ms.service: virtual-machines-sql
ms.subservice: ''
ms.devlang: ''
ms.topic: how-to
author: markjones-msft
ms.author: markjon
ms.reviewer: mathoma
ms.date: 11/06/2020
ms.openlocfilehash: 1558c396566b2fcfc098a749407d5e7a28316b6f
ms.sourcegitcommit: 10d00006fec1f4b69289ce18fdd0452c3458eca5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/21/2020
ms.locfileid: "95019456"
---
# <a name="migration-guide-sql-server-to-sql-server-on-azure-vms"></a>Guía de migración: De SQL Server a SQL Server en VM de Azure 
[!INCLUDE[appliesto--sqlmi](../../includes/appliesto-sqlvm.md)]

Esta guía de migración le enseña a **descubrir**, **evaluar** y **migrar** las bases de datos de usuario de SQL Server a una instancia de SQL Server en Azure Virtual Machines (VM) usando las características de copia de seguridad y restauración y de trasvase de registros mediante [Database Migration Assistant (DMA)](/sql/dma/dma-overview) para la evaluación. 

Puede migrar las instancias de SQL Server que se ejecutan de forma local o en:

- SQL Server en Virtual Machines  
- Amazon Web Services (AWS) EC2 
- Amazon Relational Database Service (AWS RDS) 
- Compute Engine (Google Cloud Platform o GCP)

Para obtener información sobre las estrategias de migración adicionales, consulte [Información general sobre la migración de VM con SQL Server](sql-server-to-sql-on-azure-vm-migration-overview.md).

:::image type="content" source="media/sql-server-to-sql-on-azure-vm-migration-overview/migration-process-flow-small.png" alt-text="Flujo del proceso de migración":::

## <a name="prerequisites"></a>Requisitos previos

La migración a SQL Server en VM de Azure requiere lo siguiente: 

- [Database Migration Assistant (DMA)](https://www.microsoft.com/download/details.aspx?id=53595).
- Un [proyecto de Azure Migrate](/azure/migrate/create-manage-projects).
- Una instancia de [SQL Server en una VM de Azure](/azure/azure-sql/virtual-machines/windows/create-sql-vm-portal) de destino que sea de la misma versión o una superior que la de la instancia de SQL Server de origen.
- [Conectividad entre Azure y el entorno local](/azure/architecture/reference-architectures/hybrid-networking).
- [Elección de una estrategia de migración adecuada](sql-server-to-sql-on-azure-vm-migration-overview.md#migrate).

## <a name="pre-migration"></a>Antes de la migración

Antes de iniciar la migración, descubra la topología de su entorno de SQL y evalúe la viabilidad de la migración prevista. 

### <a name="discover"></a>Descubra

Azure Migrate evalúa la idoneidad de la migración de los equipos locales, realiza el ajuste de tamaño basado en el rendimiento y proporciona estimaciones del costo para la ejecución en el entorno local. Para planear la migración, use Azure Migrate para [identificar los orígenes de datos existentes y los detalles sobre las características](../../../migrate/concepts-assessment-calculation.md) que usan sus instancias de SQL Server. Este proceso implica el examen de la red para identificar todas las instancias de SQL Server de su organización con la versión y las características en uso. 

> [!IMPORTANT]
> Al elegir una máquina virtual de Azure de destino para la instancia de SQL Server, asegúrese de tener en cuenta las [Directrices de rendimiento para SQL Server en VM de Azure](../../virtual-machines/windows/performance-guidelines-best-practices.md).

Para conocer herramientas de descubrimiento adicionales, consulte la lista de [Servicios y herramientas](../../../dms/dms-tools-matrix.md#business-justification-phase) disponibles para escenarios de migración de datos.


### <a name="assess"></a>Evaluar

Después de haber descubierto todos los orígenes de datos, use [Data Migration Assistant (DMA)](/sql/dma/dma-overview) para evaluar las instancias de SQL Server locales que se migran a una instancia de SQL Server en VM de Azure para comprender las brechas entre las instancias de origen y de destino. 


> [!NOTE]
> Si _no_ está actualizando la versión de SQL Server, omita este paso y vaya a la sección de [migración](#migrate). 


#### <a name="assess-user-databases"></a>Evaluación de bases de datos de usuario 

Data Migration Assistant (DMA) le ayuda con la migración a una plataforma de datos moderna mediante la detección de problemas de compatibilidad que pueden afectar a la funcionalidad de la base de datos en su nueva versión de SQL Server. DMA recomienda mejoras de rendimiento y confiabilidad para el entorno de destino y le permite migrar el esquema, los datos y los objetos de inicio de sesión desde el servidor de origen al de destino.

Consulte el apartado de [evaluación](/sql/dma/dma-migrateonpremsql) para obtener más información. 


> [!IMPORTANT]
>Según el tipo de evaluación, los permisos necesarios en la instancia de SQL Server de origen pueden ser diferentes. 
   > - En el caso del asesor de **paridad de características**, las credenciales proporcionadas para conectarse a la base de datos de SQL Server de origen deben ser miembros del rol *sysadmin* del servidor.
   > - Para el asesor de problemas de compatibilidad, las credenciales proporcionadas deben tener al menos los permisos `CONNECT SQL`, `VIEW SERVER STATE` y `VIEW ANY DEFINITION`.
   > - DMA resaltará los permisos necesarios para el asesor elegido antes de ejecutar la evaluación.


#### <a name="assess-applications"></a>Evaluación de aplicaciones

Normalmente, una capa de aplicación accede a las bases de datos de usuario para conservar y modificar los datos.  DMA puede evaluar la capa de acceso a datos de una aplicación de dos maneras: 

- Mediante [eventos extendidos](/sql/relational-databases/extended-events/extended-events) capturados o [seguimientos de SQL Server Profiler](/sql/tools/sql-server-profiler/create-a-trace-sql-server-profiler) de las bases de datos de usuario. También puede usar el [Asistente para experimentación con bases de datos](/sql/dea/database-experimentation-assistant-capture-trace) para crear un registro de seguimiento que también se puede usar para las pruebas A/B.

- Mediante [Data Access Migration Toolkit (versión preliminar)](https://marketplace.visualstudio.com/items?itemName=ms-databasemigration.data-access-migration-toolkit) (DAMT), que proporciona el descubrimiento y la evaluación de consultas SQL en el código y se usa para migrar el código fuente de la aplicación de una plataforma de base de datos a otra. Esta herramienta admite varios tipos de archivo conocidos, como C# y Java, XML y texto sin formato. Para obtener una guía sobre cómo realizar una evaluación de DAMT, consulte el blog sobre el [uso de DMAT](https://techcommunity.microsoft.com/t5/microsoft-data-migration/using-data-migration-assistant-to-assess-an-application-s-data/ba-p/990430).

Use DMA para [importar](/sql/dma/dma-assesssqlonprem#add-databases-and-extended-events-trace-to-assess) archivos de seguimiento capturados o archivos DAMT durante la evaluación de las bases de datos de usuario. 


#### <a name="scale-assessments"></a>Evaluaciones a escala

Si tiene varios servidores que requieren una evaluación de DMA, puede automatizar el proceso a través de la [interfaz de línea de comandos](/sql/dma/dma-commandline). Con la interfaz, puede preparar los comandos de evaluación de antemano para cada instancia de SQL Server en el ámbito de la migración. 

En el caso de los informes de resumen de grandes volúmenes, las evaluaciones de Data Migration Assistant (DMA) ahora se pueden [consolidar en Azure Migrate](/sql/dma/dma-assess-sql-data-estate-to-sqldb).

#### <a name="refactor-databases-with-dma"></a>Refactorización de bases de datos con DMA

En función de los resultados de la evaluación de DMA, puede tener una serie de recomendaciones para asegurarse de que las bases de datos de usuario se ejecutan y funcionan correctamente después de la migración. DMA proporciona detalles sobre los objetos afectados, así como recursos para resolver cada problema. Se recomienda que todos los cambios importantes y los cambios de comportamiento se resuelvan antes de la migración de producción.

En el caso de las características desusadas, puede optar por ejecutar las bases de datos de usuario en el modo de [compatibilidad](/sql/t-sql/statements/alter-database-transact-sql-compatibility-level) original si desea evitar realizar estos cambios y acelerar la migración. Sin embargo, esto impedirá [actualizar la compatibilidad de la base de datos](/sql/database-engine/install-windows/compatibility-certification#compatibility-levels-and-database-engine-upgrades) hasta que se hayan resuelto los elementos en desuso.

Se recomienda encarecidamente que todas las correcciones de DMA se incluyan en scripts y se apliquen a la base de datos de SQL Server de destino durante la fase [posterior a la migración](#post-migration).

> [!CAUTION]
> No todas las versiones de SQL Server admiten todos los modos de compatibilidad. Compruebe que la [versión de SQL Server de destino](/sql/t-sql/statements/alter-database-transact-sql-compatibility-level) admiten la compatibilidad con la base de datos elegida. Por ejemplo, SQL Server 2019 no admite bases de datos con compatibilidad de nivel 90 (que es SQL Server 2005). Estas bases de datos necesitarían, como mínimo, una actualización al nivel de compatibilidad 100.
>

## <a name="migrate"></a>Migrar

Una vez completados los pasos previos a la migración, está listo para migrar las bases de datos de usuario y los componentes. Migre sus bases de datos mediante el [método de migración](sql-server-to-sql-on-azure-vm-migration-overview.md#migrate) que prefiera.  

A continuación se proporcionan los pasos necesarios para realizar una migración mediante las características de copia de seguridad y restauración, o una migración de tiempo de inactividad mínima mediante dichas características junto con el trasvase de registros. 

### <a name="backup-and-restore"></a>Copia de seguridad y restauración

Para realizar una migración estándar mediante las características de copia de seguridad y restauración, siga estos pasos: 

1. Configure la conectividad con la instancia de SQL Server de destino en la VM de Azure, en función de sus requisitos. Consulte [Conexión a una máquina virtual de SQL Server en Azure (Resource Manager)](../../virtual-machines/windows/ways-to-connect-to-sql.md).
1. Pause o detenga las aplicaciones que usan bases de datos destinadas a la migración. 
1. Asegúrese de que las bases de datos de usuario están inactivas mediante el [modo de usuario único](/sql/relational-databases/databases/set-a-database-to-single-user-mode). 
1. Realice una copia de seguridad completa de la base de datos en una ubicación local.
1. Copie los archivos de copia de seguridad locales a la VM mediante el escritorio remoto, [Azure Data Explorer](/azure/data-explorer/data-explorer-overview)o la [utilidad de línea de comandos AZCopy](../../../storage/common/storage-use-azcopy-v10.md) (se recomiendan las copias de seguridad de > 2 TB).
1. Restaure las copias de seguridad de base de datos completas en la instancia de SQL Server en VM de Azure.

### <a name="log-shipping--minimize-downtime"></a>Trasvase de registros (minimización del tiempo de inactividad)

Para realizar una migración de tiempo de inactividad mínima mediante la copia de seguridad, la restauración y el trasvase de registros, siga estos pasos: 

1. Configure la conectividad a la instancia de SQL Server de destino en la VM de Azure, en función de sus requisitos. Consulte [Conexión a una máquina virtual de SQL Server en Azure (Resource Manager)](../../virtual-machines/windows/ways-to-connect-to-sql.md).
1. Asegúrese de que las bases de datos de usuario locales que se van a migrar están en un modelo de recuperación completo u optimizado para cargas masivas de registros.
1. Realice una copia de seguridad completa de la base de datos en una ubicación local y modifique cualquier trabajo de copia de seguridad de base de datos completa existente para usar la palabra clave [COPY_ONLY](/sql/relational-databases/backup-restore/copy-only-backups-sql-server) para conservar la cadena de registros.
1. Copie los archivos de copia de seguridad locales en la VM mediante el escritorio remoto, [Azure Data Explorer](/azure/data-explorer/data-explorer-overview)o la [utilidad de línea de comandos AZCopy](../../../storage/common/storage-use-azcopy-v10.md) (se recomiendan copias de seguridad de > 1 TB).
1. Restaure las copias de seguridad completas de la base de datos en la instancia de SQL Server en VM de Azure.
1. Configure el [trasvase de registros](/sql/database-engine/log-shipping/configure-log-shipping-sql-server) entre la base de datos local y la instancia de SQL Server de destino en la VM de Azure. Asegúrese de no reinicializar las bases de datos, ya que esta operación se completó en los pasos anteriores.
1. **Realice la transición** al servidor de destino. 
   1. Pause o detenga las aplicaciones que usan las bases de datos que se van a migrar. 
   1. Asegúrese de que las bases de datos de usuario están inactivas mediante el [modo de usuario único](/sql/relational-databases/databases/set-a-database-to-single-user-mode). 
   1. Cuando esté listo, realice una [conmutación por error controlada](/sql/database-engine/log-shipping/fail-over-to-a-log-shipping-secondary-sql-server) por el trasvase de registros de las bases de datos locales a la instancia de SQL Server de destino en la VM de Azure.



### <a name="migrating-objects-outside-user-databases"></a>Migración de objetos fuera de las bases de datos de usuario

Puede haber objetos de SQL Server adicionales que son necesarios para el funcionamiento sin problemas de las bases de datos de usuario después de la migración. 

En la tabla siguiente se proporcionan los componentes de una lista y los métodos de migración recomendados que se pueden completar antes o después de la migración de las bases de datos de usuario: 


| **Característica** | **Componente** | **Métodos de migración** |
| --- | --- | --- |
| **Bases de datos** | Modelo  | Script con SQL Server Management Studio |
|| TempDB | Planee trasladar TempDB al [disco temporal de la VM de Azure (SSD](../../virtual-machines/windows/performance-guidelines-best-practices.md#temporary-disk)) para obtener el mejor rendimiento. Asegúrese de elegir un tamaño de VM que tenga un disco SSD local suficiente para alojar TempDB. |
|| Bases de datos de usuario con secuencia de archivos |  Utilice los métodos de [copia de seguridad y restauración](../../virtual-machines/windows/migrate-to-vm-from-sql-server.md#back-up-and-restore) para la migración. DMA no admite bases de datos con secuencia de archivos. |
| **Seguridad** | Inicios de sesión de SQL Server y Windows | Use DMA para [migrar inicios de sesión de usuario](/sql/dma/dma-migrateserverlogins). |
|| Roles de SQL Server | Script con SQL Server Management Studio |
|| Proveedores de servicios criptográficos | Se recomienda la [conversión para usar el servicio Azure Key Vault](../../virtual-machines/windows/azure-key-vault-integration-configure.md). Este procedimiento usa el [proveedor de recursos de VM de SQL](../../virtual-machines/windows/sql-vm-resource-provider-register.md). |
| **Objetos de servidor** | Dispositivos de copia de seguridad | Realice el reemplazo por la copia de seguridad de base de datos mediante el [servicio Azure Backup](../../../backup/backup-sql-server-database-azure-vms.md) o escriba copias de seguridad en [Azure Storage](../../virtual-machines/windows/azure-storage-sql-server-backup-restore-use.md) (SQL Server 2012 SP1 CU2 +). Este procedimiento usa el [proveedor de recursos de VM de SQL](../../virtual-machines/windows/sql-vm-resource-provider-register.md).|
|| Servidores vinculados | Script con SQL Server Management Studio. |
|| Desencadenadores de servidor | Script con SQL Server Management Studio. |
| **Replicación** | Publicaciones locales | Script con SQL Server Management Studio. |
|| Suscriptores locales | Script con SQL Server Management Studio. |
| **Polybase** | PolyBase | Script con SQL Server Management Studio. |
| **Administración** | Correo electrónico de base de datos | Script con SQL Server Management Studio. |
| **Agente SQL Server** | Trabajos | Script con SQL Server Management Studio. |
|| Alertas | Script con SQL Server Management Studio. |
|| Operadores | Script con SQL Server Management Studio. |
|| Servidores proxy | Script con SQL Server Management Studio. |
| **Sistema operativo** | Archivos, recursos compartidos de archivos | Anote los archivos o recursos compartidos de archivos adicionales que usan sus instancias de SQL Server y replíquelos en el destino de la VM de Azure. |


## <a name="post-migration"></a>Después de la migración

Cuando haya completado correctamente la fase de migración, deberá realizar una serie de tareas posteriores a la migración para asegurarse de que todo funcione de la forma más fluida y eficaz posible.

### <a name="remediate-applications"></a>Corrección de las aplicaciones

Cuando se hayan migrado los datos al entorno de destino, todas las aplicaciones que antes utilizaban el origen deben empezar a utilizar el destino. Lograr esto puede requerir en algunos casos realizar cambios en las aplicaciones.

Aplique cualquier corrección recomendada de Database Migration Assistant a las bases de datos de usuario. Se recomienda que se incluyen en un script para garantizar la coherencia y permitir la automatización.

### <a name="perform-tests"></a>Realización de pruebas

El enfoque de prueba para la migración de bases de datos consiste en realizar las siguientes actividades:

1. **Desarrollar pruebas de validación.**  Utilice consultas SQL para probar las migraciones de base de datos. Cree consultas de validación para que se ejecuten en las bases de datos de origen y destino. Las consultas de validación deben abarcar el ámbito definido.
2. **Configurar un entorno de prueba.**  el entorno de prueba debe contener una copia de la base de datos de origen y la base de datos de destino. Asegúrese de aislar el entorno de prueba.
3. **Ejecutar pruebas de validación.**  ejecute las pruebas de validación en el origen y el destino y, luego, analice los resultados.
4. **Ejecutar pruebas de rendimiento.**  Ejecute la prueba de rendimiento en el origen y el destino y, luego, analice y compare los resultados.

> [!TIP]
> Use el [Asistente para experimentación con bases de datos (DEA)](/sql/dea/database-experimentation-assistant-overview) para ayudar a evaluar el rendimiento de SQL Server de destino.
>

### <a name="optimize"></a>Optimización

La fase posterior a la migración es fundamental para reconciliar cualquier precisión e integridad de los datos, así como para solucionar posibles problemas de rendimiento con la carga de trabajo.

Para obtener más información acerca de estos problemas y los pasos específicos para mitigarlos, consulte los siguientes recursos:

- [Guía de optimización y validación posterior a la migración.](/sql/relational-databases/post-migration-validation-and-optimization-guide)
- [Ajuste del rendimiento en Azure SQL Virtual Machines](../../virtual-machines/windows/performance-guidelines-best-practices.md).
- [Centro de optimización de costos de Azure](https://azure.microsoft.com/overview/cost-optimization/).

## <a name="next-steps"></a>Pasos siguientes

- Para comprobar la disponibilidad de los servicios aplicables a SQL Server, consulte el [centro de infraestructura global de Azure](https://azure.microsoft.com/global-infrastructure/services/?regions=all&amp;products=synapse-analytics,virtual-machines,sql-database).

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
- Para obtener más información sobre cómo realizar pruebas A/B de capa de acceso a datos, consulte [Asistente para experimentación con bases de datos](/sql/dea/database-experimentation-assistant-overview).
