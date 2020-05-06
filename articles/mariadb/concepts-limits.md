---
title: 'Limitaciones: Azure Database for MariaDB'
description: En este artículo se describen las limitaciones de Azure Database for MariaDB como el número de conexiones o las opciones de motor de almacenamiento.
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.topic: conceptual
ms.date: 4/1/2020
ms.openlocfilehash: 18f227c1888e0565eebb640fa61ced56dc994865
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2020
ms.locfileid: "80632336"
---
# <a name="limitations-in-azure-database-for-mariadb"></a>Limitaciones de Azure Database for MariaDB
En las siguientes secciones se describen la capacidad, la compatibilidad del motor de almacenamiento, la compatibilidad de los privilegios, la compatibilidad de las instrucciones de manipulación de datos y los límites funcionales del servicio de base de datos.

## <a name="server-parameters"></a>Parámetros del servidor

Los valores mínimo y máximo de varios parámetros de servidor populares vienen determinados por el plan de tarifa y los núcleos virtuales. Consulte las tablas siguientes para conocer los límites.

### <a name="max_connections"></a>max_connections

|**Plan de tarifa**|**Núcleos virtuales**|**Valor predeterminado**|**Valor mínimo**|**Valor máximo**|
|---|---|---|---|---|
|Básica|1|50|10|50|
|Básica|2|100|10|100|
|Uso general|2|300|10|600|
|Uso general|4|625|10|1250|
|Uso general|8|1250|10|2\.500|
|Uso general|16|2\.500|10|5000|
|Uso general|32|5000|10|10000|
|Uso general|64|10000|10|20000|
|Memoria optimizada|2|600|10|800|
|Memoria optimizada|4|1250|10|2\.500|
|Memoria optimizada|8|2\.500|10|5000|
|Memoria optimizada|16|5000|10|10000|
|Memoria optimizada|32|10000|10|20000|

Si las conexiones superan el límite, puede que reciba el error siguiente:
> ERROR 1040 (08004): Demasiadas conexiones

> [!IMPORTANT]
> Para obtener la mejor experiencia posible, se recomienda usar un agrupador de conexiones, como ProxySQL, para administrar las conexiones de forma eficaz.

