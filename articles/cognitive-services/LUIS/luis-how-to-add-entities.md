---
title: 'Agregar entidades: LUIS'
description: Cree entidades para extraer datos clave de las expresiones de usuario de las aplicaciones de Language Understanding (LUIS). La aplicación cliente usa los datos de entidad extraídos para satisfacer las solicitudes de los clientes.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 05/17/2020
ms.author: diberry
ms.openlocfilehash: fa7e2321d61721e370ef6b5924dc6f820dd1f724
ms.sourcegitcommit: 50673ecc5bf8b443491b763b5f287dde046fdd31
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/20/2020
ms.locfileid: "83685289"
---
# <a name="add-entities-to-extract-data"></a>Agregar entidades para extraer datos

Cree entidades para extraer datos clave de las expresiones de usuario de las aplicaciones de Language Understanding (LUIS). La aplicación cliente usa los datos de entidad extraídos para satisfacer las solicitudes de los clientes.

La entidad representa una palabra o frase dentro de la expresión que quiere extraer. Las entidades describen información relevante para la intención y a veces son esenciales para que la aplicación lleve a cabo la tarea. Puede crear entidades al agregar una expresión de ejemplo a una intención o además (antes o después) de agregar una expresión de ejemplo a una intención.

## <a name="plan-entities-then-create-and-label"></a>Planificar entidades y después crear y etiquetar

Las entidades de aprendizaje automático se pueden crear a partir de expresiones de ejemplo o desde la página **Entidades**.

En general, un procedimiento recomendado es dedicar tiempo a planificar las entidades antes de crear una entidad de aprendizaje automático en el portal. A continuación, cree la entidad de aprendizaje automático a partir de la expresión de ejemplo con tantos detalles en las subentidades y características como conozca en ese momento. En el [tutorial sobre entidades con capacidad de descomposición](tutorial-machine-learned-entity.md) se muestra cómo usar este método.

Como parte de la planificación de las entidades, es posible que necesite entidades con coincidencia de texto (como entidades precompiladas, entidades de expresiones regulares o entidades de lista). Puede crearlas desde la página **Entidades** antes de que se etiqueten en las expresiones de ejemplo.

En el etiquetado, puede etiquetar entidades individuales y después crear con ellas una entidad primaria de aprendizaje automático. O bien, puede empezar por una entidad primaria de aprendizaje automático y descomponerla en entidades secundarias.

> [!TIP]
>Etiquete todas las palabras que puedan indicar una entidad, incluso si no se usan cuando se extraen en la aplicación cliente.

## <a name="when-to-create-an-entity"></a>Cuándo crear una entidad

Después de planear las entidades, debe crear las entidades y subentidades de aprendizaje automático. Esto puede requerir la incorporación de entidades pregeneradas o entidades de coincidencia de texto para proporcionar características para las entidades de aprendizaje automático. Esto debe realizarse antes de etiquetar.

Una vez que empiece a etiquetar expresiones de ejemplo, puede crear entidades de máquina aprendida o ampliar la lista de entidades.

Use la tabla siguiente para comprender donde crear o agregar cada tipo de entidad en la aplicación.

|Tipo de entidad|Dónde crear la entidad en el portal de LUIS|
|--|--|
|Entidad de aprendizaje automático|Entidades o detalle de intención|
|Entidad de lista|Entidades o detalle de intención|
|Entidad de expresión regular|Entidades|
|Entidad Pattern.any|Entidades|
|Entidad creada previamente|Entidades|
|Entidad de dominio precompilada|Entidades|

Puede crear todas las entidades desde la página **Entidades**, o puede crear un par de entidades como parte el etiquetado de la entidad en la expresión de ejemplo de la página **Intent detail** (Detalle de intención). Solo puede _etiquetar_ una entidad en una expresión de ejemplo desde la página **Intent detail** (Detalle de intención).



## <a name="how-to-create-a-new-custom-entity"></a>Cómo crear una nueva entidad personalizada

Este proceso funciona con entidades de máquina aprendida, entidades de lista y entidades de expresión regular.

