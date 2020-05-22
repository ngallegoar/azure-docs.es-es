---
title: 'Inicio rápido: Creación de una aplicación en el portal de LUIS'
description: En este inicio rápido creará las partes básicas de una aplicación, las intenciones y las entidades; además, utilizará una expresión de ejemplo para probar la aplicación en el portal de LUIS.
ms.topic: quickstart
ms.date: 05/05/2020
ms.openlocfilehash: b2ab2ec0fa5398ae8a69906aee25ef44ad5db3c9
ms.sourcegitcommit: bb0afd0df5563cc53f76a642fd8fc709e366568b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/19/2020
ms.locfileid: "83585285"
---
# <a name="quickstart-create-a-new-app-in-the-luis-portal"></a>Inicio rápido: Creación de una aplicación en el portal de LUIS

En este inicio rápido, compilará una nueva aplicación en el portal de LUIS. En primer lugar, cree las partes básicas de una aplicación, las **intenciones** y las **entidades**. Luego, pruebe la aplicación, para lo que debe especificar una expresión de usuario de ejemplo en el panel de prueba interactiva para obtener la intención de predicción.

[!INCLUDE [Sign in to LUIS](./includes/sign-in-process.md)]

## <a name="create-an-app"></a>Creación de una aplicación

1. Seleccione **+ New app for conversation** (+ Nueva aplicación para conversación) en la barra de herramientas de contexto y, después, vuelva a seleccionar **+ New app for conversation** (+ Nueva aplicación para conversación).

    > [!div class="mx-imgBorder"]
    > [![Captura de pantalla de la creación de una nueva aplicación en el portal de LUIS](./media/create-app-in-portal.png)](./media/create-app-in-portal.png#lightbox)

1. En la ventana emergente, configure la aplicación con los siguientes valores y, a continuación, seleccione **Listo**.

   |Nombre del valor| Value | Propósito|
   |--|--|--|
   |Nombre|`myEnglishApp`|Nombre único de la aplicación de LUIS<br>requerido|
   |Referencia cultural|**Inglés**|Idioma de las expresiones de los usuarios, **en-us**<br>requerido|
   |Description (Descripción) (opcional)|`App made with LUIS Portal`|Descripción de la aplicación<br>opcional|
   |Prediction resource (Recurso de predicción) (opcional) |-  |No seleccione ninguno. LUIS proporciona una clave de inicio que puede usar de forma gratuita para la creación y que incluye 1000 solicitudes al punto de conexión de predicción. |

   ![Captura de pantalla de la especificación de la configuración de la nueva aplicación](./media/get-started-portal-build-app/create-new-app-settings.png)

## <a name="create-intents"></a>Creación de intenciones

Una vez creada la aplicación de LUIS, deberá crear las intenciones. Las intenciones son una manera de clasificar el texto de los usuarios. Por ejemplo, una aplicación de recursos humanos podría tener dos funciones. Ayudar a las personas:

 1. Buscar y solicitar trabajo
 1. Buscar formas de solicitar trabajo

Las dos _intenciones_ diferentes de la aplicación se alinean con las intenciones siguientes:

|Intención|Texto de ejemplo de usuario<br>conocido como _expresión_|
|--|--|
|ApplyForJob|`I want to apply for the new software engineering position in Cairo.`|
|FindForm|`Where is the job transfer form hrf-123456?`|

Para crear intenciones, complete los siguientes pasos:

1. Una vez creada la aplicación, se encuentra en la página **Intenciones** de la sección **Compilar**. Seleccione **Crear**.

   [![Captura de pantalla de la selección de "Crear" para crear una nueva intención](./media/get-started-portal-build-app/create-new-intent-button.png)](./media/get-started-portal-build-app/create-new-intent-button.png#lightbox)

1. Escriba el nombre de la intención, `FindForm`, y después seleccione **Listo**.

## <a name="add-an-example-utterance"></a>Incorporación de una expresión de ejemplo

Agregue expresiones de ejemplo después de crear las intenciones. Las expresiones de ejemplo son texto que un usuario escribe en un bot de chat o en otra aplicación cliente. Asignan la intención del texto del usuario a una intención de LUIS.

Para esta intención `FindForm` de la aplicación de ejemplo, las expresiones de ejemplo incluirán el número de formulario. La aplicación cliente necesita el número de formulario para satisfacer la solicitud del usuario, por lo que es importante incluirlo en la expresión.

> [!div class="mx-imgBorder"]
> [![Captura de pantalla de la especificación de expresiones de ejemplo para la intención FindForm](./media/get-started-portal-build-app/add-example-utterance.png)](./media/get-started-portal-build-app/add-example-utterance.png#lightbox)

Agregue las quince expresiones de ejemplo siguientes a la intención `FindForm`.

|#|Expresiones de ejemplo|
|--|--|
|1|`Looking for hrf-123456`|
|2|`Where is the human resources form hrf-234591?`|
|3|`hrf-345623, where is it`|
|4|`Is it possible to send me hrf-345794`|
|5|`Do I need hrf-234695 to apply for an internal job?`|
|6|`Does my manager need to know I'm applying for a job with hrf-234091`|
|7|`Where do I send hrf-234918? Do I get an email response it was received?`|
|8|`hrf-234555`|
|9|`When was hrf-234987 updated?`|
|10|`Do I use form hrf-876345 to apply for engineering positions`|
|11|`Was a new version of hrf-765234 submitted for my open req?`|
|12|`Do I use hrf-234234 for international jobs?`|
|13|`hrf-234598 spelling mistake`|
|14|`will hrf-234567 be edited for new requirements`|
|15|`hrf-123456, hrf-123123, hrf-234567`|

A propósito, estas expresiones de ejemplo varían con respecto a lo siguiente:

* longitud de la expresión
* [puntuación](luis-reference-application-settings.md#punctuation-normalization)
* elección de palabras
* tiempo verbal (presente, pasado, futuro)
* orden de las palabras


## <a name="create-a-regular-expression-entity"></a>Creación de una entidad de expresión regular

Para devolver el número de formulario en la respuesta de predicción en tiempo de ejecución, el número de formulario debe extraerse como una entidad. Puesto que el texto del número de formulario está muy estructurado, se puede usar con una entidad de expresión regular. Cree la entidad de expresión regular con los pasos siguientes:

1. Seleccione **Entities** (Entidades) en el menú de la izquierda.

1. Seleccione **Create** (Crear) en la página **Entities** (Entidades).

1. Escriba el nombre `FormNumber`, seleccione el tipo de entidad **Regex**.

1. Escriba la expresión regular `hrf-[0-9]{6}` en el campo **Regex**. Esta entrada coincide con los caracteres literales, `hrf-`, y permite exactamente seis dígitos; después, seleccione **Crear**.

    > [!div class="mx-imgBorder"]
    > ![Captura de pantalla de la creación de una entidad de expresión regular](./media/get-started-portal-build-app/create-regular-expression-entity.png)


    Esta entidad extrae cualquier texto que coincida con la expresión regular en cualquiera de las intenciones.

## <a name="add-example-utterances-to-the-none-intent"></a>Incorporación de expresiones de ejemplo a la intención None

La intención **None** es la intención de reserva y no debe dejarse vacía. Esta intención debe contener una expresión para cada diez expresiones de ejemplo agregadas para las otras intenciones de la aplicación.

Las expresiones de ejemplo de la intención **None** deben estar fuera del dominio de la aplicación cliente.

1. Seleccione **Intents** (Intenciones) en el menú izquierdo y, después, seleccione **None** (Ninguna) en la lista de intenciones.

1. Agregue las siguientes expresiones de ejemplo a la intención:

   |Expresiones de ejemplo para la intención None|
   |--|
   |`Barking dogs are annoying`|
   |`Penguins in the ocean`|

   En el caso de esta aplicación, estas expresiones de ejemplo están fuera del dominio. Si el dominio incluye animales o el océano, debe usar diferentes expresiones de ejemplo para la intención **Ninguna**.

## <a name="train-the-app"></a>Entrenamiento de la aplicación

[!INCLUDE [LUIS How to Train steps](includes/howto-train.md)]

## <a name="look-at-the-regular-expression-entity-in-the-example-utterances"></a>Examen de la entidad de la expresión regular en las expresiones de ejemplo

1. Compruebe que la entidad se encuentra en la intención **FindForm** seleccionando **Intents** (Intenciones) en el menú izquierdo. A continuación, seleccione la intención **FindForm**.

   La entidad se marca donde aparece en las expresiones de ejemplo. Si desea ver el texto original, en lugar del nombre de entidad, cambie a **Entities View** (Vista de entidades) en la barra de herramientas.

   > [!div class="mx-imgBorder"]
   > [![Captura de pantalla de todas las expresiones de ejemplo marcadas con entidades](./media/get-started-portal-build-app/all-example-utterances-marked-with-entities.png)](./media/get-started-portal-build-app/all-example-utterances-marked-with-entities.png#lightbox)

## <a name="test-your-new-app-with-the-interactive-test-pane"></a>Prueba de la nueva aplicación con el panel de prueba interactiva

Use el panel de **prueba** interactiva en el portal de LUIS para validar que la entidad se extrae de las nuevas expresiones que la aplicación aún no ha visto.

1. Seleccione **Test** (Prueba) en el menú de la parte superior derecha.

1. Agregue una expresión nueva y, después, presione ENTRAR:

   ```Is there a form named hrf-234098```

    Seleccione **Inspect** (Inspeccionar) para ver las predicciones de la entidad.

   > [!div class="mx-imgBorder"]
   > ![Captura de pantalla de la prueba de una nueva expresión en el panel de prueba](./media/get-started-portal-build-app/test-new-utterance.png)

   La intención de predicción con mayor puntuación es **FindForm**, con un nivel de confianza superior al 90 % (0,977). Se extrae la entidad **FormNumber** con un valor de hrf-234098.

## <a name="clean-up-resources"></a>Limpieza de recursos

Si ha terminado con este inicio rápido y no va a seguir con el siguiente, seleccione **Mis aplicaciones** en el menú de navegación superior. Luego, seleccione la casilla a la izquierda de la aplicación en la lista y seleccione **Eliminar** en la barra de herramientas de contexto que está encima de la lista.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [2. Implementación de una aplicación](get-started-portal-deploy-app.md)
