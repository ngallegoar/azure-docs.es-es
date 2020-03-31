---
title: 'Tipos de entidades: LUIS'
titleSuffix: Azure Cognitive Services
description: 'Las entidades extraen datos de la expresión. Los tipos de entidad realizan una extracción predecible de datos. Hay dos tipos de entidades: con aprendizaje automático y sin aprendizaje automático. Es importante saber qué tipo de entidad se usa en las expresiones.'
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 11/12/2019
ms.author: diberry
ms.openlocfilehash: 6ee156efb5512c92d86ba05513b6a2b91df4eae8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79219182"
---
# <a name="entities-and-their-purpose-in-luis"></a>Entidades y su finalidad en LUIS

El objetivo principal de las entidades es proporcionar a la aplicación cliente la extracción predecible de los datos. Un propósito _opcional_ y secundario es impulsar la predicción de la intención o de otras entidades con los descriptores.

Existen dos tipos de entidades:

* Con aprendizaje automático: desde el contexto
* Sin aprendizaje automático: para buscar coincidencias de texto exactas, coincidencias de patrones o detecciones por entidades pregeneradas

Las entidades con aprendizaje automático proporcionan la gama más amplia de opciones de extracción de datos. Las entidades sin aprendizaje automático funcionan por coincidencia de texto y se pueden usar de forma independiente o como [restricción](#design-entities-for-decomposition) en una entidad con aprendizaje automático.

## <a name="entities-represent-data"></a>Las entidades representan datos

Las entidades son los datos que desea extraer de la expresión, como nombres, fechas, nombres de producto o cualquier grupo de palabras significativo. Una expresión puede incluir varias o ninguna entidad. _Es posible_ que una aplicación cliente necesite los datos para realizar su tarea.

Las entidades deben etiquetarse de forma coherente en todas las expresiones de entrenamiento para cada intención en un modelo.

 Puede definir sus propias entidades o usar entidades pregeneradas para ahorrar tiempo para conceptos comunes como [datetimeV2](luis-reference-prebuilt-datetimev2.md), [ordinal](luis-reference-prebuilt-ordinal.md), [correo electrónico](luis-reference-prebuilt-email.md) y [número de teléfono](luis-reference-prebuilt-phonenumber.md).

|Expresión|Entidad|data|
|--|--|--|
|Comprar 3 billetes a Nueva York|Número creado previamente<br>Location.Destination|3<br>Nueva York|
|Comprar un billete de Nueva York a Londres para el 5 de marzo|Location.Origin<br>Location.Destination<br>Entidad datetimeV2 creada previamente|Nueva York<br>London<br>5 de marzo de 2018|

### <a name="entities-are-optional"></a>Las entidades son opcionales.

Mientras las intenciones son obligatorias, las entidades son opcionales. No es necesario crear entidades para cada concepto en la aplicación, sino solo para los que resulten necesarios para que la aplicación cliente pueda llevar a cabo la operación.

Si las expresiones no tienen los datos que la aplicación cliente requiere, no es necesario agregar entidades. A medida que la aplicación se desarrolla y se identifica una nueva necesidad de datos, podrá agregar las entidades adecuadas al modelo LUIS más adelante.

## <a name="entity-compared-to-intent"></a>Comparación entre entidades e intenciones

La entidad representa un concepto de datos dentro de la expresión que quiere extraer.

Opcionalmente, una expresión puede incluir entidades. En comparación, se _requiere_ la predicción de la intención para una expresión y representa toda la expresión. LUIS requiere que las expresiones de ejemplo estén contenidas en una intención.

Considere las cuatro expresiones siguientes:

|Expresión|Intención pronosticada|Entidades extraídas|Explicación|
|--|--|--|--|
|Ayuda|help|-|No hay nada que extraer.|
|Send something|sendSomething|-|No hay nada que extraer. El modelo no se ha entrenado para extraer `something` en este contexto y no hay ningún destinatario.|
|Send Bob a present|sendSomething|`Bob`, `present`|El modelo se ha entrenado con la entidad pregenerada [personName](luis-reference-prebuilt-person.md), que ha extraído el nombre `Bob`. Se ha usado una entidad con aprendizaje automático para extraer `present`.|
|Send Bob a box of chocolates|sendSomething|`Bob`, `box of chocolates`|Las entidades han extraído los dos fragmentos importantes de datos, `Bob` y el `box of chocolates`.|

## <a name="design-entities-for-decomposition"></a>Diseño de entidades para la descomposición

Es un buen diseño de entidad para hacer de la entidad de nivel superior una entidad con aprendizaje automático. Esto permite realizar cambios en el diseño de la entidad a lo largo del tiempo y el uso de **subcomponentes** (entidades secundarias), opcionalmente con **restricciones** y **descriptores**, para descomponer la entidad de nivel superior en los elementos necesarios para la aplicación cliente.

El diseño de la descomposición permite a LUIS devolver un grado profundo de resolución de la entidad a la aplicación cliente. Esto permite a la aplicación cliente centrarse en reglas de negocios y dejar a LUIS la resolución de datos.

### <a name="machine-learned-entities-are-primary-data-collections"></a>Las entidades con aprendizaje automático son colecciones de datos principales.

Las [**entidades con aprendizaje automático**](tutorial-machine-learned-entity.md) constituyen la unidad de datos de nivel superior. Los subcomponentes son entidades secundarias de las entidades con aprendizaje automático.

Un desencadenador de entidades con aprendizaje automático basado en el contexto aprendido a través de expresiones de entrenamiento. Las **restricciones** son reglas opcionales que se aplican a una entidad con aprendizaje automático que restringe aún más la activación en función de la definición de coincidencia exacta de texto de una entidad sin aprendizaje automático, como una [lista](reference-entity-list.md) o [regex](reference-entity-regular-expression.md). Por ejemplo, la entidad con aprendizaje automático `size` puede tener una restricción de una entidad de lista `sizeList` que restringe la entidad `size` para que se desencadene solo cuando se encuentren los valores contenidos en la entidad `sizeList`.

Los [**descriptores**](luis-concept-feature.md) son características aplicadas para aumentar la relevancia de las palabras o frases de la predicción. Se denominan *descriptores* porque se usan para *describir* una intención o una entidad. Los descriptores explican rasgos o atributos de datos diferenciales, como palabras o frases importantes que LUIS observa y que le permiten aprender.

Cuando se crea una característica de lista de frases en la aplicación de LUIS, se habilita globalmente de forma predeterminada y se aplica uniformemente en todas las intenciones y entidades. Sin embargo, si aplica la lista de frases como un descriptor (característica) de una entidad con aprendizaje automático (o *modelo*), su ámbito se reduce para aplicarse solo a ese modelo y ya no se usa con los demás modelos. El uso de una lista de frases como descriptor de un modelo ayuda a la descomposición al ayudar con la precisión del modelo al que se aplica.

<a name="composite-entity"></a>
<a name="list-entity"></a>
<a name="patternany-entity"></a>
<a name="prebuilt-entity"></a>
<a name="regular-expression-entity"></a>
<a name="simple-entity"></a>

## <a name="types-of-entities"></a>Tipos de entidades

Elija la entidad en función de cómo se deben extraer los datos y cómo se deben representar una vez que se extraen.

|Tipo de entidad|Propósito|
|--|--|
|[**Con aprendizaje automático**](tutorial-machine-learned-entity.md)|Las entidades con aprendizaje automático aprenden del contexto de la expresión. Agrupación primaria de entidades, independientemente del tipo de entidad. Esto hace que la variación de la colocación de las expresiones en el ejemplo sea importante. |
|[**Lista**](reference-entity-list.md)|Lista de elementos y sus sinónimos extraída con **coincidencia de texto exacta**.|
|[**Pattern.any**](reference-entity-pattern-any.md)|Entidad cuyo final es difícil de determinar. |
|[**Creada previamente**](luis-reference-prebuilt-entities.md)|Ya entrenado para extraer un tipo específico de datos, como la dirección URL o el correo electrónico. Algunas de estas entidades precompiladas se definen en el proyecto de código abierto [Recognizers-Text](https://github.com/Microsoft/Recognizers-Text). Si su referencia cultural o entidad específica no se admite actualmente, colabore en el proyecto.|
|[**Expresión regular**](reference-entity-regular-expression.md)|Usa una expresión regular para la **coincidencia de texto exacta**.|

## <a name="extracting-contextually-related-data"></a>Extracción de datos relacionados contextualmente

Una expresión puede contener dos o más apariciones de una entidad en la que el significado de los datos se basa en el contexto dentro de la expresión. Un ejemplo es una expresión para reservar un vuelo que tiene dos ubicaciones, origen y destino.

`Book a flight from Seattle to Cairo`

Deben extraerse los dos ejemplos de una entidad `location`. La aplicación cliente tiene que conocer el tipo de ubicación de cada una de ellas para completar la compra de entradas.

Hay dos técnicas para extraer los datos relacionados con el contexto:

 * La entidad `location` es una entidad con aprendizaje automático y utiliza dos entidades de subcomponentes para capturar `origin` y `destination` (preferido)
 * La entidad `location` usa dos **roles** de `origin` y `destination`.

Pueden existir varias entidades en una expresión y se pueden extraer sin usar la descomposición o los roles si el contexto en el que se usan no tiene importancia. Por ejemplo, si la expresión incluye una lista de ubicaciones, `I want to travel to Seattle, Cairo, and London.`, esta es una lista donde cada elemento no tiene un significado adicional.

### <a name="using-subcomponent-entities-of-a-machine-learned-entity-to-define-context"></a>Uso entidades de subcomponentes de una entidad con aprendizaje automático para definir el contexto

Puede usar una [**entidad con aprendizaje automático**](tutorial-machine-learned-entity.md) para extraer los datos que describen la acción de reservar un vuelo y, a continuación, descomponer la entidad de nivel superior en las partes independientes necesarias para la aplicación cliente.

En este ejemplo, `Book a flight from Seattle to Cairo`, la entidad de nivel superior podría ser `travelAction` y se etiqueta para extraer `flight from Seattle to Cairo`. A continuación, se crean dos entidades de subcomponentes, llamadas `origin` y `destination`, ambas con una restricción aplicada de la entidad `geographyV2` pregenerada. En las expresiones de entrenamiento, `origin` y `destination` se etiquetan adecuadamente.

### <a name="using-entity-role-to-define-context"></a>Uso del rol de entidad para definir el contexto

El rol es un alias con nombre para la entidad basado en el contexto dentro de la expresión. Un rol se puede usar con cualquier tipo de entidad precompilada o personalizada, y tanto en patrones como en expresiones de ejemplo. En este ejemplo, la entidad `location` necesita dos roles de `origin` y `destination`, y ambos deben marcarse en las expresiones de ejemplo.

Si LUIS encuentra `location` pero no puede determinar el rol, se seguirá devolviendo la entidad de ubicación. La aplicación cliente tendría que hacer un seguimiento con una pregunta para determinar qué tipo de ubicación quería decir el usuario.


## <a name="if-you-need-more-than-the-maximum-number-of-entities"></a>Si necesita más del número máximo de entidades

Si necesita más que el límite, póngase en contacto con el soporte técnico. Para ello, recopile información detallada sobre el sistema, vaya al sitio web de [LUIS](luis-reference-regions.md#luis-website) y seleccione **Support** (Soporte). Si la suscripción a Azure incluye servicios de soporte técnico, póngase en contacto con el [soporte técnico de Azure](https://azure.microsoft.com/support/options/).

## <a name="entity-prediction-status"></a>Estado de predicción de entidades

El portal de LUIS muestra cuando la entidad, en una expresión de ejemplo, tiene una predicción de entidad diferente a la que se ha seleccionado. Esta puntuación diferente se basa en el modelo entrenado actual.

## <a name="next-steps"></a>Pasos siguientes

Aprenda conceptos sobre las buenas [expresiones](luis-concept-utterance.md).

Vea [Add entities](luis-how-to-add-entities.md) (Agregar entidades) para obtener más información sobre cómo agregar entidades a la aplicación de LUIS.

Consulte [Tutorial: Extraiga los datos estructurados de la expresión del usuario con entidades con aprendizaje automático en Language Understanding (LUIS)](tutorial-machine-learned-entity.md) para obtener información sobre cómo extraer datos estructurados de una expresión mediante la entidad con aprendizaje automático.
 
