---
title: Auditoría de Instancia administrada
description: Obtención de información acerca de cómo auditar Instancia administrada de Azure SQL Database mediante T-SQL
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
f1_keywords:
- mi.azure.sqlaudit.general.f1
author: DavidTrigano
ms.author: datrigan
ms.reviewer: vanto
ms.date: 05/26/2020
ms.openlocfilehash: 66c2c5e52ef81f74f7177af897f33eec4ae1779e
ms.sourcegitcommit: 95269d1eae0f95d42d9de410f86e8e7b4fbbb049
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/26/2020
ms.locfileid: "83860650"
---
# <a name="azure-sql-database-managed-instance-auditing"></a>Auditoría de instancia administrada de Azure SQL Database

La auditoría de [Instancia administrada](sql-database-managed-instance.md) hace un seguimiento de eventos de bases de datos y los escribe en un registro de auditoría de su cuenta de Azure Storage. La auditoría también puede hacer lo siguiente:

- Ayudar a mantener el cumplimiento de normativas, comprender la actividad de las bases de datos y conocer las discrepancias y anomalías que pueden indicar problemas en el negocio o infracciones de seguridad sospechosas.
- Posibilitar y facilitar la observancia de estándares reguladores aunque no garantiza el cumplimiento. Para obtener más información sobre los programas de Azure que se adhieren al cumplimiento normativo, consulte el [Centro de confianza de Azure](https://gallery.technet.microsoft.com/Overview-of-Azure-c1be3942), donde podrá encontrar la lista más reciente de certificaciones de cumplimiento de SQL Database.

## <a name="set-up-auditing-for-your-server-to-azure-storage"></a>Configuración de la auditoría de un servidor en Azure Storage

En la sección siguiente se describe la configuración de auditoría en su Instancia administrada.

1. Vaya a [Azure Portal](https://portal.azure.com).
2. Cree un **contenedor** de Azure Storage donde se almacenan los registros de auditoría.

   1. Navegue hasta Azure Storage donde le gustaría almacenar los registros de auditoría.

      > [!IMPORTANT]
      > - Use una cuenta de almacenamiento de la misma región que la Instancia administrada para evitar lecturas y escrituras entre regiones. 
      > - Si la cuenta de almacenamiento está detrás de una Virtual Network o un firewall, consulte [conceder acceso desde una red virtual](https://docs.microsoft.com/azure/storage/common/storage-network-security#grant-access-from-a-virtual-network).
      > - Si cambia el período de retención de 0 (retención ilimitada) a cualquier otro valor, tenga en cuenta que la retención solo se aplicará a los registros escritos una vez cambiado el valor de retención (los registros escritos durante el período en el que la retención se estableció en ilimitada se conservan, incluso después de habilitarse la retención).

   1. En la cuenta de almacenamiento, vaya a **Información general** y haga clic en **Blobs**.

      ![Widget de blobs de Azure](./media/sql-managed-instance-auditing/1_blobs_widget.png)

   1. En el menú superior, haga clic en **+ Contenedor** para crear un nuevo contenedor.

      ![Icono Crear contenedor de blobs](./media/sql-managed-instance-auditing/2_create_container_button.png)

   1. En **Nombre** proporcione un nombre de contenedor, establezca el nivel de acceso público en **Privado**y haga clic en **Aceptar**.

      ![Configuración de la opción Crear contenedor de blobs](./media/sql-managed-instance-auditing/3_create_container_config.png)
  > [!IMPORTANT]
  > Los clientes que quieran configurar un almacén de registros inmutable para los eventos de auditoría a nivel de servidor o de base de datos deben seguir las [instrucciones que proporciona Azure Storage](https://docs.microsoft.com/azure/storage/blobs/storage-blob-immutability-policies-manage#enabling-allow-protected-append-blobs-writes). (Asegúrese de haber seleccionado **Permitir anexiones adicionales** al configurar el almacenamiento de blobs inmutable).
  
3. Después de crear el contenedor para los registros de auditoría, hay dos maneras de configurarlo como destino de los registros de auditoría: [mediante T-SQL](#blobtsql) o [mediante la interfaz de usuario de SQL Server Management Studio (SSMS)](#blobssms):

   - <a id="blobtsql"></a>Configuración del almacenamiento de blobs para los registros de auditoría con T-SQL:

     1. En la lista de contenedores, haga clic en el contenedor recién creado y luego en **Propiedades del contenedor**.

        ![Botón Propiedades del contenedor de blobs](./media/sql-managed-instance-auditing/4_container_properties_button.png)

     1. Copie la dirección URL del contenedor haciendo clic en el icono de copia y guarde dicha dirección (por ejemplo, en el Bloc de notas) para un uso futuro. El formato de dirección URL del contenedor debe ser `https://<StorageName>.blob.core.windows.net/<ContainerName>`.

        ![Dirección URL de copia de contenedores de blob](./media/sql-managed-instance-auditing/5_container_copy_name.png)

     1. Genere un **token de SAS** de Azure Storage para conceder derechos de acceso de auditoría de Instancia administrada a la cuenta de almacenamiento:

        - Vaya a la cuenta de Azure Storage donde se creó el contenedor en el paso anterior.

        - Haga clic en **Firma de acceso compartido** en el menú Configuración de Storage.

          ![Icono de Firma de acceso compartido en el menú Configuración de Storage](./media/sql-managed-instance-auditing/6_storage_settings_menu.png)

        - Configure SAS de la siguiente manera:

          - **Servicios permitidos**: Blob

          - **Fecha de inicio**: para evitar problemas relacionados con la zona horaria, se recomienda utilizar la fecha de ayer.

          - **Fecha de finalización**: elija la fecha en que expira este token de SAS.

            > [!NOTE]
            > Renueve el token tras la expiración para evitar errores de auditoría.

          - Haga clic en **Generar SAS**.
            
            ![Configuración de SAS](./media/sql-managed-instance-auditing/7_sas_configure.png)

        - Después de hacer clic en Generar SAS, el token de SAS aparecerá en la parte inferior. Copie el token haciendo clic en el icono de copia y guárdelo (por ejemplo, en el Bloc de notas) para un uso futuro.

          ![Copia del token de SAS](./media/sql-managed-instance-auditing/8_sas_copy.png)

          > [!IMPORTANT]
          > Quite el signo de interrogación ("?") caracteres del principio del token.

     1. Conéctese a la Instancia administrada mediante SQL Server Management Studio (SSMS) o cualquier otra herramienta compatible.

     1. Ejecute la siguiente instrucción T-SQL para **crear una nueva credencial** utilizando la dirección URL del contenedor y el token de SAS que creó en los pasos anteriores:

        ```SQL
        CREATE CREDENTIAL [<container_url>]
        WITH IDENTITY='SHARED ACCESS SIGNATURE',
        SECRET = '<SAS KEY>'
        GO
        ```

     1. Ejecute la siguiente instrucción T-SQL para crear una nueva auditoría de servidor (elija su propio nombre de auditoría y use la dirección URL del contenedor que creó en los pasos anteriores). Si no se especifica, el valor predeterminado de `RETENTION_DAYS` es 0 (retención ilimitada):

        ```SQL
        CREATE SERVER AUDIT [<your_audit_name>]
        TO URL ( PATH ='<container_url>' [, RETENTION_DAYS =  integer ])
        GO
        ```

        1. Continúe por la [creación de una especificación de auditoría de servidor o una de auditoría de base de datos](#createspec)

   - <a id="blobssms"></a>Configuración del almacenamiento de blobs para los registros de auditoría mediante SQL Server Management Studio (SSMS) 18 (versión preliminar):

     1. Conéctese a la instancia administrada mediante la interfaz de usuario de SQL Server Management Studio (SSMS).

     1. Expanda la nota de raíz de Explorador de objetos.

     1. Expanda el nodo **Security**, haga clic con el botón derecho en el nodo **Audits** y haga clic en "Nueva auditoría":

        ![Expandir nodo security y audit](./media/sql-managed-instance-auditing/10_mi_SSMS_new_audit.png)

     1. Asegúrese de que la opción "URL" está seleccionada en **Audit destination** (Destino de auditoría) y haga clic en **Examinar**:

        ![Examinar Azure Storage](./media/sql-managed-instance-auditing/11_mi_SSMS_audit_browse.png)

     1. (Opcional) Inicie sesión en la cuenta de Azure:

        ![Inicio de sesión en Azure](./media/sql-managed-instance-auditing/12_mi_SSMS_sign_in_to_azure.png)

     1. Seleccione una suscripción, la cuenta de almacenamiento y el contenedor de blobs en los menús desplegables o cree su propio contenedor. Para ello, haga clic en **Crear**. Cuando haya terminado, haga clic en **Aceptar**:

        ![Seleccionar la suscripción de Azure, la cuenta de almacenamiento y el contenedor de blobs](./media/sql-managed-instance-auditing/13_mi_SSMS_select_subscription_account_container.png)

     1. Haga clic en **Aceptar** en el cuadro de diálogo "Crear auditoría".

4. <a id="createspec"></a>Después de configurar el contenedor de blobs como destino para los registros de auditoría, cree y habilite una especificación de auditoría de servidor o de base de datos como lo haría para SQL Server:

   - [Crear la guía de T-SQL de especificación de auditoría de servidor](https://docs.microsoft.com/sql/t-sql/statements/create-server-audit-specification-transact-sql)
   - [Crear la guía de T-SQL de especificación de auditoría de base de datos](https://docs.microsoft.com/sql/t-sql/statements/create-database-audit-specification-transact-sql)

5. Habilite la auditoría de servidor que ha creado en el paso 3:

    ```SQL
    ALTER SERVER AUDIT [<your_audit_name>]
    WITH (STATE=ON);
    GO
    ```

Para información adicional:

- [Diferencias de auditoría entre bases de datos únicas, grupos elásticos, e instancias administradas en Azure SQL Database y bases de datos de SQL Server](#auditing-differences-between-databases-in-azure-sql-database-and-databases-in-sql-server)
- [CREATE SERVER AUDIT](https://docs.microsoft.com/sql/t-sql/statements/create-server-audit-transact-sql)
- [ALTER SERVER AUDIT](https://docs.microsoft.com/sql/t-sql/statements/alter-server-audit-transact-sql)

## <a name="set-up-auditing-for-your-server-to-event-hub-or-azure-monitor-logs"></a>Configuración de la auditoría de un servidor en Event Hubs o registros de Azure Monitor

Los registros de auditoría de una instancia administrada se pueden enviar a Event Hubs o los registros de Azure Monitor. En esta sección se describe cómo configurar todo esto:

1. En [Azure Portal ](https://portal.azure.com/), vaya a la instancia administrada.

2. Haga clic en **Configuración de diagnóstico**.

3. Haga clic en **Activar diagnóstico**. Si ya está habilitado el diagnóstico, en su lugar, se mostrará *+Add diagnostic setting* (+Agregar configuración de diagnóstico).

4. Seleccione **SQLSecurityAuditEvents** en la lista de registros.

5. Elija un destino para los eventos de auditoría: Event Hubs, registros de Azure Monitor o ambos. Configure los parámetros necesarios (por ejemplo, el área de trabajo de Log Analytics) en cada destino.

6. Haga clic en **Save**(Guardar).

    ![Configuración de valores de diagnóstico](./media/sql-managed-instance-auditing/9_mi_configure_diagnostics.png)

7. Conéctese a la instancia administrada mediante **SQL Server Management Studio (SSMS)** o cualquier otro cliente compatible.

8. Ejecute la siguiente instrucción T-SQL para crear una auditoría de servidor:

    ```SQL
    CREATE SERVER AUDIT [<your_audit_name>] TO EXTERNAL_MONITOR;
    GO
    ```

9. Cree y habilite una especificación de auditoría de servidor o de base de datos como lo haría para SQL Server:

   - [Crear la guía de T-SQL de especificación de auditoría de servidor](https://docs.microsoft.com/sql/t-sql/statements/create-server-audit-specification-transact-sql)
   - [Crear la guía de T-SQL de especificación de auditoría de base de datos](https://docs.microsoft.com/sql/t-sql/statements/create-database-audit-specification-transact-sql)

10. Habilite la auditoría de servidor que creó en el paso 8:
 
    ```SQL
    ALTER SERVER AUDIT [<your_audit_name>]
    WITH (STATE=ON);
    GO
    ```

## <a name="consume-audit-logs"></a>Uso de registros de auditoría

### <a name="consume-logs-stored-in-azure-storage"></a>Uso de registros almacenados en Azure Storage

Existen varios métodos que puede usar para ver los registros de auditoría de blobs.

- Use la función del sistema `sys.fn_get_audit_file` (T-SQL) para devolver los datos de registro de auditoría en formato tabular. Para más información sobre el uso de esta función, vea la [documentación de sys.fn_get_audit_file](https://docs.microsoft.com/sql/relational-databases/system-functions/sys-fn-get-audit-file-transact-sql).

- Puede explorar los registros de auditoría con una herramienta como el [Explorador de Azure Storage](https://azure.microsoft.com/features/storage-explorer/). En Azure Storage, los registros de auditoría se guardan como una colección de archivos de blob dentro de un contenedor definido para que almacene los registros de auditoría. Para obtener más información sobre la jerarquía de la carpeta de almacenamiento, las convenciones de nomenclatura y el formato del registro, vea la [referencia del formato de registro de auditoría de blobs](https://go.microsoft.com/fwlink/?linkid=829599).

- Para una lista completa de métodos de consumo del registro de auditoría, consulte el [Get started with SQL database auditing](sql-database-auditing.md) (Introducción a la auditoría de base de datos SQL).

### <a name="consume-logs-stored-in-event-hub"></a>Uso de registros almacenados en Event Hubs

Para consumir datos de registros de auditoría desde el centro de eventos, deberá configurar una secuencia que consuma eventos y los escriba en un destino. Para obtener más información, consulte la documentación de Azure Event Hubs.

### <a name="consume-and-analyze-logs-stored-in-azure-monitor-logs"></a>Uso y análisis de los registros almacenados en los registros de Azure Monitor

Si se escriben registros de auditoría en registros de Azure Monitor, estarán disponibles en el área de trabajo de Log Analytics, donde podrá ejecutar búsquedas avanzadas en los datos de auditoría. Empiece navegando al área de trabajo de Log Analytics y, en la sección *General*, haga clic en *Registros* y escriba una consulta simple, como `search "SQLSecurityAuditEvents"`, para ver los registros de auditoría.  

Los registros de Azure Monitor proporcionan conclusiones operativas en tiempo real gracias a uso de paneles personalizados y de búsqueda integrados para analizar fácilmente millones de registros en todas las cargas de trabajo y servidores. Para información útil adicional sobre los comandos y el lenguaje de búsqueda de registros de Azure Monitor, consulte la [referencia de búsqueda de registros de Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/log-query/log-query-overview).

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]

## <a name="auditing-differences-between-databases-in-azure-sql-database-and-databases-in-sql-server"></a>Diferencias de auditoría entre bases de datos en Azure SQL Database y bases de datos en SQL Server

Las diferencias clave entre la auditoría en bases de datos en Azure SQL Database y bases de datos en SQL Server son las siguientes:

- Con la opción de implementación de instancia administrada en Azure SQL Database, la auditoría funciona en el nivel de servidor y almacena los archivos de registro `.xel` en Azure Blob Storage.
- En las máquinas virtuales o instancias locales de SQL Server, la auditoría funciona en el nivel de servidor, pero almacena los eventos en registros de eventos de Windows o del sistema de archivos.

En Instancia administrada, la auditoría de XEvent admite Azure Blob Storage como destino. **No se admiten** archivos ni registros de Windows.

Las principales diferencias en la sintaxis de `CREATE AUDIT` para la auditoría en Azure Blob Storage son:

- Se proporciona una nueva sintaxis de `TO URL` que permite especificar la dirección URL del contenedor de Azure Blob Storage donde se colocarán los archivos `.xel`.
- Se proporciona una nueva sintaxis `TO EXTERNAL MONITOR` para habilitar los destinos Event Hubs y registros de Azure Monitor.
- La sintaxis `TO FILE`**no se admite** porque SQL Database no puede acceder a los recursos compartidos de archivos de Windows.
- La opción de apagado **no se admite**.
- `queue_delay` de 0 **no se admite**.

## <a name="next-steps"></a>Pasos siguientes

- Para una lista completa de métodos de consumo del registro de auditoría, consulte el [Get started with SQL database auditing](sql-database-auditing.md) (Introducción a la auditoría de base de datos SQL).
- Para obtener más información sobre los programas de Azure que se adhieren al cumplimiento normativo, consulte el [Centro de confianza de Azure](https://gallery.technet.microsoft.com/Overview-of-Azure-c1be3942), donde podrá encontrar la lista más reciente de certificaciones de cumplimiento de SQL Database.

<!--Image references-->









