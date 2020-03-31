---
title: 'Copia de seguridad y restauración en Azure Database for PostgreSQL: servidor único'
description: Obtenga información acerca de cómo realizar copias de seguridad y restaurar automáticamente su servidor de Azure Database for PostgreSQL con un único servidor.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 02/25/2020
ms.openlocfilehash: 3e6dfd5882e49ad903e8cff6f0ec7f3d6bd4a8b7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "77619626"
---
# <a name="backup-and-restore-in-azure-database-for-postgresql---single-server"></a>Copia de seguridad y restauración en Azure Database for PostgreSQL con un único servidor

Azure Database for PostgreSQL crea automáticamente copias de seguridad del servidor y las almacena en el almacenamiento con redundancia local o con redundancia geográfica configurado por el usuario. Las copias de seguridad pueden utilizarse para restaurar el servidor a un momento dado. Las copias de seguridad y las restauraciones son una parte esencial de cualquier estrategia de continuidad del negocio, ya que protegen los datos frente a daños o eliminaciones accidentales.

## <a name="backups"></a>Copias de seguridad

Azure Database for PostgreSQL realiza copias de seguridad de los archivos de datos y del registro de transacciones. En función del tamaño de almacenamiento máximo admitido, se realizan copias de seguridad completas y diferenciales (servidores de almacenamiento de 4 TB como máximo) o copias de seguridad de instantánea (servidores de almacenamiento de 16 TB como máximo). Estas copias de seguridad permiten restaurar un servidor a un momento dado dentro del período de retención de copias de seguridad configurado. El período de retención predeterminado es siete días. Opcionalmente, puede configurarlo hasta 35 días. Todas las copias de seguridad se cifran mediante cifrado AES de 256 bits.

Estos archivos de copia de seguridad no se pueden exportar. Las copias de seguridad solo se pueden usar para operaciones de restauración en Azure Database for PostgreSQL. Puede usar [pg_dump](howto-migrate-using-dump-and-restore.md) para copiar una base de datos.

### <a name="backup-frequency"></a>Frecuencia de copia de seguridad

Por lo general, las copias de seguridad completas se realizan semanalmente, y las copias de seguridad diferenciales se realizan dos veces al día para los servidores con un almacenamiento máximo admitido de 4 TB. Las copias de seguridad de instantáneas se realizan al menos una vez al día para los servidores que admiten hasta 16 TB de almacenamiento. Las copias de seguridad del registro de transacciones tienen lugar cada cinco minutos. La primera instantánea de una copia de seguridad completa se programa inmediatamente después de la creación del servidor. La copia de seguridad completa inicial puede tardar más en un gran servidor restaurado. El primer punto del tiempo al que se puede restaurar un servidor nuevo es la hora en que se completó la copia de seguridad completa inicial. Dado que las instantáneas son, justamente, instantáneas, los servidores que admiten hasta 16 TB de almacenamiento se pueden restaurar hasta la hora de creación.

### <a name="backup-redundancy-options"></a>Opciones de redundancia de copia de seguridad

