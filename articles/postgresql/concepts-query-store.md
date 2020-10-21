---
title: 'Almacén de consultas en Azure Database for PostgreSQL: servidor único'
description: En este artículo se describe la característica Almacén de consultas en Azure Database for PostgreSQL con un único servidor.
author: sunilagarwal
ms.author: sunila
ms.service: postgresql
ms.topic: conceptual
ms.date: 07/01/2020
ms.openlocfilehash: 7b6c8faafac34ada664ddfadebf8d71a16c73fa7
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "91710539"
---
# <a name="monitor-performance-with-the-query-store"></a>Supervisión del rendimiento con el Almacén de consultas

**Se aplica a:** Azure Database for PostgreSQL: versiones de servidor único 9.6 y posteriores

La característica Almacén de consultas de Azure Database for PostgreSQL proporciona una manera de realizar un seguimiento del rendimiento de las consultas a lo largo del tiempo. El Almacén de consultas simplifica la solución de problemas de rendimiento al ayudar a encontrar rápidamente las consultas que tardan más en ejecutarse y consumen más recursos. El Almacén de consultas captura automáticamente un historial de consultas y estadísticas de tiempo de ejecución y lo conserva para su revisión. Separa los datos por ventanas de tiempo para que pueda ver patrones de uso de la base de datos. Los datos de todos los usuarios, las bases de datos y las consultas se almacenan en una base de datos denominada **azure_sys** en la instancia de Azure Database for PostgreSQL.

> [!IMPORTANT]
> No modifique la base de datos **azure_sys** ni sus esquemas. Si lo hace, impedirá que el Almacén de consultas y las características de rendimiento relacionados funcionen correctamente.

## <a name="enabling-query-store"></a>Habilitación del Almacén de consultas
El Almacén de consultas es una característica opcional, por lo que no está activo de forma predeterminada en un servidor. El almacén se habilita o deshabilita globalmente para todas las bases de datos en un servidor determinado y no se puede activar o desactivar por base de datos.

### <a name="enable-query-store-using-the-azure-portal"></a>Habilitación del Almacén de consultas mediante Azure Portal
1. Inicie sesión en Azure Portal y seleccione el servidor de Azure Database for PostgreSQL.
2. Seleccione **Parámetros del servidor** en la sección **Configuración** del menú.
3. Busque el parámetro `pg_qs.query_capture_mode`.
4. Establezca el valor en `TOP` y haga clic en **Guardar**.

Para habilitar las estadísticas de espera en el Almacén de consultas: 
1. Busque el parámetro `pgms_wait_sampling.query_capture_mode`.
1. Establezca el valor en `ALL` y haga clic en **Guardar**.


También puede establecer estos parámetros con la CLI de Azure.
```azurecli-interactive
az postgres server configuration set --name pg_qs.query_capture_mode --resource-group myresourcegroup --server mydemoserver --value TOP
az postgres server configuration set --name pgms_wait_sampling.query_capture_mode --resource-group myresourcegroup --server mydemoserver --value ALL
```

Espere hasta 20 minutos para que el primer lote de datos se conserve en la base de datos azure_sys.

## <a name="information-in-query-store"></a>Información del Almacén de consultas
El Almacén de consultas tiene dos almacenes:
- Un almacén de estadísticas de ejecución para conservar la información de estadísticas de ejecución de consultas.
- Un almacén de estadísticas de espera para conservar la información de estadísticas de espera.

Algunos escenarios habituales para usar el Almacén de consultas son:
- Determinar el número de veces que se ha ejecutado una consulta en un período determinado
- Comparar el tiempo de ejecución medio de una consulta a través de ventanas de tiempo para ver diferenciales de gran tamaño
- Identificar las consultas de ejecución más largas en las últimas X horas
- Identificar las N principales consultas que esperan recursos
- Descripción de la naturaleza de espera de una determinada consulta

Para minimizar el uso de espacio, se agregan las estadísticas de ejecución en tiempo de ejecución en el almacén de estadísticas de ejecución en un período fijo y configurable. La información de estos almacenes está visible consultando las vistas del almacén de consultas.

