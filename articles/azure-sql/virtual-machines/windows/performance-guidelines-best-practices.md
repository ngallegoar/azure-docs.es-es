---
title: Directrices de rendimiento para SQL Server en Azure | Microsoft Docs
description: Ofrece directrices para optimizar el rendimiento de SQL Server en Microsoft Azure Virtual Machines.
services: virtual-machines-windows
documentationcenter: na
author: MashaMSFT
editor: ''
tags: azure-service-management
ms.assetid: a0c85092-2113-4982-b73a-4e80160bac36
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 11/09/2020
ms.author: mathoma
ms.reviewer: jroth
ms.openlocfilehash: 2cff67dde7cfe9e015cd25b26811410ce6e686e9
ms.sourcegitcommit: 6a350f39e2f04500ecb7235f5d88682eb4910ae8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/01/2020
ms.locfileid: "96462548"
---
# <a name="performance-guidelines-for-sql-server-on-azure-virtual-machines"></a>Directrices de rendimiento para SQL Server en Azure Virtual Machines
[!INCLUDE[appliesto-sqlvm](../../includes/appliesto-sqlvm.md)]

En este tema se ofrece una guía para optimizar el rendimiento de SQL Server en Microsoft Azure Virtual Machines.

## <a name="overview"></a>Información general

Mientras se ejecuta SQL Server en Azure Virtual Machines, se recomienda seguir usando las mismas opciones de ajuste de rendimiento de base de datos que son aplicables a SQL Server en los entornos de servidor local. Sin embargo, el rendimiento de una base de datos relacional en una nube pública depende de muchos factores como el tamaño de una máquina virtual y la configuración de los discos de datos.

Las [imágenes de SQL Server aprovisionadas en Azure Portal](sql-vm-create-portal-quickstart.md) siguen los [procedimientos recomendados de configuración](storage-configuration.md) de almacenamiento general. Después del aprovisionamiento, considere la posibilidad de aplicar otras de las optimizaciones que se describen en este artículo. Elija según la carga de trabajo y realice pruebas de verificación.

> [!TIP]
> Por lo general, existe un equilibrio entre la optimización de los costos y la optimización del rendimiento. Este artículo se centra en cómo obtener el *mejor* rendimiento de SQL Server en Azure Virtual Machines. Si su carga de trabajo no es menos exigente, podría no necesitar todas las optimizaciones enumeradas a continuación. Tenga en cuenta sus necesidades de rendimiento, costos y patrones de carga de trabajo a medida que evalúa estas recomendaciones.

## <a name="quick-checklist"></a>Lista de comprobación rápida

La siguiente es una lista de comprobación rápida para un rendimiento óptimo de SQL Server en Azure Virtual Machines:

