---
author: baanders
description: incluir archivo para límites de Azure Digital Twins
ms.service: digital-twins
ms.topic: include
ms.date: 6/9/2020
ms.author: baanders
ms.openlocfilehash: 60a5f62d4ea23db1052b2e40d10775dfaa33c632
ms.sourcegitcommit: d103a93e7ef2dde1298f04e307920378a87e982a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/13/2020
ms.locfileid: "91989690"
---
### <a name="functional-limits"></a>Límites funcionales

En la tabla siguiente se enumeran los límites funcionales de Azure Digital Twins en la versión preliminar actual.

| Área | Capacidad | Límite predeterminado | ¿Ajustable? |
| --- | --- | --- | --- |
| Recurso de Azure | Número de instancias de Azure Digital Twins en una región por suscripción | 10 | Sí |
| Gemelos digitales | Número de gemelos de una instancia de Azure Digital Twins | 200 000 | Sí |
| Gemelos digitales | Número de relaciones entrantes en un solo gemelo | 5\.000 | No |
| Gemelos digitales | Número de relaciones de salida de un solo gemelo | 5\.000 | No |
| Gemelos digitales | Tamaño máximo de un gemelo individual | 32 KB | No |
| API de Digital Twins | Tamaño máximo de carga de solicitud | 32 KB | No | 
| Enrutamiento | Número de puntos de conexión de una instancia de Azure Digital Twins | 6 | No |
| Enrutamiento | Número de rutas de una instancia de Azure Digital Twins | 6 | Sí |
| Modelos | Número de modelos de una instancia de Azure Digital Twins | 10 000 | Sí |
| Modelos | Número de modelos que se pueden cargar en una única llamada a la API | 250 | No |
| Modelos | Número de elementos devueltos en una única página | 100 | No |
| Consultar | Número de elementos devueltos en una única página | 100 | No |
| Consultar | Número de expresiones `AND` / `OR` de una consulta | 50 | Sí |
| Consultar | Número de elementos de matriz de una cláusula `IN` / `NOT IN` | 50 | Sí |
| Consultar | Número de caracteres de una consulta | 8,000 | Sí |
| Consultar | Número de `JOINS` de una consulta | 5 | Sí |

### <a name="rate-limits"></a>Límites de frecuencia

Esta tabla refleja los límites de frecuencia de distintas API.

| API | Capacidad | Límite predeterminado | ¿Ajustable? |
| --- | --- | --- | --- |
| API de modelos | Número de solicitudes por segundo | 100 | Sí |
| API de Digital Twins | Número de solicitudes por segundo | 1,000 | Sí |
| API de consulta | Número de solicitudes por segundo | 500 | Sí |
| API de consulta | [Unidades de consulta](../articles/digital-twins/concepts-query-units.md) por segundo | 4\.000 | Sí |
| API de rutas de eventos | Número de solicitudes por segundo | 100 | Sí |

### <a name="other-limits"></a>Otros límites

Los límites de los tipos de datos y los campos de los documentos de DTDL para los modelos de Azure Digital Twins se pueden encontrar en la documentación de la especificación en GitHub: [*Lenguaje de definición de Digital Twins (DTDL): versión 2*](https://github.com/Azure/opendigitaltwins-dtdl/blob/master/DTDL/v2/dtdlv2.md).
 
Los detalles de la latencia de consulta y otras instrucciones para escribir consultas en la versión preliminar se pueden encontrar en [*Procedimiento: Consulta del grafo de gemelos*](../articles/digital-twins/how-to-query-graph.md).