1. Inicie sesión en el [portal de LUIS](https://www.luis.ai), seleccione su **Suscripción** y **Recurso de creación** para ver las aplicaciones asignadas a ese recurso de creación.
1. Abra la aplicación mediante la selección de su nombre en la página **Mis aplicaciones**.
1. Seleccione la página **Entidades**.
1. Seleccione **+ Crear** y, luego, seleccione el tipo de entidad.
1. Continúe con la configuración de la entidad y seleccione **Crear** cuando haya terminado.

## <a name="create-a-machine-learned-entity"></a>Creación de una entidad con aprendizaje automático

1. Inicie sesión en el [portal de LUIS](https://www.luis.ai), seleccione su **Suscripción** y **Recurso de creación** para ver las aplicaciones asignadas a ese recurso de creación.
1. Abra la aplicación mediante la selección de su nombre en la página **Mis aplicaciones**.
1. En la sección **Compilar**, seleccione **Entidades** en el panel de la izquierda y seleccione **+ Crear**.
1. En el cuadro de diálogo **Create an entity type** (Crear un tipo de entidad), escriba el nombre de la entidad y seleccione **Machine learned** (Con aprendizaje automático). Para agregar subentidades, seleccione **Agregar estructura**. Seleccione **Crear**.

    > [!div class="mx-imgBorder"]
    > ![Captura de pantalla de la creación de una entidad con aprendizaje automático.](media/add-entities/machine-learned-entity-with-structure.png)

1. En **Add subentities** (Agregar subentidades), agregue una subentidad seleccionando **+** en la fila de la entidad primaria.

    > [!div class="mx-imgBorder"]
    > ![Captura de pantalla de la incorporación de subentidades.](media/add-entities/machine-learned-entity-with-subentities.png)

1. Seleccione **Crear** para finalizar el proceso de creación.

## <a name="add-a-feature-to-a-machine-learned-entity"></a>Incorporación de una característica a una entidad con aprendizaje automático

1. Inicie sesión en el [portal de LUIS](https://www.luis.ai), seleccione su **Suscripción** y **Recurso de creación** para ver las aplicaciones asignadas a ese recurso de creación.
1. Abra la aplicación mediante la selección de su nombre en la página **Mis aplicaciones**.
1. En la sección **Compilar**, seleccione **Entidades** en el panel de la izquierda y luego seleccione la entidad con aprendizaje automático.
1. Agregue una característica seleccionando **+ Agregar característica** en la fila de entidad o subentidad.
1. Seleccione de las listas de frases y entidades existentes.
1. Si la entidad solo debe extraerse si se encuentra la característica, seleccione el asterisco `*` para esa característica.

    > [!div class="mx-imgBorder"]
    > ![Captura de pantalla de la incorporación de la característica a la entidad.](media/add-entities/machine-learned-entity-schema-with-features.png)

## <a name="create-a-regular-expression-entity"></a>Creación de una entidad de expresión regular

1. Inicie sesión en el [portal de LUIS](https://www.luis.ai), seleccione su **Suscripción** y **Recurso de creación** para ver las aplicaciones asignadas a ese recurso de creación.
1. Abra la aplicación mediante la selección de su nombre en la página **Mis aplicaciones**.
1. En la sección **Compilar**, seleccione **Entidades** en el panel de la izquierda y seleccione **+ Crear**.

1. En el cuadro de diálogo **Create an entity type** (Crear un tipo de entidad), escriba el nombre de la entidad y seleccione **RegEx**, escriba la expresión regular en el campo **Regex** y seleccione **Crear**.

    > [!div class="mx-imgBorder"]
    > ![Captura de pantalla de la creación de una entidad de expresión regular.](media/add-entities/add-regular-expression-entity.png)


<a name="add-list-entities"></a>

## <a name="create-a-list-entity"></a>Crear una entidad de lista

Las entidades de la lista representan un conjunto fijo y cerrado de palabras relacionadas. Aunque, como autor, puede cambiar la lista, LUIS no aumentará ni reducirá la lista. También puede importar a una entidad de lista existente mediante un [formato .json de entidad de lista](reference-entity-list.md#example-json-to-import-into-list-entity).

En la siguiente lista se muestra el nombre canónico y los sinónimos.

|Color: nombre del elemento de la lista|Color: sinónimos|
|--|--|
|Rojo|Carmín, sangre, manzana, coche de bomberos|
|Azul|cielo, cobalto|
|Verde|Kelly, lima|

Use el procedimiento para crear una entidad de lista. Una vez creada la entidad de lista, no es necesario etiquetar expresiones de ejemplo en una intención. Los elementos de la lista y los sinónimos se emparejan usando texto exacto.
1. Inicie sesión en el [portal de LUIS](https://www.luis.ai), seleccione su **Suscripción** y **Recurso de creación** para ver las aplicaciones asignadas a ese recurso de creación.
1. Abra la aplicación mediante la selección de su nombre en la página **Mis aplicaciones**.
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
1. Inicie sesión en el [portal de LUIS](https://www.luis.ai), seleccione su **Suscripción** y **Recurso de creación** para ver las aplicaciones asignadas a ese recurso de creación.
1. Abra la aplicación mediante la selección de su nombre en la página **Mis aplicaciones**.
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

> [!TIP]
> Los roles se pueden reemplazar con las etiquetas de las entidades de aprendizaje automático.

1. Inicie sesión en el [portal de LUIS](https://www.luis.ai), seleccione su **Suscripción** y **Recurso de creación** para ver las aplicaciones asignadas a ese recurso de creación.
1. Abra la aplicación mediante la selección de su nombre en la página **Mis aplicaciones**.
1. Vaya a la página de detalles de intención, que tiene una expresión de ejemplo que usa el rol.
1. Para etiquetar con el rol, seleccione la etiqueta de la entidad (con una línea sólida debajo del texto) en la expresión de ejemplo y, a continuación, seleccione **View in entity pane** (Ver en panel de entidades) en la lista desplegable.

    > [!div class="mx-imgBorder"]
    > ![Captura de pantalla de la selección de la visualización en la paleta de entidades](media/add-entities/view-in-entity-pane.png)

    La paleta de entidades se abre a la derecha.

1. Seleccione la entidad y, a continuación, vaya a la parte inferior de la paleta y seleccione el rol.

    > [!div class="mx-imgBorder"]
    > ![Captura de pantalla de la selección de la visualización en la paleta de entidades](media/add-entities/select-role-in-entity-palette.png)

<a name="add-pattern-any-entities"></a>
<a name="add-a-patternany-entity"></a>
<a name="create-a-pattern-from-an-utterance"></a>

## <a name="create-a-patternany-entity"></a>Creación de una entidad pattern.any

La entidad **Pattern.any** solo está disponible con [Patrones](luis-how-to-model-intent-pattern.md).


## <a name="do-not-change-entity-type"></a>No cambiar el tipo de entidad

LUIS no permite cambiar el tipo de la entidad porque no sabe qué agregar o quitar para construir esa entidad. A fin de cambiar el tipo, se recomienda crear una entidad del tipo correcto con un nombre ligeramente distinto. Una vez creada la entidad, en cada expresión debe quitar el nombre de entidad con etiqueta anterior y agregar el nuevo nombre de entidad. Una vez que se vuelven a etiquetar las expresiones, elimine la entidad anterior.


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



