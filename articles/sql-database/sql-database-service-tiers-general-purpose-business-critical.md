---
title: De uso general y crítico para la empresa
description: En el artículo se describen los niveles de servicio de uso general y crítico para la empresa en el modelo de compra basado en núcleo virtual.
services: sql-database
ms.service: sql-database
ms.subservice: service
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: sashan, moslake, carlrab
ms.date: 01/30/2020
ms.openlocfilehash: 09cc9e1475616700aa77cdf92fd7ca808cd4290c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "76937834"
---
# <a name="azure-sql-database-service-tiers"></a>Niveles de servicio de Azure SQL Database

Azure SQL Database se basa en la arquitectura del motor de base de datos de SQL Server que se ajusta al entorno en la nube para garantizar una disponibilidad del 99,99 %, incluso en los casos de error de la infraestructura. En Azure SQL Database se usan tres niveles de servicio, cada uno con un modelo de arquitectura diferente. Estos niveles de servicio son:

- [De uso general](sql-database-service-tier-general-purpose.md), diseñado para la mayoría de cargas de trabajo guiadas por el presupuesto.
- [Hiperscala](sql-database-service-tier-hyperscale.md), diseñado para la mayoría de las cargas de trabajo empresariales; proporciona almacenamiento altamente escalable, escalado horizontal de lectura y capacidades rápidas de restauración de las bases de datos.
- [Crítico para la empresa](sql-database-service-tier-business-critical.md), diseñado para cargas de trabajo de baja latencia con alta resistencia a errores y conmutaciones por error rápidas.

Este artículo se analizan las diferencias entre los niveles de servicio, las consideraciones de almacenamiento y copia de seguridad de uso general y los niveles de servicio críticos para el negocio en el modelo de compra basado en núcleo virtual.

## <a name="service-tier-comparison"></a>Comparación de niveles de servicio

En la tabla siguiente se describen las diferencias principales entre los niveles de servicio de la última generación (Gen5). Tenga en cuenta que las características del nivel de servicio pueden ser diferentes en la Base de datos única y la Instancia administrada.