La creación de conexiones cliente a MariaDB lleva tiempo y, una vez establecidas, estas conexiones ocupan recursos de bases de datos, incluso cuando están inactivas. La mayoría de las aplicaciones solicitan muchas conexiones de corta duración, y esto es lo que conforma esta situación. El resultado es que hay menos recursos disponibles para la carga de trabajo real, lo que baja el rendimiento. Esto se puede evitar con un agrupador de conexiones, ya que reduce las conexiones inactivas y reutiliza las conexiones existentes. Para más información sobre cómo configurar ProxySQL, consulte nuestra [entrada de blog](https://techcommunity.microsoft.com/t5/azure-database-for-mysql/load-balance-read-replicas-using-proxysql-in-azure-database-for/ba-p/880042).

### <a name="query_cache_size"></a>query_cache_size

La caché de consulta está desactivada de forma predeterminada. Para habilitar la caché de consulta, configure el parámetro `query_cache_type`. 

Consulte la [documentación de MariaDB](https://mariadb.com/kb/en/server-system-variables/#query_cache_size) para más información acerca de este parámetro.

|**Plan de tarifa**|**Núcleos virtuales**|**Valor predeterminado**|**Valor mínimo**|**Valor máximo**|
|---|---|---|---|---|
|Básica|1|No se puede configurar en el nivel Básico|N/D|N/D|
|Básica|2|No se puede configurar en el nivel Básico|N/D|N/D|
|Uso general|2|0|0|16777216|
|Uso general|4|0|0|33554432|
|Uso general|8|0|0|67108864|
|Uso general|16|0|0|134217728|
|Uso general|32|0|0|134217728|
|Uso general|64|0|0|134217728|
|Memoria optimizada|2|0|0|33554432|
|Memoria optimizada|4|0|0|67108864|
|Memoria optimizada|8|0|0|134217728|
|Memoria optimizada|16|0|0|134217728|
|Memoria optimizada|32|0|0|134217728|

### <a name="sort_buffer_size"></a>sort_buffer_size

Consulte la [documentación de MariaDB](https://mariadb.com/kb/en/server-system-variables/#sort_buffer_size) para más información acerca de este parámetro.

|**Plan de tarifa**|**Núcleos virtuales**|**Valor predeterminado**|**Valor mínimo**|**Valor máximo**|
|---|---|---|---|---|
|Básica|1|No se puede configurar en el nivel Básico|N/D|N/D|
|Básica|2|No se puede configurar en el nivel Básico|N/D|N/D|
|Uso general|2|524 288|32 768|4 194 304|
|Uso general|4|524 288|32 768|8388608|
|Uso general|8|524 288|32 768|16777216|
|Uso general|16|524 288|32 768|33554432|
|Uso general|32|524 288|32 768|33554432|
|Uso general|64|524 288|32 768|33554432|
|Memoria optimizada|2|524 288|32 768|8388608|
|Memoria optimizada|4|524 288|32 768|16777216|
|Memoria optimizada|8|524 288|32 768|33554432|
|Memoria optimizada|16|524 288|32 768|33554432|
|Memoria optimizada|32|524 288|32 768|33554432|

### <a name="join_buffer_size"></a>join_buffer_size

Consulte la [documentación de MariaDB](https://mariadb.com/kb/en/server-system-variables/#join_buffer_size) para más información acerca de este parámetro.

|**Plan de tarifa**|**Núcleos virtuales**|**Valor predeterminado**|**Valor mínimo**|**Valor máximo**|
|---|---|---|---|---|
|Básica|1|No se puede configurar en el nivel Básico|N/D|N/D|
|Básica|2|No se puede configurar en el nivel Básico|N/D|N/D|
|Uso general|2|262 144|128|268435455|
|Uso general|4|262 144|128|536870912|
|Uso general|8|262 144|128|1073741824|
|Uso general|16|262 144|128|2147483648|
|Uso general|32|262 144|128|4294967295|
|Uso general|64|262 144|128|4294967295|
|Memoria optimizada|2|262 144|128|536870912|
|Memoria optimizada|4|262 144|128|1073741824|
|Memoria optimizada|8|262 144|128|2147483648|
|Memoria optimizada|16|262 144|128|4294967295|
|Memoria optimizada|32|262 144|128|4294967295|

### <a name="max_heap_table_size"></a>max_heap_table_size

Consulte la [documentación de MariaDB](https://mariadb.com/kb/en/server-system-variables/#max_heap_table_size) para más información acerca de este parámetro.

|**Plan de tarifa**|**Núcleos virtuales**|**Valor predeterminado**|**Valor mínimo**|**Valor máximo**|
|---|---|---|---|---|
|Básica|1|No se puede configurar en el nivel Básico|N/D|N/D|
|Básica|2|No se puede configurar en el nivel Básico|N/D|N/D|
|Uso general|2|16777216|16384|268435455|
|Uso general|4|16777216|16384|536870912|
|Uso general|8|16777216|16384|1073741824|
|Uso general|16|16777216|16384|2147483648|
|Uso general|32|16777216|16384|4294967295|
|Uso general|64|16777216|16384|4294967295|
|Memoria optimizada|2|16777216|16384|536870912|
|Memoria optimizada|4|16777216|16384|1073741824|
|Memoria optimizada|8|16777216|16384|2147483648|
|Memoria optimizada|16|16777216|16384|4294967295|
|Memoria optimizada|32|16777216|16384|4294967295|

### <a name="tmp_table_size"></a>tmp_table_size

Consulte la [documentación de MariaDB](https://mariadb.com/kb/en/server-system-variables/#tmp_table_size) para más información acerca de este parámetro.

|**Plan de tarifa**|**Núcleos virtuales**|**Valor predeterminado**|**Valor mínimo**|**Valor máximo**|
|---|---|---|---|---|
|Básica|1|No se puede configurar en el nivel Básico|N/D|N/D|
|Básica|2|No se puede configurar en el nivel Básico|N/D|N/D|
|Uso general|2|16777216|1024|67108864|
|Uso general|4|16777216|1024|134217728|
|Uso general|8|16777216|1024|268435456|
|Uso general|16|16777216|1024|536870912|
|Uso general|32|16777216|1024|1073741824|
|Uso general|64|16777216|1024|1073741824|
|Memoria optimizada|2|16777216|1024|134217728|
|Memoria optimizada|4|16777216|1024|268435456|
|Memoria optimizada|8|16777216|1024|536870912|
|Memoria optimizada|16|16777216|1024|1073741824|
|Memoria optimizada|32|16777216|1024|1073741824|

### <a name="time_zone"></a>time_zone

Las tablas de la zona horaria se pueden rellenar mediante una llamada al procedimiento almacenado `mysql.az_load_timezone` desde una herramienta como la línea de comandos de MySQL o MySQL Workbench. Vea los artículos de [Azure Portal](howto-server-parameters.md#working-with-the-time-zone-parameter) o de la [CLI de Azure](howto-configure-server-parameters-cli.md#working-with-the-time-zone-parameter) sobre cómo llamar al procedimiento almacenado y establecer las zonas horarias globales o de nivel de sesión.

## <a name="storage-engine-support"></a>Compatibilidad del motor de almacenamiento

### <a name="supported"></a>Compatible
- [InnoDB](https://mariadb.com/kb/en/library/xtradb-and-innodb/)
- [MEMORY](https://mariadb.com/kb/en/library/memory-storage-engine/)

### <a name="unsupported"></a>No compatible
- [MyISAM](https://mariadb.com/kb/en/library/myisam-storage-engine/)
- [BLACKHOLE](https://mariadb.com/kb/en/library/blackhole/)
- [ARCHIVE](https://mariadb.com/kb/en/library/archive/)

## <a name="privilege-support"></a>Compatibilidad de los privilegios

### <a name="unsupported"></a>No compatible
- Rol DBA: muchos parámetros de servidor y valores de configuración pueden reducir por error el rendimiento del servidor o invalidar las propiedades ACID del sistema de administración de bases de datos. Por lo tanto, para mantener la integridad del servicio y SLA en un nivel de producto, no se expone el rol DBA de este servicio. La cuenta de usuario predeterminada, que se crea a la vez que las instancias de base de datos, permite a los usuarios realizar la mayoría de las instrucciones DDL y DML en la instancia de base de datos administrados.
- Privilegio SUPER: del mismo modo, los [privilegios SUPER](https://mariadb.com/kb/en/library/grant/#global-privileges) también están restringidos.
- DEFINER: Requiere privilegios SUPER para crear y está restringido. Si importa datos mediante una copia de seguridad, quite los comandos `CREATE DEFINER` manualmente o mediante el comando `--skip-definer` durante una operación mysqldump.

## <a name="data-manipulation-statement-support"></a>Compatibilidad de las instrucciones de manipulación de datos

### <a name="supported"></a>Compatible
- `LOAD DATA INFILE` es compatible, pero el parámetro `[LOCAL]` debe especificarse y dirigirse a una ruta de acceso UNC (Azure Storage montado a través de SMB).

### <a name="unsupported"></a>No compatible
- `SELECT ... INTO OUTFILE`

## <a name="functional-limitations"></a>Limitaciones funcionales

### <a name="scale-operations"></a>Operaciones de escalado
- El escalado dinámico a y desde niveles de precios Básico no se admite en este momento.
- La reducción del tamaño de almacenamiento del servidor no se admite.

### <a name="server-version-upgrades"></a>Actualizaciones de la versión de servidor
- La migración automatizada entre las principales versiones del motor de base de datos no se admite en este momento.

### <a name="point-in-time-restore"></a>Restauración a un momento dado
- Al usar la característica PITR, el nuevo servidor se crea con la misma configuración que el servidor en el que se basa.
- La restauración a un servidor que se ha eliminado no se admite en este momento.

### <a name="subscription-management"></a>Administración de suscripciones
- El movimiento dinámico de servidores creados previamente entre grupo de suscripciones y recursos no se admite en este momento.

### <a name="vnet-service-endpoints"></a>Puntos de conexión de servicio de red virtual
- La compatibilidad con puntos de conexión de servicio de red virtual solo existe para servidores de uso general y optimizados para memoria.

### <a name="storage-size"></a>Tamaño de almacenamiento
- Consulte [Planes de tarifa](concepts-pricing-tiers.md) para obtener los límites de tamaño de almacenamiento para cada plan de tarifa.

## <a name="current-known-issues"></a>Problemas conocidos actualmente
- La instancia del servidor MariaDB muestra una versión de servidor incorrecta después de establecer la conexión. Para obtener la versión del motor de instancias de servidor correcta, use el comando `select version();`.

## <a name="next-steps"></a>Pasos siguientes
- [Qué está disponible en cada nivel de servicio](concepts-pricing-tiers.md)
- [Versiones de base de datos de MariaDB admitidas](concepts-supported-versions.md)
