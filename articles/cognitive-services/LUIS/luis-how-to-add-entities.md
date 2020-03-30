---
title: 'Agregar entidades: LUIS'
titleSuffix: Azure Cognitive Services
description: Cree entidades para extraer datos clave de las expresiones de usuario de las aplicaciones de Language Understanding (LUIS). La aplicación cliente usa los datos de entidad extraídos para satisfacer las solicitudes de los clientes.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 11/15/2019
ms.author: diberry
ms.openlocfilehash: 1f2b293acdc77e25e6b932c47d466cc28a04a2b6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79219138"
---
# <a name="add-entities-to-extract-data"></a>Agregar entidades para extraer datos 

Cree entidades para extraer datos clave de las expresiones de usuario de las aplicaciones de Language Understanding (LUIS). La aplicación cliente usa los datos de entidad extraídos para satisfacer las solicitudes de los clientes.

La entidad representa una palabra o frase dentro de la expresión que quiere extraer. Las entidades describen información relevante para la intención y a veces son esenciales para que la aplicación lleve a cabo la tarea. Puede crear entidades al agregar una expresión de ejemplo a una intención o además (antes o después) de agregar una expresión de ejemplo a una intención.

[!INCLUDE [Uses preview portal](includes/uses-portal-preview.md)]

## <a name="plan-entities-then-create-and-label"></a>Planificar entidades y después crear y etiquetar

Las entidades con aprendizaje automático se pueden crear a partir de expresiones de ejemplo o desde la página **Entidades**. 

En general, un procedimiento recomendado es dedicar tiempo a planificar las entidades antes de crear una entidad con aprendizaje automático en el portal. A continuación, cree la entidad con aprendizaje automático a partir de la expresión de ejemplo con tantos detalles de los subcomponentes, descriptores y restricciones como conozca en ese momento. En el [tutorial sobre entidades con capacidad de descomposición](tutorial-machine-learned-entity.md) se muestra cómo usar este método. 

Como parte de la planificación de las entidades, es posible que necesite entidades con coincidencia de texto (como entidades precompiladas, entidades de expresiones regulares o entidades de lista). Puede crearlas desde la página **Entidades** antes de que se etiqueten en las expresiones de ejemplo. 

En el etiquetado, puede etiquetar entidades individuales y después crear con ellas una entidad primaria con aprendizaje automático. O bien, puede empezar por una entidad primaria con aprendizaje automático y descomponerla en entidades secundarias. 

> [!TIP] 
>Etiquete todas las palabras que puedan indicar una entidad, incluso si no se usan cuando se extraen en la aplicación cliente. 

## <a name="creating-an-entity-before-or-with-labeling"></a>Creación de una entidad antes del etiquetado o con él

Use la tabla siguiente para conocer las entidades y dónde se crea o agrega cada entidad en la aplicación. 

|Tipo de entidad|Dónde crear la entidad en el portal de LUIS|
|--|--|
|Entidad de aprendizaje automático|Entidades o detalle de intención|
|Entidad de lista|Entidades o detalle de intención|
|Entidad de expresión regular|Entidades|
|Entidad Pattern.any|Entidades|
|Entidad creada previamente|Entidades|
|Entidad de dominio precompilada|Entidades|

Puede crear todas las entidades desde la página **Entidades**, o puede crear un par de entidades como parte el etiquetado de la entidad en la expresión de ejemplo de la página **Intent detail** (Detalle de intención). Solo puede _etiquetar_ una entidad en una expresión de ejemplo desde la página **Intent detail** (Detalle de intención). 

## <a name="create-a-machine-learned-entity"></a>Crear una entidad con aprendizaje automático

[!INCLUDE [Create and label entities in machine-learned tutorial](includes/decomposable-tutorial-links.md)]

## <a name="create-a-text-matching-entity"></a>Crear una entidad con coincidencia de texto

El uso de entidades con coincidencia de texto proporciona varias maneras de extraer datos:

