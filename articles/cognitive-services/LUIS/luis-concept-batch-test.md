---
title: 'Pruebas por lotes: LUIS'
titleSuffix: Azure Cognitive Services
description: Use las pruebas por lotes para trabajar continuamente en la aplicación para refinarla y mejorar la comprensión del lenguaje.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 10/25/2019
ms.author: diberry
ms.openlocfilehash: a5ebd9b05b2dea9e04d4c9745c13d692ea88fcb8
ms.sourcegitcommit: 50673ecc5bf8b443491b763b5f287dde046fdd31
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/20/2020
ms.locfileid: "83680430"
---
# <a name="batch-testing-with-1000-utterances-in-luis-portal"></a>Pruebas por lotes con 1 000 expresiones en el portal de LUIS

Las pruebas por lotes validan la versión entrenada activa para medir la precisión de la predicción. Una prueba por lotes ayuda a ver la precisión de cada intención y entidad en la versión activa, mostrando los resultados en un gráfico. Revise los resultados de la prueba por lotes para adoptar las medidas apropiadas para mejorar la precisión, como agregar más expresiones de ejemplo a una intención si la aplicación presenta errores frecuentes de identificación de la intención correcta o etiquetar entidades dentro de la expresión.

## <a name="group-data-for-batch-test"></a>Agrupación de datos para pruebas por lotes

Es importante que las expresiones utilizadas para las pruebas por lotes sean nuevas en LUIS. Si tiene un conjunto de datos de expresiones, divida las expresiones en tres conjuntos: expresiones de ejemplo agregadas a una intención, expresiones recibidas desde el punto de conexión publicado y expresiones utilizadas para las pruebas por lotes de LUIS después de su entrenamiento.

## <a name="a-data-set-of-utterances"></a>Un conjunto de datos de expresiones

Envíe un archivo por lotes de expresiones, conocido como *conjunto de datos*, para las pruebas por lotes. El conjunto de datos es un archivo con formato JSON que contiene un máximo de 1000 expresiones **no duplicadas** etiquetadas. Puede probar hasta diez conjuntos de datos en una aplicación. Si necesita probar más, elimine un conjunto de datos y luego agregue uno nuevo.

|**Reglas**|
|--|
|*Ninguna expresión duplicada|
|1000 expresiones o menos|

*Los duplicados se consideran coincidencias exactas de cadena; no se crea el token de ninguna coincidencia primero.

## <a name="entities-allowed-in-batch-tests"></a>Entidades permitidas en las pruebas por lotes

Todas las entidades personalizadas del modelo aparecen en el filtro de entidades de pruebas por lotes incluso si no existen las entidades correspondientes en los datos del archivo por lotes.

<a name="json-file-with-no-duplicates"></a>
<a name="example-batch-file"></a>

## <a name="batch-file-format"></a>Formato del archivo por lotes

El archivo por lotes está compuesto de expresiones. Cada expresión tiene que tener una predicción de intenciones esperada junto con todas las [entidades de aprendizaje automático](luis-concept-entity-types.md#types-of-entities) que espera detectar.

## <a name="batch-syntax-template-for-intents-with-entities"></a>Plantilla de sintaxis de lotes para intenciones con entidades

Use la plantilla siguiente para iniciar el archivo por lotes:

```JSON
[
  {
    "text": "example utterance goes here",
    "intent": "intent name goes here",
    "entities":
    [
        {
            "entity": "entity name 1 goes here",
            "startPos": 14,
            "endPos": 23
        },
        {
            "entity": "entity name 2 goes here",
            "startPos": 14,
            "endPos": 23
        }
    ]
  }
]
```

El archivo por lotes usa las propiedades **startPos** y **endPos** para tener en cuenta el inicio y el final de una entidad. Los valores se basan en cero y no pueden empezar ni terminar en un espacio. Esto es diferente de los registros de consultas que usan las propiedades startIndex y endIndex.

[!INCLUDE [Entity roles in batch testing - currently not supported](../../../includes/cognitive-services-luis-roles-not-supported-in-batch-testing.md)]

## <a name="batch-syntax-template-for-intents-without-entities"></a>Plantilla de sintaxis de lotes para intenciones sin entidades

Use la plantilla siguiente para iniciar el archivo por lotes sin entidades:

```JSON
[
  {
    "text": "example utterance goes here",
    "intent": "intent name goes here",
    "entities": []
  }
]
```

Si no quiere probar entidades, incluya la propiedad `entities` y establezca el valor como una matriz vacía, `[]`.


## <a name="common-errors-importing-a-batch"></a>Errores comunes al importar un lote

Estos son algunos de los errores comunes:

> * Más de 1000 expresiones
> * Un objeto JSON de expresiones que no tiene una propiedad de entidades. La propiedad puede ser una matriz vacía.
> * Palabras etiquetadas en varias entidades
> * La etiqueta de entidad empieza o acaba en un espacio.

## <a name="batch-test-state"></a>Estado de la prueba por lotes

LUIS realiza un seguimiento del estado de la última prueba de cada conjunto de datos. Esto incluye el tamaño (número de expresiones del lote), la fecha de la última ejecución y el último resultado (número de expresiones cuya predicción es correcta).

<a name="sections-of-the-results-chart"></a>

## <a name="batch-test-results"></a>Resultados de la prueba por lotes

El resultado de las pruebas por lotes es un gráfico de dispersión, que se conoce como una matriz de error. Este gráfico es una comparación de cuatro vías de las expresiones del archivo por lotes y de las intenciones y las entidades de predicción del modelo actual.

Los puntos de datos de las secciones **Falso positivo** y **Falso negativo** indican errores, que deben investigarse. Si todos los puntos de datos están en las secciones **Verdadero positivo** y **Verdadero negativo**, la precisión de la aplicación es perfecta en este conjunto de datos.

![Cuatro secciones del gráfico](./media/luis-concept-batch-test/chart-sections.png)

Este gráfico le ayuda a encontrar expresiones que LUIS predice incorrectamente en función de su aprendizaje actual. Los resultados se muestran por región del gráfico. Seleccione puntos individuales del gráfico para revisar la información sobre las expresiones o seleccione el nombre de la región para revisar los resultados de las expresiones en dicha región.

![Pruebas por lotes](./media/luis-concept-batch-test/batch-testing.png)

## <a name="errors-in-the-results"></a>Errores en los resultados

Los errores de la prueba por lotes indican intenciones de que su predicción no se ha realizado según se indica en el archivo por lotes. Los errores se indican en las dos secciones de color rojo del gráfico.

La sección de falso positivo indica que una expresión coincidía con una intención o entidad cuando no debería ser así. La sección de falso positivo indica que una expresión coincidía con una intención o entidad cuando no debería ser así.

## <a name="fixing-batch-errors"></a>Corrección de errores de lotes

Si hay errores en las pruebas por lotes, puede agregar más expresiones a una intención o etiquetar más expresiones con al entidad para ayudar a LUIS a realizar la discriminación entre intenciones. Si ha agregado expresiones y las ha etiquetado y aún obtiene errores de predicción en las pruebas por lotes, considere la adición de una característica [lista de frases](luis-concept-feature.md) con un vocabulario específico de dominios para ayudar a LUIS a aprender más rápido.

## <a name="next-steps"></a>Pasos siguientes

* Obtenga información sobre cómo [probar un lote](luis-how-to-batch-test.md).
