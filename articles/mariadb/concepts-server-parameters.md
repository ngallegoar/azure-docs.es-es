---
title: 'Parámetros del servidor: Azure Database for MariaDB'
description: En este tema se proporcionan instrucciones para configurar parámetros del servidor en Azure Database for MariaDB.
author: savjani
ms.author: pariks
ms.service: mariadb
ms.topic: conceptual
ms.date: 6/25/2020
ms.openlocfilehash: 4432178d5908d4360cda05a62b62d05687be4235
ms.sourcegitcommit: 6ab718e1be2767db2605eeebe974ee9e2c07022b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/12/2020
ms.locfileid: "94541137"
---
# <a name="server-parameters-in-azure-database-for-mariadb"></a>Parámetros del servidor en Azure Database for MariaDB

En este artículo se incluyen consideraciones e instrucciones para configurar parámetros del servidor en Azure Database for MariaDB.

## <a name="what-are-server-parameters"></a>¿Cuáles son los parámetros del servidor? 

El motor MariaDB proporciona muchas variables o parámetros de servidor diferentes que se pueden usar para configurar y ajustar el comportamiento del motor. Algunos parámetros se pueden establecer de forma dinámica en tiempo de ejecución, mientras que otros son "estáticos", requiriendo un reinicio del servidor para su aplicación.

Azure Database for MariaDB expone la capacidad de cambio del valor de diversos parámetros del servidor MariaDB mediante [Azure Portal](./howto-server-parameters.md), la [CLI de Azure](./howto-configure-server-parameters-cli.md) y [PowerShell](./howto-configure-server-parameters-using-powershell.md) para satisfacer las necesidades de la carga de trabajo.

## <a name="configurable-server-parameters"></a>Parámetros configurables del servidor

La lista de parámetros del servidor admitidos crece constantemente. Use la pestaña Parámetros del servidor de Azure Portal para ver la lista completa y configurar valores de los parámetros del servidor.

Consulte las siguientes secciones para obtener más información sobre los límites de los diferentes parámetros del servidor actualizados con más frecuencia. Los límites vienen determinados por el plan de tarifa y los núcleos virtuales del servidor.

### <a name="log_bin_trust_function_creators"></a>log_bin_trust_function_creators

En Azure Database for MariaDB, siempre están habilitados los registros binarios (p. ej., `log_bin` está establecido en ACTIVADO). En caso de que desee usar desencadenadores, obtendrá un error similar a *you do not have the SUPER privilege and binary logging is enabled (you might want to use the less safe `log_bin_trust_function_creators` variable)* [No tiene el privilegio SUPER y el registro binario está habilitado (es posible que desee usar la variable menos segura)].

