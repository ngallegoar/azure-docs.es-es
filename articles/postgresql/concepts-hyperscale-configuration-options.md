---
title: Opciones de configuración de Hiperescala (Citus) para Azure Database for PostgreSQL
description: Opciones para un grupo de servidores Hiperescala (Citus), incluidas las regiones, el almacenamiento y el proceso de nodos.
author: jonels-msft
ms.author: jonels
ms.service: postgresql
ms.subservice: hyperscale-citus
ms.topic: conceptual
ms.date: 7/1/2020
ms.openlocfilehash: 8dc70eaeb9e2c2f5d4cdfef37619e4b04217782e
ms.sourcegitcommit: 845a55e6c391c79d2c1585ac1625ea7dc953ea89
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2020
ms.locfileid: "85964522"
---
# <a name="azure-database-for-postgresql--hyperscale-citus-configuration-options"></a>Opciones de configuración de Hiperescala (Citus) para Azure Database for PostgreSQL

## <a name="compute-and-storage"></a>Compute y Storage
 
Puede seleccionar la configuración de proceso y almacenamiento por separado para los nodos de trabajo y el nodo de coordinación en un grupo de servidores Hiperescala (Citus).  Los recursos de proceso se proporcionan como núcleos virtuales, que representan la CPU lógica del hardware subyacente. El tamaño de almacenamiento para el aprovisionamiento se refiere a la capacidad disponible para los nodos de trabajo y de coordinación en el grupo de servidores Hiperescala (Citus). El almacenamiento incluye archivos de base de datos, archivos temporales, registros de transacciones y registros de servidor PostgreSQL.
 
| Recurso              | Nodo de trabajo           | Nodo de coordinación      |
|-----------------------|-----------------------|-----------------------|
| Proceso, núcleos virtuales       | 4, 8, 16, 32, 64      | 4, 8, 16, 32, 64      |
| Memoria por núcleo virtual, GiB | 8                     | 4                     |
| Tamaño de almacenamiento, TiB     | 0,5, 1, 2             | 0,5, 1, 2             |
| Tipo de almacenamiento          | Uso general (SSD) | Uso general (SSD) |
| E/S                  | Hasta 3 IOPS/GiB      | Hasta 3 IOPS/GiB      |

La cantidad total de memoria RAM en un solo nodo de Hiperescala (Citus) se basa en el número seleccionado de núcleos virtuales.

| Núcleos virtuales | Un nodo de trabajo, GiB de RAM | Nodo de coordinación, GiB de RAM |
|--------|--------------------------|---------------------------|
| 4      | 32                       | 16                        |
| 8      | 64                       | 32                        |
| 16     | 128                      | 64                        |
| 32     | 256                      | 128                       |
| 64     | 432                      | 256                       |

La cantidad total de almacenamiento que se aprovisiona también define la capacidad de E/S disponible para cada nodo de coordinación y de trabajo.

| Tamaño de almacenamiento, TiB | Número máximo de IOPS |
|-------------------|--------------|
| 0.5               | 1536        |
| 1                 | 3072        |
| 2                 | 6148        |

Para todo el clúster de Hiperescala (Citus), las IOPS agregadas alcanzan los siguientes valores:

| Nodos de trabajo | 0,5 TiB, total de IOPS | 1 TiB, total de IOPS | 2 TiB, total de IOPS |
|--------------|---------------------|-------------------|-------------------|
| 2            | 3072               | 6144             | 12 296            |
| 3            | 4608               | 9216             | 18 444            |
| 4            | 6144               | 12 288            | 24 592            |
| 5            | 7680               | 15 360            | 30 740            |
| 6            | 9216               | 18 432            | 36 888            |
| 7            | 10 752              | 21 504            | 43 036            |
| 8            | 12 288              | 24 576            | 49 184            |
| 9            | 13 824              | 27 648            | 55 332            |
| 10           | 15 360              | 30 720            | 61 480            |
| 11           | 16 896              | 33 792            | 67 628            |
| 12           | 18 432              | 36 864            | 73 776            |
| 13           | 19 968              | 39 936            | 79 924            |
| 14           | 21 504              | 43 008            | 86 072            |
| 15           | 23 040              | 46 080            | 92 220            |
| 16           | 24 576              | 49 152            | 98 368            |
| 17           | 26 112              | 52 224            | 104 516           |
| 18           | 27 648              | 55 296            | 110 664           |
| 19           | 29 184              | 58 368            | 116 812           |
| 20           | 30 720              | 61 440            | 122 960           |

