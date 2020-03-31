---
title: archivo de inclusión
description: archivo de inclusión
services: search
author: HeidiSteen
ms.service: cognitive-search
ms.topic: include
ms.date: 05/06/2019
ms.author: heidist
ms.custom: include file
ms.openlocfilehash: 179f525b7b6a7e51889b14b66df6c537ca56bd75
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80272924"
---
El almacenamiento está limitado por el espacio en disco o el *número máximo* de índices, documentos u otros recursos de alto nivel, lo que ocurra primero. En la tabla siguiente se documentan los límites de almacenamiento. Para conocer los límites máximos sobre índices, documentos y otros objetos, consulte los [límites por recurso](../articles/search/search-limits-quotas-capacity.md#index-limits).

| Resource | Gratuito | Básico<sup>1</sup> | S1 | S2 | S3 | S3&nbsp;HD<sup>2</sup> | L1 | L2 |
| -------- | --- | --- | --- | --- | --- | --- | --- | --- |
| Contrato de nivel de servicio (SLA)<sup>3</sup>  |No |Sí |Sí |Sí |Sí |Sí |Sí |Sí |
| Almacenamiento por partición |50 MB |2 GB |25 GB |100 GB |200 GB |200 GB |1 TB |2 TB |
| Particiones por servicio |N/D |1 |12 |12 |12 |3 |12 |12 |
| Tamaño de la partición |N/D |2 GB |25 GB |100 GB |200 GB |200 GB |1 TB |2 TB |
| Réplicas |N/D |3 |12 |12 |12 |12 |12 |12 |

<sup>1</sup> Básico tiene una partición fija. En este nivel, se usan unidades de búsqueda adicionales para asignar más réplicas a más cargas de trabajo de consulta.

<sup>2</sup> S3 HD tiene un límite máximo de tres particiones, que es inferior al límite de partición para S3. El límite inferior de la partición se impone porque el número de índice para S3 HD es mucho más alto. Como existen límites de servicio en ambos recursos informáticos (almacenamiento y procesamiento) y el contenido (índices y documentos), el límite de contenido se alcanza primero.

<sup>3</sup> Los contratos de nivel de servicio se ofrecen para los servicios facturables en recursos dedicados. Los servicios gratuitos y las características de versión preliminar no tienen SLA. Para los servicios facturables, los SLA tomarán efecto cuando se aprovisione suficiente redundancia para el servicio. Se necesitan dos o más réplicas para los contrato de nivel de servicio de consulta (lectura). Se necesitan tres o más réplicas para los contratos de nivel de servicio de consulta e indexación (lectura y escritura). El número de particiones no se tiene en cuenta en el contrato de nivel de servicio. 