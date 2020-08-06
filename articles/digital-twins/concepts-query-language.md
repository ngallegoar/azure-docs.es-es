---
title: Lenguaje de consulta
titleSuffix: Azure Digital Twins
description: Comprenda los aspectos básicos del lenguaje de almacén de consultas Azure Digital Twins.
author: baanders
ms.author: baanders
ms.date: 3/26/2020
ms.topic: conceptual
ms.service: digital-twins
ms.openlocfilehash: f7e9a76309b4d9dcd010b85d1b55f340374be5c4
ms.sourcegitcommit: 46f8457ccb224eb000799ec81ed5b3ea93a6f06f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/28/2020
ms.locfileid: "87337932"
---
# <a name="about-the-query-language-for-azure-digital-twins"></a>Acerca del lenguaje de consulta para Azure Digital Twins

Recuerde que el centro de Azure Digital Twins es el [**grafo de gemelos**](concepts-twins-graph.md), que se crea partir de **gemelos digitales** y **relaciones**. Este grafo se puede consultar para obtener información sobre los gemelos digitales y las relaciones que contiene. Estas consultas se escriben en un lenguaje de consulta personalizado similar a SQL llamado **lenguaje de almacén de consultas Azure Digital Twins**.

Para enviar una consulta al servicio desde una aplicación cliente, usará la [**API de consulta**](https://docs.microsoft.com/dotnet/api/azure.digitaltwins.core.digitaltwinsclient.query?view=azure-dotnet-preview) de Azure Digital Twins. Esto permite a los desarrolladores escribir consultas y aplicar filtros para buscar conjuntos de gemelos digitales en el grafo de gemelos y otra información sobre el escenario de Azure Digital Twins.

## <a name="query-language-features"></a>Características del lenguaje de consulta

Azure Digital Twins proporciona completas funciones de consulta en el grafo de gemelos. Las consultas se describen mediante una sintaxis de tipo SQL, en un lenguaje de consulta similar al [lenguaje de consulta de IoT Hub](../iot-hub/iot-hub-devguide-query-language.md) con muchas características parecidas.

> [!NOTE]
> Todas las operaciones de consulta de Azure Digital Twins distinguen mayúsculas de minúsculas.

Estas son las operaciones disponibles en el lenguaje de consulta de Azure Digital Twins:
* Obtención de gemelos mediante las propiedades de los gemelos digitales (incluidas las [etiquetas](how-to-use-tags.md)).
* Obtención de gemelos mediante las interfaces de los gemelos digitales.
* Obtención de gemelos mediante propiedades de relación.
* Obtención de gemelos en varios tipos de relación (consultas `JOIN`). Hay limitaciones con respecto al número de operaciones `JOIN` permitidas (un nivel para la versión preliminar pública).
* Uso de la función personalizada `IS_OF_MODEL(twinCollection, twinTypeName)`, que permite filtrar en función del [modelo](concepts-models.md) del gemelo. Admite la herencia.
* Uso de funciones escalares: `IS_BOOL`, `IS_DEFINED`, `IS_NULL`, `IS_NUMBER`, `IS_OBJECT`, `IS_PRIMITIVE`, `IS_STRING`, `STARTS_WITH`, `ENDS_WITH`.
* Uso de operadores de comparación de consulta: `IN`/`NIN`, `=`, `!=`, `<`, `>`, `<=` y `>=`.
* Uso de cualquier combinación (operador `AND`, `OR`, `NOT`) de lo anterior.
* Uso de la continuación: se crea una instancia del objeto de consulta con un tamaño de páginas (hasta 100). Puede recuperar de página en página los gemelos digitales. Para ello, proporcione el token de continuación en las llamadas subsiguientes a la API.

## <a name="next-steps"></a>Pasos siguientes

Aprenda a escribir consultas y ver ejemplos de código de cliente en [*Procedimientos: Consulta del grafo gemelo*](how-to-query-graph.md).
