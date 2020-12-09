---
title: De SQL Server a SQL Managed Instance (información general sobre la migración)
description: Obtenga información sobre las distintas herramientas y opciones disponibles para migrar las bases de datos de SQL Server a Azure SQL Managed Instance.
ms.service: sql-managed-instance
ms.subservice: migration-guide
ms.custom: ''
ms.devlang: ''
ms.topic: how-to
author: mokabiru
ms.author: mokabiru
ms.reviewer: MashaMSFT
ms.date: 11/06/2020
ms.openlocfilehash: 878d59445dde839ca9e702ac0c49af676e48a42f
ms.sourcegitcommit: 5b93010b69895f146b5afd637a42f17d780c165b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/02/2020
ms.locfileid: "96531197"
---
# <a name="migration-overview-sql-server-to-sql-managed-instance"></a>Información general sobre la migración: SQL Server a Instancia administrada de SQL
[!INCLUDE[appliesto--sqlmi](../../includes/appliesto-sqlmi.md)]

Obtenga información sobre las diferentes opciones y consideraciones de migración para migrar su instancia de SQL Server a Azure SQL Managed Instance. 

Puede migrar las instancias de SQL Server que se ejecutan de forma local o en: 

- SQL Server en Virtual Machines  
- Amazon Web Services (AWS) EC2 
- Amazon Relational Database Service (AWS RDS) 
- Compute Engine (Google Cloud Platform o GCP)  
- SQL en la nube para SQL Server (Google Cloud Platform o GCP) 

