---
title: Límites de recursos y objetos de Azure Analysis Services | Microsoft Docs
description: En este artículo se describen los límites de recursos y objetos de un servidor de Azure Analysis Services.
author: minewiskan
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 05/19/2020
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: c2caab4e449f4299d00fff14b697887ec00f35e6
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "83697495"
---
# <a name="analysis-services-resource-and-object-limits"></a>Límites de recursos y objetos de Azure Analysis Services

En este artículo se describen los límites de recursos y objetos de modelo.

## <a name="tier-limits"></a>Límites por nivel

Para QPU y los límites de memoria para los niveles de desarrollador, básico y estándar, consulte la [página de precios de Azure Analysis Services](https://azure.microsoft.com/pricing/details/analysis-services/).

## <a name="object-limits"></a>Límites de objetos

Estos límites son teóricos. El rendimiento se verá reducido con números más bajos.

|Object|Tamaños/números máximos|  
|------------|----------------------------|  
|Bases de datos en una instancia|16 000|  
|Número combinado de tablas y columnas en una base de datos|16 000|  
|Filas de una tabla|Sin límite<br /><br /> **Advertencia:** Con la restricción de que ninguna columna de la tabla puede tener más de 1 999 999 997 valores distintos.|  
|Jerarquías de una tabla|15 999|  
|Niveles de una jerarquía|15 999|  
|Relaciones|8,000|  
|Columnas de clave en toda la tabla|15 999|  
|Medidas en las tablas|2^31-1 = 2 147 483 647|  
|Celdas devueltas por una consulta|2^31-1 = 2 147 483 647|  
|Tamaño del registro de la consulta de origen|64 K|  
|Longitud de nombres de objeto|512 caracteres|  


