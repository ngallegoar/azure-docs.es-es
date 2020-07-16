---
title: 'Registros de auditoría: Azure Database for MariaDB'
description: Se describen los registros de auditoría disponibles en Azure Database for MariaDB y los parámetros disponibles para habilitar los niveles de registro.
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.topic: conceptual
ms.date: 6/24/2020
ms.openlocfilehash: 7c9d59eee1e1ce69394301023b108952eaf46790
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85362431"
---
# <a name="audit-logs-in-azure-database-for-mariadb"></a>Registros de auditoría en Azure Database for MariaDB

Los usuarios tienen disponible el registro de auditoría en Azure Database for MariaDB. El registro de auditoría se puede usar para realizar un seguimiento de la actividad a nivel de la base de datos y se usa normalmente para cumplimiento.

## <a name="configure-audit-logging"></a>Configuración del registro de auditoría

>[!IMPORTANT]
> Se recomienda registrar solo los tipos de evento y los usuarios necesarios con fines de auditoría para asegurarse de que el rendimiento del servidor no se ve afectado en gran medida.

De forma predeterminada, el registro de auditoría está deshabilitado. Para habilitarlo, cambie `audit_log_enabled` a Activado.

Otros parámetros que se pueden ajustar son los siguientes:

- `audit_log_events`: controla los eventos que se registrarán. Consulte la tabla a continuación para ver los eventos de auditoría específicos.
- `audit_log_include_users`: usuarios de MariaDB que se incluirán en el registro. El valor predeterminado de este parámetro es estar vacío, lo que incluirá todos los usuarios en el registro. Este tiene una mayor prioridad que `audit_log_exclude_users`. La longitud máxima del parámetro es de 512 caracteres.
- `audit_log_exclude_users`: Los usuarios de MariaDB que se excluirán del registro. Permite un máximo de cuatro usuarios. La longitud máxima del parámetro es de 256 caracteres.

> [!Note]
> `audit_log_include_users` tiene mayor prioridad sobre `audit_log_exclude_users`. Por ejemplo, si `audit_log_include_users` = `demouser` y `audit_log_exclude_users` = `demouser`, el usuario se incluirá en los registros de auditoría porque `audit_log_include_users` tiene mayor prioridad.

| **Evento** | **Descripción** |
|---|---|
| `CONNECTION` | - Iniciación de la conexión (correcta o incorrecta) <br> - Reautenticación de usuario con usuario/contraseña diferente durante la sesión <br> - Terminación de la conexión |
| `DML_SELECT`| Consultas SELECT |
| `DML_NONSELECT` | Consultas INSERT/DELETE/UPDATE |
| `DML` | DML = DML_SELECT + DML_NONSELECT |
| `DDL` | Consultas como "DROP DATABASE" |
| `DCL` | Consultas como "GRANT PERMISSION" |
| `ADMIN` | Consultas como "SHOW STATUS" |
| `GENERAL` | Todos en DML_SELECT, DML_NONSELECT, DML, DDL, DCL y ADMIN |

## <a name="access-audit-logs"></a>Acceso a registros de auditoría

