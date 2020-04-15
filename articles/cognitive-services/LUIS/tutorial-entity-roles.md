---
title: 'Tutorial: Datos contextuales con roles: LUIS'
description: Busque datos relacionados en función del contexto. Por ejemplo, las ubicaciones de origen y destino para un traslado físico de un edificio y oficina a otro edificio y oficina están relacionadas.
ms.topic: tutorial
ms.date: 03/30/2020
ms.openlocfilehash: fdb463896e531619ea7ebe7c384729763dc84138
ms.sourcegitcommit: efefce53f1b75e5d90e27d3fd3719e146983a780
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/01/2020
ms.locfileid: "80475827"
---
# <a name="tutorial-extract-contextually-related-data-from-an-utterance"></a>Tutorial: Extracción de datos relacionados contextualmente de una expresión

En este tutorial, busque datos relacionados en función del contexto. Por ejemplo, ubicaciones de origen y destino para un traslado de una ciudad a otra. Ambas partes de datos pueden ser necesarias y están relacionadas entre sí.

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

1. Inicie sesión en el [portal de la **versión preliminar** de LUIS](https://preview.luis.ai).

1. Seleccione **+ New app for conversation** (Nueva aplicación para conversación), escriba el nombre `HumanResources` y mantenga la referencia cultural predeterminada, **English** (Inglés). Deje la descripción y el recurso de predicción vacíos. Seleccione **Listo**.

## <a name="create-an-intent-to-move-employees-between-cities"></a>Creación de una intención para mover los empleados entre ciudades

Las intenciones se usan para clasificar las expresiones de usuario en función del propósito del usuario, que se determina a partir del texto del lenguaje natural.

Para clasificar una expresión, la intención necesita ejemplos de expresiones del usuario que se deben clasificar con esta intención.

1. [!INCLUDE [Start in Build section](../../../includes/cognitive-services-luis-tutorial-build-section.md)]

1. Seleccione **+ Create** (+ Crear).

1. Escriba `MoveEmployeeToCity` en el cuadro de diálogo emergente y seleccione **Done** (Listo).

    > [!div class="mx-imgBorder"]
    > ![Captura de pantalla del cuadro de diálogo Create new intent (Crear nueva intención)](./media/tutorial-entity-roles/create-new-intent-move-employee-to-city.png)

1. Agregue varias expresiones de ejemplo a esta intención que espera que un usuario pida.

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

La entidad precompilada, **geographyV2**, extrae información de ubicación, incluidos los nombres de las ciudades. Dado que las expresiones tienen dos nombres de ciudad, relacionados entre sí en contexto, use roles para extraer ese contexto.

1. Seleccione **Entities** (Entidades) en el menú de navegación de la izquierda.

1. Seleccione **+ Add prebuilt entity** (+ Agregar entidad precompilada) y escriba `geo` en la barra de búsqueda para filtrar las entidades precompiladas.

    > [!div class="mx-imgBorder"]
    > ![Adición de la entidad precompilada geographyV2 a la aplicación](media/tutorial-entity-roles/add-geographyV2-prebuilt-entity.png)

1. Seleccione la casilla de verificación y **Listo**.

## <a name="add-roles-to-prebuilt-entity"></a>Adición de roles a la entidad precompilada

1. En la lista **Entidades**, seleccione **geographyV2** para abrir la nueva entidad.
1. Para agregar un rol, seleccione **+** y agregue los dos roles siguientes: `Origin` y `Destination`.

    > [!div class="mx-imgBorder"]
    > ![Adición de roles a la entidad precompilada](media/tutorial-entity-roles/add-roles-to-prebuilt-entity.png)

## <a name="label-entity-roles-in-example-utterances"></a>Roles de entidad de etiqueta en las expresiones de ejemplo

1. Seleccione **Intenciones** desde el panel de navegación del lado izquierdo y la intención **MoveEmployeeToCity**. Tenga en cuenta que los nombres de ciudades están etiquetados con la entidad precompilada **geographyV2**.
1. En la barra de herramientas de contexto, seleccione la **paleta de entidades** con el _icono del lápiz_.

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

Para entrenar la aplicación, seleccione **Entrenar**. El entrenamiento aplica los cambios, como las nuevas entidades y las expresiones etiquetadas, al modelo activo.

## <a name="publish-the-app-to-access-it-from-the-http-endpoint"></a>Publicación de la aplicación para tener acceso a ella desde el punto de conexión HTTP

[!INCLUDE [LUIS How to Publish steps](includes/howto-publish.md)]


## <a name="get-intent-and-entity-prediction-from-endpoint"></a>Obtención de intención y predicción de entidad desde el punto de conexión

1. [!INCLUDE [LUIS How to get endpoint first step](includes/howto-get-endpoint.md)]


1. Vaya al final de la dirección URL de la barra de direcciones y reemplace _YOUR_QUERY_HERE_ por: `Please move Carl Chamerlin from Tampa to Portland`.

Esta expresión no es la misma que las expresiones etiquetadas, por lo que es una buena prueba y debería devolver la intención `MoveEmployee` con la entidad extraída.

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

    The correct intent is predicted and the entities array has both the origin and destination roles in the corresponding **entities** property.

[!INCLUDE [LUIS How to clean up resources](includes/quickstart-tutorial-cleanup-resources.md)]

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
