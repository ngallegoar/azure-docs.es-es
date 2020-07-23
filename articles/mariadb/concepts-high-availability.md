---
title: 'Alta disponibilidad: Azure Database for MariaDB'
description: En este artículo se proporciona información sobre la alta disponibilidad en Azure Database for MariaDB
author: kummanish
ms.author: manishku
ms.service: mariadb
ms.topic: conceptual
ms.date: 7/7/2020
ms.openlocfilehash: bea32b3b60c9013ea223513c95629092b9ab231b
ms.sourcegitcommit: 3541c9cae8a12bdf457f1383e3557eb85a9b3187
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/09/2020
ms.locfileid: "86203335"
---
# <a name="high-availability-in-azure-database-for-mariadb"></a>Alta disponibilidad en Azure Database for MariaDB
El servicio Azure Database for MariaDB proporciona un alto nivel de disponibilidad garantizada gracias al Acuerdo de Nivel de Servicio (SLA) respaldado económicamente, con un tiempo de actividad del [99,99 %](https://azure.microsoft.com/support/legal/sla/MariaDB). Azure Database for MariaDB proporciona una alta disponibilidad durante los eventos planeados, como la operación de proceso de escalado iniciada por el usuario, y también cuando se producen eventos no planeados, como los errores subyacentes de hardware, software o red. Azure Database for MariaDB puede recuperarse rápidamente de las circunstancias más críticas, lo que garantiza que las aplicaciones prácticamente no tienen tiempo de inactividad al usar este servicio.

Azure Database for MariaDB resulta adecuado para ejecutar bases de datos esenciales que requieren un tiempo de actividad elevado. Basado en la arquitectura de Azure, el servicio tiene funcionalidades intrínsecas de alta disponibilidad, redundancia y resistencia para mitigar el tiempo de inactividad de la base de datos frente a interrupciones planeadas y no planeadas, sin necesidad de configurar ningún componente adicional. 

## <a name="components-in-azure-database-for-mariadb"></a>Componentes en Azure Database for MariaDB

| **Componente** | **Descripción**|
| ------------ | ----------- |
| <b>Servidor de bases de datos MariaDB | Azure Database for MariaDB proporciona seguridad, aislamiento, medidas de seguridad para recursos y una funcionalidad de reinicio rápido para los servidores de bases de datos. Estas funcionalidades facilitan las operaciones como el escalado y la operación de recuperación del servidor de bases de datos después de una interrupción en segundos. <br/> Normalmente, las modificaciones de datos en el servidor de base de datos se producen en el contexto de una transacción de base de datos. Todos los cambios en la base de datos se registran sincrónicamente como registros de escritura previa (ib_log) en Azure Storage, que está conectado al servidor de bases de datos. Durante el proceso de [punto de comprobación](https://mariadb.com/kb/innodb-redo-log/#checkpoints), las páginas de datos de la memoria del servidor de bases de datos también se vacían en el almacenamiento. |
| <b>Almacenamiento remoto | Todos los archivos de datos físicos de MariaDB y los archivos de registro se almacenan en Azure Storage, que está diseñado para almacenar tres copias de los datos en una región para garantizar la redundancia, disponibilidad y confiabilidad de los datos. La capa de almacenamiento también es independiente del servidor de bases de datos. Se puede desasociar de un servidor de base de datos con errores y asociarse de nuevo a un nuevo servidor de base de datos en pocos segundos. Además, Azure Storage supervisa continuamente el entorno en busca de errores de almacenamiento. Si se detectan daños en un bloque, se corrige automáticamente mediante la creación de una nueva copia de almacenamiento. |
| <b>Puerta de enlace | La puerta de enlace actúa como un proxy de base de datos, enruta todas las conexiones de cliente al servidor de base de datos. |

## <a name="planned-downtime-mitigation"></a>Mitigación del tiempo de inactividad planeado
Azure Database for MariaDB está diseñado para proporcionar alta disponibilidad durante las operaciones de tiempo de inactividad planeado. 

![Vista del escalado elástico en Azure para MariaDB](./media/concepts-high-availability/elastic-scaling-mariadb-server.png)

Estos son algunos escenarios de mantenimiento planeado:

| **Escenario** | **Descripción**|
| ------------ | ----------- |
| <b>Escalado y reducción vertical de proceso | Cuando el usuario realiza una operación de escalado o reducción vertical de procesos, se aprovisiona un nuevo servidor de base de datos con la configuración de proceso escalado. En el servidor de base de datos anterior, se permite que se finalicen los puntos de comprobación activos, se purgan las conexiones de cliente, se cancelan las transacciones no confirmadas y, a continuación, se apaga el servidor. A continuación, el almacenamiento se desasocia del servidor de base de datos anterior y se conecta al nuevo servidor de base de datos. Cuando la aplicación cliente reintenta la conexión o trata de establecer una conexión nueva, la puerta de enlace dirige la solicitud de conexión al nuevo servidor de bases de datos.|
| <b>Escalado vertical del almacenamiento | El escalado vertical del almacenamiento es una operación en línea y no interrumpe el servidor de base de datos.|
| <b>Nueva implementación de software (Azure) | Las nuevas características de implementación o corrección de errores se producen automáticamente como parte del mantenimiento planeado del servicio. Para obtener más información, consulte la [documentación](concepts-monitoring.md#planned-maintenance-notification) y consulte también el [portal](https://aka.ms/servicehealthpm).|
| <b>Actualizaciones de versión secundarias | Azure Database for MariaDB revisa automáticamente los servidores de bases de datos según la versión secundaria determinada por Azure. Se produce como parte del mantenimiento planeado del servicio. Esto provocaría un breve tiempo de inactividad en términos de segundos y el servidor de base de datos se reiniciará automáticamente con la nueva versión secundaria. Para obtener más información, consulte la [documentación](concepts-monitoring.md#planned-maintenance-notification) y consulte también el [portal](https://aka.ms/servicehealthpm).|


##  <a name="unplanned-downtime-mitigation"></a>Mitigación del tiempo de inactividad no planeado

Se puede producir un tiempo de inactividad no planeado como resultado de errores imprevistos, incluidos los errores subyacentes de hardware, los problemas de red y los errores de software. Si el servidor de base de datos deja de funcionar de forma inesperada, se aprovisiona automáticamente un nuevo servidor de base de datos en segundos. El almacenamiento remoto se asocia automáticamente al nuevo servidor de base de datos. El motor de MariaDB realiza la operación de recuperación mediante WAL y archivos de base de datos, y abre el servidor de base de datos para permitir que los clientes se conecten. Las transacciones no confirmadas se pierden y la aplicación debe volver a intentarlas. Aunque no se puede evitar un tiempo de inactividad no planeado, Azure Database for MariaDB reduce el tiempo de inactividad al realizar de forma automática operaciones de recuperación en el servidor de base de datos y en las capas de almacenamiento sin necesidad de intervención humana. 


![Vista de alta disponibilidad en Azure para MariaDB](./media/concepts-high-availability/availability-mariadb-server.png)

### <a name="unplanned-downtime-failure-scenarios-and-service-recovery"></a>Tiempo de inactividad no planeado: escenarios de error y recuperación de servicio
Estos son algunos escenarios de error y cómo Azure Database for MariaDB se recupera automáticamente:

| **Escenario** | **Recuperación automática** |
| ---------- | ---------- |
| <B>Error de servidor de bases de datos | Si el servidor de base de datos está inactivo debido a un error de hardware subyacente, se quitan las conexiones activas y se anulan las transacciones inactivas. Se implementa automáticamente un nuevo servidor de base de datos y el almacenamiento de datos remotos se adjunta al nuevo servidor de base de datos. Una vez completada la recuperación de la base de datos, los clientes pueden conectarse al nuevo servidor de base de datos a través de la puerta de enlace. <br /> <br /> Las aplicaciones que usan bases de datos de MariaDB se deben crear de forma que detecten y vuelvan a intentar establecer las conexiones eliminadas y las transacciones erróneas.  Cuando la aplicación vuelve a intentarlo, la puerta de enlace redirige de forma transparente la conexión al servidor de base de datos recién creado. |
| <B>Error de almacenamiento | Las aplicaciones no verán ningún impacto por los problemas relacionados con el almacenamiento, como un error de disco o un daño de bloque físico. A medida que los datos se almacenan en tres copias, el almacenamiento sobreviviente proporciona la copia de los datos. Los daños en bloques se corrigen automáticamente. Si se pierde una copia de los datos, se crea automáticamente una nueva. |

Estos son algunos escenarios de error que requieren de la acciones del usuario para recuperarse:

| **Escenario** | **Plan de recuperación** |
| ---------- | ---------- |
| <b> Error de región | Un error en una región es un evento poco habitual. Sin embargo, si necesita protección ante un error de región, puede configurar una o varias réplicas de lectura en otras regiones para la recuperación ante desastres (DR). (Consulte [este artículo](howto-read-replicas-portal.md) sobre cómo crear y administrar réplicas de lectura para más información). En caso de que se produzca un error de nivel de región, puede promover manualmente la réplica de lectura configurada en la otra región para que sea el servidor de base de datos de producción. |
| <b> Errores de usuario o lógicos | La recuperación de los errores de usuario, como las tablas eliminadas accidentalmente o los datos actualizados incorrectamente, implica la realización de una [recuperación a un momento dado](concepts-backup.md) (PITR), de modo que se restauran y recuperan los datos hasta el momento justo antes de que se produjera el error.<br> <br>  Si quiere restaurar únicamente un subconjunto de bases de datos o tablas específicas en lugar de todas las bases de datos del servidor de bases de datos, puede restaurar el servidor de base de datos en una nueva instancia, exportar las tablas mediante [mysqldump](howto-migrate-dump-restore.md) y usar [restore](howto-migrate-dump-restore.md#restore-your-mariadb-database) para restaurar esas tablas en la base de datos. |



## <a name="summary"></a>Resumen

Azure Database for MariaDB ofrece una funcionalidad de reinicio rápido de servidores de bases de datos, almacenamiento redundante y enrutamiento eficaz desde la puerta de enlace. Para proteger los datos de forma adicional, puede configurar las copias de seguridad para que se repliquen geográficamente, así como implementar una o varias réplicas de lectura en otras regiones. Con funcionalidades de alta disponibilidad intrínsecas, Azure Database for MariaDB protege las bases de datos de las interrupciones más comunes y ofrece un [Acuerdo de Nivel de Servicio con tiempo de actividad del 99,99 %](https://azure.microsoft.com/support/legal/sla/MariaDB) con respaldo económico. Todas estas funcionalidades de disponibilidad y confiabilidad permiten que Azure sea la plataforma ideal para ejecutar aplicaciones críticas.

## <a name="next-steps"></a>Pasos siguientes
- Más información sobre las [regiones de Azure](../availability-zones/az-overview.md)
- Más información sobre el [control de errores de conectividad transitorios](concepts-connectivity.md)
- Más información sobre cómo [replicar los datos con réplicas de lectura](howto-read-replicas-portal.md)