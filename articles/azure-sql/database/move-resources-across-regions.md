---
title: Traslado de recursos a una nueva región
titleSuffix: Azure SQL Database & Azure SQL Managed Instance
description: Obtenga información sobre cómo mover una instancia de Azure SQL Database o Instancia administrada de Azure SQL a otra región.
services: sql-database
ms.service: sql-database
ms.subservice: data-movement
ms.custom: sqldbrb=2
ms.devlang: ''
ms.topic: conceptual
author: MashaMSFT
ms.author: mathoma
ms.reviewer: carlrab
ms.date: 06/25/2019
ms.openlocfilehash: 82481375e5901c681cc921da733377b2ab01d206
ms.sourcegitcommit: 053e5e7103ab666454faf26ed51b0dfcd7661996
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/27/2020
ms.locfileid: "84040926"
---
# <a name="move-resources-to-new-region---azure-sql-database--azure-sql-managed-instance"></a>Traslado de recursos a una nueva región: Azure SQL Database e Instancia administrada de Azure SQL
[!INCLUDE[appliesto-sqldb-sqlmi](../includes/appliesto-sqldb-sqlmi.md)]

En este artículo se enseña un flujo de trabajo genérico sobre cómo mover una instancia de Azure SQL Database o Instancia administrada de Azure SQL a otra región.

## <a name="overview"></a>Información general

Hay varios escenarios en los que puede desear mover una instancia existente de Azure SQL Database o Instancia administrada de una región a otra. Por ejemplo, puede ampliar su negocio a una nueva región y desear optimizarla para la nueva base de clientes. O bien, debe migrar las operaciones a una otra región por motivos de cumplimiento. O también que Azure ha lanzado una nueva región que proporciona una mejor proximidad y mejora la experiencia del cliente.  

En este artículo se proporciona un flujo de trabajo general para migrar los recursos a otra región. El flujo de trabajo consta de los pasos siguientes:

- Comprobar los requisitos previos para el traslado
- Preparar la migración de los recursos en el ámbito
- Supervisión del proceso de preparación
- Probar el proceso de migración
- Iniciar la migración real
- Quitar los recursos de la región de origen

> [!NOTE]
> Este artículo se aplica a las migraciones dentro de la nube pública de Azure o dentro de la misma nube soberana.

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

## <a name="move-azure-sql-database"></a>Traslado de Azure SQL Database

### <a name="verify-prerequisites"></a>Verificar los requisitos previos

1. Cree un servidor de destino para cada servidor de origen.
1. Configure el firewall con las excepciones correctas mediante [PowerShell](scripts/create-and-configure-database-powershell.md).  
1. Configure los servidores con los inicios de sesión correctos. Si no es el administrador de la suscripción o el administrador del servidor de SQL, solicite al administrador que le asigne los permisos que necesita. Para obtener más información, consulte [Administración de la seguridad de Azure SQL Database después de la recuperación ante desastres](active-geo-replication-security-configure.md).
1. Si las bases de datos están cifradas con TDE y usan su propia clave de cifrado en Azure Key Vault, asegúrese de que se aprovisiona el material de cifrado correcto en las regiones de destino. Para más información, consulte [Cifrado de datos transparente de Azure SQL con una clave administrada por el cliente de Azure Key Vault](transparent-data-encryption-byok-overview.md).
1. Si está habilitada la auditoría de nivel de base de datos, deshabilítela y habilite la auditoría de nivel de servidor en su lugar. Después de la conmutación por error, la auditoría de nivel de base de datos requerirá el tráfico entre regiones, que no es lo que se desea ni es posible después de la migración.
1. En el caso de las auditorías de nivel de servidor, asegúrese de que:
   - El contenedor de almacenamiento, Log Analytics o el centro de eventos con los registros de auditoría existentes migran a la región de destino.
   - La auditoría se configura en el servidor de destino. Para obtener más información, consulte la [Introducción a la auditoría de SQL Database](../../azure-sql/database/auditing-overview.md).
1. Si la instancia tiene una directiva de retención a largo plazo (LTR), las copias de seguridad de LTR existentes permanecerán asociadas al servidor actual. Dado que el servidor de destino es diferente, podrá tener acceso a las copias de seguridad de LTR anteriores de la región de origen mediante el servidor de origen, aunque se elimine el servidor.

  > [!NOTE]
  > Esto no será suficiente para el cambio entre la nube soberana y una región pública. Este tipo de migración requerirá mover las copias de seguridad de LTR al servidor de destino, que no se admite actualmente.

### <a name="prepare-resources"></a>Preparación de recursos