Para ver otros escenarios, consulte la [Guía de migración de bases de datos](https://datamigration.microsoft.com/). 

## <a name="overview"></a>Información general

[Azure SQL Managed Instance](../../managed-instance/sql-managed-instance-paas-overview.md) es una opción de destino recomendada para cargas de trabajo de SQL Server que requieren un servicio totalmente administrado sin tener que administrar máquinas virtuales ni sus sistemas operativos. SQL Managed Instance le permite migrar mediante lift-and-shift sus aplicaciones locales a Azure con los cambios mínimos en la aplicación o la base de datos, al mismo tiempo que mantiene un aislamiento completo de las instancias con compatibilidad con la red virtual (VNet) nativa. 

## <a name="considerations"></a>Consideraciones 

Los factores clave que hay que tener en cuenta al evaluar las opciones de migración dependen de los siguientes factores: 
- Número de servidores y bases de datos
- Tamaño de las bases de datos
- Tiempo de inactividad empresarial aceptable durante el proceso de migración 

Una de las principales ventajas de migrar las instancias de SQL Server a SQL Managed Instance es que puede optar por migrar toda la instancia o simplemente un subconjunto de bases de datos individuales. Elabore el plan cuidadosamente para incluir lo siguiente en el proceso de migración: 
- Todas las bases de datos que deben colocarse en la misma instancia. 
- Los objetos de nivel de instancia necesarios para la aplicación, incluidos inicios de sesión, credenciales, trabajos y operadores del Agente SQL, y desencadenadores de nivel de servidor. 

> [!NOTE]
> Azure SQL Managed Instance garantiza el 99,99 % de disponibilidad incluso en escenarios críticos, por lo que la sobrecarga que provocan algunas características de SQL Managed Instance no se pueden deshabilitar. Para más información, consulte las [causas principales que podrían provocar un rendimiento diferente en SQL Server y Azure SQL Managed Instance](https://azure.microsoft.com/blog/key-causes-of-performance-differences-between-sql-managed-instance-and-sql-server/). 


## <a name="choose-appropriate-target"></a>Elección del destino adecuado

Algunas directrices generales que le ayudarán a elegir el nivel de servicio adecuado y las características de SQL Managed Instance para ayudarle a satisfacer su [base de referencia de rendimiento](sql-server-to-managed-instance-performance-baseline.md): 

- Use la base de referencia de uso de CPU para aprovisionar una instancia administrada que coincida con el número de núcleos que usa la instancia de SQL Server. Puede que sea necesario escalar los recursos para que coincidan con las [características de generación de hardware](../../managed-instance/resource-limits.md#hardware-generation-characteristics). 
- Use la base de referencia de uso de memoria para elegir una [opción de núcleo virtual](../../managed-instance/resource-limits.md#service-tier-characteristics) que coincida debidamente con la asignación de memoria. 
- Use la latencia de E/S de la base de referencia del subsistema de archivos para elegir entre los niveles de servicio De uso general (latencia mayor que 5 ms) y Crítico para la empresa (latencia inferior a 3 ms). 
- Use el rendimiento de la base de referencia para la asignación previa del tamaño de los archivos de datos y de registro para lograr el rendimiento de E/S esperado. 

Puede seleccionar recursos de almacenamiento y proceso durante la implementación y, posteriormente, cambiarlos con [Azure Portal](../../database/scale-resources.md) sin provocar tiempo de inactividad de la aplicación. 

> [!IMPORTANT]
> Cualquier discrepancia en los [requisitos de red virtual de instancia administrada](../../managed-instance/connectivity-architecture-overview.md#network-requirements) puede impedir la creación de nuevas instancias o el uso de las existentes. Obtenga más información acerca de la [creación de redes nuevas](../../managed-instance/virtual-network-subnet-create-arm-template.md) y la [configuración de las existentes](../../managed-instance/vnet-existing-add-subnet.md?branch=release-ignite-arc-data) . 

### <a name="sql-server-vm-alternative"></a>Alternativa a las VM con SQL Server

Es posible que su empresa tenga requisitos por los que SQL Server en VM de Azure sea un destino más adecuado que Azure SQL Managed Instance. 

Si la condición siguiente se aplica a su empresa, considere la posibilidad de cambiar a una VM con SQL Server en su lugar: 

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
|[Azure Database Migration Service (DMS)](../../../dms/tutorial-sql-server-to-managed-instance.md)  | Servicio de Azure propio que admite la migración en el modo sin conexión para las aplicaciones que pueden permitirse un tiempo de inactividad durante el proceso de migración. A diferencia de la migración continua en el modo en línea, la migración en el modo sin conexión ejecuta una restauración única de una copia de seguridad completa de la base de datos del origen al destino. | 
|[Copia de seguridad y restauración nativa](../../managed-instance/restore-sample-database-quickstart.md) | SQL Managed Instance admite la restauración de copias de seguridad de bases de datos SQL Server nativas (archivos .bak), lo que la convierte en la opción de migración más simple para los clientes que pueden proporcionar copias de seguridad completas de la base de datos a Azure Storage. También se admiten las copias de seguridad completas y diferenciales, que se documentan en la [sección de recursos de migración](#migration-assets) más adelante en este artículo.| 
| | |

### <a name="alternative-tools"></a>Herramientas alternativas

En la tabla siguiente se enumeran las herramientas de migración alternativas: 

|Technology |Descripción  |
|---------|---------|
|[Replicación transaccional](../../managed-instance/replication-transactional-overview.md) | Replique los datos de las tablas de base de datos de SQL Server de origen a SQL Managed Instance al mismo tiempo que mantiene la coherencia transaccional. Para ello, proporcione una opción de migración de tipo publicador y suscriptor. |  |
|[Copia masiva](/sql/relational-databases/import-export/import-and-export-bulk-data-by-using-the-bcp-utility-sql-server)| La [utilidad del programa de copia masiva (BCP)](/sql/tools/bcp-utility) copia datos de una instancia de SQL Server a un archivo de datos. Emplee la utilidad BCP para exportar los datos del origen e importar el archivo de datos en la instancia de SQL Managed Instance de destino.</br></br> En el caso de las operaciones de copia masiva de alta velocidad para trasladar datos a Azure SQL Database, la [herramienta Smart Bulk Copy](/samples/azure-samples/smartbulkcopy/smart-bulk-copy/) se puede usar para maximizar las velocidades de transferencia mediante el aprovechamiento de las tareas de copia paralelas. | 
|[Asistente de importación y exportación/BACPAC](../../database/database-import.md?tabs=azure-powershell)| [BACPAC](/sql/relational-databases/data-tier-applications/data-tier-applications#bacpac) es un archivo de Windows con una extensión `.bacpac` que encapsula el esquema y los datos de una base de datos. El archivo BACPAC se puede usar para exportar datos de una instancia de SQL Server de origen y para volver a importar el archivo a Azure SQL Managed Instance.  |  
|[Azure Data Factory (ADF)](../../../data-factory/connector-azure-sql-managed-instance.md)| La [actividad de copia](../../../data-factory/copy-activity-overview.md) de Azure Data Factory migra los datos de las bases de datos de SQL Server de origen a SQL Managed Instance mediante conectores integrados y una instancia de [Integration Runtime](../../../data-factory/concepts-integration-runtime.md).</br> </br> ADF admite una amplia gama de [conectores](../../../data-factory/connector-overview.md) para trasladar datos de orígenes de SQL Server a SQL Managed Instance. |
| | |

## <a name="compare-migration-options"></a>Comparación de las opciones de migración

Compare las opciones de migración para elegir la ruta de acceso adecuada para las necesidades de su empresa. 

### <a name="recommended-options"></a>Opciones recomendadas

En la tabla siguiente se comparan las opciones de migración recomendadas: 

|Opción de migración  |Cuándo se usa  |Consideraciones  |
|---------|---------|---------|
|[Azure Database Migration Service (DMS)](../../../dms/tutorial-sql-server-to-managed-instance.md) | - Migración de bases de datos únicas o de varias bases de datos a escala. </br> - Puede admitir tiempo de inactividad durante el proceso de migración. </br> </br> Orígenes compatibles: </br> - SQL Server (2005 - 2019) local o VM de Azure </br> - AWS EC2 </br> - AWS RDS </br> - VM con SQL Server de GCP Compute |  - Las migraciones a escala se pueden automatizar a través de [PowerShell](../../../dms/howto-sql-server-to-azure-sql-mi-powershell.md). </br> - El tiempo para completar la migración depende del tamaño de la base de datos y se ve afectado por el tiempo de la copia de seguridad y la restauración. </br> - Es posible que se requiera tiempo de inactividad suficiente. |
|[Copia de seguridad y restauración nativa](../../managed-instance/restore-sample-database-quickstart.md) | - Migración de bases de datos individuales de aplicación de línea de negocio.  </br> - Migración rápida y fácil sin necesidad de una herramienta o un servicio de migración independiente.  </br> </br> Orígenes compatibles: </br> - SQL Server (2005 - 2019) local o VM de Azure </br> - AWS EC2 </br> - AWS RDS </br> - VM con SQL Server de GCP Compute | - La copia de seguridad de base de datos usa varios subprocesos para optimizar la transferencia de datos a Azure Blob Storage, pero el ancho de banda de ISV y el tamaño de la base de datos pueden afectar la velocidad de transferencia. </br> - El tiempo de inactividad debe permitir el tiempo necesario para realizar una operación de copia de seguridad y restauración completa (que es un tamaño de operación de datos).| 
| | | |

### <a name="alternative-options"></a>Opciones alternativas

En la tabla siguiente se comparan las opciones de migración alternativas: 

|Método o tecnología |Cuándo se usa |Consideraciones  |
|---------|---------|---------|
|[Replicación transaccional](../../managed-instance/replication-transactional-overview.md) | - Migración mediante la publicación continua de los cambios de las tablas de base de datos de origen a las tablas de base de datos de SQL Managed Instance de destino. </br> - Migraciones de base de datos completas o parciales de las tablas seleccionadas (subconjunto de base de datos).  </br> </br> Orígenes compatibles: </br> - SQL Server (2012 - 2019) con algunas limitaciones </br> - AWS EC2  </br> - VM con SQL Server de GCP Compute | </br> - La configuración es relativamente compleja en comparación con otras opciones de migración.   </br> - Proporciona una opción de replicación continua para migrar datos (sin desconectar las bases de datos).</br> - La replicación transaccional tiene una serie de limitaciones que se deben tener en cuenta al configurar el publicador en la instancia de SQL Server de origen. Consulte [Limitaciones en la publicación de objetos](/sql/relational-databases/replication/publish/publish-data-and-database-objects#limitations-on-publishing-objects) para obtener más información.  </br> - La funcionalidad de [supervisión de la actividad de replicación](/sql/relational-databases/replication/monitor/monitoring-replication) está disponible.    |
|[Copia masiva](/sql/relational-databases/import-export/import-and-export-bulk-data-by-using-the-bcp-utility-sql-server)| - Migraciones de datos completas o parciales. </br> - Puede admitir tiempo de inactividad. </br> </br> Orígenes compatibles: </br> - SQL Server (2005 - 2019) local o VM de Azure </br> - AWS EC2 </br> - AWS RDS </br> - VM con SQL Server de GCP Compute   | - Requiere tiempo de inactividad para exportar datos del origen e importarlos en el destino. </br> - Los formatos de archivo y los tipos de datos que se usan en la exportación o importación deben ser coherentes con los esquemas de la tabla. |
|[Asistente de importación y exportación/BACPAC](../../database/database-import.md)| - Migración de bases de datos individuales de la aplicación de línea de negocio. </br>- Adecuado para bases de datos más pequeñas.  </br>  No se requieren herramientas ni servicios de migración independientes. </br> </br> Orígenes compatibles: </br> - SQL Server (2005 - 2019) local o VM de Azure </br> - AWS EC2 </br> - AWS RDS </br> - VM con SQL Server de GCP Compute  |   </br> - Requiere tiempo de inactividad, ya que los datos se deben exportar en el origen y importar en el destino.   </br> - Los formatos de archivo y los tipos de datos que se usan en la exportación o importación deben ser coherentes con los esquemas de la tabla para evitar errores de incoherencia de tipo de datos o truncamiento. </br> - El tiempo necesario para exportar una base de datos con un gran número de objetos puede ser mucho mayor. |
|[Azure Data Factory (ADF)](../../../data-factory/connector-azure-sql-managed-instance.md)| - Migración o transformación de datos desde las bases de datos de SQL Server de origen.</br> - Combinación de datos de varios orígenes de datos en Azure SQL Managed Instance, normalmente para cargas de trabajo de Business Intelligence (BI).   </br> - Requiere la creación de canalizaciones de movimiento de datos en ADF para trasladar datos del origen al destino.   </br> El - [costo](https://azure.microsoft.com/pricing/details/data-factory/data-pipeline/) es una consideración importante y se basa en los desencadenadores de canalización, las ejecuciones de actividad, la duración del movimiento de datos, etc. |
| | | |

## <a name="feature-interoperability"></a>Interoperabilidad de las características 

Existen consideraciones adicionales a la hora de migrar las cargas de trabajo que dependen de otras características de SQL Server. 

#### <a name="sql-server-integration-services"></a>SQL Server Integration Services

Migre paquetes y proyectos de SQL Server Integration Services (SSIS) de SSISDB a Azure SQL Managed Instance mediante [Azure Database Migration Service (DMS)](../../../dms/how-to-migrate-ssis-packages-managed-instance.md). 

Solo se admiten para la migración los paquetes SSIS de SSISDB que empiecen por SQL Server 2012. Convierta los paquetes SSIS heredados antes de la migración. Consulte el [tutorial de conversión de proyectos](/sql/integration-services/lesson-6-2-converting-the-project-to-the-project-deployment-model) para obtener más información. 


#### <a name="sql-server-reporting-services"></a>SQL Server Reporting Services

Los informes de SQL Server Reporting Services (SSRS) se pueden migrar a informes paginados en Power BI. Use la  [herramienta de migración de RDL](https://github.com/microsoft/RdlMigration) para ayudar a preparar y migrar los informes. Microsoft desarrolló esta herramienta para ayudar a los clientes a migrar informes RDL desde sus servidores SSRS a Power BI. Está disponible en GitHub y documenta un tutorial completo del escenario de migración. 

#### <a name="sql-server-analysis-services"></a>SQL Server Analysis Services

Los modelos tabulares de SQL Server Analysis Services de SQL Server 2012 y versiones posteriores se pueden migrar a Azure Analysis Services, que es un modelo de implementación de PaaS para el modelo tabular de Analysis Services en Azure. Puede obtener más información sobre la migración de modelos locales a Azure Analysis Services en este [tutorial en vídeo](https://azure.microsoft.com/resources/videos/azure-analysis-services-moving-models/).

También puede considerar la posibilidad de migrar los modelos tabulares de Analysis Services locales a [Power BI Premium con los nuevos puntos de conexión de lectura y escritura XMLA](/power-bi/admin/service-premium-connect-tools). 
> [!NOTE]
> La funcionalidad de puntos de conexión de lectura y escritura XMLA de Power BI está actualmente en versión preliminar pública y no debe tenerse en cuenta para las cargas de trabajo de producción hasta que la funcionalidad esté disponible con carácter general.

#### <a name="high-availability"></a>Alta disponibilidad

Las características de alta disponibilidad de SQL Server Instancias de clúster de conmutación por error Always On y Grupos de disponibilidad Always On quedan obsoletas en el instancia de Azure SQL Managed Instance de destino, ya que la arquitectura de alta disponibilidad ya está integrada en las opciones de SQL Managed Instance [De uso general (modelo de disponibilidad estándar)](../../database/high-availability-sla.md#basic-standard-and-general-purpose-service-tier-locally-redundant-availability) y [Crítico para la empresa (modelo de disponibilidad prémium)](../../database/high-availability-sla.md#premium-and-business-critical-service-tier-locally-redundant-availability). El modelo de disponibilidad prémium también proporciona escalado de lectura, que permite conectarse a uno de los nodos secundarios para fines de solo lectura.     

Además de la arquitectura de alta disponibilidad que se incluye en SQL Managed Instance, también existe la característica de [grupos de conmutación por error automática](../../database/auto-failover-group-overview.md), que permite administrar la replicación y la conmutación por error de las bases de datos de una instancia administrada en otra región. 

#### <a name="sql-agent-jobs"></a>Trabajos del Agente SQL

Use la opción Azure Database Migration Service (DMS) sin conexión para migrar [trabajos del Agente SQL](../../../dms/howto-sql-server-to-azure-sql-mi-powershell.md#offline-migrations). En caso contrario, cree un script de los trabajos en Transact-SQL (T-SQL) mediante SQL Server Management Studio y, a continuación, vuelva a crearlos manualmente en la instancia de SQL Managed Instance de destino. 

> [!IMPORTANT]
> Actualmente, Azure DMS solo admite trabajos con pasos del subsistema de T-SQL. Los trabajos con pasos de paquetes SSIS se deben migrar manualmente. 

#### <a name="logins-and-groups"></a>Inicios de sesión y grupos

Los inicios de sesión de SQL desde la instancia de SQL Server de origen se pueden migrar a Azure SQL Managed Instance mediante Database Migration Service (DMS) en el modo sin conexión.  Use la hoja **[Seleccionar inicios de sesión](../../../dms/tutorial-sql-server-to-managed-instance.md#select-logins)** del **Asistente para migración** para migrar inicios de sesión a su instancia de SQL Managed Instance de destino. 

De forma predeterminada, Azure Database Migration Service solo admite la migración de inicios de sesión de SQL. Sin embargo, puede habilitar la capacidad de migrar inicios de sesión de Windows de la siguiente manera:

Asegúrese de que la instancia SQL Managed Instance de destino tiene acceso de lectura de Azure AD, que un usuario con el rol **Administrador de empresa** o **Administrador global** puede configurar a través de Azure Portal.
Configure la instancia de Azure Database Migration Service para habilitar las migraciones de inicio de sesión de usuario o grupo de Windows, que se configuran mediante Azure Portal, en la página Configuración. Después de habilitar esta configuración, reinicie el servicio para que los cambios se apliquen.

Después de reiniciar el servicio, los inicios de sesión de usuario o grupo de Windows aparecen en la lista de inicios de sesión disponibles para la migración. En el caso de los inicios de sesión de usuario o grupo de Windows que migre, se le pedirá que proporcione el nombre de dominio asociado. No se admiten las cuentas de usuario de servicio (cuenta con el nombre de dominio NT AUTHORITY) y las cuentas de usuario virtual (nombre de cuenta con el nombre de dominio NT SERVICE).

Para obtener más información, consulte [Migración de los usuarios y los grupos de Windows de una instancia de SQL Server a Instancia administrada de Azure SQL mediante la sintaxis DDL de T-SQL](../../managed-instance/migrate-sql-server-users-to-instance-transact-sql-tsql-tutorial.md).

Como alternativa, puede usar la [herramienta de utilidad de PowerShell](https://github.com/microsoft/DataMigrationTeam/tree/master/IP%20and%20Scripts/MoveLogins) especialmente diseñada por los arquitectos de migración de datos de Microsoft. La utilidad usa PowerShell para crear un script de T-SQL para volver a crear inicios de sesión y seleccionar usuarios de base de datos del origen para el destino. La herramienta asigna automáticamente cuentas de Windows AD a cuentas de Azure AD, y puede realizar una búsqueda de UPN para cada inicio de sesión en la instancia de Active Directory de origen. La herramienta incluye en scripts los roles de servidor y de base de datos personalizados, así como la pertenencia a roles, el rol de base de datos y los permisos de usuario. Actualmente no se admiten bases de datos independientes y solo un subconjunto de los permisos de SQL Server posibles se incluyen en el script. 

#### <a name="encryption"></a>Cifrado

Al migrar bases de datos protegidas mediante  [Cifrado de datos transparente](../../database/transparent-data-encryption-tde-overview.md) a una instancia administrada con la opción de restauración nativa, [se debe migrar el certificado correspondiente](../../managed-instance/tde-certificate-migrate.md) de la instancia de SQL Server de origen a la instancia de SQL Managed Instance de destino *antes* de restaurar la base de datos. 

#### <a name="system-databases"></a>Bases de datos del sistema

No se permite restaurar bases de datos del sistema. Para migrar objetos de nivel de instancia (almacenados en bases de datos maestras o msdb), inclúyalos en scripts con Transact-SQL (T-SQL) y vuelva a crearlos en la instancia administrada de destino. 

## <a name="leverage-advanced-features"></a>Aprovechamiento de las características avanzadas 

Asegúrese de aprovechar las características avanzadas basadas en la nube que ofrece SQL Managed Instance. Por ejemplo, ya no necesita preocuparse por la administración de copias de seguridad, ya que el servicio la hace automáticamente. Puede realizar la restauración a cualquier [momento dado dentro del período de retención](../../database/recovery-using-backups.md#point-in-time-restore). Además, dado que la  [alta disponibilidad está integrada](../../database/high-availability-sla.md), no es necesario preocuparse por su configuración. 

Para reforzar la seguridad, considere la posibilidad de usar las características de  [autenticación](../../database/authentication-aad-overview.md), [auditoría](../../managed-instance/auditing-configure.md),  [detección de amenazas](../../database/azure-defender-for-sql.md),  [seguridad de nivel de fila](/sql/relational-databases/security/row-level-security) y  [enmascaramiento dinámico de datos](/sql/relational-databases/security/dynamic-data-masking) de Azure Active Directory.

Además de la administración avanzada y las características de seguridad, SQL Managed Instance ofrece un conjunto de herramientas avanzadas que pueden ayudarle a [supervisar y optimizar la carga de trabajo](../../database/monitor-tune-overview.md). [Azure SQL Analytics](../../../azure-monitor/insights/azure-sql.md) le permite supervisar un gran conjunto de instancias administradas de manera centralizada. [Ajuste automático](/sql/relational-databases/automatic-tuning/automatic-tuning#automatic-plan-correction) en instancias administradas supervisa continuamente el rendimiento de las estadísticas de ejecución del plan de SQL y corrige automáticamente los problemas de rendimiento identificados. 

Algunas características solo están disponibles cuando se cambia el [nivel de compatibilidad de la base de datos](/sql/relational-databases/databases/view-or-change-the-compatibility-level-of-a-database) al nivel de compatibilidad más reciente (150). 

## <a name="migration-assets"></a>Recursos de migración 

Para obtener más ayuda, consulte los siguientes recursos, que se desarrollaron para proyectos de migración reales.

|Recurso  |Descripción  |
|---------|---------|
|[Herramienta y modelo de evaluación de la carga de trabajo de datos](https://github.com/Microsoft/DataMigrationTeam/tree/master/Data%20Workload%20Assessment%20Model%20and%20Tool)| Esta herramienta proporciona las plataformas de destino de ajuste perfecto sugeridas, la preparación para la nube, y el nivel de corrección de la aplicación o base de datos para una carga de trabajo determinada. Ofrece un cálculo sencillo con un solo clic y una función de generación de informes que ayuda a acelerar las evaluaciones de grandes volúmenes, ya que proporciona un proceso de toma de decisiones de plataforma de destino uniforme y automatizado.|
|[Utilidad DBLoader](https://github.com/microsoft/DataMigrationTeam/tree/master/DBLoader%20Utility)|DBLoader se puede usar para cargar datos de archivos de texto delimitados en SQL Server. Esta utilidad de consola de Windows usa la interfaz de carga masiva de cliente nativa de SQL Server, que funciona en todas las versiones de SQL Server, incluida Azure SQL Managed Instance.|
|[Utilidad para migrar los inicios de sesión de SQL Server locales a Azure SQL Managed Instance](https://github.com/microsoft/DataMigrationTeam/tree/master/IP%20and%20Scripts/MoveLogins)|Un script de PowerShell que crea un script de comandos de T-SQL para volver a crear inicios de sesión y seleccionar usuarios de base de datos de SQL Server local para Azure SQL Managed Instance. La herramienta permite la asignación automática de cuentas de Windows AD a cuentas de Azure AD y, opcionalmente, la migración de inicios de sesión nativos de SQL Server.|
|[Automatización de la recopilación de datos de Perfmon mediante Logman](https://github.com/microsoft/DataMigrationTeam/tree/master/IP%20and%20Scripts/Perfmon%20Data%20Collection%20Automation%20Using%20Logman)|Herramienta que recopila datos de Perfmon para comprender el rendimiento de la base de referencia que ayuda en la recomendación de destino de la migración. Esta herramienta utiliza logman.exe para crear el comando que creará, iniciará, detendrá y eliminará los contadores de rendimiento establecidos en una instancia de SQL Server remota.|
|[Notas del producto: migración de bases de datos a Azure SQL Managed Instance mediante la restauración de copias de seguridad completas y diferenciales](https://github.com/microsoft/DataMigrationTeam/blob/master/Whitepapers/Database%20migrations%20to%20Azure%20SQL%20DB%20Managed%20Instance%20-%20%20Restore%20with%20Full%20and%20Differential%20backups.pdf)|En estas notas del producto se proporcionan instrucciones y pasos para ayudar a acelerar las migraciones de SQL Server a Azure SQL Managed Instance si solo tiene copias de seguridad completas y diferenciales (y ninguna funcionalidad de copia de seguridad de registros).|

Estos recursos se desarrollaron como parte del programa Ninja SQL de datos, que está patrocinado por el equipo de ingeniería de grupos de datos de Azure. El objetivo principal del programa Ninja SQL de datos es lograr y acelerar la modernización compleja, y competir por las oportunidades de migración de las plataformas de datos a la de Azure, de Microsoft. Si cree que su organización estaría interesada en participar en el programa Ninja SQL de datos, póngase en contacto con su equipo de cuentas y pídale que le envíe una nominación.


## <a name="next-steps"></a>Pasos siguientes

Para empezar a migrar SQL Server a Azure SQL Managed Instance, consulte la [guía de migración de SQL Server a SQL Managed Instance](sql-server-to-managed-instance-guide.md).

- Para obtener una matriz de los servicios y las herramientas de Microsoft y de otros fabricantes que están disponibles para ayudarlo en diversos escenarios de migración de datos y bases de datos, además de las tareas especializadas, consulte [Servicios y herramientas de migración de datos](../../../dms/dms-tools-matrix.md).

- Para obtener más información acerca Azure SQL Managed Instance, consulte:
   - [Niveles de servicio en Azure SQL Managed Instance](../../managed-instance/sql-managed-instance-paas-overview.md#service-tiers)
   - [Diferencias entre SQL Server y Azure SQL Managed Instance](../../managed-instance/transact-sql-tsql-differences-sql-server.md)
   - [Calculadora del costo total de propiedad de Azure](https://azure.microsoft.com/pricing/tco/calculator/) 


- Para más información sobre el marco y el ciclo de adopción de las migraciones en la nube, consulte:
   -  [Cloud Adoption Framework para Azure](/azure/cloud-adoption-framework/migrate/azure-best-practices/contoso-migration-scale)
   -  [Procedimientos recomendados para la gestión de los costos y los ajustes de tamaño de las cargas de trabajo migradas a Azure](/azure/cloud-adoption-framework/migrate/azure-best-practices/migrate-best-practices-costs) 


- Para evaluar el nivel de acceso de la aplicación, consulte [Data Access Migration Toolkit (versión preliminar)](https://marketplace.visualstudio.com/items?itemName=ms-databasemigration.data-access-migration-toolkit)
- Para más información sobre cómo realizar pruebas A/B en la capa de acceso a datos, consulte [Información general del Asistente para experimentación con bases de datos](/sql/dea/database-experimentation-assistant-overview).