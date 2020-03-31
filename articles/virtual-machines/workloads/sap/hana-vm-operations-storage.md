---
title: Configuraciones de almacenamiento de máquinas virtuales de Azure en SAP HANA | Microsoft Docs
description: Recomendaciones de almacenamiento para máquinas virtuales con SAP HANA implementado en ellas.
services: virtual-machines-linux,virtual-machines-windows
documentationcenter: ''
author: msjuergent
manager: bburns
editor: ''
tags: azure-resource-manager
keywords: ''
ms.service: virtual-machines-linux
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 03/10/2020
ms.author: juergent
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 4b469c098db4f8d90147b491bcb54bd55d326b03
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79080315"
---
# <a name="sap-hana-azure-virtual-machine-storage-configurations"></a>Configuraciones de almacenamiento de máquinas virtuales de Azure en SAP HANA

Azure proporciona distintos tipos de almacenamiento adecuados para máquinas virtuales de Azure que ejecutan SAP HANA. Los **tipos de almacenamiento de Azure con certificación de SAP HANA** que pueden considerarse para las implementaciones de SAP HANA, como: 

- SSD Premium de Azure  
- [Disco Ultra](https://docs.microsoft.com/azure/virtual-machines/linux/disks-enable-ultra-ssd)
- [Azure NetApp Files](https://azure.microsoft.com/services/netapp/) 

Para información acerca de estos tipos de disco, consulte el artículo de [Selección de un tipo de disco](https://docs.microsoft.com/azure/virtual-machines/linux/disks-types).

Azure ofrece dos métodos de implementación de discos duros virtuales en Azure Standard Storage y Azure Premium Storage. Si el escenario global lo permite, aproveche las implementaciones de [Azure Managed Disks](https://azure.microsoft.com/services/managed-disks/). 

Para obtener una lista de tipos de almacenamiento y sus Acuerdos de Nivel de Servicio sobre IOPS y rendimiento del almacenamiento, revise la [documentación de Azure para Managed Disks](https://azure.microsoft.com/pricing/details/managed-disks/).

> [!IMPORTANT]
> Independientemente del tipo de almacenamiento de Azure elegido, el sistema de archivos que se usa en ese almacenamiento debe ser compatible con SAP para el sistema operativo y el DBMS específicos. La [nota de compatibilidad 405827 de SAP](https://launchpad.support.sap.com/#/notes/405827) enumera los sistemas de archivos admitidos para los diferentes sistemas operativos y bases de datos, incluido SAP HANA. Esto se aplica a todos los volúmenes SAP HANA con acceso de lectura y escritura para cualquier tarea. Especialmente si usa NFS en Azure para SAP HANA, se aplican restricciones adicionales de versiones de NFS como se indica más adelante en este artículo. 


Las condiciones de certificación mínimas de SAP HANA para los diferentes tipos de almacenamiento son: 

- SSD Premium de Azure: /hana/log se debe almacenar en caché con el [Acelerador de escritura](https://docs.microsoft.com/azure/virtual-machines/linux/how-to-enable-write-accelerator) de Azure. El volumen /hana/data puede colocarse en el SSD Premium sin el Acelerador de escritura de Azure o bien en un disco Ultra.
- Disco Ultra de Azure al menos para el volumen /hana/log. El volumen /hana/data se puede colocar en el SSD Premium sin el Acelerador de escritura de Azure o en un disco Ultra para obtener un tiempo de reinicio más rápido.
- Volúmenes de **NFS v4.1** sobre Azure NetApp Files para /hana/log **y** /hana/data. El volumen de/hana/sharedpuede usar el protocolo NFS v3 o NFS v 4.1. El protocolo NFS v4.1 es obligatorio para los volúmenes /hana/data y /hana/log.

Algunos de los tipos de almacenamiento se pueden combinar. Por ejemplo, es posible colocar /hana/data en Premium Storage y /hana/log se puede colocar en un almacenamiento en disco Ultra para obtener la baja latencia necesaria. Sin embargo, si usa un volumen NFS v4.1 basado en ANF para /hana/data, es necesario usar otro volumen NFS v4.1 basado en ANF para /hana/log. **No se admite** el uso de NFS sobre ANF para uno de los volúmenes (como /hana/data) y Azure Premium Storage o almacenamiento en disco Ultra para el otro volumen (como /hana/log).

En el mundo local, casi nunca ha tenido que preocuparse sobre los subsistemas de E/S y sus funcionalidades. El motivo era que el proveedor de las aplicaciones tenía que asegurarse de que se cumplieran los requisitos de almacenamiento mínimo para SAP HANA. Mientras crea la infraestructura de Azure, debe tener en cuenta algunos de estos requisitos que emite SAP. Algunas de las características de rendimiento mínimas que se requieren por parte de SAP comportan la necesidad de:

- Habilitar la lectura/escritura en **/hana/log** de 250 MB/s con tamaños de E/S de 1 MB
- Habilitar la actividad de lectura de al menos 400 MB/s para **/hana/data** con tamaños de E/S de 16 MB y 64 MB
- Habilitar la actividad de escritura de al menos 250 MB/s para **/hana/data** con tamaños de E/S de 16 MB y 64 MB

Debido a que la baja latencia de almacenamiento es fundamental para los sistemas DBMS, incluso estos DBMS, como SAP HANA, mantienen los datos en memoria. La ruta crítica en el almacenamiento normalmente está relacionada con las escrituras del registro de transacciones de los sistemas DBMS. Sin embargo, también las operaciones como la escritura de puntos de retorno o la carga de datos en memoria después de la recuperación tras un bloqueo pueden ser críticas. Por ello es **obligatorio** aprovechar Azure Premium Disks para los volúmenes **/hana/data** y **/hana/log**. Para lograr el rendimiento mínimo de **/hana/log** y **/hana/data** como desea SAP, tiene que compilar un RAID 0 utilizando MDADM o LVM en varios discos de Azure Premium Storage, y usar los volúmenes RAID como volúmenes de **/hana/data** y **/hana/log**. 

**Recomendación: como tamaños de franja para RAID 0 la recomendación es utilizar:**

- 256 KB para **/hana/data**
- 32 KB para **/hana/log**

> [!IMPORTANT]
> El tamaño de franja de /hana/data ha cambiado con respecto a las recomendaciones anteriores que precisan de 64 KB o 128 KB a 256 KB en función de las experiencias del cliente con versiones más recientes de Linux. El tamaño de 256 KB proporciona un rendimiento ligeramente superior.

> [!NOTE]
> No es necesario configurar ningún nivel de redundancia con volúmenes RAID, ya que Azure Premium Storage y Azure Standard Storage mantienen tres imágenes de un disco duro virtual. El uso de un volumen RAID tiene como finalidad configurar volúmenes que proporcionen un rendimiento de E/S suficiente.

La acumulación de un número de discos duros virtuales de Azure bajo un RAID es acumulativo por parte de un IOPS y del rendimiento de almacenamiento. Por lo tanto, si coloca una RAID 0 en P30 x 3 discos de Azure Premium Storage, debe proporcionarle tres veces el IOPS y tres veces el rendimiento de almacenamiento de un solo disco P30 de Azure Premium Storage.

Tenga también en cuenta el rendimiento global de E/S de la máquina virtual al elegir una máquina virtual o determinar su tamaño. El rendimiento general del almacenamiento de las máquinas virtuales está documentado en el artículo [Tamaños de máquina virtual optimizada para memoria](https://docs.microsoft.com/azure/virtual-machines/linux/sizes-memory).

## <a name="linux-io-scheduler-mode"></a>Modo de programador de E/S de Linux
Linux tiene varios modos diferentes de programación de E/S. Una recomendación común de los proveedores de Linux y SAP consiste en reconfigurar el modo de programador de E/S para los volúmenes de disco del modo **cfq** en el modo **noop** o (no multicola) **none** para el modo (multicola). Los detalles se incluyen en la [nota de SAP n.º 1984787](https://launchpad.support.sap.com/#/notes/1984787). 


## <a name="solutions-with-premium-storage-and-azure-write-accelerator-for-azure-m-series-virtual-machines"></a>Soluciones con Premium Storage y el Acelerador de escritura de Azure para máquinas virtuales de la serie M de Azure
El Acelerador de escritura de Azure es una funcionalidad que está disponible exclusivamente para las máquinas virtuales de la serie M de Azure. Como el nombre indica, el propósito de la funcionalidad es mejorar la latencia de E/S de las escrituras en Azure Premium Storage. Para SAP HANA, el Acelerador de escritura se supone que debe usarse solo en el volumen **/hana/log**. Por lo tanto, **/hana/data** y **/hana/log** son volúmenes independientes y el Acelerador de escritura de Azure admite solo el volumen **/hana/log**. 

> [!IMPORTANT]
> Cuando se utiliza Azure Premium Storage, es obligatorio usar el [Acelerador de escritura](https://docs.microsoft.com/azure/virtual-machines/linux/how-to-enable-write-accelerator) de Azure o el volumen /hana/log. El Acelerador de escritura solo está disponible para Premium Storage y para las máquinas virtuales de las series M y Mv2.

Estas recomendaciones de almacenamiento en caché suponen que las características de E/S para SAP HANA que se enumeran son como las siguientes:

- Apenas hay carga de trabajo de lectura en los archivos de datos de HANA. Las excepciones son las operaciones de E/S de tamaño grande después del reinicio de la instancia HANA o cuando se cargan datos en HANA. Otro caso de operaciones de E/S de lectura de mayor tamaño con archivos de datos pueden ser las copias de seguridad de base de datos de HANA. Como resultado, el almacenamiento en caché de lectura no tiene sentido ya que, en la mayoría de los casos, todos los volúmenes de archivos de datos tienen que leerse por completo.
- La escritura en los archivos de datos se produce en ráfagas según los puntos de retorno de HANA y la recuperación tras bloqueos de HANA. La escritura de los puntos de retorno es asincrónica y no bloquea las transacciones de usuario. La escritura de datos durante la recuperación tras bloqueo es esencial en el rendimiento a fin de lograr que el sistema responda de nuevo rápidamente. Sin embargo, la recuperación tras bloqueo debería constituir una situación excepcional.
- Apenas hay lecturas de los archivos de puesta al día de HANA. Las operaciones de E/S grandes al realizar copias de seguridad del registro de transacciones, la recuperación tras bloqueo o en la fase de reinicio de una instancia de HANA constituyen excepciones.  
- La carga principal en el caso del archivo de registro de puesta al día SAP HANA la constituyen las escrituras. Según la naturaleza de la carga de trabajo, puede tener operaciones de E/S de solo 4 KB o llegar a superar 1 MB. La latencia de las escrituras en relación con el registro de puesta al día SAP HANA es esencial para el rendimiento.
- Todas las operaciones de escritura se deben conservar en el disco de forma confiable.

**Recomendación: Como resultado de estos patrones de E/S observados por SAP HANA, debe establecer el almacenamiento en caché de los diferentes volúmenes con Azure Premium Storage como sigue:**

- **/hana/data**: sin almacenamiento en caché
- **/hana/log**: sin almacenamiento en caché. Excepción para las máquinas virtuales de las series M y Mv2, en las que el Acelerador de escritura debe estar habilitado sin almacenamiento en caché de lectura. 
- **/hana/shared**: almacenamiento en caché de lecturas

### <a name="production-recommended-storage-solution"></a>Solución de almacenamiento recomendada para producción

> [!IMPORTANT]
> La certificación de SAP HANA para las máquinas virtuales de serie M de Azure es exclusivamente con el Acelerador de escritura de Azure para el volumen **/hana/log**. Como resultado, las implementaciones de SAP HANA en un escenario de producción en máquinas virtuales de la serie M de Azure se espera que estén configuradas con el Acelerador de escritura de Azure para el volumen **/hana/log**.  

> [!NOTE]
> Para escenarios de producción, compruebe si un determinado tipo de máquina virtual es compatible con SAP HANA de SAP en la [documentación de SAP para IaaS](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html).

A menudo, las recomendaciones superan los requisitos mínimos de SAP como se indicó anteriormente en este artículo. Las recomendaciones enumeradas son un compromiso entre las recomendaciones de tamaño de SAP y el rendimiento de almacenamiento máximo que proporcionan los diferentes tipos de máquinas virtuales.

**Recomendación: las configuraciones recomendadas para escenarios de producción tienen el siguiente aspecto:**

| SKU de la máquina virtual | RAM | Máx. E/S de VM<br /> Throughput | /hana/data | /hana/log | /hana/shared | /root volume | /usr/sap | hana/backup |
| --- | --- | --- | --- | --- | --- | --- | --- | -- |
| M32ts | 192 GiB | 500 MB/s | 3 x P20 | 2 x P20 | 1 x P20 | 1 x P6 | 1 x P6 |1 x P20 |
| M32ls | 256 GiB | 500 MB/s | 3 x P20 | 2 x P20 | 1 x P20 | 1 x P6 | 1 x P6 |1 x P20 |
| M64ls | 512 GB | 1000 MB/s | 3 x P20 | 2 x P20 | 1 x P20 | 1 x P6 | 1 x P6 |1 x P30 |
| M64s | 1000 GiB | 1000 MB/s | 4 x P20 | 2 x P20 | 1 x P30 | 1 x P6 | 1 x P6 |2 x P30 |
| M64ms | 1750 GiB | 1000 MB/s | 3 x P30 | 2 x P20 | 1 x P30 | 1 x P6 | 1 x P6 | 3 x P30 |
| M128s | 2000 GiB | 2000 MB/s |3 x P30 | 2 x P20 | 1 x P30 | 1 x P10 | 1 x P6 | 2 x P40 |
| M128ms | 3800 GiB | 2000 MB/s | 5 x P30 | 2 x P20 | 1 x P30 | 1 x P10 | 1 x P6 | 4 x P40 |
| M208s_v2 | 2850 GiB | 1000 MB/s | 4 x P30 | 2 x P20 | 1 x P30 | 1 x P10 | 1 x P6 | 3 x P40 |
| M208ms_v2 | 5700 GiB | 1000 MB/s | 4 x P40 | 2 x P20 | 1 x P30 | 1 x P10 | 1 x P6 | 3 x P50 |
| M416s_v2 | 5700 GiB | 2000 MB/s | 4 x P40 | 2 x P20 | 1 x P30 | 1 x P10 | 1 x P6 | 3 x P50 |
| M416ms_v2 | 11400 GiB | 2000 MB/s | 8 x P40 | 2 x P20 | 1 x P30 | 1 x P10 | 1 x P6 | 4 x P50 |

Compruebe si el rendimiento del almacenamiento para los diferentes volúmenes sugeridos se adapta a la carga de trabajo que se va a ejecutar. Si la carga de trabajo requiere volúmenes mayores para **/hana/data** y **/hana/log**, tiene que aumentar el número de discos duros virtuales de Azure Premium Storage. Ajustar el tamaño de un volumen con más discos duros virtuales de los que se enumeran, aumenta el IOPS y el rendimiento de E/S dentro de los límites del tipo de máquina virtual de Azure.

El Acelerador de escritura de Azure solo funciona en conjunto con [Azure Managed Disks](https://azure.microsoft.com/services/managed-disks/). Por lo que al menos los discos de Azure Premium Storage que forman el volumen **/hana/log** tienen que implementarse como discos administrados.

Hay límites en los discos duros virtuales de Azure Premium Storage por máquina virtual que el Acelerador de escritura de Azure puede admitir. Los límites actuales son:

- 16 VHD para una máquina virtual M128xx y M416xx
- 8 VHD para una máquina virtual M64xx y M208xx
- 4 discos duros virtuales para una máquina virtual M32xx

Puede encontrar instrucciones más detalladas sobre cómo habilitar el Acelerador de escritura de Azure en el artículo [Write Accelerator](https://docs.microsoft.com/azure/virtual-machines/linux/how-to-enable-write-accelerator) (Acelerador de escritura).

En la misma documentación puede encontrar información más detallada y las restricciones para el Acelerador de escritura de Azure.

**Recomendación: debe usar el Acelerador de escritura para los discos que forman el volumen/hana/log**


### <a name="cost-conscious-azure-storage-configuration"></a>Configuración de Azure Storage con control de costo
En la tabla siguiente se muestra una configuración de tipos de máquina virtual que los clientes también usan para hospedar SAP HANA en máquinas virtuales de Azure. Puede haber algunos tipos de máquina virtual que no cumplan todos los criterios mínimos de almacenamiento para SAP HANA o no se admitan de forma oficial con SAP HANA en SAP. Pero hasta ahora parece que esas máquinas virtuales presentan un rendimiento adecuado en escenarios que no son de producción. **/hana/data** y **/hana/log** se combinan en un volumen. Como resultado, el uso del Acelerador de escritura de Azure puede ser una limitación en términos de IOPS.

> [!NOTE]
> Para escenarios compatibles con SAP, compruebe si un determinado tipo de máquina virtual es compatible con SAP HANA de SAP en la [documentación de SAP para IaaS](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html).

> [!NOTE]
> Un cambio de las recomendaciones anteriores para una solución que considere los costos consiste en cambiar de los [discos HDD estándar de Azure](https://docs.microsoft.com/azure/virtual-machines/windows/disks-types#standard-hdd) a los[discos SSD estándar de Azure](https://docs.microsoft.com/azure/virtual-machines/windows/disks-types#standard-ssd) de mayor rendimiento y más confiables.

A menudo, las recomendaciones superan los requisitos mínimos de SAP como se indicó anteriormente en este artículo. Las recomendaciones enumeradas son un compromiso entre las recomendaciones de tamaño de SAP y el rendimiento de almacenamiento máximo que proporcionan los diferentes tipos de máquinas virtuales.

| SKU de la máquina virtual | RAM | Máx. E/S de VM<br /> Throughput | /hana/data y /hana/log<br /> seccionado con LVM o MDAADM | /hana/shared | /root volume | /usr/sap | hana/backup |
| --- | --- | --- | --- | --- | --- | --- | -- |
| DS14v2 | 112 GiB | 768 MB/s | 3 x P20 | 1 x E20 | 1 x E6 | 1 x E6 | 1 x E15 |
| E16v3 | 128 GB | 384 MB/s | 3 x P20 | 1 x E20 | 1 x E6 | 1 x E6 | 1 x E15 |
| E32v3 | 256 GiB | 768 MB/s | 3 x P20 | 1 x E20 | 1 x E6 | 1 x E6 | 1 x E20 |
| E64v3 | 432 GiB | 1200 MB/s | 3 x P20 | 1 x E20 | 1 x E6 | 1 x E6 | 1 x E30 |
| GS5 | 448 GiB | 2000 MB/s | 3 x P20 | 1 x E20 | 1 x E6 | 1 x E6 | 1 x E30 |
| M32ts | 192 GiB | 500 MB/s | 3 x P20 | 1 x E20 | 1 x E6 | 1 x E6 | 1 x E20 |
| M32ls | 256 GiB | 500 MB/s | 3 x P20 | 1 x E20 | 1 x E6 | 1 x E6 | 1 x E20 |
| M64ls | 512 GB | 1000 MB/s | 3 x P20 | 1 x E20 | 1 x E6 | 1 x E6 |1 x E30 |
| M64s | 1000 GiB | 1000 MB/s | 2 x P30 | 1 x E30 | 1 x E6 | 1 x E6 |2 x E30 |
| M64ms | 1750 GiB | 1000 MB/s | 3 x P30 | 1 x E30 | 1 x E6 | 1 x E6 | 3 x E30 |
| M128s | 2000 GiB | 2000 MB/s |3 x P30 | 1 x E30 | 1 x E10 | 1 x E6 | 2 x E40 |
| M128ms | 3800 GiB | 2000 MB/s | 5 x P30 | 1 x E30 | 1 x E10 | 1 x E6 | 2 x E50 |
| M208s_v2 | 2850 GiB | 1000 MB/s | 4 x P30 | 1 x E30 | 1 x E10 | 1 x E6 |  3 x E40 |
| M208ms_v2 | 5700 GiB | 1000 MB/s | 4 x P40 | 1 x E30 | 1 x E10 | 1 x E6 |  4 x E40 |
| M416s_v2 | 5700 GiB | 2000 MB/s | 4 x P40 | 1 x E30 | 1 x E10 | 1 x E6 |  4 x E40 |
| M416ms_v2 | 11400 GiB | 2000 MB/s | 8 x P40 | 1 x E30 | 1 x E10 | 1 x E6 |  4 x E50 |


Discos recomendados para los tipos de máquinas virtuales más pequeños con un exceso de tamaño de los volúmenes de 3 x P20 con respecto a las recomendaciones de espacio según las [notas del producto sobre el almacenamiento de TDI de SAP](https://www.sap.com/documents/2015/03/74cdb554-5a7c-0010-82c7-eda71af511fa.html). No obstante, la elección que se muestra en la tabla se realizó para proporcionar un rendimiento del disco suficiente para SAP HANA. Si necesitar realizar cambios en el volumen **/hana/backup**, cuyo tamaño se estableció para mantener copias de seguridad que representan el doble del volumen de memoria, puede realizar los ajustes que requiera.   
Compruebe si el rendimiento del almacenamiento para los diferentes volúmenes sugeridos se adapta a la carga de trabajo que se va a ejecutar. Si la carga de trabajo requiere volúmenes mayores para **/hana/data** y **/hana/log**, tiene que aumentar el número de discos duros virtuales de Azure Premium Storage. Ajustar el tamaño de un volumen con más discos duros virtuales de los que se enumeran, aumenta el IOPS y el rendimiento de E/S dentro de los límites del tipo de máquina virtual de Azure. 

> [!NOTE]
> Las configuraciones anteriores NO se beneficiarían del [Acuerdo de nivel de servicio de máquinas virtuales de Azure para una máquina virtual única](https://azure.microsoft.com/support/legal/sla/virtual-machines/v1_6/) debido a que utilizan una combinación de Azure Premium Storage y Azure Standard Storage. Sin embargo, la selección se ha elegido para optimizar los costos. Debe elegir el almacenamiento Premium para todos los discos por encima del que aparece como Azure Standard Storage (Sxx) para que la configuración de máquina virtual sea compatible con el Acuerdo de Nivel de Servicio de máquina virtual de Azure único.


> [!NOTE]
> Las recomendaciones de configuración de discos que se han indicado se dirigen a los requisitos mínimos que SAP expresa a sus proveedores de infraestructura. En las implementaciones reales de clientes y en los escenarios de carga de trabajo, se encontraron situaciones en las que estas recomendaciones aún no proporcionaban suficientes funcionalidades. Estas podrían ser situaciones en las que un cliente necesitara una recarga más rápida de los datos después de un reinicio de HANA o en las que las configuraciones de copia de seguridad requirieran un mayor ancho de banda para el almacenamiento. Otros casos incluían **/hana/log**, en los que 5000 IOPS no fueron suficientes para la carga de trabajo específica. Por lo tanto, tome estas recomendaciones como punto de partida y adáptelas en función de los requisitos de la carga de trabajo.
>  

## <a name="azure-ultra-disk-storage-configuration-for-sap-hana"></a>Configuración de almacenamiento en el disco Ultra de Azure para SAP HANA
Microsoft está en proceso de lanzar un nuevo tipo de almacenamiento de Azure denominado [disco Ultra de Azure](https://docs.microsoft.com/azure/virtual-machines/windows/disks-types#ultra-disk). Una diferencia considerable entre el almacenamiento de Azure que se ofrecía hasta ahora y el disco Ultra es que las funcionalidades del disco ya no están asociadas a su tamaño. Como un cliente, para el disco Ultra puede definir estas funcionalidades:

- El tamaño de disco, de 4 GiB a 65 536 GiB
- Intervalo de 100 operaciones de E/S por segundo a 160 000 operaciones de E/S por segundo (el máximo depende también de los tipos de máquina virtual)
- Rendimiento del almacenamiento de 300 MB/s a 2000 MB por segundo

El disco Ultra ofrece la posibilidad de definir un único disco que responda a su rango de necesidades de tamaño, IOPS y rendimiento de disco. en lugar de usar administradores de volúmenes lógicos como LVM o MDADM además de Azure Premium Storage para construir los volúmenes que cumplan los requisitos de rendimiento de almacenamiento y de operaciones de E/S por segundo. Puede ejecutar una configuración combinada entre el disco Ultra y Premium Storage. Como resultado, puede limitar el uso el disco Ultra a los volúmenes /hana/data y /hana/log críticos para el rendimiento y cubrir los demás volúmenes con Azure Premium Storage.

Otra ventaja del disco Ultra es una mejor latencia de lectura en comparación con Premium Storage. Una latencia de lectura más rápida puede tener ventajas si desea reducir los tiempos de inicio de HANA y la carga posterior de los datos en la memoria. También se pueden apreciar las ventajas del almacenamiento en disco Ultra cuando HANA está escribiendo puntos de retorno. Como los discos de Premium Storage para /hana/data no se almacenan normalmente en la caché del Acelerador de escritura, la latencia de escritura de /hana/data en Premium Storage es mayor en comparación con la del disco Ultra. Lógicamente, la escritura de puntos de retorno con discos Ultra se lleva a cabo mejor en este tipo de discos.

> [!IMPORTANT]
> El disco Ultra todavía no está presente en todas las regiones de Azure y aún no es compatible con todos los tipos de máquinas virtuales que se indican a continuación. Para obtener información detallada sobre dónde está disponible el disco Ultra y qué familias de máquinas virtuales se admiten, consulte el artículo [¿Qué tipos de disco están disponibles en Azure?](https://docs.microsoft.com/azure/virtual-machines/windows/disks-types#ultra-disk).

### <a name="production-recommended-storage-solution-with-pure-ultra-disk-configuration"></a>Solución de almacenamiento recomendada para producción con configuración de disco Ultra pura
En esta configuración, puede conservar los volúmenes /hana/data y /hana/log por separado. Los valores sugeridos se derivan de los KPI que SAP tiene para certificar los tipos de máquina virtual de SAP HANA y las configuraciones de almacenamiento como se recomienda en las [notas del producto SAP TDI Storage](https://www.sap.com/documents/2015/03/74cdb554-5a7c-0010-82c7-eda71af511fa.html).

A menudo, las recomendaciones superan los requisitos mínimos de SAP como se indicó anteriormente en este artículo. Las recomendaciones enumeradas son un compromiso entre las recomendaciones de tamaño de SAP y el rendimiento de almacenamiento máximo que proporcionan los diferentes tipos de máquinas virtuales.

| SKU de la máquina virtual | RAM | Máx. E/S de VM<br /> Throughput | Volumen /hana/data | Rendimiento de E/S de /hana/data | Operaciones de E/S por segundo en /hana/data | Volumen /hana/log | Rendimiento de E/S de /hana/log | Operaciones de E/S por segundo en /hana/log |
| --- | --- | --- | --- | --- | --- | --- | --- | -- |
| E64s_v3 | 432 GiB | 1200 MB/s | 600 GB | 700 Mbps | 7500 | 512 GB | 500 MBps  | 2\.000 |
| M32ts | 192 GiB | 500 MB/s | 250 GB | 400 MBps | 7500 | 256 GB | 250 MBps  | 2\.000 |
| M32ls | 256 GiB | 500 MB/s | 300 GB | 400 MBps | 7500 | 256 GB | 250 MBps  | 2\.000 |
| M64ls | 512 GB | 1000 MB/s | 600 GB | 600 MBps | 7500 | 512 GB | 400 MBps  | 2500 |
| M64s | 1000 GiB | 1000 MB/s |  1200 GB | 600 MBps | 7500 | 512 GB | 400 MBps  | 2500 |
| M64ms | 1750 GiB | 1000 MB/s | 2100 GB | 600 MBps | 7500 | 512 GB | 400 MBps  | 2500 |
| M128s | 2000 GiB | 2000 MB/s |2400 GB | 1200 MBps |9000 | 512 GB | 800 MBps  | 3000 | 
| M128ms | 3800 GiB | 2000 MB/s | 4800 GB | 1200 MBps |9000 | 512 GB | 800 MBps  | 3000 | 
| M208s_v2 | 2850 GiB | 1000 MB/s | 3500 GB | 1000 MBps | 9000 | 512 GB | 400 MBps  | 2500 | 
| M208ms_v2 | 5700 GiB | 1000 MB/s | 7200 GB | 1000 MBps | 9000 | 512 GB | 400 MBps  | 2500 | 
| M416s_v2 | 5700 GiB | 2000 MB/s | 7200 GB | 1500 MBps | 9000 | 512 GB | 800 MBps  | 3000 | 
| M416ms_v2 | 11 400 GiB | 2000 MB/s | 14 400 GB | 1500 MBps | 9000 | 512 GB | 800 MBps  | 3000 |   

Los valores de la lista están diseñados como punto de partida y deben valorarse según las demandas reales. La ventaja del disco Ultra de Azure es que los valores de IOPS y de rendimiento se pueden adaptar sin necesidad de apagar la máquina virtual o detener la carga de trabajo que se aplica al sistema.   

> [!NOTE]
> Por ahora las instantáneas de almacenamiento con el disco Ultra no están disponibles. Esto bloquea el uso de las instantáneas de máquina virtual con los servicios de Azure Backup

### <a name="cost-conscious-storage-solution-with-pure-ultra-disk-configuration"></a>Solución de almacenamiento asequible con configuración de disco Ultra pura
En esta configuración, puede conservar los volúmenes /hana/data y /hana/log en el mismo disco. Los valores sugeridos se derivan de los KPI que SAP tiene para certificar los tipos de máquina virtual de SAP HANA y las configuraciones de almacenamiento como se recomienda en las [notas del producto SAP TDI Storage](https://www.sap.com/documents/2015/03/74cdb554-5a7c-0010-82c7-eda71af511fa.html). 

A menudo, las recomendaciones superan los requisitos mínimos de SAP como se indicó anteriormente en este artículo. Las recomendaciones enumeradas son un compromiso entre las recomendaciones de tamaño de SAP y el rendimiento de almacenamiento máximo que proporcionan los diferentes tipos de máquinas virtuales.

| SKU de la máquina virtual | RAM | Máx. E/S de VM<br /> Throughput | Volumen para /hana/data y /log | Rendimiento de E/S de /hana/data y /log | IOPS de /hana/data y /hana/log |
| --- | --- | --- | --- | --- | --- |
| E64s_v3 | 432 GiB | 1200 MB/s | 1200 GB | 1200 MBps | 9500 | 
| M32ts | 192 GiB | 500 MB/s | 512 GB | 400 MBps | 9500 | 
| M32ls | 256 GiB | 500 MB/s | 600 GB | 400 MBps | 9500 | 
| M64ls | 512 GB | 1000 MB/s | 1100 GB | 900 MBps | 10 000 | 
| M64s | 1000 GiB | 1000 MB/s |  1700 GB | 900 MBps | 10 000 | 
| M64ms | 1750 GiB | 1000 MB/s | 2600 GB | 900 MBps | 10 000 | 
| M128s | 2000 GiB | 2000 MB/s |2900 GB | 1800 MBps |12,000 | 
| M128ms | 3800 GiB | 2000 MB/s | 5300 GB | 1800 MBps |12,000 |  
| M208s_v2 | 2850 GiB | 1000 MB/s | 4000 GB | 900 MBps | 10 000 |  
| M208ms_v2 | 5700 GiB | 1000 MB/s | 7700 GB | 900 MBps | 10 000 | 
| M416s_v2 | 5700 GiB | 2000 MB/s | 7700 GB | 1800 MBps | 12,000 |  
| M416ms_v2 | 11 400 GiB | 2000 MB/s | 15 000 GB | 1800 MBps | 12,000 |    

Los valores de la lista están diseñados como punto de partida y deben valorarse según las demandas reales. La ventaja del disco Ultra de Azure es que los valores de IOPS y de rendimiento se pueden adaptar sin necesidad de apagar la máquina virtual o detener la carga de trabajo que se aplica al sistema.  

## <a name="nfs-v41-volumes-on-azure-netapp-files"></a>Volúmenes de NFS v4.1 en Azure NetApp Files
Azure NetApp Files proporciona recursos compartidos de NFS nativos que se pueden usar para los volúmenes /hana/shared, /hana/data y /hana/log. El uso de recursos compartidos de NFS basados en ANF para los volúmenes /hana/data y /hana/log requiere el uso del protocolo NFS v4.1. El protocolo NFS v3 no admite el uso de los volúmenes/hana/data y /hana/log al basar los recursos compartidos en ANF. 

> [!IMPORTANT]
> **No** se admite el protocolo NFS v3 implementado en Azure NetApp Files para su uso con /hana/shared, /hana/data y /hana/log. El uso de NFS 4.1 es obligatorio para los volúmenes/hana/data y /hana/log desde un punto de vista funcional. Por su parte, para el volumen /hana/shared se puede usar el protocolo NFS v3 o NFS v4.1 desde un punto de vista funcional.

### <a name="important-considerations"></a>Consideraciones importantes
A la hora de considerar Azure NetApp Files para SAP Netweaver y SAP HANA, tenga en cuenta los siguientes aspectos importantes:

- El grupo de capacidad mínimo es de 4 TiB.  
- El tamaño del volumen mínimo es de 100 GiB.
- Azure NetApp Files y todas las máquinas virtuales en las que los volúmenes de Azure NetApp Files se montarán, se deben implementar en la misma red virtual de Azure o en [redes virtuales emparejadas](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview) de la misma región.  
- La red virtual seleccionada debe tener una subred delegada en Azure NetApp Files.
- El rendimiento de un volumen de Azure NetApp es una función de la cuota del volumen y del nivel de servicio, como se documenta en [Nivel de servicio para Azure NetApp Files](https://docs.microsoft.com/azure/azure-netapp-files/azure-netapp-files-service-levels). Al ajustar el tamaño de los volúmenes de Azure NetApp de HANA, asegúrese de que el rendimiento resultante cumple los requisitos del sistema HANA.  
- Azure NetApp Files ofrece la [directiva de exportación](https://docs.microsoft.com/azure/azure-netapp-files/azure-netapp-files-configure-export-policy): puede controlar los clientes permitidos, el tipo de acceso (lectura y escritura, solo lectura, etc.). 
- La característica Azure NetApp Files no depende aún de la zona. En la actualidad, la característica Azure NetApp Files no se implementa en todas las zonas de disponibilidad de una región de Azure. Tenga en cuenta las posibles implicaciones de latencia en algunas regiones de Azure.  
- Es importante que las máquinas virtuales se implementen muy cerca del almacenamiento de Azure NetApp para conseguir una latencia baja. 
- El identificador de usuario para <b>sid</b>adm y el identificador de grupo para `sapsys` en las máquinas virtuales deben coincidir con la configuración de Azure NetApp Files. 

> [!IMPORTANT]
> Para las cargas de trabajo de SAP HANA, una baja latencia resulta fundamental. Trabaje con su representante de Microsoft para asegurarse de que las máquinas virtuales y los volúmenes de Azure NetApp Files se implementan en ubicaciones muy cercanas entre sí.  

> [!IMPORTANT]
> Si hay una discrepancia entre el identificador de usuario para <b>sid</b>adm y el identificador de grupo para `sapsys` entre la máquina virtual y la configuración de Azure NetApp, los permisos de archivos en volúmenes de Azure NetApp, montados en máquinas virtuales, aparecerán como `nobody`. Asegúrese de especificar el identificador de usuario correcto para <b>sid</b>adm y el identificador de grupo para `sapsys` al [incorporar un nuevo sistema](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbRxjSlHBUxkJBjmARn57skvdUQlJaV0ZBOE1PUkhOVk40WjZZQVJXRzI2RC4u) a Azure NetApp Files.

### <a name="sizing-for-hana-database-on-azure-netapp-files"></a>Ajuste del tamaño de la base de datos HANA en Azure NetApp Files

El rendimiento de un volumen de Azure NetApp es una función del tamaño del volumen y del nivel de servicio, como se documenta en [Nivel de servicio para Azure NetApp Files](https://docs.microsoft.com/azure/azure-netapp-files/azure-netapp-files-service-levels). 

Al diseñar la infraestructura de SAP en Azure, debe tener en cuenta los requisitos mínimos de rendimiento del almacenamiento de SAP, lo cual se traduce en estas características de rendimiento mínimas:

- Habilitación de la lectura o escritura en /hana/log de 250 MB/s con tamaños de E/S de 1 MB  
- Habilitación de la actividad de lectura de al menos 400 MB/s para /hana/data con tamaños de E/S de 16 MB y 64 MB  
- Habilitación de la actividad de escritura de al menos 250 MB/s para /hana/data con tamaños de E/S de 16 MB y 64 MB  

Los [límites de rendimiento de Azure NetApp Files](https://docs.microsoft.com/azure/azure-netapp-files/azure-netapp-files-service-levels) por 1 TiB de cuota de volumen son los siguientes:
- Capa Premium Storage: 64 MiB/s  
- Capa de almacenamiento Ultra: 128 MiB/s  

> [!IMPORTANT]
> Independientemente de la capacidad que implemente en un volumen NFS individual, se espera que el rendimiento se nivele en el rango de 1,2-1,4 GB/s de ancho de banda usado por un consumidor en una máquina virtual. Esto tiene que ver con la arquitectura subyacente de la oferta de ANF y los límites de sesiones de Linux relacionadas alrededor de NFS. Las cifras relativas al rendimiento y a la capacidad de proceso que se indican en el artículo [Banco de pruebas de rendimiento de resultados de pruebas para Azure NetApp Files](https://docs.microsoft.com/azure/azure-netapp-files/azure-netapp-files-performance-benchmarks) se obtuvieron en un volumen NFS compartido con varias máquinas virtuales cliente y como resultado con varias sesiones. Dicho escenario es diferente del escenario que medimos en SAP. Ahí medimos la capacidad de procesamiento de una sola máquina virtual en un volumen NFS hospedado en ANF.

Para cumplir los requisitos de rendimiento mínimo de SAP para los datos y el registro, y de acuerdo con las directrices para `/hana/shared`, los tamaños recomendados serían los siguientes:

| Volumen | Size<br /> Capa Premium Storage | Size<br /> Capa de almacenamiento Ultra | Protocolo NFS admitido |
| --- | --- | --- |
| /hana/log/ | 4 TiB | 2 TiB | v4.1 |
| /hana/data | 6,3 TiB | 3,2 TiB | v4.1 |
| /hana/shared | Max (512 GB, 1xRAM) por cuatro nodos de trabajo | Max (512 GB, 1xRAM) por cuatro nodos de trabajo | v3 o v4.1 |


> [!NOTE]
> Las recomendaciones de tamaño de Azure NetApp Files que se han indicado se dirigen a satisfacer los requisitos mínimos que SAP expresa a sus proveedores de infraestructura. En escenarios reales de implementaciones de clientes y de cargas de trabajo, es posible que no sea suficiente. Utilice estas recomendaciones como punto de partida y adáptelas en función de los requisitos de la carga de trabajo específica.  

Por tanto, podría considerar la posibilidad de implementar un rendimiento similar para los volúmenes de ANF, tal como se indicó anteriormente en el almacenamiento de disco Ultra. Tenga en cuenta también los tamaños enumerados para los volúmenes de las diferentes SKU de máquina virtual como ya se ha hecho en las tablas de disco Ultra.

> [!TIP]
> Puede cambiar el tamaño de los volúmenes de Azure NetApp Files de manera dinámica, sin necesidad de `unmount` los volúmenes, detener las máquinas virtuales o detener SAP HANA. Esto permite que la aplicación se adapte a las demandas de rendimiento esperadas e imprevistas.

La documentación sobre cómo implementar una configuración de escalabilidad horizontal de SAP HANA con un nodo de espera mediante los volúmenes de NFS v4.1 hospedados en ANF y publicados en [Escalabilidad horizontal de SAP HANA con nodo en espera en máquinas virtuales de Azure con Azure NetApp Files en SUSE Linux Enterprise Server](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-scale-out-standby-netapp-files-suse).


## <a name="next-steps"></a>Pasos siguientes
Para más información, consulte:

- [Guía sobre la alta disponibilidad de SAP HANA para máquinas virtuales de Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-availability-overview).
