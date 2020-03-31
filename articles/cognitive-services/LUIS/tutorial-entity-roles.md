---
title: 'Tutorial: Datos contextuales con roles: LUIS'
titleSuffix: Azure Cognitive Services
description: Busque datos relacionados en función del contexto. Por ejemplo, las ubicaciones de origen y destino para un traslado físico de un edificio y oficina a otro edificio y oficina están relacionadas.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: tutorial
ms.date: 12/17/2019
ms.author: diberry
ms.openlocfilehash: cd646ef061a0be06a9b1a56b72a4f35d9796aa63
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "75447879"
---
# <a name="tutorial-extract-contextually-related-data-from-an-utterance"></a>Tutorial: Extracción de datos relacionados contextualmente de una expresión

En este tutorial, busque datos relacionados en función del contexto. Por ejemplo, una ubicación de origen y destino para una transferencia de una ciudad a otra. Ambas partes de datos pueden ser necesarias y están relacionadas entre sí.

Un rol se puede usar con cualquier tipo de entidad precompilada o personalizada, y tanto en patrones como en expresiones de ejemplo.

**En este tutorial, aprenderá a:**

> [!div class="checklist"]
> * Crear nueva aplicación
> * Agregar intención
> * Obtener información sobre el origen y el destino mediante roles
> * Train
> * Publicar
> * Obtener intenciones y roles de entidad del punto de conexión

[!INCLUDE [LUIS Free account](../../../includes/cognitive-services-luis-free-key-short.md)]

## <a name="related-data"></a>Datos relacionados

Esta aplicación determina dónde se va a mover un empleado desde la ciudad de origen a la ciudad de destino. Utiliza una entidad precompilada GeographyV2 para identificar los nombres de las ciudades y usa roles para determinar los tipos de ubicación (origen y destino) dentro de la expresión.

Un rol se debe utilizar cuando los datos de la entidad que se deben extraer:

* Se relacionan entre sí en el contexto de la expresión.
* Utilizan una selección de palabras específicas para indicar cada rol. Ejemplos de estas palabras incluyen: from/to, leaving/headed to, away from/toward.
* Ambos roles se encuentran a menudo en la misma expresión, lo que permite a LUIS aprender de este uso contextual frecuente.
* Debe estar agrupadas y procesarse por la aplicación cliente como una unidad de información.

## <a name="create-a-new-app"></a>Creación de una nueva aplicación

