---
title: 'Diseño con modelos: LUIS'
description: Language Understanding proporciona varios tipos de modelos. Algunos modelos se puede usar de varias formas.
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 04/30/2020
ms.openlocfilehash: 576ba945018d13db9cd24888f3c41a2215857694
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "91316518"
---
# <a name="design-with-intent-and-entity-models"></a>Diseñe con intenciones y modelos de entidad

Language Understanding proporciona dos tipos de modelos para definir el esquema de la aplicación. El esquema de la aplicación determina la información que se recibe de la predicción de la expresión de un nuevo usuario.

El esquema de la aplicación se construye a partir de modelos creados mediante la [enseñanza automática](#authoring-uses-machine-teaching):
* Las [intenciones](#intents-classify-utterances) clasifican expresiones del usuario.
* Las [entidades](#entities-extract-data) extraen datos de la expresión.

## <a name="authoring-uses-machine-teaching"></a>La creación usa la enseñanza automática

La metodología de enseñanza automática de LUIS le permite enseñar conceptos a una máquina con facilidad. No es necesario comprender el _aprendizaje automático_ para usar LUIS. En su lugar, usted, como profesor, comunica un concepto a LUIS proporcionándole ejemplos del concepto y explicándole cómo se debe modelar mediante otros conceptos relacionados. Como profesor también puede mejorar el modelo de LUIS de forma interactiva. Para ello, se deben identificar y corregir los errores de predicción.

<a name="v3-authoring-model-decomposition"></a>

## <a name="intents-classify-utterances"></a>Las intenciones clasifican expresiones

Una intención clasifica expresiones de ejemplo para enseñar a LUIS sobre dicha intención. Los expresiones de ejemplo de una intención se usan como ejemplos positivos de la expresión. Estas mismas expresiones se usan como ejemplos negativos en todas las demás intenciones.

Considere una aplicación que necesita determinar la intención de un usuario de ordenar un libro. Además, una aplicación necesita la dirección de envío del cliente. Esta aplicación tiene dos intenciones: `OrderBook` y `ShippingLocation`.

La siguiente expresión es un **ejemplo positivo** para la intención `OrderBook` y un **ejemplo negativo** para la intención `ShippingLocation` y `None`:

`Buy the top-rated book on bot architecture.`

## <a name="entities-extract-data"></a>Las entidades extraen datos

Una entidad representa una unidad de datos que quiere extraer de la expresión. Una entidad de aprendizaje automático es una entidad de nivel superior que contiene subentidades, las cuales también son entidades de aprendizaje automático.

Un ejemplo de una entidad de aprendizaje automático es un pedido de billete de avión. En teoría, se trata de una única transacción con muchas unidades de datos más pequeñas, como la fecha, la hora, la cantidad de puestos, el tipo de asiento (como primera clase o clase económica), la ubicación de origen, la ubicación de destino y la elección de comida.

## <a name="intents-versus-entities"></a>Intenciones frente a entidades

Una intención es el resultado deseado de una expresión _completa_, mientras que las entidades son fragmentos de datos extraídos de la expresión. Normalmente, las intenciones están relacionadas con acciones que la aplicación cliente debe realizar. Las entidades son la información necesaria para realizar esta acción. Desde la perspectiva de la programación, una intención desencadenaría una llamada al método y las entidades se utilizarían como parámetros para esa llamada al método.

Esta expresión _debe_ tener una intención y _puede_ incluir entidades:

`Buy an airline ticket from Seattle to Cairo`

Esta expresión tiene una única intención:

* Comprar un boleto de avión

Esta expresión _puede_ tener varias entidades:

* Ubicaciones de Seattle (origen) y Cairo (destino);
* La cantidad de un boleto.

## <a name="entity-model-decomposition"></a>Descomposición del modelo de entidad

LUIS admite la _descomposición del modelo_ con las API de creación, mediante la división del modelo en partes más pequeñas. Esto le permite compilar los modelos con confianza en la construcción y predicción de las distintas partes.

La descomposición del modelo tiene las siguientes partes:

* [intenciones](#intents-classify-utterances);
    * [features](#features)
* [entidades de aprendizaje automático](reference-entity-machine-learned-entity.md)
    * subentidades (también entidades de aprendizaje automático)
        * [features](#features)
            * [lista de frases](luis-concept-feature.md)
            * [entidades sin aprendizaje automático](luis-concept-feature.md) como [expresiones regulares](reference-entity-regular-expression.md), [listas](reference-entity-list.md) y [entidades pregeneradas](luis-reference-prebuilt-entities.md)

<a name="entities-extract-data"></a>
<a name="machine-learned-entities"></a>

## <a name="features"></a>Características

Una [característica](luis-concept-feature.md) es un rasgo distintivo o un atributo de datos que el sistema observa. Las características del aprendizaje automático proporcionan a LUIS indicaciones importantes sobre dónde buscar los elementos que distinguirán un concepto. Son sugerencias que LUIS puede usar, pero no reglas rígidas. Estas sugerencias se usan en combinación con las etiquetas para buscar los datos.

## <a name="patterns"></a>Patrones

Los [patrones](luis-concept-patterns.md) están diseñados para mejorar la precisión cuando varias expresiones son muy parecidas. Un patrón permite lograr más precisión en una intención sin proporcionar muchas más expresiones.

## <a name="extending-the-app-at-runtime"></a>Extensión de la aplicación en runtime

El esquema de la aplicación (modelos y características) se entrena y publica en el punto de conexión de predicción. Puede [pasar información nueva](schema-change-prediction-runtime.md), junto con la expresión del usuario, al punto de conexión de predicción para aumentar la predicción.

## <a name="next-steps"></a>Pasos siguientes

* Obtenga más información sobre [intenciones](luis-concept-intent.md) y [entidades](luis-concept-entity-types.md).
* Más información sobre las [características](luis-concept-feature.md)