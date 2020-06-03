---
title: 'Características: LUIS'
description: Agregue características a un modelo de lenguaje para proporcionar sugerencias sobre cómo reconocer la entrada que quiera etiquetar o clasificar.
ms.topic: conceptual
ms.date: 05/14/2020
ms.openlocfilehash: c4f19ceed2e48f3f6ec2ed0958bccb7a85cff44f
ms.sourcegitcommit: 493b27fbfd7917c3823a1e4c313d07331d1b732f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/21/2020
ms.locfileid: "83742709"
---
# <a name="machine-learning-ml-features"></a>Características de Machine Learning (ML)

En el aprendizaje automático, una  **característica**  es un rasgo distintivo o un atributo de datos que el sistema observa.

Las características del aprendizaje automático proporcionan a LUIS indicaciones importantes sobre dónde buscar los elementos que distinguirán un concepto. Son sugerencias que LUIS puede usar, pero no reglas rígidas.  Estas sugerencias se usan en combinación con las etiquetas para buscar los datos.

 LUIS es compatible tanto con las listas de frases como con el uso de otras entidades como características:
* Característica de lista de frases
* Modelo (intención o entidad) como una característica

Las características deben considerarse como una parte necesaria del diseño del esquema.

## <a name="a-phrase-list-for-a-particular-concept"></a>Lista de frases para un concepto determinado

Una lista de frases es una lista de palabras o frases que encapsula un concepto determinado.

