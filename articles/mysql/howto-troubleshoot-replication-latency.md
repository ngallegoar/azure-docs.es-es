---
title: 'Solución de problemas de latencia de replicación: Azure Database for MySQL'
description: Obtenga información acerca de cómo solucionar problemas de latencia de replicación con réplicas de lectura de Azure Database for MySQL.
keywords: MySQL, solución de problemas, latencia de replicación en segundos
author: savjani
ms.author: pariks
ms.service: mysql
ms.topic: troubleshooting
ms.date: 10/08/2020
ms.openlocfilehash: 16a502a53b4441faf68ea342e0bc865731d38b1a
ms.sourcegitcommit: fbb620e0c47f49a8cf0a568ba704edefd0e30f81
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "91876866"
---
# <a name="troubleshoot-replication-latency-in-azure-database-for-mysql"></a>Solución de problemas de la latencia de replicación en Azure Database for MySQL

La característica de [réplica de lectura](concepts-read-replicas.md) permite replicar datos de un servidor de Azure Database for MySQL en un servidor de réplica de solo lectura. Las réplicas de lectura se usan para escalar horizontalmente la carga de trabajo mediante el enrutamiento de consultas de lectura e informes desde la aplicación hacia los servidores de réplica. Esto reduce la presión del servidor principal y mejora el rendimiento general y la latencia de la aplicación a medida que se escala. Las réplicas se actualizan asincrónicamente mediante la tecnología de replicación basada en la posición de los archivos de registros binarios nativos (binlog) del motor de MySQL. Para obtener más información acerca de la replicación de binlog, consulte la [Introducción a la replicación de binlog de MySQL](https://dev.mysql.com/doc/refman/5.7/en/binlog-replication-configuration-overview.html). 

El retraso de la replicación en las réplicas de lectura secundarias depende del número de factores, incluyendo, entre otros: 

- Latencia de red
- Volumen de transacciones en el servidor de origen.
- Nivel de proceso del servidor de réplica de lectura secundaria y de origen.
- Consultas que se ejecutan en el servidor principal y secundario. 

En este documento obtendrá información sobre cómo solucionar problemas de latencia de replicación en Azure Database for MySQL. Asimismo, también conocerá algunas de las causas comunes del aumento de la latencia de replicación en los servidores de réplica.

## <a name="replication-concepts"></a>Conceptos de replicación

Cuando el registro binario está habilitado, el servidor de origen escribe la transacción confirmada en el registro binario que se usa para la replicación. El registro binario está activado de forma predeterminada en todos los servidores recién aprovisionados que admiten un almacenamiento de hasta 16 TB. En los servidores de réplica, hay dos subprocesos que se ejecutan por servidor de réplica: uno denominado subproceso de E/S y el otro denominado subproceso de SQL.

- El **subproceso de E/S** se conecta al servidor de origen y solicita los registros binarios actualizados. Una vez que este subproceso recibe las actualizaciones de registro binarias, se guardan en un servidor de réplicas, en un registro local denominado registro de retransmisión.
- El **subproceso de SQL** lee el registro de retransmisión y aplica los cambios de datos en los servidores de réplica.

## <a name="monitoring-replication-latency"></a>Supervisar la latencia de replicación

Azure Database for MySQL proporciona la métrica de retraso de replicación en segundos en [Azure Monitor](concepts-monitoring.md). Esta métrica solo está disponible en los servidores de réplica de lectura. Esta métrica se calcula con la métrica seconds_behind_master disponible en MySQL. Para comprender la causa principal del aumento de la latencia de replicación, conéctese al servidor de réplicas mediante [MySQL Workbench](connect-workbench.md) o [Azure Cloud Shell](https://shell.azure.com) y ejecute el siguiente comando:

 Reemplace los valores por el nombre del servidor de réplica real y el nombre de inicio de sesión del usuario administrador. El nombre de usuario del administrador requiere el valor "@\<servername>" en Azure Database for MySQL:

  ```azurecli-interactive
  mysql --host=myreplicademoserver.mysql.database.azure.com --user=myadmin@mydemoserver -p 
  ```

  Este es el aspecto de la experiencia en el terminal de Cloud Shell.
  ```
  Requesting a Cloud Shell.Succeeded.
  Connecting terminal...

  Welcome to Azure Cloud Shell

  Type "az" to use Azure CLI
  Type "help" to learn about Cloud Shell

  user@Azure:~$mysql -h myreplicademoserver.mysql.database.azure.com -u myadmin@mydemoserver -p
  Enter password:
  Welcome to the MySQL monitor.  Commands end with ; or \g.
  Your MySQL connection id is 64796
  Server version: 5.6.42.0 Source distribution

  Copyright (c) 2000, 2020, Oracle and/or its affiliates. All rights reserved.

  Oracle is a registered trademark of Oracle Corporation and/or its
  affiliates. Other names may be trademarks of their respective
  owners.

  Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.
  mysql>
  ```
  En el mismo terminal de Azure Cloud Shell, ejecute el siguiente comando:

  ```
  mysql> SHOW SLAVE STATUS;
  ```

  Una salida típica tendrá el siguiente aspecto:
  
>[!div class="mx-imgBorder"]
> :::image type="content" source="./media/howto-troubleshoot-replication-latency/show-status.png" alt-text="Supervisar la latencia de replicación&quot;:::


La salida contiene mucha información, pero normalmente solo es importante centrarse en las siguientes columnas:

|Métrica|Descripción|
|---|---|
|Slave_IO_State| Estado actual del subproceso de E/S. Normalmente, el estado es &quot;Esperando a que el maestro envíe el evento&quot; si se está sincronizando. Sin embargo, si ve un estado como &quot;Conectando con el maestro&quot;, quiere decir que la réplica ha perdido la conexión con el servidor maestro. Compruebe si se está ejecutando el maestro o si un firewall está bloqueando la conexión.|
|Master_Log_File| Archivo de registro binario en el que se escribe el maestro.|
|Read_Master_Log_Pos| Representa la posición en el archivo de registro binario anterior en el que se escribe el maestro.|
|Relay_Master_Log_File| Cuando se indica, representa el archivo de registro binario que el servidor de réplica lee del maestro.|
|Slave_IO_Running| Indica si el subproceso de E/S se está ejecutando. El valor debería ser &quot;Sí&quot;. Si es &quot;No&quot;, lo más probable es que la replicación se interrumpa.|
|Slave_SQL_Running| Indica si se está ejecutando el subproceso de SQL. El valor debería ser &quot;Sí&quot;. Si es &quot;No&quot;, lo más probable es que la replicación se interrumpa.|
|Exec_Master_Log_Pos| Muestra la posición del elemento Relay_Master_Log_File al que se aplica la réplica. Si hay latencia, esta secuencia de posición debe ser menor que Read_Master_Log_Pos.|
|Relay_Log_Space|Muestra el límite superior del tamaño del registro de retransmisión. Puede comprobar el tamaño consultando variables globales como &quot;relay_log_space_limit&quot;.|
|Seconds_Behind_Master| Muestra la latencia de replicación en segundos.|
|Last_IO_Errno|Muestra el código de error del subproceso de E/S, si existe. Para obtener más información sobre estos códigos, consulte la [Documentación de MySQL](https://dev.mysql.com/doc/refman/5.7/en/server-error-reference.html).|
|Last_IO_Error| Muestra el mensaje de error del subproceso de E/S, si existe.|
|Last_SQL_Errno|Muestra el código de error del subproceso de SQL, si existe. Para obtener más información sobre estos códigos, consulte la [Documentación de MySQL](https://dev.mysql.com/doc/refman/5.7/en/server-error-reference.html).|
|Last_SQL_Error|Muestra el mensaje de error del subproceso de SQL, si existe.|
|Slave_SQL_Running_State| Indica el estado actual del subproceso de SQL. Tenga en cuenta que la opción &quot;Bloqueo del sistema&quot; que se muestra en este estado es un comportamiento normal. Es normal ver el estado &quot;Esperando la confirmación de la transacción dependiente&quot;. Esta indica que la réplica está esperando a que el maestro actualice las transacciones confirmadas.|

Si Slave_IO_Running tiene el valor &quot;Sí" y Slave_SQL_Running tiene el valor "Sí", la replicación se ejecutará correctamente. 

A continuación, debe comprobar el elemento Last_IO_Errno, Last_IO_Error, Last_SQL_Errno y Last_SQL_Error.  Estos campos contienen el número y el mensaje del error más reciente que hizo que se detuviera el subproceso de SQL. Un número de error 0 y un mensaje vacío significa que no hay ningún error. Un valor distinto de cero en el error se debe investigar con más detalle mediante la búsqueda del código de error en la [documentación de MySQL](https://dev.mysql.com/doc/refman/5.7/en/server-error-reference.html).

## <a name="common-scenarios-for-high-replication-latency"></a>Escenarios comunes de latencia de replicación elevada

### <a name="network-latency-or-high-cpu-on-source-server"></a>Latencia de red o CPU alta en el servidor de origen

Si observa los valores siguientes, la causa más común de la latencia de replicación es la alta latencia de red o un alto consumo de CPU en el servidor de origen. En este caso, el subproceso de E/S se está ejecutando y está esperando en el maestro. El maestro (servidor de origen) ya ha escrito en el archivo de registro binario número 20, mientras que la réplica solo se ha recibido hasta el archivo número 10. Los principales factores que contribuyen a la latencia de replicación elevada en este escenario son la velocidad de red o el uso intensivo de la CPU en el servidor de origen.  En Azure, la latencia de red dentro de una región normalmente oscila en milisegundos; entre regiones, puede llegar hasta segundos. En la mayoría de los casos, el retraso en el subproceso de E/S para conectarse al servidor de origen se debe a un uso intensivo de la CPU en el servidor de origen que ralentiza el procesamiento del subproceso de E/S. Esto puede detectarse supervisando el uso de la CPU y observando el número de conexiones simultáneas en el servidor de origen mediante Azure monitor.

Si no ve un uso intensivo de la CPU en el servidor de origen, las posibles causas pueden ser la latencia de red. Si ve una latencia de red excesiva que sea anormalmente alta, es recomendable comprobar la [página de estado de Azure](https://status.azure.com/status) para asegurarse de que no haya problemas o interrupciones inesperados. 

```
Slave_IO_State: Waiting for master to send event
Master_Log_File: the binary file sequence is larger then Relay_Master_Log_File, e.g. mysql-bin.00020
Relay_Master_Log_File: the file sequence is smaller than Master_Log_File, e.g. mysql-bin.00010
```

### <a name="heavy-burst-of-transactions-on-source-server"></a>Ráfaga intensa de transacciones en el servidor de origen

Si observa los valores siguientes, la causa más común de la latencia de la replicación es una ráfaga intensa de transacciones en el servidor de origen. En el siguiente resultado, aunque la réplica puede recuperar el registro binario correspondiente al maestro, el subproceso de E/S de la réplica indica que el espacio del registro de retransmisión ya está lleno. Por lo tanto, la velocidad de red no produce el retraso, porque la réplica ya ha estado intentando ponerse al día tan rápido como le es posible. En su lugar, el tamaño del registro binario actualizado supera el límite superior del espacio del registro de retransmisión. Para solucionar este problema aún más, es necesario que habilite el [registro de consultas lentas](concepts-server-logs.md) en el servidor maestro. Los registros de consultas lentas le permiten identificar transacciones de larga duración en el servidor de origen. Las consultas identificadas deben ajustarse para reducir la latencia en el servidor. 

```
Slave_IO_State: Waiting for the slave SQL thread to free enough relay log space
Master_Log_File: the binary file sequence is larger then Relay_Master_Log_File, e.g. mysql-bin.00020
Relay_Master_Log_File: the file sequence is smaller then Master_Log_File, e.g. mysql-bin.00010
```

A continuación se indican las causas comunes de la latencia en esta categoría:

#### <a name="replication-latency-due-to-data-load-on-source-server"></a>Latencia de replicación debido a la carga de datos en el servidor de origen
En algunos casos, existe una carga de datos semanal o mensual en los servidores de origen. Desafortunadamente, la latencia de replicación es inevitable en este caso. En este ejemplo, los servidores de réplica acaban de actualizarse después de haber completado la carga de datos en el servidor de origen.


### <a name="slowness-on-the-replica-server"></a>Lentitud en el servidor de réplicas

Si observa los valores siguientes, la causa más común puede ser algún problema en el servidor de réplicas que necesita más investigación. En este ejemplo, tal y como se muestra en el resultado, los subprocesos de E/S y SQL se están ejecutando correctamente y la réplica lee el mismo archivo de registro binario que las escrituras maestras. Sin embargo, se produce cierta latencia en el servidor de réplicas para reflejar la misma transacción del servidor de origen. 

```
Slave_IO_State: Waiting for master to send event
Master_Log_File: The binary log file sequence equals to Relay_Master_Log_File, e.g. mysql-bin.000191
Read_Master_Log_Pos: The position of master server written to the above file is larger than Relay_Log_Pos, e.g. 103978138
Relay_Master_Log_File: mysql-bin.000191
Slave_IO_Running: Yes
Slave_SQL_Running: Yes
Exec_Master_Log_Pos: The position of slave reads from master binary log file is smaller than Read_Master_Log_Pos, e.g. 13468882
Seconds_Behind_Master: There is latency and the value here is greater than 0
```

A continuación se indican las causas comunes de la latencia en esta categoría:

#### <a name="no-primary-or-unique-key-on-a-table"></a>No hay ninguna clave principal o única en una tabla

Azure Database for MySQL utiliza la replicación basada en filas. Gracias a la replicación basada en filas, el servidor maestro escribe los eventos en el registro binario sobre el cambio de filas de la tabla individual. A su vez, el subproceso de SQL ejecuta esos cambios en las filas correspondientes de la tabla que se encuentra en el servidor de réplicas. Ninguna clave principal o única de una tabla es una de las causas comunes de la latencia de replicación. La falta de claves principales o únicas provoca que el subproceso de SQL examine todas las filas de la tabla de destino para aplicar los cambios.

En MySQL, la clave principal es un índice asociado que garantiza un rendimiento de consultas rápido, ya que no puede incluir valores NULL. Con el motor de almacenamiento de InnoDB, los datos de la tabla se organizan físicamente para realizar búsquedas y ordenaciones ultrarápidas según la clave principal. Por lo tanto, se recomienda agregar una clave principal en las tablas del servidor de origen antes de crear el servidor de réplicas. En este ejemplo, debe agregar las claves principales en el servidor de origen y volver a crear las réplicas de lectura para que pueda mejorar la latencia de replicación.

Puede usar la siguiente consulta para determinar en qué tablas falta la clave principal en el servidor de origen:

```sql 
select tab.table_schema as database_name, tab.table_name 
from information_schema.tables tab left join 
information_schema.table_constraints tco 
on tab.table_schema = tco.table_schema 
and tab.table_name = tco.table_name 
and tco.constraint_type = 'PRIMARY KEY' 
where tco.constraint_type is null 
and tab.table_schema not in('mysql', 'information_schema', 'performance_schema', 'sys') 
and tab.table_type = 'BASE TABLE' 
order by tab.table_schema, tab.table_name;

```

#### <a name="replication-latency-due-to-long-running-queries-on-replica-server"></a>Latencia de replicación debido a las consultas de larga duración en el servidor de réplicas

Es posible que la carga de trabajo del servidor de réplica pueda impedir que el subproceso de SQL se mantenga actualizado con el subproceso de E/S. Esta es una de las causas comunes de la latencia de replicación elevada si hay una consulta de larga duración en el servidor de réplicas. En este caso, debe habilitar el [registro de consultas lentas](concepts-server-logs.md) en el servidor de réplicas para poder solucionar el problema. Las consultas lentas pueden aumentar el consumo de recursos o ralentizar el servidor, por lo que la réplica no podrá ponerse al día con el maestro. En este ejemplo, debe optimizar las consultas lentas. Las consultas más rápidas evitan el bloqueo de los subprocesos de SQL y mejoran significativamente la latencia de replicación.


#### <a name="replication-latency-due-to-ddl-queries-on-source-server"></a>Latencia de replicación debido a las consultas DDL en el servidor de origen
Si se ejecuta un comando DDL de larga duración como [ALTER TABLE](https://dev.mysql.com/doc/refman/5.7/en/alter-table.html) en el servidor de origen, es posible que tarde una hora en ejecutarse. Durante ese tiempo, probablemente se ejecuten miles de consultas en paralelo en el servidor de origen. Cuando DDL se replica, para garantizar la coherencia de la base de datos, el motor de MySQL tiene que ejecutar DDL en un único subproceso de replicación. Por lo tanto, todas las demás consultas replicadas se bloquearán y tendrán que esperar una hora o más hasta que se complete la operación DDL en el servidor de réplicas. Esto es así independientemente de si la operación DDL se realiza en línea o no. Con las operaciones DDL, se espera que en la replicación haya un aumento de la latencia de replicación.

Si ha habilitado un [registro de consultas lentas](concepts-server-logs.md) en el servidor de origen, este escenario se puede detectar examinando los registros de consultas lentas para ver si se ha ejecutado un comando DDL en el servidor de origen. A pesar de que la eliminación de índices, el cambio de nombre y la creación deben usar el algoritmo INPLACE para la instrucción ALTER TABLE, esto puede implicar la copia de datos de la tabla y que se vuelva a generar la tabla. Normalmente se admite un lenguaje de manipulación de datos (DML) simultáneo de algoritmos de INPLACE, pero se puede realizar brevemente un bloqueo de metadatos exclusivo en la tabla durante las fases de preparación y ejecución de la operación. Así pues, en la instrucción CREATE INDEX se pueden usar las cláusulas ALGORITHM y LOCK para influir en el método de copia de tablas y en el nivel de simultaneidad de las opciones de lectura y escritura. Sin embargo, si se agrega un índice de tipo FULLTEXT o SPATIAL, se evitarán las operaciones DML. A continuación tiene un ejemplo de creación de un índice con cláusulas de tipo ALGORITHM y LOCK:

```sql
ALTER TABLE table_name ADD INDEX index_name (column), ALGORITHM=INPLACE, LOCK=NONE;
```

Desafortunadamente, en el caso de la instrucción DDL que requiere un bloqueo, no se puede evitar la latencia de la replicación; por lo tanto, estos tipos de operaciones DDL deben realizarse durante las horas de mínima actividad (por ejemplo, durante la noche) para reducir el impacto potencial.

#### <a name="replication-latency-due-to-replica-server-lower-sku"></a>Latencia de replicación debida al SKU inferior del servidor de réplicas

En Azure Database for MySQL las réplicas de lectura se crean con la misma configuración de servidor que el servidor maestro. Una vez creado, se puede cambiar la configuración del servidor de réplica. Sin embargo, si el servidor de réplicas se va a cambiar a una versión anterior, la carga de trabajo puede producir un mayor consumo de recursos que, a su vez, puede provocar la latencia de replicación. Esto se puede observar supervisando el consumo de CPU y memoria del servidor de réplicas desde Azure Monitor. En este caso, se recomienda mantener la configuración del servidor de réplicas con valores iguales o mayores que el de origen, para asegurarse de que la réplica funciona al mismo nivel que el servidor maestro.

#### <a name="improving-replication-latency-using-server-parameter-tuning-on-source-server"></a>Mejora de la latencia de replicación mediante el ajuste de parámetros de servidor en el servidor de origen

En Azure Database for MySQL, la replicación está optimizada para ejecutarse de forma predeterminada con subprocesos paralelos en réplicas. En el caso de las cargas de trabajo de simultaneidad elevadas que están en el servidor de origen y en el cual el servidor de réplicas no se puede actualizar, la latencia de replicación se puede mejorar si se configuran los parámetros binlog_group_commit_sync_delay en el servidor de origen. Este parámetro controla cuántos microsegundos espera la confirmación de registro binario antes de sincronizar el archivo de registro binario. La ventaja es que, en lugar de aplicar inmediatamente cada transacción confirmada, el maestro envía las actualizaciones del registro binario de forma masiva. Esto reduce el estándar E/S en la réplica y ayuda a mejorar el rendimiento. En este ejemplo, puede ser útil establecer binlog_group_commit_sync_delay en 1000, así como supervisar la latencia de replicación. Este parámetro debe establecerse con precaución y solo ha de aprovecharse en las cargas de trabajo simultáneas. En un escenario de baja concurrencia con muchas transacciones de singleton, la configuración de binlog_group_commit_sync_delay puede aumentar la latencia porque el subproceso de E/S está esperando actualizaciones masivas del registro binario, mientras que solo se pueden confirmar algunas transacciones. 

## <a name="next-steps"></a>Pasos siguientes
Obtenga más información en [Información general sobre la replicación de binlog de MySQL](https://dev.mysql.com/doc/refman/5.7/en/binlog-replication-configuration-overview.html).