1. Inicie sesión en la versión preliminar del portal de LUIS con la dirección URL [https://preview.luis.ai](https://preview.luis.ai).

1. Seleccione **Crear una aplicación**, escriba el nombre `HumanResources` y mantenga la referencia cultural predeterminada, **English** (Inglés). Deje en blanco la descripción.

1. Seleccione **Listo**.

## <a name="create-an-intent-to-move-employees-between-cities"></a>Creación de una intención para mover los empleados entre ciudades

1. [!INCLUDE [Start in Build section](../../../includes/cognitive-services-luis-tutorial-build-section.md)]

1. Haga clic en **Create new intent** (Crear intención).

1. Escriba `MoveEmployeeToCity` en el cuadro de diálogo emergente y seleccione **Done** (Listo).

    > [!div class="mx-imgBorder"]
    > ![Captura de pantalla del cuadro de diálogo Create new intent (Crear nueva intención)](./media/tutorial-entity-roles/create-new-intent-move-employee-to-city.png)

1. Agregue expresiones de ejemplo a la intención.

    |Expresiones de ejemplo|
    |--|
    |move John W. Smith leaving Seattle headed to Orlando|
    |transfer Jill Jones from Seattle to Cairo|
    |Place John Jackson away from Tampa, coming to Atlanta |
    |move Debra Doughtery to Tulsa from Chicago|
    |mv Jill Jones leaving Cairo headed to Tampa|
    |Shift Alice Anderson to Oakland from Redmond|
    |Carl Chamerlin from San Francisco to Redmond|
    |Transfer Steve Standish from San Diego toward Bellevue |
    |lift Tanner Thompson from Kansas city and shift to Chicago|

    > [!div class="mx-imgBorder"]
    > ![Captura de pantalla de LUIS con nuevas expresiones en la intención MoveEmployee](./media/tutorial-entity-roles/hr-enter-utterances.png)

## <a name="add-prebuilt-entity-geographyv2"></a>Adición de la entidad precompilada geographyV2

La entidad precompilada, geographyV2, extrae información de ubicación, incluidos los nombres de las ciudades. Dado que las expresiones tienen dos nombres de ciudad, relacionados entre sí en contexto, use roles para extraer ese contexto.

1. Seleccione **Entities** (Entidades) en el menú de navegación de la izquierda.

1. Seleccione **Add prebuilt entity** (Agregar entidad precompilada) y `geo` en la barra de búsqueda para filtrar las entidades precompiladas.

    > [!div class="mx-imgBorder"]
    > ![Adición de la entidad precompilada geographyV2 a la aplicación](media/tutorial-entity-roles/add-geographyV2-prebuilt-entity.png)

1. Seleccione la casilla de verificación y **Listo**.
1. En la lista **Entidades**, seleccione **geographyV2** para abrir la nueva entidad.
1. Agregue dos roles (`Origin` y `Destination`).

    > [!div class="mx-imgBorder"]
    > ![Adición de roles a la entidad precompilada](media/tutorial-entity-roles/add-roles-to-prebuilt-entity.png)

1. Seleccione **Intenciones** desde el panel de navegación del lado izquierdo y la intención **MoveEmployeeToCity**. Tenga en cuenta que los nombres de ciudades están etiquetados con la entidad precompilada **geographyV2**.
1. En la barra de herramientas de contexto, seleccione la **paleta de entidades**.

    > [!div class="mx-imgBorder"]
    > ![Seleccionar paleta de entidades en la barra de herramientas del contenido](media/tutorial-entity-roles/intent-detail-context-toolbar-select-entity-palette.png)

1. Seleccione la entidad precompilada, **geographyV2**, después, seleccione el **inspector de entidades**.
1. En el **inspector de entidades**, seleccione un rol, **Destino**. Esto cambia el cursor del mouse. Use el cursor para etiquetar el texto de todas las expresiones que estén en la ubicación de destino.

    > [!div class="mx-imgBorder"]
    > ![Seleccionar rol en la paleta de entidades](media/tutorial-entity-roles/entity-palette-select-entity-role.png)


1. Vuelva al **inspector de entidades** y cambie el rol a **Origen**. Use el cursor para etiquetar el texto de todas las expresiones que estén en la ubicación de origen.

## <a name="add-example-utterances-to-the-none-intent"></a>Incorporación de expresiones de ejemplo a la intención None

[!INCLUDE [Follow these steps to add the None intent to the app](../../../includes/cognitive-services-luis-create-the-none-intent.md)]

## <a name="train-the-app-so-the-changes-to-the-intent-can-be-tested"></a>Entrenamiento de la aplicación para probar los cambios en la intención

[!INCLUDE [LUIS How to Train steps](../../../includes/cognitive-services-luis-tutorial-how-to-train.md)]

## <a name="publish-the-app-so-the-trained-model-is-queryable-from-the-endpoint"></a>Publicación de la aplicación para que se pueda consultar al modelo entrenado desde el punto de conexión

[!INCLUDE [LUIS How to Publish steps](../../../includes/cognitive-services-luis-tutorial-how-to-publish.md)]

## <a name="get-intent-and-entity-prediction-from-endpoint"></a>Obtención de intención y predicción de entidad desde el punto de conexión

1. [!INCLUDE [LUIS How to get endpoint first step](../../../includes/cognitive-services-luis-tutorial-how-to-get-endpoint.md)]


1. Vaya al final de la dirección URL en la barra de direcciones y escriba `Please move Carl Chamerlin from Tampa to Portland`. El último parámetro de la cadena de consulta es `q`, la expresión **query**. Esta expresión no es la misma que las expresiones etiquetadas, por lo que es una buena prueba y debería devolver la intención `MoveEmployee` con la entidad extraída.

    ```json
    {
      "query": "Please move Carl Chamerlin from Tampa to Portland",
      "topScoringIntent": {
        "intent": "MoveEmployeeToCity",
        "score": 0.9706451
      },
      "intents": [
        {
          "intent": "MoveEmployeeToCity",
          "score": 0.9706451
        },
        {
          "intent": "None",
          "score": 0.0307451729
        }
      ],
      "entities": [
        {
          "entity": "tampa",
          "type": "builtin.geographyV2.city",
          "startIndex": 32,
          "endIndex": 36,
          "role": "Origin"
        },
        {
          "entity": "portland",
          "type": "builtin.geographyV2.city",
          "startIndex": 41,
          "endIndex": 48,
          "role": "Destination"
        }
      ]
    }
    ```

    La intención correcta se predice y la matriz de entidades tiene los roles de origen y destino en la propiedad **entities** correspondiente.

## <a name="clean-up-resources"></a>Limpieza de recursos

[!INCLUDE [LUIS How to clean up resources](../../../includes/cognitive-services-luis-tutorial-how-to-clean-up-resources.md)]

## <a name="related-information"></a>Información relacionada

* [Conceptos de entidades](luis-concept-entity-types.md)
* [Conceptos de roles](luis-concept-roles.md)
* [Lista de entidades precompiladas](luis-reference-prebuilt-entities.md)
* [Cómo se realiza el entrenamiento](luis-how-to-train.md)
* [Publicación](luis-how-to-publish-app.md)
* [Prueba de la aplicación el portal de LUIS](luis-interactive-test.md)
* [Roles](luis-concept-roles.md)

## <a name="next-steps"></a>Pasos siguientes

Este tutorial creó una nueva intención y agregó expresiones de ejemplo para los datos aprendidos contextualmente de las ubicaciones de origen y destino. Una vez que la aplicación está entrenada y publicada, una aplicación cliente puede usar esa información para crear un vale de movimiento con la información relevante.

> [!div class="nextstepaction"]
> [Más información sobre cómo agregar una entidad compuesta](luis-tutorial-composite-entity.md)
