---
title: Formato del registro de auditoría de SQL Database
description: Comprenda cómo se estructuran los registros de auditoría de Azure SQL Database.
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.topic: conceptual
author: DavidTrigano
ms.author: datrigan
ms.reviewer: vanto
ms.custom: sqldbrb=1
ms.date: 06/03/2020
ms.openlocfilehash: 17d985681ab7a547bf715b1f8bb8d37cbf7ab662
ms.sourcegitcommit: 845a55e6c391c79d2c1585ac1625ea7dc953ea89
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2020
ms.locfileid: "85954118"
---
# <a name="sql-database-audit-log-format"></a>Formato del registro de auditoría de SQL Database

[!INCLUDE[appliesto-sqldb-asa](../includes/appliesto-sqldb-sqlmi-asa.md)]

La [auditoría de Azure SQL Database](auditing-overview.md) realiza el seguimiento de eventos de base de datos y los escribe en una auditoría de registro en la cuenta de Azure Storage, o los envía al centro de eventos o a Log Analytics para el análisis y el procesamiento de bajada.

## <a name="naming-conventions"></a>Convenciones de nomenclatura

### <a name="blob-audit"></a>Auditoría de blobs

Los registros de auditoría almacenados en Azure Blob Storage se almacenan en un contenedor denominado `sqldbauditlogs` en la cuenta de almacenamiento de Azure. La jerarquía de directorios dentro del contenedor tiene el formato `<ServerName>/<DatabaseName>/<AuditName>/<Date>/`. El formato de nombre de archivo del blob es `<CreationTime>_<FileNumberInSession>.xel`, donde `CreationTime` está en formato UTC `hh_mm_ss_ms` y `FileNumberInSession` es un índice en ejecución en el caso de intervalos de registros de sesión en varios archivos de blob.

Por ejemplo, para la base de datos `Database1` en `Server1`, la siguiente es una posible ruta de acceso válida:

`Server1/Database1/SqlDbAuditing_ServerAudit_NoRetention/2019-02-03/12_23_30_794_0.xel`

Los registros de auditoría de las [réplicas de solo lectura](read-scale-out.md) se almacenan en el mismo contenedor. La jerarquía de directorios dentro del contenedor tiene el formato `<ServerName>/<DatabaseName>/<AuditName>/<Date>/RO/`. El nombre de archivo del blob comparte el mismo formato. Los registros de auditoría de las réplicas de solo lectura se almacenan en el mismo contenedor.


### <a name="event-hub"></a>Centro de eventos

