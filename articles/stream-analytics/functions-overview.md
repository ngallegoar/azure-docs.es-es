---
title: Funciones definidas por el usuario de Azure Stream Analytics
description: Este artículo contiene información general sobre las funciones definidas por el usuario de Azure Stream Analytics.
author: mamccrea
ms.author: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 04/07/2020
ms.openlocfilehash: d167c603ada885a1a4917c66bab110e4ce38cab4
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "82598375"
---
# <a name="user-defined-functions-in-azure-stream-analytics"></a>Funciones definidas por el usuario de Azure Stream Analytics

El lenguaje de consulta de tipo SQL de Azure Stream Analytics facilita la implementación de la lógica de análisis en tiempo real en los datos de streaming. Stream Analytics proporciona mayor flexibilidad a través de las funciones personalizadas que se invocan en la consulta. El siguiente ejemplo de código es un UDF denominado `sampleFunction` que acepta un parámetro, cada registro de entrada que recibe el trabajo, y el resultado se escribe en la salida como `sampleResult`.

```sql
SELECT 
    UDF.sampleFunction(InputStream) AS sampleResult 
INTO 
    output 
FROM 
    InputStream 
```

## <a name="types-of-functions"></a>Tipos de funciones

Azure Stream Analytics admite los cuatro tipos de funciones siguientes: 

* Funciones definidas por el usuario en JavaScript 
* Agregados definidos por el usuario en JavaScript 
* Funciones de C# definidas por el usuario (que usan Visual Studio) 
* Azure Machine Learning 

Estas funciones se pueden usar para escenarios como la puntuación en tiempo real con modelos de aprendizaje automático, manipulaciones de cadenas, cálculos matemáticos complejos y codificación y descodificación de datos. 

## <a name="limitations"></a>Limitaciones

Las funciones definidas por el usuario no tienen estado y el valor devuelto solo puede ser un valor escalar. No se puede llamar a los puntos de conexión REST externos desde estas funciones definidas por el usuario, ya que es probable que ello afecte al rendimiento del trabajo. 

Azure Stream Analytics no mantiene un registro de todas las invocaciones de funciones y los resultados devueltos. Para garantizar la repetibilidad (por ejemplo, si se vuelve a ejecutar un trabajo desde una marca de tiempo anterior, se obtienen los mismos resultados) no use funciones como `Date.GetData()` o `Math.random()`, ya que estas funciones no devuelven el mismo resultado cada vez que se invocan.  

## <a name="resource-logs"></a>Registros del recurso

Los errores del runtime se consideran graves y se presentan en los registros de actividad y de recursos. Se recomienda que la función controle todas las excepciones y los errores, y que devuelva un resultado válido a la consulta. De esta forma se evita que el trabajo pase al [estado de error](job-states.md).  

## <a name="exception-handling"></a>Control de excepciones

Cualquier excepción durante el procesamiento de datos se considera un error catastrófico al consumir datos en Azure Stream Analytics. Las funciones definidas por el usuario tienen mayor potencial para devolver excepciones y hacer que se detenga el procesamiento. Para evitar este problema, use un bloque *try-catch* en JavaScript o C# para capturar excepciones durante la ejecución del código. Las excepciones que se detectan se pueden registrar y tratar sin provocar un error del sistema. Se recomienda encapsular siempre el código personalizado en un bloque *try-catch* para evitar que se devuelvan excepciones inesperadas en el motor de procesamiento.

## <a name="next-steps"></a>Pasos siguientes

* [Funciones definidas por el usuario de JavaScript de Azure Stream Analytics](stream-analytics-javascript-user-defined-functions.md)
* [Agregados definidos por el usuario en JavaScript para Azure Stream Analytics](stream-analytics-javascript-user-defined-aggregates.md)
* [Desarrollo de funciones definidas por el usuario de .NET Standard para trabajos de Azure Stream Analytics](stream-analytics-edge-csharp-udf-methods.md)
* [Integración de Azure Stream Analytics con Azure Machine Learning](machine-learning-udf.md)