El formato de registro binario siempre es **ROW** y todas las conexiones al servidor **ALWAYS** usan un registro binario basado en filas. Con el registro binario basado en filas, no existen problemas de seguridad y el registro binario no se puede interrumpir, de modo que puede establecer [`log_bin_trust_function_creators`](https://mariadb.com/docs/reference/mdb/system-variables/log_bin_trust_function_creators/) en **TRUE** de forma segura.

### <a name="innodb_buffer_pool_size"></a>innodb_buffer_pool_size

Consulte la [documentación de MariaDB](https://mariadb.com/kb/en/innodb-system-variables/#innodb_buffer_pool_size) para más información acerca de este parámetro.

#### <a name="servers-supporting-up-to-4-tb-storage"></a>Servidores que admiten un almacenamiento de hasta 4 TB

|**Plan de tarifa**|**Núcleos virtuales**|**Valor predeterminado (bytes)**|**Valor mínimo (bytes)**|**Valor máximo (bytes)**|
|---|---|---|---|---|
|Básico|1|872415232|134217728|872415232|
|Básico|2|2684354560|134217728|2684354560|
|De uso general|2|3758096384|134217728|3758096384|
|De uso general|4|8053063680|134217728|8053063680|
|De uso general|8|16106127360|134217728|16106127360|
|De uso general|16|32749125632|134217728|32749125632|
|De uso general|32|66035122176|134217728|66035122176|
|De uso general|64|132070244352|134217728|132070244352|
|Memoria optimizada|2|7516192768|134217728|7516192768|
|Memoria optimizada|4|16106127360|134217728|16106127360|
|Memoria optimizada|8|32212254720|134217728|32212254720|
|Memoria optimizada|16|65498251264|134217728|65498251264|
|Memoria optimizada|32|132070244352|134217728|132070244352|

#### <a name="servers-support-up-to-16-tb-storage"></a>Servidores que admiten un almacenamiento de hasta 16 TB

|**Plan de tarifa**|**Núcleos virtuales**|**Valor predeterminado (bytes)**|**Valor mínimo (bytes)**|**Valor máximo (bytes)**|
|---|---|---|---|---|
|Básico|1|872415232|134217728|872415232|
|Básico|2|2684354560|134217728|2684354560|
|De uso general|2|7516192768|134217728|7516192768|
|De uso general|4|16106127360|134217728|16106127360|
|De uso general|8|32212254720|134217728|32212254720|
|De uso general|16|65498251264|134217728|65498251264|
|De uso general|32|132070244352|134217728|132070244352|
|De uso general|64|264140488704|134217728|264140488704|
|Memoria optimizada|2|15032385536|134217728|15032385536|
|Memoria optimizada|4|32212254720|134217728|32212254720|
|Memoria optimizada|8|64424509440|134217728|64424509440|
|Memoria optimizada|16|130996502528|134217728|130996502528|
|Memoria optimizada|32|264140488704|134217728|264140488704|

### <a name="innodb_file_per_table"></a>innodb_file_per_table

> [!NOTE]
> `innodb_file_per_table` solo se puede actualizar en los planes de tarifa de uso general y optimizada para memoria.

MariaDB almacena la tabla InnoDB en distintos espacios de tabla en función de la configuración proporcionada durante la creación de la tabla. El [espacio de tabla del sistema](https://mariadb.com/kb/en/innodb-system-tablespaces/) es el área de almacenamiento para el diccionario de datos de InnoDB. Un [espacio de tabla de archivo por tabla](https://mariadb.com/kb/en/innodb-file-per-table-tablespaces/) contiene datos e índices para una sola tabla de InnoDB y se almacena en el sistema de archivos, en su propio archivo de datos. Este comportamiento se controla mediante el parámetro de servidor `innodb_file_per_table`. Si `innodb_file_per_table` se establece en `OFF`, InnoDB crea tablas en el espacio de tablas del sistema. De lo contrario, InnoDB crea tablas en los espacios de tabla de archivo por tabla.

Azure Database for MariaDB admite **1 TB** como máximo en un solo archivo de datos. Si el tamaño de la base de datos es superior a 1 TB, debería crear la tabla en el espacio de tabla [innodb_file_per_table](https://mariadb.com/kb/en/innodb-system-variables/#innodb_file_per_table). Si tiene una sola tabla de tamaño superior a 1 TB, debería usar la tabla de particiones.

### <a name="join_buffer_size"></a>join_buffer_size

Consulte la [documentación de MariaDB](https://mariadb.com/kb/en/server-system-variables/#join_buffer_size) para más información acerca de este parámetro.

|**Plan de tarifa**|**Núcleos virtuales**|**Valor predeterminado (bytes)**|**Valor mínimo (bytes)**|**Valor máximo (bytes)**|
|---|---|---|---|---|
|Básico|1|No se puede configurar en el nivel Básico|N/D|N/D|
|Básico|2|No se puede configurar en el nivel Básico|N/D|N/D|
|De uso general|2|262 144|128|268435455|
|De uso general|4|262 144|128|536870912|
|De uso general|8|262 144|128|1073741824|
|De uso general|16|262 144|128|2147483648|
|De uso general|32|262 144|128|4294967295|
|De uso general|64|262 144|128|4294967295|
|Memoria optimizada|2|262 144|128|536870912|
|Memoria optimizada|4|262 144|128|1073741824|
|Memoria optimizada|8|262 144|128|2147483648|
|Memoria optimizada|16|262 144|128|4294967295|
|Memoria optimizada|32|262 144|128|4294967295|

### <a name="max_connections"></a>max_connections

|**Plan de tarifa**|**Núcleos virtuales**|**Valor predeterminado**|**Valor mínimo**|**Valor máximo**|
|---|---|---|---|---|
|Básico|1|50|10|50|
|Básico|2|100|10|100|
|De uso general|2|300|10|600|
|De uso general|4|625|10|1250|
|De uso general|8|1250|10|2.500|
|De uso general|16|2.500|10|5000|
|De uso general|32|5000|10|10000|
|De uso general|64|10000|10|20000|
|Memoria optimizada|2|625|10|1250|
|Memoria optimizada|4|1250|10|2.500|
|Memoria optimizada|8|2.500|10|5000|
|Memoria optimizada|16|5000|10|10000|
|Memoria optimizada|32|10000|10|20000|

Si las conexiones superan el límite, puede que reciba el error siguiente:
> ERROR 1040 (08004): Demasiadas conexiones

> [!IMPORTANT]
> Para obtener la mejor experiencia posible, se recomienda usar un agrupador de conexiones, como ProxySQL, para administrar las conexiones de forma eficaz.

La creación de conexiones cliente a MariaDB lleva tiempo y, una vez establecidas, estas conexiones ocupan recursos de bases de datos, incluso cuando están inactivas. La mayoría de las aplicaciones solicitan muchas conexiones de corta duración, y esto es lo que conforma esta situación. El resultado es que hay menos recursos disponibles para la carga de trabajo real, lo que baja el rendimiento. Esto se puede evitar con un agrupador de conexiones, ya que reduce las conexiones inactivas y reutiliza las conexiones existentes. Para más información sobre cómo configurar ProxySQL, consulte nuestra [entrada de blog](https://techcommunity.microsoft.com/t5/azure-database-for-mysql/load-balance-read-replicas-using-proxysql-in-azure-database-for/ba-p/880042).

>[!Note]
>ProxySQL es una herramienta de la comunidad de código abierto. Es compatible con Microsoft dentro de lo posible. Para obtener soporte a la producción con instrucciones autoritativas, puede evaluar y ponerse en contacto con el [servicio de soporte técnico de ProxySQL](https://proxysql.com/services/support/).

### <a name="max_heap_table_size"></a>max_heap_table_size

Consulte la [documentación de MariaDB](https://mariadb.com/kb/en/server-system-variables/#max_heap_table_size) para más información acerca de este parámetro.

|**Plan de tarifa**|**Núcleos virtuales**|**Valor predeterminado (bytes)**|**Valor mínimo (bytes)**|**Valor máximo (bytes)**|
|---|---|---|---|---|
|Básico|1|No se puede configurar en el nivel Básico|N/D|N/D|
|Básico|2|No se puede configurar en el nivel Básico|N/D|N/D|
|De uso general|2|16777216|16384|268435455|
|De uso general|4|16777216|16384|536870912|
|De uso general|8|16777216|16384|1073741824|
|De uso general|16|16777216|16384|2147483648|
|De uso general|32|16777216|16384|4294967295|
|De uso general|64|16777216|16384|4294967295|
|Memoria optimizada|2|16777216|16384|536870912|
|Memoria optimizada|4|16777216|16384|1073741824|
|Memoria optimizada|8|16777216|16384|2147483648|
|Memoria optimizada|16|16777216|16384|4294967295|
|Memoria optimizada|32|16777216|16384|4294967295|

### <a name="query_cache_size"></a>query_cache_size

La caché de consulta está habilitada de forma predeterminada en MariaDB con el parámetro `have_query_cache`. 

Consulte la [documentación de MariaDB](https://mariadb.com/kb/en/server-system-variables/#query_cache_size) para más información acerca de este parámetro.

|**Plan de tarifa**|**Núcleos virtuales**|**Valor predeterminado (bytes)**|**Valor mínimo (bytes)**|**Valor máximo **|
|---|---|---|---|---|
|Básico|1|No se puede configurar en el nivel Básico|N/D|N/D|
|Básico|2|No se puede configurar en el nivel Básico|N/D|N/D|
|De uso general|2|0|0|16777216|
|De uso general|4|0|0|33554432|
|De uso general|8|0|0|67108864|
|De uso general|16|0|0|134217728|
|De uso general|32|0|0|134217728|
|De uso general|64|0|0|134217728|
|Memoria optimizada|2|0|0|33554432|
|Memoria optimizada|4|0|0|67108864|
|Memoria optimizada|8|0|0|134217728|
|Memoria optimizada|16|0|0|134217728|
|Memoria optimizada|32|0|0|134217728|

### <a name="sort_buffer_size"></a>sort_buffer_size

Consulte la [documentación de MariaDB](https://mariadb.com/kb/en/server-system-variables/#sort_buffer_size) para más información acerca de este parámetro.

|**Plan de tarifa**|**Núcleos virtuales**|**Valor predeterminado (bytes)**|**Valor mínimo (bytes)**|**Valor máximo (bytes)**|
|---|---|---|---|---|
|Básico|1|No se puede configurar en el nivel Básico|N/D|N/D|
|Básico|2|No se puede configurar en el nivel Básico|N/D|N/D|
|De uso general|2|524 288|32 768|4 194 304|
|De uso general|4|524 288|32 768|8388608|
|De uso general|8|524 288|32 768|16777216|
|De uso general|16|524 288|32 768|33554432|
|De uso general|32|524 288|32 768|33554432|
|De uso general|64|524 288|32 768|33554432|
|Memoria optimizada|2|524 288|32 768|8388608|
|Memoria optimizada|4|524 288|32 768|16777216|
|Memoria optimizada|8|524 288|32 768|33554432|
|Memoria optimizada|16|524 288|32 768|33554432|
|Memoria optimizada|32|524 288|32 768|33554432|

### <a name="tmp_table_size"></a>tmp_table_size

Consulte la [documentación de MariaDB](https://mariadb.com/kb/en/server-system-variables/#tmp_table_size) para más información acerca de este parámetro.

|**Plan de tarifa**|**Núcleos virtuales**|**Valor predeterminado (bytes)**|**Valor mínimo (bytes)**|**Valor máximo (bytes)**|
|---|---|---|---|---|
|Básico|1|No se puede configurar en el nivel Básico|N/D|N/D|
|Básico|2|No se puede configurar en el nivel Básico|N/D|N/D|
|De uso general|2|16777216|1024|67108864|
|De uso general|4|16777216|1024|134217728|
|De uso general|8|16777216|1024|268435456|
|De uso general|16|16777216|1024|536870912|
|De uso general|32|16777216|1024|1073741824|
|De uso general|64|16777216|1024|1073741824|
|Memoria optimizada|2|16777216|1024|134217728|
|Memoria optimizada|4|16777216|1024|268435456|
|Memoria optimizada|8|16777216|1024|536870912|
|Memoria optimizada|16|16777216|1024|1073741824|
|Memoria optimizada|32|16777216|1024|1073741824|

### <a name="time_zone"></a>time_zone

Tras la implementación inicial, un servidor de Azure para MariaDB incluye tablas de sistemas para la información de zona horaria, pero no se han rellenado. Las tablas de la zona horaria se pueden rellenar mediante una llamada al procedimiento almacenado `mysql.az_load_timezone` desde una herramienta como la línea de comandos de MySQL o MySQL Workbench. Vea los artículos de [Azure Portal](howto-server-parameters.md#working-with-the-time-zone-parameter) o de la [CLI de Azure](howto-configure-server-parameters-cli.md#working-with-the-time-zone-parameter) sobre cómo llamar al procedimiento almacenado y establecer las zonas horarias globales o de nivel de sesión.

## <a name="non-configurable-server-parameters"></a>Parámetros no configurables del servidor

Estos parámetros del servidor no son configurables en el servicio:

|**Parámetro**|**Valor fijo**|
| :------------------------ | :-------- |
|innodb_file_per_table en el nivel básico|Apagado|
|innodb_flush_log_at_trx_commit|1|
|sync_binlog|1|
|innodb_log_file_size|256 MB|
|innodb_log_files_in_group|2|

Los demás parámetros del servidor que no aparecen aquí se establecen en sus valores predeterminados de MariaDB para [MariaDB](https://mariadb.com/kb/en/server-system-variables/).

## <a name="next-steps"></a>Pasos siguientes

- Obtenga información sobre cómo [configurar parámetros del servidor con Azure Portal](./howto-server-parameters.md).
- Obtenga información sobre cómo [configurar parámetros del servidor con la CLI de Azure](./howto-configure-server-parameters-cli.md).
- Obtenga información sobre cómo [configurar parámetros del servidor con PowerShell](./howto-configure-server-parameters-using-powershell.md).