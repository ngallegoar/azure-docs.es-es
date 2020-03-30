---
title: 'Diseño con modelos: LUIS'
titleSuffix: Azure Cognitive Services
description: Language Understanding proporciona varios tipos de modelos. Algunos modelos se puede usar de varias formas.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 10/25/2019
ms.author: diberry
ms.openlocfilehash: d721ceb25b3ce2408563a0bed16457d05affe7b4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79218806"
---
# <a name="design-with-intent-and-entity-models"></a>Diseñe con intenciones y modelos de entidad 

Language Understanding proporciona varios tipos de modelos. Algunos modelos se puede usar de varias formas. 

## <a name="v3-authoring-uses-machine-teaching"></a>Creación V3 usa la enseñanza automática

LUIS permite a los usuarios enseñar conceptos fácilmente a una máquina. A continuación, el equipo puede compilar modelos (aproximaciones funcionales de conceptos como clasificadores y extractores) que se pueden usar para potenciar las aplicaciones inteligentes. Aunque LUIS está basado en el aprendizaje automático, no es necesario conocer el aprendizaje automático para usarlo. En su lugar, los profesores de las máquinas comunican conceptos a LUIS al mostrarle ejemplos positivos y negativos del concepto y explicar cómo se debe modelar un concepto a través de otros conceptos relacionados. Los profesores también pueden mejorar el modelo de LUIS de forma interactiva. Para ello, se deben identificar y corregir los errores de predicción. 

## <a name="v3-authoring-model-decomposition"></a>Descomposición de modelos de creación V3

LUIS admite la _descomposición de modelos_ con las API de creación V3, de modo que se divide el modelo en partes más pequeñas. Esto le permite compilar los modelos con confianza en la construcción y predicción de las distintas partes.

La descomposición del modelo tiene las siguientes partes:

