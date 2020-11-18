---
title: De SQL Server a SQL Database (información general sobre la migración)
description: Obtenga información sobre las distintas herramientas y opciones disponibles para migrar las bases de datos de SQL Server a Azure SQL Database.
ms.service: sql-database
ms.subservice: migration-guide
ms.custom: ''
ms.devlang: ''
ms.topic: how-to
author: mokabiru
ms.author: mokabiru
ms.reviewer: MashaMSFT
ms.date: 11/06/2020
ms.openlocfilehash: cafb32e5bd91c6b7f3cfef4641828963e0731797
ms.sourcegitcommit: b4880683d23f5c91e9901eac22ea31f50a0f116f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/11/2020
ms.locfileid: "94496462"
---
# <a name="migration-overview-sql-server-to-sql-database"></a>Información general sobre la migración: SQL Server a SQL Database
[!INCLUDE[appliesto--sqldb](../../includes/appliesto-sqldb.md)]

Obtenga información acerca de las diferentes opciones y consideraciones de migración para migrar su instancia de SQL Server a Azure SQL Database. 

Puede migrar las instancias de SQL Server que se ejecutan de forma local o en: 

- SQL Server en Virtual Machines  
- Amazon Web Services (AWS) EC2 
- Amazon Relational Database Service (AWS RDS) 
- Compute Engine (Google Cloud Platform o GCP)  
- SQL en la nube para SQL Server (Google Cloud Platform o GCP) 

