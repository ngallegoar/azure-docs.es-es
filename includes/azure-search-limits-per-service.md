---
title: archivo de inclusión
description: archivo de inclusión
services: search
author: HeidiSteen
ms.service: cognitive-search
ms.topic: include
ms.date: 05/11/2020
ms.author: heidist
ms.custom: include file
ms.openlocfilehash: fe5c8129434ddb4eec2dd25a3f123f28b4db221b
ms.sourcegitcommit: 50673ecc5bf8b443491b763b5f287dde046fdd31
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/20/2020
ms.locfileid: "83682622"
---
El límite de los servicios de búsqueda lo marcan el espacio en disco o por el número máximo de índices o indexadores, lo que ocurra primero. En la tabla siguiente se documentan los límites de almacenamiento. Para conocer los límites máximos de los objetos, consulte el artículo en que se especifican los [límites por recurso](../articles/search/search-limits-quotas-capacity.md#index-limits).

| Resource | Gratuito | Básico<sup>1</sup> | S1 | S2 | S3 | S3&nbsp;HD | L1 | L2 |
| -------- | --- | --- | --- | --- | --- | --- | --- | --- |
| Contrato de nivel de servicio (SLA)<sup>2</sup>  |No |Sí |Sí |Sí |Sí |Sí |Sí |Sí |
| Almacenamiento por partición |50 MB |2 GB |25 GB |100 GB |200 GB |200 GB |1 TB |2 TB |
| Particiones por servicio |N/D |1 |12 |12 |12 |3 |12 |12 |
| Tamaño de la partición |N/D |2 GB |25 GB |100 GB |200 GB |200 GB |1 TB |2 TB |
| Réplicas |N/D |3 |12 |12 |12 |12 |12 |12 |

<sup>1</sup> Básico tiene una partición fija. Se pueden usar unidades de búsqueda adicionales para agregar réplicas para volúmenes de consultas mayores.

<sup>2</sup> Los contratos de nivel de servicio están en vigor para los servicios facturables en los recursos dedicados. Los servicios gratuitos y las características de versión preliminar no tienen SLA. Para los servicios facturables, los SLA tomarán efecto cuando se aprovisione suficiente redundancia para el servicio. Se necesitan dos o más réplicas para los contrato de nivel de servicio de consulta (lectura). Se necesitan tres o más réplicas para los contratos de nivel de servicio de consulta e indexación (lectura y escritura). El número de particiones no se tiene en cuenta en el contrato de nivel de servicio. 