1. Cree un [grupo de conmutación por error](failover-group-add-single-database-tutorial.md#2---create-the-failover-group) entre el servidor del origen y el servidor del destino.  
1. Agregue las bases de datos que desea migrar al grupo de conmutación por error.
    - La replicación de todas las bases de datos agregadas se iniciará automáticamente. Para más información, consulte [Procedimientos recomendados para usar grupos de conmutación por error con bases de datos únicas](auto-failover-group-overview.md#best-practices-for-sql-database).

### <a name="monitor-the-preparation-process"></a>Supervisión del proceso de preparación

Puede llamar periódicamente a [Get-AzSqlDatabaseFailoverGroup](/powershell/module/az.sql/get-azsqldatabasefailovergroup) para supervisar la replicación de las bases de datos desde el origen al destino. El objeto de salida de `Get-AzSqlDatabaseFailoverGroup` incluye una propiedad para **ReplicationState**:

- **ReplicationState = 2** (CATCH_UP) indica que la base de datos está sincronizada y se puede conmutar por error de forma segura.
- **ReplicationState = 0** (SEEDING) indica que la base de datos aún no se ha inicializado y se producirá un error en un intento de conmutación por error.

### <a name="test-synchronization"></a>Prueba de la sincronización

Una vez que **ReplicationState** es `2`, se conecta a cada base de datos o a un subconjunto de bases de datos que usan el punto de conexión secundario `<fog-name>.secondary.database.windows.net` y realiza cualquier consulta en las bases de datos para garantizar la conectividad, la configuración de seguridad adecuada y la replicación de datos.

### <a name="initiate-the-move"></a>Inicio de la migración

1. Conéctese al servidor de destino mediante el punto de conexión secundario `<fog-name>.secondary.database.windows.net`.
1. Use [Switch-AzSqlDatabaseFailoverGroup](/powershell/module/az.sql/switch-azsqldatabasefailovergroup) para cambiar la instancia administrada secundaria para que sea la principal con sincronización completa. Esta operación se realizará correctamente o se revertirá.
1. Compruebe que el comando se ha completado correctamente mediante `nslook up <fog-name>.secondary.database.windows.net` para determinar que la entrada CNAME de DNS apunta a la dirección IP de la región de destino. Si se produce un error en el comando switch, no se actualizará CNAME.

### <a name="remove-the-source-databases"></a>Supresión de las bases de datos de origen

Una vez completada la migración, quite los recursos de la región de origen para evitar cargos innecesarios.

1. Elimine el grupo de conmutación por error mediante [Remove-AzSqlDatabaseFailoverGroup](/powershell/module/az.sql/remove-azsqldatabasefailovergroup).
1. Elimine cada base de datos de origen mediante [Remove-AzSqlDatabase](/powershell/module/az.sql/remove-azsqldatabase) para cada una de las bases de datos del servidor de origen. Esto finalizará automáticamente los vínculos de replicación geográfica.
1. Elimine el servidor de origen mediante [Remove-AzSqlServer](/powershell/module/az.sql/remove-azsqlserver).
1. Quite el almacén de claves, los contenedores de almacenamiento de auditoría, el centro de eventos, la instancia de AAD y otros recursos dependientes para dejar de recibir facturas por ellos.

## <a name="move-elastic-pools"></a>Migración de grupos elásticos

### <a name="verify-prerequisites"></a>Verificar los requisitos previos

1. Cree un servidor de destino para cada servidor de origen.
1. Configure el firewall con las excepciones correctas mediante [PowerShell](scripts/create-and-configure-database-powershell.md).
1. Configure los servidores con los inicios de sesión correctos. Si no es el administrador de la suscripción ni el administrador del servidor, solicite al administrador que le asigne los permisos que necesita. Para obtener más información, consulte [Administración de la seguridad de Azure SQL Database después de la recuperación ante desastres](active-geo-replication-security-configure.md).
1. Si las bases de datos están cifradas con TDE y usan su propia clave de cifrado en Azure Key Vault, asegúrese de que se aprovisiona el material de cifrado correcto en las regiones de destino.
1. Cree un grupo elástico de destino para cada grupo elástico de origen y asegúrese de que el grupo se crea en el mismo nivel de servicio, con el mismo nombre y el mismo tamaño.
1. Si está habilitada una auditoría de nivel de base de datos, deshabilítela y habilite la auditoría de nivel de servidor en su lugar. Después de la conmutación por error, la auditoría de nivel de base de datos requerirá el tráfico entre regiones, que no es lo que se desea ni es posible después de la migración.
1. En el caso de las auditorías de nivel de servidor, asegúrese de que:
    - El contenedor de almacenamiento, Log Analytics o el centro de eventos con los registros de auditoría existentes migran a la región de destino.
    - La configuración de la auditoría se prepara en el servidor de destino. Para más información, consulte [Introducción a la auditoría de bases de datos SQL](../../azure-sql/database/auditing-overview.md).
1. Si la instancia tiene una directiva de retención a largo plazo (LTR), las copias de seguridad de LTR existentes permanecerán asociadas al servidor actual. Dado que el servidor de destino es diferente, podrá tener acceso a las copias de seguridad de LTR anteriores de la región de origen mediante el servidor de origen, aunque se elimine el servidor.

  > [!NOTE]
  > Esto no será suficiente para el cambio entre la nube soberana y una región pública. Este tipo de migración requerirá mover las copias de seguridad de LTR al servidor de destino, que no se admite actualmente.

### <a name="prepare-to-move"></a>Preparación para la migración

1. Cree un [grupo de conmutación por error](failover-group-add-elastic-pool-tutorial.md#3---create-the-failover-group) independiente entre cada grupo elástico del servidor de origen y su grupo elástico equivalente en el servidor de destino.
1. Agregue todas las bases de datos del grupo al grupo de conmutación por error.
    - La replicación de las bases de datos agregadas se iniciará automáticamente. Para obtener más información, consulte los [procedimientos recomendados para grupos de conmutación por error con grupos elásticos](auto-failover-group-overview.md#best-practices-for-sql-database).

  > [!NOTE]
  > Aunque es posible crear un grupo de conmutación por error que incluya varios grupos elásticos, se recomienda especialmente que cree un grupo de conmutación por error independiente para cada grupo. Si tiene un gran número de bases de datos en varios grupos elásticos que debe migrar, puede ejecutar los pasos de preparación en paralelo y, a continuación, iniciar el paso de la migración en paralelo. Este proceso se escalará mejor y tendrá menos tiempo en comparación con la existencia de varios grupos elásticos en el mismo grupo de conmutación por error.

### <a name="monitor-the-preparation-process"></a>Supervisión del proceso de preparación

Puede llamar periódicamente a [Get-AzSqlDatabaseFailoverGroup](/powershell/module/az.sql/get-azsqldatabasefailovergroup) para supervisar la replicación de las bases de datos desde el origen al destino. El objeto de salida de `Get-AzSqlDatabaseFailoverGroup` incluye una propiedad para **ReplicationState**:

- **ReplicationState = 2** (CATCH_UP) indica que la base de datos está sincronizada y se puede conmutar por error de forma segura.
- **ReplicationState = 0** (SEEDING) indica que la base de datos aún no se ha inicializado y se producirá un error en un intento de conmutación por error.

### <a name="test-synchronization"></a>Prueba de la sincronización

Una vez que **ReplicationState** es `2`, se conecta a cada base de datos o a un subconjunto de bases de datos que usan el punto de conexión secundario `<fog-name>.secondary.database.windows.net` y realiza cualquier consulta en las bases de datos para garantizar la conectividad, la configuración de seguridad adecuada y la replicación de datos.

### <a name="initiate-the-move"></a>Inicio de la migración

1. Conéctese al servidor de destino mediante el punto de conexión secundario `<fog-name>.secondary.database.windows.net`.
1. Use [Switch-AzSqlDatabaseFailoverGroup](/powershell/module/az.sql/switch-azsqldatabasefailovergroup) para cambiar la instancia administrada secundaria para que sea la principal con sincronización completa. Esta operación se realizará correctamente o se revertirá.
1. Compruebe que el comando se ha completado correctamente mediante `nslook up <fog-name>.secondary.database.windows.net` para determinar que la entrada CNAME de DNS apunta a la dirección IP de la región de destino. Si se produce un error en el comando switch, no se actualizará CNAME.

### <a name="remove-the-source-elastic-pools"></a>Eliminación de los grupos elásticos de origen

Una vez completada la migración, quite los recursos de la región de origen para evitar cargos innecesarios.

1. Elimine el grupo de conmutación por error mediante [Remove-AzSqlDatabaseFailoverGroup](/powershell/module/az.sql/remove-azsqldatabasefailovergroup).
1. Elimine cada grupo elástico de origen en el servidor de origen mediante [Remove-AzSqlElasticPool](/powershell/module/az.sql/remove-azsqlelasticpool).
1. Elimine el servidor de origen mediante [Remove-AzSqlServer](/powershell/module/az.sql/remove-azsqlserver).
1. Quite el almacén de claves, los contenedores de almacenamiento de auditoría, el centro de eventos, la instancia de AAD y otros recursos dependientes para dejar de recibir facturas por ellos.

## <a name="move-azure-sql-managed-instance"></a>Traslado de Instancia administrada de Azure SQL

### <a name="verify-prerequisites"></a>Verificar los requisitos previos

1. Para cada Instancia administrada de SQL de origen, cree una Instancia administrada de SQL de destino del mismo tamaño en la región de destino.  
1. Configure la red para la Instancia administrada de SQL. Para más información, consulte [Configuración de la red](../managed-instance/how-to-content-reference-guide.md#network-configuration).
1. Configure la base de datos maestra de destino con los inicios de sesión correctos. Si no es el administrador de la suscripción ni de la Instancia administrada de SQL, solicite al administrador que le asigne los permisos que necesita.
1. Si las bases de datos están cifradas con TDE y usan su propia clave de cifrado en Azure Key Vault, asegúrese de que la instancia de Azure Key Vault con claves de cifrado idénticas exista en las regiones de origen y de destino. Para más información, consulte [Cifrado de datos transparente con claves administradas por el cliente en Azure Key Vault](transparent-data-encryption-byok-overview.md).
1. Si la auditoría está habilitada para la Instancia administrada de SQL, asegúrese de que:
    - El contenedor de almacenamiento o el centro de eventos con los registros existentes migran a la región de destino.
    - La auditoría se configura en la instancia de destino. Para más información, consulte la [introducción a la auditoría con la instancia administrada](../managed-instance/auditing-configure.md).
1. Si la instancia tiene una directiva de retención a largo plazo (LTR), las copias de seguridad de LTR existentes permanecerán asociadas a la instancia actual. Dado que la instancia de destino es diferente, podrá tener acceso a las copias de seguridad de LTR anteriores de la región de origen mediante la instancia de origen, aunque se elimine la instancia.

  > [!NOTE]
  > Esto no será suficiente para el cambio entre la nube soberana y una región pública. Este tipo de migración requerirá mover las copias de seguridad de LTR a la instancia de destino, lo que no se admite actualmente.

### <a name="prepare-resources"></a>Preparación de recursos

Cree un grupo de conmutación por error entre cada Instancia administrada de SQL de origen y la Instancia administrada de SQL de destino correspondiente.
    - La replicación de todas las bases de datos en cada instancia se iniciará automáticamente. Para más información, consulte [Grupos de conmutación por error automática](auto-failover-group-overview.md).

### <a name="monitor-the-preparation-process"></a>Supervisión del proceso de preparación

Puede llamar periódicamente a [Get-AzSqlDatabaseFailoverGroup](/powershell/module/az.sql/get-azsqldatabasefailovergroup?view=azps-2.3.2) para supervisar la replicación de las bases de datos desde el origen al destino. El objeto de salida de `Get-AzSqlDatabaseFailoverGroup` incluye una propiedad para **ReplicationState**:

- **ReplicationState = 2** (CATCH_UP) indica que la base de datos está sincronizada y se puede conmutar por error de forma segura.
- **ReplicationState = 0** (SEEDING) indica que la base de datos aún no se ha inicializado y se producirá un error en un intento de conmutación por error.

### <a name="test-synchronization"></a>Prueba de la sincronización

Una vez que **ReplicationState** es `2`, se conecta a cada base de datos o a un subconjunto de bases de datos que usan el punto de conexión secundario `<fog-name>.secondary.database.windows.net` y realiza cualquier consulta en las bases de datos para garantizar la conectividad, la configuración de seguridad adecuada y la replicación de datos.

### <a name="initiate-the-move"></a>Inicio de la migración

1. Conéctese a la Instancia administrada de SQL de destino mediante el punto de conexión secundario `<fog-name>.secondary.database.windows.net`.
1. Use [Switch-AzSqlDatabaseFailoverGroup](/powershell/module/az.sql/switch-azsqldatabasefailovergroup?view=azps-2.3.2) para cambiar la instancia administrada secundaria para que sea la principal con sincronización completa. Esta operación se realizará correctamente o se revertirá.
1. Compruebe que el comando se ha completado correctamente mediante `nslook up <fog-name>.secondary.database.windows.net` para determinar que la entrada CNAME de DNS apunta a la dirección IP de la región de destino. Si se produce un error en el comando switch, no se actualizará CNAME.

### <a name="remove-the-source-managed-instances"></a>Eliminación de las instancias administradas de origen

Una vez completada la migración, quite los recursos de la región de origen para evitar cargos innecesarios.

1. Elimine el grupo de conmutación por error mediante [Remove-AzSqlDatabaseFailoverGroup](/powershell/module/az.sql/remove-azsqldatabasefailovergroup). Esto quitará la configuración del grupo de conmutación por error y finalizará los vínculos de replicación geográfica entre las dos instancias.
1. Elimine la instancia administrada de origen mediante [Remove-AzSqlInstance](/powershell/module/az.sql/remove-azsqlinstance).
1. Quite cualquier recurso adicional en el grupo de recursos, como el clúster virtual, la red virtual y el grupo de seguridad.

## <a name="next-steps"></a>Pasos siguientes

[Administre](manage-data-after-migrating-to-database.md) la instancia de Azure SQL Database una vez que se haya migrado.
