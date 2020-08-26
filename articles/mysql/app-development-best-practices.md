---
title: 'Procedimientos recomendados de desarrollo de aplicaciones: Azure Database for MySQL'
description: Más información sobre los procedimientos recomendados al compilar una aplicación con Azure Database for MySQL
author: mksuni
ms.author: sumuth
ms.service: mysql
ms.topic: conceptual
ms.date: 08/11/2020
ms.openlocfilehash: 36f9cfa2369032351f6ed2948fc0c98c1648bcb6
ms.sourcegitcommit: ef055468d1cb0de4433e1403d6617fede7f5d00e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2020
ms.locfileid: "88259238"
---
# <a name="best-practices-for-building-applications-with-azure-database-for-mysql"></a>Procedimientos recomendados para compilar aplicaciones con Azure Database for MySQL 

Estos son algunos procedimientos recomendados para ayudar a compilar aplicaciones listas para la nube con Azure Database for MySQL que pueden reducir el tiempo de desarrollo de la aplicación. 

## <a name="application-and-database-resource-configuration"></a>Configuración de recursos de la aplicación y de la base de datos

### <a name="application-and-database-in-the-same-region"></a>Aplicación y base de datos en la misma región
Asegúrese de que **todas las dependencias están en la misma región** al implementar la aplicación en Azure. La propagación de instancias entre regiones o zonas de disponibilidad crea una latencia de red, lo que puede afectar al rendimiento general de la aplicación. 

