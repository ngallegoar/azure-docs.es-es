---
title: Reglas para asignar nombres a entidades de Azure Data Factory
description: Describe las reglas de nomenclatura para las entidades de Factoría de datos.
services: data-factory
documentationcenter: ''
author: djpmsft
ms.author: daperlov
manager: jroth
ms.reviewer: maghan
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 01/16/2018
ms.openlocfilehash: f922ada663391cf65a61f4e18bba53668f9c4a1a
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/16/2020
ms.locfileid: "81419415"
---
# <a name="azure-data-factory---naming-rules"></a>Azure Data Factory: reglas de nomenclatura

[!INCLUDE[appliesto-adf-xxx-md](includes/appliesto-adf-xxx-md.md)]

La tabla siguiente proporciona las reglas de nomenclatura para los artefactos de Factoría de datos.

| Nombre | Exclusividad del nombre | Comprobaciones de validación |
|:--- |:--- |:--- |
| Data Factory |Único en Microsoft Azure. Los nombres no distinguen mayúsculas de minúsculas, es decir, `MyDF` y `mydf` hacen referencia a la misma factoría de datos. |<ul><li>Cada factoría de datos está asociada exactamente a una suscripción de Azure.</li><li>Los nombres de objeto deben comenzar por una letra o un número, y pueden contener solo letras, números y el carácter de guión (-).</li><li>Los caracteres de guión (-) debe estar inmediatamente precedidos y seguidos por una letra o un número. No se permiten guiones consecutivos en los nombres de contenedor.</li><li>El nombre puede tener entre 3 y 63 caracteres.</li></ul> |
| Servicios vinculados, conjuntos de datos o canalizaciones |Único en una factoría de datos. Los nombres no distinguen mayúsculas de minúsculas. |<ul><li>Los nombres de objeto deben empezar con una letra, un número o un carácter de subrayado (_).</li><li>No se permiten los caracteres siguientes: “.”, “+”, “?”, “/”, “<”, ”>”,”*”,”%”,”&”,”:”,”\\”</li><li>Solo no se permiten guiones ("-") en los nombres de los servicios vinculados y en el de los conjuntos de datos.</li></ul>  |
| Grupo de recursos |Único en Microsoft Azure. Los nombres no distinguen mayúsculas de minúsculas. | Para más información, consulte [Reglas y restricciones de nomenclatura de Azure](/azure/cloud-adoption-framework/ready/azure-best-practices/naming-and-tagging#resource-naming). |

## <a name="next-steps"></a>Pasos siguientes
Aprenda a crear factorías de datos siguiendo las instrucciones paso a paso del artículo [Inicio rápido: Creación de una factoría de datos](quickstart-create-data-factory-powershell.md). 