Los eventos de auditoría se escriben en el espacio de nombres y en el centro de eventos que se definió durante la configuración de auditoría. Se capturan en el cuerpo de los eventos de [Apache Avro](https://avro.apache.org/) y se almacenan mediante el formato JSON con codificación UTF-8. Para leer los registros de auditoría, puede usar [Avro Tools](../../event-hubs/event-hubs-capture-overview.md#use-avro-tools) o herramientas similares que procesen este formato.

### <a name="log-analytics"></a>Log Analytics

Los eventos de auditoría se escriben en el área de trabajo de Log Analytics definida durante la configuración de auditoría, en la tabla `AzureDiagnostics` con la categoría `SQLSecurityAuditEvents`. Para información útil adicional sobre los comandos y el lenguaje de búsqueda de Log Analytics, consulte la [referencia de búsqueda de Log Analytics](../../azure-monitor/log-query/log-query-overview.md).

## <a name="audit-log-fields"></a><a id="subheading-1"></a>Campos del registro de auditoría

| Nombre (blob) | Nombre (Event Hubs/Log Analytics) | Descripción | Tipo de blob | Tipo de Event Hubs/Log Analytics |
|-------------|---------------------------------|-------------|-----------|-------------------------------|
| action_id | action_id_s | Identificador de la acción. | varchar(4) | string |
| action_name | action_name_s | Nombre de la acción | N/D | string |
| additional_information | additional_information_s | Cualquier información adicional sobre el evento, almacenada en formato XML | nvarchar(4000) | string |
| affected_rows | affected_rows_d | Número de filas afectadas por la consulta | bigint | int |
| application_name | application_name_s| Nombre de la aplicación cliente | nvarchar(128) | string |
| audit_schema_version | audit_schema_version_d | Siempre 1 | int | int |
| class_type | class_type_s | Tipo de entidad auditable en la que se produce la auditoría | varchar(2) | string |
| class_type_desc | class_type_description_s | Descripción de la entidad auditable en la que se produce la auditoría | N/D | string |
| client_ip | client_ip_s | IP de origen de la aplicación cliente | nvarchar(128) | string |
| connection_id | N/D | Identificador de la conexión en el servidor | GUID | N/D |
| data_sensitivity_information | data_sensitivity_information_s | Tipos de información y etiquetas de confidencialidad devueltas por la consulta auditada, basados en las columnas clasificadas en la base de datos. Obtenga más información sobre la [clasificación y detección de datos de Azure SQL Database](data-discovery-and-classification-overview.md). | nvarchar(4000) | string |
| database_name | database_name_s | Contexto de base de datos en el que se produjo la acción | sysname | string |
| database_principal_id | database_principal_id_d | Id. del contexto de usuario de la base de datos donde se realiza la acción. | int | int |
| database_principal_name | database_principal_name_s | Nombre del contexto de usuario de la base de datos donde se realiza la acción. | sysname | string |
| duration_milliseconds | duration_milliseconds_d | Duración de la ejecución de la consulta, en milisegundos. | bigint | int |
| event_time | event_time_t | Fecha y hora en que se activó la acción auditable. | datetime2 | datetime |
| host_name | N/D | Nombre de host del cliente. | string | N/D |
| is_column_permission | is_column_permission_s | Marca que indica si se trata de un permiso de nivel de columna. 1 = true, 0 = false | bit | string |
| N/D | is_server_level_audit_s | Marca que indica si esta auditoría está en el nivel de servidor. | N/D | string |
| object_ id | object_id_d | Identificador de la entidad en la que se produjo la auditoría. Incluye objetos de servidor, bases de datos, objetos de base de datos y objetos de esquema. 0 si la entidad es el propio servidor o si la auditoría no se realiza en un nivel de objeto. | int | int |
| object_name | object_name_s | Nombre de la entidad en la que se produjo la auditoría. Incluye objetos de servidor, bases de datos, objetos de base de datos y objetos de esquema. 0 si la entidad es el propio servidor o si la auditoría no se realiza en un nivel de objeto. | sysname | string |
| permission_bitmask | permission_bitmask_s | Cuando es aplicable, muestra los permisos concedidos, denegados, o revocados. | varbinary(16) | string |
| response_rows | response_rows_d | Número de filas devueltas en el conjunto de resultados. | bigint | int |
| schema_name | schema_name_s | Contexto de esquema en el que se produjo la acción. NULL para las auditorías que se producen fuera de un esquema. | sysname | string |
| N/D | securable_class_type_s | Objeto protegible que se asigna al valor de class_type que se está auditando. | N/D | string |
| sequence_group_id | sequence_group_id_g | Identificador único | varbinary | GUID |
| sequence_number | sequence_number_d | Realiza un seguimiento de la secuencia de registros en un único registro de auditoría demasiado grande para caber en el búfer de escritura para las auditorías. | int | int |
| server_instance_name | server_instance_name_s | Nombre de la instancia del servidor donde se produjo la auditoría. | sysname | string |
| server_principal_id | server_principal_id_d | Identificador del contexto de inicio de sesión donde se realiza la acción. | int | int |
| server_principal_name | server_principal_name_s | Inicio de sesión actual. | sysname | string |
| server_principal_sid | server_principal_sid_s | Id. de seguridad de inicio de sesión actual. | varbinary | string |
| session_id | session_id_d | Identificador de la sesión en la que se produjo el evento. | SMALLINT | int |
| session_server_principal_name | session_server_principal_name_s | Entidad de seguridad del servidor para la sesión. | sysname | string |
| statement | statement_s | Instrucción T-SQL ejecutada (si existe). | nvarchar(4000) | string |
| succeeded | succeeded_s | Indica si la acción que desencadenó el evento se realizó correctamente. Para los eventos que no son de inicio de sesión ni por lotes, solo notifica si la comprobación del permiso se realizó correctamente o con errores, no la operación. 1 = correcto, 0 = error | bit | string |
| target_database_principal_id | target_database_principal_id_d | Entidad de seguridad de la base de datos donde se realizó la operación GRANT/DENY/REVOKE. 0 si no es aplicable. | int | int |
| target_database_principal_name | target_database_principal_name_s | Usuario de destino de la acción. NULL si no es aplicable. | string | string |
| target_server_principal_id | target_server_principal_id_d | Entidad de seguridad del servidor donde se realizó la operación GRANT/DENY/REVOKE. Devuelve 0 si no es aplicable. | int | int |
| target_server_principal_name | target_server_principal_name_s | Inicio de sesión de destino de la acción. NULL si no es aplicable. | sysname | string |
| target_server_principal_sid | target_server_principal_sid_s | Id. de seguridad del inicio de sesión de destino. NULL si no es aplicable. | varbinary | string |
| transaction_id | transaction_id_d | Solo SQL Server (a partir de 2016): 0 para Azure SQL Database | bigint | int |
| user_defined_event_id | user_defined_event_id_d | Id. de evento definido por el usuario que se pasa como un argumento a sp_audit_write. NULL para eventos del sistema (valor predeterminado) y distinto de cero para eventos definidos por el usuario. Para más información, consulte [sp_audit_write (Transact-SQL)](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-audit-write-transact-sql) | SMALLINT | int |
| user_defined_information | user_defined_information_s | Información definida por el usuario pasada como un argumento a sp_audit_write. NULL para eventos del sistema (valor predeterminado) y distinto de cero para eventos definidos por el usuario. Para más información, consulte [sp_audit_write (Transact-SQL)](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-audit-write-transact-sql) | nvarchar(4000) | string |

## <a name="next-steps"></a>Pasos siguientes

Obtenga más información sobre la [auditoría de Azure SQL Database](auditing-overview.md).
