---
title: Migrar a una entidad de aprendizaje automático de V3
titleSuffix: Azure Cognitive Services
description: La creación de V3 proporciona un nuevo tipo de entidad, la entidad de aprendizaje automático, junto con la capacidad de agregar relaciones a la entidad de aprendizaje automático y a otras entidades o características de la aplicación.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 12/30/2019
ms.author: diberry
ms.openlocfilehash: b5dbcd9033d9a41e43ea907d043e0c0486b236db
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "75563824"
---
# <a name="migrate-to-v3-authoring-entity"></a>Migración a la creación de V3

La creación de V3 proporciona un nuevo tipo de entidad, la entidad de aprendizaje automático, junto con la capacidad de agregar relaciones a la entidad de aprendizaje automático y a otras entidades o características de la aplicación.

## <a name="entities-are-decomposable-in-v3"></a>Las entidades se pueden descomponer en V3

Las entidades creadas con las API de creación de V3, ya sea mediante las [API](https://westeurope.dev.cognitive.microsoft.com/docs/services/luis-programmatic-apis-v3-0-preview) o con el [portal de vista previa](https://preview.luis.ai/), le permiten crear un modelo de entidad con capas con un elemento principal y elementos secundarios. Al elemento principal se le conoce como **entidad de aprendizaje automático** y a los elementos secundarios se les conoce como **subcomponentes** de la entidad de aprendizaje automático.

Cada subcomponente es también una entidad de aprendizaje automático, pero con las opciones de configuración agregadas de restricciones y descriptores.

* Las **restricciones** son reglas exactas de coincidencia de texto que garantizan que una entidad se extrae cuando coincide con una regla. La regla se define mediante una entidad de coincidencia de texto exacta, actualmente: una [entidad generada previamente](luis-reference-prebuilt-entities.md), una [entidad de expresión regular](reference-entity-regular-expression.md) o una [entidad de lista](reference-entity-list.md).
* Los **descriptores** son [características](luis-concept-feature.md), como listas de frases o entidades, que se usan para indicar la entidad de forma segura.

La creación de V3 proporciona un nuevo tipo de entidad, la entidad de aprendizaje automático, junto con la capacidad de agregar relaciones a la entidad de aprendizaje automático y a otras entidades o características de la aplicación.

## <a name="how-do-these-new-relationships-compare-to-v2-authoring"></a>Comparación de las nuevas relaciones con la creación de V2

La creación de V2 proporciona entidades jerárquicas y compuestas junto con roles y características para realizar esta misma tarea. Dado que las entidades, las características y los roles no se relacionaban explícitamente entre sí, era difícil entender cómo LUIS deducía las relaciones durante la predicción.

Con V3, la relación es explícita y está diseñada por los autores de la aplicación. Esto le permite, como autor de la aplicación:

* Ver visualmente cómo LUIS está prediciendo estas relaciones en las expresiones de ejemplo
* Probar estas relaciones con el [panel de prueba interactiva](luis-interactive-test.md) o en el punto de conexión
* Usar estas relaciones en la aplicación cliente, a través de un [objeto .json](reference-entity-machine-learned-entity.md) anidado bien estructurado y con nombre

## <a name="planning"></a>Planificación

Cuando migre, tenga en cuenta lo siguiente en el plan de migración:

* Realice una copia de seguridad de la aplicación de LUIS y realice la migración en una aplicación independiente. Tener una aplicación de V2 y V3 disponible al mismo tiempo le permite validar los cambios necesarios y el impacto en los resultados de la predicción.
* Capture las métricas de éxito de predicción actuales
* Capture la información del panel actual como una instantánea del estado de la aplicación
* Revise las intenciones, entidades, listas de frases, patrones y pruebas por lotes existentes
* Los siguientes elementos se pueden migrar **sin necesidad de cambios**:
    * Intenciones
    * Entidades
        * Entidad de expresión regular
        * Entidad de lista
    * Características
        * Lista de frases
* Los siguientes elementos se deben migrar **con cambios**:
    * Entidades
        * Entidad jerárquica
        * Entidad compuesta
    * Roles: los roles solo se pueden aplicar a una entidad de aprendizaje automático (principal). Los roles no se pueden aplicar a subcomponentes
    * Pruebas por lotes y patrones que usan las entidades jerárquicas y compuestas

Al diseñar el plan de migración, deje tiempo para revisar las entidades finales de aprendizaje automático, después de que se hayan migrado todas las entidades jerárquicas y compuestas. Aunque una migración directa funcionará, después de realizar el cambio y revisar los resultados de las pruebas por lotes, y el JSON de predicción, el JSON más unificado puede hacer que realice cambios para que la información final que se entrega a la aplicación del lado del cliente se organice de manera diferente. Esto es similar a la refactorización de código y debe tratarse con el mismo proceso de revisión que tiene su organización.

Si no tiene pruebas por lotes para su modelo de V2 y migra las pruebas por lotes al modelo de V3 como parte de la migración, no podrá validar cómo afectará la migración a los resultados de predicción de puntos de conexión.

## <a name="migrating-from-v2-entities"></a>Migración desde entidades de V2

Cuando empiece a migrar al modelo de creación de V3, debe considerar cómo migrar a la entidad de aprendizaje automático y sus subcomponentes, incluyendo las restricciones y los descriptores.

En la tabla siguiente se indican las entidades que se deben migrar de un diseño de entidad de V2 a V3.

|Tipo de entidad de creación de V2|Tipo de entidad de creación de V3|Ejemplo|
|--|--|--|
|Entidad compuesta|Entidad de aprendizaje automático|[más información](#migrate-v2-composite-entity)|
|Entidad jerárquica|Rol de entidad de aprendizaje automático|[más información](#migrate-v2-hierarchical-entity)|

## <a name="migrate-v2-composite-entity"></a>Migración de una entidad compuesta de V2

Cada elemento secundario de la entidad compuesta de V2 se debe representar con un subcomponente de la entidad de aprendizaje automático de V3. Si el elemento secundario compuesto es una expresión regular o una entidad de lista generada previamente, debe aplicarse como **restricción** en el subcomponente que representa el elemento secundario.

Consideraciones a la hora de planear la migración de una entidad compuesta a una entidad de aprendizaje automático:
* Las entidades secundarias no se pueden usar en patrones
* Las entidades secundarias ya no se comparten
* Las entidades secundarias se deben etiquetar si no eran de aprendizaje automático

### <a name="existing-descriptors"></a>Descriptores existentes

Cualquier lista de frases utilizada para potenciar las palabras en la entidad compuesta debe aplicarse como descriptor a la entidad de aprendizaje automático (principal), la entidad del subcomponente (secundaria) o la intención (si la lista de frases solo se aplica a una intención). Planee agregar el descriptor a la entidad que debe potenciar de forma significativa. No agregue el descriptor genéricamente a la entidad de aprendizaje automático (principal), en caso de que potencie de forma significativa la predicción de un subcomponente (secundario).

### <a name="new-descriptors"></a>Nuevos descriptores

En la creación de V3, agregue un paso de planificación para evaluar las entidades como posibles descriptores para todas las entidades e intenciones.

### <a name="example-entity"></a>Entidad de ejemplo

Esta entidad es solo un ejemplo. Su propia migración de entidades puede requerir otras consideraciones.

Considere una entidad compuesta de V2 para modificar un `order` de pizza que usa:
* entidad datetimeV2 creada previamente para la hora de entrega
* lista de frases para potenciar ciertas palabras, como pizza, tarta, corteza e ingrediente
* lista de entidades para detectar los ingredientes, como setas, aceitunas, pepperoni.

Un ejemplo de expresión para esta entidad es:

`Change the toppings on my pie to mushrooms and delivery it 30 minutes later`

En la tabla siguiente se muestra la migración:

|Modelos de V2|Modelos de V3|
|--|--|
|Principal: entidad de componente llamada `Order`|Principal: entidad de aprendizaje automático llamada `Order`|
|Secundario: entidad datetimeV2 creada previamente|* Migre la entidad creada previamente a la nueva aplicación.<br>* Agregue una restricción en el elemento principal para la entidad datetimeV2 creada previamente.|
|Secundario: entidad de lista para los ingredientes|* Migre la entidad de lista a la nueva aplicación.<br>* A continuación, agregue una restricción en el elemento principal para la entidad de lista.|


## <a name="migrate-v2-hierarchical-entity"></a>Migración de una entidad jerárquica de V2

En la creación de V2, se proporcionaba una entidad jerárquica antes de los roles existentes en LUIS. Ambos tenían el mismo propósito de extraer entidades según el uso del contexto. Si tiene entidades jerárquicas, puede considerarlas como entidades simples con roles.

En la creación de V3:
* Un rol se puede aplicar a la entidad de aprendizaje automático (principal).
* Los roles no se pueden aplicar a subcomponentes.

Esta entidad es solo un ejemplo. Su propia migración de entidades puede requerir otras consideraciones.

Considere una entidad jerárquica de V2 para modificar un `order` de pizza:
* donde cada elemento secundario determina un ingrediente original o el ingrediente final

Un ejemplo de expresión para esta entidad es:

`Change the topping from mushrooms to olives`

En la tabla siguiente se muestra la migración:

|Modelos de V2|Modelos de V3|
|--|--|
|Principal: entidad de componente llamada `Order`|Principal: entidad de aprendizaje automático llamada `Order`|
|Secundario: entidad jerárquica con el ingrediente de la pizza original y final|* Agregue un rol a `Order` para cada ingrediente.|

## <a name="next-steps"></a>Pasos siguientes

* [Recursos para desarrolladores](developer-reference-resource.md)