Los registros de auditoría están integrados en los registros de diagnóstico de Azure Monitor. Una vez que haya habilitado los registros de auditoría en el servidor de MariaDB, puede emitirlos a los registros de Azure Monitor, Event Hubs o Azure Storage. Para obtener más información sobre cómo habilitar los registros de diagnóstico en Azure Portal, consulte el [artículo sobre registros de auditoría en el portal](howto-configure-audit-logs-portal.md#set-up-diagnostic-logs).

## <a name="diagnostic-logs-schemas"></a>Esquemas de registros de diagnóstico

En las secciones siguientes se describe lo que generan los registros de auditoría de MariaDB en función del tipo de evento. En función del método de salida que elija, pueden variar los campos incluidos y el orden en el que aparecen.

### <a name="connection"></a>Conexión

| **Propiedad** | **Descripción** |
|---|---|
| `TenantId` | El identificador de inquilino |
| `SourceSystem` | `Azure` |
| `TimeGenerated [UTC]` | Marca de tiempo de cuando se grabó el registro en UTC |
| `Type` | Tipo del registro. Siempre `AzureDiagnostics` |
| `SubscriptionId` | GUID de la suscripción a la que pertenece el servidor |
| `ResourceGroup` | Nombre del grupo de recursos al que pertenece el servidor |
| `ResourceProvider` | Nombre del proveedor de recursos Siempre `MICROSOFT.DBFORMARIADB` |
| `ResourceType` | `Servers` |
| `ResourceId` | URI de recurso |
| `Resource` | Nombre del servidor |
| `Category` | `MySqlAuditLogs` |
| `OperationName` | `LogEvent` |
| `event_class_s` | `connection_log` |
| `event_subclass_s` | `CONNECT`, `DISCONNECT` |
| `connection_id_d` | Identificador único de conexión generado por MariaDB |
| `host_s` | En blanco |
| `ip_s` | Dirección IP del cliente que se conecta a MariaDB |
| `user_s` | Nombre del usuario que ejecuta la consulta |
| `db_s` | Nombre de la base de datos a la que se conecta |
| `\_ResourceId` | URI de recurso |

### <a name="general"></a>General

El esquema siguiente se aplica a los tipos de evento GENERAL, DML_SELECT, DML_NONSELECT, DML, DDL, DCL y ADMIN.

> [!NOTE]
> Para `sql_text`, el registro se truncará si supera los 2048 caracteres.

| **Propiedad** | **Descripción** |
|---|---|
| `TenantId` | El identificador de inquilino |
| `SourceSystem` | `Azure` |
| `TimeGenerated [UTC]` | Marca de tiempo de cuando se grabó el registro en UTC |
| `Type` | Tipo del registro. Siempre `AzureDiagnostics` |
| `SubscriptionId` | GUID de la suscripción a la que pertenece el servidor |
| `ResourceGroup` | Nombre del grupo de recursos al que pertenece el servidor |
| `ResourceProvider` | Nombre del proveedor de recursos Siempre `MICROSOFT.DBFORMARIADB` |
| `ResourceType` | `Servers` |
| `ResourceId` | URI de recurso |
| `Resource` | Nombre del servidor |
| `Category` | `MySqlAuditLogs` |
| `OperationName` | `LogEvent` |
| `LogicalServerName_s` | Nombre del servidor |
| `event_class_s` | `general_log` |
| `event_subclass_s` | `LOG`, `ERROR`, `RESULT` |
| `event_time` | Segundos a partir del inicio de la consulta en formato de marca de tiempo de UNIX |
| `error_code_d` | Código de error si la consulta no es correcta. `0` significa que no hay error |
| `thread_id_d` | Id. del subproceso que ejecutó la consulta |
| `host_s` | En blanco |
| `ip_s` | Dirección IP del cliente que se conecta a MariaDB |
| `user_s` | Nombre del usuario que ejecuta la consulta |
| `sql_text_s` | Texto de la consulta completa |
| `\_ResourceId` | URI de recurso |

## <a name="analyze-logs-in-azure-monitor-logs"></a>Análisis de registros en los registros de Azure Monitor

Una vez que los registros de auditoría se canalizan a los registros de Azure Monitor a través de registros de diagnóstico, puede realizar un análisis en mayor profundidad de sus eventos auditados. A continuación encontrará algunas consultas de ejemplo que le ayudarán a ponerse en marcha. Asegúrese de que actualizar los datos siguientes con el nombre del servidor.

- Enumeración de los eventos GENERALES en un servidor determinado

    ```kusto
    AzureDiagnostics
    | where LogicalServerName_s == '<your server name>'
    | where Category == 'MySqlAuditLogs' and event_class_s == "general_log"
    | project TimeGenerated, LogicalServerName_s, event_class_s, event_subclass_s, event_time_t, user_s , ip_s , sql_text_s 
    | order by TimeGenerated asc nulls last 
    ```

- Enumeración de los eventos de CONEXIÓN en un servidor determinado

    ```kusto
    AzureDiagnostics
    | where LogicalServerName_s == '<your server name>'
    | where Category == 'MySqlAuditLogs' and event_class_s == "connection_log"
    | project TimeGenerated, LogicalServerName_s, event_class_s, event_subclass_s, event_time_t, user_s , ip_s , sql_text_s 
    | order by TimeGenerated asc nulls last
    ```

- Resumen de los eventos auditados en un servidor determinado

    ```kusto
    AzureDiagnostics
    | where LogicalServerName_s == '<your server name>'
    | where Category == 'MySqlAuditLogs'
    | project TimeGenerated, LogicalServerName_s, event_class_s, event_subclass_s, event_time_t, user_s , ip_s , sql_text_s 
    | summarize count() by event_class_s, event_subclass_s, user_s, ip_s
    ```

- Representación de una distribución de tipo de evento de auditoría en un servidor concreto

    ```kusto
    AzureDiagnostics
    | where LogicalServerName_s == '<your server name>'
    | where Category == 'MySqlAuditLogs'
    | project TimeGenerated, LogicalServerName_s, event_class_s, event_subclass_s, event_time_t, user_s , ip_s , sql_text_s 
    | summarize count() by LogicalServerName_s, bin(TimeGenerated, 5m)
    | render timechart 
    ```

- Enumeración de los eventos auditados en todos los servidores de MariaDB con registros de diagnóstico habilitados para los registros de auditoría

    ```kusto
    AzureDiagnostics
    | where Category == 'MySqlAuditLogs'
    | project TimeGenerated, LogicalServerName_s, event_class_s, event_subclass_s, event_time_t, user_s , ip_s , sql_text_s 
    | order by TimeGenerated asc nulls last
    ``` 

## <a name="next-steps"></a>Pasos siguientes

- [Procedimientos para configurar los registros de auditoría en Azure Portal](howto-configure-audit-logs-portal.md)