## <a name="access-query-store-information"></a>Acceso a información del Almacén de consultas

Los datos del Almacén de consultas se almacenan en la base de datos azure_sys del servidor de Postgres. 

La consulta siguiente devuelve información sobre las consultas en el Almacén de consultas:
```sql
SELECT * FROM query_store.qs_view; 
``` 

O esta consulta para estadísticas de espera:
```sql
SELECT * FROM query_store.pgms_wait_sampling_view;
```

## <a name="finding-wait-queries"></a>Búsqueda de consultas de espera
Los tipos de evento de espera combinan eventos de espera diferentes en ciclos por similitud. El Almacén de consultas proporciona el tipo de evento de espera, el nombre del evento de espera específico y la consulta en cuestión. Poder correlacionar esta información de espera con las estadísticas de tiempo de ejecución de consultas significa que puede mejorar la comprensión de lo que contribuye a las características de rendimiento de consulta.

Estos son algunos ejemplos de cómo puede obtener más información sobre la carga de trabajo con las estadísticas de espera del Almacén de consultas:

| **Observación** | **Acción** |
|---|---|
|Largas esperas de bloqueo | Compruebe los textos de consulta para las consultas afectadas e identifique las entidades de destino. Busque en el Almacén de consultas otras consultas que modifiquen la misma entidad, que se ejecuta con frecuencia o tiene una gran duración. Tras identificar estas consultas, considere la posibilidad de cambiar la lógica de aplicación para mejorar la simultaneidad o usar un nivel de aislamiento menos restrictivo.|
| Largas esperas de E/S de búfer | Encuentre las consultas con un gran número de lecturas físicas en el Almacén de consultas. Si coinciden con las consultas con largas esperas de E/S, considere la posibilidad de introducir un índice en la entidad subyacente para llevar a cabo búsquedas en lugar de exámenes. Esto podría minimizar la sobrecarga de E/S de las consultas. Compruebe las **Recomendaciones de rendimiento** para el servidor en el portal para ver si hay recomendaciones de índices para este servidor que optimizarían las consultas.|
| Largas esperas de memoria | Encuentre las consultas que más memoria consumen en el Almacén de consultas. Estas consultas probablemente retrasan el progreso de las consultas afectadas. Compruebe las **Recomendaciones de rendimiento** para el servidor en el portal para ver si hay recomendaciones de índices que optimizarían estas consultas.|

## <a name="configuration-options"></a>Opciones de configuración
Cuando el Almacén de consultas está habilitado, guarda los datos en ventanas de agregación de 15 minutos, hasta 500 consultas diferentes en cada ventana. 

Las siguientes opciones están disponibles para configurar los parámetros del Almacén de consultas.

| **Parámetro** | **Descripción** | **Valor predeterminado** | **Range**|
|---|---|---|---|
| pg_qs.query_capture_mode | Establece las instrucciones de las que se realiza el seguimiento. | None | none, top, all |
| pg_qs.max_query_text_length | Establece la longitud máxima de consulta que se puede guardar. Las consultas más largas se truncarán. | 6000 | 100 - 10K |
| pg_qs.retention_period_in_days | Establece el período de retención. | 7 | 1 - 30 |
| pg_qs.track_utility | Establece si se realiza un seguimiento de los comandos de la utilidad. | en | on, off |

Las siguientes opciones afectan específicamente a las estadísticas de espera.

| **Parámetro** | **Descripción** | **Valor predeterminado** | **Range**|
|---|---|---|---|
| pgms_wait_sampling.query_capture_mode | Establece de qué instrucciones se realiza un seguimiento para las estadísticas de espera. | None | none, all|
| Pgms_wait_sampling.history_period | Establece la frecuencia, en milisegundos, con la que se muestrean los eventos de espera. | 100 | 1-600000 |

