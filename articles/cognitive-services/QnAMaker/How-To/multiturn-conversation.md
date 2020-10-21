---
title: 'Conversaciones de varios turnos: QnA Maker'
description: Use solicitudes y contexto para administrar los múltiples turnos del bot, lo que se conoce como multiturno, de una pregunta a otra. Multiturno es la capacidad de mantener una conversación fluida en la que el contexto de la pregunta anterior influye en la siguiente pregunta y en su respuesta.
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: conceptual
ms.date: 04/13/2020
ms.openlocfilehash: 57e878f11f5857f4c5b629a8069f64de259e8e7a
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "91777803"
---
# <a name="use-follow-up-prompts-to-create-multiple-turns-of-a-conversation"></a>Uso de avisos de seguimiento para crear múltiples turnos de una conversación

Use avisos de seguimiento y contexto para administrar los múltiples turnos del bot, lo que se conoce como _multiturno_, de una pregunta a otra.

Para ver cómo funciona el multiturno, vea el siguiente vídeo de demostración:

[![Multi-turn conversation in QnA Maker](../media/conversational-context/youtube-video.png)](https://aka.ms/multiturnexample) (Conversaciones multiturno en QnA Maker)

## <a name="what-is-a-multi-turn-conversation"></a>¿Qué es una conversación multiturno?

Algunas preguntas no se pueden responder en un único turno. Cuando diseña conversaciones de la aplicación cliente (bot de chat), el usuario puede formular una pregunta que debe filtrarse o mejorarse para determinar la respuesta correcta. Puede crear este flujo de las preguntas posibles presentando al usuario *avisos de seguimiento*.

Cuando un usuario realiza una pregunta, QnA Maker devuelve la respuesta _y_ los avisos de seguimiento. Esta respuesta le permite presentar las opciones de seguimiento como posibles respuestas.

> [!CAUTION]
> Los avisos de múltiples turnos no se extraen de los documentos de preguntas más frecuentes. Si necesita la extracción de avisos de múltiples turnos, quite los signos de interrogación que designan los pares de preguntas y respuestas como preguntas más frecuentes.

## <a name="example-multi-turn-conversation-with-chat-bot"></a>Ejemplo de conversación multiturno con bot de chat

Con multiturno, un bot de chat administra una conversación con un usuario para determinar la respuesta final como se indica en la imagen siguiente:

![Un diálogo multiturno con preguntas que guían a un usuario por una conversación](../media/conversational-context/conversation-in-bot.png)

En la imagen anterior, un usuario ha iniciado una conversación escribiendo **Mi cuenta**. La base de conocimiento tiene tres pares de preguntas y respuestas vinculados. Para mejorar la respuesta, el usuario selecciona una de las tres opciones en la base de conocimiento. La pregunta (1), incluye tres avisos de seguimiento, que se presentan en el bot de chat como tres posibles opciones (2).

Cuando el usuario selecciona una opción (3), se presenta la siguiente lista de opciones de mejora (4). Esta secuencia continúa (5) hasta que el usuario determina la respuesta correcta, final (6).


### <a name="use-multi-turn-in-a-bot"></a>Uso del multiturno en un bot

Después de publicar la base de conocimiento, puede seleccionar el botón **Create Bot** (Crear bot) para implementar el bot QnA Maker en el servicio de bots de Azure. Los mensajes aparecerán en los clientes de chat que haya habilitado para el bot.

## <a name="create-a-multi-turn-conversation-from-a-documents-structure"></a>Creación de una conversación multiturno a partir de la estructura de un documento

Cuando crea una base de conocimiento, la sección **Populate your KB** (Rellenar la base de conocimiento) muestra la casilla **Enable multi-turn extraction from URLs, .pdf or .docx files** (Habilitar extracción multiturno a partir de URL o de archivos .pdf o .docx).

![Casilla para habilitar la extracción multiturno](../media/conversational-context/enable-multi-turn.png)

Cuando se selecciona esta opción, QnA Maker extrae la jerarquía presente en la estructura del documento. La jerarquía se convierte para hacer un seguimiento de las solicitudes y la raíz de la jerarquía actúa como las preguntas y respuestas principales. En algunos documentos, la raíz de la jerarquía no tiene contenido que podría servir como respuesta, puede proporcionar el "texto de respuesta predeterminado" que se usará como texto sustituto de la respuesta para extraer dichas jerarquías.

Solo se puede inferir la estructura multiturno a partir de direcciones URL y de archivos PDF o DOCX. Para obtener un ejemplo de estructura, vea una imagen de un [archivo PDF de manual de usuario de Microsoft Surface](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/qna-maker/data-source-formats/product-manual.pdf).

:::image type="content" source="../media/conversational-context/import-file-with-conversational-structure.png" alt-text="Captura de pantalla que muestra un ejemplo de la estructura en un manual de usuario." lightbox="../media/conversational-context/import-file-with-conversational-structure.png":::

### <a name="building-your-own-multi-turn-document"></a>Creación del propio documento multiturno

Si va a crear un documento multiturno, tenga en cuenta las siguientes directrices:

* Use encabezados y subtítulos para denotar la jerarquía. Por ejemplo, puede agregar h1 para indicar las preguntas y respuestas principales y h2 para las que se deben tomar como aviso. Use el tamaño de encabezado pequeño para indicar la jerarquía subsiguiente. No utilice el estilo, el color u otro mecanismo para inferir la estructura en el documento, QnA Maker no extraerá las indicaciones multiturno.

* El primer carácter del encabezado debe escribirse en mayúsculas.

* No termine un encabezado con un signo de interrogación, `?`.

* Puede usar el [documento de ejemplo](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/qna-maker/data-source-formats/multi-turn.docx) como ejemplo para crear su propio documento multiturno.

### <a name="adding-files-to-a-multi-turn-kb"></a>Adición de archivos a una base de conocimiento multiturno

Al agregar un documento multiturno, QnA Maker determina los avisos de seguimiento a partir de la estructura para crear un flujo de conversación.

1. En QnA Maker, seleccione una base de conocimiento existente que se creó con la opción para **habilitar la extracción multiturno de URL, archivos o documentos .pdf o .docx.** habilitado.
1. Vaya a la página de **configuración**  y seleccione el archivo o URL que quiere agregar.
1. **Guarde y entrene** la base de conocimiento.

> [!Caution]
> No se admite el uso de un archivo de base de conocimiento multiturno de tipo TSV o XLS que haya sido exportado como origen de datos para una base de conocimiento nueva o vacía. Debe **importar** ese tipo de archivo, desde la página de **configuración** del portal de QnA Maker para agregar mensajes exportados multiturno a una base de conocimiento.


## <a name="create-knowledge-base-with-multi-turn-prompts-with-the-create-api"></a>Creación de la base de conocimiento con avisos multiturno con Create API

Puede crear una base de conocimiento con avisos multiturno mediante [Create API de QnA Maker](https://docs.microsoft.com/rest/api/cognitiveservices/qnamaker/knowledgebase/create). Los avisos se agregan en la matriz `prompts` de la propiedad `context`.

## <a name="show-questions-and-answers-with-context"></a>Mostrar preguntas y respuestas con contexto

Reduzca los pares de preguntas y respuestas que aparecen a solo aquellos con conversaciones contextuales.

Seleccione **View options** (Opciones de vista) y, después, seleccione **Show context** (Mostrar contexto). La lista muestra los pares de preguntas y respuestas que contienen avisos de seguimiento.

![Filtrar pares de preguntas y respuestas por conversaciones contextuales](../media/conversational-context/filter-question-and-answers-by-context.png)

El contexto multiturno se muestra en la primera columna.

:::image type="content" source="../media/conversational-context/surface-manual-pdf-follow-up-prompt.png" alt-text="Captura de pantalla que muestra un ejemplo de la estructura en un manual de usuario." lightbox="../media/conversational-context/surface-manual-pdf-follow-up-prompt.png":::

En la imagen anterior, **1** indica texto en negrita en la columna, lo cual indica la pregunta actual. La pregunta principal es el elemento superior de la fila. Todas las preguntas por debajo de ella son pares de preguntas y respuestas vinculadas. Estos elementos son seleccionables, por lo que puede pasar inmediatamente a los demás elementos de contexto.

## <a name="add-an-existing-question-and-answer-pair-as-a-follow-up-prompt"></a>Incorporación de un par de pregunta y respuesta existente como opción de seguimiento

La pregunta original, **Mi cuenta**, tiene avisos de seguimiento, como **Accounts and signing in** (Cuentas e inicio de sesión).

![Respuesta "Accounts and signing in" (Cuentas e inicio de sesión) y avisos de seguimiento](../media/conversational-context/detected-and-linked-follow-up-prompts.png)

Agregue una opción de seguimiento a un par de pregunta y respuesta que no esté actualmente vinculado. Como la pregunta no está vinculada a ningún par de pregunta y respuesta, se debe cambiar la configuración actual de la vista.

1. Para vincular un par de pregunta y respuesta existente como opción de seguimiento, seleccione la fila del par de pregunta y respuesta. Para el manual de Surface, busque **Sign out** (Cerrar sesión) para reducir la lista.
1. En la fila de **Signout** (Cerrar sesión), en la columna **Respuesta**, seleccione **Add follow-up prompt** (Agregar opción de seguimiento).
1. En los campos de la ventana emergente **Follow-up prompt** (Aviso de seguimiento), escriba los siguientes valores:

    |Campo|Value|
    |--|--|
    |Display text|Escriba **Apagar el dispositivo**. Este es el texto personalizado que se mostrará en el aviso de seguimiento.|
    |Context-only (Solo contexto)| Seleccione esta casilla. Solo se devolverá una respuesta si la pregunta especifica contexto.|
    |Link to answer (Vínculo a la respuesta)|Escriba **Usar la pantalla de inicio de sesión** para encontrar el par de pregunta y respuesta existente.|


1.  Se devuelve una coincidencia. Seleccione esta respuesta como seguimiento y, a continuación, seleccione **Guardar**.

    ![Página "Follow-up prompt (PREVIEW)" [Aviso de seguimiento (VERSIÓN PRELIMINAR)]](../media/conversational-context/search-follow-up-prompt-for-existing-answer.png)

1. Una vez que haya agregado el aviso de seguimiento, seleccione **Save and train** (Guardar y entrenar) en la barra de navegación superior.

### <a name="edit-the-display-text"></a>Edición del texto para mostrar

Cuando se crea un aviso de seguimiento, y se especifica un par de pregunta y respuesta como **vínculo a la respuesta**, puede escribir un nuevo **texto para mostrar**. Este texto no reemplaza la pregunta existente y no agrega una nueva pregunta alternativa. Es independiente de esos valores.

1. Para editar el texto para mostrar, busque y seleccione la pregunta en el campo **Context** (Contexto).
1. En la fila de esa pregunta, seleccione el aviso de seguimiento de la columna de respuesta.
1. Seleccione el texto para mostrar que desea editar y, a continuación, seleccione **Editar**.

    ![Comando Editar texto para mostrar](../media/conversational-context/edit-existing-display-text.png)

1. En la ventana emergente **Follow-up prompt** (Aviso de seguimiento), cambie el texto para mostrar existente.
1. Cuando haya terminado de editar el texto para mostrar, seleccione **Guardar**.
1. En la barra de navegación superior, seleccione **Save and train** (Guardar y entrenar).


## <a name="add-a-new-question-and-answer-pair-as-a-follow-up-prompt"></a>Incorporación de un nuevo par de pregunta y respuesta como aviso de seguimiento

Cuando se agrega un nuevo par de pregunta y respuesta a la base de conocimiento, cada par se debe vincular a una pregunta existente como un aviso de seguimiento.

1. En la barra de herramientas de la base de conocimiento, busque y seleccione el par de pregunta y respuesta existente para **Accounts and signing in** (Cuentas e inicio de sesión).

1. En la columna **Respuesta** seleccione **Add follow-up prompt** (Agregar opción de seguimiento).
1. En **Follow-up prompt (PREVIEW)** [Aviso de seguimiento (VERSIÓN PRELIMINAR)], cree un nuevo aviso de seguimiento mediante la especificación de los valores siguientes:

    |Campo|Value|
    |--|--|
    |Display text|*Create a Windows Account* (Crear una cuenta de Windows). El texto personalizado que se mostrará en el aviso de seguimiento.|
    |Context-only (Solo contexto)|Seleccione esta casilla. Solo se devolverá esta respuesta si la pregunta especifica contexto.|
    |Link to answer (Vínculo a la respuesta)|Escriba el texto siguiente como respuesta:<br>*[Crear](https://account.microsoft.com/) una cuenta de Windows con una cuenta de correo electrónico nueva o existente*.<br>Cuando guarde y entrene la base de datos, este texto se convertirá. |
    |||

    ![Crear una nueva pregunta y respuesta de seguimiento](../media/conversational-context/create-child-prompt-from-parent.png)


1. Seleccione **Crear nuevo** y, a continuación, seleccione **Guardar**.

    Esta acción crea un nuevo par de pregunta y respuesta, y vincula la pregunta seleccionada como un aviso de seguimiento. La columna **Contexto** de ambas preguntas, indica una relación de aviso de seguimiento.

1. Seleccione **Ver opciones** y, a continuación, seleccione [**Mostrar contexto (VERSIÓN PRELIMINAR)** ](#show-questions-and-answers-with-context).

    La nueva pregunta muestra cómo se vincula.

    ![Crear un nuevo aviso de seguimiento](../media/conversational-context/new-qna-follow-up-prompt.png)

    La pregunta primaria muestra una nueva pregunta como una de sus opciones.

    :::image type="content" source="../media/conversational-context/child-prompt-created.png" alt-text="Captura de pantalla que muestra un ejemplo de la estructura en un manual de usuario." lightbox="../media/conversational-context/child-prompt-created.png":::

1. Una vez que haya agregado el aviso de seguimiento, seleccione **Save and train** (Guardar y entrenar) en la barra de navegación superior.

<a name="enable-multi-turn-during-testing-of-follow-up-prompts"></a>

## <a name="view-multi-turn-during-testing-of-follow-up-prompts"></a>Visualización del multiturno durante la prueba de los avisos de seguimiento

Al probar la pregunta con mensajes de seguimiento en el panel de **pruebas**, la respuesta incluye las solicitudes de seguimiento.

![La respuesta incluye los avisos de seguimiento](../media/conversational-context/test-pane-with-question-having-follow-up-prompts.png)

## <a name="a-json-request-to-return-an-initial-answer-and-follow-up-prompts"></a>Una solicitud JSON para devolver una respuesta inicial y avisos de seguimiento

Utilice el objeto `context` vacío para solicitar la respuesta a la pregunta del usuario e incluir avisos de seguimiento.

```JSON
{
  "question": "accounts and signing in",
  "top": 10,
  "userId": "Default",
  "isTest": false,
  "context": {}
}
```

## <a name="a-json-response-to-return-an-initial-answer-and-follow-up-prompts"></a>Una respuesta JSON para devolver una respuesta inicial y avisos de seguimiento

En la sección anterior se solicitó una respuesta y todos los avisos de seguimiento para **Accounts and signing in** (Cuentas e inicio de sesión). La respuesta incluye la información del aviso, que se encuentra en *answers[0].context* y el texto para mostrar al usuario.

```JSON
{
    "answers": [
        {
            "questions": [
                "Accounts and signing in"
            ],
            "answer": "**Accounts and signing in**\n\nWhen you set up your Surface, an account is set up for you. You can create additional accounts later for family and friends, so each person using your Surface can set it up just the way he or she likes. For more info, see All about accounts on Surface.com. \n\nThere are several ways to sign in to your Surface Pro 4: ",
            "score": 100.0,
            "id": 15,
            "source": "product-manual.pdf",
            "metadata": [],
            "context": {
                "isContextOnly": true,
                "prompts": [
                    {
                        "displayOrder": 0,
                        "qnaId": 16,
                        "qna": null,
                        "displayText": "Use the sign-in screen"
                    }
                ]
            }
        },
        {
            "questions": [
                "Sign out"
            ],
            "answer": "**Sign out**\n\nHere's how to sign out: \n\n Go to Start, and right-click your name. Then select Sign out. ",
            "score": 38.01,
            "id": 18,
            "source": "product-manual.pdf",
            "metadata": [],
            "context": {
                "isContextOnly": true,
                "prompts": [
                    {
                        "displayOrder": 0,
                        "qnaId": 16,
                        "qna": null,
                        "displayText": "Turn off the device"
                    }
                ]
            }
        },
        {
            "questions": [
                "Use the sign-in screen"
            ],
            "answer": "**Use the sign-in screen**\n\n1.  \n\nTurn on or wake your Surface by pressing the power button. \n\n2.  \n\nSwipe up on the screen or tap a key on the keyboard. \n\n3.  \n\nIf you see your account name and account picture, enter your password and select the right arrow or press Enter on your keyboard. \n\n4.  \n\nIf you see a different account name, select your own account from the list at the left. Then enter your password and select the right arrow or press Enter on your keyboard. ",
            "score": 27.53,
            "id": 16,
            "source": "product-manual.pdf",
            "metadata": [],
            "context": {
                "isContextOnly": true,
                "prompts": []
            }
        }
    ]
}
```

La matriz `prompts` proporciona el texto en la propiedad `displayText` y el valor `qnaId`. Puede mostrar estas respuestas como las siguientes opciones mostradas en el flujo de conversación y, a continuación, enviar el `qnaId` seleccionado a QnA Maker en la siguiente solicitud.

<!--

The `promptsToDelete` array provides the ...

-->

## <a name="a-json-request-to-return-a-non-initial-answer-and-follow-up-prompts"></a>Una solicitud JSON para devolver una respuesta que no es la inicial y avisos de seguimiento

Rellene el objeto `context` para que incluya el contexto anterior.

En la siguiente solicitud JSON, la pregunta actual es *Use Windows Hello to sign in* (Usar Windows Hello para iniciar sesión) y la pregunta anterior era *Accounts and signing in* (Cuentas e inicio de sesión).

```JSON
{
  "question": "Use Windows Hello to sign in",
  "top": 10,
  "userId": "Default",
  "isTest": false,
  "qnaId": 17,
  "context": {
    "previousQnAId": 15,
    "previousUserQuery": "accounts and signing in"
  }
}
```

##  <a name="a-json-response-to-return-a-non-initial-answer-and-follow-up-prompts"></a>Una respuesta JSON para devolver una respuesta que no es la inicial y avisos de seguimiento

La respuesta JSON _GenerateAnswer_ de QnA Maker incluye los avisos de seguimiento de la propiedad `context` del primer elemento del objeto `answers`:

```JSON
{
    "answers": [
        {
            "questions": [
                "Use Windows Hello to sign in"
            ],
            "answer": "**Use Windows Hello to sign in**\n\nSince Surface Pro 4 has an infrared (IR) camera, you can set up Windows Hello to sign in just by looking at the screen. \n\nIf you have the Surface Pro 4 Type Cover with Fingerprint ID (sold separately), you can set up your Surface sign you in with a touch. \n\nFor more info, see What is Windows Hello? on Windows.com. ",
            "score": 100.0,
            "id": 17,
            "source": "product-manual.pdf",
            "metadata": [],
            "context": {
                "isContextOnly": true,
                "prompts": []
            }
        },
        {
            "questions": [
                "Meet Surface Pro 4"
            ],
            "answer": "**Meet Surface Pro 4**\n\nGet acquainted with the features built in to your Surface Pro 4. \n\nHere's a quick overview of Surface Pro 4 features: \n\n\n\n\n\n\n\nPower button \n\n\n\n\n\nPress the power button to turn your Surface Pro 4 on. You can also use the power button to put it to sleep and wake it when you're ready to start working again. \n\n\n\n\n\n\n\nTouchscreen \n\n\n\n\n\nUse the 12.3" display, with its 3:2 aspect ratio and 2736 x 1824 resolution, to watch HD movies, browse the web, and use your favorite apps. \n\nThe new Surface G5 touch processor provides up to twice the touch accuracy of Surface Pro 3 and lets you use your fingers to select items, zoom in, and move things around. For more info, see Surface touchscreen on Surface.com. \n\n\n\n\n\n\n\nSurface Pen \n\n\n\n\n\nEnjoy a natural writing experience with a pen that feels like an actual pen. Use Surface Pen to launch Cortana in Windows or open OneNote and quickly jot down notes or take screenshots. \n\nSee Using Surface Pen (Surface Pro 4 version) on Surface.com for more info. \n\n\n\n\n\n\n\nKickstand \n\n\n\n\n\nFlip out the kickstand and work or play comfortably at your desk, on the couch, or while giving a hands-free presentation. \n\n\n\n\n\n\n\nWi-Fi and Bluetooth&reg; \n\n\n\n\n\nSurface Pro 4 supports standard Wi-Fi protocols (802.11a/b/g/n/ac) and Bluetooth 4.0. Connect to a wireless network and use Bluetooth devices like mice, printers, and headsets. \n\nFor more info, see Add a Bluetooth device and Connect Surface to a wireless network on Surface.com. \n\n\n\n\n\n\n\nCameras \n\n\n\n\n\nSurface Pro 4 has two cameras for taking photos and recording video: an 8-megapixel rear-facing camera with autofocus and a 5-megapixel, high-resolution, front-facing camera. Both cameras record video in 1080p, with a 16:9 aspect ratio. Privacy lights are located on the right side of both cameras. \n\nSurface Pro 4 also has an infrared (IR) face-detection camera so you can sign in to Windows without typing a password. For more info, see Windows Hello on Surface.com. \n\nFor more camera info, see Take photos and videos with Surface and Using autofocus on Surface 3, Surface Pro 4, and Surface Book on Surface.com. \n\n\n\n\n\n\n\nMicrophones \n\n\n\n\n\nSurface Pro 4 has both a front and a back microphone. Use the front microphone for calls and recordings. Its noise-canceling feature is optimized for use with Skype and Cortana. \n\n\n\n\n\n\n\nStereo speakers \n\n\n\n\n\nStereo front speakers provide an immersive music and movie playback experience. To learn more, see Surface sound, volume, and audio accessories on Surface.com. \n\n\n\n\n",
            "score": 21.92,
            "id": 3,
            "source": "product-manual.pdf",
            "metadata": [],
            "context": {
                "isContextOnly": true,
                "prompts": [
                    {
                        "displayOrder": 0,
                        "qnaId": 4,
                        "qna": null,
                        "displayText": "Ports and connectors"
                    }
                ]
            }
        },
        {
            "questions": [
                "Use the sign-in screen"
            ],
            "answer": "**Use the sign-in screen**\n\n1.  \n\nTurn on or wake your Surface by pressing the power button. \n\n2.  \n\nSwipe up on the screen or tap a key on the keyboard. \n\n3.  \n\nIf you see your account name and account picture, enter your password and select the right arrow or press Enter on your keyboard. \n\n4.  \n\nIf you see a different account name, select your own account from the list at the left. Then enter your password and select the right arrow or press Enter on your keyboard. ",
            "score": 19.04,
            "id": 16,
            "source": "product-manual.pdf",
            "metadata": [],
            "context": {
                "isContextOnly": true,
                "prompts": []
            }
        }
    ]
}
```

## <a name="query-the-knowledge-base-with-the-qna-maker-id"></a>Consulta de la base de conocimiento con el identificador de QnA Maker

Si va a compilar una aplicación personalizada mediante la característica multiturno. En la respuesta de la pregunta inicial, se devuelven los avisos de seguimiento y sus identificadores `qnaId` asociados. Ahora que tiene el identificador, puede pasarlo en el cuerpo de la solicitud del aviso de seguimiento. Si el cuerpo de la solicitud contiene el identificador `qnaId` y el objeto de contexto (que contiene las propiedades anteriores de QnA Maker), GenerateAnswer devolverá la pregunta exacta por identificador en lugar de usar el algoritmo de clasificación para encontrar la respuesta por el texto de la pregunta.


## <a name="display-order-is-supported-in-the-update-api"></a>Update API admite el orden de presentación

El [texto para mostrar y el orden de presentación](https://docs.microsoft.com/rest/api/cognitiveservices/qnamaker/knowledgebase/update#promptdto) que se devuelve en la respuesta JSON, es compatible para su edición con [Update API](https://docs.microsoft.com/rest/api/cognitiveservices/qnamaker/knowledgebase/update).

## <a name="add-or-delete-multi-turn-prompts-with-the-update-api"></a>Incorporación o eliminación de avisos multiturno con Update API

Puede agregar o eliminar avisos multiturno mediante [Update API de QnA Maker](https://docs.microsoft.com/rest/api/cognitiveservices/qnamaker/knowledgebase/update).  Los avisos se agregan en la matriz `promptsToAdd` y `promptsToDelete` de la propiedad `context`.

## <a name="export-knowledge-base-for-version-control"></a>Exportar la base de conocimiento para el control de versiones

QnA Maker admite el control de versiones al incluir pasos de conversación de varios turnos en el archivo exportado.

## <a name="next-steps"></a>Pasos siguientes

Más información acerca de las conversaciones contextuales de este [ejemplo de diálogo](https://github.com/microsoft/BotBuilder-Samples/blob/master/samples/csharp_dotnetcore/adaptive-dialog/07.qnamaker/QnAMaker.csproj) o más información acerca del [diseño de bots conceptuales para conversaciones multiturno](https://docs.microsoft.com/azure/bot-service/bot-builder-conversations?view=azure-bot-service-4.0).

> [!div class="nextstepaction"]
> [Migración de una base de conocimiento](../Tutorials/migrate-knowledge-base.md)
