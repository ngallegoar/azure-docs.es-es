---
title: 'Continuidad empresarial en la nube: recuperación de bases de datos'
description: Obtenga información acerca de cómo Azure SQL Database permite la continuidad del negocio en la nube y la recuperación de la base de datos, y ayuda a que las aplicaciones críticas de la nube se sigan ejecutando.
keywords: continuidad del negocio, continuidad del negocio en la nube, recuperación de desastres de la base de datos, recuperación de la base de datos
services: sql-database
ms.service: sql-database
ms.subservice: high-availability
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: anosov1960
ms.author: sashan
ms.reviewer: mathoma, carlrab
ms.date: 06/25/2019
ms.openlocfilehash: 4f30bf112175742566c2957d78154e5a7abd1733
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79096865"
---
# <a name="overview-of-business-continuity-with-azure-sql-database"></a>Introducción a la continuidad empresarial con Azure SQL Database

**Continuidad empresarial** en Azure SQL Database hace referencia a los mecanismos, directivas y procedimientos que permiten a su empresa seguir funcionando en caso de interrupciones, especialmente en su infraestructura de computación. En la mayoría de los casos, Azure SQL Database controlará los eventos de interrupción que puedan ocurrir en el entorno en la nube y permitirá que se sigan ejecutando las aplicaciones y los procesos empresariales. No obstante, hay algunos eventos de interrupción que no se pueden controlar mediante SQL Database automáticamente, por ejemplo:

- El usuario eliminó o actualizó accidentalmente una fila de una tabla.
- Un atacante malintencionado consiguió eliminar datos o quitar una base de datos.
- Un terremoto provocó un corte del suministro eléctrico y deshabilitó temporalmente el centro de datos.

En este artículo de introducción se describen las funcionalidades de continuidad empresarial y recuperación ante desastres que proporciona Azure SQL Database. Conozca más información sobre opciones, recomendaciones y tutoriales para recuperarse de eventos de interrupción que podrían provocar la pérdida de información o que su base de datos y aplicación dejen de estar disponibles. Sepa qué hacer en caso de que un error del usuario o la aplicación afecte a la integridad de los datos, se produzca una interrupción en una región de Azure o su aplicación requiera mantenimiento.

## <a name="sql-database-features-that-you-can-use-to-provide-business-continuity"></a>Características de SQL Database que puede utilizar para proporcionar continuidad empresarial

Desde la perspectiva de una base de datos, hay cuatro escenarios principales de posibles interrupciones:

- Errores de hardware o software locales que afectan al nodo de la base de datos; por ejemplo, un error de la unidad de disco.
- Daños o eliminación de datos que suelen deberse a un error de la aplicación o a un error humano. Estos errores son específicos de la aplicación y el servicio de la base de datos no los detecta.
- Interrupción del centro de datos, provocado posiblemente por un desastre natural. Este escenario requiere cierto nivel de redundancia geográfica con conmutación por error de la aplicación en un centro de datos alternativo.
- Los errores de actualización o mantenimiento y problemas imprevistos que se producen durante las actualizaciones o el mantenimiento planeados pueden requerir una rápida reversión a un estado anterior de la base de datos.

Para mitigar los errores de hardware local y de software, SQL Database incluye una [arquitectura de alta disponibilidad](sql-database-high-availability.md) que garantiza una recuperación automática de estos errores con un Acuerdo de Nivel de Servicio de disponibilidad del 99,995 %.  

Para proteger su empresa de la pérdida de datos, SQL Database realiza automáticamente copias de seguridad completas semanales de la base de datos, copias de seguridad diferenciales de la base de datos cada 12 horas y copias de seguridad del registro de transacciones cada 5-10 minutos. Las copias de seguridad se guardan en almacenamiento con redundancia geográfica con acceso de lectura durante al menos 7 días para todos los niveles de servicio. Todos los niveles de servicio, excepto el soporte técnico Basic, admiten el período de retención de copia de seguridad configurable hasta 35 días para la restauración a un momento dado. 

SQL Database también proporciona varias características de continuidad empresarial que puede usar para mitigar varios escenarios no planeados. 