* [intenciones](#intents-classify-utterances);
    * [descriptores](#descriptors-are-features) proporcionados por las características;
* [entidades de aprendizaje automático](#machine-learned-entities);
    * [subcomponentes](#entity-subcomponents-help-extract-data) (también entidades de aprendizaje automático);
        * [descriptores](#descriptors-are-features) proporcionados por las características; 
        * [restricciones](#constraints-are-text-rules) proporcionadas por entidades sin aprendizaje automático, como expresiones regulares y listas.

## <a name="v2-authoring-models"></a>Modelos de creación V2

LUIS admite entidades compuestas con las API de creación V2. Esto proporciona una descomposición de modelos similar, pero no es igual que la descomposición de modelos V3. La arquitectura de modelo recomendada es pasar a la descomposición del modelo en las API de creación V3. 

## <a name="intents-classify-utterances"></a>Las intenciones clasifican expresiones

Una intención clasifica expresiones de ejemplo para enseñar a LUIS sobre dicha intención. Los expresiones de ejemplo de una intención se usan como ejemplos positivos de la expresión. Estas mismas expresiones se usan como ejemplos negativos en todas las demás intenciones.

Considere una aplicación que necesita determinar la intención de un usuario de ordenar un libro. Además, una aplicación necesita la dirección de envío del cliente. Esta aplicación tiene dos intenciones: `OrderBook` y `ShippingLocation`.

La siguiente expresión es un **ejemplo positivo** para la intención `OrderBook` y un **ejemplo negativo** para la intención `ShippingLocation` y `None`: 

`Buy the top-rated book on bot architecture.`

El resultado de las intenciones bien diseñadas, con sus expresiones de ejemplo, es una predicción de alto nivel de intención. 

## <a name="entities-extract-data"></a>Las entidades extraen datos

Una entidad representa una unidad de datos que quiere extraer de la expresión. 

### <a name="machine-learned-entities"></a>Entidades de aprendizaje automático

Una entidad de aprendizaje automático es una entidad de nivel superior que contiene subcomponentes, los cuales también son entidades de aprendizaje automático. 

**Use una entidad de aprendizaje automático**:

* cuando la aplicación cliente necesita los subcomponentes;
* para ayudar a que el algoritmo de aprendizaje automático descomponga entidades.

Cada subcomponente puede tener:

* subcomponentes;
* restricciones (entidad de expresión regular o entidad de lista);
* descriptores (características como una lista de frases). 

Un ejemplo de una entidad de aprendizaje automático es una orden de boleto de avión. En teoría, se trata de una única transacción con muchas unidades de datos más pequeñas, como la fecha, la hora, la cantidad de puestos, el tipo de asiento (como primera clase o clase económica), la ubicación de origen, la ubicación de destino y la elección de comida.


### <a name="entity-subcomponents-help-extract-data"></a>Los subcomponentes de entidad ayudan a extraer los datos

Un subcomponente es una entidad secundaria de aprendizaje automático dentro de una entidad primaria de aprendizaje automático. 

**Use el subcomponente para**:

* descomponer las partes de la entidad de aprendizaje automático (entidad primaria).

Lo siguiente representa una entidad de aprendizaje automático con todos estos fragmentos de datos independientes:

* TravelOrder (entidad de aprendizaje automático);
    * DateTime (datetimeV2 precompilado);
    * Location (entidad de aprendizaje automático);
        * Origin (rol encontrado a través de un contexto, como `from`);
        * Destination (rol encontrado a través de un contexto, como `to`);
    * Seating (entidad de aprendizaje automático);
        * Quantity (número precompilado);
        * Quality (entidad de aprendizaje automático con un descriptor de la lista de frases);
    * Meals (entidad de aprendizaje automático con restricciones de la entidad de lista, como opciones de comida).

Algunos de estos datos, como la ubicación de origen y la ubicación de destino, se deben extraer del contexto de la expresión, quizás con palabras tales como `from` y `to`. Otras partes de los datos se pueden extraer con coincidencias exactas de cadena (`Vegan`) o entidades precompiladas (geographyV2 de `Seattle` y `Cairo`). 

Diseña la forma en que los datos se comparan y extraen, qué modelos elige y cómo los configura.

### <a name="constraints-are-text-rules"></a>Las restricciones son reglas de texto

Una restricción es una regla de coincidencia de texto proporcionada por una entidad sin aprendizaje automático, como la entidad de expresión regular o la entidad de lista. La restricción se aplica en el momento de la predicción para limitar la predicción y proporcionar la resolución de la entidad necesaria para la aplicación cliente. Estas reglas se definen al crear el subcomponente. 

**Use una restricción**:
* cuando conozca el texto exacto que se va a extraer.

Las restricciones incluyen:

* entidades de [expresión regular](reference-entity-regular-expression.md);
* entidades de [lista](reference-entity-list.md); 
* entidades [precompiladas](luis-reference-prebuilt-entities.md).

Para continuar con el ejemplo del boleto de avión, los códigos de aeropuerto pueden estar en una entidad de lista para las coincidencias exactas de texto. 

En el caso de una lista de aeropuertos, la entrada de lista para Seattle es el nombre de la ciudad (`Seattle`) y los sinónimos de Seattle incluyen el código de aeropuerto de Seattle junto con las poblaciones y ciudades circundantes:

|Sinónimos de la entidad de lista `Seattle`|
|--|
|`Sea`|
|`seatac`|
|`Bellevue`|

Si solo quiere reconocer códigos de 3 letras para códigos de aeropuerto, use una expresión regular como restricción. 

`/^[A-Z]{3}$/`

## <a name="intents-versus-entities"></a>Intenciones frente a entidades

Una intención es el resultado deseado de una expresión _completa_, mientras que las entidades son fragmentos de datos extraídos de la expresión. Normalmente, las intenciones están relacionadas con las acciones que la aplicación cliente debe realizar y las entidades son la información necesaria para realizar dicha acción. Desde la perspectiva de la programación, una intención desencadenaría una llamada al método y las entidades se utilizarían como parámetros para esa llamada al método.

Esta expresión _debe_ tener una intención y _puede_ incluir entidades:

`Buy an airline ticket from Seattle to Cairo`

Esta expresión tiene una única intención:

* Comprar un boleto de avión

Esta expresión _puede_ tener varias entidades:

* Ubicaciones de Seattle (origen) y Cairo (destino);
* La cantidad de un boleto.

## <a name="descriptors-are-features"></a>Los descriptores son características

Un descriptor es una característica aplicada a un modelo al momento de entrenarlo, incluidas las listas de frases y las entidades. 

**Use un descriptor cuando quiera**:

* aumentar la importancia de las palabras y frases identificadas por el descriptor;
* hacer que LUIS recomiende el texto o frases nuevos para el descriptor;
* corregir un error en los datos de entrenamiento.

## <a name="next-steps"></a>Pasos siguientes

* Obtenga más información sobre [intenciones](luis-concept-intent.md) y [entidades](luis-concept-entity-types.md). 