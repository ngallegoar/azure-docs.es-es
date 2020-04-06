---
title: Datos de Advisor en Azure Resource Graph
description: Creación de consultas para datos de Advisor en Azure Resource Graph
ms.topic: article
ms.date: 03/12/2020
ms.author: sagupt
ms.openlocfilehash: f8ad8fd450bc004d9caa2699922717f38d38b482
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79503916"
---
# <a name="query-for-advisor-data-in-resource-graph-explorer-azure-resource-graph"></a>Consulta de datos de Advisor en Resource Graph Explorer (Azure Resource Graph)

Los recursos de Advisor ahora están incorporados en [Azure Resource Graph](https://azure.microsoft.com/features/resource-graph/). Esta es la base de muchos escenarios de clientes a escala para las recomendaciones de Advisor. Los siguientes son algunos de los escenarios que no eran posibles antes de hacerlo a escala y que ahora se pueden lograr con Resource Graph:
* Proporciona la capacidad de realizar consultas complejas para todas las suscripciones en Azure Portal.
* Recomendaciones resumidas por tipos de categoría (como alta disponibilidad, rendimiento) y tipos de impacto (alto, medio, bajo).
* Todas las recomendaciones para un tipo de recomendación concreto.
* Recuento de recursos afectados por categoría de recomendación.

![Advisor en Azure Resource Graph Explorer](./media/azure-resource-graph-1.png)  


## <a name="advisor-resource-types-in-azure-graph"></a>Tipos de recursos de Advisor en Azure Graph

Tipos de recursos de Advisor disponibles en [Resource Graph](https://docs.microsoft.com/azure/governance/resource-graph/): hay tres tipos de recursos disponibles para las consultas en recursos de Advisor. Esta es la lista de los recursos que ahora están disponibles para realizar consultas en Resource Graph.
* Microsoft.Advisor/configurations
* Microsoft.Advisor/recommendations
* Microsoft.Advisor/suppressions

Estos tipos de recursos se muestran en una nueva tabla denominada AdvisorResources, que también se puede consultar en Resource Graph Explorer en Azure Portal.


## <a name="next-steps"></a>Pasos siguientes

Para más información acerca de las recomendaciones de Advisor, consulte:
* [Introducción a Azure Advisor](advisor-overview.md)
* [Introducción a Advisor](advisor-get-started.md)
* [Recomendaciones sobre el costo de Advisor](advisor-cost-recommendations.md)
* [Recomendaciones sobre rendimiento de Advisor](advisor-performance-recommendations.md)
* [Recomendaciones sobre seguridad de Advisor](advisor-security-recommendations.md)
* [Recomendaciones de excelencia operativa de Advisor](advisor-operational-excellence-recommendations.md)
* [API REST de Advisor](https://docs.microsoft.com/rest/api/advisor/)
