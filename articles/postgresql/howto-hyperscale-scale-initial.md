---
title: 'Tamaño inicial de un grupo de servidores de Hiperescala (Citus): Azure Database for PostgreSQL'
description: Elija el tamaño inicial adecuado para su caso de uso.
author: jonels-msft
ms.author: jonels
ms.service: postgresql
ms.subservice: hyperscale-citus
ms.topic: how-to
ms.date: 11/17/2020
ms.openlocfilehash: 7e68e9f8caad7d7e4bc44bc4e1e55150a78b4a98
ms.sourcegitcommit: 10d00006fec1f4b69289ce18fdd0452c3458eca5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/21/2020
ms.locfileid: "95026410"
---
# <a name="pick-initial-size-for-hyperscale-citus-server-group"></a>Elección del tamaño inicial para un grupo de servidores de Hiperescala (Citus)

El tamaño de un grupo de servidores, tanto en su número de nodos como en su capacidad de hardware, es [fácil de cambiar](howto-hyperscale-scale-grow.md). Sin embargo, sigue siendo necesario elegir un tamaño inicial para un nuevo grupo de servidores. Estas son algunas sugerencias para una opción razonable.

## <a name="multi-tenant-saas-use-case"></a>Caso de uso de SaaS multiinquilino

Al migrar a Hiperescala (Citus) desde una instancia existente de base de datos de PostgreSQL de un solo nodo, elija un clúster en el que el número de núcleos virtuales de trabajo y la RAM total sean iguales a los de la instancia original. En estos escenarios se han detectado mejoras de rendimiento de dos a tres veces mayores porque el particionamiento mejora el uso de recursos, lo que permite, por ejemplo, índices más pequeños.

El recuento de núcleo virtual es realmente la única decisión. La asignación de RAM se determina actualmente según el recuento de núcleos virtuales, como se describe en la página [Opciones de configuración de Hiperescala (Citus)](concepts-hyperscale-configuration-options.md).
El nodo de coordinación no requiere tanta RAM como los trabajo, pero no hay ninguna manera de elegir RAM y núcleos virtuales de manera independiente.

## <a name="real-time-analytics-use-case"></a>Caso de uso de análisis en tiempo real

Total de núcleos virtuales: cuando los datos de trabajo se ajustan a la RAM, se puede esperar una mejora de rendimiento lineal en Hiperescala (Citus) proporcional al número de núcleos de trabajo. Para determinar el número adecuado de núcleos virtuales para sus necesidades, tenga en cuenta la latencia actual de las consultas en la base de datos de un solo nodo y la latencia necesaria en Hiperescala (Citus). Divida la latencia actual según la latencia deseada y redondee el resultado.

RAM del trabajo: el mejor caso sería proporcionar suficiente memoria para que la mayor parte del espacio de trabajo se ajuste a la memoria. El tipo de consultas que utiliza la aplicación afecta a los requisitos de memoria. Puede ejecutar EXPLAIN ANALYZE en una consulta para determinar cuánta memoria se requiere. Recuerde que los núcleos virtuales y la RAM se escalan a la vez, tal y como se describe en el artículo sobre [opciones de configuración de Hiperescala (Citus)](concepts-hyperscale-configuration-options.md).

## <a name="next-steps"></a>Pasos siguientes

- [Escalado de un grupo de servidores](howto-hyperscale-scale-grow.md)
- Más información sobre el grupo de servidores en [opciones de rendimiento](concepts-hyperscale-configuration-options.md).