> [!NOTE] 
> **pg_qs.query_capture_mode** reemplaza a **pgms_wait_sampling.query_capture_mode**. Si el valor de pg_qs.query_capture_mode es NONE, la configuración de pgms_wait_sampling.query_capture_mode no tiene ningún efecto.


Use [Azure Portal](howto-configure-server-parameters-using-portal.md) o la [CLI de Azure](howto-configure-server-parameters-using-cli.md) para obtener o establecer otro valor para un parámetro.

## <a name="views-and-functions"></a>Funciones y vistas
Vea y administre el Almacén de consultas mediante las siguientes vistas y funciones. Cualquier usuario en el rol público PostgreSQL puede utilizar estas vistas para ver los datos en el Almacén de consultas. Estas vistas solo están disponibles en la base de datos **azure_sys**.

Las consultas se normalizan examinando su estructura después de quitar los literales y constantes. Si dos consultas son idénticas salvo por los valores literales, tienen el mismo hash.

### <a name="query_storeqs_view"></a>query_store.qs_view
Esta vista devuelve todos los datos del Almacén de consultas. Hay una fila por cada identificador de base de datos, de usuario y de consulta diferentes. 

|**Nombre**   |**Tipo** | **Referencias**  | **Descripción**|
|---|---|---|---|
|runtime_stats_entry_id |bigint | | Id. de la tabla runtime_stats_entries|
|user_id    |oid    |pg_authid.oid  |OID del usuario que ha ejecutado la instrucción.|
|db_id  |oid    |pg_database.oid    |OID de la base de datos en la que se ha ejecutado la instrucción.|
|query_id   |bigint  || Código hash interno, calculado a partir del árbol de análisis de la instrucción.|
|query_sql_text |Varchar(10000)  || Texto de una instrucción representativa. Las consultas diferentes con la misma estructura se agrupan; este texto es el texto para la primera consulta del clúster.|
|plan_id    |bigint |   |Identificador del plan correspondiente a esta consulta, no está disponible todavía.|
|start_time |timestamp  ||  Las consultas se agregan por ciclos: el intervalo de tiempo de un ciclo es de 15 minutos de forma predeterminada. Se trata de la hora de inicio correspondiente al ciclo para esta entrada.|
|end_time   |timestamp  ||  Hora de finalización correspondiente al ciclo para esta entrada.|
|calls  |bigint  || Número de veces que se ejecuta la consulta.|
|total_time |double precision   ||  Tiempo total de ejecución de las consultas, en milisegundos.|
|min_time   |double precision   ||  Tiempo mínimo de ejecución de las consultas, en milisegundos.|
|max_time   |double precision   ||  Tiempo máximo de ejecución de las consultas, en milisegundos.|
|mean_time  |double precision   ||  Tiempo medio de ejecución de las consultas, en milisegundos.|
|stddev_time|   double precision    ||  Desviación estándar del tiempo de ejecución de las consultas, en milisegundos. |
|rows   |bigint ||  Número total de filas recuperadas o afectadas por la instrucción.|
|shared_blks_hit|   bigint  ||  Número total de aciertos de caché de bloque compartidos por la instrucción.|
|shared_blks_read|  bigint  ||  Número total de bloques compartidos leídos por la instrucción.|
|shared_blks_dirtied|   bigint   || Número total de bloques compartidos desfasados por la instrucción. |
|shared_blks_written|   bigint  ||  Número total de bloques compartidos escritos por la instrucción.|
|local_blks_hit|    bigint ||   Número total de aciertos de caché de bloque locales por la instrucción.|
|local_blks_read|   bigint   || Número total de bloques locales leídos por la instrucción.|
|local_blks_dirtied|    bigint  ||  Número total de bloques locales desfasados por la instrucción.|
|local_blks_written|    bigint  ||  Número total de bloques locales escritos por la instrucción.|
|temp_blks_read |bigint  || Número total de bloques temporales leídos por la instrucción.|
|temp_blks_written| bigint   || Número total de bloques temporales escritos por la instrucción.|
|blk_read_time  |double precision    || Tiempo total que la instrucción dedica a leer los bloques, en milisegundos (si está habilitado track_io_timing; de lo contrario, cero).|
|blk_write_time |double precision    || Tiempo total que la instrucción dedica a escribir los bloques, en milisegundos (si está habilitado track_io_timing; de lo contrario, cero).|
    
