---
title: Grupos de instancias (versión preliminar)
description: En este artículo se describen los grupos de instancias de Azure SQL Database (versión preliminar).
services: sql-database
ms.service: sql-database
ms.subservice: operations
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: bonova
ms.author: bonova
ms.reviewer: sstein, carlrab
ms.date: 09/05/2019
ms.openlocfilehash: a418972b78750dbed90f0148aac45dbcc2617f8e
ms.sourcegitcommit: 318d1bafa70510ea6cdcfa1c3d698b843385c0f6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/21/2020
ms.locfileid: "83773090"
---
# <a name="what-are-sql-database-instance-pools-preview"></a>¿Qué son los grupos de instancias de SQL Database (versión preliminar)?

Los grupos de instancias son un recurso nuevo de Azure SQL Database que proporciona una manera cómoda y rentable de migrar instancias de SQL más pequeñas a la nube a escala.

Los grupos de instancias permiten aprovisionar previamente recursos de proceso según los requisitos totales de la migración. Después, puede implementar varias instancias administradas individuales hasta el nivel de proceso aprovisionado previamente. Por ejemplo, si aprovisiona previamente 8 núcleos virtuales, puede implementar dos instancias de 2 núcleos virtuales y una instancia de 4 núcleos virtuales y, a continuación, migrar las bases de datos a estas instancias. Antes de que los grupos de instancias estuvieran disponibles, las cargas de trabajo de procesos intensivos más pequeñas y menos frecuentes a menudo hubiesen tenido que consolidarse en una instancia administrada más grande al migrar a la nube. La necesidad de migrar grupos de bases de datos a una instancia grande normalmente requería un cuidadoso planeamiento de la capacidad y gobernanza de los recursos, consideraciones de seguridad adicionales y algunos trabajos de consolidación de datos adicionales en el nivel de instancia.

Además, los grupos de instancias admiten la integración de redes virtuales nativas, por lo que puede implementar varios grupos de instancias y varias instancias únicas en la misma subred.


## <a name="key-capabilities-of-instance-pools"></a>Funcionalidades clave de los grupos de instancias

Los grupos de instancias ofrece las ventajas siguientes:

1. Capacidad de hospedar instancias de 2 núcleos virtuales. *\*Solo para las instancias en grupos de instancias*.
2. Tiempo de implementación de instancia predecible y rápida (hasta 5 minutos).
3. Asignación mínima de direcciones IP.

En el diagrama siguiente se muestra un grupo de instancias con varias instancias implementadas dentro de una subred de red virtual.

![Grupo de instancias con varias instancias](./media/sql-database-instance-pools/instance-pools1.png)

Los grupos de instancias permiten la implementación de varias instancias en la misma máquina virtual en la que el tamaño de proceso de la máquina virtual se basa en el número total de núcleos virtuales asignados para el grupo. Esta arquitectura permite la *creación de particiones* de la máquina virtual en varias instancias, que pueden ser de cualquier tamaño admitido, incluidos 2 núcleos virtuales (las instancias de 2 núcleos virtuales solo están disponibles para instancias en grupos).

