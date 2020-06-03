---
title: 'Limitaciones: Azure Database for MySQL'
description: En este artículo, se describen limitaciones de Azure Database for MySQL tales como el número de opciones del motor de almacenamiento y de conexión.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: conceptual
ms.date: 4/1/2020
ms.openlocfilehash: 9cf5c958a0dd9a19e6b976ff36a18c45e062f604
ms.sourcegitcommit: fdec8e8bdbddcce5b7a0c4ffc6842154220c8b90
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/19/2020
ms.locfileid: "83659934"
---
# <a name="limitations-in-azure-database-for-mysql"></a>Limitaciones en Azure Database for MySQL
En las siguientes secciones se describen la capacidad, la compatibilidad del motor de almacenamiento, la compatibilidad de los privilegios, la compatibilidad de las instrucciones de manipulación de datos y los límites funcionales del servicio de base de datos. Consulte también las [limitaciones generales](https://dev.mysql.com/doc/mysql-reslimits-excerpt/5.6/en/limits.html) que se aplican al motor de base de datos MySQL.

## <a name="server-parameters"></a>Parámetros del servidor

Los valores mínimo y máximo de varios parámetros de servidor populares vienen determinados por el plan de tarifa y los núcleos virtuales. Consulte las tablas siguientes para conocer los límites.

### <a name="max_connections"></a>max_connections

|**Plan de tarifa**|**Núcleos virtuales**|**Valor predeterminado**|**Valor mínimo**|**Valor máximo**|
|---|---|---|---|---|
|Básico|1|50|10|50|
|Básico|2|100|10|100|
|De uso general|2|300|10|600|
|De uso general|4|625|10|1250|
|De uso general|8|1250|10|2500|
|De uso general|16|2500|10|5000|
|De uso general|32|5000|10|10 000|
|De uso general|64|10 000|10|20 000|
|Memoria optimizada|2|600|10|800|
|Memoria optimizada|4|1250|10|2500|
|Memoria optimizada|8|2500|10|5000|
|Memoria optimizada|16|5000|10|10 000|
|Memoria optimizada|32|10 000|10|20 000|

Si las conexiones superan el límite, puede que reciba el error siguiente:
> ERROR 1040 (08004): Demasiadas conexiones

> [!IMPORTANT]
> Para obtener la mejor experiencia posible, se recomienda usar un agrupador de conexiones, como ProxySQL, para administrar las conexiones de forma eficaz.

La creación de conexiones de cliente a MySQL lleva tiempo y, una vez establecidas, estas conexiones ocupan recursos de bases de datos, incluso cuando están inactivas. La mayoría de las aplicaciones solicitan muchas conexiones de corta duración, y esto es lo que conforma esta situación. El resultado es que hay menos recursos disponibles para la carga de trabajo real, lo que baja el rendimiento. Esto se puede evitar con un agrupador de conexiones, ya que reduce las conexiones inactivas y reutiliza las conexiones existentes. Para más información sobre cómo configurar ProxySQL, consulte nuestra [entrada de blog](https://techcommunity.microsoft.com/t5/azure-database-for-mysql/load-balance-read-replicas-using-proxysql-in-azure-database-for/ba-p/880042).

### <a name="query_cache_size"></a>query_cache_size

La caché de consulta está desactivada de forma predeterminada. Para habilitar la caché de consulta, configure el parámetro `query_cache_type`. 

Consulte la [documentación de MySQL](https://dev.mysql.com/doc/refman/5.7/en/server-system-variables.html#sysvar_query_cache_size) para más información acerca de este parámetro.

> [!NOTE]
> La caché de consulta está en desuso desde MySQL 5.7.20 y se quitó en MySQL 8.0.

|**Plan de tarifa**|**Núcleos virtuales**|**Valor predeterminado**|**Valor mínimo**|**Valor máximo**|
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

Consulte la [documentación de MySQL](https://dev.mysql.com/doc/refman/5.7/en/server-system-variables.html#sysvar_sort_buffer_size) para más información acerca de este parámetro.

|**Plan de tarifa**|**Núcleos virtuales**|**Valor predeterminado**|**Valor mínimo**|**Valor máximo**|
|---|---|---|---|---|
|Básico|1|No se puede configurar en el nivel Básico|N/D|N/D|
|Básico|2|No se puede configurar en el nivel Básico|N/D|N/D|
|De uso general|2|524288|32768|4 194 304|
|De uso general|4|524288|32768|8388608|
|De uso general|8|524288|32768|16777216|
|De uso general|16|524288|32768|33554432|
|De uso general|32|524288|32768|33554432|
|De uso general|64|524288|32768|33554432|
|Memoria optimizada|2|524288|32768|8388608|
|Memoria optimizada|4|524288|32768|16777216|
|Memoria optimizada|8|524288|32768|33554432|
|Memoria optimizada|16|524288|32768|33554432|
|Memoria optimizada|32|524288|32768|33554432|

### <a name="join_buffer_size"></a>join_buffer_size

Consulte la [documentación de MySQL](https://dev.mysql.com/doc/refman/5.7/en/server-system-variables.html#sysvar_join_buffer_size) para más información acerca de este parámetro.

|**Plan de tarifa**|**Núcleos virtuales**|**Valor predeterminado**|**Valor mínimo**|**Valor máximo**|
|---|---|---|---|---|
|Básico|1|No se puede configurar en el nivel Básico|N/D|N/D|
|Básico|2|No se puede configurar en el nivel Básico|N/D|N/D|
|De uso general|2|262144|128|268435455|
|De uso general|4|262144|128|536870912|
|De uso general|8|262144|128|1073741824|
|De uso general|16|262144|128|2147483648|
|De uso general|32|262144|128|4294967295|
|De uso general|64|262144|128|4294967295|
|Memoria optimizada|2|262144|128|536870912|
|Memoria optimizada|4|262144|128|1073741824|
|Memoria optimizada|8|262144|128|2147483648|
|Memoria optimizada|16|262144|128|4294967295|
|Memoria optimizada|32|262144|128|4294967295|

### <a name="max_heap_table_size"></a>max_heap_table_size

Consulte la [documentación de MySQL](https://dev.mysql.com/doc/refman/5.7/en/server-system-variables.html#sysvar_max_heap_table_size) para más información acerca de este parámetro.

|**Plan de tarifa**|**Núcleos virtuales**|**Valor predeterminado**|**Valor mínimo**|**Valor máximo**|
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

### <a name="tmp_table_size"></a>tmp_table_size

Consulte la [documentación de MySQL](https://dev.mysql.com/doc/refman/5.7/en/server-system-variables.html#sysvar_tmp_table_size) para más información acerca de este parámetro.

|**Plan de tarifa**|**Núcleos virtuales**|**Valor predeterminado**|**Valor mínimo**|**Valor máximo**|
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

Las tablas de la zona horaria se pueden rellenar mediante una llamada al procedimiento almacenado `mysql.az_load_timezone` desde una herramienta como la línea de comandos de MySQL o MySQL Workbench. Vea los artículos de [Azure Portal](howto-server-parameters.md#working-with-the-time-zone-parameter) o de la [CLI de Azure](howto-configure-server-parameters-using-cli.md#working-with-the-time-zone-parameter) sobre cómo llamar al procedimiento almacenado y establecer las zonas horarias globales o de nivel de sesión.

### <a name="innodb_file_per_table"></a>innodb_file_per_table

MySQL almacena la tabla InnoDB en distintos espacios de tabla en función de la configuración proporcionada durante la creación de la tabla. El [espacio de tablas del sistema](https://dev.mysql.com/doc/refman/5.7/en/innodb-system-tablespace.html) es el área de almacenamiento del diccionario de datos de InnoDB. Un [espacio de tablas de archivo por tabla](https://dev.mysql.com/doc/refman/5.7/en/innodb-file-per-table-tablespaces.html) contiene datos e índices para una sola tabla de InnoDB y se almacena en el sistema de archivos en su propio archivo de datos. Este comportamiento se controla mediante el parámetro de servidor `innodb_file_per_table`. Si `innodb_file_per_table` se establece en `OFF`, InnoDB crea tablas en el espacio de tablas del sistema. De lo contrario, InnoDB crea tablas en espacios de tabla de archivo por tabla.

Azure Database for MySQL admite **1 TB** como máximo en un solo archivo de datos. Si el tamaño de la base de datos es superior a 1 TB, hay que crear la tabla en el espacio de tabla [innodb_file_per_table](https://dev.mysql.com/doc/refman/5.7/en/innodb-parameters.html#sysvar_innodb_file_per_table). Si tiene una sola tabla de tamaño superior a 1 TB, debe usar la tabla de particiones.

## <a name="storage-engine-support"></a>Compatibilidad del motor de almacenamiento

### <a name="supported"></a>Compatible
- [InnoDB](https://dev.mysql.com/doc/refman/5.7/en/innodb-introduction.html)
- [MEMORY](https://dev.mysql.com/doc/refman/5.7/en/memory-storage-engine.html)

### <a name="unsupported"></a>No compatible
- [MyISAM](https://dev.mysql.com/doc/refman/5.7/en/myisam-storage-engine.html)
- [BLACKHOLE](https://dev.mysql.com/doc/refman/5.7/en/blackhole-storage-engine.html)
- [ARCHIVE](https://dev.mysql.com/doc/refman/5.7/en/archive-storage-engine.html)
- [FEDERATED](https://dev.mysql.com/doc/refman/5.7/en/federated-storage-engine.html)

## <a name="privilege-support"></a>Compatibilidad de los privilegios

### <a name="unsupported"></a>No compatible
- Rol DBA: muchos parámetros y valores de servidor pueden reducir por error el rendimiento del servidor o invalidar las propiedades ACID del sistema de administración de bases de datos (DBMS). Por lo tanto, para mantener la integridad del servicio y el SLA en un nivel de producto, no se expone el rol DBA en este servicio. La cuenta de usuario predeterminada, que se crea a la vez que las instancias de base de datos, permite a los usuarios realizar la mayoría de las instrucciones DDL y DML en la instancia de base de datos administrada. 
- Privilegio SUPER: del mismo modo, los [privilegios SUPER](https://dev.mysql.com/doc/refman/5.7/en/privileges-provided.html#priv_super) también están restringidos.
- DEFINER: requiere privilegios SUPER para crear y está restringido. Si importa datos mediante una copia de seguridad, quite los comandos `CREATE DEFINER` manualmente o mediante el comando `--skip-definer` durante una operación mysqldump.

## <a name="data-manipulation-statement-support"></a>Compatibilidad de las instrucciones de manipulación de datos

### <a name="supported"></a>Compatible
- `LOAD DATA INFILE` es compatible, pero el parámetro `[LOCAL]` debe especificarse y dirigirse a una ruta de acceso UNC (Azure Storage montado a través de SMB).

### <a name="unsupported"></a>No compatible
- `SELECT ... INTO OUTFILE`

## <a name="functional-limitations"></a>Limitaciones funcionales

### <a name="scale-operations"></a>Operaciones de escalado
- El escalado dinámico hacia y desde planes de tarifa Básico no se admite en este momento.
- La reducción del tamaño de almacenamiento del servidor no se admite.

### <a name="server-version-upgrades"></a>Actualizaciones de la versión de servidor
- La migración automatizada entre versiones principales del motor de base de datos no se admite en este momento. Si quiere actualizar a la siguiente versión principal, realice un [volcado y restáurelo](./concepts-migrate-dump-restore.md) a un servidor que se haya creado con la nueva versión del motor.

### <a name="point-in-time-restore"></a>Restauración a un momento dado
- Al usar la característica PITR, el nuevo servidor se crea con la misma configuración que el servidor en el que se basa.
- La restauración a un servidor que se ha eliminado no se admite en este momento.

### <a name="vnet-service-endpoints"></a>Puntos de conexión de servicio de red virtual
- La compatibilidad con puntos de conexión de servicio de red virtual solo existe para servidores de uso general y optimizados para memoria.

### <a name="storage-size"></a>Tamaño de almacenamiento
- Consulte [Planes de tarifa](concepts-pricing-tiers.md) para obtener los límites de tamaño de almacenamiento para cada plan de tarifa.

## <a name="current-known-issues"></a>Problemas conocidos actualmente
- La instancia del servidor MySQL muestra una versión de servidor errónea después de establecer la conexión. Para obtener la versión del motor de instancias de servidor correcta, use el comando `select version();`.

## <a name="next-steps"></a>Pasos siguientes
- [Qué está disponible en cada nivel de servicio](concepts-pricing-tiers.md)
- [Versiones de base de datos de MySQL admitidas](concepts-supported-versions.md)