- [Tablas temporales](sql-database-temporal-tables.md) que le permiten restaurar versiones de fila desde cualquier momento dado.
- Las [copias de seguridad automatizadas integradas](sql-database-automated-backups.md) y la [restauración a un momento dado](sql-database-recovery-using-backups.md#point-in-time-restore) le permiten restaurar la base de datos completa a un momento dado dentro del período de retención configurado de hasta 35 días.
- Puede [restaurar una base de datos eliminada](sql-database-recovery-using-backups.md#deleted-database-restore) al momento en que se eliminó si el **servidor de SQL Database no se ha eliminado**.
- La [retención de copia de seguridad a largo plazo](sql-database-long-term-retention.md) le permite conservar las copias de seguridad hasta 10 años.
- La [replicación geográfica activa](sql-database-active-geo-replication.md) le permite crear réplicas legibles y realizar una conmutación por error manual a cualquier réplica en el caso de una interrupción en el centro de datos o una actualización de aplicación.
- El [grupo de conmutación por error automática](sql-database-auto-failover-group.md#auto-failover-group-terminology-and-capabilities) permite que la aplicación se recupere automáticamente en el caso de que se produzca una interrupción en el centro de datos.

## <a name="recover-a-database-within-the-same-azure-region"></a>Recuperación de una base de datos en la misma región de Azure

Puede usar copias de seguridad de la base de datos automáticas para restaurar una base de datos a un momento anterior. De este modo puede recuperarse de los daños en los datos causados por errores humanos. La restauración a un momento dado le permite crear una base de datos en el mismo servidor que represente el estado de los datos antes del evento de daño. Para la mayoría de las bases de datos, las operaciones de restauración tardan menos de 12 horas. La recuperación de una base de datos muy grande o muy activa puede tardar más tiempo. Para más información sobre el tiempo de recuperación, consulte el apartado sobre el [tiempo de recuperación de bases de datos](sql-database-recovery-using-backups.md#recovery-time). 

Si el período de retención de la restauración a un momento dado (PITR) máximo admitido no es suficiente para su aplicación, puede ampliarlo mediante la configuración de una directiva de retención a largo plazo (LTR) para las bases de datos. Para obtener más información, vea [Retención de copias de seguridad a largo plazo](sql-database-long-term-retention.md).

## <a name="compare-geo-replication-with-failover-groups"></a>Comparación de la replicación geográfica con los grupos de conmutación por error

Los [grupos de conmutación por error automática](sql-database-auto-failover-group.md#auto-failover-group-terminology-and-capabilities) simplifican la implementación y el uso de la [replicación geográfica](sql-database-active-geo-replication.md) y agregan las funcionalidades adicionales, como se describe en la tabla siguiente:

|                                              | Replicación geográfica | Grupos de conmutación por error  |
|:---------------------------------------------| :-------------- | :----------------|
| Conmutación por error automática                           |     No          |      Sí         |
| Conmutación por error de varias bases de datos simultáneamente  |     No          |      Sí         |
| El usuario debe actualizar la cadena de conexión tras la conmutación por error      |     Sí         |      No          |
| Compatibilidad con la instancia administrada                   |     No          |      Sí         |
| Posibilidad de estar en la misma región que la principal             |     Sí         |      No          |
| Varias réplicas                            |     Sí         |      No          |
| Admisión del escalado de lectura                          |     Sí         |      Sí         |
| &nbsp; | &nbsp; | &nbsp; |


## <a name="recover-a-database-to-the-existing-server"></a>Recuperación de una base de datos en el servidor existente

Aunque es poco habitual, en los centros de datos de Azure pueden producirse interrupciones. Cuando esto ocurre, provoca también una interrupción en el negocio que podría extenderse solo unos pocos minutos o, incluso, horas.

- Una opción consiste en esperar a que la base de datos vuelva a estar en línea cuando termine la interrupción del centro de datos. Esto puede hacerse con las aplicaciones que pueden permitirse que la base de datos esté desconectada. Por ejemplo, los proyectos de desarrollo o las pruebas gratuitas no tienen que estar en funcionamiento constantemente. Cuando se produce una interrupción en un centro de datos, no sabe cuánto durará, por lo que esta opción solo es útil si no necesita utilizar la base de datos durante un tiempo.
- Otra opción consiste en restaurar una base de datos en cualquier servidor de cualquier región de Azure mediante [copias de seguridad de base de datos con redundancia geográfica](sql-database-recovery-using-backups.md#geo-restore) (restauración geográfica). La funcionalidad de restauración geográfica utiliza una copia de seguridad con redundancia geográfica como origen y se puede usar para recuperar una base de datos, aunque no se pueda acceder a dicha base de datos o al centro de datos debido a una interrupción.
- Por último, puede recuperarse rápidamente de una interrupción si ha configurado, o bien, una réplica geográfica secundaria con la [replicación geográfica activa](sql-database-active-geo-replication.md) o un [grupo de conmutación por error automática](sql-database-auto-failover-group.md) para las bases de datos. Dependiendo de la selección de estas tecnologías, puede usar la conmutación manual o automática por error. Mientras la propia conmutación por error solo tarda unos segundos, el servicio tardará al menos de 1 hora en activarla. Esto es necesario asegurarse de que la conmutación por error está justificada por la escala de la interrupción. Además, la conmutación por error puede provocar pérdida de datos pequeño debido a la naturaleza de la replicación asincrónica. 

A medida que desarrolle el plan de continuidad empresarial, tendrá que saber el tiempo máximo aceptable para que la aplicación se recupere por completo tras un evento de interrupción. El tiempo necesario para que la aplicación se recupere totalmente se conoce como objetivo de tiempo de recuperación (RTO). También debe conocer el período máximo de actualizaciones de datos recientes (intervalo de tiempo) que la aplicación puede tolerar perder al recuperarse de un evento de interrupción no planeado. La posible pérdida de datos se conoce como objetivo de punto de recuperación (RPO).

Los diferentes métodos de recuperación ofrecen distintos niveles de RPO y RTO. Puede elegir un método de recuperación específico o usar una combinación de métodos para lograr la total recuperación de la aplicación. En la tabla siguiente se comparan el RPO y el RTO de cada opción de recuperación. Los grupos de conmutación por error automática simplifican la implementación y el uso de la replicación geográfica y agregan las funcionalidades adicionales, como se describe en la tabla siguiente.

| Método de recuperación | RTO | RPO |
| --- | --- | --- | 
| Restauración geográfica de las copias de seguridad con replicación geográfica | 12 h | 1 h |
| Grupos de conmutación por error automática | 1 h | 5 s |
| Conmutación por error de base de datos manual | 30 s | 5 s |

> [!NOTE]
> La *conmutación por error de base de datos manual* hace referencia a la conmutación por error de una sola base de datos en su replicación geográfica secundaria realizada en [modo no planeado](sql-database-active-geo-replication.md#active-geo-replication-terminology-and-capabilities).
Consulte la tabla anterior de este artículo para detalles sobre RTO y RPO de conmutación por error automática.


Use grupos de conmutación por error automática si su aplicación cumple alguno de estos criterios:

- Es crítica.
- Tiene un SLA que no se permite que se produzcan tiempos de inactividad de más de 12 horas.
- El tiempo de inactividad incurrirá en responsabilidades financieras.
- Tiene una tasa de cambio de datos elevada y una hora de pérdida de datos es inaceptable.
- El costo adicional por utilizar la replicación geográfica activa es menor que el de la posible responsabilidad financiera y la pérdida de negocio asociada que habría que asumir.

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Azure-SQL-Database-protecting-important-DBs-from-regional-disasters-is-easy/player]
>

Puede decidir usar una combinación de copias de seguridad de base de datos y replicación geográfica activa según los requisitos de la aplicación. Para una explicación de las consideraciones de diseño para bases de datos independientes y grupos elásticos con estas características de continuidad empresarial, consulte los artículos sobre [diseño de una aplicación para la recuperación ante desastres](sql-database-designing-cloud-solutions-for-disaster-recovery.md) y [estrategias de recuperación ante desastres para los grupos elásticos](sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool.md).

En las siguientes secciones se ofrece información general de los pasos para realizar tareas de recuperación mediante copias de seguridad de bases de datos o la replicación geográfica activa. Para ver los pasos detallados, como el planeamiento de los requisitos, los pasos posteriores a la recuperación y los detalles sobre cómo simular una interrupción para llevar a cabo tareas de recuperación ante desastres, consulte el artículo [Recuperación de una instancia de SQL Database después de una interrupción](sql-database-disaster-recovery.md).

### <a name="prepare-for-an-outage"></a>Preparativos para interrupciones

Con independencia de la característica de continuidad empresarial que use, debe hacer lo siguiente:

- Identificar y preparar el servidor de destino, incluidas las reglas de firewall de IP en el nivel de servidor, los inicios de sesión y los permisos de nivel de base de datos maestra.
- Determinar cómo se redirigirán los clientes y las aplicaciones cliente al nuevo servidor
- Documentar otras dependencias, como las alertas y la configuración de auditoría

Si no se prepara correctamente, el proceso de conectar las aplicaciones después de una conmutación por error o una recuperación de base de datos llevará más tiempo y, probablemente, también haya que solucionar problemas en momentos de estrés, por lo que no es nada recomendable.

### <a name="fail-over-to-a-geo-replicated-secondary-database"></a>Conmutación por error de la base de datos secundaria de replicación geográfica

Si usa grupos de conmutación automática por error o replicación geográfica activa como mecanismo de recuperación, puede configurar una directiva de conmutación automática por error o usar la [conmutación por error manual no planeada](sql-database-active-geo-replication-portal.md#initiate-a-failover). Tras iniciar la conmutación por error, la base de datos secundaria pasa a ser la principal y está lista para registrar nuevas transacciones y responder a consultas, con una pérdida mínima de los datos que aún no se han replicado. Para obtener información sobre el diseño del proceso de conmutación por error, vea [Diseño de aplicaciones para la recuperación ante desastres en la nube](sql-database-designing-cloud-solutions-for-disaster-recovery.md).

> [!NOTE]
> Cuando el centro de datos vuelve a estar en línea, las bases de datos principales anteriores se vuelven a conectar automáticamente con la nueva base de datos principal y se convierten en bases de datos secundarias. Si necesita reubicar la base de datos principal de nuevo en la región original, puede iniciar una conmutación por error manual planificada (conmutación por recuperación).

### <a name="perform-a-geo-restore"></a>Realización de restauraciones geográficas

Si utiliza las copias de seguridad automatizadas con almacenamiento con redundancia geográfica (habilitado de forma predeterminada), puede recuperar la base de datos mediante [restauración geográfica](sql-database-disaster-recovery.md#recover-using-geo-restore). El proceso de recuperación empieza en un plazo de 12 horas como máximo y la pérdida de datos supone como mucho una hora, lo cual viene determinado por la última copia de seguridad y réplica de registros. Hasta que no se complete la recuperación, la base de datos no puede registrar transacciones ni responder a las consultas. Tenga en cuenta que con la restauración geográfica solo se restaura la base de datos al último momento disponible.

> [!NOTE]
> Si el centro de datos vuelve a estar en línea antes de cambiar la aplicación en la base de datos recuperada, puede cancelar el proceso de recuperación.

### <a name="perform-post-failover--recovery-tasks"></a>Realización de tareas posteriores a la recuperación y conmutación por error

Cuando efectúe la recuperación con cualquiera de los mecanismos para llevarla a cabo, debe realizar las siguientes tareas adicionales antes de que los usuarios y las aplicaciones vuelvan a conectarse:

- Redirija los clientes y las aplicaciones cliente al nuevo servidor y a la base de datos restaurada.
- Asegúrese de aplicar reglas de firewall de IP en el nivel de servidor adecuadas para que se conecten los usuarios o use [firewalls de nivel de base de datos](sql-database-firewall-configure.md#use-the-azure-portal-to-manage-server-level-ip-firewall-rules) para habilitar las reglas adecuadas.
- No se olvide de emplear permisos de nivel de base de datos maestra e inicios de sesión apropiados (o use [usuarios contenidos](https://docs.microsoft.com/sql/relational-databases/security/contained-database-users-making-your-database-portable)).
- Configure la auditoría según corresponda.
- Configure las alertas según corresponda.

> [!NOTE]
> Si está utilizando un grupo de conmutación por error y se conecta a las bases de datos mediante el agente de escucha de lectura y escritura, el redireccionamiento después de la conmutación por error se realizará de forma automática y transparente a la aplicación.

## <a name="upgrade-an-application-with-minimal-downtime"></a>Actualice una aplicación con el mínimo tiempo de inactividad posible.

A veces, debe desconectar una aplicación debido a un mantenimiento planeado, por ejemplo, para actualizarla. En el artículo [Administración de actualizaciones graduales de aplicaciones](sql-database-manage-application-rolling-upgrade.md) se explica cómo utilizar la replicación geográfica activa para poder actualizar gradualmente su aplicación en la nube con el objetivo de minimizar el tiempo de inactividad durante este proceso. Además, se describe una ruta de recuperación para cuando se empiece a detectar errores.

## <a name="next-steps"></a>Pasos siguientes

Para una explicación de las consideraciones de diseño de las aplicaciones para bases de datos independientes y grupos elásticos, consulte los artículos sobre [diseño de una aplicación para la recuperación ante desastres en la nube](sql-database-designing-cloud-solutions-for-disaster-recovery.md) y [estrategias de recuperación ante desastres para los grupos elásticos](sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool.md).
