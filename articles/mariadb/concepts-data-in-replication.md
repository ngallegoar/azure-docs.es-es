---
title: 'Replicación de datos de entrada: Azure Database for MariaDB'
description: Conozca más información acerca de la replicación de datos de entrada para sincronizar datos de un servidor externo con el servicio Azure Database for MariaDB.
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.topic: conceptual
ms.date: 3/18/2020
ms.openlocfilehash: 66e280f20109967f029a14e368fdb0aeea269aad
ms.sourcegitcommit: f5580dd1d1799de15646e195f0120b9f9255617b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/29/2020
ms.locfileid: "91536620"
---
# <a name="replicate-data-into-azure-database-for-mariadb"></a>Replicar datos en Azure Database for MariaDB

La característica Replicación de datos de entrada permite sincronizar los datos de un servidor de MariaDB que se ejecuta de forma local, de máquinas virtuales o de servicios de base de datos hospedados por otros proveedores de nube con el servicio Azure Database for MariaDB. La Replicación de datos de entrada se basa en la replicación según la posición del archivo de registro binario (binlog), que es nativa de MariaDB. Para obtener más información sobre la replicación de binlog, consulte la [introducción a la replicación de binlog](https://mariadb.com/kb/en/library/replication-overview/).

## <a name="when-to-use-data-in-replication"></a>Cuándo utilizar Data-in Replication
Los escenarios principales para considerar el uso de Data-in Replication son:

- **Sincronización de datos híbrida:** con la Replicación de datos de entrada, se pueden mantener los datos sincronizados entre los servidores locales y Azure Database for MariaDB. Esta sincronización resulta útil para crear aplicaciones híbridas. Este método resulta atractivo cuando se tiene un servidor de base de datos local existente, pero quiere mover los datos a una región más cercana de los usuarios finales.
- **Sincronización de multi nube:** para soluciones de nube complejas, use la Replicación de datos de entrada para sincronizar datos entre Azure Database for MariaDB y distintos proveedores de nube, incluidas las máquinas virtuales y servicios de base de datos hospedados en dichas nubes.

## <a name="limitations-and-considerations"></a>Limitaciones y consideraciones

### <a name="data-not-replicated"></a>Datos no replicados
La [*base de datos del sistema mysql*](https://mariadb.com/kb/en/library/the-mysql-database-tables/) del servidor de origen no se replica. No se replican los cambios en las cuentas y los permisos en el servidor de origen. Si crea una cuenta en el servidor de origen y esta cuenta debe tener acceso al servidor de réplica, cree manualmente la misma cuenta en el servidor de réplica. Para reconocer qué tablas se encuentran en la base de datos del sistema, vea la [documentación de MariaDB](https://mariadb.com/kb/en/library/the-mysql-database-tables/).

### <a name="requirements"></a>Requisitos
- La versión del servidor de origen debe ser al menos la versión 10.2 de MariaDB.
- Las versiones del servidor de origen y de réplica deben ser las mismas. Por ejemplo, ambas deben ser la versión 10.2 de MariaDB.
- Cada tabla debe tener una clave principal.
- El servidor de origen debe usar el motor InnoDB.
- El usuario debe tener permisos para configurar el registro binario y crear nuevos usuarios en el servidor de origen.
- Si el servidor de origen tiene SSL habilitado, asegúrese de que el certificado de entidad de certificación de SSL proporcionado para el dominio se haya incluido en el procedimiento almacenado `mariadb.az_replication_change_master`. Consulte los [ejemplos](https://docs.microsoft.com/azure/mariadb/howto-data-in-replication#link-the-master-and-replica-servers-to-start-data-in-replication) siguientes y el parámetro `master_ssl_ca`.
- Asegúrese de que la dirección IP del servidor de origen se ha agregado a las reglas de firewall del servidor de réplica de Azure Database for MariaDB. Actualice las reglas de firewall mediante [Azure Portal](https://docs.microsoft.com/azure/mariadb/howto-manage-firewall-portal) o la [CLI de Azure](https://docs.microsoft.com/azure/mariadb/howto-manage-firewall-cli).
- Asegúrese de que la máquina que hospeda el servidor de origen permite el tráfico entrante y saliente en el puerto 3306.
- Asegúrese de que el servidor de origen tenga una **dirección IP pública**, de que el DNS sea accesible públicamente o de que tenga un nombre de dominio completo (FQDN).

### <a name="other"></a>Otros
- La Replicación de datos de entrada solo se admite en los planes de tarifa De uso general y Optimizada para memoria.

## <a name="next-steps"></a>Pasos siguientes
- Información sobre cómo [configurar la replicación de datos internos](howto-data-in-replication.md).
