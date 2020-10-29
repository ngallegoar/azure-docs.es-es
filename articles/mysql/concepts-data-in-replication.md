---
title: Replicación de datos de entrada en Azure Database for MySQL
description: Conozca más información acerca de la replicación de datos de entrada para sincronizar datos de un servidor externo con el servicio Azure Database for MySQL.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: conceptual
ms.date: 8/7/2020
ms.openlocfilehash: e84f0c9beaee8a755499467925d28a83ba3139fc
ms.sourcegitcommit: d767156543e16e816fc8a0c3777f033d649ffd3c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/26/2020
ms.locfileid: "92544059"
---
# <a name="replicate-data-into-azure-database-for-mysql"></a>Replicar datos en Azure Database for MySQL

Replicación de datos de entrada permite sincronizar datos de un servidor MySQL externo en el servicio Azure Database for MySQL. El servidor externo puede ser local, de máquinas virtuales o un servicio de base de datos hospedado por otros proveedores de nube. Data-in Replication se basa en la replicación según la posición del archivo de registro binario (binlog), que es nativa de MySQL. Para obtener más información acerca de la replicación de binlog, consulte la [Introducción a la replicación de binlog de MySQL](https://dev.mysql.com/doc/refman/5.7/en/binlog-replication-configuration-overview.html). 

## <a name="when-to-use-data-in-replication"></a>Cuándo utilizar Data-in Replication
Los escenarios principales para considerar el uso de Data-in Replication son:

- **Sincronización de datos híbridos:** con Replicación de datos de entrada, se pueden mantener los datos sincronizados entre los servidores locales y Azure Database for MySQL. Esta sincronización resulta útil para crear aplicaciones híbridas. Este método resulta atractivo cuando se tiene un servidor de base de datos local existente, pero quiere mover los datos a una región más cercana a los usuarios finales.
- **Sincronización de multi-nube:** para soluciones de nube complejas, use Replicación de datos de entrada para sincronizar datos entre Azure Database for MySQL y distintos proveedores de nube, incluidas las máquinas virtuales y los servicios de base de datos hospedados en dichas nubes.
 
Para los escenarios de migración, use [Azure Database Migration Service](https://azure.microsoft.com/services/database-migration/) (DMS).

## <a name="limitations-and-considerations"></a>Limitaciones y consideraciones

### <a name="data-not-replicated"></a>Datos no replicados
La [*base de datos del sistema mysql*](https://dev.mysql.com/doc/refman/5.7/en/system-schema.html) del servidor de origen no se replica. Los cambios en las cuentas y en los permisos del servidor de origen no se replican. Si crea una cuenta en el servidor de origen y esta cuenta necesita acceder al servidor de réplica, cree manualmente la misma cuenta en el servidor de réplica. Para reconocer qué tablas se encuentran en la base de datos del sistema, vea el [manual de MySQL](https://dev.mysql.com/doc/refman/5.7/en/system-schema.html).

### <a name="filtering"></a>Filtrado
Para omitir la replicación de tablas del servidor de origen (hospedado de forma local, en máquinas virtuales o en un servicio de base de datos hospedado por otros proveedores de nube), se admite el parámetro `replicate_wild_ignore_table`. También tiene la opción de actualizar este parámetro en el servidor de réplica hospedado en Azure mediante [Azure Portal](howto-server-parameters.md) o la [CLI de Azure](howto-configure-server-parameters-using-cli.md).

Consulte la [documentación de MySQL](https://dev.mysql.com/doc/refman/8.0/en/replication-options-replica.html#option_mysqld_replicate-wild-ignore-table) para más información acerca de este parámetro.

### <a name="requirements"></a>Requisitos
- La versión del servidor de origen debe ser al menos MySQL 5.6. 
- Las versiones del servidor de origen y de réplica deben ser las mismas. Por ejemplo, ambas deben ser MySQL versión 5.6 o ambas deben ser MySQL versión 5.7.
- Cada tabla debe tener una clave principal.
- El servidor de origen debe usar el motor InnoDB de MySQL.
- El usuario debe tener permisos para configurar el registro binario y crear nuevos usuarios en el servidor de origen.
- Si el servidor de origen tiene SSL habilitado, asegúrese de que el certificado de entidad de certificación de SSL proporcionado para el dominio se haya incluido en el procedimiento almacenado `mysql.az_replication_change_master`. Consulte los [ejemplos](./howto-data-in-replication.md#link-source-and-replica-servers-to-start-data-in-replication) siguientes y el parámetro `master_ssl_ca`.
- Asegúrese de que la dirección IP del servidor de origen se ha agregado a las reglas de firewall del servidor de réplica de Azure Database for MySQL. Actualice las reglas de firewall mediante [Azure Portal](./howto-manage-firewall-using-portal.md) o la [CLI de Azure](./howto-manage-firewall-using-cli.md).
- Asegúrese de que la máquina que hospeda el servidor de origen permite el tráfico entrante y saliente en el puerto 3306.
- Asegúrese de que el servidor de origen tenga una **dirección IP pública** , de que el DNS sea accesible públicamente o de que tenga un nombre de dominio completo (FQDN).

### <a name="other"></a>Otros
- La Replicación de datos de entrada solo se admite en los planes de tarifa De uso general y Optimizada para memoria.
- No se admiten identificadores de transacción global (GTID).

## <a name="next-steps"></a>Pasos siguientes
- Información acerca de cómo [configurar la replicación de datos internos](howto-data-in-replication.md)
- Información acerca de cómo [replicar en Azure con réplicas de lectura](concepts-read-replicas.md)
- Información acerca de cómo [migrar datos con un tiempo de inactividad mínimo mediante DMS](howto-migrate-online.md)