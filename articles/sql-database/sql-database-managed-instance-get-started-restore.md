---
title: Restauración de una copia de seguridad en una instancia administrada
description: Restaure una copia de seguridad de datos a una instancia administrada de Azure SQL Database mediante SSMS.
services: sql-database
ms.service: sql-database
ms.subservice: managed-instance
ms.custom: ''
ms.devlang: ''
ms.topic: quickstart
author: srdan-bozovic-msft
ms.author: srbozovi
ms.reviewer: sstein, carlrab, bonova
ms.date: 12/14/2018
ms.openlocfilehash: 3b12aa860bee189f706bd98df63fc194a3a0cc71
ms.sourcegitcommit: 2d7910337e66bbf4bd8ad47390c625f13551510b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80874711"
---
# <a name="quickstart-restore-a-database-to-a-managed-instance"></a>Inicio rápido: Restauración de una base de datos en una instancia administrada

En este inicio rápido, usará SQL Server Management Studio (SSMS) para restaurar una base de datos (el archivo de copia de seguridad de Wide World Importers - Standard) de Azure Blob Storage en una [instancia administrada](sql-database-managed-instance.md) de Azure SQL Database.

> [!VIDEO https://www.youtube.com/embed/RxWYojo_Y3Q]

> [!NOTE]
> Para más información sobre la migración con Azure Database Migration Service (DMS), consulte [Migración de Instancia administrada mediante DMS](../dms/tutorial-sql-server-to-managed-instance.md).
> Para más información sobre los diversos métodos de migración, consulte [Migración de una instancia de SQL Server a Instancia administrada de Azure SQL Database](sql-database-managed-instance-migrate.md).

## <a name="prerequisites"></a>Prerrequisitos

En esta guía de inicio rápido:

- Se usan los recursos del inicio rápido [Creación de una instancia administrada](sql-database-managed-instance-get-started.md).
- Es necesario que el equipo tenga instalada la versión más reciente de [SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/sql-server-management-studio-ssms).
- Es necesario el uso de SSMS para conectarse a la instancia administrada. Consulte estos inicios rápidos sobre procedimientos de conexión:
  - [Habilitar el punto de conexión público](sql-database-managed-instance-public-endpoint-configure.md) en una instancia administrada: este es el método recomendado para este tutorial.
  - [Conexión a una Instancia administrada de Azure SQL Database desde una máquina virtual de Azure](sql-database-managed-instance-configure-vm.md)
  - [Configuración de una conexión de punto a sitio a una instancia administrada de Azure SQL Database desde el entorno local](sql-database-managed-instance-configure-p2s.md).

> [!NOTE]
> Para más información sobre cómo realizar una copia de seguridad de una base de datos de SQL Server y restaurarla mediante Azure Blob Storage y una clave de [Firma de acceso compartido (SAS)](https://docs.microsoft.com/azure/storage/common/storage-dotnet-shared-access-signature-part-1), consulte [Copia de seguridad de SQL Server en una dirección URL](https://docs.microsoft.com/sql/relational-databases/backup-restore/sql-server-backup-to-url?view=sql-server-2017).

## <a name="restore-the-database-from-a-backup-file"></a>Restauración de la base de datos desde un archivo de copia de seguridad

En SSMS, siga estos pasos para restaurar la base de datos Wide World Importers en la instancia administrada. El archivo de copia de seguridad de base de datos se almacena en una cuenta de Azure Blob Storage configurada previamente.

1. Abra SSMS y conéctese a la instancia administrada.
2. En el menú izquierdo, haga clic con el botón derecho en la instancia administrada y seleccione **Nueva consulta** para abrir una nueva ventana de consulta.
3. Ejecute el siguiente script de SQL, que usa una cuenta de almacenamiento configurada previamente y la clave SAS para [crear una credencial](https://docs.microsoft.com/sql/t-sql/statements/create-credential-transact-sql) en la instancia administrada.

   ```sql
   CREATE CREDENTIAL [https://mitutorials.blob.core.windows.net/databases]
   WITH IDENTITY = 'SHARED ACCESS SIGNATURE'
   , SECRET = 'sv=2017-11-09&ss=bfqt&srt=sco&sp=rwdlacup&se=2028-09-06T02:52:55Z&st=2018-09-04T18:52:55Z&spr=https&sig=WOTiM%2FS4GVF%2FEEs9DGQR9Im0W%2BwndxW2CQ7%2B5fHd7Is%3D'
   ```

    ![crear credencial](./media/sql-database-managed-instance-get-started-restore/credential.png)

4. Para comprobar sus credenciales, ejecute el siguiente script, que usa una dirección URL de [contenedor](https://azure.microsoft.com/services/container-instances/) para obtener una lista de archivos de copia de seguridad.

   ```sql
   RESTORE FILELISTONLY FROM URL =
      'https://mitutorials.blob.core.windows.net/databases/WideWorldImporters-Standard.bak'
   ```

    ![lista de archivos](./media/sql-database-managed-instance-get-started-restore/file-list.png)

5. Ejecute el siguiente script para restaurar la base de datos Wide World Importers.

   ```sql
   RESTORE DATABASE [Wide World Importers] FROM URL =
     'https://mitutorials.blob.core.windows.net/databases/WideWorldImporters-Standard.bak'
   ```

    ![Restauración](./media/sql-database-managed-instance-get-started-restore/restore.png)

6. Ejecute el siguiente script para realizar un seguimiento del estado de la restauración.

   ```sql
   SELECT session_id as SPID, command, a.text AS Query, start_time, percent_complete
      , dateadd(second,estimated_completion_time/1000, getdate()) as estimated_completion_time
   FROM sys.dm_exec_requests r
   CROSS APPLY sys.dm_exec_sql_text(r.sql_handle) a
   WHERE r.command in ('BACKUP DATABASE','RESTORE DATABASE')
   ```

7. Cuando se complete la restauración, consulte la base de datos en el Explorador de objetos. Puede comprobar si se ha completado la restauración de bases de datos mediante la vista [sys.dm_operation_status](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-operation-status-azure-sql-database).

> [!NOTE]
> La operación de restauración de bases de datos es asincrónica y admite reintentos. Es posible que se produzca algún error en SQL Server Management Studio si se interrumpe la conexión o se agota el tiempo de espera. Azure SQL Database seguirá intentando restaurar la base de datos en segundo plano y puede realizar un seguimiento del progreso de la restauración mediante las vistas [sys.dm_exec_requests](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql) y [sys.dm_operation_status](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-operation-status-azure-sql-database).
> En algunas fases del proceso de restauración verá un identificador único en lugar del nombre de base de datos real de las vistas del sistema. Obtenga información sobre las diferencias de comportamiento de la instrucción `RESTORE`[aquí](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-transact-sql-information#restore-statement).

## <a name="next-steps"></a>Pasos siguientes

- Para solucionar problemas con una copia de seguridad en la dirección URL, consulte [Prácticas recomendadas y solución de problemas de Copia de seguridad en URL de SQL Server](https://docs.microsoft.com/sql/relational-databases/backup-restore/sql-server-backup-to-url-best-practices-and-troubleshooting).
- Para información general sobre las opciones de conexión de aplicaciones, consulte [Conexión de las aplicaciones a Instancia administrada](sql-database-managed-instance-connect-app.md).
- Para realizar consultas con sus herramientas o lenguajes favoritos, consulte [Inicios rápidos: Conexión y consulta de Azure SQL Database](sql-database-connect-query.md).