| | Tipo de recurso | Uso general |  Hiperescala | Crítico para la empresa |
|:---:|:---:|:---:|:---:|:---:|
| **Más adecuado para** | |  Ofrece opciones de proceso y almacenamiento equilibradas adecuadas para un presupuesto limitado. | La mayoría de las cargas de trabajo empresariales. Escalado automático del tamaño de almacenamiento hasta 100 TB, escalado de procesos vertical y horizontal fluido, restauración rápida de bases de datos. | Aplicaciones de OLTP con una alta tasa de transacciones y latencia de E/S baja. Ofrece mayor resistencia a los errores y rapidez en las conmutaciones por error mediante varias réplicas actualizadas sincrónicamente.|
|  **Disponible en estos tipos de recurso:** ||Base de datos única / grupo elástico / instancia administrada | Base de datos única | Base de datos única / grupo elástico / instancia administrada |
| **Tamaño de proceso**|Base de datos única / grupo elástico | 1 a 80 núcleos virtuales | 1 a 80 núcleos virtuales | 1 a 80 núcleos virtuales |
| | instancia administrada | 4, 8, 16, 24, 32, 40, 64 y 80 núcleos virtuales | N/D | 4, 8, 16, 24, 32, 40, 64 y 80 núcleos virtuales |
| | Grupos de instancias administradas | 2, 4, 8, 16, 24, 32, 40, 64 y 80 núcleos virtuales | N/D | N/D |
| **Tipo de almacenamiento** | All | Almacenamiento remoto Premium (por instancia) | Almacenamiento desacoplado con caché de SSD local (por instancia) | Almacenamiento SSD local extremadamente rápido (por instancia) |
| **Tamaño de la base de datos** | Base de datos única / grupo elástico | 5 GB – 4 TB | Hasta 100 TB | 5 GB – 4 TB |
| | instancia administrada  | 32 GB–8 TB | N/D | 32 GB – 4 TB |
| **Tamaño de almacenamiento** | Base de datos única / grupo elástico | 5 GB – 4 TB | Hasta 100 TB | 5 GB – 4 TB |
| | instancia administrada  | 32 GB–8 TB | N/D | 32 GB – 4 TB |
| **Tamaño de TEMPDB** | Base de datos única / grupo elástico | [32 GB por núcleo virtual](sql-database-vcore-resource-limits-single-databases.md#general-purpose---provisioned-compute---gen4) | [32 GB por núcleo virtual](sql-database-vcore-resource-limits-single-databases.md#hyperscale---provisioned-compute---gen5) | [32 GB por núcleo virtual](sql-database-vcore-resource-limits-single-databases.md#business-critical---provisioned-compute---gen4) |
| | instancia administrada  | [24 GB por núcleo virtual](sql-database-managed-instance-resource-limits.md#service-tier-characteristics) | N/D | Hasta 4 TB: [limitado en función del tamaño de almacenamiento](sql-database-managed-instance-resource-limits.md#service-tier-characteristics) |
| **Rendimiento de escritura de registros** | Base de datos única | [1,875 MB/s por núcleo virtual (máximo 30 MB/s)](sql-database-vcore-resource-limits-single-databases.md#general-purpose---provisioned-compute---gen4) | 100 MB/s | [6 MB/s por núcleo virtual (máximo 96 MB/s)](sql-database-vcore-resource-limits-single-databases.md#business-critical---provisioned-compute---gen4) |
| | instancia administrada | [3 MB/s por núcleo virtual (máximo 22 MB/s)](sql-database-managed-instance-resource-limits.md#service-tier-characteristics) | N/D | [4 MB/s por núcleo virtual (máximo 48 MB/s)](sql-database-managed-instance-resource-limits.md#service-tier-characteristics) |
|**Disponibilidad**|All| 99,99% |  [99,95 % con una réplica secundaria; 99,99 % con más réplicas](sql-database-service-tier-hyperscale-faq.md#what-slas-are-provided-for-a-hyperscale-database) | 99,99% <br/> [99,995 % con una base de datos única con redundancia de zona](https://azure.microsoft.com/blog/understanding-and-leveraging-azure-sql-database-sla/) |
|**Copias de seguridad**|All|RA-GRS, de 7 a 35 días (7 días de forma predeterminada)| RA-GRS, 7 días, recuperación a un momento dado (PITR) en un tiempo constante | RA-GRS, de 7 a 35 días (7 días de forma predeterminada) |
|**OLTP en memoria** | | N/D | N/D | Disponible |
|**Réplicas de solo lectura**| | 0 integradas <br> 0 a 4 con [replicación geográfica](sql-database-active-geo-replication.md) | 0 a 4 integradas | 1 integrada, incluida en el precio <br> 0 a 4 con [replicación geográfica](sql-database-active-geo-replication.md) |
|**Precios y facturación** | Base de datos única | [El núcleo virtual, el almacenamiento reservado y el almacenamiento de copia de seguridad](https://azure.microsoft.com/pricing/details/sql-database/single/) se cobran. <br/>Las IOPS no se cobran. | Se cobran los [núcleos virtuales de cada réplica y el almacenamiento usado](https://azure.microsoft.com/pricing/details/sql-database/single/). <br/>IOPS todavía no se ha cargado. | [El núcleo virtual, el almacenamiento reservado y el almacenamiento de copia de seguridad](https://azure.microsoft.com/pricing/details/sql-database/single/) se cobran. <br/>Las IOPS no se cobran. |
|| de SQL DB | [El núcleo virtual, el almacenamiento reservado y el almacenamiento de copia de seguridad](https://azure.microsoft.com/pricing/details/sql-database/managed/) se cobran. <br/>Las IOPS no se cobran.| N/D | [El núcleo virtual, el almacenamiento reservado y el almacenamiento de copia de seguridad](https://azure.microsoft.com/pricing/details/sql-database/managed/) se cobran. <br/>Las IOPS no se cobran.| 
|**Modelos de descuento**| | [Instancias reservadas](sql-database-reserved-capacity.md)<br/>[Ventaja híbrida de Azure](sql-database-azure-hybrid-benefit.md) (no disponible en suscripciones de desarrollo y pruebas)<br/>Suscripciones de Desarrollo/pruebas de [Enterprise](https://azure.microsoft.com/offers/ms-azr-0148p/) y [de pago por uso](https://azure.microsoft.com/offers/ms-azr-0023p/)| [Ventaja híbrida de Azure](sql-database-azure-hybrid-benefit.md) (no disponible en suscripciones de desarrollo y pruebas)<br/>Suscripciones de Desarrollo/pruebas de [Enterprise](https://azure.microsoft.com/offers/ms-azr-0148p/) y [de pago por uso](https://azure.microsoft.com/offers/ms-azr-0023p/)| [Instancias reservadas](sql-database-reserved-capacity.md)<br/>[Ventaja híbrida de Azure](sql-database-azure-hybrid-benefit.md) (no disponible en suscripciones de desarrollo y pruebas)<br/>Suscripciones de Desarrollo/pruebas de [Enterprise](https://azure.microsoft.com/offers/ms-azr-0148p/) y [de pago por uso](https://azure.microsoft.com/offers/ms-azr-0023p/)|

Para más información, consulte las diferencias detalladas entre los niveles de servicio en las páginas de la [base de datos única (núcleo virtual)](sql-database-vcore-resource-limits-single-databases.md), los [grupos de bases de datos únicas (núcleo virtual)](sql-database-dtu-resource-limits-single-databases.md), la [base de datos única (DTU)](sql-database-dtu-resource-limits-single-databases.md), los [grupos de bases de datos únicas(DTU)](sql-database-dtu-resource-limits-single-databases.md) y de [Instancia administrada](sql-database-managed-instance-resource-limits.md).

> [!NOTE]
> Para obtener información sobre el nivel de servicio hiperescala en el modelo de compra basado en núcleo virtual, consulte el [nivel de servicio de hiperescala](sql-database-service-tier-hyperscale.md). Para ver una comparación entre el modelo de compra basado en núcleo virtual y el modelo de compra basado en DTU, consulte [Modelos de compra y recursos de Azure SQL Database](sql-database-purchase-models.md).

## <a name="data-and-log-storage"></a>Almacenamiento de datos y de registro

Los siguientes factores afectan a la cantidad de almacenamiento utilizado para los datos y los archivos de registro, y se aplica a De uso general y Crítico para la empresa. Para información detallada sobre el almacenamiento de datos y registros en Hiperescala, consulte [Nivel de servicio Hiperescala](sql-database-service-tier-hyperscale.md).

- El almacenamiento asignado lo usan los archivos de datos (MDF) y los archivos de registro (LDF).
- Cada tamaño de proceso de una base de datos única admite un tamaño máximo de base de datos, con un tamaño máximo predeterminado de 32 GB.
- Cuando se configura el tamaño de base de datos única requerido (el tamaño del archivo MDF), el 30 % de almacenamiento adicional se agrega automáticamente para admitir archivos LDF.
- El tamaño de almacenamiento para la instancia administrada de SQL Database debe especificarse en múltiplos de 32 GB.
- Puede seleccionar cualquier tamaño de base de datos única entre 10 GB y el máximo admitido.
  - En el caso del almacenamiento en los niveles de servicio estándar o de uso general, aumente o disminuya el tamaño en incrementos de 10 GB.
  - Para el almacenamiento en niveles de servicio premium o crítico para la empresa, aumente o disminuya el tamaño en incrementos de 250 GB.
- En el nivel de servicio de uso general, `tempdb` usa una SSD asociada y este costo de almacenamiento se incluye en el precio del núcleo virtual.
- En el nivel de servicio crítico para la empresa, `tempdb` comparte la SSD asociada con los archivos MDF y LDF, y el costo de almacenamiento de `tempdb` se incluye en el precio del núcleo virtual.

> [!IMPORTANT]
> Se le cobra por el almacenamiento total asignado para los archivos MDF y LDF.

Para supervisar el tamaño total actual de los archivos MDF y LDF, use [sp_spaceused](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-spaceused-transact-sql). Para supervisar el tamaño actual de los archivos MDF y LDF individuales, use [sys.database_files](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-database-files-transact-sql).

> [!IMPORTANT]
> En algunas circunstancias, puede que deba reducir una base de datos para reclamar el espacio no utilizado. Para obtener más información, consulte [Administración del espacio de archivo en Azure SQL Database](sql-database-file-space-management.md).

## <a name="backups-and-storage"></a>Copias de seguridad y almacenamiento

Para admitir las funcionalidades de restauración a un momento dado (PITR) y [retención a largo plazo (LTR)](sql-database-long-term-retention.md) de SQL Database, se asigna almacenamiento a las copias de seguridad de base de datos. Este almacenamiento se asigna por separado para cada base de datos y se factura como dos cargos independientes por base de datos.

- **PITR**: las copias de seguridad de base de datos individuales se copian en el [almacenamiento con redundancia geográfica con acceso de lectura (RA-GRS)](../storage/common/storage-designing-ha-apps-with-ragrs.md) automáticamente. El tamaño de almacenamiento aumenta dinámicamente a medida que se crean nuevas copias de seguridad. El almacenamiento se usa para realizar cada cinco minutos copias de seguridad completas semanales, copias de seguridad diferenciales diarias y copias de seguridad de registros de transacciones. El consumo de almacenamiento depende de la tasa de cambio de la base de datos y del período de retención de las copias de seguridad. Puede configurar un período de retención diferente para cada base de datos de entre 7 y 35 días. Se ofrece una cantidad de almacenamiento mínimo igual al 100 % (1x) del tamaño de la base de datos sin costo adicional. En la mayoría de las bases de datos, esta cantidad es suficiente para almacenar copias de seguridad durante 7 días.
- **LTR**: SQL Database ofrece la opción de configurar la retención a largo plazo de las copias de seguridad completas durante un período máximo de 10 años. Si ha instalado la directiva de LTR, estas copias de seguridad se almacenan en almacenamiento RA-GRS automáticamente, pero puede controlar la frecuencia con que se realizan las copias de seguridad. Para satisfacer los distintos requisitos de cumplimiento, puede seleccionar distintos períodos de retención para copias de seguridad semanales, mensuales o anuales. La configuración que elija determina la cantidad de almacenamiento que se usará para las copias de seguridad de LTR. Para estimar el costo del almacenamiento de LTR, se puede usar la calculadora de precios de LTR. Para más información, consulte [SQL Database long-term retention](sql-database-long-term-retention.md) (Retención a largo plazo de SQL Database).

## <a name="next-steps"></a>Pasos siguientes

- Para obtener más información sobre las opciones de tamaños de proceso y de almacenamiento específicas que están disponibles para una base de datos única en los niveles de servicio de uso general y crítico para la empresa, consulte [Límites del modelo de compra basado en núcleo virtual de Azure SQL Database para una base de datos única](sql-database-vcore-resource-limits-single-databases.md).
- Para obtener más información sobre las opciones de tamaños de proceso y de almacenamiento específicas que están disponibles para los grupos elásticos en los niveles de servicio de uso general y crítico para la empresa, consulte [Límites del modelo de compra basado en núcleo virtual de Azure SQL Database para grupos elásticos](sql-database-vcore-resource-limits-elastic-pools.md).
