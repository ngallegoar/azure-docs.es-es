---
title: 'Tutorial: Integración con Power Virtual Agents: QnA Maker'
description: En este tutorial, mejorará la calidad de su base de conocimientos con el aprendizaje activo. Revise, acepte o rechace, o agregue sin quitar ni cambiar las preguntas existentes.
ms.topic: tutorial
ms.date: 03/11/2020
ms.openlocfilehash: 283667c587e395a1d712f82f3385582b4c5c3227
ms.sourcegitcommit: 632e7ed5449f85ca502ad216be8ec5dd7cd093cb
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/30/2020
ms.locfileid: "80398635"
---
# <a name="tutorial-add-knowledge-base-to-power-virtual-agent"></a>Tutorial: Incorporación de una base de conocimiento a Power Virtual Agents
Crea y extienda un bot de [Power Virtual Agents](https://powervirtualagents.microsoft.com/) para ofrecer respuestas desde su base de conocimiento.

**En este tutorial, aprenderá a:**

<!-- green checkmark -->
> [!div class="checklist"]
> * Crear un agente de Power Virtual Agents
> * Crear un tema de reserva del sistema
> * Agregar QnA Maker como acción a un tema como un flujo de Power Automate
> * Crear una solución de Power Automate
> * Agregar un flujo de Power Automate a una solución
> * Publicar un agente de Power Virtual Agents
> * Probar un agente de Power Virtual Agents, recibir una respuesta de la base de conocimiento de QnA Maker

## <a name="integrate-a-power-virtual-agent-with-a-knowledge-base"></a>Integrar un agente de Power Virtual Agents en una base de conocimiento

[Power Virtual Agents](https://powervirtualagents.microsoft.com/) permite a los equipos crear fácilmente bots eficaces mediante una interfaz gráfica guiada, sin código y sin necesidad de científicos de datos ni desarrolladores.

Los agentes de Power Virtual Agents se crean con una serie de temas (áreas temáticas), con el fin de responder a las preguntas de los usuarios mediante la realización de acciones. Si no se puede encontrar una respuesta, una reserva del sistema puede encargarse de devolverla.

Configure el agente para que envíe la pregunta a su base de conocimiento como parte de la acción de un tema o como parte de la ruta de acceso del tema **System Fallback** (Reserva del sistema). Ambos usan el mismo mecanismo de una acción para conectarse a su base de conocimiento y devolver una respuesta.

## <a name="power-automate-connects-to-generateanswer-action"></a>Power Automate se conecta a la acción GenerateAnswer

Para conectar un agente a una base de conocimiento, se usa Power Automate para crear la acción. Power Automate proporciona un **flujo** de procesos que se conecta a GenerateAnswer API de QnA Maker.

Una vez que el **flujo** se ha diseñado y guardado, está disponible en una **solución** de Power Automate.  Una vez que el flujo GenerateAnswer se agrega a una solución, esta se usa como acción en el agente.

## <a name="process-steps-to-connect-an-agent-to-your-knowledge-base"></a>Pasos del proceso para conectar un agente a una base de conocimiento

Los pasos siguientes contienen información general que le ayudará a saber cómo se relacionan los pasos con el objetivo de conectar un agente de Power Virtual Agents a una base de conocimiento de QnA Maker.

Pasos para usar un agente de Power Virtual Agents con QnA Maker:
* En el portal de [QnA Maker](https://www.qnamaker.ai/)
    * Crear y publicar la base de conocimiento
    * Copiar los detalles de la base de conocimiento, lo que incluye el identificador de la base de conocimiento y la clave y el host del punto de conexión del runtime.
* En el portal de [Power Virtual Agents](https://powerva.microsoft.com/)
    * Crear un tema de agente
    * Llamar a una acción (en el flujo de Power Automate)
* En el portal de [Power Automate](https://us.flow.microsoft.com/)
    * Crear un flujo con un conector a [GenerateAnswer de QnA Maker](https://docs.microsoft.com/connectors/cognitiveservicesqnamaker/)
        * Información de la base de conocimiento publicada por QnA Maker
            * Id. de base de conocimiento
            * Host del punto de conexión de recursos de QnA Maker
            * Clave del punto de conexión de recursos de QnA Maker
        * Entrada: consulta del usuario
        * Salida: respuesta de la base de conocimiento
    * Crear una solución y agregar un flujo
* Volver a un agente de Power Virtual Agents
    * Seleccionar la salida de la solución como mensaje para el tema

## <a name="create-and-publish-a-knowledge-base"></a>Crear y publicar una base de conocimiento

1. Siga el [inicio rápido](../Quickstarts/create-publish-knowledge-base.md) para crear una base de conocimiento. No complete la última sección para crear un bot. Este tutorial reemplaza la última sección del inicio rápido, ya que en este tutorial se usa un agente de Power Virtual Agents para crear un bot, en lugar del bot de Bot Framework del inicio rápido.

    > [!div class="mx-imgBorder"]
    > ![Escriba la configuración de la base de conocimiento publicada que se encuentra en la página **Settings** (Configuración) del portal de [QnA Maker] (https://www.qnamaker.ai/).](../media/how-to-integrate-power-virtual-agent/published-knowledge-base-settings.png)

    Necesitará esta información en el [paso de Power Automate](#create-power-automate-flow-to-connect-to-your-knowledge-base), para configurar la conexión de GenerateAnswer de QnA Maker.

1. Busque la clave y el host del punto de conexión, y el identificador de la base de conocimiento en la página **Settings** (Configuración) del portal de QnA Maker.

## <a name="create-power-virtual-agent"></a>Crear un agente de Power Virtual Agents

1. [Inicie sesión](https://go.microsoft.com/fwlink/?LinkId=2108000&clcid=0x409) en Power Virtual Agents con su cuenta de correo electrónico profesional o educativa.
1. Si es su primer bot, debería estar en la página **principal** del agente. Si no es el primer agente de Power Virtual Agents, seleccione el bot en la parte superior derecha del panel de navegación y, después, seleccione **+ New Bot** (+ Nuevo bot).

    > [!div class="mx-imgBorder"]
    > ![Escriba la configuración de la base de conocimiento publicada que se encuentra en la página **Settings** (Configuración) del portal de [QnA Maker] (https://www.qnamaker.ai/).](../media/how-to-integrate-power-virtual-agent/power-virtual-agent-home.png)

## <a name="several-topics-are-provided-in-the-bot"></a>En el bot se proporcionan varios temas

El agente usa la colección de temas para responder las preguntas sobre una materia. En este tutorial, el agente tiene muchos temas, que se dividen en **Temas de usuario** y **Temas del sistema**.

> [!div class="mx-imgBorder"]
> ![El agente usa la colección de temas para responder las preguntas sobre una materia. En este tutorial, el agente tiene muchos temas, que se dividen en **Temas de usuario** y **Temas del sistema**.](../media/how-to-integrate-power-virtual-agent/power-virtual-agent-topics-provided.png)


## <a name="create-power-virtual-agents-system-fallback-topic"></a>Creación de un tema de reserva del sistema del agente de Power Virtual Agents

Aunque el agente puede conectarse a su base de conocimiento desde cualquier tema, en este tutorial se usa el tema **System Fallback** (Reserva del sistema). El tema de reserva se usa cuando el agente no encuentra una respuesta. El agente pasa el texto del usuario a GenerateAnswer API de QnA Maker, recibe la respuesta de la base de conocimiento y se la muestra al usuario en forma de mensaje.

1. En el portal de [Power Virtual Agents](https://powerva.microsoft.com/#/), en la esquina superior derecha del panel de navegación, seleccione la página **Settings** (Configuración). El icono de esta página es el engranaje. Seleccione **System Fallback** (Reserva del sistema).

    > [!div class="mx-imgBorder"]
    > ![Elemento de menú del agente de Power Virtual Agents para reserva del sistema](../media/how-to-integrate-power-virtual-agent/power-virtual-agent-settings-system-fallback.png)

1. En la ventana emergente **Settings** (Configuración), seleccione **+ Add** (+ Agregar) para agregar un tema de reserva del sistema.

    > [!div class="mx-imgBorder"]
    > ![En la ventana de configuración, agregue un tema de reserva.](../media/how-to-integrate-power-virtual-agent/power-virtual-agent-settings-add-fallback-topic.png)

1. Una vez agregado el tema, seleccione **Go to Fallback topic** (Ir a tema de reserva) para crear el tema de reserva en el lienzo de creación.

    > [!TIP]
    > Si necesita volver a este tema, está disponible en la sección **Topics** (Temas), como parte de los temas **System** (Sistema).

## <a name="use-authoring-canvas-to-add-an-action"></a>Uso de un lienzo de creación para agregar una acción

Use el lienzo de creación de Power Virtual Agents para conectar el tema de reserva con la base de conocimiento. El tema comienza con el **texto de usuario no reconocido**. Agregue una acción que pase dicho texto a QnA Maker y, después, muestre la respuesta en forma de mensaje. El último paso para mostrar una respuesta se tratará como un [paso independiente](#add-solutions-flow-to-power-virtual-agent) más adelante en el tutorial.

En esta sección se crea un flujo de conversación del tema de reserva.

1. Es posible que la nueva acción de reserva ya tenga elementos del flujo de conversación. Elimine el elemento **Escalate** (Escalar), para lo que debe seleccionar el menú Options (Opciones).

    > [!div class="mx-imgBorder"]
    > ![Iniciar la acción de reserva con las fases del desencadenador](../media/how-to-integrate-power-virtual-agent/power-virtual-agent-fallback-topic-delete-escalate.png)

1. Seleccione el conector **+** que fluye desde el cuadro **Message** (Mensaje) y, después, seleccione **Call an action** (Llamar a una acción).

    > [!div class="mx-imgBorder"]
    > ![Call an action](../media/how-to-integrate-power-virtual-agent/create-new-item-call-an-action.png) (Llamar a una acción)

1. Seleccione **Create a flow** (Crear un flujo). El proceso le lleva a **Power Automate**, un portal diferente basado en explorador.

    > [!div class="mx-imgBorder"]
    > ![Call an action](../media/how-to-integrate-power-virtual-agent/create-a-flow.png) (Llamar a una acción)

## <a name="create-power-automate-flow-to-connect-to-your-knowledge-base"></a>Creación de un flujo de Power Automate para conectarse a una base de conocimiento

En el procedimiento siguiente se crea un flujo de **Power Automate** que:
* toma el texto de usuario de entrada;
* lo envía a QnA Maker;
* asigna la respuesta principal de QnA Maker a una variable;
* devuelve la variable (respuesta principal) como respuesta al agente.

1. En **Power Automate**, la plantilla **Flow Template** se inicia automáticamente. En el elemento del flujo **Power Virtual Agents**, seleccione **Edit** (Editar) para configurar la variable de entrada desde el agente a la base de conocimiento. La variable de entrada basada en texto es la pregunta en forma de texto enviada por el usuario de su agente.

    > [!div class="mx-imgBorder"]
    > ![Configure su variable de entrada como si fuera una cadena de texto](../media/how-to-integrate-power-virtual-agent/power-automate-configure-input-variable.png)

1. Agregue una entrada de texto y asigne a la variable el nombre `InputText` con una descripción de `IncomingUserQuestion`. Esta nomenclatura ayuda a distinguir el texto de entrada del texto de salida que creará más adelante.

    > [!div class="mx-imgBorder"]
    > ![Agregue una entrada de texto y asigne a la variable el nombre "InputText" con una descripción de "UserQuestion"](../media/how-to-integrate-power-virtual-agent/power-automate-configure-input-variable-name-and-description.png).

1. Seleccione el conector **+** que sale del cuadro **Power Virtual Agents** para insertar un paso nuevo en el flujo, antes de **Return value(s) to Power Virtual Agent** (Devolver valores a Power Virtual Agents), y seleccione **Add an action** (Agregar una acción).

1. Busque `Qna` para encontrar las acciones de **QnA Maker** y, después, seleccione **Generate answer** (Generar respuesta).

    > [!div class="mx-imgBorder"]
    > ![Busque "Qna" para encontrar las acciones de **QnA Maker** y, después, seleccione **Generate answer** (Generar respuesta)](../media/how-to-integrate-power-virtual-agent/generate-answer-action-selected.png).

    Los tres valores de conexión necesarios para QnA Maker aparecen en la configuración de la acción y la pregunta del agente de Power Virtual Agents.

    > [!div class="mx-imgBorder"]
    > ![La configuración de las conexiones de QnA Maker aparecen en la acción.](../media/how-to-integrate-power-virtual-agent/generate-answer-knowledge-base-settings.png)

1. Configure la acción con el identificador de la base de conocimiento, así como con el host y la clave del punto de conexión. Estos valores se encuentran en la página **Settings** (Configuración) de la base de conocimiento, en el portal de QnA Maker.

    > [!div class="mx-imgBorder"]
    > ![Escriba la configuración de la base de conocimiento publicada que se encuentra en la página **Settings** (Configuración) del portal de [QnA Maker] (https://www.qnamaker.ai/).](../media/how-to-integrate-power-virtual-agent/published-knowledge-base-settings.png)

1. Para configurar el valor de **Question** (Pregunta), seleccione el cuadro de texto y, después, seleccione el valor de `InputText` en la lista.

1. Para insertar un nuevo paso en el flujo, seleccione el conector **+** que fluye desde el cuadro de la acción **Generate answer** (Generar respuesta) y, después, seleccione **Add an action** (Agregar una acción).

1. Para agregar una variable para capturar el texto de respuesta que devuelve GenerateAnswer, busque la acción `Initialize variable` y selecciónela.

    Establezca el nombre de la variable en `OutgoingQnAAnswer` y seleccione el tipo **String** (Cadena). No establezca **Value** (Valor).

    > [!div class="mx-imgBorder"]
    > ![Establezca el nombre de la variable en "QnAAnswer" y seleccione el tipo **String** (Cadena)](../media/how-to-integrate-power-virtual-agent/initialize-output-variable-for-qna-answer.png)

1. Para insertar un nuevo paso en el flujo, seleccione el conector **+** que fluye desde el cuadro de la acción **Initialize variable** (Inicializar variable) y, después, seleccione **Add an action** (Agregar una acción).

1. Para establecer toda la respuesta JSON de la base de conocimiento en la variable, busque la acción `Apply to each` y selecciónela. Seleccione el objeto `answers` de GenerateAnswer.

1. Para devolver solo la principal respuesta, en el mismo cuadro **Apply to each** (Aplicar a cada uno), seleccione **Add an action** (Agregar una acción). Busque y seleccione **Set variable** (Establecer variable).

    En el cuadro **Set variable** (Establecer variable), seleccione el cuadro de texto **Name** (Nombre) y, luego, seleccione **OutgoingQnAAnswer** en la lista.

    Seleccione el cuadro de texto **Value** (Valor) y, después, seleccione **Answers Answer** (Responde la respuesta) en la lista.

    > [!div class="mx-imgBorder"]
    > ![Establecer el nombre y valor de la variable ](../media/how-to-integrate-power-virtual-agent/power-automate-flow-apply-to-each-set-variable.png)

1. Para devolver la variable y su valor, seleccione el elemento del flujo **Return value(s) to Power Virtual Agent** (Devolver valores a Power Virtual Agents), seleccione **Edit** (Editar) y, luego, **Add an output** (Agregar una salida). Seleccione el tipo de salida **Text** (Texto) y, en **Title** (Título), especifique `FinalAnswer`. Seleccione el cuadro de texto **Value** (Valor) y, después, seleccione la variable `OutgoingQnAAnswer`.

    > [!div class="mx-imgBorder"]
    > ![Establecer el valor devuelto](../media/how-to-integrate-power-virtual-agent/power-automate-flow-return-value.png)

1. Seleccione **Save** (Guardar) para guardar el flujo.

## <a name="create-solution-and-add-flow"></a>Creación de una solución e incorporación de un flujo

Para que el agente de Power Virtual Agents encuentre el flujo y conecte con él, este debe estar incluido en una solución de Power Automate.

1. En el portal de Power Automate, seleccione **Solutions** (Soluciones) en el panel de navegación izquierdo.

1. Seleccione **+ New solution** (+ Nueva solución).

1. Escriba un nombre para mostrar. La lista de soluciones incluye todas y cada una de las soluciones de su organización o centro educativo. Elija una convención de nomenclatura que le ayude a filtrar de tal forma que aparezcan solo sus soluciones, como anteponer en el correo electrónico el nombre de la solución: `jondoe-power-virtual-agent-qnamaker-fallback`.

1. Seleccione su editor en la lista de opciones.

1. Acepte los valores predeterminados del nombre y la versión.

1. Seleccione **Create** (Crear) para finalizar el proceso.

## <a name="add-flow-to-solution"></a>Incorporación de un flujo a una solución

1. En la lista de soluciones, seleccione la que acaba de crear. Debería estar la primera de la lista. Si no lo está, busque por nombre de correo electrónico, que es parte del nombre de la solución.

1. En la solución, seleccione **+ Add existing** (+ Agregar existente) y, luego, **Flow** (Flujo) en la lista.

1. Busque su flujo y seleccione **Add** (Agregar) para finalizar el proceso. Si hay muchos flujos, examine la columna **Modified** (Modificado) para encontrar el flujo más reciente.

## <a name="add-solutions-flow-to-power-virtual-agent"></a>Incorporación del flujo de una solución a Power Virtual Agents

1. Vuelva a la pestaña del explorador que contiene el agente de Power Virtual Agents. El lienzo de creación debería estar aún abierto.

1. Seleccione el conector **+** en el cuadro de la acción **Message** (Mensaje) para insertar un nuevo paso en el flujo y seleccione **Call an action** (Llamar a una acción).

1. En la nueva acción, seleccione el valor de entrada de **UnrecognizedTriggerPhrase**. Así se pasa el texto del agente al flujo.

    > [!div class="mx-imgBorder"]
    > ![En la nueva acción, seleccione el valor de entrada de **UnrecognizedTriggerPhrase**.](../media/how-to-integrate-power-virtual-agent/power-virtual-agent-select-unrecognized-trigger-phrase.png)

1. Seleccione el conector **+** en el cuadro de la acción **Action** (Acción) para insertar un nuevo paso en el flujo y seleccione **Show a message** (Mostrar un mensaje).

1. Escriba el texto del mensaje `Your answer is:` y seleccione `FinalAnswer` como variable de contexto que usa la función de la barra de herramientas en vigor.

    > [!div class="mx-imgBorder"]
    > ![Escriba el texto del mensaje y "FinalAnswer" desde el flujo de Power Automate.](../media/how-to-integrate-power-virtual-agent/power-virtual-agent-topic-authoring-canvas-show-message-final-answer.png)

1. Seleccione **Save** (Guardar) en la barra de herramientas contextual para guardar los detalles del lienzo del tema.

A continuación, se muestra el lienzo final.

> [!div class="mx-imgBorder"]
> ![Lienzo del agente final](../media/how-to-integrate-power-virtual-agent/power-virtual-agent-topic-authoring-canvas-full-flow.png)

## <a name="test-power-virtual-agent"></a>Prueba del agente de Power Virtual Agents

1. En el panel del prueba, alterne **Track between topics** (Seguimiento entre temas). Esto le permite ver la progresión tanto entre los temas como en un tema individual.

1. Pruebe el agente; para ello, escriba el texto del usuario en el orden que se indica a continuación. En el lienzo de creación, los pasos correctos tienen una marca de comprobación verde.

|Orden de pregunta|Preguntas de prueba|Propósito|
|--|--|--|
|1|Hola|Empezar conversación|
|2|Horario de la tienda|Tema de ejemplo: configurado automáticamente, por lo que no necesita que haga nada más.|
|3|Sí|En respuesta a `Did that answer your question?`|
|4|Excelente|En respuesta a `Please rate your experience.`|
|5|Sí|En respuesta a `Can I help with anything else?`|
|6|¿Qué es una base de conocimiento?|Esta pregunta desencadena la acción de reserva, que envía el texto a la base de conocimiento para responder y, después, se muestra la respuesta. |

> [!div class="mx-imgBorder"]
> ![Lienzo del agente final](../media/how-to-integrate-power-virtual-agent/power-virtual-agent-test-tracked.png)

## <a name="publish-your-bot"></a>Publicar el bot

Para que el agente esté disponible para todos los miembros del centro educativo o de la organización, es preciso que lo publique.

1. Seleccione **Publish** (Publicar) en el panel de navegación izquierdo y, después, seleccione **Publish** (Publicar) en la página.

1. Pruebe el bot en un sitio web de ejemplo, que se proporciona en forma de vínculo debajo del botón **Publish** (Publicar).

    Se abre una nueva página web con su bot. Realice la misma pregunta de prueba al bot: `What is a knowledge base?`

    > [!div class="mx-imgBorder"]
    > ![Lienzo del agente final](../media/how-to-integrate-power-virtual-agent/demo-chat-bot.png)

## <a name="share-your-bot"></a>Uso compartido de un bot

Para compartir el sitio web de demostración, configúrelo como si fuera un canal.

1. Seleccione **Manage** (Administrar) y luego **Channels** (Canales) en el panel de navegación izquierdo.

1. Seleccione **Demo website** (Sitio web de demostración) en la lista de canales.

1. Copie el vínculo y seleccione **Save** (Guardar). Pegue el vínculo al sitio web de demostración en un correo electrónico dirigido a los miembros de su organización o centro educativo.

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando haya terminado con la base de conocimiento, elimine los recursos de QnA Maker en Azure Portal.

## <a name="next-step"></a>Paso siguiente

[Obtener análisis en la base de conocimiento](../How-To/get-analytics-knowledge-base.md)

Más información sobre:
* [Power Virtual Agents](https://docs.microsoft.com/power-virtual-agents/)
* [Power Automate](https://docs.microsoft.com/power-automate/)
* [Conector de QnA Maker](https://us.flow.microsoft.com/connectors/shared_cognitiveservicesqnamaker/qna-maker/) y la [configuración del conector](https://docs.microsoft.com/connectors/cognitiveservicesqnamaker/)