Azure Database for PostgreSQL permite elegir entre almacenamiento de copia de seguridad con redundancia local o con redundancia geográfica en los planes Uso general y Memoria optimizada. Cuando las copias de seguridad se almacenan en un almacenamiento de copia de seguridad con redundancia geográfica, no solo se almacenan en la región en la que se hospeda el servidor, también se replican en un [centro de datos emparejado](https://docs.microsoft.com/azure/best-practices-availability-paired-regions). Esto proporciona una mejor protección y capacidad de restaurar el servidor en una región diferente en caso de desastre. El nivel Básico solo ofrece almacenamiento de copia de seguridad con redundancia local.

> [!IMPORTANT]
> La configuración de un almacenamiento con redundancia local o con redundancia geográfica para copia de seguridad solo se puede realizar durante la creación del servidor. Una vez que se ha aprovisionado el servidor, no se puede cambiar la opción de redundancia del almacenamiento de copia de seguridad.

### <a name="backup-storage-cost"></a>Costo del almacenamiento de copia de seguridad

Azure Database for PostgreSQL proporciona hasta un 100 % del almacenamiento del servidor aprovisionado como almacenamiento de copia de seguridad, sin costos adicionales. Normalmente, esto es apropiado para una retención de copias de seguridad de siete días. Cualquier almacenamiento de copia de seguridad adicional que se use se cargará por GB/mes.

Por ejemplo, si ha aprovisionado un servidor con 250 GB, tendrá 250 GB para almacenar copias de seguridad sin costos adicionales. El almacenamiento que supere los 250 GB se cobrará.

## <a name="restore"></a>Restauración

En Azure Database for PostgreSQL, al realizar una restauración se crea un nuevo servidor a partir de las copias de seguridad del servidor original.

Hay dos tipos de restauración disponibles:

- **Restauración a un momento dado**: está disponible con cualquier opción de redundancia de copia de seguridad y crea un nuevo servidor en la misma región que el servidor original.
- **Restauración geográfica**: solo está disponible si ha configurado el servidor para almacenamiento con redundancia geográfica y permite restaurar el servidor en una región diferente.

El tiempo estimado de recuperación depende de varios factores, como el tamaño de la bases de datos, el tamaño del registro de transacciones, el ancho de banda de red y el número total de bases de datos que se están recuperando en la misma región al mismo tiempo. Normalmente, el tiempo de recuperación es inferior a 12 horas.

> [!IMPORTANT]
> Los servidores eliminados **no se pueden** restaurar. Si elimina el servidor, todas las bases de datos que pertenecen al servidor también se eliminan y no se pueden recuperar. Para proteger los recursos del servidor, después de la implementación, de eliminaciones accidentales o cambios inesperados, los administradores pueden aprovechar los [bloqueos de administración](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-lock-resources).

### <a name="point-in-time-restore"></a>Restauración a un momento dado

Independientemente de la opción de redundancia de copia de seguridad, puede realizar una restauración a cualquier momento del tiempo dentro de su período de retención de copias de seguridad. Se crea un nuevo servidor en la misma región de Azure que el servidor original. Se crea con la configuración del servidor original para el plan de tarifa, generación de procesos, número de núcleos virtuales, tamaño de almacenamiento, período de retención de copia de seguridad y opción de redundancia de copia de seguridad.

La restauración a un momento dado es útil en diversos escenarios. Por ejemplo, cuando un usuario elimina accidentalmente los datos, elimina una tabla importante o la base de datos, o si una aplicación sobrescribe accidentalmente los datos correctos con datos incorrectos debido a un defecto de la aplicación.

Quizás deba esperar a que se realice la siguiente copia de seguridad del registro de transacciones antes de poder restaurar a un momento dado de los últimos cinco minutos.

### <a name="geo-restore"></a>Geo-restore

Puede restaurar un servidor en otra región de Azure donde el servicio esté disponible, si ha configurado el servidor para copias de seguridad con redundancia geográfica. Los servidores que admiten hasta 4 TB de almacenamiento se pueden restaurar en la región emparejada geográficamente o en cualquier región que admita hasta 16 TB de almacenamiento. En el caso de los servidores que admiten hasta 16 TB de almacenamiento, las copias de seguridad geográficas se pueden restaurar en cualquier región que admita también servidores de 16 TB. Revise los [planes de tarifa de Azure Database for PostgeSQL](concepts-pricing-tiers.md) para ver la lista de regiones admitidas.

La restauración geográfica es la opción de recuperación predeterminada cuando el servidor no está disponible debido a una incidencia en la región en la que se hospeda el servidor. Si un incidente a gran escala en una región provoca la falta de disponibilidad de una aplicación de base de datos, puede restaurar un servidor a partir de las copias de seguridad con redundancia geográfica en un servidor de cualquier otra región. Hay un retraso entre momento en que se realiza una copia de seguridad y el momento en que se replica en una región diferente. Este retraso puede ser de hasta una hora; por lo tanto, si se produce un desastre, puede haber una pérdida de datos de hasta una hora.

Durante la restauración geográfica, las configuraciones de servidor que se pueden cambiar incluyen la generación de procesos, núcleos virtuales, período de retención de copia de seguridad y opciones de redundancia de copia de seguridad. No se permite cambiar el plan de tarifa (Básico, Uso general o Memoria optimizada) ni el tamaño de almacenamiento.

### <a name="perform-post-restore-tasks"></a>Tareas posteriores a la restauración

Cuando efectúe la restauración con cualquiera de los mecanismos de recuperación, deberá realizar las siguientes tareas para que los usuarios y las aplicaciones vuelvan a conectarse:

- Si el nuevo servidor está destinado a reemplazar al servidor original, redirija los clientes y las aplicaciones de cliente al nuevo servidor.
- Asegúrese de aplicar reglas de red virtual y de firewall de nivel de servidor adecuadas para que se conecten los usuarios. Estas reglas no se copian desde el servidor original.
- No se olvide de emplear los permisos de nivel de base de datos y los inicios de sesión apropiados.
- Configure las alertas según corresponda.

## <a name="next-steps"></a>Pasos siguientes

- Aprenda a restaurar mediante el  [Azure Portal](howto-restore-server-portal.md).
- Aprenda a restaurar mediante la  [CLI de Azure](howto-restore-server-cli.md).
- Para más información acerca de la continuidad del negocio, consulte la  [introducción a la continuidad de negocio](concepts-business-continuity.md).