Al agregar una lista de frases, puede establecer la característica como:
* **[Global](#global-features)** . Una característica global se aplica a toda la aplicación.

### <a name="when-to-use-a-phrase-list"></a>Cuándo usar una lista de frases

Si es necesario que la aplicación LUIS sea capaz de generalizar e identificar los elementos nuevos del concepto, use una lista de frases. Las listas de frases son similares al vocabulario específico de dominio que ayuda a mejorar la calidad de comprensión de las intenciones y entidades.

### <a name="how-to-use-a-phrase-list"></a>Cómo utilizar una lista de frases

Con una lista de frases, LUIS considera el contexto y lo generaliza para identificar los elementos que son similares, pero que no son una coincidencia de texto exacta.

Pasos para utilizar una lista de frases:
* Empezar con una entidad de aprendizaje automático
    * Incorporación de expresiones de ejemplo
    * Etiquetado con una entidad de aprendizaje automático
* Agregar una lista de frases
    * Agregue palabras con significado similar: **no** agregue todas las palabras o frases posibles. En su lugar, agregue algunas palabras o frases a la vez, y vuelva a entrenar y a publicar.
    * Revise y agregue palabras sugeridas.

### <a name="a-typical-scenario-for-a-phrase-list"></a>Escenario típico para una lista de frases

Un escenario típico para una lista de frases es potenciar las palabras relacionadas con una idea concreta.

Un ejemplo de palabras que pueden necesitar una lista de frases para aumentar su importancia son los términos médicos. Los términos pueden tener un significado específico físico, químico, terapéutico o abstracto. LUIS no sabrá que los términos son importantes para el dominio del tema sin una lista de frases.

Si desea extraer términos médicos:
* En primer lugar, cree expresiones de ejemplo y etiquete los términos médicos dentro de esas expresiones.
* A continuación, cree una lista de frases con ejemplos de los términos dentro del dominio del tema. En esta lista de frases se debe incluir el término real que etiquetó y otros términos que describen el mismo concepto.
* Agregue la lista de frases a la entidad o subentidad que extrae el concepto utilizado en la lista de frases. El escenario más común es un componente (secundario) de una entidad de aprendizaje automático. Si la lista de frases debe aplicarse a todos las intenciones o entidades, marque la lista de frases como lista de frases global. La marca `enabledForAllModels` controla este ámbito del modelo en la API.

<a name="how-to-use-phrase-lists"></a>
<a name="how-to-use-a-phrase-lists"></a>
<a name="phrase-lists-help-identify-simple-exchangeable-entities"></a>

## <a name="a-model-as-a-feature-helps-another-model"></a>Modelo como una característica que ayuda a otro modelo

Puede agregar un modelo (intención o entidad) como una característica para otro modelo (intención o entidad). Al agregar una intención o entidad existente como una característica, agrega un concepto bien definido con ejemplos con etiquetas.

Al agregar un modelo como una característica, puede establecer la característica como:
* **[Obligatoria](#required-features)** . Se deben encontrar las características que son obligatorias para que se devuelva el modelo desde el punto de conexión de predicción.
* **[Global](#global-features)** . Una característica global se aplica a toda la aplicación.

### <a name="when-to-use-an-entity-as-a-feature-to-an-intent"></a>Cuándo usar una entidad como una característica para una intención

Agregue una entidad como una característica a una intención cuando la detección de esa entidad sea significativa para la intención.

Por ejemplo, si la intención es reservar un vuelo, `BookFlight`, y la entidad es la información sobre el billete (como el número de asiento, el origen y el destino), la búsqueda de la entidad de información del billete debe agregar un peso significativo a la predicción de la intención `BookFlight`.

### <a name="when-to-use-an-entity-as-a-feature-to-another-entity"></a>Cuándo usar una entidad como una característica para otra entidad

Una entidad (A) debe agregarse como una característica a otra entidad (B), cuando la detección de esa entidad (A) es importante para la predicción de la entidad (B).

Por ejemplo, si la entidad de dirección de envío n contiene una subentidad de dirección postal, la búsqueda de la subentidad de dirección postal agrega peso significativo a la predicción para la entidad de dirección de envío.

* Dirección de envío (entidad con aprendizaje automático)
    * Número de la calle (subentidad)
    * Dirección postal (subentidad)
    * Ciudad (subentidad)
    * Estado o provincia (subentidad)
    * País o región (subentidad)
    * Código postal (subentidad)

## <a name="nested-subentities-with-features"></a>Subentidades anidadas con características

Una subentidad con aprendizaje automático indica que hay un concepto en la entidad primaria, tanto si ese elemento primario es otra subentidad o la entidad superior. El valor de la subentidad actúa como una característica de su elemento primario.

Una subentidad puede tener tanto una lista de frases como una característica, así como un modelo (otra entidad) como una característica.

Cuando la subentidad tiene una lista de frases, aumentará el vocabulario del concepto, pero no agregará información a la respuesta JSON de la predicción.

Cuando la subentidad tiene una característica de otra entidad, la respuesta JSON incluye los datos extraídos de esa otra entidad.

## <a name="required-features"></a>Características obligatorias

Se deben encontrar las características que son obligatorias para que se devuelva el modelo desde el punto de conexión de predicción. Use una característica obligatoria si sabe que los datos entrantes deben coincidir con la característica.

Si el texto de la expresión no coincide con la característica requerida, no se extraerá.

**Una característica obligatoria utiliza una entidad sin aprendizaje automático**:
* Entidad de expresión regular
* Entidad de lista
* Entidad creada previamente

¿Qué características son buenas para marcarlas como obligatorias? Si está seguro de que se encontrará el modelo en los datos, establezca la característica como obligatoria. Una característica obligatoria no devuelve nada, si no se encuentra.

Continuando con el ejemplo de la dirección de envío:
* Dirección de envío (entidad con aprendizaje automático)
    * Número de la calle (subentidad)
    * Dirección postal (subentidad)
    * Nombre de la calle (subentidad)
    * Ciudad (subentidad)
    * Estado o provincia (subentidad)
    * País o región (subentidad)
    * Código postal (subentidad)

### <a name="required-feature-using-prebuilt-entities"></a>Característica obligatoria con entidades precompiladas

La ciudad, el estado y el país o región suelen ser un conjunto cerrado de listas, lo que significa que no cambian mucho con el tiempo. Estas entidades podrían tener las características recomendadas pertinentes y estas características podrían marcarse como obligatorias. Significa que no se devuelve la dirección de envío completa si no se encuentran las entidades con las características obligatorias.

¿Qué ocurre si la ciudad, el estado o el país se encuentran en la expresión, pero en una ubicación o una jerga que LUIS no espera? Si desea proporcionar algún procesamiento posterior para ayudar a resolver la entidad, debido a una puntuación de confianza baja de LUIS, no marque la característica como obligatoria.

Otro ejemplo de una característica obligatoria para la dirección de envío es hacer que el número de la calle sea un número [precompilado](luis-reference-prebuilt-entities.md) obligatorio. Esto permite que el usuario pueda escribir "1 Microsoft Way" o "One Microsoft Way". Ambos se resolverán en el número "1" para la subentidad de número de la calle.

### <a name="required-feature-using-list-entities"></a>Característica obligatoria con entidades de lista

Una [entidad de lista](reference-entity-list.md) se utiliza como una lista de nombres canónicos junto con sus sinónimos. Como característica obligatoria, si la expresión no incluye el nombre canónico o un sinónimo, la entidad no se devuelve como parte del punto de conexión de predicción.

Siguiendo con el ejemplo de la dirección de envío, supongamos que su empresa solo realiza envíos a un conjunto limitado de países o regiones. Puede crear una entidad de lista que incluya varias maneras en las que el cliente puede hacer referencia al país. Si LUIS no encuentra una coincidencia exacta en el texto de la expresión, la entidad (que tiene la característica obligatoria de la entidad de lista) no se devuelve en la predicción.

|Nombre canónico|Sinónimos|
|--|--|
|Estados Unidos|EE. UU.<br>EE. UU.<br>US<br>EE. UU.<br>0|

La aplicación cliente, como un bot de chat, puede formular una pregunta de seguimiento, para que el cliente entienda que la selección del país o región está limitada y es _obligatoria_.

### <a name="required-feature-using-regular-expression-entities"></a>Característica obligatoria con entidades de expresión regular

Una [entidad de expresión regular](reference-entity-regular-expression.md) utilizada como característica obligatoria proporciona funciones de coincidencia de texto enriquecidas.

Continuando con la dirección de envío, puede crear una expresión regular que capture las reglas de sintaxis de los códigos postales de los países o regiones.

## <a name="global-features"></a>Características globales

Aunque el uso más común es aplicar una característica a un modelo específico, puede configurar la característica como **característica global** para aplicarla a toda la aplicación.

El uso más común de una característica global es agregar un vocabulario adicional, como palabras de otro idioma, a la aplicación. Si los clientes usan un idioma principal, pero se espera poder usar otro idioma en la misma expresión, puede agregar una característica que incluya palabras del idioma secundario.

Como el usuario esperaba usar el segundo idioma en cualquier intención o entidad, debe agregarse en una lista de frases que esté configurada como una característica global.

## <a name="best-practices"></a>Procedimientos recomendados
Obtenga información sobre los [procedimientos recomendados](luis-concept-best-practices.md).

## <a name="next-steps"></a>Pasos siguientes

* [Extienda](schema-change-prediction-runtime.md) los modelos de aplicación en tiempo de ejecución de predicción.
* Vea [Agregar características](luis-how-to-add-features.md) para obtener más información sobre cómo agregar características a la aplicación de LUIS.
