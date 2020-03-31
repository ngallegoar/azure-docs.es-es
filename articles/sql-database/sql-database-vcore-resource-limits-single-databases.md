---
title: 'Límites de recursos de núcleo virtual: base de datos única'
description: En esta página se describen algunos límites de recursos en núcleos virtuales comunes para una base de datos única en Azure SQL Database.
services: sql-database
ms.service: sql-database
ms.subservice: single-database
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: carlrab
ms.date: 03/11/2020
ms.openlocfilehash: 38d44f5a2ce22de15cb14ea5aa7a9ca1ea7c03cf
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79481072"
---
# <a name="resource-limits-for-single-databases-using-the-vcore-purchasing-model"></a>Límites de recursos para bases de datos únicas que utilizan el modelo de compra en núcleos virtuales

En este artículo se proporcionan los límites de recursos detallados para bases de datos únicas de Azure SQL Database que utilizan el modelo de compra en núcleos virtuales.

Para conocer los límites del modelo de compra en DTU para las bases de datos únicas en un servidor de SQL Database, consulte la [información general de los límites de recursos para un servidor de SQL Database](sql-database-resource-limits-database-server.md).

Puede establecer el nivel de servicio, el tamaño de proceso y la cantidad de almacenamiento para una base de datos única mediante [Azure Portal](sql-database-single-databases-manage.md#manage-an-existing-sql-database-server), [Transact-SQL](sql-database-single-databases-manage.md#transact-sql-manage-sql-database-servers-and-single-databases), [PowerShell](sql-database-single-databases-manage.md#powershell-manage-sql-database-servers-and-single-databases), la [CLI de Azure](sql-database-single-databases-manage.md#azure-cli-manage-sql-database-servers-and-single-databases) o la [API REST](sql-database-single-databases-manage.md#rest-api-manage-sql-database-servers-and-single-databases).

> [!IMPORTANT]
> Para información y consideraciones sobre el escalado, consulte [Escalado de una base de datos única](sql-database-single-database-scale.md).

## <a name="general-purpose---serverless-compute---gen5"></a>Uso general: proceso sin servidor: Gen5

El [nivel de proceso sin servidor](sql-database-serverless.md) está actualmente disponible en el hardware de Gen5 solo.

### <a name="gen5-compute-generation-part-1"></a>Generación de proceso Gen5 (parte 1)

|Tamaño de proceso|GP_S_Gen5_1|GP_S_Gen5_2|GP_S_Gen5_4|GP_S_Gen5_6|GP_S_Gen5_8|
|:--- | --: |--: |--: |--: |--: |
|Generación de procesos|Gen5|Gen5|Gen5|Gen5|Gen5|
|Mínimo y máximo de núcleos virtuales|0,5 - 1|0,5 - 2|0,5 - 4|0,75 - 6|1,0 - 8|
|Mínimo y máximo de memoria (GB)|2,02 - 3|2,05 - 6|2,10 - 12|2,25 - 18|3,00 - 24|
|Retraso mínimo de pausa automática (minutos)|60|60|60|60|60|
|Compatible con almacén de columnas|Sí|Sí|Sí|Sí|Sí|
|Almacenamiento OLTP en memoria (GB)|N/D|N/D|N/D|N/D|N/D|
|Tamaño máximo de datos (GB)|512|1024|1024|1024|1536|
|Tamaño máximo de registro (GB)|154|307|307|307|461|
|Tamaño máximo de datos de TempDB (GB)|32|64|128|192|256|
|Tipo de almacenamiento|SSD remoto|SSD remoto|SSD remoto|SSD remoto|SSD remoto|
|Latencia de E/S (aproximada)|5-7 ms (escritura)<br>5-10 ms (lectura)|5-7 ms (escritura)<br>5-10 ms (lectura)|5-7 ms (escritura)<br>5-10 ms (lectura)|5-7 ms (escritura)<br>5-10 ms (lectura)|5-7 ms (escritura)<br>5-10 ms (lectura)|
|Número máx. de IOPS de datos *|320|640|1280|1920|2560|
|Velocidad de registro máx. (Mbps)|3.8|7.5|15|22.5|30|
|Cantidad máxima de trabajos (solicitudes) simultáneos|75|150|300|450|600|
|N.º máximo de sesiones simultáneas|30,000|30,000|30,000|30,000|30,000|
|Número de réplicas|1|1|1|1|1|
|AZ múltiple|N/D|N/D|N/D|N/D|N/D|
|Escalado horizontal de lectura|N/D|N/D|N/D|N/D|N/D|
|Almacenamiento de copia de seguridad incluido|1X el tamaño de base de datos|1X el tamaño de base de datos|1X el tamaño de base de datos|1X el tamaño de base de datos|1X el tamaño de base de datos|

\* El valor máximo de los tamaños de e/s que oscilan entre 8 KB y 64 KB. Las IOPS reales dependen de la carga de trabajo. Para obtener más información, consulte [Regulación de E/S de los datos](sql-database-resource-limits-database-server.md#resource-governance).

### <a name="gen5-compute-generation-part-2"></a>Generación de proceso Gen5 (parte 2)

|Tamaño de proceso|GP_S_Gen5_10|GP_S_Gen5_12|GP_S_Gen5_14|GP_S_Gen5_16|
|:--- | --: |--: |--: |--: |
|Generación de procesos|Gen5|Gen5|Gen5|Gen5|
|Mínimo y máximo de núcleos virtuales|1,25 - 10|1,50 - 12|1,75-14|2,00 - 16|
|Mínimo y máximo de memoria (GB)|3,75 - 30|4,50 - 36|5,25 - 42|6,00 - 48|
|Retraso mínimo de pausa automática (minutos)|60|60|60|60|
|Compatible con almacén de columnas|Sí|Sí|Sí|Sí|
|Almacenamiento OLTP en memoria (GB)|N/D|N/D|N/D|N/D|
|Tamaño máximo de datos (GB)|1536|3072|3072|3072|
|Tamaño máximo de registro (GB)|461|461|461|922|
|Tamaño máximo de datos de TempDB (GB)|320|384|448|512|
|Tipo de almacenamiento|SSD remoto|SSD remoto|SSD remoto|SSD remoto|
|Latencia de E/S (aproximada)|5-7 ms (escritura)<br>5-10 ms (lectura)|5-7 ms (escritura)<br>5-10 ms (lectura)|5-7 ms (escritura)<br>5-10 ms (lectura)|5-7 ms (escritura)<br>5-10 ms (lectura)|
|Número máx. de IOPS de datos *|3200|3840|4480|5120|
|Velocidad de registro máx. (Mbps)|30|30|30|30|
|Cantidad máxima de trabajos (solicitudes) simultáneos|750|900|1050|1200|
|N.º máximo de sesiones simultáneas|30,000|30,000|30,000|30,000|
|Número de réplicas|1|1|1|1|
|AZ múltiple|N/D|N/D|N/D|N/D|
|Escalado horizontal de lectura|N/D|N/D|N/D|N/D|
|Almacenamiento de copia de seguridad incluido|1X el tamaño de base de datos|1X el tamaño de base de datos|1X el tamaño de base de datos|1X el tamaño de base de datos|

\* El valor máximo de los tamaños de e/s que oscilan entre 8 KB y 64 KB. Las IOPS reales dependen de la carga de trabajo. Para obtener más información, consulte [Regulación de E/S de los datos](sql-database-resource-limits-database-server.md#resource-governance).

## <a name="hyperscale---provisioned-compute---gen4"></a>Hiperescala: proceso aprovisionado: Gen4

### <a name="gen4-compute-generation-part-1"></a>Generación de proceso Gen4 (parte 1)

|Nivel de rendimiento|HS_Gen4_1|HS_Gen4_2|HS_Gen4_3|HS_Gen4_4|HS_Gen4_5|HS_Gen4_6|
|:--- | --: |--: |--: |---: | --: |--: |
|Generación de procesos|Gen4|Gen4|Gen4|Gen4|Gen4|Gen4|
|Núcleos virtuales|1|2|3|4|5|6|
|Memoria (GB)|7|14|21|28|35|42|
|Tamaño de [RBPEX](sql-database-service-tier-hyperscale.md#compute)|Memoria x3|Memoria x3|Memoria x3|Memoria x3|Memoria x3|Memoria x3|
|Compatible con almacén de columnas|Sí|Sí|Sí|Sí|Sí|Sí|
|Almacenamiento OLTP en memoria (GB)|N/D|N/D|N/D|N/D|N/D|N/D|
|Tamaño máximo de datos (TB)|100 |100 |100 |100 |100 |100|
|Tamaño máximo de registro (TB)|1 |1 |1 |1 |1 |1 |
|Tamaño máximo de datos de TempDB (GB)|32|64|96|128|160|192|
|Tipo de almacenamiento| [Nota 1](#notes) |[Nota 1](#notes)|[Nota 1](#notes) |[Nota 1](#notes) |[Nota 1](#notes) |[Nota 1](#notes) |
|Número máx. de IOPS de datos *|[Nota 2](#notes)|[Nota 2](#notes)|[Nota 2](#notes)|[Nota 2](#notes)|[Nota 2](#notes)|[Nota 2](#notes)|
|Velocidad de registro máx. (Mbps)|100 |100 |100 |100 |100 |100 |
|Latencia de E/S (aproximada)|[Nota 3](#notes)|[Nota 3](#notes)|[Nota 3](#notes)|[Nota 3](#notes)|[Nota 3](#notes)|[Nota 3](#notes)|
|Cantidad máxima de trabajos (solicitudes) simultáneos|200|400|600|800|1000|1200|
|N.º máximo de sesiones simultáneas|30,000|30,000|30,000|30,000|30,000|30,000|
|Réplicas secundarias|0-4|0-4|0-4|0-4|0-4|0-4|
|AZ múltiple|N/D|N/D|N/D|N/D|N/D|N/D|
|Escalado horizontal de lectura|Sí|Sí|Sí|Sí|Sí|Sí|
|Retención de almacenamiento de copia de seguridad|7 días|7 días|7 días|7 días|7 días|7 días|
|||

### <a name="gen4-compute-generation-part-2"></a>Generación de proceso Gen4 (parte 2)

|Nivel de rendimiento|HS_Gen4_7|HS_Gen4_8|HS_Gen4_9|HS_Gen4_10|HS_Gen4_16|HS_Gen4_24|
|:--- | ---: |--: |--: | --: |--: |--: |
|Generación de procesos|Gen4|Gen4|Gen4|Gen4|Gen4|Gen4|
|Núcleos virtuales|7|8|9|10|16|24|
|Memoria (GB)|49|56|63|70|112|159,5|
|Tamaño de [RBPEX](sql-database-service-tier-hyperscale.md#compute)|Memoria x3|Memoria x3|Memoria x3|Memoria x3|Memoria x3|Memoria x3|
|Compatible con almacén de columnas|Sí|Sí|Sí|Sí|Sí|Sí|
|Almacenamiento OLTP en memoria (GB)|N/D|N/D|N/D|N/D|N/D|N/D|
|Tamaño máximo de datos (TB)|100 |100 |100 |100 |100 |100 |
|Tamaño máximo de registro (TB)|1 |1 |1 |1 |1 |1 |
|Tamaño máximo de datos de TempDB (GB)|224|256|288|320|512|768|
|Tipo de almacenamiento| [Nota 1](#notes) |[Nota 1](#notes) |[Nota 1](#notes) |[Nota 1](#notes) |[Nota 1](#notes) |[Nota 1](#notes) |
|Número máx. de IOPS de datos *|[Nota 2](#notes)|[Nota 2](#notes)|[Nota 2](#notes)|[Nota 2](#notes)|[Nota 2](#notes)|[Nota 2](#notes)|
|Velocidad de registro máx. (Mbps)|100 |100 |100 |100 |100 |100 |
|Latencia de E/S (aproximada)|[Nota 3](#notes)|[Nota 3](#notes)|[Nota 3](#notes)|[Nota 3](#notes)|[Nota 3](#notes)|[Nota 3](#notes)|
|Cantidad máxima de trabajos (solicitudes) simultáneos|1400|1600|1800|2000|3200|4800|
|N.º máximo de sesiones simultáneas|30,000|30,000|30,000|30,000|30,000|30,000|
|Réplicas secundarias|0-4|0-4|0-4|0-4|0-4|0-4|
|AZ múltiple|N/D|N/D|N/D|N/D|N/D|N/D|
|Escalado horizontal de lectura|Sí|Sí|Sí|Sí|Sí|Sí|
|Retención de almacenamiento de copia de seguridad|7 días|7 días|7 días|7 días|7 días|7 días|
|||

\* Valor máximo para los tamaños de E/S que oscilan entre 8 KB y 64 KB. Las IOPS reales dependen de la carga de trabajo. Para obtener más información, consulte [Regulación de E/S de los datos](sql-database-resource-limits-database-server.md#resource-governance).

## <a name="hyperscale---provisioned-compute---gen5"></a>Hiperescala: proceso aprovisionado: Gen5

### <a name="gen5-compute-generation-part-1"></a>Generación de proceso Gen5 (parte 1)

|Nivel de rendimiento|HS_Gen5_2|HS_Gen5_4|HS_Gen5_6|HS_Gen_8|HS_Gen5_10|HS_Gen5_12|HS_Gen5_14|
|:--- | --: |--: |--: |--: |---: | --: |--: |--: |
|Generación de procesos|Gen5|Gen5|Gen5|Gen5|Gen5|Gen5|Gen5|
|Núcleos virtuales|2|4|6|8|10|12|14|
|Memoria (GB)|10,4|20,8|31,1|41,5|51,9|62,3|72,7|
|Tamaño de [RBPEX](sql-database-service-tier-hyperscale.md#compute)|Memoria x3|Memoria x3|Memoria x3|Memoria x3|Memoria x3|Memoria x3|Memoria x3|
|Compatible con almacén de columnas|Sí|Sí|Sí|Sí|Sí|Sí|Sí|
|Almacenamiento OLTP en memoria (GB)|N/D|N/D|N/D|N/D|N/D|N/D|N/D|
|Tamaño máximo de datos (TB)|100 |100 |100 |100 |100 |100 |100|
|Tamaño máximo de registro (TB)|1 |1 |1 |1 |1 |1 |1 |
|Tamaño máximo de datos de TempDB (GB)|64|128|192|256|320|384|448|
|Tipo de almacenamiento| [Nota 1](#notes) |[Nota 1](#notes)|[Nota 1](#notes) |[Nota 1](#notes) |[Nota 1](#notes) |[Nota 1](#notes) |[Nota 1](#notes) |
|Número máx. de IOPS de datos *|[Nota 2](#notes)|[Nota 2](#notes)|[Nota 2](#notes)|[Nota 2](#notes)|[Nota 2](#notes)|[Nota 2](#notes)|[Nota 2](#notes)|
|Velocidad de registro máx. (Mbps)|100 |100 |100 |100 |100 |100 |100 |
|Latencia de E/S (aproximada)|[Nota 3](#notes)|[Nota 3](#notes)|[Nota 3](#notes)|[Nota 3](#notes)|[Nota 3](#notes)|[Nota 3](#notes)|[Nota 3](#notes)|
|Cantidad máxima de trabajos (solicitudes) simultáneos|200|400|600|800|1000|1200|1400|
|N.º máximo de sesiones simultáneas|30,000|30,000|30,000|30,000|30,000|30,000|30,000|
|Réplicas secundarias|0-4|0-4|0-4|0-4|0-4|0-4|0-4|
|AZ múltiple|N/D|N/D|N/D|N/D|N/D|N/D|N/D|
|Escalado horizontal de lectura|Sí|Sí|Sí|Sí|Sí|Sí|Sí|
|Retención de almacenamiento de copia de seguridad|7 días|7 días|7 días|7 días|7 días|7 días|7 días|
|||

\* Valor máximo para los tamaños de E/S que oscilan entre 8 KB y 64 KB. Las IOPS reales dependen de la carga de trabajo. Para obtener más información, consulte [Regulación de E/S de los datos](sql-database-resource-limits-database-server.md#resource-governance).

### <a name="gen5-compute-generation-part-2"></a>Generación de proceso Gen5 (parte 2)

|Nivel de rendimiento|HS_Gen5_16|HS_Gen5_18|HS_Gen5_20|HS_Gen_24|HS_Gen5_32|HS_Gen5_40|HS_Gen5_80|
|:--- | --: |--: |--: |--: |---: |--: |--: |
|Generación de procesos|Gen5|Gen5|Gen5|Gen5|Gen5|Gen5|Gen5|
|Núcleos virtuales|16|18|20|24|32|40|80|
|Memoria (GB)|83|93,4|103,8|124,6|166,1|207,6|415,2|
|Tamaño de [RBPEX](sql-database-service-tier-hyperscale.md#compute)|Memoria x3|Memoria x3|Memoria x3|Memoria x3|Memoria x3|Memoria x3|Memoria x3|
|Compatible con almacén de columnas|Sí|Sí|Sí|Sí|Sí|Sí|Sí|
|Almacenamiento OLTP en memoria (GB)|N/D|N/D|N/D|N/D|N/D|N/D|N/D|
|Tamaño máximo de datos (TB)|100 |100 |100 |100 |100 |100 |100 |
|Tamaño máximo de registro (TB)|1 |1 |1 |1 |1 |1 |1 |
|Tamaño máximo de datos de TempDB (GB)|512|576|640|768|1024|1280|2560|
|Tipo de almacenamiento| [Nota 1](#notes) |[Nota 1](#notes)|[Nota 1](#notes)|[Nota 1](#notes) |[Nota 1](#notes) |[Nota 1](#notes) |[Nota 1](#notes) |
|Número máx. de IOPS de datos *|[Nota 2](#notes)|[Nota 2](#notes)|[Nota 2](#notes)|[Nota 2](#notes)|[Nota 2](#notes)|[Nota 2](#notes)|[Nota 2](#notes)|
|Velocidad de registro máx. (Mbps)|100 |100 |100 |100 |100 |100 |100 |
|Latencia de E/S (aproximada)|[Nota 3](#notes)|[Nota 3](#notes)|[Nota 3](#notes)|[Nota 3](#notes)|[Nota 3](#notes)|[Nota 3](#notes)|[Nota 3](#notes)|
|Cantidad máxima de trabajos (solicitudes) simultáneos|1600|1800|2000|2400|3200|4000|8000|
|N.º máximo de sesiones simultáneas|30,000|30,000|30,000|30,000|30,000|30,000|30,000|
|Réplicas secundarias|0-4|0-4|0-4|0-4|0-4|0-4|0-4|
|AZ múltiple|N/D|N/D|N/D|N/D|N/D|N/D|N/D|
|Escalado horizontal de lectura|Sí|Sí|Sí|Sí|Sí|Sí|Sí|
|Retención de almacenamiento de copia de seguridad|7 días|7 días|7 días|7 días|7 días|7 días|7 días|
|||

\* Valor máximo para los tamaños de E/S que oscilan entre 8 KB y 64 KB. Las IOPS reales dependen de la carga de trabajo. Para obtener más información, consulte [Regulación de E/S de los datos](sql-database-resource-limits-database-server.md#resource-governance).

#### <a name="notes"></a>Notas

**Nota 1**: La hiperescala es una arquitectura de varios niveles con componentes de proceso y almacenamiento independientes: [Arquitectura de nivel de servicio Hiperescala](sql-database-service-tier-hyperscale.md#distributed-functions-architecture)

**Nota 2**: Hiperescala es una arquitectura de varios niveles con almacenamiento en caché en varios niveles. Los IOPS efectivos dependen de la carga de trabajo.

**Nota 3**: La latencia es de 1-2 ms para los datos de la caché basada en SSD de RBPEX en las réplicas de proceso, que almacena en la memoria caché las páginas de datos más usadas. Mayor latencia de los datos recuperados de los servidores de páginas.

## <a name="general-purpose---provisioned-compute---gen4"></a>Uso general: proceso aprovisionado: Gen4

> [!IMPORTANT]
> Las nuevas bases de datos de Gen4 ya no se admiten en las regiones Este de Australia o Sur de Brasil.

### <a name="gen4-compute-generation-part-1"></a>Generación de proceso Gen4 (parte 1)

|Tamaño de proceso|GP_Gen4_1|GP_Gen4_2|GP_Gen4_3|GP_Gen4_4|GP_Gen4_5|GP_Gen4_6
|:--- | --: |--: |--: |--: |--: |--: |
|Generación de procesos|Gen4|Gen4|Gen4|Gen4|Gen4|Gen4|
|Núcleos virtuales|1|2|3|4|5|6|
|Memoria (GB)|7|14|21|28|35|42|
|Compatible con almacén de columnas|Sí|Sí|Sí|Sí|Sí|Sí|
|Almacenamiento OLTP en memoria (GB)|N/D|N/D|N/D|N/D|N/D|N/D|
|Tamaño máximo de datos (GB)|1024|1024|1536|1536|1536|3072|
|Tamaño máximo de registro (GB)|307|307|461|461|461|922|
|Tamaño máximo de datos de TempDB (GB)|32|64|96|128|160|192|
|Tipo de almacenamiento|SSD remoto|SSD remoto|SSD remoto|SSD remoto|SSD remoto|SSD remoto|
|Latencia de E/S (aproximada)|5-7 ms (escritura)<br>5-10 ms (lectura)|5-7 ms (escritura)<br>5-10 ms (lectura)|5-7 ms (escritura)<br>5-10 ms (lectura)|5-7 ms (escritura)<br>5-10 ms (lectura)|5-7 ms (escritura)<br>5-10 ms (lectura)|5-7 ms (escritura)<br>5-10 ms (lectura)|
|Número máx. de IOPS de datos *|320|640|960|1280|1600|1920|
|Velocidad de registro máx. (Mbps)|3,75|7.5|11,25|15|18,75|22.5|
|Cantidad máxima de trabajos (solicitudes) simultáneos|200|400|600|800|1000|1200|
|N.º máximo de sesiones simultáneas|30,000|30,000|30,000|30,000|30,000|30,000|
|Número de réplicas|1|1|1|1|1|1|
|AZ múltiple|N/D|N/D|N/D|N/D|N/D|N/D|
|Escalado horizontal de lectura|N/D|N/D|N/D|N/D|N/D|N/D|
|Almacenamiento de copia de seguridad incluido|1X el tamaño de base de datos|1X el tamaño de base de datos|1X el tamaño de base de datos|1X el tamaño de base de datos|1X el tamaño de base de datos|1X el tamaño de base de datos|

\* El valor máximo de los tamaños de e/s que oscilan entre 8 KB y 64 KB. Las IOPS reales dependen de la carga de trabajo. Para obtener más información, consulte [Regulación de E/S de los datos](sql-database-resource-limits-database-server.md#resource-governance).

### <a name="gen4-compute-generation-part-2"></a>Generación de proceso Gen4 (parte 2)

|Tamaño de proceso|GP_Gen4_7|GP_Gen4_8|GP_Gen4_9|GP_Gen4_10|GP_Gen4_16|GP_Gen4_24
|:--- | --: |--: |--: |--: |--: |--: |
|Generación de procesos|Gen4|Gen4|Gen4|Gen4|Gen4|Gen4|
|Núcleos virtuales|7|8|9|10|16|24|
|Memoria (GB)|49|56|63|70|112|159,5|
|Compatible con almacén de columnas|Sí|Sí|Sí|Sí|Sí|Sí|
|Almacenamiento OLTP en memoria (GB)|N/D|N/D|N/D|N/D|N/D|N/D|
|Tamaño máximo de datos (GB)|3072|3072|3072|3072|4096|4096|
|Tamaño máximo de registro (GB)|922|922|922|922|1229|1229|
|Tamaño máximo de datos de TempDB (GB)|224|256|288|320|512|768|
|Tipo de almacenamiento|SSD remoto|SSD remoto|SSD remoto|SSD remoto|SSD remoto|SSD remoto|
|Latencia de E/S (aproximada)|5-7 ms (escritura)<br>5-10 ms (lectura)|5-7 ms (escritura)<br>5-10 ms (lectura)|5-7 ms (escritura)<br>5-10 ms (lectura)|5-7 ms (escritura)<br>5-10 ms (lectura)|5-7 ms (escritura)<br>5-10 ms (lectura)|5-7 ms (escritura)<br>5-10 ms (lectura)
|Número máx. de IOPS de datos *|2240|2560|2880|3200|5120|7680|
|Velocidad de registro máx. (Mbps)|26,3|30|30|30|30|30|
|Cantidad máxima de trabajos (solicitudes) simultáneos|1400|1600|1800|2000|3200|4800|
|N.º máximo de sesiones simultáneas|30,000|30,000|30,000|30,000|30,000|30,000|
|Número de réplicas|1|1|1|1|1|1|
|AZ múltiple|N/D|N/D|N/D|N/D|N/D|N/D|
|Escalado horizontal de lectura|N/D|N/D|N/D|N/D|N/D|N/D|
|Almacenamiento de copia de seguridad incluido|1X el tamaño de base de datos|1X el tamaño de base de datos|1X el tamaño de base de datos|1X el tamaño de base de datos|1X el tamaño de base de datos|1X el tamaño de base de datos|

\* El valor máximo de los tamaños de e/s que oscilan entre 8 KB y 64 KB. Las IOPS reales dependen de la carga de trabajo. Para obtener más información, consulte [Regulación de E/S de los datos](sql-database-resource-limits-database-server.md#resource-governance).

## <a name="general-purpose---provisioned-compute---gen5"></a>Uso general: proceso aprovisionado: Gen5

### <a name="gen5-compute-generation-part-1"></a>Generación de proceso Gen5 (parte 1)

|Tamaño de proceso|GP_Gen5_2|GP_Gen5_4|GP_Gen5_6|GP_Gen5_8|GP_Gen5_10|GP_Gen5_12|GP_Gen5_14|
|:--- | --: |--: |--: |--: |---: | --: |--: |
|Generación de procesos|Gen5|Gen5|Gen5|Gen5|Gen5|Gen5|Gen5|
|Núcleos virtuales|2|4|6|8|10|12|14|
|Memoria (GB)|10,4|20,8|31,1|41,5|51,9|62,3|72,7|
|Compatible con almacén de columnas|Sí|Sí|Sí|Sí|Sí|Sí|Sí|
|Almacenamiento OLTP en memoria (GB)|N/D|N/D|N/D|N/D|N/D|N/D|N/D|
|Tamaño máximo de datos (GB)|1024|1024|1536|1536|1536|3072|3072|
|Tamaño máximo de registro (GB)|307|307|461|461|461|922|922|
|Tamaño máximo de datos de TempDB (GB)|64|128|192|256|320|384|384|
|Tipo de almacenamiento|SSD remoto|SSD remoto|SSD remoto|SSD remoto|SSD remoto|SSD remoto|SSD remoto|
|Latencia de E/S (aproximada)|5-7 ms (escritura)<br>5-10 ms (lectura)|5-7 ms (escritura)<br>5-10 ms (lectura)|5-7 ms (escritura)<br>5-10 ms (lectura)|5-7 ms (escritura)<br>5-10 ms (lectura)|5-7 ms (escritura)<br>5-10 ms (lectura)|5-7 ms (escritura)<br>5-10 ms (lectura)|5-7 ms (escritura)<br>5-10 ms (lectura)|
|Número máx. de IOPS de datos *|640|1280|1920|2560|3200|3840|4480|
|Velocidad de registro máx. (Mbps)|7.5|15|22.5|30|30|30|30|
|Cantidad máxima de trabajos (solicitudes) simultáneos|200|400|600|800|1000|1200|1400|
|N.º máximo de sesiones simultáneas|30,000|30,000|30,000|30,000|30,000|30,000|30,000|
|Número de réplicas|1|1|1|1|1|1|1|
|AZ múltiple|N/D|N/D|N/D|N/D|N/D|N/D|N/D|
|Escalado horizontal de lectura|N/D|N/D|N/D|N/D|N/D|N/D|N/D|
|Almacenamiento de copia de seguridad incluido|1X el tamaño de base de datos|1X el tamaño de base de datos|1X el tamaño de base de datos|1X el tamaño de base de datos|1X el tamaño de base de datos|1X el tamaño de base de datos|1X el tamaño de base de datos|

\* El valor máximo de los tamaños de e/s que oscilan entre 8 KB y 64 KB. Las IOPS reales dependen de la carga de trabajo. Para obtener más información, consulte [Regulación de E/S de los datos](sql-database-resource-limits-database-server.md#resource-governance).

### <a name="gen5-compute-generation-part-2"></a>Generación de proceso Gen5 (parte 2)

|Tamaño de proceso|GP_Gen5_16|GP_Gen5_18|GP_Gen5_20|GP_Gen5_24|GP_Gen5_32|GP_Gen5_40|GP_Gen5_80|
|:--- | --: |--: |--: |--: |---: | --: |--: |
|Generación de procesos|Gen5|Gen5|Gen5|Gen5|Gen5|Gen5|Gen5|
|Núcleos virtuales|16|18|20|24|32|40|80|
|Memoria (GB)|83|93,4|103,8|124,6|166,1|207,6|415,2|
|Compatible con almacén de columnas|Sí|Sí|Sí|Sí|Sí|Sí|Sí|
|Almacenamiento OLTP en memoria (GB)|N/D|N/D|N/D|N/D|N/D|N/D|N/D|
|Tamaño máximo de datos (GB)|3072|3072|3072|4096|4096|4096|4096|
|Tamaño máximo de registro (GB)|922|922|922|1229|1229|1229|1229|
|Tamaño máximo de datos de TempDB (GB)|512|576|640|768|1024|1280|2560|
|Tipo de almacenamiento|SSD remoto|SSD remoto|SSD remoto|SSD remoto|SSD remoto|SSD remoto|SSD remoto|
|Latencia de E/S (aproximada)|5-7 ms (escritura)<br>5-10 ms (lectura)|5-7 ms (escritura)<br>5-10 ms (lectura)|5-7 ms (escritura)<br>5-10 ms (lectura)|5-7 ms (escritura)<br>5-10 ms (lectura)|5-7 ms (escritura)<br>5-10 ms (lectura)|5-7 ms (escritura)<br>5-10 ms (lectura)|5-7 ms (escritura)<br>5-10 ms (lectura)|
|Número máx. de IOPS de datos *|5120|5760|6400|7680|10240|12800|25 600|
|Velocidad de registro máx. (Mbps)|30|30|30|30|30|30|30|
|Cantidad máxima de trabajos (solicitudes) simultáneos|1600|1800|2000|2400|3200|4000|8000|
|N.º máximo de sesiones simultáneas|30,000|30,000|30,000|30,000|30,000|30,000|30,000|
|Número de réplicas|1|1|1|1|1|1|1|
|AZ múltiple|N/D|N/D|N/D|N/D|N/D|N/D|N/D|
|Escalado horizontal de lectura|N/D|N/D|N/D|N/D|N/D|N/D|N/D|
|Almacenamiento de copia de seguridad incluido|1X el tamaño de base de datos|1X el tamaño de base de datos|1X el tamaño de base de datos|1X el tamaño de base de datos|1X el tamaño de base de datos|1X el tamaño de base de datos|1X el tamaño de base de datos|

\* El valor máximo de los tamaños de e/s que oscilan entre 8 KB y 64 KB. Las IOPS reales dependen de la carga de trabajo. Para obtener más información, consulte [Regulación de E/S de los datos](sql-database-resource-limits-database-server.md#resource-governance).

## <a name="general-purpose---provisioned-compute---fsv2-series"></a>Uso general: proceso aprovisionado: serie Fsv2

### <a name="fsv2-series-compute-generation-preview"></a>Generación de proceso de la serie Fsv2 (versión preliminar)

|Tamaño de proceso|GP_Fsv2_72|
|:--- | --: |
|Generación de procesos|Serie Fsv2|
|Núcleos virtuales|72|
|Memoria (GB)|136,2|
|Compatible con almacén de columnas|Sí|
|Almacenamiento OLTP en memoria (GB)|N/D|
|Tamaño máximo de datos (GB)|4096|
|Tamaño máximo de registro (GB)|1024|
|Tamaño máximo de datos de TempDB (GB)|333|
|Tipo de almacenamiento|SSD remoto|
|Latencia de E/S (aproximada)|5-7 ms (escritura)<br>5-10 ms (lectura)|
|Número máx. de IOPS de datos *|12.800|
|Velocidad de registro máx. (Mbps)|30|
|Cantidad máxima de trabajos (solicitudes) simultáneos|3600|
|Máximo de inicios de sesión simultáneos|3600|
|N.º máximo de sesiones simultáneas|30,000|
|Número de réplicas|1|
|AZ múltiple|N/D|
|Escalado horizontal de lectura|N/D|
|Almacenamiento de copia de seguridad incluido|1X el tamaño de base de datos|

\* El valor máximo de los tamaños de e/s que oscilan entre 8 KB y 64 KB. Las IOPS reales dependen de la carga de trabajo. Para obtener más información, consulte [Regulación de E/S de los datos](sql-database-resource-limits-database-server.md#resource-governance).

## <a name="business-critical---provisioned-compute---gen4"></a>Crítico para la empresa: proceso aprovisionado: Gen4

> [!IMPORTANT]
> Las nuevas bases de datos de Gen4 ya no se admiten en las regiones Este de Australia o Sur de Brasil.

### <a name="gen4-compute-generation-part-1"></a>Generación de proceso Gen4 (parte 1)

|Tamaño de proceso|BC_Gen4_1|BC_Gen4_2|BC_Gen4_3|BC_Gen4_4|BC_Gen4_5|BC_Gen4_6|
|:--- | --: |--: |--: |--: |--: |--: |
|Generación de procesos|Gen4|Gen4|Gen4|Gen4|Gen4|Gen4|
|Núcleos virtuales|1|2|3|4|5|6|
|Memoria (GB)|7|14|21|28|35|42|
|Compatible con almacén de columnas|Sí|Sí|Sí|Sí|Sí|Sí|
|Almacenamiento OLTP en memoria (GB)|1|2|3|4|5|6|
|Tipo de almacenamiento|SSD local|SSD local|SSD local|SSD local|SSD local|SSD local|
|Tamaño máximo de datos (GB)|1024|1024|1024|1024|1024|1024|
|Tamaño máximo de registro (GB)|307|307|307|307|307|307|
|Tamaño máximo de datos de TempDB (GB)|32|64|96|128|160|192|
|Latencia de E/S (aproximada)|1-2 ms (escritura)<br>1-2 ms (lectura)|1-2 ms (escritura)<br>1-2 ms (lectura)|1-2 ms (escritura)<br>1-2 ms (lectura)|1-2 ms (escritura)<br>1-2 ms (lectura)|1-2 ms (escritura)<br>1-2 ms (lectura)|1-2 ms (escritura)<br>1-2 ms (lectura)|
|Número máx. de IOPS de datos *|4\.000|8,000|12,000|16 000|20.000|24,000|
|Velocidad de registro máx. (Mbps)|8|16|24|32|40|48|
|Cantidad máxima de trabajos (solicitudes) simultáneos|200|400|600|800|1000|1200|
|Máximo de inicios de sesión simultáneos|200|400|600|800|1000|1200|
|N.º máximo de sesiones simultáneas|30,000|30,000|30,000|30,000|30,000|30,000|
|Número de réplicas|4|4|4|4|4|4|
|AZ múltiple|Sí|Sí|Sí|Sí|Sí|Sí|
|Escalado horizontal de lectura|Sí|Sí|Sí|Sí|Sí|Sí|
|Almacenamiento de copia de seguridad incluido|1X el tamaño de base de datos|1X el tamaño de base de datos|1X el tamaño de base de datos|1X el tamaño de base de datos|1X el tamaño de base de datos|1X el tamaño de base de datos|

\* El valor máximo de los tamaños de e/s que oscilan entre 8 KB y 64 KB. Las IOPS reales dependen de la carga de trabajo. Para obtener más información, consulte [Regulación de E/S de los datos](sql-database-resource-limits-database-server.md#resource-governance).

### <a name="gen4-compute-generation-part-2"></a>Generación de proceso Gen4 (parte 2)

|Tamaño de proceso|BC_Gen4_7|BC_Gen4_8|BC_Gen4_9|BC_Gen4_10|BC_Gen4_16|BC_Gen4_24|
|:--- | --: |--: |--: |--: |--: |--: |
|Generación de procesos|Gen4|Gen4|Gen4|Gen4|Gen4|Gen4|
|Núcleos virtuales|7|8|9|10|16|24|
|Memoria (GB)|49|56|63|70|112|159,5|
|Compatible con almacén de columnas|Sí|Sí|Sí|Sí|Sí|Sí|
|Almacenamiento OLTP en memoria (GB)|7|8|9.5|11|20|36|
|Tipo de almacenamiento|SSD local|SSD local|SSD local|SSD local|SSD local|SSD local|
|Tamaño máximo de datos (GB)|1024|1024|1024|1024|1024|1024|
|Tamaño máximo de registro (GB)|307|307|307|307|307|307|
|Tamaño máximo de datos de TempDB (GB)|224|256|288|320|512|768|
|Latencia de E/S (aproximada)|1-2 ms (escritura)<br>1-2 ms (lectura)|1-2 ms (escritura)<br>1-2 ms (lectura)|1-2 ms (escritura)<br>1-2 ms (lectura)|1-2 ms (escritura)<br>1-2 ms (lectura)|1-2 ms (escritura)<br>1-2 ms (lectura)|1-2 ms (escritura)<br>1-2 ms (lectura)|
|Número máx. de IOPS de datos |28 000|32 000|36 000|40.000|64 000|76 800|
|Velocidad de registro máx. (Mbps)|56|64|64|64|64|64|
|Cantidad máxima de trabajos (solicitudes) simultáneos|1400|1600|1800|2000|3200|4800|
|Máximo de inicios de sesión simultáneos (solicitudes)|1400|1600|1800|2000|3200|4800|
|N.º máximo de sesiones simultáneas|30,000|30,000|30,000|30,000|30,000|30,000|
|Número de réplicas|4|4|4|4|4|4|
|AZ múltiple|Sí|Sí|Sí|Sí|Sí|Sí|
|Escalado horizontal de lectura|Sí|Sí|Sí|Sí|Sí|Sí|
|Almacenamiento de copia de seguridad incluido|1X el tamaño de base de datos|1X el tamaño de base de datos|1X el tamaño de base de datos|1X el tamaño de base de datos|1X el tamaño de base de datos|1X el tamaño de base de datos|

\* El valor máximo de los tamaños de e/s que oscilan entre 8 KB y 64 KB. Las IOPS reales dependen de la carga de trabajo. Para obtener más información, consulte [Regulación de E/S de los datos](sql-database-resource-limits-database-server.md#resource-governance).

## <a name="business-critical---provisioned-compute---gen5"></a>Crítico para la empresa: proceso aprovisionado: Gen5

### <a name="gen5-compute-generation-part-1"></a>Generación de proceso Gen5 (parte 1)

|Tamaño de proceso|BC_Gen5_2|BC_Gen5_4|BC_Gen5_6|BC_Gen5_8|BC_Gen5_10|BC_Gen5_12|BC_Gen5_14|
|:--- | --: |--: |--: |--: |---: | --: |--: |
|Generación de procesos|Gen5|Gen5|Gen5|Gen5|Gen5|Gen5|Gen5|
|Núcleos virtuales|2|4|6|8|10|12|14|
|Memoria (GB)|10,4|20,8|31,1|41,5|51,9|62,3|72,7|
|Compatible con almacén de columnas|Sí|Sí|Sí|Sí|Sí|Sí|Sí|
|Almacenamiento OLTP en memoria (GB)|1,57|3,14|4.71|6,28|8,65|11,02|13,39|
|Tamaño máximo de datos (GB)|1024|1024|1536|1536|1536|3072|3072|
|Tamaño máximo de registro (GB)|307|307|461|461|461|922|922|
|Tamaño máximo de datos de TempDB (GB)|64|128|192|256|320|384|448|
|Tipo de almacenamiento|SSD local|SSD local|SSD local|SSD local|SSD local|SSD local|SSD local|
|Latencia de E/S (aproximada)|1-2 ms (escritura)<br>1-2 ms (lectura)|1-2 ms (escritura)<br>1-2 ms (lectura)|1-2 ms (escritura)<br>1-2 ms (lectura)|1-2 ms (escritura)<br>1-2 ms (lectura)|1-2 ms (escritura)<br>1-2 ms (lectura)|1-2 ms (escritura)<br>1-2 ms (lectura)|1-2 ms (escritura)<br>1-2 ms (lectura)|
|Número máx. de IOPS de datos *|8000|16 000|24,000|32 000|40.000|48 000|56 000|
|Velocidad de registro máx. (Mbps)|24|48|72|96|96|96|96|
|Cantidad máxima de trabajos (solicitudes) simultáneos|200|400|600|800|1000|1200|1400|
|Máximo de inicios de sesión simultáneos|200|400|600|800|1000|1200|1400|
|N.º máximo de sesiones simultáneas|30,000|30,000|30,000|30,000|30,000|30,000|30,000|
|Número de réplicas|4|4|4|4|4|4|4|
|AZ múltiple|Sí|Sí|Sí|Sí|Sí|Sí|Sí|
|Escalado horizontal de lectura|Sí|Sí|Sí|Sí|Sí|Sí|Sí|
|Almacenamiento de copia de seguridad incluido|1X el tamaño de base de datos|1X el tamaño de base de datos|1X el tamaño de base de datos|1X el tamaño de base de datos|1X el tamaño de base de datos|1X el tamaño de base de datos|1X el tamaño de base de datos|

\* El valor máximo de los tamaños de e/s que oscilan entre 8 KB y 64 KB. Las IOPS reales dependen de la carga de trabajo. Para obtener más información, consulte [Regulación de E/S de los datos](sql-database-resource-limits-database-server.md#resource-governance).

### <a name="gen5-compute-generation-part-2"></a>Generación de proceso Gen5 (parte 2)

|Tamaño de proceso|BC_Gen5_16|BC_Gen5_18|BC_Gen5_20|BC_Gen5_24|BC_Gen5_32|BC_Gen5_40|BC_Gen5_80|
|:--- | --: |--: |--: |--: |---: | --: |--: |
|Generación de procesos|Gen5|Gen5|Gen5|Gen5|Gen5|Gen5|Gen5|
|Núcleos virtuales|16|18|20|24|32|40|80|
|Memoria (GB)|83|93,4|103,8|124,6|166,1|207,6|415,2|
|Compatible con almacén de columnas|Sí|Sí|Sí|Sí|Sí|Sí|Sí|
|Almacenamiento OLTP en memoria (GB)|15,77|18,14|20.51|25,25|37,94|52,23|131,64|
|Tamaño máximo de datos (GB)|3072|3072|3072|4096|4096|4096|4096|
|Tamaño máximo de registro (GB)|922|922|922|1229|1229|1229|1229|
|Tamaño máximo de datos de TempDB (GB)|512|576|640|768|1024|1280|2560|
|Tipo de almacenamiento|SSD local|SSD local|SSD local|SSD local|SSD local|SSD local|SSD local|
|Latencia de E/S (aproximada)|1-2 ms (escritura)<br>1-2 ms (lectura)|1-2 ms (escritura)<br>1-2 ms (lectura)|1-2 ms (escritura)<br>1-2 ms (lectura)|1-2 ms (escritura)<br>1-2 ms (lectura)|1-2 ms (escritura)<br>1-2 ms (lectura)|1-2 ms (escritura)<br>1-2 ms (lectura)|1-2 ms (escritura)<br>1-2 ms (lectura)|
|Número máx. de IOPS de datos *|64 000|72 000|80 000|96 000|128 000|160 000|204 800|
|Velocidad de registro máx. (Mbps)|96|96|96|96|96|96|96|
|Cantidad máxima de trabajos (solicitudes) simultáneos|1600|1800|2000|2400|3200|4000|8000|
|Máximo de inicios de sesión simultáneos|1600|1800|2000|2400|3200|4000|8000|
|N.º máximo de sesiones simultáneas|30,000|30,000|30,000|30,000|30,000|30,000|30,000|
|Número de réplicas|4|4|4|4|4|4|4|
|AZ múltiple|Sí|Sí|Sí|Sí|Sí|Sí|Sí|
|Escalado horizontal de lectura|Sí|Sí|Sí|Sí|Sí|Sí|Sí|
|Almacenamiento de copia de seguridad incluido|1X el tamaño de base de datos|1X el tamaño de base de datos|1X el tamaño de base de datos|1X el tamaño de base de datos|1X el tamaño de base de datos|1X el tamaño de base de datos|1X el tamaño de base de datos|

\* El valor máximo de los tamaños de e/s que oscilan entre 8 KB y 64 KB. Las IOPS reales dependen de la carga de trabajo. Para obtener más información, consulte [Regulación de E/S de los datos](sql-database-resource-limits-database-server.md#resource-governance).

## <a name="business-critical---provisioned-compute---m-series"></a>Crítico para la empresa: proceso aprovisionado: serie M

### <a name="m-series-compute-generation-preview"></a>Generación de proceso de la serie M (versión preliminar)

|Tamaño de proceso|BC_M_128|
|:--- | --: |
|Generación de procesos|Serie M|
|Núcleos virtuales|128|
|Memoria (GB)|3767.1|
|Compatible con almacén de columnas|Sí|
|Almacenamiento OLTP en memoria (GB)|1768|
|Tamaño máximo de datos (GB)|4096|
|Tamaño máximo de registro (GB)|2048|
|Tamaño máximo de datos de TempDB (GB)|4096|
|Tipo de almacenamiento|SSD local|
|Latencia de E/S (aproximada)|1-2 ms (escritura)<br>1-2 ms (lectura)|
|Número máx. de IOPS de datos *|160 000|
|Velocidad de registro máx. (Mbps)|264|
|Cantidad máxima de trabajos (solicitudes) simultáneos|12.800|
|Máximo de inicios de sesión simultáneos|12.800|
|N.º máximo de sesiones simultáneas|30000|
|Número de réplicas|4|
|AZ múltiple|Sí|
|Escalado horizontal de lectura|Sí|
|Almacenamiento de copia de seguridad incluido|1X el tamaño de base de datos|

\* El valor máximo de los tamaños de e/s que oscilan entre 8 KB y 64 KB. Las IOPS reales dependen de la carga de trabajo. Para obtener más información, consulte [Regulación de E/S de los datos](sql-database-resource-limits-database-server.md#resource-governance).

> [!IMPORTANT]
> En algunas circunstancias, puede que deba reducir una base de datos para reclamar el espacio no utilizado. Para obtener más información, consulte [Administración del espacio de archivo en Azure SQL Database](sql-database-file-space-management.md).

## <a name="next-steps"></a>Pasos siguientes

- Para conocer los límites de recursos de DTU para una base de datos única, consulte los [límites de recursos para bases de datos únicas que usan el modelo de compra de DTU](sql-database-dtu-resource-limits-single-databases.md).
- Para conocer los límites de recursos de núcleos virtuales para grupos elásticos, consulte los [límites de recursos para grupos elásticos con el modelo de compra del núcleo virtual](sql-database-vcore-resource-limits-elastic-pools.md).
- Para conocer los límites de recursos de DTU para grupos elásticos, consulte los [límites de recursos para grupos elásticos que usan el modelo de compra de DTU](sql-database-dtu-resource-limits-elastic-pools.md).
- Para conocer los límites de recursos para instancias administradas, consulte los [límites de recursos para instancias administradas](sql-database-managed-instance-resource-limits.md).
- Para más información sobre los límites generales de Azure, consulte [Límites, cuotas y restricciones de suscripción y servicios de Microsoft Azure](../azure-resource-manager/management/azure-subscription-service-limits.md).
- Para más información sobre los límites de recursos en un servidor de bases de datos, consulte [Límites de recursos en un servidor de SQL Database](sql-database-resource-limits-database-server.md) para obtener información acerca de los límites en los niveles de servidor y suscripción.