### <a name="query_storequery_texts_view"></a>query_store.query_texts_view
Esta vista devuelve datos de texto de consulta en el Almacén de consultas. Hay una fila para cada argumento de consulta diferente.

|**Nombre**|  **Tipo**|   **Descripción**|
|---|---|---|
|query_text_id  |bigint     |Identificador de la tabla query_texts.|
|query_sql_text |Varchar(10000)     |Texto de una instrucción representativa. Las consultas diferentes con la misma estructura se agrupan; este texto es el texto para la primera consulta del clúster.|

### <a name="query_storepgms_wait_sampling_view"></a>query_store.pgms_wait_sampling_view
Esta vista devuelve datos de eventos de espera en el Almacén de consultas. Hay una fila por cada identificador de base de datos, identificador de usuario, identificador de consulta y evento únicos.

|**Nombre**|  **Tipo**|   **Referencias**| **Descripción**|
|---|---|---|---|
|user_id    |oid    |pg_authid.oid  |OID del usuario que ha ejecutado la instrucción.|
|db_id  |oid    |pg_database.oid    |OID de la base de datos en la que se ha ejecutado la instrucción.|
|query_id   |bigint     ||Código hash interno, calculado a partir del árbol de análisis de la instrucción.|
|event_type |text       ||Tipo de evento que está esperando el back-end.|
|event  |text       ||Nombre del evento de espera si el back-end está esperando.|
|calls  |Entero        ||Número del mismo evento capturado.|


### <a name="functions"></a>Functions
Query_store.qs_reset() devuelve void.

`qs_reset` descarta todas las estadísticas recopiladas hasta ahora por el Almacén de consultas. Esta función solo se puede ejecutar mediante el rol de administrador del servidor.

Query_store.staging_data_reset() devuelve void.

`staging_data_reset` descarta todas las estadística recopiladas en la memoria por el Almacén de consultas (es decir, los datos en la memoria que no se ha vaciado aún a la base de datos). Esta función solo se puede ejecutar mediante el rol de administrador del servidor.


## <a name="azure-monitor"></a>Azure Monitor
Azure Database for PostgreSQL se integra con la [configuración de diagnóstico de Azure Monitor](../azure-monitor/platform/diagnostic-settings.md). Esta configuración permite enviar los registros de Postgres en formato JSON a los [registros de Azure Monitor](../azure-monitor/log-query/log-query-overview.md) para llevar a cabo análisis y creación de alertas, a Event Hubs para streaming y a Azure Storage para el archivado.

>[!IMPORTANT]
> Esta característica de diagnóstico solo está disponible en los planes de tarifa de uso general y optimizados para memoria.

### <a name="configure-diagnostic-settings"></a>Configuración de valores de diagnóstico
Puede habilitar la configuración de diagnóstico para el servidor de Postgres mediante Azure Portal, la CLI, la API REST y PowerShell. Las categorías de registro que se configuran son **QueryStoreRuntimeStatistics** y **QueryStoreWaitStatistics**. 

Para habilitar los registros de recursos mediante Azure Portal:

1. En el portal, vaya a Configuración de diagnóstico en el menú de navegación del servidor de Postgres.
2. Seleccione Agregar configuración de diagnóstico.
3. Asigne un nombre a esta configuración.
4. Seleccione el punto de conexión preferido (cuenta de almacenamiento, centro de eventos, análisis de registros).
5. Seleccione los tipos de registro **QueryStoreRuntimeStatistics** y **QueryStoreWaitStatistics**.
6. Guarde la configuración.

Para habilitar esta configuración mediante PowerShell, la CLI o la API REST, consulte el [artículo sobre la configuración de diagnóstico](../azure-monitor/platform/diagnostic-settings.md).

