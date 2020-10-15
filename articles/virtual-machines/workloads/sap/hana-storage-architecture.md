---
title: Arquitectura de almacenamiento de SAP HANA en Azure (instancias grandes) | Microsoft Docs
description: Arquitectura de almacenamiento para implementar SAP HANA en Azure (instancias grandes).
services: virtual-machines-linux
documentationcenter: ''
author: msjuergent
manager: bburns
editor: ''
ms.service: virtual-machines-linux
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 06/10/2020
ms.author: juergent
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: aff1c8f68e3950b49a0a1bd8e99020b77e0f2019
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "84677311"
---
# <a name="sap-hana-large-instances-storage-architecture"></a>Arquitectura de almacenamiento de SAP HANA (instancias grandes)

El diseño de almacenamiento para SAP HANA en Azure (instancias grandes) es configurado por SAP HANA en el modelo de implementación clásica según las instrucciones recomendadas de SAP. Las instrucciones se documentan en las notas del producto [Requisitos de almacenamiento de SAP HANA](https://go.sap.com/documents/2015/03/74cdb554-5a7c-0010-82c7-eda71af511fa.html).

La clase de tipo I de HANA (instancias grandes) suele tener cuatro veces el volumen de memoria como volumen de almacenamiento. Para las unidades de HANA (instancias grandes) de clase de tipo II, el almacenamiento no es cuatro veces superior. Las unidades tienen un volumen previsto para almacenar copias de seguridad del registro de transacciones de HANA. Para más información, consulte [Instalación y configuración de SAP HANA en Azure (instancias grandes)](hana-installation.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Vea la tabla siguiente en relación a la asignación de almacenamiento. En la tabla se enumera la capacidad aproximada de los diferentes volúmenes que se proporcionan con las diferentes unidades de HANA (instancias grandes).

| SKU de Instancias grandes de HANA | hana/data | hana/log | hana/shared | hana/logbackups |
| --- | --- | --- | --- | --- |
| S72 | 1 280 GB | 512 GB | 768 GB | 512 GB |
| S72m | 3 328 GB | 768 GB |1 280 GB | 768 GB |
| S96 | 1 280 GB | 512 GB | 768 GB | 512 GB |
| S192 | 4 608 GB | 1 024 GB | 1 536 GB | 1 024 GB |
| S192m | 11.520 GB | 1 536 GB | 1 792 GB | 1 536 GB |
| S192xm |  11.520 GB |  1 536 GB |  1 792 GB |  1 536 GB |
| S384 | 11.520 GB | 1 536 GB | 1 792 GB | 1 536 GB |
| S384m | 12.000 GB | 2 050 GB | 2 050 GB | 2 040 GB |
| S384xm | 16.000 GB | 2 050 GB | 2 050 GB | 2 040 GB |
| S384xxm |  20.000 GB | 3 100 GB | 2 050 GB | 3 100 GB |
| S576m | 20.000 GB | 3 100 GB | 2 050 GB | 3 100 GB |
| S576xm | 31 744 GB | 4 096 GB | 2 048 GB | 4 096 GB |
| S768m | 28.000 GB | 3 100 GB | 2 050 GB | 3 100 GB |
| S768xm | 40 960 GB | 6 144 GB | 4 096 GB | 6 144 GB |
| S960m | 36.000 GB | 4 100 GB | 2 050 GB | 4 100 GB |
| S896m | 33 792 GB | 512 GB | 1 024 GB | 512 GB |

Las SKU más recientes de instancias grandes de HANA se entregan con configuraciones de almacenamiento similares a estas:

| SKU de Instancias grandes de HANA | hana/data | hana/log | hana/shared | hana/logbackups |
| --- | --- | --- | --- | --- |
| S224 | 4224 GB | 512 GB | 1 024 GB | 512 GB |
| S224oo | 6 336 GB | 512 GB | 1 024 GB | 512 GB |
| S224 m | 8448 GB | 512 GB | 1 024 GB | 512 GB |
| S224om | 8448 GB | 512 GB | 1 024 GB | 512 GB |
| S224ooo | 10 560 GB | 512 GB | 1 024 GB | 512 GB |
| S224oom | 12 672 GB | 512 GB | 1 024 GB | 512 GB |
| S448 | 8448 GB | 512 GB | 1 024 GB | 512 GB |
| S448oo | 12 672 GB | 512 GB | 1 024 GB | 512 GB |
| S448m | 16 896 GB | 512 GB | 1 024 GB | 512 GB |
| S448om | 16 896 GB | 512 GB | 1 024 GB | 512 GB |
| S448ooo | 21 120 GB | 512 GB | 1 024 GB | 512 GB |
| S448oom | 25 344 GB | 512 GB | 1 024 GB | 512 GB |
| S672 | 12 672 GB | 512 GB | 1 024 GB | 512 GB |
| S672oo | 19 008 GB | 512 GB | 1 024 GB | 512 GB |
| S672m | 25 344 GB | 512 GB | 1 024 GB | 512 GB |
| S672om | 25 344 GB | 512 GB | 1 024 GB | 512 GB |
| S672ooo | 31 680 GB | 512 GB | 1 024 GB | 512 GB |
| S672oom | 38 016 GB | 512 GB | 1 024 GB | 512 GB |
| S896 | 16 896 GB | 512 GB | 1 024 GB | 512 GB |
| S896oo | 25 344 GB | 512 GB | 1 024 GB | 512 GB |
| S896om | 33 792 GB | 512 GB | 1 024 GB | 512 GB |
| S896ooo | 42 240 GB | 512 GB | 1 024 GB | 512 GB |
| S896oom | 50 688 GB | 512 GB | 1 024 GB | 512 GB |


Los volúmenes implementados reales pueden variar en función de la implementación y la herramienta que se usa para mostrar el tamaño de los volúmenes.

Si subdivide una SKU de HANA (instancias grandes), algunos ejemplos de los posibles fragmentos de división serían como sigue:

| Partición de memoria en GB | hana/data | hana/log | hana/shared | hana/log/backup |
| --- | --- | --- | --- | --- |
| 256 | 400 GB | 160 GB | 304 GB | 160 GB |
| 512 | 768 GB | 384 GB | 512 GB | 384 GB |
| 768 | 1 280 GB | 512 GB | 768 GB | 512 GB |
| 1024 | 1 792 GB | 640 GB | 1 024 GB | 640 GB |
| 1536 | 3 328 GB | 768 GB | 1 280 GB | 768 GB |


Estos tamaños son números de volumen aproximados que pueden variar ligeramente en función de la implementación y las herramientas que se usan para analizar los volúmenes. También hay otros tamaños de partición, como 2,5 TB. Estos tamaños de almacenamiento se calculan con una fórmula similar a la utilizada para las particiones anteriores. El término "particiones" no indica en absoluto que el sistema operativo, la memoria o los recursos de CPU se dividan en particiones. Indica las particiones de almacenamiento para las distintas instancias de HANA que es posible que quiera implementar en una única unidad de HANA (instancias grandes). 

Podría necesitar más almacenamiento. Puede agregar almacenamiento comprando almacenamiento adicional en unidades de 1 TB. Este almacenamiento adicional se puede agregar como volumen adicional. También se puede utilizar para extender uno o varios de los volúmenes existentes. No es posible reducir el tamaño de los volúmenes que implementó originalmente y que se documentan principalmente en las tablas anteriores. Tampoco es posible cambiar los nombres de los volúmenes ni los de montaje. Los volúmenes de almacenamiento descritos anteriormente se conectan a las unidades de HANA (instancias grandes) como volúmenes NFS4.

Puede usar instantáneas de almacenamiento para la copia de seguridad y la restauración, además de para la recuperación ante desastres. Para más información, consulte [Alta disponibilidad y recuperación ante desastres de SAP HANA en Azure (instancias grandes)](hana-overview-high-availability-disaster-recovery.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Consulte los [escenarios admitidos por HLI](hana-supported-scenario.md) para información detallada sobre la distribución de almacenamiento en su caso.

## <a name="run-multiple-sap-hana-instances-on-one-hana-large-instance-unit"></a>Ejecución de varias instancias de SAP HANA en una unidad de HANA (instancias grandes)

Se puede hospedar más de una instancia de SAP HANA activa en las unidades de HANA (instancias grandes). Para proporcionar las funcionalidades de instantáneas de almacenamiento y recuperación ante desastres, esta configuración requiere un volumen por cada instancia. Actualmente, las unidades de HANA (instancias grandes) se pueden subdividir de esta forma:

- **S72, S72m, S96, S144, S192**: en incrementos de 256 GB con la unidad más pequeña inicial de 256 GB. Se pueden combinar incrementos diferentes, como 256 GB y 512 GB, hasta el máximo de memoria de la unidad.
- **S144m y S192m**: en incrementos de 256 GB con la unidad más pequeña de 512 GB. Se pueden combinar incrementos diferentes, como 512 GB y 768 GB, hasta el máximo de memoria de la unidad.
- **Clase de tipo II**: en incrementos de 512 GB con la unidad más pequeña inicial de 2 TB. Se pueden combinar incrementos diferentes, como 512 GB, 1 GB y 1,5 GB, hasta el máximo de memoria de la unidad.

Algunos ejemplos de la ejecución de varias instancias de SAP HANA tendrían este aspecto.

| SKU | Tamaño de memoria | Tamaño de almacenamiento | Tamaños con varias bases de datos |
| --- | --- | --- | --- |
| S72 | 768 GB | 3 TB | 1 instancia de HANA de 768 GB<br /> o 1 instancia de 512 GB + 1 instancia de 256 GB<br /> o 3 instancias de 256 GB | 
| S72m | 1,5 TB | 6 TB | 3 instancias de HANA de 512 GB<br />o 1 instancia de 512 GB + 1 instancia de 1 TB<br />o 6 instancias de 256 GB<br />o 1 instancia de 1,5 TB | 
| S192m | 4 TB | 16 TB | 8 instancias de 512 GB<br />o 4 instancias de 1 TB<br />o 4 instancias de 512 GB + 2 instancias de 1 TB<br />o 4 instancias de 768 GB + 2 instancias de 512 GB<br />o 1 instancia de 4 TB |
| S384xm | 8 TB | 22 TB | 4 instancias de 2 TB<br />o 2 instancias de 4 TB<br />o 2 instancias de 3 TB + 1 instancia de 2 TB<br />o 2 instancias de 2,5 TB + 1 instancia de 3 TB<br />o 1 instancia de 8 TB |


Hay también otras variaciones. 

## <a name="encryption-of-data-at-rest"></a>Cifrado de datos en reposo
Desde finales del año 2018, el almacenamiento usado para HANA (instancias grandes) usa un cifrado transparente para los datos cuando se almacenan en los discos. En las implementaciones anteriores, podía optar por obtener los volúmenes cifrados. Si decidió realizar esta opción, puede solicitar que los volúmenes se cifren en línea. El movimiento de volúmenes no cifrados a cifrados es transparente y no requiere ningún tiempo de inactividad. 

Con las SKU de clase de tipo I, se cifra el volumen en el que está almacenado el LUN de arranque. En los sellos de HANA (instancias grandes) de la revisión 3, con la clase de tipo II de HANA (instancias grandes), es necesario cifrar el LUN de arranque con los métodos del sistema operativo. En los sellos de HANA (instancias grandes) de la revisión 4, con unidades de tipo II, el volumen en el que el LUN de arranque está almacenado también se cifra en reposo. 

## <a name="required-settings-for-larger-hana-instances-on-hana-large-instances"></a>Configuración necesaria para instancias más grandes de HANA en instancias grandes de HANA
El almacenamiento usado en instancias grandes de HANA tiene una limitación de tamaño de archivo. La [limitación de tamaño es de 16 TB](https://docs.netapp.com/ontap-9/index.jsp?topic=%2Fcom.netapp.doc.dot-cm-vsmg%2FGUID-AA1419CF-50AB-41FF-A73C-C401741C847C.html) por archivo. A diferencia de las limitaciones de tamaño de archivo de los sistemas de archivos EXT3, HANA no es consciente implícitamente de la limitación de almacenamiento exigida por el almacenamiento de instancias grandes de HANA. Por consiguiente, HANA no creará automáticamente un nuevo archivo de datos cuando se alcance el límite de tamaño de archivo de 16 TB. Cuando HANA intente aumentar el archivo más allá de 16 TB, informará de errores y el servidor de índice terminará bloqueándose.

> [!IMPORTANT]
> Con el fin de impedir que HANA intente aumentar los archivos de datos más allá del límite de tamaño de archivo de 16 TB del almacenamiento de instancias grandes de HANA, tiene que establecer los parámetros siguientes en el archivo de configuración global.ini de HANA:
> 
> - datavolume_striping=true
> - datavolume_striping_size_gb = 15000
> - Vea también la nota de SAP [2400005](https://launchpad.support.sap.com/#/notes/2400005)
> - Tenga en cuenta la nota de SAP [2631285](https://launchpad.support.sap.com/#/notes/2631285).




**Pasos siguientes**
- Consulte [Escenarios admitidos para instancias grandes de HANA](hana-supported-scenario.md).