Para ver otros escenarios, consulte la [Guía de migración de bases de datos](https://datamigration.microsoft.com/). 

## <a name="overview"></a>Información general

[Azure SQL Database](../../database/sql-database-paas-overview.md) es una opción de destino recomendada para cargas de trabajo de SQL Server que requieren una plataforma como servicio (PaaS) totalmente administrada. SQL Database controla la mayoría de las funciones de administración de bases de datos, junto con la alta disponibilidad, el procesamiento de consultas inteligentes, la escalabilidad y las capacidades de rendimiento integradas para satisfacer muchos tipos de aplicaciones diferentes. 

SQL Database proporciona flexibilidad con varios [modelos de implementación](../../database/sql-database-paas-overview.md#deployment-models) y [niveles de servicio](../../database/service-tiers-vcore.md#service-tiers) que satisfacen diferentes tipos de aplicaciones o cargas de trabajo.

Una de las principales ventajas de migrar a SQL Database es que puede modernizar su aplicación aprovechando las funcionalidades de PaaS y eliminar cualquier dependencia de los componentes técnicos que se encuentran en el ámbito del nivel de instancia, como los trabajos del Agente SQL.

También puede ahorrar costos mediante la migración de sus licencias de SQL Server locales a Azure SQL Database mediante la [Ventaja híbrida de Azure](https://azure.microsoft.com/pricing/hybrid-benefit/) para SQL Server si elige el [modelo de compra basado en núcleos virtuales](../../database/service-tiers-vcore.md).

Esta guía pretende aclarar las opciones de migración y las consideraciones que se deben tener en cuenta al preparar la migración de las bases de datos de SQL Server a Azure SQL Database.  

## <a name="considerations"></a>Consideraciones 

Los factores clave que hay que tener en cuenta al evaluar las opciones de migración dependen de los siguientes factores: 

- Número de servidores y bases de datos
- Tamaño de las bases de datos
- Tiempo de inactividad empresarial aceptable durante el proceso de migración 

Las opciones de migración que se enumeran en esta guía tienen en cuenta estos factores. Para que la migración de datos lógicos a Azure SQL Database, el tiempo de migración puede depender del número de objetos de una base de datos y del tamaño de la base de datos. 

Hay distintas herramientas disponibles para diferentes cargas de trabajo y preferencias de usuario. Algunas herramientas se pueden usar para realizar una migración rápida de una sola base de datos mediante una herramienta basada en la interfaz de usuario, mientras que otras herramientas pueden migrar varias bases de datos que se pueden automatizar para controlar las migraciones a escala. 

## <a name="choose-appropriate-target"></a>Elección del destino adecuado

Tenga en cuenta las directrices generales para ayudarle a elegir el modelo de implementación y el nivel de servicio adecuados de Azure SQL Database. Puede seleccionar recursos de almacenamiento y proceso durante la implementación y, posteriormente, cambiarlos mediante [Azure Portal](../../database/scale-resources.md) sin provocar tiempo de inactividad de la aplicación.


**Modelos de implementación**: comprenda la carga de trabajo de la aplicación y el patrón de uso para decidir entre una sola base de datos o un grupo elástico. 

- Una [base de datos única](../../database/single-database-overview.md) representa una base de datos totalmente administrada adecuada para la mayoría de las aplicaciones y los microservicios en la nube modernos.
- Un [grupo elástico](../../database/elastic-pool-overview.md) es una colección de bases de datos únicas con un conjunto compartido de recursos, como CPU o memoria, y adecuado para combinar bases de datos en un grupo con patrones de uso predecibles que puedan compartir de manera eficaz el mismo conjunto de recursos.

**Modelos de compra**: elija entre los modelos de compra de núcleo virtual, DTU o sin servidor. 

- El [modelo de núcleo virtual](../../database/service-tiers-vcore.md) le permite elegir el número de núcleos virtuales para su instancia de Azure SQL Database, lo que lo convierte en la opción más sencilla para la conversión desde SQL Server local. Esta es la única opción que permite ahorrar costos de licencia con la [Ventaja híbrida de Azure](https://azure.microsoft.com/pricing/hybrid-benefit/). 
- El [modelo de DTU](../../database/service-tiers-dtu.md) abstrae los recursos de proceso, memoria y E/S subyacentes para proporcionar una DTU combinada. 
- El [modelo sin servidor](../../database/serverless-tier-overview.md) está diseñado para cargas de trabajo que requieren un escalado automático a petición con recursos de proceso facturados por segundo de uso. El nivel de proceso sin servidor pausa automáticamente las bases de datos durante períodos de inactividad (cuando solo se factura el almacenamiento), y reactiva automáticamente las bases de datos cuando se reanuda la actividad. 

**Niveles de servicio**: elija entre tres niveles de servicio diseñados para diferentes tipos de aplicaciones.

- [Nivel de servicio De uso general/Estándar](../../database/service-tier-general-purpose.md): ofrece una opción equilibrada orientada al presupuesto con el proceso y el almacenamiento adecuados para ofrecer aplicaciones de nivel medio-bajo, con redundancia integrada en la capa de almacenamiento para la recuperación de errores. Diseñado para la mayoría de las cargas de trabajo de base de datos. 
- [Nivel de servicio Crítico para la empresa/prémium](../../database/service-tier-business-critical.md): destinado a aplicaciones de alto nivel que requieren altas tasas de transacciones, E/S de baja latencia y un alto nivel de resistencia con las réplicas secundarias disponibles para la conmutación por error y para descargar las cargas de trabajo de lectura.
- [Nivel de servicio de hiperescala](../../database/service-tier-hyperscale.md): destinado a bases de datos que tienen volúmenes de datos crecientes y que necesitan escalar automáticamente a un tamaño de base de datos de 100 TB. Diseñado para bases de datos muy grandes. 

> [!IMPORTANT]
> [La velocidad del registro de transacciones se rige](../../database/resource-limits-logical-server.md#transaction-log-rate-governance) en Azure SQL Database para limitar las altas tasas de ingesta. Por lo tanto, durante la migración, puede que sea necesario escalar recursos de base de datos de destino (núcleos virtuales/DTU) para aliviar la presión de la CPU o el rendimiento. Elija la base de datos de destino de tamaño adecuado, pero planee el escalado de los recursos para la migración si es necesario. 


### <a name="sql-server-on-azure-vm-alternative"></a>Alternativa a SQL Server en VM de Azure

Su empresa puede tener requisitos que hagan de [SQL Server en Azure Virtual Machines](../../virtual-machines/windows/sql-server-on-azure-vm-iaas-what-is-overview.md) un destino más adecuado que Azure SQL Database. 

Si se aplica lo siguiente a su empresa, considere la posibilidad de pasar a una instancia de SQL Server en VM de Azure en su lugar: 

- Si necesita acceso directo al sistema operativo o al sistema de archivos, por ejemplo, por ejemplo para instalar agentes personalizados o de otros fabricantes en la misma máquina virtual con SQL Server. 
- Si las características tienen dependencias estrictas que aún no se admiten, por ejemplo, FileStream o FileTable, PolyBase y transacciones entre instancias. 
- Si es imprescindible que permanezca en una versión específica de SQL Server (2012, por ejemplo). 
- Si los requisitos de proceso son mucho menores que los que la instancia administrada ofrece (un núcleo virtual, por ejemplo) y la consolidación de la base de datos no es una opción aceptable. 


## <a name="migration-tools"></a>Herramientas de migración 

Las herramientas recomendadas para la migración son Data Migration Assistant y Azure Database Migration Service. También hay otras opciones de migración alternativas disponibles. 

### <a name="recommended-tools"></a>Herramientas recomendadas

En la tabla siguiente se enumeran las herramientas de migración recomendadas: 

|Technology | Descripción|
|---------|---------|
|[Data Migration Assistant (DMA)](/sql/dma/dma-migrateonpremsqltosqldb)|Data Migration Assistant es una herramienta de escritorio que proporciona evaluaciones perfectas de SQL Server y migraciones a Azure SQL Database (tanto de esquemas como de datos). La herramienta se puede instalar en un servidor local o en la máquina local con conectividad a las bases de datos de origen. El proceso de migración es un movimiento de datos lógico entre los objetos de la base de datos de origen y de destino. </br> - Migración de bases de datos únicas (esquema y datos)|
|[Azure Database Migration Service (DMS)](../../../dms/tutorial-sql-server-to-azure-sql.md)|Servicio de Azure propio que puede migrar las bases de datos de SQL Server a Azure SQL Database con Azure Portal o de forma automatizada con PowerShell. Azure DMS requiere la selección de una instancia de Azure Virtual Network (VNet) preferida durante el aprovisionamiento para asegurarse de que hay conectividad con las bases de datos de SQL Server de origen. </br> - Migración de bases de datos únicas o a escala. |
| | |


### <a name="alternative-tools"></a>Herramientas alternativas

En la tabla siguiente se enumeran las herramientas de migración alternativas: 

|Technology |Descripción  |
|---------|---------|
|[Replicación transaccional](../../database/replication-to-sql-database.md)|Replique los datos de las tablas de base de datos de SQL Server de origen a SQL Database al mismo tiempo que mantiene la coherencia transaccional. Para ello, proporcione una opción de migración de tipo publicador y suscriptor. Los cambios de datos incrementales se propagan a los suscriptores a medida que se producen en los publicadores.|
|[Servicio de importación y exportación/BACPAC](../../database/database-import.md)|[BACPAC](/sql/relational-databases/data-tier-applications/data-tier-applications#bacpac) es un archivo de Windows con una extensión `.bacpac` que encapsula el esquema y los datos de una base de datos. BACPAC se puede usar para exportar datos desde una instancia de SQL Server de origen y para importar los datos en Azure SQL Database. Un archivo BACPAC se puede importar en una nueva instancia de Azure SQL Database mediante Azure Portal. </br></br> Para el escalado y el rendimiento de tamaños de bases de datos grandes o un gran número de bases de datos, debe considerar la posibilidad de emplear la utilidad de línea de comandos [SqlPackage](../../database/database-import.md#using-sqlpackage) para exportar e importar bases de datos.|
|[Copia masiva](/sql/relational-databases/import-export/import-and-export-bulk-data-by-using-the-bcp-utility-sql-server)|La [utilidad del programa de copia masiva (BCP)](/sql/tools/bcp-utility) copia datos de una instancia de SQL Server a un archivo de datos. Emplee la utilidad BCP para exportar los datos del origen e importar el archivo de datos en la instancia de SQL Database de destino. </br></br> En el caso de las operaciones de copia masiva de alta velocidad para trasladar datos a Azure SQL Database, la [herramienta Smart Bulk Copy](/samples/azure-samples/smartbulkcopy/smart-bulk-copy/) se puede usar para maximizar la velocidad de transferencia mediante el aprovechamiento de las tareas de copia paralelas.|
|[Azure Data Factory (ADF)](../../../data-factory/connector-azure-sql-database.md)|La [actividad de copia](../../../data-factory/copy-activity-overview.md) de Azure Data Factory migra los datos de las bases de datos de SQL Server de origen a SQL Database mediante conectores integrados y una instancia de [Integration Runtime](../../../data-factory/concepts-integration-runtime.md).</br> </br> ADF admite una amplia gama de [conectores](../../../data-factory/connector-overview.md) para trasladar datos de orígenes de SQL Server a SQL Database.|
|[SQL Data Sync](../../database/sql-data-sync-data-sql-server-sql-database.md)|SQL Data Sync es un servicio basado en Azure SQL Database que permite sincronizar los datos seleccionados de manera bidireccional entre varias bases de datos, tanto locales como en la nube.</br>Data Sync es útil en casos en que es necesario mantener los datos actualizados entre varias bases de datos de Azure SQL Database o de SQL Server.|
| | |

## <a name="compare-migration-options"></a>Comparación de las opciones de migración

Compare las opciones de migración para elegir la ruta de acceso adecuada para las necesidades de su empresa. 

### <a name="recommended-options"></a>Opciones recomendadas

En la tabla siguiente se comparan las opciones de migración recomendadas: 

|Opción de migración  |Cuándo se usa  |Consideraciones  |
|---------|---------|---------|
|[Data Migration Assistant (DMA)](/sql/dma/dma-migrateonpremsqltosqldb) | - Migración de bases de datos únicas (esquema y datos).  </br> - Puede admitir tiempo de inactividad durante el proceso de migración de datos. </br> </br> Orígenes compatibles: </br> - SQL Server (2005 - 2019) local o VM de Azure </br> - AWS EC2 </br> - AWS RDS </br> - VM con SQL Server de GCP Compute | - La actividad de migración realiza el movimiento de datos entre objetos de base de datos (del origen al destino) y, por lo tanto, se recomienda ejecutarla durante las horas de menor actividad. </br> - DMA informa del estado de la migración por objeto de base de datos, incluido el número de filas migradas.  </br> - Para migraciones grandes (número de bases de datos o tamaño de la base de datos), use el servicio Azure Database Migration Service que se indica a continuación.|
|[Azure Database Migration Service (DMS)](../../../dms/tutorial-sql-server-to-azure-sql.md)| - Migración de bases de datos únicas o a escala. </br> - Puede admitir tiempo de inactividad durante el proceso de migración. </br> </br> Orígenes compatibles: </br> - SQL Server (2005 - 2019) local o VM de Azure </br> - AWS EC2 </br> - AWS RDS </br> - VM con SQL Server de GCP Compute | - Las migraciones a escala se pueden automatizar a través de [PowerShell](../../../dms/howto-sql-server-to-azure-sql-powershell.md). </br> - El tiempo para completar la migración depende del tamaño y del número de objetos de la base de datos. </br> - Necesita que la base de datos de origen esté establecida como de solo lectura. |
| | | |

### <a name="alternative-options"></a>Opciones alternativas

En la tabla siguiente se comparan las opciones de migración alternativas: 

|Método o tecnología |Cuándo se usa    |Consideraciones  |
|---------|---------|---------|
|[Replicación transaccional](../../database/replication-to-sql-database.md)| - Migración mediante la publicación continua de los cambios de las tablas de base de datos de origen a las tablas de base de datos de SQL Database de destino. </br> - Migraciones de base de datos completas o parciales de las tablas seleccionadas (subconjunto de base de datos).  </br> </br> Orígenes compatibles: </br> - [SQL Server (2016 - 2019) con algunas limitaciones](/sql/relational-databases/replication/replication-backward-compatibility) </br> - AWS EC2  </br> - VM con SQL Server de GCP Compute  | - La configuración es relativamente compleja en comparación con otras opciones de migración.   </br> - Proporciona una opción de replicación continua para migrar datos (sin desconectar las bases de datos).  </br> - La replicación transaccional tiene una serie de limitaciones que se deben tener en cuenta al configurar el publicador en la instancia de SQL Server de origen. Consulte [Limitaciones en la publicación de objetos](/sql/relational-databases/replication/publish/publish-data-and-database-objects#limitations-on-publishing-objects) para obtener más información. </br>- Es posible [supervisar la actividad de replicación](/sql/relational-databases/replication/monitor/monitoring-replication).    |
|[Servicio de importación y exportación/BACPAC](../../database/database-import.md)| - Migración de bases de datos individuales de la aplicación de línea de negocio. </br>- Adecuado para bases de datos más pequeñas.  </br>  - No se requieren herramientas ni servicios de migración independientes. </br> </br> Orígenes compatibles: </br> - SQL Server (2005 - 2019) local o VM de Azure </br> - AWS EC2 </br> - AWS RDS </br> - VM con SQL Server de GCP Compute  |  - Requiere tiempo de inactividad, ya que los datos se deben exportar en el origen y importar en el destino.   </br> - Los formatos de archivo y los tipos de datos que se usan en la exportación o importación deben ser coherentes con los esquemas de la tabla para evitar errores de incoherencia de tipo de datos o truncamiento.  </br> - El tiempo necesario para exportar una base de datos con un gran número de objetos puede ser mucho mayor.       |
|[Copia masiva](/sql/relational-databases/import-export/import-and-export-bulk-data-by-using-the-bcp-utility-sql-server)| - Migraciones de datos completas o parciales. </br> - Puede admitir tiempo de inactividad. </br> </br> Orígenes compatibles: </br> - SQL Server (2005 - 2019) local o VM de Azure </br> - AWS EC2 </br> - AWS RDS </br> - VM con SQL Server de GCP Compute   | - Requiere tiempo de inactividad para exportar datos del origen e importarlos en el destino. </br> - Los formatos de archivo y los tipos de datos que se usan en la exportación o importación deben ser coherentes con los esquemas de la tabla. |
|[Azure Data Factory (ADF)](../../../data-factory/connector-azure-sql-database.md)| - Migración o transformación de datos desde las bases de datos de SQL Server de origen. </br> - Combinación de datos de varios orígenes de datos en Azure SQL Database, normalmente para cargas de trabajo de Business Intelligence (BI).  |  - Requiere la creación de canalizaciones de movimiento de datos en ADF para trasladar datos del origen al destino.   </br> El - [costo](https://azure.microsoft.com/pricing/details/data-factory/data-pipeline/) es una consideración importante y se basa en los desencadenadores de canalización, las ejecuciones de actividad, la duración del movimiento de datos, etc. |
|[SQL Data Sync](../../database/sql-data-sync-data-sql-server-sql-database.md)| - Sincronización de datos entre las bases de datos de origen y de destino.</br> - Adecuado para ejecutar la sincronización continua entre Azure SQL Database y SQL Server local en un flujo bidireccional. | - Azure SQL Database debe ser la base de datos central para la sincronización con la base de datos de SQL Server local como base de datos miembro.</br> - En comparación con la replicación transaccional, SQL Data Sync admite la sincronización de datos bidireccional entre el entorno local y Azure SQL Database. </br> - Puede tener un mayor impacto en el rendimiento en función de la carga de trabajo.|
| | | |

## <a name="feature-interoperability"></a>Interoperabilidad de las características 

Existen consideraciones adicionales a la hora de migrar las cargas de trabajo que dependen de otras características de SQL Server.

#### <a name="sql-server-integration-services"></a>SQL Server Integration Services
Migre los paquetes de SQL Server Integration Services (SSIS) a Azure mediante la reimplementación de los paquetes en el tiempo de ejecución de SSIS de Azure en [Azure Data Factory](../../../data-factory/introduction.md). Para [admitir la migración de paquetes SSIS](../../../data-factory/scenario-ssis-migration-overview.md#azure-sql-database-as-database-workload-destination), Azure Data Factory proporciona un tiempo de ejecución creado para ejecutar paquetes SSIS en Azure. Como alternativa, también puede volver a escribir la lógica de ETL de SSIS de forma nativa en ADF mediante [flujos de datos](../../../data-factory/concepts-data-flow-overview.md).


#### <a name="sql-server-reporting-services"></a>SQL Server Reporting Services
Migre informes de SQL Server Reporting Services (SSRS) a informes paginados en Power BI. Use la  [herramienta de migración de RDL](https://github.com/microsoft/RdlMigration) para ayudar a preparar y migrar los informes. Microsoft desarrolló esta herramienta para ayudar a los clientes a migrar informes RDL desde sus servidores SSRS a Power BI. Está disponible en GitHub y documenta un tutorial completo del escenario de migración. 

#### <a name="high-availability"></a>Alta disponibilidad
La configuración manual de características de alta disponibilidad de SQL Server como las instancias de clúster de conmutación por error Always On y los grupos de disponibilidad Always On queda obsoleta en el instancia de Azure SQL Database de destino, ya que la arquitectura de alta disponibilidad ya está integrada en las opciones de SQL Database [De uso general (modelo de disponibilidad estándar)](../../database/high-availability-sla.md#basic-standard-and-general-purpose-service-tier-locally-redundant-availability) y [Crítico para la empresa (modelo de disponibilidad prémium)](../../database/high-availability-sla.md#premium-and-business-critical-service-tier-locally-redundant-availability). El nivel de servicio Crítico para la empresa/prémium también proporciona escalado de lectura que permite conectarse a uno de los nodos secundarios para fines de solo lectura. 

Además de la arquitectura de alta disponibilidad que se incluye en SQL Database, también existe la característica de [grupos de conmutación por error automática](../../database/auto-failover-group-overview.md), que permite administrar la replicación y la conmutación por error de las bases de datos de una instancia administrada en otra región. 

#### <a name="sql-agent-jobs"></a>Trabajos del Agente SQL
Los trabajos del Agente SQL no se admiten directamente en Azure SQL Database y deberán implementarse en [trabajos de Base de datos elástica (versión preliminar)](../../database/job-automation-overview.md#elastic-database-jobs-preview).

#### <a name="logins-and-groups"></a>Inicios de sesión y grupos
Mueva los inicios de sesión de SQL de la instancia de SQL Server de origen a Azure SQL Database mediante Database Migration Service (DMS) en el modo sin conexión.  Use la hoja **Inicios de sesión seleccionados** del **Asistente para migración** para migrar inicios de sesión a la instancia de SQL Database de destino. 

Los usuarios y grupos de Windows también se pueden migrar a través de DMS mediante la habilitación del botón de alternancia correspondiente en la página de configuración de DMS. 

Como alternativa, puede usar la [herramienta de utilidad de PowerShell](https://github.com/microsoft/DataMigrationTeam/tree/master/IP%20and%20Scripts/MoveLogins) especialmente diseñada por los arquitectos de migración de datos de Microsoft. La utilidad usa PowerShell para crear un script de Transact-SQL (T-SQL) para volver a crear inicios de sesión y seleccionar usuarios de base de datos del origen para el destino. La herramienta asigna automáticamente cuentas de Windows AD a cuentas de Azure AD, y puede realizar una búsqueda de UPN para cada inicio de sesión en la instancia de Active Directory de origen. La herramienta incluye en scripts los roles de servidor y de base de datos personalizados, así como la pertenencia a roles, el rol de base de datos y los permisos de usuario. Todavía no se admiten bases de datos independientes y solo un subconjunto de los permisos de SQL Server posibles se incluyen en scripts. 


#### <a name="system-databases"></a>Bases de datos del sistema
Para Azure SQL Database, las únicas bases de datos del sistema aplicables son [maestras](/sql/relational-databases/databases/master-database) y tempdb. Para más información, consulte [Tempdb en Azure SQL Database](/sql/relational-databases/databases/tempdb-database#tempdb-database-in-sql-database).

## <a name="leverage-advanced-features"></a>Aprovechamiento de las características avanzadas 

Asegúrese de aprovechar las características avanzadas basadas en la nube que ofrece SQL Database. Por ejemplo, ya no necesita preocuparse por la administración de copias de seguridad, ya que el servicio la hace automáticamente. Puede realizar la restauración a cualquier [momento dado dentro del período de retención](../../database/recovery-using-backups.md#point-in-time-restore). 

Para reforzar la seguridad, considere la posibilidad de usar las características de  [autenticación](../../database/authentication-aad-overview.md), [auditoría](../../database/auditing-overview.md),  [detección de amenazas](../../database/advanced-data-security.md),  [seguridad de nivel de fila](/sql/relational-databases/security/row-level-security) y  [enmascaramiento dinámico de datos](/sql/relational-databases/security/dynamic-data-masking) de Azure Active Directory.

Además de la administración avanzada y las características de seguridad, SQL Database ofrece un conjunto de herramientas avanzadas que pueden ayudarle a [supervisar y optimizar la carga de trabajo](../../database/monitor-tune-overview.md). [Azure SQL Analytics (versión preliminar)](../../../azure-monitor/insights/azure-sql.md) es una solución avanzada de supervisión en la nube que se utiliza para supervisar el rendimiento de todas las bases de datos de Azure SQL Database a escala, entre varias suscripciones y en una vista única. Azure SQL Analytics recopila y visualiza métricas clave del rendimiento con inteligencia integrada para solucionar problemas de rendimiento.

[Ajuste automático](/sql/relational-databases/automatic-tuning/automatic-tuning#automatic-plan-correction) supervisa continuamente el rendimiento de las estadísticas del plan de ejecución de SQL y corrige automáticamente los problemas de rendimiento identificados. 


## <a name="migration-assets"></a>Recursos de migración 

Para obtener más ayuda, consulte los siguientes recursos, que se desarrollaron para proyectos de migración reales.

|Recurso  |Descripción  |
|---------|---------|
|[Herramienta y modelo de evaluación de la carga de trabajo de datos](https://github.com/Microsoft/DataMigrationTeam/tree/master/Data%20Workload%20Assessment%20Model%20and%20Tool)| Esta herramienta proporciona las plataformas de destino de ajuste perfecto sugeridas, la preparación para la nube, y el nivel de corrección de la aplicación o base de datos para una carga de trabajo determinada. Ofrece un cálculo sencillo con un solo clic y una función de generación de informes que ayuda a acelerar las evaluaciones de grandes volúmenes, ya que proporciona un proceso de toma de decisiones de plataforma de destino uniforme y automatizado.|
|[Utilidad DBLoader](https://github.com/microsoft/DataMigrationTeam/tree/master/DBLoader%20Utility)|DBLoader se puede usar para cargar datos de archivos de texto delimitados en SQL Server. Esta utilidad de consola de Windows usa la interfaz de carga masiva de cliente nativa de SQL Server, que funciona en todas las versiones de SQL Server, incluida Azure SQL Database.|
|[Creación masiva de bases de datos con PowerShell](https://github.com/Microsoft/DataMigrationTeam/tree/master/Bulk%20Database%20Creation%20with%20PowerShell)|Incluye un conjunto de tres scripts de PowerShell que crean un grupo de recursos (create_rg.ps1), el [servidor lógico de Azure](../../database/logical-servers.md) (create_sqlserver.ps1) y Azure SQL Database (create_sqldb.ps1). Los scripts incluyen funcionalidades de bucle para que pueda iterar y crear tantos servidores y bases de datos como sea necesario.|
|[Implementación masiva de esquemas con MSSQL-Scripter y PowerShell](https://github.com/Microsoft/DataMigrationTeam/tree/master/Bulk%20Schema%20Deployment%20with%20MSSQL-Scripter%20&%20PowerShell)|Este recurso crea un grupo de recursos, uno o varios [servidores lógicos en Azure](../../database/logical-servers.md) para hospedar Azure SQL Database, exporta todos los esquemas de una instancia de SQL Server local (o varios servidores SQL Server (2005 y posteriores) y los importa en Azure SQL Database.|
|[Convertir trabajos del Agente SQL Server en trabajos de Base de datos elástica](https://github.com/microsoft/DataMigrationTeam/tree/master/IP%20and%20Scripts/Convert%20SQL%20Server%20Agent%20Jobs%20into%20Elastic%20Database%20Jobs)|Este script migra los trabajos de Agente SQL Server de origen a trabajos de Base de datos elástica.|
|[Envío de correo desde Azure SQL Database](https://github.com/microsoft/DataMigrationTeam/tree/master/IP%20and%20Scripts/AF%20SendMail)|Proporciona una solución como alternativa a la funcionalidad de SendMail que está disponible en la instancia de SQL Server local. La solución utiliza Azure Functions y el servicio SendGrid de Azure para enviar correos electrónicos desde Azure SQL Database.|
|[Utilidad de migración de inicios de sesión de SQL Server local a Azure SQL Database](https://github.com/microsoft/DataMigrationTeam/tree/master/IP%20and%20Scripts/MoveLogins)|Script de PowerShell que crea un script de comandos de T-SQL para volver a crear inicios de sesión y seleccionar usuarios de base de datos de SQL Server local para Azure SQL Database. La herramienta permite la asignación automática de cuentas de Windows AD a cuentas de Azure AD y, opcionalmente, la migración de inicios de sesión nativos de SQL Server.|
|[Automatización de la colección de datos de PerfMon mediante Logman](https://github.com/microsoft/DataMigrationTeam/tree/master/IP%20and%20Scripts/Perfmon%20Data%20Collection%20Automation%20Using%20Logman)|Herramienta que recopila datos de PerfMon para comprender el rendimiento de la base de referencia y ayuda en las recomendaciones de destino de la migración. Esta herramienta utiliza el archivo logman.exe para crear el comando que creará, iniciará, detendrá y eliminará el conjunto de contadores de rendimiento en una instancia de SQL Server remota.|
|[Notas del producto: migración de bases de datos a Azure SQL Database mediante BACPAC](https://github.com/microsoft/DataMigrationTeam/blob/master/Whitepapers/Database%20migrations%20-%20Benchmarks%20and%20Steps%20to%20Import%20to%20Azure%20SQL%20DB%20Single%20Database%20from%20BACPAC.pdf)|En estas notas del producto se proporcionan instrucciones y pasos para ayudar a acelerar las migraciones desde SQL Server a Azure SQL Database con archivos BACPAC.|

Estos recursos se desarrollaron como parte del programa Ninja SQL de datos, que está patrocinado por el equipo de ingeniería de grupos de datos de Azure. El objetivo principal del programa Ninja SQL de datos es lograr y acelerar la modernización compleja, y competir por las oportunidades de migración de las plataformas de datos a la de Azure, de Microsoft. Si cree que su organización estaría interesada en participar en el programa Ninja SQL de datos, póngase en contacto con su equipo de cuentas y pídale que le envíe una nominación.


## <a name="next-steps"></a>Pasos siguientes

Para empezar a migrar la instancia de SQL Server a Azure SQL Database, consulte la [guía de migración de SQL Server a SQL Database](sql-server-to-sql-database-guide.md).

- Para obtener una matriz de los servicios y las herramientas de Microsoft y de otros fabricantes que están disponibles para ayudarlo en diversos escenarios de migración de datos y bases de datos, además de las tareas especializadas, consulte [Servicios y herramientas de migración de datos](../../../dms/dms-tools-matrix.md).

- Para más información sobre SQL Database, consulte:
   - [Información general sobre Azure SQL Database](../../database/sql-database-paas-overview.md)
   - [Calculadora del costo total de propiedad de Azure](https://azure.microsoft.com/pricing/tco/calculator/) 

- Para obtener más información sobre el marco y el ciclo de adopción de las migraciones en la nube, consulte:
   -  [Cloud Adoption Framework para Azure](/azure/cloud-adoption-framework/migrate/azure-best-practices/contoso-migration-scale)
   -  [Procedimientos recomendados para la gestión de los costos y los ajustes de tamaño de las cargas de trabajo migradas a Azure](/azure/cloud-adoption-framework/migrate/azure-best-practices/migrate-best-practices-costs) 


- Para evaluar el nivel de acceso de la aplicación, consulte [Data Access Migration Toolkit (versión preliminar)](https://marketplace.visualstudio.com/items?itemName=ms-databasemigration.data-access-migration-toolkit)
- Para obtener más información sobre cómo realizar pruebas A/B de capa de acceso a datos, consulte [Asistente para experimentación con bases de datos](/sql/dea/database-experimentation-assistant-overview).