| Área | Optimizaciones |
| --- | --- |
| [Tamaño de VM](#vm-size-guidance) | - Use tamaños de VM con 4 vCPU o más, como [Standard_M8-4ms](/../../virtual-machines/m-series), [E4ds_v4](../../../virtual-machines/edv4-edsv4-series.md#edv4-series) o [DS12_v2](../../../virtual-machines/dv2-dsv2-series-memory.md#dsv2-series-11-15) o superior. <br/><br/> - Use tamaños de máquinas virtuales [optimizadas para memoria](../../../virtual-machines/sizes-memory.md) para obtener el mejor rendimiento de las cargas de trabajo SQL Server. <br/><br/> - Las series [DSv2 11-15](../../../virtual-machines/dv2-dsv2-series-memory.md), [Edsv4](../../../virtual-machines/edv4-edsv4-series.md), [M-](../../../virtual-machines/m-series.md) y [MV2-](../../../virtual-machines/mv2-series.md) ofrecen la proporción óptima entre memoria y núcleo virtual necesaria para las cargas de trabajo de OLTP. Las series M ofrecen la mayor proporción de memoria a núcleo virtual necesaria para las cargas de trabajo críticas y también es ideal para cargas de trabajo de almacenamiento de datos. <br/><br/> - Se puede requerir una mayor proporción de memoria a núcleo virtual para cargas de trabajo críticas y de almacenamiento de datos. <br/><br/> - Aproveche las imágenes de Marketplace de máquina virtual de Azure, ya que la configuración de SQL Server y las opciones de almacenamiento están configuradas para un rendimiento óptimo de SQL Server. <br/><br/> - Recopile las características de rendimiento de la carga de trabajo de destino y úselas para determinar el tamaño de VM adecuado para su negocio.|
| [Storage](#storage-guidance) | - Para realizar pruebas detalladas del rendimiento de SQL Server en Azure Virtual Machines con los bancos de pruebas TPC-E y TPC_C, consulte el blog en el que se explica cómo [optimizar el rendimiento de OLTP](https://techcommunity.microsoft.com/t5/SQL-Server/Optimize-OLTP-Performance-with-SQL-Server-on-Azure-VM/ba-p/916794). <br/><br/> - Use [discos SSD Premium](https://techcommunity.microsoft.com/t5/SQL-Server/Optimize-OLTP-Performance-with-SQL-Server-on-Azure-VM/ba-p/916794) para obtener la mejor relación precio/rendimiento. Configure la [caché de solo lectura](../../../virtual-machines/premium-storage-performance.md#disk-caching) para los archivos de datos y que el archivo de registro no se almacene en la caché. <br/><br/> - Use [Ultra Disks](../../../virtual-machines/disks-types.md#ultra-disk) si la carga de trabajo requiere latencias de almacenamiento inferiores a 1 ms. Consulte cómo [realizar una migración a Disco Ultra](storage-migrate-to-ultradisk.md) para obtener más información. <br/><br/> - Recopile los requisitos de latencia de almacenamiento de los archivos de datos, de registro y de base de datos temporal de SQL Server, para lo que deberá [supervisar la aplicación](../../../virtual-machines/premium-storage-performance.md#application-performance-requirements-checklist) antes de elegir el tipo de disco. Si se requieren latencias de almacenamiento inferiores a 1 ms, use Ultra Disks; de lo contrario, use discos SSD Premium. Si solo se necesitan latencias bajas para el archivo de registro, no para los archivos de datos, [aprovisione el Ultra Disk](../../../virtual-machines/disks-enable-ultra-ssd.md) en los niveles de IOPS y rendimiento necesarios solo para el archivo de registro. <br/><br/>  - El almacenamiento estándar solo se recomienda con fines de desarrollo y realización de pruebas, o bien para archivos de copia de seguridad, y no debe usarse para cargas de trabajo de producción. <br/><br/> - Mantenga la [cuenta de almacenamiento](../../../storage/common/storage-account-create.md) y la máquina virtual con SQL Server en la misma región.<br/><br/> - Deshabilite el [almacenamiento con redundancia geográfica](../../../storage/common/storage-redundancy.md) (replicación geográfica) de Azure en la cuenta de almacenamiento.  |
| [Discos](#disks-guidance) | - Use un mínimo de 2 [discos SSD Premium](../../../virtual-machines/disks-types.md#premium-ssd) (uno para el archivo de registro y el otro para los archivos de datos). <br/><br/> - En cargas de trabajo que requieran latencias de E/S inferiores a 1 ms, habilite el acelerador de escritura, en el caso de la serie M, y considere la posibilidad de usar discos SSD Ultra en el caso de las series Es y DS. <br/><br/> - Habilite el [almacenamiento en caché de solo lectura](../../../virtual-machines/premium-storage-performance.md#disk-caching) en los discos que hospedan los archivos de datos.<br/><br/> - Agregue un 20 % más de capacidad de IOPS/rendimiento Premium de lo que requiere la carga de trabajo cuando [configure el almacenamiento para archivos de datos, de registro y de Temp DB de SQL Server](storage-configuration.md). <br/><br/> - Evite el uso del sistema operativo o de discos temporales para el registro o almacenamiento de bases de datos.<br/><br/> - No habilite el almacenamiento en caché en los discos que hospedan el archivo de registro.  **Importante**: Detenga el servicio de SQL Server al cambiar la configuración de caché para un disco de Azure Virtual Machines.<br/><br/> - Seccione varios discos de datos de Azure para obtener un mayor rendimiento de almacenamiento.<br/><br/> - Formatee con tamaños de asignación documentados. <br/><br/> - Coloque TempDB en el disco SSD local `D:\` para cargas de trabajo de SQL Server críticas (después de elegir el tamaño de máquina virtual correcto). Si crea la máquina virtual desde Azure Portal o con las plantillas de inicio rápido de Azure y [coloca Temp DB en el disco local](https://techcommunity.microsoft.com/t5/SQL-Server/Announcing-Performance-Optimized-Storage-Configuration-for-SQL/ba-p/891583), no es preciso que haga nada más; en los restantes casos, siga los pasos descritos en el blog acerca del [Uso de unidades de estado sólido para almacenar bases de datos temporales](https://cloudblogs.microsoft.com/sqlserver/2014/09/25/using-ssds-in-azure-vms-to-store-sql-server-TempDB-and-buffer-pool-extensions/) para evitar que se produzcan errores después de reiniciarse. Si la capacidad de la unidad local no es suficiente para el tamaño de la base de datos temporal, coloque la base de datos temporal en un bloque de almacenamiento [seccionado](../../../virtual-machines/premium-storage-performance.md) en discos SSD Premium con [almacenamiento en caché de solo lectura](../../../virtual-machines/premium-storage-performance.md#disk-caching). |
| [E/S](#io-guidance) |- Habilite la compresión de páginas de bases de datos.<br/><br/> - Habilite la inicialización instantánea de archivos para archivos de datos.<br/><br/> - Limite el crecimiento automático de la base de datos.<br/><br/> - Deshabilite la reducción automática de la base de datos.<br/><br/> - Mueva todas las bases de datos a discos de datos, incluidas las bases de datos del sistema.<br/><br/> - Mueva los directorios de archivos de seguimiento y registros de errores de SQL Server a discos de datos.<br/><br/> - Configure ubicaciones predeterminadas para los archivos de base de datos y de copia de seguridad.<br/><br/> - [Habilite las páginas bloqueadas en la memoria](/sql/database-engine/configure-windows/enable-the-lock-pages-in-memory-option-windows).<br/><br/> - Evalúe y aplique las [actualizaciones acumulativas más recientes](/sql/database-engine/install-windows/latest-updates-for-microsoft-sql-server) para la versión instalada de SQL Server. |
| [Características específicas](#feature-specific-guidance) | - Realice copias de seguridad directamente en Azure Blob Storage.<br/><br/>- Use [copias de seguridad de instantáneas de archivos](/sql/relational-databases/backup-restore/file-snapshot-backups-for-database-files-in-azure) en caso de que las bases de datos sean de más de 12 TB. <br/><br/>- Use varios archivos Temp BD, un archivo por núcleo, hasta ocho archivos.<br/><br/>-Establezca la memoria máxima de servidor en un 90 % o deje un máximo de 50 GB para el sistema operativo. <br/><br/>- Habilite el acceso a la memoria no uniforme temporal. |


<br/>
Para más información sobre *cómo* y *por qué* llevar a cabo estas optimizaciones, repase los detalles y las instrucciones proporcionadas en las siguientes secciones.
<br/><br/>

## <a name="getting-started"></a>Introducción

Si va a crear una nueva instancia de SQL Server en la máquina virtual de Azure y no va a migrar un sistema de origen actual, cree la nueva máquina virtual de SQL Server según los requisitos del proveedor.  Los requisitos de proveedor para una VM con SQL Server son los mismos que los que implementaría localmente. 

Si va a crear una nueva VM con SQL Server con una nueva aplicación compilada para la nube, puede cambiar fácilmente el tamaño de la VM con SQL Server a medida que evolucionen los requisitos de uso y datos.
Inicie los entornos de desarrollo con las series D, B y Av2 de nivel inferior y aumente el entorno con el tiempo. 

El mínimo recomendado para un entorno de OLTP de producción es 4 núcleos virtuales, 32 GB de memoria y una proporción de memoria a núcleo virtual de 8. En el caso de los nuevos entornos, empiece con 4 máquinas de núcleo virtual y modifique la escala a 8, 16, 32 núcleos virtuales o más cuando cambien los requisitos de datos y proceso. Para el rendimiento de OLTP, dirija las VM con SQL Server que tengan 5000 IOPS para cada núcleo virtual. 

Use las imágenes de marketplace de VM con SQL Server con la configuración de almacenamiento del portal. Esto hará que sea más fácil crear correctamente los grupos de almacenamiento necesarios para obtener el tamaño, IOPS y el rendimiento necesarios para las cargas de trabajo. Es importante elegir VM con SQL Server compatibles con Premium Storage y almacenamiento en caché de Premium Storage. Para más información, consulte la sección [Almacenamiento](#storage-guidance). 

El almacenamiento de datos de SQL Server y los entornos críticos suelen necesitar modificar la escala más allá de la proporción de memoria a núcleo virtual de 8. En el caso de los entornos de tamaño medio, puede que desee elegir una proporción de núcleo a memoria de 16 y, para entornos de almacenamiento de datos más grandes, una proporción de núcleo a memoria de 32. 

Los entornos de almacenamiento de datos de SQL Server suelen beneficiarse del procesamiento paralelo de máquinas de mayor tamaño. Por esta razón, la serie M y la serie Mv2 son opciones seguras para entornos de almacenamiento de datos más grandes.

## <a name="vm-size-guidance"></a>Orientación sobre el tamaño de máquina virtual

Use la vCPU y la configuración de memoria de la máquina de origen como base de referencia para migrar una base de datos de SQL Server local actual a SQL Server en máquinas virtuales de Azure. Traiga su licencia principal a Azure para aprovechar los beneficios de [Ventaja híbrida de Azure](https://azure.microsoft.com/pricing/hybrid-benefit/) y ahorre en los costos de licencias de SQL Server.

**Microsoft recomienda una proporción de memoria a núcleo virtual de 8 como punto inicial para cargas de trabajo de SQL Server de producción.** Las proporciones más pequeñas son aceptables para cargas de trabajo que no son de producción. 

Elija un tamaño de máquina virtual [optimizada para memoria](../../../virtual-machines/sizes-memory.md), [de uso general](../../../virtual-machines/sizes-general.md), [optimizada para almacenamiento](../../../virtual-machines/sizes-storage.md), o de [núcleo virtual restringido](../../../virtual-machines/constrained-vcpu.md) que sea más óptimo para el rendimiento de SQL Server en función de la carga de trabajo (OLTP o almacenamiento de datos). 

### <a name="memory-optimized"></a>Memoria optimizada

Los [tamaños de máquina virtual optimizada para memoria](../../../virtual-machines/sizes-memory.md) son un destino principal para VM con SQL Server y la opción recomendada por Microsoft. Las máquinas virtuales optimizadas para memoria ofrecen proporciones más seguras de memoria a CPU y opciones de caché de tamaño medio a grande. 

#### <a name="m-and-mv2-series"></a>Series M y Mv2

La [serie M](../../../virtual-machines/m-series.md) ofrece recuentos de núcleos virtuales y memoria para algunas de las cargas de trabajo de SQL Server más grandes.  

La [serie Mv2](../../../virtual-machines/mv2-series.md) tiene los mayores recuentos de núcleos virtuales y memoria y se recomienda para cargas de trabajo críticas y de almacenamiento de datos. Las instancias de la serie Mv2 son tamaños de máquinas virtuales optimizados para memoria que proporcionan un rendimiento de proceso sin precedentes para admitir grandes bases de datos y cargas de trabajo en memoria, con una proporción elevada de memoria y CPU que es perfecta para servidores de bases de datos relacionales, grandes almacenamientos en caché y análisis en memoria.

Por ejemplo, [Standard_M64ms](../../../virtual-machines/m-series.md) tiene una proporción de memoria a núcleo virtual de 28.

Algunas de las características de la serie M y Mv2 atractivas para el rendimiento de SQL Server incluyen compatibilidad con [Premium Storage](../../../virtual-machines/premium-storage-performance.md) y [almacenamiento en caché de Premium Storage](../../../virtual-machines/premium-storage-performance.md#disk-caching), compatibilidad con [disco Ultra](../../../virtual-machines/disks-enable-ultra-ssd.md) y [aceleración de escritura](../../../virtual-machines/how-to-enable-write-accelerator.md).

#### <a name="edsv4-series"></a>Serie Edsv4

La [serie Edsv4](../../../virtual-machines/edv4-edsv4-series.md) está diseñada para aplicaciones con un uso intensivo de memoria. Estas máquinas virtuales tienen una gran capacidad de SSD de almacenamiento local, IOPS de disco local seguro, hasta 504 GiB de RAM y un proceso mejorado en comparación con los tamaños anteriores de Ev3/Esv3 con máquinas virtuales Gen2. Hay una proporción de memoria a núcleo virtual de 8 casi coherente en todas estas máquinas virtuales, que es ideal para cargas de trabajo de SQL Server estándar. 

Esta serie de VM es ideales para aplicaciones empresariales de uso intensivo de memoria y aplicaciones que se benefician del almacenamiento local de baja latencia y alta velocidad.

Las máquinas virtuales de la serie Edsv4 admiten [Premium Storage](../../../virtual-machines/premium-storage-performance.md) y [almacenamiento en caché de Premium Storage](../../../virtual-machines/premium-storage-performance.md#disk-caching).

#### <a name="dsv2-series-11-15"></a>DSv2-series 11-15

La [serie DSv2 11-15](../../../virtual-machines/dv2-dsv2-series-memory.md#dsv2-series-11-15) tiene las mismas configuraciones de disco y memoria que la serie D. Esta serie tiene una proporción de memoria a CPU de 7 coherente en todas las máquinas virtuales. 

La [serie DSv2 11-15](../../../virtual-machines/dv2-dsv2-series-memory.md#dsv2-series-11-15) admite [Premium Storage](../../../virtual-machines/premium-storage-performance.md) y [almacenamiento en caché de Premium Storage](../../../virtual-machines/premium-storage-performance.md#disk-caching), que se recomienda encarecidamente para un rendimiento óptimo.

### <a name="general-purpose"></a>De uso general

Los [tamaños de máquina virtual de uso general](../../../virtual-machines/sizes-general.md) están diseñados para ofrecer proporciones de memoria a núcleo virtual equilibradas para cargas de trabajo de nivel de entrada más pequeñas, como desarrollo y pruebas, servidores web y servidores de bases de datos más pequeños. 

Debido a las proporciones de memoria a núcleo virtual más pequeñas con las máquinas virtuales de uso general, es importante supervisar detenidamente los contadores de rendimiento basados en memoria para asegurarse de que SQL Server sea capaz de obtener la memoria caché del búfer que necesita. Para obtener más información, consulte [Línea base de rendimiento de memoria](#memory). 

Dado que la recomendación de inicio para las cargas de trabajo de producción es una proporción de memoria a núcleo virtual de 8, la configuración mínima recomendada para una máquina virtual de uso general que ejecuta SQL Server es de 4 vCPU y 32 GB de memoria. 

#### <a name="ddsv4-series"></a>Serie Ddsv4

La [serie Ddsv4](../../../virtual-machines/ddv4-ddsv4-series.md) ofrece una combinación justa de vCPU, memoria y disco temporal, pero con una compatibilidad menor de memoria a núcleo virtual. 

Las VM de Ddsv4 incluyen latencia inferior y un almacenamiento local de mayor velocidad.

Estas máquinas son ideales para implementaciones de SQL y aplicaciones en paralelo que requieren un acceso rápido a las bases de datos relacionales del almacenamiento temporal y del departamento. Hay una proporción de memoria a núcleo virtual estándar de 4 en todas las máquinas virtuales de esta serie. 

Por esta razón, se recomienda aprovechar D8ds_v4 como la máquina virtual de inicio de esta serie, que tiene 8 núcleos virtuales y 32 GB de memoria. El equipo más grande es D64ds_v4, que tiene 64 núcleos virtuales y 256 GB de memoria.

Las máquinas virtuales de la [serie Ddsv4](../../../virtual-machines/ddv4-ddsv4-series.md) admiten [Premium Storage](../../../virtual-machines/premium-storage-performance.md) y [almacenamiento en caché de Premium Storage](../../../virtual-machines/premium-storage-performance.md#disk-caching).

> [!NOTE]
> La [serie Ddsv4](../../../virtual-machines/ddv4-ddsv4-series.md) no tiene la proporción de memoria a núcleo virtual de 8 que se recomienda para las cargas de trabajo de SQL Server. Como tal, considere la posibilidad de usar estas máquinas virtuales para cargas de trabajo de desarrollo y aplicaciones más pequeñas únicamente.

#### <a name="b-series"></a>Serie B

Los tamaños de las máquinas virtuales [ampliables de serie B](../../../virtual-machines/sizes-b-series-burstable.md) son ideales para cargas de trabajo que no necesitan un rendimiento coherente, como prueba de concepto y servidores de desarrollo y aplicaciones muy pequeños. 

La mayoría de los tamaños de las máquinas virtuales [ampliables de serie B](../../../virtual-machines/sizes-b-series-burstable.md) tienen una proporción de memoria a núcleo virtual de 4. La mayor de estas máquinas es [Standard_B20ms](../../../virtual-machines/sizes-b-series-burstable.md) con 20 núcleos virtuales y 80 GB de memoria.

Esta serie es única, ya que las aplicaciones tienen la capacidad de **ampliar** durante el horario comercial con créditos ampliables en función del tamaño de la máquina. 

Cuando se acaban los créditos, la VM vuelve al rendimiento de la máquina de base de referencia.

La ventaja de la serie B es el ahorro de proceso que podría lograr en comparación con los demás tamaños de máquina virtual de otras series, especialmente si necesita la potencia de procesamiento con moderación a lo largo del día.

Esta serie admite [Premium Storage](../../../virtual-machines/premium-storage-performance.md), pero **no admite** [almacenamiento en caché de Premium Storage](../../../virtual-machines/premium-storage-performance.md#disk-caching).

> [!NOTE] 
> La [serie B ampliable](../../../virtual-machines/sizes-b-series-burstable.md) no tiene la proporción de memoria a núcleo virtual de 8 que se recomienda para las cargas de trabajo de SQL Server. Como tal, considere la posibilidad de usar estas máquinas virtuales para cargas de trabajo de desarrollo, servidores web y aplicaciones más pequeñas únicamente.

#### <a name="av2-series"></a>Serie Av2

Las máquinas virtuales de la [serie Av2](../../../virtual-machines/av2-series.md) son más adecuadas para cargas de trabajo de nivel de entrada, como desarrollo y pruebas, servidores web de tráfico bajo, bases de datos de aplicaciones de tamaño pequeño a medio y prueba de conceptos.

Solo [Standard_A2m_v2](../../../virtual-machines/av2-series.md) (2 núcleos virtuales y 16 GB de memoria), [Standard_A4m_v2](../../../virtual-machines/av2-series.md) (4 núcleos virtuales y 32 GB de memoria) y [Standard_A8m_v2](../../../virtual-machines/av2-series.md) (8 núcleos virtuales y 64 GB de memoria) tienen una buena proporción de memoria a núcleo virtual de 8 para estas tres máquinas virtuales principales. 

Estas máquinas virtuales son buenas opciones para máquinas de SQL Server de desarrollo y pruebas más pequeñas. 

[Standard_A8m_v2](../../../virtual-machines/av2-series.md) de 8 núcleos virtuales también puede ser una buena opción para aplicaciones y servidores web pequeños.

> [!NOTE] 
> La serie Av2 no admite Premium Storage y, por tanto, no se recomienda para las cargas de trabajo de SQL Server de producción, incluso con las máquinas virtuales que tienen una proporción de memoria a núcleo virtual de 8.

### <a name="storage-optimized"></a>Almacenamiento optimizado

Los [tamaños de máquina virtual optimizada para almacenamiento](../../../virtual-machines/sizes-storage.md) son para casos de uso específicos. Estas máquinas virtuales están diseñadas específicamente con rendimiento optimizado de disco y E/S. Esta serie de máquinas virtuales está pensada para escenarios de macrodatos, almacenamiento de datos y bases de datos transaccionales grandes. 

#### <a name="lsv2-series"></a>Serie Lsv2

Las máquinas virtuales de la [serie Lsv2](../../../virtual-machines/lsv2-series.md) presentan alto rendimiento, baja latencia y almacenamiento local de NVMe. Las máquinas virtuales de la serie Lsv2 están optimizadas para usar el disco local del nodo conectado directamente a la máquina virtual, en lugar de utilizar discos de datos duraderos. 

Estas máquinas virtuales son opciones seguras para macrodatos, almacenamiento de datos, informes y cargas de trabajo de ETL. El alto rendimiento e IOPS del almacenamiento local de NVMe es un buen caso de uso para procesar archivos que se cargarán en la base de datos y en otros escenarios donde los datos de origen se pueden volver a crear desde el sistema de origen u otros repositorios, como Azure Blob Storage o Azure Data Lake. Las VM de la [serie Lsv2](../../../virtual-machines/lsv2-series.md) también pueden expandir su rendimiento de disco hasta 30 minutos cada vez.

Estas máquinas virtuales tienen un tamaño de 8 a 80 vCPU con 8 GiB de memoria por vCPU y por cada 8 vCPU hay 1,92 TB de SSD de NVMe. Esto significa que para la máquina virtual más grande de esta serie, la [L80s_v2](../../../virtual-machines/lsv2-series.md), hay 80 vCPU y 640 BiB de memoria con 10x1,92 TB de almacenamiento de NVMe.  Hay una proporción de memoria a núcleo virtual coherente de 8 en todas estas máquinas virtuales.

El almacenamiento de NVMe es efímero, lo que significa que los datos se perderán en estos discos si reinicia la máquina virtual.

Las series Lsv2 y Ls admiten [Premium Storage](../../../virtual-machines/premium-storage-performance.md), pero no almacenamiento en caché de Premium Storage. No se admite la creación de una memoria caché local para aumentar IOPS. 

> [!WARNING]
> Si almacena los archivos de datos en el almacenamiento efímero de NVMe, podría provocar la pérdida de datos cuando se desasigne la máquina virtual. 

### <a name="constrained-vcores"></a>Núcleos virtuales restringidos

Las cargas de trabajo de alto rendimiento de SQL Server suelen necesitar grandes cantidades de memoria, E/S y rendimiento sin los mayores recuentos de núcleos virtuales. 

La mayoría de las cargas de trabajo de OLTP son bases de datos de aplicación controladas por un gran número de transacciones más pequeñas. Con las cargas de trabajo de OLTP, solo se lee o se modifica una pequeña cantidad de datos, pero los volúmenes de transacciones controlados por recuentos de usuarios son mucho mayores. Es importante tener la memoria de SQL Server disponible para los planes de caché, almacenar los datos a los que se ha tenido acceso recientemente para el rendimiento y asegurarse de que las lecturas físicas se pueden leer en la memoria rápidamente. 

Estos entornos de OLTP necesitan mayor cantidad de memoria, almacenamiento rápido y el ancho de banda de E/S necesario para que funcionen de forma óptima. 

Con el fin de mantener este nivel de rendimiento sin los mayores costos de licencias de SQL Server, Azure ofrece tamaños de máquina virtual con [recuentos de vCPU restringidas](../../../virtual-machines/constrained-vcpu.md). 

Esto ayuda a controlar los costos de las licencias al reducir los núcleos virtuales disponibles manteniendo la misma memoria, almacenamiento y ancho de banda de E/S de la máquina virtual principal.

Se puede restringir el número de vCPU a la mitad o un cuarto del tamaño de VM original. Al reducir los núcleos virtuales disponibles para la máquina virtual, se logran mayores proporciones de memoria a núcleo virtual.

Estos nuevos tamaños de VM tienen un sufijo que especifica el número de vCPU activas para facilitar su identificación. 

Por ejemplo, [M64-32ms](../../../virtual-machines/constrained-vcpu.md) solo requiere la concesión de licencias de 32 núcleos virtuales de SQL Server con la memoria, E/S y el rendimiento de [M64ms](../../../virtual-machines/m-series.md), y [M64-16ms](../../../virtual-machines/constrained-vcpu.md) solo requiere la concesión de licencias de 16 núcleos virtuales.  Aunque [M64-16ms](../../../virtual-machines/constrained-vcpu.md) supone un cuarto del costo de las licencias de SQL Server de M64ms, el costo de proceso de la máquina virtual será el mismo.

> [!NOTE] 
> - Las cargas de trabajo de almacenamiento de datos de tamaño medio a grande pueden seguir beneficiándose de las [VM de núcleo virtual restringido](../../../virtual-machines/constrained-vcpu.md), pero las cargas de trabajo de almacenamiento de datos se caracterizan normalmente por menos usuarios y procesos que tratan mayores cantidades de datos a través de planes de consulta que se ejecutan en paralelo. 
> - El costo de proceso, que incluye las licencias del sistema operativo, seguirá siendo el mismo que el de la máquina virtual principal. 

## <a name="storage-guidance"></a>Orientación sobre el almacenamiento

Para realizar pruebas detalladas del rendimiento de SQL Server en Azure Virtual Machines con los bancos de pruebas TPC-E y TPC-C, consulte el blog en el que se explica cómo [optimizar el rendimiento de OLTP](https://techcommunity.microsoft.com/t5/SQL-Server/Optimize-OLTP-Performance-with-SQL-Server-on-Azure-VM/ba-p/916794). 

Para las cargas de trabajo de producción se recomienda la caché de blobs de Azure con discos SSD Premium. 

> [!WARNING]
> Los HDD estándar y las unidades de estado sólido (SSD) presentan ancho de banda y latencias variables, de modo que solo se recomiendan para cargas de trabajo de desarrollo o de prueba. En las cargas de trabajo de producción conviene usar SSD prémium.

Además, se recomienda crear su cuenta de almacenamiento de Azure en el mismo centro de datos que sus máquinas virtuales de SQL Server para reducir los retrasos en la transferencia. Al crear una cuenta de almacenamiento, deshabilite la replicación geográfica ya que no se garantiza el orden de escritura coherente entre varios discos. En su lugar, considere la posibilidad de configurar una tecnología de recuperación ante desastres de SQL Server entre dos centros de datos de Azure. Para más información, consulte [Alta disponibilidad y recuperación ante desastres para SQL Server en Azure Virtual Machines](business-continuity-high-availability-disaster-recovery-hadr-overview.md).

## <a name="disks-guidance"></a>Orientación sobre los discos

En las máquinas virtuales de Azure hay tres tipos de disco principales:

* **Disco del sistema operativo**: al crear una máquina virtual de Azure, la plataforma conectará al menos un disco (etiquetado como unidad **C**) a la máquina virtual como disco del sistema operativo. El disco es un VHD almacenado como blob en páginas en almacenamiento.
* **Disco temporal**: las máquinas virtuales de Azure contienen otro disco denominado disco temporal (etiquetado como unidad **D**:). Se trata de un disco en el nodo que se puede usar para el espacio de desecho.
* **Discos de datos**: También puede conectar discos adicionales a la máquina virtual como discos de datos y estos se almacenarán en el almacenamiento como blobs en páginas.

En las siguientes secciones se incluyen recomendaciones sobre cómo usar cada uno de estos discos.

### <a name="operating-system-disk"></a>Disco del sistema operativo

Un disco del sistema operativo es un VHD que se puede arrancar y montar como una versión en ejecución de un sistema operativo y está etiquetado como la unidad **C**.

La directiva del almacenamiento en caché predeterminada en el disco del sistema operativo es **Lectura/escritura**. Para aplicaciones sensibles al rendimiento, se recomienda usar discos de datos en lugar del disco del sistema operativo. Vea la sección sobre Discos de datos a continuación.

### <a name="temporary-disk"></a>Disco temporal

La unidad de almacenamiento temporal, etiquetada como unidad **D**, no se conserva en Azure Blob Storage. No almacene archivos de base de datos de usuarios ni archivos de registro de transacciones de usuarios en la unidad **D:** .

Coloque TempDB en la unidad de estado sólido local `D:\` para cargas de trabajo de SQL Server críticas (después de elegir el tamaño de máquina virtual correcto). Si crea la máquina virtual desde Azure Portal o con las plantillas de inicio rápido de Azure y [coloca Temp DB en el disco local](https://techcommunity.microsoft.com/t5/SQL-Server/Announcing-Performance-Optimized-Storage-Configuration-for-SQL/ba-p/891583), no es preciso que haga nada más; en los restantes casos, siga los pasos descritos en el blog acerca del [Uso de unidades de estado sólido para almacenar bases de datos temporales](https://cloudblogs.microsoft.com/sqlserver/2014/09/25/using-ssds-in-azure-vms-to-store-sql-server-TempDB-and-buffer-pool-extensions/) para evitar que se produzcan errores después de reiniciarse. Si la capacidad de la unidad local no es suficiente para el tamaño de la base de datos temporal, coloque la base de datos temporal en un bloque de almacenamiento [seccionado](../../../virtual-machines/premium-storage-performance.md) en discos SSD Premium con [almacenamiento en caché de solo lectura](../../../virtual-machines/premium-storage-performance.md#disk-caching).

En las máquinas virtuales que admiten discos SSD Premium, las bases de datos temporales también se puede almacenar en un disco que admita discos SSD Premium con el almacenamiento en caché de lectura habilitado.


### <a name="data-disks"></a>Discos de datos.

* **Uso de discos SSD Premium para archivos de datos y de registro**: si no usa el seccionamiento de discos, utilice dos discos SSD Premium, uno para el archivo de registro y otro para los datos. Cada disco SSD Premium proporciona un número de IOPS y ancho de banda (MB/s) según su tamaño, como se describe en el artículo en el que se indica cómo se debe [seleccionar el tipo de disco](../../../virtual-machines/disks-types.md). Si usa una técnica de fragmentación de discos, como los espacios de almacenamiento, puede lograr un rendimiento óptimo si tiene dos grupos: uno para los archivos de registro y otro para los archivos de datos. Sin embargo, si planea usar instancias de clúster de conmutación por error (FCI) de SQL Server (FCI), debe configurar un grupo o utilizar [recursos compartidos de archivos Premium](failover-cluster-instance-premium-file-share-manually-configure.md).

   > [!TIP]
   > - Para obtener resultados de la pruebas con varias configuraciones de discos y cargas de trabajo, consulte la siguiente entrada de blog: [Instrucciones de configuración del almacenamiento para SQL Server en Azure Virtual Machines](/archive/blogs/sqlserverstorageengine/storage-configuration-guidelines-for-sql-server-on-azure-vm).
   > - Para obtener un rendimiento crítico en servidores con SQL Server que necesitan aproximadamente 50.000 E/S por segundo, considere la posibilidad de reemplazar 10 - discos P30 por uno SSD Ultra. Para más información, consulte la siguiente entrada de blog: [Mission critical performance with Ultra SSD](https://azure.microsoft.com/blog/mission-critical-performance-with-ultra-ssd-for-sql-server-on-azure-vm/) (Rendimiento crítico con SSD Ultra).

   > [!NOTE]
   > Al aprovisionar una máquina virtual de SQL Server en el portal, tiene la opción de modificar la configuración de almacenamiento. Según la configuración, Azure configura uno o varios discos. Varios discos se combinan en un único grupo de almacenamiento con seccionamiento. Los archivos de datos y de registro residen juntos en esta configuración. Para más información, consulte [Configuración del almacenamiento para máquinas virtuales de SQL Server](storage-configuration.md).

* **Seccionamiento de discos**: Para disfrutar de un mayor rendimiento, puede agregar más discos de datos y usar el seccionamiento de discos. Para determinar el número de discos de datos, debe analizar el número de IOPS y ancho de banda necesario para los archivos de registro, así como para los datos y los archivos TempDB. Observe que diferentes tamaños de máquinas virtuales tienen distintos límites en el número de IOPS y ancho de banda admitidos. Vea las tablas de IOPS por [tamaño de máquina virtual](../../../virtual-machines/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Use estas directrices:

  * Para Windows 8/Windows Server 2012 o posterior, use [espacios de almacenamiento](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831739(v=ws.11)) con las siguientes directrices:

      1. Configure la intercalación (tamaño de sección) en 64 KB (65 536 bytes) en el caso de las cargas de trabajo de OLTP y 256 KB (262 144 bytes) en el caso de las cargas de trabajo de almacenamiento de datos para evitar que el rendimiento se vea afectado debido a una mala alineación de las particiones. Esta característica debe establecerse con PowerShell.
      2. Establezca recuento de columnas = número de discos físicos. Use PowerShell (no la interfaz de usuario del Administrador del servidor) al configurar más de 8 discos. 

    Por ejemplo, aquí PowerShell crea un nuevo grupo de almacenamiento con el tamaño de intercalación de 64 KB y un número de columnas igual a la cantidad de disco físico en el bloque de almacenamiento:

    ```powershell
    $PhysicalDisks = Get-PhysicalDisk | Where-Object {$_.FriendlyName -like "*2" -or $_.FriendlyName -like "*3"}
    
    New-StoragePool -FriendlyName "DataFiles" -StorageSubsystemFriendlyName "Storage Spaces*" `
        -PhysicalDisks $PhysicalDisks | New- VirtualDisk -FriendlyName "DataFiles" `
        -Interleave 65536 -NumberOfColumns $PhysicalDisks .Count -ResiliencySettingName simple `
        –UseMaximumSize |Initialize-Disk -PartitionStyle GPT -PassThru |New-Partition -AssignDriveLetter `
        -UseMaximumSize |Format-Volume -FileSystem NTFS -NewFileSystemLabel "DataDisks" `
        -AllocationUnitSize 65536 -Confirm:$false 
    ```

  * Para Windows 2008 R2 o versiones anteriores, puede usar discos dinámicos (volúmenes seccionados del SO) y el tamaño de la franja siempre es 64 KB. Esta opción está en desuso a partir de Windows 8 y Windows Server 2012. Para obtener información, vea la declaración de soporte técnico en [Servicio de disco virtual está realizando la transición a la API de administración de almacenamiento de Windows](/windows/win32/w8cookbook/vds-is-transitioning-to-wmiv2-based-windows-storage-management-api).

  * Si usa [Espacios de almacenamiento directo (S2D)](/windows-server/storage/storage-spaces/storage-spaces-direct-in-vm) con [instancias del clúster de conmutación por error de SQL Server](failover-cluster-instance-storage-spaces-direct-manually-configure.md), debe configurar un solo grupo. Aunque se pueden crear diferentes volúmenes en ese único grupo, todos ellos compartirán las mismas características, como por ejemplo, la misma directiva de almacenamiento en caché.

  * Determine el número de discos asociados al grupo de almacenamiento en función de sus expectativas de carga. Tenga en cuenta que diferentes tamaños de máquina virtual permiten diferentes números de discos de datos conectados. Para más información, consulte [Tamaños de las máquinas virtuales Linux en Azure](../../../virtual-machines/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

  * Si no usa SSD Premium (escenarios de desarrollo y pruebas), se recomienda agregar el número máximo de discos de datos admitidos por el [tamaño de la máquina virtual](../../../virtual-machines/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) y usar el seccionamiento de discos.

* **Directiva de caché**: tenga en cuenta las siguientes recomendaciones para la directiva según la configuración del almacenamiento en caché.

  * Si usa discos independientes para los archivos de datos y de registro, habilite el almacenamiento en caché de lectura en los discos de datos que hospeden los archivos de datos y de datos de TempDB. Esto puede suponer una mejora del rendimiento significativa. No habilite el almacenamiento en caché en el disco que contiene el archivo de registro, ya que esto produce una pequeña disminución del rendimiento.

  * Si usa la fragmentación de discos en un solo grupo de almacenamiento, la mayoría de las cargas de trabajo se beneficiarán de la memoria caché de lectura. Si tiene grupos de almacenamiento independientes para los archivos de registro y de datos, habilite el almacenamiento en caché de lectura solo en el grupo de almacenamiento de los archivos de datos. Para ciertas cargas de trabajo de escritura intensivas, podría lograr un mejor rendimiento sin almacenamiento en caché. Esto solo se puede determinar mediante pruebas.

  * Las recomendaciones anteriores se aplican a los SSD prémium. Si no usa SSD prémium, no habilite el almacenamiento en caché en los discos de datos.

  * Para obtener instrucciones sobre la configuración del almacenamiento en caché de disco, consulte los siguientes artículos. Para el modelo de implementación clásico (ASM), consulte: [Set-AzureOSDisk](/previous-versions/azure/jj152847(v=azure.100)) y [Set-AzureDataDisk](/previous-versions/azure/jj152851(v=azure.100)). Para el modelo de implementación mediante Azure Resource Manager, consulte: [Set-AzOSDisk](/powershell/module/az.compute/set-azvmosdisk) y [Set-AzVMDataDisk](/powershell/module/az.compute/set-azvmdatadisk).

     > [!WARNING]
     > Detenga el servicio SQL Server al cambiar la configuración de la caché de los discos de Azure Virtual Machines para evitar la posibilidad de que se produzcan daños en la base de datos.

* **Tamaño de la unidad de asignación de NTFS**: Al formatear el disco de datos, se recomienda usar un tamaño de unidad de asignación de 64 KB para los archivos de datos y de registro, además de TempDB. Si TempDB se incluye en el disco temporal (unidad D:\), el rendimiento obtenido aprovechando esta unidad supera la necesidad de un tamaño de la unidad de asignación de 64 KB. 

* **Procedimientos recomendados de administración de discos**: al quitar un disco de datos o cambiar su tipo de caché, detenga el servicio SQL Server durante el cambio. Cuando se modifica la configuración de almacenamiento en caché en el disco del sistema operativo, Azure detiene la máquina virtual, cambia el tipo de caché y reinicia la máquina virtual. Cuando se modifica la configuración de la caché de un disco de datos, no se detiene la máquina virtual, pero el disco de datos se desconecta de la máquina virtual durante el cambio y, posteriormente, se vuelve a conectar.

  > [!WARNING]
  > De no detenerse el servicio SQL Server durante estas operaciones, podrían producirse daños en la base de datos.


## <a name="io-guidance"></a>Orientación sobre E/S

* Los mejores resultados con las unidades de estado sólido prémium se logran al establecer paralelismos entre la aplicación y las solicitudes. Las SSD prémium están diseñadas para escenarios donde la profundidad de la cola de E/S es mayor que 1, por lo que verá poco o ningún aumento del rendimiento para las solicitudes de serie de subproceso único (incluso si son intensivas de almacenamiento). Por ejemplo, esto podría afectar a los resultados de pruebas de subproceso único de herramientas de análisis de rendimiento, como SQLIO.

* Recuerde que también puede usar la [compresión de páginas de bases de datos](/sql/relational-databases/data-compression/data-compression) , ya que puede ayudarle a mejorar el rendimiento de las cargas de trabajo intensivas de E/S. Sin embargo, la compresión de datos puede aumentar el consumo de la CPU en el servidor de bases de datos.

* Considere la posibilidad de habilitar la inicialización instantánea de archivos para reducir el tiempo necesario para la asignación inicial de archivos. Para aprovechar la inicialización instantánea de archivos, se concede la cuenta de servicio de SQL Server (MSSQLSERVER) con SE_MANAGE_VOLUME_NAME y se agrega a la directiva de seguridad **Realizar tareas de mantenimiento del volumen**. Si usa una imagen de la plataforma de SQL Server para Azure, la cuenta de servicio predeterminada (NT Service\MSSQLSERVER) no se agrega a la directiva de seguridad **Realizar tareas de mantenimiento del volumen**. En otras palabras, la inicialización instantánea de archivos no se habilita en una imagen de plataforma de SQL Server Azure. Después de agregar la cuenta de servicio de SQL Server a la directiva de seguridad **Realizar tareas de mantenimiento del volumen** , reinicie el servicio de SQL Server. Puede haber consideraciones de seguridad para usar esta característica. Para obtener más información, vea [Inicialización de archivos de base de datos](/sql/relational-databases/databases/database-instant-file-initialization).

* Tenga en cuenta que el **crecimiento automático** apenas se considera una contingencia para el crecimiento inesperado. No administre su crecimiento de datos y registros a diario con el crecimiento automático. Si se usa el crecimiento automático, haga crecer previamente el archivo mediante el modificador Tamaño.

* Asegúrese de que la **reducción automática** está deshabilitada para evitar una sobrecarga innecesaria que puede afectar negativamente al rendimiento.

* Mueva todas las bases de datos a discos de datos, incluidas bases de datos del sistema. Para obtener más información, vea [Mover bases de datos del sistema](/sql/relational-databases/databases/move-system-databases).

* Mueva los directorios de archivos de seguimiento y registros de errores de SQL Server a discos de datos. Para llevar esto a cabo en el Administrador de configuración de SQL Server, haga clic con el botón secundario en la instancia de SQL Server y seleccione Propiedades. La configuración de los archivos de registro y de seguimiento de errores se puede cambiar en la pestaña **Parámetros de inicio** . El directorio de volcado se especifica en la pestaña **Opciones avanzadas** . En la siguiente captura de pantalla se muestra dónde buscar el parámetro de inicio del registro de errores.

    ![Captura de pantalla de registro de errores de SQL](./media/performance-guidelines-best-practices/sql_server_error_log_location.png)

* Configure ubicaciones predeterminadas para los archivos de base de datos y de copia de seguridad. Use las recomendaciones de este artículo y realice los cambios necesarios en la ventana Propiedades del servidor. Para obtener instrucciones, consulte [Ver o cambiar las ubicaciones predeterminadas de los archivos de datos y registro (SQL Server Management Studio)](/sql/database-engine/configure-windows/view-or-change-the-default-locations-for-data-and-log-files). En la siguiente captura de pantalla se muestra dónde realizar estos cambios.

    ![Archivos de copia de seguridad y de registro de datos de SQL](./media/performance-guidelines-best-practices/sql_server_default_data_log_backup_locations.png)
* Habilite páginas bloqueadas para reducir la E/S y las actividades de paginación. Para más información, consulte [Habilitar la opción Bloquear páginas en la memoria (Windows)](/sql/database-engine/configure-windows/enable-the-lock-pages-in-memory-option-windows).

* Si está ejecutando SQL Server 2012, instale la actualización acumulativa 10 del Service Pack 1. Esta actualización contiene la corrección para el rendimiento deficiente en la E/S al ejecutar select en la instrucción de tabla temporal en SQL Server 2012. Para obtener información, consulte este [artículo de Knowledge Base](https://support.microsoft.com/kb/2958012).

* Considere la posibilidad de comprimir los archivos de datos cuando se transfieren dentro y fuera de Azure.

## <a name="feature-specific-guidance"></a>Guía específica de la característica

Algunas implementaciones pueden lograr ventajas de rendimiento adicionales mediante técnicas de configuración más avanzadas. En la siguiente lista se destacan algunas características de SQL Server que pueden ayudarle a lograr un mejor rendimiento:

### <a name="back-up-to-azure-storage"></a>Copia de seguridad en Azure Storage
Al realizar copias de seguridad para SQL Server que se ejecutan en Azure Virtual Machines, puede usar [Copia de seguridad en URL de SQL Server](/sql/relational-databases/backup-restore/sql-server-backup-to-url). Esta característica está disponible a partir de SQL Server 2012 SP1 CU2 y se recomienda para las copias de seguridad en los discos de datos conectados. Al realizar copias de seguridad o restaurar en o desde Azure Storage, siga las recomendaciones que se ofrecen en [Solución de problemas y prácticas recomendadas de copia de seguridad de SQL Server en URL y restauración a partir de copias de seguridad almacenadas en Azure Storage](/sql/relational-databases/backup-restore/sql-server-backup-to-url-best-practices-and-troubleshooting). También puede automatizar estas copias de seguridad mediante la [Copia de seguridad automatizada para SQL Server en Azure Virtual Machines](../../../azure-sql/virtual-machines/windows/automated-backup-sql-2014.md).

Antes de SQL Server 2012, puede usar la [Herramienta de copia de seguridad de SQL Server a Azure](https://www.microsoft.com/download/details.aspx?id=40740). Esta herramienta puede ayudar a aumentar el rendimiento de la copia de seguridad con varios destinos de franjas de copia de seguridad.

### <a name="sql-server-data-files-in-azure"></a>Archivos de datos de SQL Server en Azure

esta nueva característica ([Archivos de datos de SQL Server en Azure](/sql/relational-databases/databases/sql-server-data-files-in-microsoft-azure)) está disponible a partir de SQL Server 2014. La ejecución de SQL Server con archivos de datos en Azure muestra características de rendimiento comparables con el uso de discos de datos de Azure.

### <a name="failover-cluster-instance-and-storage-spaces"></a>Instancia de clúster de conmutación por error y Espacios de almacenamiento

Si usa Espacios de almacenamiento, al agregar nodos al clúster en la página **Confirmación**, desactive la casilla **Agregar todo el almacenamiento apto al clúster**. 

![Desactivación del almacenamiento apto](./media/performance-guidelines-best-practices/uncheck-eligible-cluster-storage.png)

Si utiliza espacios de almacenamiento y no desactiva la casilla **Add all eligible storage to the cluster** (Agregar todo el almacenamiento apto al clúster), Windows separa los discos virtuales durante el proceso de agrupación en clústeres. Como resultado, no aparecen en el Administrador de discos ni el Explorador hasta que se quiten los espacios de almacenamiento del clúster y se vuelvan a asociar mediante PowerShell. Espacios de almacenamiento agrupa varios discos en grupos de almacenamiento. Para obtener más información, consulte el artículo sobre [espacios de almacenamiento](/windows-server/storage/storage-spaces/overview).

## <a name="multiple-instances"></a>Varias instancias 

Tenga en cuenta los procedimientos recomendados siguientes al implementar varias instancias de SQL Server en una sola máquina virtual: 

- Establezca la memoria máxima de servidor para cada instancia de SQL Server y asegúrese de que quede memoria para el sistema operativo. Asegúrese de actualizar las restricciones de memoria para las instancias de SQL Server si cambia la cantidad de memoria que se asigna a la máquina virtual. 
- Tenga LUN independientes para los datos, los registros y TempDB, ya que todos tienen distintos patrones de carga de trabajo y debe evitar que se afecten entre sí. 
- Compruebe minuciosamente el entorno con cargas de trabajo intensas para asegurarse de que puede controlar la capacidad de carga de trabajo máxima con sus contratos de nivel de servicio de aplicación. 

Los signos de sistemas sobrecargados pueden incluir, pero no limitarse a los siguientes: agotamiento de subprocesos de trabajo, tiempos de respuesta lentos o memoria del sistema del distribuidor detenida. 



## <a name="collect-performance-baseline"></a>Recopilación de base de referencia de rendimiento

Para un enfoque más preceptivo, recopile contadores de rendimiento mediante PerfMon/LogMan y capture estadísticas de espera de SQL Server para comprender mejor las presiones generales y los posibles cuellos de botella del entorno de origen. 

Comience por recopilar la CPU, la memoria, el [IOPS](../../../virtual-machines/premium-storage-performance.md#iops), el [rendimiento](../../../virtual-machines/premium-storage-performance.md#throughput) y la [latencia](../../../virtual-machines/premium-storage-performance.md#latency) de la carga de trabajo de origen en horas punta siguiendo la [lista de comprobación de rendimiento de la aplicación](../../../virtual-machines/premium-storage-performance.md#application-performance-requirements-checklist). 

Recopile datos durante las horas punta, como las cargas de trabajo durante un día laboral habitual, pero también otros procesos de carga alta, como el procesamiento de fin de jornada y las cargas de trabajo de ETL de fines de semana. Considere la posibilidad de escalar verticalmente los recursos para cargas de trabajo inusualmente intensas, como el procesamiento de fin de trimestre, y después modifique la escala una vez que se complete la carga de trabajo. 

Use el análisis de rendimiento para seleccionar el [tamaño de VM](../../../virtual-machines/sizes-memory.md) que se puede escalar a los requisitos de rendimiento de la carga de trabajo.


### <a name="iops-and-throughput"></a>IOPS y rendimiento

El rendimiento de SQL Server depende en gran medida del subsistema de E/S. A menos que la base de datos se ajuste a la memoria física, SQL Server pone las páginas de la base de datos dentro y fuera del grupo de búferes constantemente. Los archivos de datos de SQL Server se deben tratar de forma diferente. El acceso a los archivos de registro es secuencial, excepto cuando es necesario revertir una transacción en la que se tiene acceso de forma aleatoria a los archivos de datos, incluido TempDB. Si tiene un subsistema de E/S lento, es posible que los usuarios experimenten problemas de rendimiento, como tiempos de respuesta lentos y tareas que no se completan debido a los tiempos de espera. 

Las máquinas virtuales de Azure Marketplace tienen archivos de registro en un disco físico que es independiente de los archivos de datos de manera predeterminada. El número y el tamaño de los archivos de datos de TempDB cumplen los procedimientos recomendados y se destinan a la unidad D:/. 

Los siguientes contadores de PerfMon pueden ayudar a validar el rendimiento de E/S requerido por SQL Server: 
* **\LogicalDisk\Disk Reads/Sec** (IOPS de lectura y escritura)
* **\LogicalDisk\Disk Writes/Sec** (IOPS de lectura y escritura) 
* **\LogicalDisk\Disk Bytes/Sec** (requisitos de rendimiento para los archivos de datos, registro y TempDB)

Mediante los requisitos de rendimiento e IOPS en los niveles de máxima actividad, evalúe los tamaños de VM que coincidan con la capacidad de sus mediciones. 

Si la carga de trabajo requiere 20 000 IOPS de lectura y 10 000 IOPS de escritura, puede elegir E16s_v3 (con hasta 32 000 IOPS en caché y 25 600 no almacenadas en la caché), o bien M16_s (con un máximo de 20 000 IOPS en caché y 10 000 no almacenadas en la caché) con 2 discos P30 seccionados con espacios de almacenamiento. 

Asegúrese de que conoce los requisitos tanto de rendimiento como de IOPS de la carga de trabajo, ya que las máquinas virtuales tienen diferentes límites de escala para ambos.

### <a name="memory"></a>Memoria

Realice un seguimiento de la memoria externa usada por el sistema operativo, así como la memoria que usa internamente SQL Server. La identificación de la presión de cualquier componente ayudará a cambiar el tamaño de las máquinas virtuales e identificar las oportunidades de ajuste. 

Los siguientes contadores de PerfMon pueden ayudar a validar el estado de la memoria de una máquina virtual de SQL Server: 
* [\Memoria\MB disponibles](/azure/monitoring/infrastructure-health/vmhealth-windows/winserver-memory-availmbytes)
* [\SQLServer:Memory Manager\Target Server Memory (KB)](/sql/relational-databases/performance-monitor/sql-server-buffer-manager-object)
* [\SQLServer:Memory Manager\Total Server Memory (KB)](/sql/relational-databases/performance-monitor/sql-server-buffer-manager-object)
* [\SQLServer:Buffer Manager\Lazy writes/sec](/sql/relational-databases/performance-monitor/sql-server-buffer-manager-object)
* [\SQLServer:Buffer Manager\Page life expectancy](/sql/relational-databases/performance-monitor/sql-server-buffer-manager-object)

### <a name="compute--processing"></a>Proceso y procesamiento

El proceso en Azure se administra de forma diferente que en el entorno local. Los servidores locales se compilan en los últimos años sin necesidad de una actualización debido a la sobrecarga de administración y al costo de adquisición de hardware nuevo. La virtualización mitiga algunos de estos problemas, pero las aplicaciones están optimizadas para sacar el máximo partido del hardware subyacente, lo que significa que cualquier cambio significativo en el consumo de recursos requiere el reequilibrio de todo el entorno físico. 

Esto no es un reto en Azure, donde una nueva máquina virtual en una serie diferente de hardware, e incluso en otra región, es fácil de conseguir. 

En Azure, debe aprovechar la mayor parte posible de los recursos de máquinas virtuales, por lo que Azure Virtual Machines debe configurarse para mantener el promedio de CPU lo más alto posible sin afectar a la carga de trabajo. 

Los siguientes contadores de PerfMon pueden ayudar a validar el estado del proceso de una máquina virtual de SQL Server:
* **\Processor Information(_Total)\% de tiempo de procesador**
* **\Process(sqlservr)\% de tiempo de procesador**

> [!NOTE] 
> Idealmente, intente usar el 80 % del proceso, con picos por encima del 90 %, pero que no alcancen el 100 % durante un período de tiempo prolongado. Básicamente, solo debería aprovisionar el proceso que necesita la aplicación y, a continuación, planear el escalado o reducción vertical según requiera la empresa. 

## <a name="next-steps"></a>Pasos siguientes

Para ver los procedimientos recomendados de seguridad, consulte [Consideraciones de seguridad para SQL Server en Azure Virtual Machines](security-considerations-best-practices.md).

Revise otros artículos sobre la máquina virtual de SQL Server en [Introducción a SQL Server en Azure Virtual Machines](sql-server-on-azure-vm-iaas-what-is-overview.md). Si tiene alguna pregunta sobre las máquinas virtuales de SQL Server, consulte las [Preguntas más frecuentes](frequently-asked-questions-faq.md).
