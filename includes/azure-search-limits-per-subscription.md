---
title: archivo de inclusión
description: archivo de inclusión
author: HeidiSteen
ms.service: cognitive-search
ms.topic: include
ms.date: 05/06/2019
ms.author: heidist
ms.custom: include file
ms.openlocfilehash: ee430241173a6c19e2a32e176f28411631d9cb19
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "80272836"
---
Puede crear varios servicios dentro de una suscripción. Cada uno de ellos se puede aprovisionar en un nivel concreto. La única limitación es el número de servicios permitidos en cada nivel. Por ejemplo, puede crear hasta doce servicios en el nivel básico y otros doce en el nivel de S1 dentro de la misma suscripción. Para más información sobre los niveles, consulte [Selección de una SKU o de un plan de tarifa de Azure Cognitive Search](../articles/search/search-sku-tier.md).

El límite máximo de servicios se puede elevar a petición. Si necesita tener más servicios en la misma suscripción, póngase en contacto con el soporte técnico de Azure.

| Resource            | Gratis<sup></sup>1{3} | Básica | S1  | S2 | S3 | S3&nbsp;HD | L1 | L2 |
| ------------------- | ---- | ----- | --- | -- | -- | ----- | -- | -- |
| Servicios máximos    |1     | 16    | 16  | 8  | 6  | 6     | 6  | 6  |
| Escala máxima en unidades de búsqueda (SU)<sup>2</sup> |N/D |3 unidades de búsqueda |36 unidades de búsqueda |36 unidades de búsqueda |36 unidades de búsqueda |36 unidades de búsqueda |36 unidades de búsqueda |36 unidades de búsqueda |

<sup>1</sup>Gratis se basa en recursos compartidos, no dedicados. El escalado vertical no se admite en los recursos compartidos.

<sup>2</sup> Las unidades de búsqueda son unidades facturables, asignadas como *réplica* o como *partición*. Ambos recursos se necesitan para las operaciones de almacenamiento, indexación y consulta. Para más información sobre los cálculos de SU, consulte [Escalado de niveles de recursos para cargas de trabajo de indexación y consulta en Azure Search](../articles/search/search-capacity-planning.md). 