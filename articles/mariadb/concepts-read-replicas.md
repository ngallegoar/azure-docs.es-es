---
title: 'Réplicas de lectura: Azure Database for MariaDB'
description: 'Obtenga información sobre las réplicas de lectura en Azure Database for MariaDB: elección de regiones, creación de réplicas, conexión a réplicas, supervisión de la replicación y detención de la replicación.'
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.topic: conceptual
ms.date: 3/18/2020
ms.openlocfilehash: 39edaa32b0695f4ab83206cd5701629f12295a0f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79527918"
---
# <a name="read-replicas-in-azure-database-for-mariadb"></a>Réplicas de lectura en Azure Database for MariaDB

La característica de réplica de lectura permite replicar datos de un servidor Azure Database for MariaDB en un servidor de solo lectura. Puede crear hasta cinco réplicas desde el servidor maestro. Las réplicas se actualizan asincrónicamente mediante la tecnología de replicación basada en la posición de los archivos del registro binario (binlog) del motor de MariaDB. Para obtener más información sobre la replicación de binlog, consulte la [introducción a la replicación de binlog](https://mariadb.com/kb/en/library/replication-overview/).

Las réplicas son nuevos servidores que se administran de forma similar a los servidores de Azure Database for MariaDB normales. En cada réplica de lectura, se le cobra por el proceso aprovisionado en núcleos virtuales y el almacenamiento aprovisionado en GB/mes.

Para más información acerca de la replicación de GTID, consulte la [documentación de replicación de MariaDB](https://mariadb.com/kb/en/library/gtid/).

## <a name="when-to-use-a-read-replica"></a>Casos en los que utilizar las réplicas de lectura

La finalidad de la característica de réplica de lectura es ayudar a mejorar el rendimiento y la escala de las cargas de trabajo que conllevan un gran número de operaciones de lectura. Las cargas de trabajo de lectura se pueden aislar en las réplicas, mientras que las cargas de trabajo de escritura se pueden dirigir al servidor maestro.

Un caso habitual es que las cargas de trabajo de análisis e inteligencia empresarial utilicen la réplica de lectura como origen de datos para los informes.

Dado que las réplicas son de solo lectura, no reducen directamente las cargas de capacidad de escritura en el servidor maestro. Esta característica no está destinada a cargas de trabajo intensivas de escritura.

Esta característica de réplica de lectura utiliza la replicación asincrónica nativa. La característica no está diseñada para escenarios de replicación sincrónica. Habrá un retraso medible entre el servidor maestro y la réplica. Los datos de la réplica se vuelven finalmente coherentes con los datos del servidor maestro. Use esta característica con cargas de trabajo que puedan admitir este retraso.

## <a name="cross-region-replication"></a>Replicación entre regiones
Puede crear una réplica de lectura en una región distinta del servidor maestro. La replicación entre regiones puede ser útil para escenarios como el planeamiento de la recuperación ante desastres o la incorporación de datos más cerca de los usuarios.

> [!NOTE]
> La replicación entre regiones está en versión preliminar.

Puede tener un servidor maestro en cualquier [región de Azure Database for MariaDB](https://azure.microsoft.com/global-infrastructure/services/?products=mariadb).  Un servidor maestro puede tener una réplica emparejada en su región o en las regiones de la réplica universal. En la imagen siguiente se muestran las regiones de réplica disponibles en función de la región maestra.

[ ![Regiones de réplica de lectura](media/concepts-read-replica/read-replica-regions.png)](media/concepts-read-replica/read-replica-regions.png#lightbox)

### <a name="universal-replica-regions"></a>Regiones de réplica universal
Puede crear una réplica de lectura en cualquiera de las siguientes regiones, con independencia de dónde se encuentre el servidor maestro. Entre las regiones de réplica universales admitidas se incluyen:

Este de Australia, Sudeste de Australia, Centro de EE. UU., Este de Asia, Este de EE. UU. 2, Este de Japón, Oeste de Japón, Centro de Corea del Sur, Sur de Corea del Sur, Centro y norte de EE. UU., Norte de Europa, Centro y Sur de EE. UU., Sudeste de Asia, Sur de Reino Unido, Oeste de Reino Unido, Oeste de Europa, Oeste de EE. UU.

*Oeste de EE  UU. 2 no está disponible temporalmente como ubicación de réplica entre regiones.

### <a name="paired-regions"></a>Regiones emparejadas
Además de las regiones de réplica universal, puede crear una réplica de lectura en la región emparejada de Azure del servidor maestro. Si no conoce el par de la región, puede obtener más información en el [artículo sobre regiones emparejadas de Azure](../best-practices-availability-paired-regions.md).

Si usa réplicas entre regiones para planear la recuperación ante desastres, se recomienda que cree la réplica en la región emparejada en lugar de en una de las otras regiones. Las regiones emparejadas evitan actualizaciones simultáneas y priorizan el aislamiento físico y la residencia de datos.  

Sin embargo, existen limitaciones que deben considerarse: 

* Disponibilidad regional: Azure Database for MariaDB está disponible en Oeste de EE. UU. 2, Centro de Francia, Norte de Emiratos Árabes Unidos y Centro de Alemania. Sin embargo, sus regiones emparejadas no están disponibles.
    
* Pares unidireccionales: Algunas regiones de Azure se emparejan solo en una dirección. Estas regiones incluyen Oeste de la India, Sur de Brasil y US Gov Virginia. 
   Esto significa que un servidor maestro de Oeste de la India puede crear una réplica en Sur de la India. Sin embargo, un servidor maestro de Sur de la India no puede crear una réplica en Oeste de la India. Esto es debido a que la región secundaria del Oeste de la India es Sur de la India, pero la región secundaria de esta última no es Oeste de la India.

## <a name="create-a-replica"></a>Creación de una réplica

Si un servidor maestro no tiene ningún servidor de réplica existente, el maestro se reiniciará en primer lugar para prepararse para la replicación.

Cuando se inicia el flujo de trabajo de creación de la réplica, se crea un servidor de Azure Database for MariaDB en blanco. El nuevo servidor se rellena con los datos que estaban en el servidor maestro. El tiempo que se tarda en crear la réplica depende de la cantidad de datos en el servidor maestro y del tiempo desde la última copia de seguridad completa semanal. Puede oscilar desde unos minutos hasta varias horas.

> [!NOTE]
> Si no tiene alertas de almacenamiento en sus servidores, se recomienda que lo haga. La alerta le informa cuando un servidor está alcanzando el límite de almacenamiento, lo que afectará a la replicación.

Aprenda a [crear una réplica de lectura en Azure Portal](howto-read-replicas-portal.md).

## <a name="connect-to-a-replica"></a>Conexión a una réplica

Durante la creación, una réplica hereda las reglas de firewall o el punto de conexión de servicio de red virtual del servidor maestro. Posteriormente, estas reglas son independientes de las del servidor maestro.

La réplica hereda su cuenta de administrador del servidor maestro. Todas las cuentas de usuario existentes en el servidor se replican en las réplicas de lectura. Solo se puede conectar a una réplica de lectura utilizando las cuentas de usuario disponibles en el servidor maestro.

Puede conectarse a la réplica mediante su nombre de host y una cuenta de usuario válida, igual que haría en un servidor Azure Database for MariaDB normal. En un servidor denominado **myreplica** con el nombre de usuario administrador **myadmin**, puede conectarse a la réplica mediante la CLI de mysql:

```bash
mysql -h myreplica.mariadb.database.azure.com -u myadmin@myreplica -p
```

Cuando se le solicite, escriba la contraseña de la cuenta de usuario.

## <a name="monitor-replication"></a>Supervisión de la replicación

Azure Database for MariaDB proporciona la métrica de **retraso de replicación en segundos** en Azure Monitor. Esta métrica está disponible únicamente para las réplicas.

Esta métrica se calcula utilizando la métrica `seconds_behind_master` disponible en el comando `SHOW SLAVE STATUS` de MariaDB.

Establezca una alerta que le informe cuando el retraso de replicación alcance un valor que no sea aceptable para la carga de trabajo.

## <a name="stop-replication"></a>Detención replicación

Puede decidir detener la replicación entre un servidor maestro y una réplica. Una vez que se ha detenido la replicación entre un servidor maestro y una réplica de lectura, la réplica se convierte en un servidor independiente. Los datos del servidor independiente son los datos que estaban disponibles en la réplica en el momento en que se inició el comando para detener la replicación. El servidor independiente no alcanza al servidor maestro.

Si decide detener la replicación en una réplica, perderá todos los vínculos con su servidor maestro anterior y otras réplicas. No se produce ninguna conmutación por error automatizada entre un servidor maestro y su réplica.

> [!IMPORTANT]
> Este servidor independiente no puede volver a convertirse en una réplica.
> Asegúrese de que la réplica tenga todos los datos que necesita antes de detener la replicación en una réplica de lectura.

Aprenda a [detener la replicación en una réplica](howto-read-replicas-portal.md).

## <a name="considerations-and-limitations"></a>Consideraciones y limitaciones

### <a name="pricing-tiers"></a>Planes de tarifa

Actualmente, las réplicas de lectura solo están disponibles en los planes de tarifa de uso general y optimizados para memoria.

### <a name="master-server-restart"></a>Reinicio del servidor maestro

Cuando se crea una réplica para un servidor maestro que no tiene réplicas existentes, el maestro se reiniciará en primer lugar para prepararse para la replicación. Téngalo en cuenta y realice estas operaciones durante un período de poca actividad.

### <a name="new-replicas"></a>Nuevas réplicas

Las réplicas de lectura se crean como nuevos servidores Azure Database for MariaDB. Un servidor no puede volver a convertirse en una réplica. No se puede crear una réplica de otra réplica de lectura.

### <a name="replica-configuration"></a>Configuración de réplicas

Las réplicas se crean con la misma configuración de servidor que el servidor maestro. Después de crear una réplica, se pueden cambiar varias configuraciones independientemente del servidor maestro: generación de proceso, núcleos virtuales, almacenamiento, período de retención de copia de seguridad y versión del motor de MariaDB. El plan de tarifa también se puede cambiar de forma independiente, excepto si es con origen o destino en el nivel Básico.

> [!IMPORTANT]
> Antes de actualizar la configuración de un servidor maestro con nuevos valores, actualice la configuración de las réplicas a valores iguales o mayores. Esta acción garantiza que la réplica puede hacer frente a los cambios realizados en el servidor maestro.

Las reglas de firewall, las reglas de red virtual y la configuración de parámetros se heredan del servidor maestro a la réplica cuando se crea la réplica. Después, las reglas de la réplica son independientes.

### <a name="stopped-replicas"></a>Réplicas detenidas

Si detiene la replicación entre un servidor maestro y una réplica de lectura, la réplica detenida se convierte en un servidor independiente que acepta operaciones de lectura y escritura. Este servidor independiente no puede volver a convertirse en una réplica.

### <a name="deleted-master-and-standalone-servers"></a>Servidores maestro e independiente eliminados

Al eliminar el servidor maestro, la replicación se detiene en todas las réplicas de lectura. Estas réplicas se convierten automáticamente en servidores independientes y pueden aceptar operaciones de lectura y de escritura. El propio servidor maestro se elimina.

### <a name="user-accounts"></a>Cuentas de usuario

Los usuarios del servidor principal se replican en las réplicas de lectura. Solo se puede conectar a una réplica de lectura utilizando las cuentas de usuario disponibles en el servidor maestro.

### <a name="server-parameters"></a>Parámetros del servidor

Para evitar que los datos dejen de estar sincronizados y evitar posibles pérdidas o daños en los datos, se bloquean algunos parámetros del servidor para que no se actualicen mientras se usan réplicas de lectura.

Los siguientes parámetros de servidor están bloqueados tanto en el servidor maestro como en los de réplica:
- [`innodb_file_per_table`](https://mariadb.com/kb/en/library/innodb-system-variables/#innodb_file_per_table) 
- [`log_bin_trust_function_creators`](https://mariadb.com/kb/en/library/replication-and-binary-log-system-variables/#log_bin_trust_function_creators)

El parámetro [`event_scheduler`](https://mariadb.com/kb/en/library/server-system-variables/#event_scheduler) está bloqueado en los servidores de réplica.

### <a name="other"></a>Otros

- No permite crear réplicas de réplicas.
- Las tablas en memoria pueden provocar que las réplicas dejen de sincronizarse. Esto es una limitación de la tecnología de replicación de MariaDB.
- Asegúrese de que las tablas del servidor maestro tienen claves principales. La falta de claves principales puede generar una latencia de replicación entre la tabla principal y la réplica.

## <a name="next-steps"></a>Pasos siguientes

- Obtenga información sobre cómo [crear y administrar réplicas de lectura mediante Azure Portal](howto-read-replicas-portal.md).
- Aprenda a [crear y administrar réplicas de lectura mediante la CLI de Azure y API REST](howto-read-replicas-cli.md).