### <a name="keep-your-mysql-server-secure"></a>Mantenimiento de la seguridad del servidor MySQL
El servidor MySQL debe configurarse para que sea [seguro](https://docs.microsoft.com/azure/mysql/concepts-security) y para que no se pueda acceder a él de forma pública. Use cualquiera de estas opciones para proteger el servidor: 
- [Reglas de firewall](https://docs.microsoft.com/azure/mysql/concepts-firewall-rules)
- [Redes virtuales](https://docs.microsoft.com/azure/mysql/concepts-data-access-and-security-vnet) 
- [Private Link](https://docs.microsoft.com/azure/mysql/concepts-data-access-security-private-link)

Por seguridad, siempre debe conectarse al servidor MySQL a través de **SSL** y configurar el servidor MySQL y la aplicación para que usen **TLS1.2**. Vea [Conectividad SSL/TLS](https://docs.microsoft.com/azure/mysql/concepts-ssl-connection-security). 

### <a name="tune-your-server-parameters"></a>Ajuste de los parámetros del servidor
En el caso de cargas de trabajo de lectura intensiva que ajustan estos parámetros, "tmp_table_size y max_heap_table_size" pueden ayudar realizar la optimización para un mayor rendimiento. Para calcular los valores necesarios para tmp_table_size y max_heap_table_size, examine los valores de memoria total por conexión y la memoria base. La suma de los parámetros de memoria por conexión, excluido tmp_table_size, junto con la memoria base, da como resultado la memoria total del servidor.

Para calcular el máximo tamaño posible de tmp_table_size y max_heap_table_size, utilice la siguiente fórmula:

```(total memory - (base memory + (sum of per-connection memory * # of connections)) / # of connections```

>[!NOTE]
> La memoria total indica la cantidad total de memoria que el servidor tiene en los núcleos virtuales aprovisionados.  Por ejemplo, en un servidor de Azure Database for MySQL con dos núcleos virtuales de uso general, la memoria total será 5 GB * 2.  Puede encontrar más información sobre la memoria para cada nivel en la documentación del [plan de tarifa](https://docs.microsoft.com/azure/mysql/concepts-pricing-tiers).
> La memoria base indica las variables de memoria, como query_cache_size y innodb_buffer_pool_size, que MySQL inicializará y asignará al iniciar el servidor.  La memoria por conexión, como sort_buffer_size y join_buffer_size, es la memoria que se asigna solo cuando una consulta lo requiere.

### <a name="create-a-non-admin-user"></a>Creación de un usuario que no sea administrador 
[Cree usuarios que no sean administradores](https://docs.microsoft.com/azure/mysql/howto-create-users) para cada una de las bases de datos. Normalmente, los nombres de usuario se identifican como nombres de base de datos.

### <a name="resetting-your-password"></a>Restablecimiento de la contraseña
Puede [restablecer la contraseña](https://docs.microsoft.com/azure/mysql/howto-create-manage-server-portal#update-admin-password) del servidor MySQL en Azure Portal. 

El restablecimiento de la contraseña del servidor de una base de datos de producción puede desactivar la aplicación. Es un buen patrón restablecer la contraseña de cualquier carga de trabajo de producción en horas de poca actividad para minimizar el impacto en los usuarios finales de la aplicación.

## <a name="performance-and-resiliency"></a>Rendimiento y resistencia 
Estas son algunas herramientas y patrones que puede usar para depurar problemas de rendimiento con la aplicación.

### <a name="enable-slow-query-logs-identify-performance-issues"></a>Habilitar registros de consultas lentas para identificar problemas de rendimiento
Puede habilitar los [registros de consultas lentas](https://docs.microsoft.com/azure/mysql/concepts-server-logs) y los [registros de auditoría](https://docs.microsoft.com/azure/mysql/concepts-audit-logs) en el servidor. El análisis de consultas lentas puede ayudar a identificar cuellos de botella que afectan al rendimiento a fin de solucionar el problema. 

Los registros de auditoría también están disponibles mediante los registros de diagnóstico de Azure en los registros de Azure Monitor, Event Hubs y la cuenta de almacenamiento. Vea [Solución de problemas relacionados con el rendimiento de consultas](https://docs.microsoft.com/azure/mysql/howto-troubleshoot-query-performance).

### <a name="use-connection-pooling"></a>Use agrupar conexiones
La administración de conexiones de base de datos puede tener un impacto significativo en el rendimiento de la aplicación en su conjunto. Para optimizar el rendimiento, debe reducir el número de veces que se establecen las conexiones y el tiempo para establecer las conexiones en las rutas de acceso clave del código.  Use [Agrupación de conexiones](https://docs.microsoft.com/azure/mysql/concepts-connectivity#access-databases-by-using-connection-pooling-recommended) para conectarse a Azure Database for MySQL, a fin de mejorar la resistencia y el rendimiento. 

[ProxySQL](https://proxysql.com/), que es un agrupador de conexiones, se puede usar de forma eficaz para administrar conexiones. Con un agrupador de conexiones, puede reducir las conexiones inactivas y reutilizar las conexiones existentes para evitar problemas. Vea [Configuración de ProxySQL](https://techcommunity.microsoft.com/t5/azure-database-for-mysql/connecting-efficiently-to-azure-database-for-mysql-with-proxysql/ba-p/1279842) para obtener más información. 

### <a name="retry-logic-to-handle-transient-errors"></a>Lógica de reintento para controlar errores transitorios
La aplicación podría experimentar [errores transitorios](https://docs.microsoft.com/azure/mysql/concepts-connectivity#handling-transient-errors) donde las conexiones a la base de datos se caen o se pierden de manera intermitente. En tales situaciones, el servidor se pone en funcionamiento después de uno o dos reintentos en 5-10 segundos. 

Un buen patrón para seguir con el reintento es esperar 5 segundos antes del primer reintento y después seguir con cada reintento aumentando la espera gradualmente hasta 60 segundos. Limite el número máximo de reintentos como momento en que la aplicación considera que se produjo un error en la operación, para poder investigar más. Vea [Cómo solucionar los errores de conexión](https://docs.microsoft.com/azure/mysql/howto-troubleshoot-common-connection-issues) para obtener más información. 

### <a name="enable-read-replication-to-mitigate-failovers"></a>Habilitar la replicación de lectura para mitigar las conmutaciones por error
Puede usar la [Replicación de datos internos](https://docs.microsoft.com/azure/mysql/howto-data-in-replication) en los escenarios de conmutación por error. Cuando se usan réplicas de lectura, no se produce una conmutación por error automatizada entre servidores maestros y de réplicas. Observará un retraso entre el servidor maestro y el de réplica, ya que la replicación es asincrónica. El retraso de red puede verse afectado por muchos factores, como lo pesada que sea la carga de trabajo que se ejecuta en el servidor maestro y la latencia entre los centros de datos. En la mayoría de los casos, el retraso de la réplica oscila entre unos segundos y un par de minutos.

## <a name="database-deployment"></a>Implementación de bases de datos 

### <a name="configure-azure-database-for-mysql-task-in-your-cicd-deployment-pipeline"></a>Configuración de la tarea de Azure Database for MySQL en la canalización de implementación de CI/CD
En ocasiones, es necesario implementar los cambios en la base de datos. En tales casos, puede crear el uso de la integración continua (CI) y la entrega continua (CD) a través de [canalizaciones de Azure](https://azure.microsoft.com/services/devops/pipelines/) y usar una tarea para que el [servidor MySQL](https://docs.microsoft.com/azure/devops/pipelines/tasks/deploy/azure-mysql-deployment?view=azure-devops) actualice la base de datos mediante la ejecución de un script personalizado en la base de datos.

### <a name="manual-database-deployment"></a>Implementación manual de la base de datos 
Durante la implementación manual de la base de datos, este es un buen patrón para minimizar el tiempo de inactividad o reducir el riesgo de que se produzca un error en la implementación: 

1. Cree una copia de la base de datos de producción en una nueva base de datos mediante [mysqldump](https://dev.mysql.com/doc/refman/8.0/en/mysqldump.html) o [MySQL Workbench](https://dev.mysql.com/doc/workbench/en/wb-admin-export-import-management.html) 
2. Actualice la nueva base de datos con los nuevos cambios de esquema o las actualizaciones necesarias de la base de datos. 
3. Ponga la base de datos de producción en estado de solo lectura. No debe tener operaciones de escritura en la base de datos de producción hasta que se complete la implementación. 
4. Pruebe la aplicación con la base de datos recién actualizada del paso 1.
5. Implemente los cambios de la aplicación y asegúrese de que la aplicación usa ahora la nueva base de datos que tiene las actualizaciones más recientes. 
6. Mantenga la antigua base de datos de producción para poder revertir los cambios. Después, puede evaluar para eliminar la base de datos de producción antigua o exportarla en Azure Storage, si es necesario. 

>[!NOTE]
>  - Si la aplicación es como una aplicación de comercio electrónico en la que es posible que no pueda ponerla en estado de solo lectura, implemente los cambios directamente en la base de datos de producción después de realizar una copia de seguridad.  Estos cambios deben realizarse durante las horas de poca actividad con poco tráfico a la aplicación, para minimizar el impacto, ya que algunos usuarios pueden experimentar solicitudes con error. 
>  - Asegúrese de que el código de aplicación también controla las solicitudes con error.

### <a name="use-mysql-native-metrics-to-see-if-your-workload-is-exceeding-in-memory-temporary-table-sizes"></a>Uso de las métricas nativas de MySQL para ver si la carga de trabajo supera los tamaños de tablas temporales en memoria
Con una carga de trabajo con mucha lectura, las consultas que se ejecutan en el servidor MySQL pueden superar los tamaños de tablas temporales en memoria. Puede hacer que el servidor cambie a escribir tablas temporales en el disco, lo que afecta al rendimiento de la aplicación. Para determinar si el servidor está escribiendo en el disco como resultado de superar el tamaño de la tabla temporal, examine las métricas siguientes:

```
show global status like 'created_tmp_disk_tables';
show global status like 'created_tmp_tables';
```
La métrica de created_tmp_disk_tables indica el número de tablas que se crearon en el disco, mientras que la métrica de created_tmp_table indica cuántas tablas temporales se deben formar en la memoria en función de la carga de trabajo. Para determinar si la ejecución de una consulta específica usará tablas temporales, ejecute la explicación en la consulta. Los detalles de la columna "extra" indican "Usando temporal" si la consulta se ejecutará con tablas temporales.

Para calcular el porcentaje de la carga de trabajo con consultas que desbordan los discos, use los valores de las métricas en la fórmula siguiente:

```(created_tmp_disk_tables / (created_tmp_disk_tables + created_tmp_tables)) * 100```

Idealmente, este porcentaje debe ser inferior al 25 %. Si ve que el porcentaje es del 25 % o superior, se recomienda modificar dos parámetros de servidor, tmp_table_size y max_heap_table_si.


## <a name="database-schema-and-queries"></a>Consultas y esquema de la base de datos

Estos son algunos consejos y trucos que hay que tener en cuenta al compilar el esquema de la base de datos y las consultas.

### <a name="always-use-the-right-datatype-for-your--table-columns"></a>Usar siempre el tipo de datos correcto para las columnas de la tabla
El uso de los tipos de datos correctos en función del tipo de datos que desea almacenar puede optimizar el almacenamiento y reducir los errores que pueden producirse debido a los tipos de datos incorrectos.

### <a name="use-indexes"></a>Uso de índices
Para evitar consultas lentas, puede utilizar índices. Los índices pueden ayudar a buscar filas con columnas específicas rápidamente. Consulte [Cómo usar índices en MySQL](https://dev.mysql.com/doc/refman/8.0/en/mysql-indexes.html).

### <a name="explain-your-select-queries"></a>EXPLICACIÓN de las consultas SELECT
Use [EXPLAIN](https://dev.mysql.com/doc/refman/8.0/en/explain.html) para obtener información sobre lo que hace MySQL para ejecutar la consulta. Esto puede ayudarle a detectar cuellos de botella o problemas con la consulta. Vea [Uso de EXPLAIN para solucionar problemas relacionados con el rendimiento de consultas](https://docs.microsoft.com/azure/mysql/howto-troubleshoot-query-performance).


