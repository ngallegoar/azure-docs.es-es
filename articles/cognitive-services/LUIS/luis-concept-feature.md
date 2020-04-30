---
title: 'Características: LUIS'
titleSuffix: Azure Cognitive Services
description: Agregue características a un modelo de lenguaje para proporcionar sugerencias sobre cómo reconocer la entrada que quiera etiquetar o clasificar.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 11/03/2019
ms.author: diberry
ms.openlocfilehash: 5b8257e24cf52d01be8065d97db17fd685aa316d
ms.sourcegitcommit: 31ef5e4d21aa889756fa72b857ca173db727f2c3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/16/2020
ms.locfileid: "81531905"
---
# <a name="machine-learned-features"></a>Características con aprendizaje automático

En el aprendizaje automático, una _característica_ es un rasgo distintivo o un atributo de datos que el sistema observa y aprende. En Language Understanding (LUIS), una característica describe y explica lo que es importante sobre las intenciones y entidades.

En el [portal en versión preliminar de LUIS](https://preview.luis.ai), las características son _descriptores_ porque se usan para _describir_ la intención o la entidad.

## <a name="features-_descriptors_-in-language-understanding"></a>Características (_descriptores_) de Language Understanding

Las características, también conocidas como descriptores, describen pistas que ayudan a Language Understanding a identificar las expresiones de ejemplo. Características incluidas:

* Lista de frases como una característica para las intenciones o entidades
* Entidades como características para intenciones o entidades

Las características deben considerarse como una parte necesaria del esquema para la descomposición del modelo.

## <a name="what-is-a-phrase-list"></a>¿Qué es una lista de frases?

Una lista de frases es una lista de palabras, frases, números u otros caracteres que ayudan a identificar el concepto que está intentando identificar. La lista distingue mayúsculas de minúsculas.

## <a name="when-to-use-a-phrase-list"></a>Cuándo usar una lista de frases

Con una lista de frases, LUIS considera el contexto y lo generaliza para identificar los elementos que son similares, pero que no son una coincidencia de texto exacta. Si es necesario que la aplicación de LUIS sea capaz de generalizar e identificar los elementos nuevos, use una lista de frases.

Si quiere poder reconocer nuevas instancias, como un programador de reuniones que deba reconocer los nombres de los contactos nuevos, o bien una aplicación de inventario que deba reconocer los productos nuevos, comience con una entidad con aprendizaje automático. A continuación, cree una lista de frases que ayude a LUIS a encontrar palabras con un significado similar. Esta lista de frases sirve de guía a LUIS para reconocer los ejemplos mediante la adición de más importancia al valor de esas palabras.

Las listas de frases son similares al vocabulario específico de dominio que ayuda a mejorar la calidad de comprensión de las intenciones y entidades.

## <a name="considerations-when-using-a-phrase-list"></a>Consideraciones al usar una lista de frases

De forma predeterminada, se aplica una lista de frases a todos los modelos de la aplicación. Esto funcionará en las listas de frases que pueden cruzar todas las intenciones y entidades. Para la capacidad de descomposición, debería aplicar una lista de frases solo a los modelos para los que es relevante.

Si crea una lista de frases (que se crea globalmente de forma predeterminada), después la aplica como descriptor (característica) a un modelo específico y la quita de otros modelos. Esta eliminación agrega relevancia a la lista de frases para el modelo al que se aplica, lo que ayuda a mejorar la precisión que proporciona en el modelo.

La marca `enabledForAllModels` controla este ámbito del modelo en la API.

<a name="how-to-use-phrase-lists"></a>

### <a name="how-to-use-a-phrase-list"></a>Cómo utilizar una lista de frases

[Cree una lista de frases](luis-how-to-add-features.md) cuando la intención o entidad tenga palabras o frases que son importantes, como:

* términos del sector
* jerga
* abreviaturas
* lenguaje específico de la empresa
* lenguaje que pertenece a otro idioma, pero que se utiliza con frecuencia en la aplicación
* palabras y frases clave en las expresiones de ejemplo

**No** agregue todas las palabras o frases posibles. En su lugar, agregue algunas palabras o frases a la vez, y vuelva a entrenar y a publicar. Conforme vaya creciendo la lista, es posible que algunos de los términos presenten varias formas (sinónimos). Divídala en otra lista.

<a name="phrase-lists-help-identify-simple-exchangeable-entities"></a>

## <a name="when-to-use-an-entity-as-a-feature"></a>Cuándo usar una entidad como una característica

Una entidad se puede agregar como una característica al nivel de la intención o de entidad.

### <a name="entity-as-a-feature-to-an-intent"></a>Entidad como característica para una intención

Agregue una entidad como un descriptor (característica) a una intención cuando la detección de esa entidad sea significativa para la intención.

Por ejemplo, si la intención es reservar un vuelo y la entidad es la información sobre los billetes (como el número de asiento, el origen y el destino), la búsqueda de la entidad de información de los billetes debe agregar peso a la predicción de la intención de la reserva del vuelo.

### <a name="entity-as-a-feature-to-another-entity"></a>Entidad como característica de otra entidad

Una entidad (A) debe agregarse como una característica a otra entidad (B), cuando la detección de esa entidad (A) es importante para la predicción de la entidad (B).

Por ejemplo, si se detecta la entidad de dirección postal (A), la búsqueda de la dirección postal (A) agrega el peso a la predicción para la entidad de dirección de envío (B).

## <a name="best-practices"></a>Procedimientos recomendados
Obtenga información sobre los [procedimientos recomendados](luis-concept-best-practices.md).

## <a name="next-steps"></a>Pasos siguientes

* [Extienda](schema-change-prediction-runtime.md) los modelos de aplicación en tiempo de ejecución de predicción.
* Vea [Agregar características](luis-how-to-add-features.md) para obtener más información sobre cómo agregar características a la aplicación de LUIS.
