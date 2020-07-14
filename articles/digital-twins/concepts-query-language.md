---
title: Lenguaje de consulta
titleSuffix: Azure Digital Twins
description: Comprenda los aspectos básicos del lenguaje de almacén de consultas Azure Digital Twins.
author: baanders
ms.author: baanders
ms.date: 3/26/2020
ms.topic: conceptual
ms.service: digital-twins
ms.openlocfilehash: 3196004015046b4d3d2789745c80d323bacdced9
ms.sourcegitcommit: 93462ccb4dd178ec81115f50455fbad2fa1d79ce
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/06/2020
ms.locfileid: "85985248"
---
# <a name="about-the-query-language-for-azure-digital-twins"></a>Acerca del lenguaje de consulta para Azure Digital Twins

Recuerde que el centro de Azure Digital Twins es el [**grafo de gemelos**](concepts-twins-graph.md), que se crea partir de **gemelos digitales** y **relaciones**. Este grafo se puede consultar para obtener información sobre los gemelos digitales y las relaciones que contiene. Estas consultas se escriben en un lenguaje de consulta personalizado similar a SQL llamado **lenguaje de almacén de consultas Azure Digital Twins**.

Para enviar una consulta al servicio desde una aplicación cliente, usará la **API Query** de Azure Digital Twins. Esto permite a los desarrolladores escribir consultas y aplicar filtros para buscar conjuntos de gemelos digitales en el grafo de gemelos y otra información sobre el escenario de Azure Digital Twins.

## <a name="query-language-features"></a>Características del lenguaje de consulta

Azure Digital Twins proporciona completas funciones de consulta en el grafo de gemelos. Las consultas se describen mediante una sintaxis de tipo SQL, en un lenguaje de consulta similar al [lenguaje de consulta de IoT Hub](../iot-hub/iot-hub-devguide-query-language.md) con muchas características parecidas.

Estas son las operaciones disponibles en el lenguaje de consulta de Azure Digital Twins:
* Obtención de gemelos mediante las propiedades de los gemelos digitales.
* Obtención de gemelos mediante las interfaces de los gemelos digitales.
* Obtención de gemelos mediante propiedades de relación.
* Obtención de gemelos en varios tipos de relación (consultas `JOIN`). Hay limitaciones con respecto al número de operaciones `JOIN` permitidas (un nivel para la versión preliminar pública).
* Uso de la función personalizada `IS_OF_MODEL(twinCollection, twinTypeName)`, que permite filtrar en función del [modelo](concepts-models.md) del gemelo. Admite la herencia.
* Uso de cualquier combinación (operador `AND`, `OR`, `NOT`) de lo anterior.
* Uso de funciones escalares: `IS_BOOL`, `IS_DEFINED`, `IS_NULL`, `IS_NUMBER`, `IS_OBJECT`, `IS_PRIMITIVE`, `IS_STRING`, `STARTS_WITH`, `ENDS_WITH`.
* Uso de operadores de comparación de consulta: `AND`/`OR`/`NOT`,  `IN`/`NOT IN`, `STARTSWITH`/`ENDSWITH`, `=`, `!=`, `<`, `>`, `<=`, `>=`.
* Uso de la continuación: se crea una instancia del objeto de consulta con un tamaño de páginas (hasta 100). Puede recuperar los gemelos digitales una página a la vez, si repite las llamadas al método `nextAsTwin`.

## <a name="next-steps"></a>Pasos siguientes

Obtenga información sobre cómo escribir consultas y ver ejemplos de código de cliente en [Procedimientos para consultar el grafo de gemelos](how-to-query-graph.md).