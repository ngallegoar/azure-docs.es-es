---
title: Niveles de servicio de Azure NetApp Files | Microsoft Docs
description: Se describe el rendimiento de los niveles de servicio de Azure NetApp Files.
services: azure-netapp-files
documentationcenter: ''
author: b-juche
manager: ''
editor: ''
ms.assetid: ''
ms.service: azure-netapp-files
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 10/12/2020
ms.author: b-juche
ms.openlocfilehash: 7eac6a40476cffe875a03de49c9c9311ffbf4d39
ms.sourcegitcommit: a43a59e44c14d349d597c3d2fd2bc779989c71d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/25/2020
ms.locfileid: "96017073"
---
# <a name="service-levels-for-azure-netapp-files"></a>Niveles de servicio para Azure NetApp Files
Los niveles de servicio son un atributo de los grupos de capacidad. Los niveles de servicio se definen y se diferencian por el rendimiento máximo permitido en un volumen en el grupo de capacidad en función de la cuota asignada a ese volumen.

## <a name="supported-service-levels"></a>Niveles de servicio admitidos

Azure NetApp Files admite tres niveles de servicio: *Ultra*, *Premium* y *estándar*. 

* <a name="Ultra"></a>Almacenamiento Ultra

    La capa de almacenamiento Ultra ofrece hasta 128 MiB/s de rendimiento por 1 TiB de la capacidad proporcionada. 

* <a name="Premium"></a>Almacenamiento Premium

    La capa de almacenamiento Premium ofrece hasta 64 MiB/s de rendimiento por 1 TiB de la capacidad proporcionada. 

* <a name="Standard"></a>Almacenamiento Standard

    La capa de almacenamiento Estándar ofrece hasta 16 MiB/s de rendimiento por 1 TiB de la capacidad proporcionada.

## <a name="throughput-limits"></a>Límites de rendimiento

El límite de rendimiento de un volumen viene determinado por una combinación de los siguientes factores:
* El nivel de servicio del grupo de capacidad al que pertenece el volumen.
* La cuota asignada al volumen.  
* El tipo de QoS (*automática* o *manual*) del grupo de capacidad  

### <a name="throughput-limit-examples-of-volumes-in-an-auto-qos-capacity-pool"></a>Ejemplos de límite de rendimiento de volúmenes en un grupo de capacidad de QoS automática

En el siguiente diagrama se muestran ejemplos de límite de rendimiento de volúmenes en un grupo de capacidad de QoS automática:

![Ilustración de nivel de servicio](../media/azure-netapp-files/azure-netapp-files-service-levels.png)

* En el ejemplo 1, un volumen de un grupo de capacidad de QoS automática con el nivel de almacenamiento Premium y una cuota de 2 TiB tendrá asignado un límite de rendimiento de 128 MiB/s (2 TiB x 64 MiB/s). Este escenario será así independientemente del tamaño del grupo de capacidad o del consumo de volumen real.

* En el ejemplo 2, un volumen de un grupo de capacidad de QoS automática con el nivel de almacenamiento Premium y una cuota de 100 GiB tendrá asignado un límite de rendimiento de 6,25 MiB/s (0.09765625 TiB * 64 MiB/s). Este escenario será así independientemente del tamaño del grupo de capacidad o del consumo de volumen real.

### <a name="throughput-limit-examples-of-volumes-in-a-manual-qos-capacity-pool"></a>Ejemplos de límite de rendimiento de volúmenes en un grupo de capacidad de QoS manual 

Si utiliza un grupo de capacidad de QoS manual, puede asignar la capacidad y el rendimiento de un volumen de forma independiente. Al crear un volumen en un grupo de capacidad de QoS manual, puede especificar el valor de rendimiento (MiB/S). El rendimiento total asignado a los volúmenes de un grupo de capacidad de QoS manual depende del tamaño del grupo y del nivel de servicio. Está limitado por (tamaño del grupo de capacidad en el nivel de servicio TiB x rendimiento/TiB). Por ejemplo, un grupo de capacidad de 10 TiB con el nivel de servicio Ultra tiene una capacidad de rendimiento total de 1280 MiB/s (10 TiB x 128 MiB/s/TiB) disponible para los volúmenes.

Por ejemplo, para un sistema SAP HANA, este grupo de capacidad puede usarse para crear los siguientes volúmenes. Cada volumen proporciona el tamaño y rendimiento individuales para satisfacer los requisitos de la aplicación:

* Volumen de datos de SAP HANA: Tamaño 4 TiB con hasta 704 MiB/s
* Volumen de registro de SAP HANA: Tamaño 0,5 TiB con hasta 256 MiB/s
* Volumen de recursos compartidos de SAP HANA: Tamaño 1 TiB con hasta 64 MiB/s
* Volumen de copia de seguridad de SAP HANA: Tamaño 4,5 TiB con hasta 256 MiB/s

En el siguiente diagrama se muestran los escenarios de los volúmenes de SAP HANA:

![Escenarios de volúmenes de SAP HANA de QoS](../media/azure-netapp-files/qos-sap-hana-volume-scenarios.png) 

## <a name="next-steps"></a>Pasos siguientes

- [Página de precios de Azure NetApp Files](https://azure.microsoft.com/pricing/details/storage/netapp/)
- [Modelo de costo de Azure NetApp Files](azure-netapp-files-cost-model.md) 
- [Creación de un grupo de capacidad](azure-netapp-files-set-up-capacity-pool.md)
- [Acuerdo de Nivel de Servicio (SLA) para Azure NetApp Files](https://azure.microsoft.com/support/legal/sla/netapp/)
- [Cambio dinámico del nivel de servicio de un volumen](dynamic-change-volume-service-level.md) 
- [Objetivos de nivel de servicio para la replicación entre regiones](cross-region-replication-introduction.md#service-level-objectives)