## <a name="regions"></a>Regions
Los grupos de servidores Hiperescala (Citus) están disponibles en las siguientes regiones de Azure:

* América:
    * Centro de Canadá
    * Centro de EE. UU.
    * Este de EE. UU.
    * Este de EE. UU. 2
    * Centro-Norte de EE. UU
    * Oeste de EE. UU. 2
* Asia Pacífico:
    * Este de Australia
    * Japón Oriental
    * Centro de Corea del Sur
    * Sudeste de Asia
* Europa:
    * Norte de Europa
    * Sur de Reino Unido
    * Oeste de Europa

Es posible que algunas de estas regiones no se activen inicialmente en todas las suscripciones de Azure. Si desea utilizar una región de la lista anterior y no la ve en su suscripción, o si desea usar una región que no está en esta lista, abra una [solicitud de soporte técnico](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest).

## <a name="limits-and-limitations"></a>Límites y limitaciones

En la siguiente sección se describen los límites de capacidad y funcionales en el servicio de Hiperescala (Citus).

### <a name="maximum-connections"></a>Número máximo de conexiones

Cada conexión de PostgreSQL (incluso inactiva) usa al menos 10 MB de memoria, por lo que es importante limitar las conexiones simultáneas. Estos son los límites que elegimos para mantener los nodos en buen estado:

* Nodo de coordinación
   * Número máximo de conexiones: 300
   * Número máximo de conexiones de usuario: 297
* Nodo de trabajo
   * Número máximo de conexiones: 600
   * Número máximo de conexiones de usuario: 597

Se producirá un error al intentar conectarse más allá de estos límites. El sistema reserva tres conexiones para supervisar los nodos, por lo que hay tres menos conexiones disponibles para las consultas de usuario que el total de conexiones.

El establecimiento de las nuevas conexiones lleva su tiempo. Esto funciona con la mayoría de las aplicaciones, que solicitan muchas conexiones de corta duración. Se recomienda usar un agrupador de conexiones para reducir las transacciones inactivas y reutilizar las conexiones existentes. Para más información, visite nuestra [entrada de blog](https://techcommunity.microsoft.com/t5/azure-database-for-postgresql/not-all-postgres-connection-pooling-is-equal/ba-p/825717).

### <a name="storage-scaling"></a>Escalado de almacenamiento

El almacenamiento en el coordinador y los nodos de trabajo se pueden escalar verticalmente (aumentar), pero no se puede reducir verticalmente (disminuir).

### <a name="storage-size"></a>Tamaño de almacenamiento

Se admite hasta 2 TiB de almacenamiento en los nodos de coordinador y de trabajo. Consulte las opciones de almacenamiento disponibles y el cálculo de IOPS [anterior](#compute-and-storage) para los tamaños de nodo y clúster.

## <a name="pricing"></a>Precios
Para conocer la información más actualizada sobre precios, consulte la [página de precios](https://azure.microsoft.com/pricing/details/postgresql/) del servicio.
Para ver el costo de la configuración deseada, en [Azure Portal](https://portal.azure.com/#create/Microsoft.PostgreSQLServer) se muestra el costo mensual en la pestaña **Configurar** según las opciones que seleccione. Si no tiene una suscripción de Azure, puede usar la calculadora de precios de Azure para obtener un precio estimado. En el sitio web [Calculadora de precios de Azure](https://azure.microsoft.com/pricing/calculator/), seleccione **Agregar elementos**, expanda la categoría **Bases de datos** y elija **Azure Database for PostgreSQL: Hiperescala (Citus)** para personalizar las opciones.
 
## <a name="next-steps"></a>Pasos siguientes
Obtenga información sobre cómo [crear un grupo de servidores Hiperescala (Citus) en el portal](quickstart-create-hyperscale-portal.md).
