---
title: 'Tutorial: Integración con Power Virtual Agents: QnA Maker'
titleSuffix: Azure Cognitive Services
description: En este tutorial, mejorará la calidad de su base de conocimientos con el aprendizaje activo. Revise, acepte, rechace las preguntas existentes, o agregue nuevas sin quitar ni cambiar las existentes.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: tutorial
ms.date: 03/11/2020
ms.author: diberry
ms.openlocfilehash: 4557dee995c8a01067f7e6ad0e79bb7115b6ecdb
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/16/2020
ms.locfileid: "81402812"
---
# <a name="tutorial-add-your-knowledge-base-to-power-virtual-agents"></a>Tutorial: Incorporación de una base de conocimiento a Power Virtual Agents
Crea y extienda un bot de [Power Virtual Agents](https://powervirtualagents.microsoft.com/) para ofrecer respuestas desde su base de conocimiento.

En este tutorial, aprenderá a:

<!-- green checkmark -->
> [!div class="checklist"]
> * Crear un bot de Power Virtual Agents
> * Crear un tema de reserva del sistema
> * Agregar QnA Maker como acción a un tema en forma de flujo de Power Automate
> * Crear una solución de Power Automate
> * Agregar un flujo de Power Automate a la solución
> * Publicar agentes de Power Virtual Agents
> * Probar Power Virtual Agents y recibir una respuesta de la base de conocimiento de QnA Maker

## <a name="integrate-an-agent-with-a-knowledge-base"></a>Integración de un agente con una base de conocimiento

[Power Virtual Agents](https://powervirtualagents.microsoft.com/) permite a los equipos crear bots eficaces mediante una interfaz gráfica guiada y sin código. No necesita científicos ni desarrolladores de datos.

En Power Virtual Agents, un agente se crea con una serie de temas (áreas temáticas), con el fin de responder a las preguntas de los usuarios mediante la realización de acciones. Si no se puede encontrar una respuesta, una reserva del sistema puede encargarse de devolverla.

Configure el agente para que envíe la pregunta a su base de conocimiento como parte de la acción de un tema o como parte de la ruta del tema *System Fallback* (Reserva del sistema). Ambos usan una acción para conectarse a la base de conocimiento y devolver una respuesta.

## <a name="power-automate-connects-to-generateanswer-action"></a>Power Automate se conecta a la acción `GenerateAnswer`

Para conectar un agente a una base de conocimiento, se usa Power Automate para crear la acción. Power Automate proporciona un flujo de procesos que se conecta a la API `GenerateAnswer` de QnA Maker.

Después de diseñar el flujo y guardarlo, está disponible desde cualquier solución de Power Automate. Use esa solución como una acción en el agente.

## <a name="connect-an-agent-to-your-knowledge-base"></a>Conexión de un agente a la base de conocimiento

Esta es una introducción a los pasos para conectar un agente de Power Virtual Agents a una base de conocimiento en QnA Maker.

* En el portal de [QnA Maker](https://www.qnamaker.ai/):
    * Se crea y publica la base de conocimiento.
    * Se copian los detalles de la base de conocimiento, lo que incluye el identificador, y la clave y el host del punto de conexión del entorno de ejecución.
* En el portal de [Power Virtual Agents](https://powerva.microsoft.com/):
    * Se crea un tema del agente.
    * Se llama a una acción (en el flujo de Power Automate).
* En el portal de [Power Automate](https://us.flow.microsoft.com/):
    * Se crea un flujo con un conector a [GenerateAnswer de QnA Maker](https://docs.microsoft.com/connectors/cognitiveservicesqnamaker/).
        * Información de la base de conocimiento publicada por QnA Maker:
            * Id. de base de conocimiento
            * Host del punto de conexión de recursos de QnA Maker
            * Clave del punto de conexión de recursos de QnA Maker
        * Entrada: consulta del usuario
        * Salida: respuesta de la base de conocimiento
    * Se crea una solución y se agrega el flujo.
* De vuelta a Power Virtual Agents:
    * Se selecciona la salida de la solución como un mensaje para un tema.

## <a name="create-and-publish-a-knowledge-base"></a>Crear y publicar una base de conocimiento

1. Siga el [inicio rápido](../Quickstarts/create-publish-knowledge-base.md) para crear una base de conocimiento. No complete la última sección sobre la creación de un bot. En su lugar, use este tutorial para crear un bot con Power Virtual Agents.

    > [!div class="mx-imgBorder"]
    > ![Captura de pantalla de la configuración de la base de conocimiento publicada](../media/how-to-integrate-power-virtual-agent/published-knowledge-base-settings.png)

    Escriba la configuración de la base de conocimiento publicada que se encuentra en la página **Settings** (Configuración) del portal de [QnA Maker](https://www.qnamaker.ai/). Necesitará esta información en el [paso de Power Automate](#create-a-power-automate-flow-to-connect-to-your-knowledge-base), para configurar la conexión de `GenerateAnswer` de QnA Maker.

1. En el portal de QnA Maker, en la página **Settings** (Configuración), busque la clave y el host del punto de conexión, y el identificador de la base de conocimiento.

## <a name="create-an-agent-in-power-virtual-agents"></a>Creación de un agente en Power Virtual Agents

1. [Inicie sesión en Power Virtual Agents](https://go.microsoft.com/fwlink/?LinkId=2108000&clcid=0x409). Use su cuenta de correo electrónico profesional o educativa.
1. Si este es su primer bot, debería estar en la página **principal** del agente. Si no, seleccione el bot en el área superior derecha de la página y, después, seleccione **+ New Bot** (+ Nuevo bot).

    > [!div class="mx-imgBorder"]
    > ![Captura de pantalla de la página principal de Power Virtual Agents](../media/how-to-integrate-power-virtual-agent/power-virtual-agent-home.png)

1. Escriba la configuración de la base de conocimiento publicada que se encuentra en la página **Settings** (Configuración) del portal de [QnA Maker](https://www.qnamaker.ai/).

## <a name="topics-provided-in-the-bot"></a>Temas incluidos en el bot

El agente usa la colección de temas para responder las preguntas sobre una materia. En este tutorial, el agente tiene muchos temas, que se dividen en temas de usuario y temas del sistema.

> [!div class="mx-imgBorder"]
> ![Captura de pantalla de los temas proporcionados en el agente](../media/how-to-integrate-power-virtual-agent/power-virtual-agent-topics-provided.png)


## <a name="create-the-system-fallback-topic"></a>Creación de un tema de reserva del sistema

Aunque el agente puede conectarse a su base de conocimiento desde cualquier tema, en este tutorial se usa el tema *System Fallback* (Reserva del sistema). El tema de reserva se usa cuando el agente no encuentra una respuesta. El agente pasa el texto del usuario a la API `GenerateAnswer` de QnA Maker, recibe la respuesta de la base de conocimiento y se la muestra al usuario en forma de mensaje.

1. En el portal de [Power Virtual Agents](https://powerva.microsoft.com/#/), en la esquina superior derecha, seleccione **Settings** (Configuración) (el icono de engranaje). Después, seleccione **System Fallback** (Reserva del sistema).

    > [!div class="mx-imgBorder"]
    > ![Captura de pantalla del elemento de menú de Power Virtual Agents para la reserva del sistema](../media/how-to-integrate-power-virtual-agent/power-virtual-agent-settings-system-fallback.png)

1. Seleccione **+ Add** (+ Agregar) para agregar un tema de reserva del sistema.

    > [!div class="mx-imgBorder"]
    > ![Captura de pantalla de la incorporación de un tema de reserva.](../media/how-to-integrate-power-virtual-agent/power-virtual-agent-settings-add-fallback-topic.png)

1. Una vez agregado el tema, seleccione **Go to Fallback topic** (Ir a tema de reserva) para crear el tema de reserva en el lienzo de creación.

    > [!TIP]
    > Si necesita volver a este tema, está disponible en la sección **Topics** (Temas), como parte de los temas **System** (Sistema).

## <a name="use-the-authoring-canvas-to-add-an-action"></a>Uso de un lienzo de creación para agregar una acción

Use el lienzo de creación de Power Virtual Agents para conectar el tema de reserva con la base de conocimiento. El tema comienza con el texto de usuario no reconocido. Agregue una acción que pase dicho texto a QnA Maker y, después, muestre la respuesta en forma de mensaje. El último paso para mostrar una respuesta se tratará como un [paso independiente](#add-your-solutions-flow-to-power-virtual-agents), más adelante en el tutorial.

En esta sección se crea un flujo de conversación del tema de reserva.

1. Es posible que la nueva acción de reserva tenga elementos del flujo de conversación. Elimine el elemento **Escalate** (Escalar), para lo que debe seleccionar el menú **Options** (Opciones).

    > [!div class="mx-imgBorder"]
    > ![Captura de pantalla del tema de reserva de Power Virtual Agents](../media/how-to-integrate-power-virtual-agent/power-virtual-agent-fallback-topic-delete-escalate.png)

1. Seleccione el conector **+** que fluye desde el cuadro **Message** (Mensaje) y, después, seleccione **Call an action** (Llamar a una acción).

    > [!div class="mx-imgBorder"]
    > ![Captura de pantalla de la llamada a una acción](../media/how-to-integrate-power-virtual-agent/create-new-item-call-an-action.png)

1. Seleccione **Create a flow** (Crear un flujo). El proceso le llevará al portal de Power Automate.

    > [!div class="mx-imgBorder"]
    > ![Captura de pantalla de la creación de un flujo](../media/how-to-integrate-power-virtual-agent/create-a-flow.png)

## <a name="create-a-power-automate-flow-to-connect-to-your-knowledge-base"></a>Creación de un flujo de Power Automate para conectarse a una base de conocimiento

En el procedimiento siguiente se crea un flujo de Power Automate que:
* Toma el texto de usuario de entrada y lo envía a QnA Maker.
* Asigna la respuesta principal de QnA Maker a una variable y envía esta (respuesta principal) de vuelta al agente.

1. En **Power Automate**, la plantilla **Flow Template** se inicia automáticamente. En el elemento del flujo **Power Virtual Agents**, seleccione **Edit** (Editar) para configurar la variable de entrada desde el agente a la base de conocimiento. La variable de entrada basada en texto es la pregunta en forma de texto enviada por el usuario de su agente.

    > [!div class="mx-imgBorder"]
    > ![Captura de pantalla de la opción de Power Automate para configurar la variable de entrada como una cadena de texto](../media/how-to-integrate-power-virtual-agent/power-automate-configure-input-variable.png)

1. Agregue una entrada de texto y asigne a la variable el nombre `InputText` con la descripción `IncomingUserQuestion`. Esta nomenclatura ayuda a distinguir el texto de entrada del texto de salida que creará más adelante.

    > [!div class="mx-imgBorder"]
    > ![Captura de pantalla de la opción de Power Automate para configurar el nombre y la descripción de la variable de entrada](../media/how-to-integrate-power-virtual-agent/power-automate-configure-input-variable-name-and-description.png)

1. Seleccione el conector **+** que sale del cuadro **Power Virtual Agents** para insertar un paso nuevo en el flujo (antes de **Return value(s) to Power Virtual Agent** [Devolver valores a Power Virtual Agents]). Después, seleccione **Add an action** (Agregar una acción).

1. Busque `Qna` para encontrar las acciones de **QnA Maker** y, después, seleccione **Generate answer** (Generar respuesta).

    > [!div class="mx-imgBorder"]
    > ![Captura de pantalla de la acción para generar respuesta](../media/how-to-integrate-power-virtual-agent/generate-answer-action-selected.png)

    Los valores de conexión necesarios para QnA Maker aparecen en la configuración de la acción y la pregunta del agente.

    > [!div class="mx-imgBorder"]
    > ![Captura de pantalla de la configuración de conexión necesaria](../media/how-to-integrate-power-virtual-agent/generate-answer-knowledge-base-settings.png)

1. Configure la acción con el identificador de la base de conocimiento, así como con el host y la clave del punto de conexión. Estos valores se encuentran en la página **Settings** (Configuración) de la base de conocimiento, en el portal de QnA Maker.

    > [!div class="mx-imgBorder"]
    > ![Captura de pantalla de la configuración de la base de conocimiento publicada](../media/how-to-integrate-power-virtual-agent/published-knowledge-base-settings.png)

1. Para configurar el valor de **Question** (Pregunta), seleccione el cuadro de texto y, después, seleccione el valor de `InputText` en la lista.

1. Para insertar un nuevo paso en el flujo, seleccione el conector **+** que fluye desde el cuadro de la acción **Generate answer** (Generar respuesta). Después, seleccione **Add an action** (Agregar una acción).

1. Para agregar una variable para capturar el texto de respuesta que `GenerateAnswer` devuelve, busque la acción `Initialize variable` y selecciónela.

    Establezca el nombre de la variable en `OutgoingQnAAnswer` y seleccione el tipo **String** (Cadena). No establezca **Value** (Valor).

    > [!div class="mx-imgBorder"]
    > ![Captura de pantalla de la inicialización de la variable de salida](../media/how-to-integrate-power-virtual-agent/initialize-output-variable-for-qna-answer.png)

1. Para insertar un nuevo paso en el flujo, seleccione el conector **+** que fluye desde el cuadro de la acción **Initialize variable** (Inicializar variable). Después, seleccione **Add an action** (Agregar una acción).

1. Para establecer toda la respuesta JSON de la base de conocimiento en la variable, busque la acción `Apply to each` y selecciónela. Seleccione las `answers` `GenerateAnswer`.

1. Para devolver solo la principal respuesta, en el mismo cuadro **Apply to each** (Aplicar a cada uno), seleccione **Add an action** (Agregar una acción). Busque y seleccione **Set variable** (Establecer variable).

    En el cuadro **Set variable** (Establecer variable), seleccione el cuadro de texto **Name** (Nombre) y, luego, seleccione **OutgoingQnAAnswer** en la lista.

    Seleccione el cuadro de texto **Value** (Valor) y, después, seleccione **Answers Answer** (Responde la respuesta) en la lista.

    > [!div class="mx-imgBorder"]
    > ![Captura de pantalla del establecimiento del nombre y el valor de la variable](../media/how-to-integrate-power-virtual-agent/power-automate-flow-apply-to-each-set-variable.png)

1. Para devolver la variable y su valor, seleccione el elemento del flujo **Return value(s) to Power Virtual Agent** (Devolver valores a Power Virtual Agents). A continuación, seleccione **Edit** > **Add an output** (Editar > Agregar una salida). Seleccione el tipo de salida **Text** (Texto) y, en **Title** (Título), especifique `FinalAnswer`. Seleccione el cuadro de texto **Value** (Valor) y, después, seleccione la variable `OutgoingQnAAnswer`.

    > [!div class="mx-imgBorder"]
    > ![Captura de pantalla del establecimiento del valor devuelto](../media/how-to-integrate-power-virtual-agent/power-automate-flow-return-value.png)

1. Seleccione **Guardar** para guardar el flujo.

## <a name="create-a-solution-and-add-the-flow"></a>Creación de una solución e incorporación del flujo

Para que el agente encuentre el flujo y se conecte a él, este debe estar incluido en una solución de Power Automate.

1. En el portal de Power Automate, seleccione **Solutions** (Soluciones) en el panel de navegación izquierdo.

1. Seleccione **+ New solution** (+ Nueva solución).

1. Escriba un nombre para mostrar. La lista de soluciones incluye todas y cada una de las soluciones de su organización o centro educativo. Elija una Convención de nomenclatura que le ayude a filtrar solo por sus soluciones. Por ejemplo, puede anteponer el correo electrónico al nombre de la solución: `jondoe-power-virtual-agent-qnamaker-fallback`.

1. Seleccione su editor en la lista de opciones.

1. Acepte los valores predeterminados del nombre y la versión.

1. Seleccione **Create** (Crear) para finalizar el proceso.

## <a name="add-your-flow-to-the-solution"></a>Incorporación del flujo a la solución

1. En la lista de soluciones, seleccione la que acaba de crear. Debería estar la primera de la lista. Si no lo está, busque por nombre de correo electrónico, que es parte del nombre de la solución.

1. En la solución, seleccione **+ Add existing** (+ Agregar existente) y, luego, **Flow** (Flujo) en la lista.

1. Busque su flujo y seleccione **Add** (Agregar) para finalizar el proceso. Si hay muchos flujos, examine la columna **Modified** (Modificado) para encontrar el flujo más reciente.

## <a name="add-your-solutions-flow-to-power-virtual-agents"></a>Incorporación del flujo de una solución a Power Virtual Agents

1. Vuelva a la pestaña del explorador con el agente en Power Virtual Agents. El lienzo de creación debería estar aún abierto.

1. Para insertar un nuevo paso en el flujo, en el cuadro de acción **Message** (Mensaje), seleccione el conector **+** . A continuación, seleccione **Call an action** (Llamar a una acción).

1. En la nueva acción, seleccione el valor de entrada de **UnrecognizedTriggerPhrase**. Así se pasa el texto del agente al flujo.

    > [!div class="mx-imgBorder"]
    > ![Captura de pantalla de la opción de Power Virtual Agents para seleccionar una frase de desencadenador no reconocida](../media/how-to-integrate-power-virtual-agent/power-virtual-agent-select-unrecognized-trigger-phrase.png)

1. Para insertar un paso nuevo en el flujo, en el cuadro **Action** (Acción), seleccione el conector **+** . A continuación, seleccione **Show a message** (Mostrar un mensaje).

1. Escriba el texto del mensaje, `Your answer is:`. Seleccione `FinalAnswer` como una variable de contexto mediante la función de la barra de herramientas en contexto.

    > [!div class="mx-imgBorder"]
    > ![Captura de pantalla de la opción de Power Virtual Agents para escribir el texto del mensaje](../media/how-to-integrate-power-virtual-agent/power-virtual-agent-topic-authoring-canvas-show-message-final-answer.png)

1. En la barra de herramientas contextual, seleccione **Save** (Guardar) a fin de guardar los detalles del lienzo de creación para el tema.

Este es el aspecto final del lienzo del agente.

> [!div class="mx-imgBorder"]
> ![Captura de pantalla del lienzo final del agente](../media/how-to-integrate-power-virtual-agent/power-virtual-agent-topic-authoring-canvas-full-flow.png)

## <a name="test-the-agent"></a>Prueba del agente

1. En el panel del prueba, alterne **Track between topics** (Seguimiento entre temas). De este modo puede ver la progresión tanto entre los temas como en un tema individual.

1. Pruebe el agente; para ello, escriba el texto del usuario en el orden siguiente. En el lienzo de creación, los pasos correctos tienen una marca de comprobación verde.

    |Orden de pregunta|Preguntas de prueba|Propósito|
    |--|--|--|
    |1|Hola|Empezar conversación|
    |2|Horario de la tienda|Tema de ejemplo. Se configura automáticamente, por lo que no tiene que hacer nada más.|
    |3|Sí|En respuesta a `Did that answer your question?`|
    |4|Excelente|En respuesta a `Please rate your experience.`|
    |5|Sí|En respuesta a `Can I help with anything else?`|
    |6|¿Qué es una base de conocimiento?|Esta pregunta desencadena la acción de reserva, que envía el texto a la base de conocimiento para responder. A continuación, se muestra la respuesta. |

> [!div class="mx-imgBorder"]
> ![Captura de pantalla del lienzo final del agente](../media/how-to-integrate-power-virtual-agent/power-virtual-agent-test-tracked.png)

## <a name="publish-your-bot"></a>Publicar el bot

Para que el agente esté disponible para todos los miembros del centro educativo o de la organización, es preciso que lo publique.

1. En el panel de navegación de la izquierda, seleccione **Publish** (Publicar). A continuación, seleccione **Publish** (Publicar) en la página.

1. Pruebe su bot en el sitio web de demostración (busque el vínculo en **Publish** [Publicar]).

    Se abre una nueva página web con su bot. Realice la misma pregunta de prueba al bot: `What is a knowledge base?`

    > [!div class="mx-imgBorder"]
    > ![Captura de pantalla del lienzo final del agente](../media/how-to-integrate-power-virtual-agent/demo-chat-bot.png)

## <a name="share-your-bot"></a>Uso compartido de un bot

Para compartir el sitio web de demostración, configúrelo como si fuera un canal.

1. En el panel de navegación de la izquierda, seleccione **Manage** > **Channels** (Administrar > Canales).

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