|Entidades con coincidencia de texto|Propósito|
|--|--|
|[Entidad de lista](#add-list-entities-for-exact-matches)|Lista de nombres canónicos junto con sinónimos como formas alternativas|
|Entidad de expresión regular|Coincidencia de texto mediante una entidad de expresiones regulares|
|[Entidad precompilada](tutorial-machine-learned-entity.md#add-prebuilt-number-to-help-extract-data)|Coincidencia de tipos de datos comunes, como el número, el correo electrónico o la fecha|
|Entidad de dominio precompilada|Coincidencia de dominios de asunto seleccionados|
|[Pattern.any](#add-a-patternany-entity)| Coincidencia de entidades que pueden confundirse fácilmente con el texto circundante|  

Las entidades precompiladas funcionan sin proporcionar ningún dato de entrenamiento personalizado. Las demás entidades necesitan que proporcione datos de entrenamiento del cliente (por ejemplo, los elementos de la entidad de lista) o una expresión (como una expresión regular o pattern.any).

<a name="add-list-entities"></a>

### <a name="how-to-create-a-new-custom-entity"></a>Cómo crear una nueva entidad personalizada

1. En el portal de LUIS, vaya a la sección **Administrar** y, a continuación, a la página **Entidades**. 
1. Seleccione **+ Crear** y, luego, seleccione el tipo de entidad. 
1. Continúe con la configuración de la entidad y seleccione **Crear** cuando haya terminado. 

### <a name="add-list-entities-for-exact-matches"></a>Adición de entidades de lista para coincidencias exactas

Las entidades de la lista representan un conjunto fijo y cerrado de palabras relacionadas. Aunque, como autor, puede cambiar la lista, LUIS no aumentará ni reducirá la lista. También puede importar a una entidad de lista existente con el formato [list Entity. JSON (list entity .json format(reference-entity-list.md#example-json-to-import-into-list-entity). 

En la siguiente lista se muestra el nombre canónico y los sinónimos. 

|Color: nombre del elemento de la lista|Color: sinónimos|
|--|--|
|Rojo|Carmín, sangre, manzana, coche de bomberos|
|Azul|Cielo, celeste, cobalto|
|Verde|Kelly, lima|

Use el procedimiento para crear una entidad de lista. Una vez creada la entidad de lista, no es necesario etiquetar expresiones de ejemplo en una intención. Los elementos de la lista y los sinónimos se emparejan usando texto exacto. 

1. En la sección **Compilar**, seleccione **Entidades** en el panel de la izquierda y seleccione **+ Crear**.

1. En el cuadro de diálogo **Create an entity type** (Crear un tipo de entidad), escriba el nombre de la entidad, como `Colors`, y seleccione **Lista**.
1. En el cuadro de diálogo **Create a list entity** (Crear una entidad de lista), en **Agregar nueva sublista...** , escriba el nombre del elemento de lista, por ejemplo, `Green`, y agregue sinónimos.

    > [!div class="mx-imgBorder"]
    > ![Cree una lista de colores como una entidad de lista en la página de detalles de la entidad.](media/how-to-add-entities/create-list-entity-of-colors.png) 

1. Cuando haya terminado de agregar los elementos de la lista y los sinónimos, seleccione **Crear**.

    Cuando haya terminado con un grupo de cambios en la aplicación, recuerde **Entrenar** la aplicación. No entrene la aplicación después de un único cambio. 

    > [!NOTE]
    > En este procedimiento se muestra cómo crear y etiquetar una entidad de lista a partir de una expresión de ejemplo en la página **Intent detail** (Detalle de intención). También puede crear la misma entidad desde la página **Entidades**.

## <a name="add-a-role-for-an-entity"></a>Agregar un rol para una entidad

Un rol es un subtipo con nombre de una entidad, basado en el contexto. 

### <a name="add-a-role-to-distinguish-different-contexts"></a>Adición de un rol para distinguir diferentes contextos

En la siguiente expresión, hay dos ubicaciones, y cada una se especifica semánticamente con las palabras que la rodean, como `to` y `from`: 

`Pick up the package from Seattle and deliver to New York City.`

En este procedimiento, se agregan los roles `origin` y `destination` a una entidad geographyV2 precompilada.

1. En la sección **Build** (Compilar), seleccione **Entities** (Entidades) en el panel de la izquierda.

1. Seleccione **+ Agregar entidad predefinida**. Seleccione **geographyV2** y, a continuación, seleccione **Listo**. Esto agrega una entidad precompilada a la aplicación.
    
    Si descubre que el patrón, cuando incluye una entidad Pattern.any, extrae entidades de forma incorrecta, use una [lista explícita](reference-pattern-syntax.md#explicit-lists) para corregir este problema. 

1. Seleccione la entidad precompilada geographyV2 recién agregada en la lista de entidades de la página **Entidades**. 
1. Para agregar un nuevo rol, seleccione **+** junto a **No roles added** (No se agregaron roles).
1. En el cuadro de texto **Nombre de rol...** , escriba el nombre del rol `Origin` y presione ENTRAR. Agregue un segundo nombre de rol `Destination` y presione ENTRAR. 

    > [!div class="mx-imgBorder"]
    > ![Captura de pantalla de la incorporación del rol de origen a la entidad de ubicación](media/how-to-add-entities//add-role-to-prebuilt-geographyv2-entity.png)

    El rol se agrega a la entidad precompilada, pero no se agrega a ninguna expresión que use esa entidad. 

### <a name="label-text-with-a-role-in-an-example-utterance"></a>Etiquetar texto con un rol en una expresión de ejemplo

1. Vaya a la página de detalles de intención, que tiene una expresión de ejemplo que usa el rol. 
1. Para etiquetar con el rol, seleccione la etiqueta de la entidad (con una línea sólida debajo del texto) en la expresión de ejemplo y, a continuación, seleccione **View in entity palette** (Ver en paleta de entidades) en la lista desplegable. 

    > [!div class="mx-imgBorder"]
    > ![Captura de pantalla de la selección de la visualización en la paleta de entidades](media/how-to-add-entities/select-text-label-with-entity-palette-for-role.png)   

    La paleta de entidades se abre a la derecha. 

1. Seleccione la entidad y, a continuación, vaya a la parte inferior de la paleta y seleccione el rol. 

    > [!div class="mx-imgBorder"]
    > ![Captura de pantalla de la selección de la visualización en la paleta de entidades](media/how-to-add-entities/select-role-from-entity-palette-entity-inspector.png)

<a name="add-pattern-any-entities"></a>

## <a name="add-a-patternany-entity"></a>Incorporación de una entidad pattern.any

Las entidades [pattern.any](luis-concept-entity-types.md) solo son válidas en los [patrones](luis-how-to-model-intent-pattern.md), no en las expresiones de ejemplo de las intenciones. Este tipo de entidad ayuda a LUIS a buscar el final de las entidades de longitud variable y la elección de palabras. Dado que esta entidad se usa en un patrón, LUIS sabe dónde está el final de la entidad en la plantilla de expresión.

### <a name="steps-to-create-a-patternany-entity"></a>Pasos para crear una entidad pattern.any

1. En la sección **Compilar**, seleccione **Entidades** en el panel de la izquierda y seleccione **+ Crear**.

1. En el cuadro de diálogo **Choose an entity type** (Elegir un tipo de entidad), escriba el nombre de la entidad en el cuadro **Nombre** y seleccione **Pattern.Any** como **Tipo** y luego seleccione **Crear**.

    Después de [crear una expresión de patrón](luis-how-to-model-intent-pattern.md) con esta entidad, la entidad se extrae con un algoritmo combinado de coincidencia de texto y aprendizaje automático. 

### <a name="create-a-pattern-template-utterance-to-use-patternany-entity"></a>Cree una expresión de plantilla de patrón para usar la entidad pattern.any

Para usar la entidad pattern.any, agregue un patrón en la página **Patterns** (Patrones), en la sección **Mejorar el rendimiento de las aplicaciones** con la sintaxis de llave correcta, como `Where is **{HumanResourcesFormTitle}** on the server?`.

Si descubre que el patrón, cuando incluye una entidad Pattern.any, extrae entidades de forma incorrecta, use una [lista explícita](reference-pattern-syntax.md#explicit-lists) para corregir este problema. 

## <a name="do-not-change-entity-type"></a>No cambiar el tipo de entidad

LUIS no permite cambiar el tipo de la entidad porque no sabe qué agregar o quitar para construir esa entidad. A fin de cambiar el tipo, se recomienda crear una entidad del tipo correcto con un nombre ligeramente distinto. Una vez creada la entidad, en cada expresión debe quitar el nombre de entidad con etiqueta anterior y agregar el nuevo nombre de entidad. Una vez que se vuelven a etiquetar las expresiones, elimine la entidad anterior. 

<a name="create-a-pattern-from-an-utterance"></a>

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"] 
> [Uso de modelos pregenerados](howto-add-prebuilt-models.md) 

Más información sobre:
* Procedimientos para [entrenar](luis-how-to-train.md)
* Procedimientos para [probar](luis-interactive-test.md)
* Procedimientos para [publicar](luis-how-to-publish-app.md)
* Patrones:
    * [Conceptos](luis-concept-patterns.md)
    * [Sintaxis](reference-pattern-syntax.md)
* [Repositorio de GitHub de entidades pregeneradas](https://github.com/Microsoft/Recognizers-Text)
* [Conceptos de extracción de datos](luis-concept-data-extraction.md)


 
