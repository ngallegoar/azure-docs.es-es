---
title: 'Procedimientos almacenados de administración: Azure Database for MySQL'
description: Obtenga información acerca de qué procedimientos almacenados de Azure Database for MySQL son útiles para ayudarle a configurar la replicación de datos de entrada y a establecer la zona horaria y a terminar consultas.
author: savjani
ms.author: pariks
ms.service: mysql
ms.topic: conceptual
ms.date: 3/18/2020
ms.openlocfilehash: 7a1aa061bb8c8be3a676e0e5bb690b2a9749b6c8
ms.sourcegitcommit: 6ab718e1be2767db2605eeebe974ee9e2c07022b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/12/2020
ms.locfileid: "94536139"
---
# <a name="azure-database-for-mysql-management-stored-procedures"></a>Procedimientos almacenados de administración de Azure Database for MySQL

Los procedimientos almacenados están disponibles en servidores Azure Database for MySQL para ayudar a administrar el servidor MySQL. Esto incluye la administración de las conexiones del servidor, las consultas y la configuración de replicación de datos de entrada.  

## <a name="data-in-replication-stored-procedures"></a>Procedimientos almacenados de replicación de datos de entrada

Data-in Replication permite sincronizar los datos de un servidor de MySQL que se ejecuta de forma local, en máquinas virtuales o servicios de base de datos hospedados por otros proveedores de nube en el servicio de Azure Database for MySQL.

Los siguientes procedimientos almacenados se usan para configurar o quitar la relación de Replicación de datos de entrada entre un origen y una réplica.

|**Nombre del procedimiento almacenado**|**Parámetros de entrada**|**Parámetros de salida**|**Nota sobre el uso**|
|-----|-----|-----|-----|
|*mysql.az_replication_change_master*|master_host<br/>master_user<br/>master_password<br/>master_host<br/>master_log_file<br/>master_log_file<br/>master_ssl_ca|N/D|Para transferir los datos con el modo SSL, transfiera el contexto del certificado de entidad de certificación en el parámetro master_ssl_ca. </br><br>Para transferir datos sin SSL, transfiera una cadena vacía en el parámetro master_ssl_ca.|
|*mysql.az_replication _start*|N/D|N/D|Inicia la replicación.|
|*mysql.az_replication _stop*|N/D|N/D|Detiene la replicación.|
|*mysql.az_replication _remove_master*|N/D|N/D|Quita la relación de replicación entre el origen y la réplica.|
|*mysql.az_replication_skip_counter*|N/D|N/D|Emite un error de replicación.|

Para configurar la relación de Replicación de datos de entrada entre un origen y una réplica en Azure Database for MySQL, vea [Configuración de Replicación de datos de entrada](howto-data-in-replication.md).

## <a name="other-stored-procedures"></a>Otros procedimientos almacenados

Los siguientes procedimientos almacenados están disponibles en servidores Azure Database for MySQL para administrar el servidor.

|**Nombre del procedimiento almacenado**|**Parámetros de entrada**|**Parámetros de salida**|**Nota sobre el uso**|
|-----|-----|-----|-----|
|*mysql.az_kill*|processlist_id|N/D|Equivalente al comando [`KILL CONNECTION`](https://dev.mysql.com/doc/refman/8.0/en/kill.html). Finalizará la conexión asociada con el processlist_id proporcionado después de finalizar cualquier instrucción que la conexión esté ejecutando.|
|*mysql.az_kill_query*|processlist_id|N/D|Equivalente al comando [`KILL QUERY`](https://dev.mysql.com/doc/refman/8.0/en/kill.html). Finalizará la instrucción que la conexión está ejecutando actualmente. Deja la propia conexión activa.|
|*mysql.az_load_timezone*|N/D|N/D|Carga las tablas de zona horaria para permitir que el parámetro `time_zone` se establezca en valores con nombre (por ejemplo, "US/Pacific").|

## <a name="next-steps"></a>Pasos siguientes
- Aprenda a configurar [Replicación de datos de entrada](howto-data-in-replication.md)
- Aprenda a usar las [tablas de zona horaria](howto-server-parameters.md#working-with-the-time-zone-parameter)