Las operaciones de administración en las instancias de un grupo son mucho más rápidas una vez que el grupo se implementa inicialmente. Estas operaciones son más rápidas porque la implementación o la extensión de un [clúster virtual](sql-database-managed-instance-connectivity-architecture.md#high-level-connectivity-architecture) (un conjunto dedicado de máquinas virtuales) no forma parte del aprovisionamiento de la instancia administrada.

Dado que todas las instancias de un grupo comparten la misma máquina virtual, la asignación de direcciones IP total no depende del número de instancias implementadas, lo que resulta práctico para la implementación en subredes con un intervalo IP restringido.

Cada grupo tiene una asignación de direcciones IP fija de solo nueve direcciones IP (sin incluir las cinco direcciones IP de la subred que están reservadas para sus propias necesidades). Para información detallada, consulte los [requisitos de tamaño de la subred para instancias únicas](sql-database-managed-instance-determine-size-vnet-subnet.md).

## <a name="application-scenarios-for-instance-pools"></a>Escenarios de aplicación para grupos de instancias

En la lista siguiente se proporcionan los casos de uso principales en los que se deben tener en cuenta los grupos de instancias:

- Migración de *un grupo de instancias de SQL* al mismo tiempo, donde la mayoría es de un tamaño más pequeño (por ejemplo, 2 o 4 núcleos virtuales).
- Escenarios en los que es importante *la creación y el escalado de instancias cortas y predecibles*. Por ejemplo, la implementación de un inquilino nuevo en un entorno de aplicación SaaS multiinquilino que requiere funcionalidades de nivel de instancia.
- Escenarios en los que es importante tener un *costo fijo* o un *límite de gasto*. Por ejemplo, la ejecución de entornos compartidos de desarrollo y pruebas o de demostración de un tamaño fijo (o que cambia con poca frecuencia), donde se implementan periódicamente instancias administradas cuando es necesario.
- Escenarios en los que es importante la *asignación mínima de direcciones IP* en una subred de red virtual. Todas las instancias de un grupo comparten una máquina virtual, por lo que el número de direcciones IP asignadas es menor que en el caso de las instancias únicas.


## <a name="architecture-of-instance-pools"></a>Arquitectura de grupos de instancias

Los grupos de instancias tienen una arquitectura similar a las instancias administradas normales (*instancias únicas*). Para admitir [implementaciones dentro de máquinas virtuales de Azure](../virtual-network/virtual-network-for-azure-services.md) y proporcionar aislamiento y seguridad para los clientes, los grupos de instancias también dependen de  [clústeres virtuales](sql-database-managed-instance-connectivity-architecture.md#high-level-connectivity-architecture). Los clústeres virtuales representan un conjunto dedicado de máquinas virtuales aisladas implementadas dentro de la subred de la red virtual del cliente.

La principal diferencia entre los dos modelos de implementación es que los grupos de instancias permiten varias implementaciones de procesos de SQL Server en el mismo nodo de máquina virtual, que se rigen por los recursos mediante [objetos de trabajo de Windows](https://docs.microsoft.com/windows/desktop/ProcThread/job-objects), mientras que las instancias únicas siempre están solas en un nodo de máquina virtual.

En el diagrama siguiente se muestra un grupo de instancias y dos instancias individuales implementadas en la misma subred y se muestran los detalles principales de la arquitectura de ambos modelos de implementación:

![grupo de instancias y dos instancias individuales](./media/sql-database-instance-pools/instance-pools2.png)

Cada grupo de instancias crea un clúster virtual independiente debajo. Las instancias de un grupo y las instancias únicas implementadas en la misma subred no comparten recursos de proceso asignados a los componentes de la puerta de enlace y los procesos de SQL Server, lo que garantiza la predicción del rendimiento.

## <a name="instance-pools-resource-limitations"></a>Limitaciones de recursos de grupos de instancias

Existen varias limitaciones de recursos con respecto a los grupos de instancias y a las instancias dentro de los grupos:

- Los grupos de instancias solo están disponibles en hardware Gen5.
- Las instancias de un grupo tienen CPU y RAM dedicadas, por lo que el número agregado de núcleos virtuales entre todas las instancias debe ser menor o igual que el número de núcleos virtuales asignados al grupo.
- Todos los [límites de nivel de instancia](sql-database-managed-instance-resource-limits.md#service-tier-characteristics) se aplican a las instancias creadas dentro de un grupo.
- Además de los límites de nivel de instancia, también hay dos límites impuestos *en el nivel de grupo de instancias*:
  - Tamaño de almacenamiento total por grupo (8 TB).
  - Número total de bases de datos por grupo (100).

La asignación de almacenamiento total y el número de bases de datos en todas las instancias debe ser inferior o igual que los límites expuestos por los grupos de instancias.

- Los grupos de instancias admiten 8, 16, 24, 32, 40, 64 y 80 núcleos virtuales.
- Las instancias administradas dentro de los grupos admiten 2, 4, 8, 16, 24, 32, 40, 64 y 80 núcleos virtuales.
- Las instancias administradas dentro de los grupos admiten tamaños de almacenamiento entre 32 GB y 8 TB, excepto:
  - Las instancias con 2 núcleos virtuales admiten tamaños entre 32 GB y 640 GB.
  - Las instancias con 4 núcleos virtuales admiten tamaños entre 32 GB y 2 TB.

La [propiedad de nivel de servicio](sql-database-managed-instance-resource-limits.md#service-tier-characteristics) está asociada al recurso de grupo de instancias, por lo que todas las instancias de un grupo deben ser del mismo nivel de servicio que el nivel de servicio del grupo. En este momento, solo está disponible el nivel de servicio De uso general (consulte la sección siguiente sobre las limitaciones de la versión preliminar actual).

### <a name="public-preview-limitations"></a>Limitaciones de la vista preliminar pública

La versión preliminar pública tiene las limitaciones siguientes:

- Actualmente, solo está disponible el nivel de servicio De uso general.
- Los grupos de instancias no se pueden escalar durante la versión preliminar pública, por lo que es importante planear la capacidad antes de la implementación.
- Todavía no está disponible la compatibilidad de Azure Portal con la creación y configuración de los grupos de instancias. Todas las operaciones en grupos de instancias se admiten solo a través de PowerShell. La implementación de instancia inicial en un grupo creado previamente también se admite solo a través de PowerShell. Una vez implementadas en un grupo, las instancias administradas se pueden actualizar con Azure Portal.
- Las instancias administradas creadas fuera del grupo no se pueden mover a un grupo existente y las instancias creadas dentro de un grupo no se pueden migrar fuera como una sola instancia o a otro grupo.
- Los precios de las instancias reservadas (con licencia incluida o con Ventaja híbrida de Azure) no están disponibles.

## <a name="sql-features-supported"></a>Características de SQL admitidas

Las instancias creadas en grupos admiten los mismos [niveles de compatibilidad y características compatibles en instancias administradas únicas](sql-database-managed-instance.md#sql-features-supported).

Cada instancia administrada implementada en un grupo tiene una instancia independiente del Agente SQL.

Las características opcionales o las que requieren que elija valores específicos (como la intercalación de nivel de instancia, la zona horaria, el punto de conexión público para el tráfico de datos, los grupos de conmutación por error) se configuran en el nivel de instancia y pueden ser diferentes para cada instancia de un grupo.

## <a name="performance-considerations"></a>Consideraciones de rendimiento

Aunque las instancias administradas dentro de los grupos tienen RAM y núcleos virtuales dedicados, comparten el disco local (para el uso de tempdb) y los recursos de red. No es probable, pero es posible experimentar el efecto del *entorno ruidoso* si varias instancias del grupo tienen un consumo elevado de recursos al mismo tiempo. Si observa este comportamiento, considere la posibilidad de implementar estas instancias en un grupo más grande o como instancias únicas.

## <a name="security-considerations"></a>Consideraciones sobre la seguridad

Dado que las instancias implementadas en un grupo comparten la misma máquina virtual, es posible que quiera considerar la posibilidad de deshabilitar las características que presentan mayores riesgos de seguridad o de controlar estrictamente los permisos de acceso a estas características. Por ejemplo, la integración con CLR, la copia de seguridad y restauración nativa, el correo electrónico de base de datos, etc.

## <a name="instance-pool-support-requests"></a>Solicitudes de soporte técnico de un grupo de instancias

Cree y administre solicitudes de soporte técnico para grupos de instancias en [Azure Portal](https://portal.azure.com).

Si tiene problemas relacionados con la implementación de un grupo de instancias (creación o eliminación), asegúrese de especificar **Grupos de instancias** en el campo **Subtipo de problema**.

![solicitud de soporte técnico de grupos de instancias](./media/sql-database-instance-pools/support-request.png)

Si tiene problemas relacionados con bases de datos o instancias únicas dentro de un grupo, debe crear una incidencia de soporte técnico normal para las instancias administradas de Azure SQL Database.

Para crear implementaciones de instancias administradas más grandes (con o sin grupos de instancias), es posible que tenga que obtener una cuota regional más grande. Para más información, consulte [Solicitud de aumentos de cuota para Azure SQL Database](quota-increase-request.md). Tenga en cuenta que si usa grupos de instancias, la lógica de implementación compara el consumo total de núcleos virtuales *en el nivel de grupo* con respecto a la cuota para determinar si se permite crear recursos nuevos sin aumentar aún más la cuota.

## <a name="instance-pool-billing"></a>Facturación de un grupo de instancias

Los grupos de instancias permiten escalar el proceso y el almacenamiento de manera independiente. Los clientes pagan por el proceso asociado al recurso de grupo medido en núcleos virtuales y el almacenamiento asociado a cada instancia medido en gigabytes (los primeros 32 GB son gratuitos para cada instancia).

El precio de núcleo virtual para un grupo se cobra independientemente del número de instancias que se implementen en ese grupo.

Para el precio de proceso (medido en núcleos virtuales), hay disponibles dos opciones de precios:

  1. *Con licencia incluida*: se incluye el precio de las licencias de SQL. Esto es para los clientes que eligen no aplicar las licencias de SQL Server existentes con Software Assurance.
  2. *Ventaja híbrida de Azure*: precio reducido que incluye la Ventaja híbrida de Azure para SQL Server. Los clientes pueden optar a este precio si utilizan sus licencias de SQL Server con Software Assurance. Para información sobre la idoneidad y otros detalles, consulte [Ventaja híbrida de Azure](https://azure.microsoft.com/pricing/hybrid-benefit/).

No es posible establecer diferentes opciones de precios para las instancias individuales de un grupo. Todas las instancias del grupo primario deben tener un precio con licencia incluida o precio con Ventaja híbrida de Azure. El modelo de licencia para el grupo se puede modificar una vez creado el grupo.

> [!IMPORTANT]
> Si especifica un modelo de licencia para la instancia que sea diferente que en el grupo, se usa el precio del grupo y se omite el valor de nivel de instancia.

Si crea grupos de instancias en [suscripciones válidas para la ventaja de desarrollo y pruebas](https://azure.microsoft.com/pricing/dev-test/), recibirá automáticamente tarifas con descuento de hasta el 55 % en una instancia administrada de Azure SQL.

Para información detallada sobre los precios de un grupo de instancias, consulte la sección sobre los *grupos de instancias* en la [página de precios de Instancia administrada](https://azure.microsoft.com/pricing/details/sql-database/managed/).

## <a name="next-steps"></a>Pasos siguientes

- Para empezar a trabajar con grupos de instancias, consulte [Guía paso a paso sobre los grupos de instancias de SQL Database](sql-database-instance-pools-how-to.md).
- Para más información sobre cómo crear su primera instancia administrada, vea la [Guía de inicio rápido](sql-database-managed-instance-get-started.md).
- Para obtener una lista de características y una comparación, consulte [Características comunes de SQL](sql-database-features.md).
- Para más información sobre la configuración de redes virtuales, vea [Configuración de una red virtual de instancia administrada](sql-database-managed-instance-connectivity-architecture.md).
- Para ver una guía de inicio rápido en la que se crea una instancia administrada y se restaura una base de datos desde un archivo de copia de seguridad, vea [Creación de una instancia administrada](sql-database-managed-instance-get-started.md).
- Para consultar un tutorial con Azure Database Migration Service (DMS) para la migración, vea [Migración a Instancia administrada con DMS](../dms/tutorial-sql-server-to-managed-instance.md).
- Para una supervisión avanzada del rendimiento de una base de datos de instancia administrada con inteligencia de solución de problemas integrada, consulte [Supervisión de instancias de Azure SQL Database con Azure SQL Analytics](../azure-monitor/insights/azure-sql.md).
- Para obtener información de precios, vea [Precios de Instancia administrada de SQL Database](https://azure.microsoft.com/pricing/details/sql-database/managed/).
