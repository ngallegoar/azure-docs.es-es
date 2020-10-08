---
title: 'Tutorial: Integración con Power Virtual Agents: QnA Maker'
description: En este tutorial, mejorará la calidad de su base de conocimientos con el aprendizaje activo. Revise, acepte, rechace las preguntas existentes, o agregue nuevas sin quitar ni cambiar las existentes.
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: tutorial
ms.date: 06/08/2020
ms.openlocfilehash: 42b50fcf0df27ddbc3e587a7d8e038e4979935ae
ms.sourcegitcommit: ef69245ca06aa16775d4232b790b142b53a0c248
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/06/2020
ms.locfileid: "91777419"
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
    * Busque la plantilla _Generate answer using QnA Maker_ (Generar respuesta con QnA Maker).
    * Use la plantilla para configurar el flujo para usar [GenerateAnswer de QnA Maker](https://docs.microsoft.com/connectors/cognitiveservicesqnamaker/).
        * Información de la base de conocimiento publicada por QnA Maker:
            * Id. de base de conocimiento
            * Host del punto de conexión de recursos de QnA Maker
            * Clave del punto de conexión de recursos de QnA Maker
        * Entrada: consulta del usuario
        * Salida: respuesta de la base de conocimiento
    * Cree una solución y agregue el flujo, o bien agregue el flujo a la solución existente.
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

## <a name="topics-provided-in-the-bot"></a>Temas incluidos en el bot

El agente usa la colección de temas para responder las preguntas sobre una materia. En este tutorial, el agente tiene muchos temas, que se dividen en temas de usuario y temas del sistema.

Seleccione **Temas** en el panel de navegación izquierdo para ver los temas que proporciona el bot.

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

    :::image type="content" source="../media/how-to-integrate-power-virtual-agent/delete-escalate-action-using-option-menu.png" alt-text="Captura de pantalla parcial del flujo de conversación, con la opción Eliminar resaltada.":::

1. Seleccione el conector **+** que fluye al cuadro **Mensaje** y, después, seleccione **Call an action** (Llamar a una acción).

    :::image type="content" source="../media/how-to-integrate-power-virtual-agent/create-new-item-call-an-action.png" alt-text="Captura de pantalla parcial del flujo de conversación, con la opción Eliminar resaltada.":::

1. Seleccione **Create a flow** (Crear un flujo). El proceso le llevará al portal de Power Automate.

    > [!div class="mx-imgBorder"]
    > ![Captura de pantalla de la creación de un flujo](../media/how-to-integrate-power-virtual-agent/create-a-flow.png)


    Power Automate se abre con una nueva plantilla. No usará esta nueva plantilla.

    :::image type="content" source="../media/how-to-integrate-power-virtual-agent/power-automate-flow-initial-template.png" alt-text="Captura de pantalla parcial del flujo de conversación, con la opción Eliminar resaltada.":::

## <a name="create-a-power-automate-flow-to-connect-to-your-knowledge-base"></a>Creación de un flujo de Power Automate para conectarse a una base de conocimiento

En el procedimiento siguiente se crea un flujo de Power Automate que:
* Toma el texto de usuario de entrada y lo envía a QnA Maker.
* Devuelve la respuesta principal al agente.

1. En **Power Automate**, seleccione **Plantillas** en el panel de navegación izquierdo. Si se le pregunta si desea salir de la página del explorador, acepte Salir.

1. En la página de plantillas, busque la plantilla **Generate answer using QnA Maker** (Generar respuesta con QnA Maker) y, a continuación, selecciónela. Esta plantilla incluye todos los pasos necesarios para llamar a QnA Maker con la configuración de la base de conocimiento y devolver la respuesta principal.

1. En la nueva pantalla del flujo de QnA Maker, seleccione **Continuar**.

    :::image type="content" source="../media/how-to-integrate-power-virtual-agent/power-automate-qna-flow-template-continue.png" alt-text="Captura de pantalla parcial del flujo de conversación, con la opción Eliminar resaltada.":::

1. Seleccione el cuadro de acción **Generate Answer** (Generar respuesta) y rellene la configuración de QnA Maker de la sección anterior titulada [Crear y publicar una base de conocimiento](#create-and-publish-a-knowledge-base). El **host del servicio** de la imagen siguiente hace referencia al host de la base de conocimiento **Host** y tiene el formato `https://YOUR-RESOURCE-NAME.azurewebsites.net/qnamaker`.


    :::image type="content" source="../media/how-to-integrate-power-virtual-agent/power-virtual-agent-fill-in-generate-answer-settings.png" alt-text="Captura de pantalla parcial del flujo de conversación, con la opción Eliminar resaltada.":::

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

1. Busque el flujo en la lista **Outside solutions** (Soluciones externas) y, a continuación, seleccione **Agregar** para finalizar el proceso. Si hay muchos flujos, examine la columna **Modified** (Modificado) para encontrar el flujo más reciente.

## <a name="add-your-solutions-flow-to-power-virtual-agents"></a>Incorporación del flujo de una solución a Power Virtual Agents

1. Vuelva a la pestaña del explorador con el agente en Power Virtual Agents. El lienzo de creación debería estar aún abierto.

1. Para insertar un nuevo paso en el flujo, encima del cuadro de acción **Mensaje**, seleccione el conector **+** . A continuación, seleccione **Call an action** (Llamar a una acción).

1. En la ventana emergente **Flujo**, seleccione el nuevo flujo llamado **Generate answers using QnA Maker knowledge base** (Generar respuestas con la base de conocimiento de QnA Maker). La nueva acción aparece en el flujo.

    :::image type="content" source="../media/how-to-integrate-power-virtual-agent/power-virtual-agent-flow-after-adding-action.png" alt-text="Captura de pantalla parcial del flujo de conversación, con la opción Eliminar resaltada.":::

1. Para establecer correctamente la variable de entrada en la acción de QnA Maker, elija **Seleccionar una variable** y, a continuación, **bot.UnrecognizedTriggerPhrase**.

    :::image type="content" source="../media/how-to-integrate-power-virtual-agent/power-virtual-agent-selection-action-input.png" alt-text="Captura de pantalla parcial del flujo de conversación, con la opción Eliminar resaltada.":::


1. Para establecer correctamente la variable de salida en la acción de QnA Maker, en la acción **Mensaje**, seleccione **UnrecognizedTriggerPhrase**, el icono para insertar una variable, `{x}`, y, a continuación, **FinalAnswer**.

1. En la barra de herramientas contextual, seleccione **Save** (Guardar) a fin de guardar los detalles del lienzo de creación para el tema.

Este es el aspecto final del lienzo del agente.

> [!div class="mx-imgBorder"]
> ![La captura de pantalla muestra el lienzo final del agente, con las secciones de frases desencadenadoras, acción y mensaje.](../media/how-to-integrate-power-virtual-agent/power-virtual-agent-topic-authoring-canvas-full-flow.png)

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
    |6|¿Cómo puedo mejorar el rendimiento de las predicciones de consulta?|Esta pregunta desencadena la acción de reserva, que envía el texto a la base de conocimiento para responder. A continuación, se muestra la respuesta. Las marcas de verificación verdes de las acciones individuales indican la correcta finalización de cada acción.|

    :::image type="content" source="../media/how-to-integrate-power-virtual-agent/power-virtual-agent-test-tracked.png" alt-text="Captura de pantalla parcial del flujo de conversación, con la opción Eliminar resaltada.":::

## <a name="publish-your-bot"></a>Publicar el bot

Para que el agente esté disponible para todos los miembros del centro educativo o de la organización, es preciso que lo publique.

1. En el panel de navegación de la izquierda, seleccione **Publish** (Publicar). A continuación, seleccione **Publish** (Publicar) en la página.

1. Pruebe su bot en el sitio web de demostración (busque el vínculo en **Publish** [Publicar]).

    Se abre una nueva página web con su bot. Realice la misma pregunta de prueba al bot: `How can I improve the throughput performance for query predictions?`

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