### <a name="json-log-format"></a>Formato de registros JSON
En las tablas siguientes se describen los campos de los dos tipos de registro. En función del punto de conexión de salida que elija, pueden variar los campos incluidos y el orden en el que aparecen.

#### <a name="querystoreruntimestatistics"></a>QueryStoreRuntimeStatistics
|**Campo** | **Descripción** |
|---|---|
| TimeGenerated [UTC] | Marca de tiempo de cuando se grabó el registro en UTC |
| ResourceId | URI de recurso de Azure del servidor de Postgres |
| Category | `QueryStoreRuntimeStatistics` |
| OperationName | `QueryStoreRuntimeStatisticsEvent` |
| LogicalServerName_s | Nombre del servidor de Postgres | 
| runtime_stats_entry_id_s | Id. de la tabla runtime_stats_entries |
| user_id_s | OID del usuario que ha ejecutado la instrucción. |
| db_id_s | OID de la base de datos en la que se ha ejecutado la instrucción. |
| query_id_s | Código hash interno, calculado a partir del árbol de análisis de la instrucción. |
| end_time_s | Hora de finalización correspondiente al ciclo para esta entrada. |
| calls_s | Número de veces que se ejecuta la consulta. |
| total_time_s | Tiempo total de ejecución de las consultas, en milisegundos. |
| min_time_s | Tiempo mínimo de ejecución de las consultas, en milisegundos. |
| max_time_s | Tiempo máximo de ejecución de las consultas, en milisegundos. |
| mean_time_s | Tiempo medio de ejecución de las consultas, en milisegundos. |
| ResourceGroup | El grupo de recursos | 
| SubscriptionId | Su Id. de suscripción |
| ResourceProvider | `Microsoft.DBForPostgreSQL` | 
| Recurso | Nombre del servidor de Postgres |
| ResourceType | `Servers` | 


#### <a name="querystorewaitstatistics"></a>QueryStoreWaitStatistics
|**Campo** | **Descripción** |
|---|---|
| TimeGenerated [UTC] | Marca de tiempo de cuando se grabó el registro en UTC |
| ResourceId | URI de recurso de Azure del servidor de Postgres |
| Category | `QueryStoreWaitStatistics` |
| OperationName | `QueryStoreWaitEvent` |
| user_id_s | OID del usuario que ha ejecutado la instrucción. |
| db_id_s | OID de la base de datos en la que se ha ejecutado la instrucción. |
| query_id_s | Código hash interno de la consulta |
| calls_s | Número del mismo evento capturado. |
| event_type_s | Tipo de evento que está esperando el back-end. |
| event_s | Nombre del evento de espera si el back-end está esperando |
| start_time_t | Hora de inicio del evento |
| end_time_s | Hora de finalización del evento | 
| LogicalServerName_s | Nombre del servidor de Postgres | 
| ResourceGroup | El grupo de recursos | 
| SubscriptionId | Su Id. de suscripción |
| ResourceProvider | `Microsoft.DBForPostgreSQL` | 
| Recurso | Nombre del servidor de Postgres |
| ResourceType | `Servers` | 

## <a name="limitations-and-known-issues"></a>Limitaciones y problemas conocidos
- Si un servidor de PostgreSQL tiene el parámetro default_transaction_read_only activado, el Almacén de consultas no puede capturar datos.
- Se puede interrumpir la funcionalidad de Almacén de consultas si encuentra consultas largas de Unicode (>= 6000 bytes).
- [Las réplicas de lectura](concepts-read-replicas.md) replican los datos del Almacén de consultas del servidor principal. Esto significa que el Almacén de consultas de una réplica de lectura no proporciona estadísticas sobre las consultas que se ejecutan en la réplica de lectura.


## <a name="next-steps"></a>Pasos siguientes
- Obtenga más información sobre los [escenarios en los que el Almacén de consultas puede ser especialmente útil](concepts-query-store-scenarios.md).
- Más información sobre los [procedimientos recomendados para el uso del Almacén de consultas](concepts-query-store-best-practices.md)
