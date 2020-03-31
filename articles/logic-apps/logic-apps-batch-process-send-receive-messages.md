---
title: Procesamiento por lotes de mensajes como grupo
description: Enviar y recibir mensajes en grupos entre los flujos de trabajo mediante el procesamiento por lotes en Azure Logic Apps
services: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: estfan, jonfan, logicappspm
ms.topic: article
ms.date: 01/16/2019
ms.openlocfilehash: e48d2bb2ffce0dd4f9293417534165165d426784
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "75666761"
---
# <a name="send-receive-and-batch-process-messages-in-azure-logic-apps"></a>Envío, recepción y procesamiento por lotes de mensajes en Azure Logic Apps

Para enviar y procesar mensajes juntos de forma específica como grupos, puede crear una solución de procesamiento por lotes que recopile los mensajes en un *lote* hasta que se cumplan los criterios especificados para liberar y procesar los mensajes por lotes. El procesamiento por lotes puede reducir la frecuencia con la que aplicación lógica procesa los mensajes. En este artículo se muestra cómo crear una solución de procesamiento por lotes al crear dos aplicaciones lógicas en la misma suscripción a Azure, región de Azure y siguiendo este orden específico: 

* La aplicación lógica ["receptora de lotes"](#batch-receiver), que acepta y recopila los mensajes en un lote hasta que se cumplan los criterios especificados para liberar y procesar esos mensajes.

  Asegúrese de crear primero la receptora de lotes para que más tarde pueda seleccionar el destino de los lotes cuando cree la remitente de lotes.

* Una o varias aplicaciones lógicas ["remitente de lotes"](#batch-sender), que envíen los mensajes a la receptora de lotes creada anteriormente. 

   También puede especificar una clave única, como un número de cliente, que *particione* o divida el lote de destino en subconjuntos lógicos basándose en esa clave. De este modo, la aplicación receptora puede recopilar todos los elementos con la misma clave y procesarlos juntos.

Asegúrese de que la receptora de lotes y la remitente de lotes compartan la misma suscripción a Azure *y* región de Azure. Si no es así, no puede seleccionar la receptora de lotes al crear la remitente de lotes, ya que no se verán entre ellas.

## <a name="prerequisites"></a>Prerrequisitos

Para seguir este ejemplo, necesita estos elementos:

* Suscripción a Azure. Si no tiene una suscripción, puede [comenzar con una cuenta de Azure gratuita](https://azure.microsoft.com/free/). También puede [registrarse para obtener una suscripción de pago por uso](https://azure.microsoft.com/pricing/purchase-options/).

* Una cuenta de correo electrónico con cualquier [proveedor de correo electrónico compatible con Azure Logic Apps](../connectors/apis-list.md)

* Conocimientos básicos acerca de [cómo crear aplicaciones lógicas](../logic-apps/quickstart-create-first-logic-app-workflow.md) 

* Para usar Visual Studio, en lugar de Azure Portal, asegúrese de [configurar Visual Studio para trabajar con Logic Apps](../logic-apps/quickstart-create-logic-apps-with-visual-studio.md).

<a name="batch-receiver"></a>

## <a name="create-batch-receiver"></a>Creación de la receptora de lotes

Antes de poder enviar mensajes a un lote, ese lote debe existir como el destino adonde enviar esos mensajes. Primero, debe crear la aplicación lógica "batch receiver", que comienza con el desencadenador **Batch**. De este modo, al crear la aplicación lógica "batch sender", podrá seleccionar la aplicación lógica batch receiver. La receptora de lotes continúa recopilando los mensajes hasta que se cumplan los criterios especificados para liberar y procesar esos mensajes. Si bien las receptoras de lotes no necesitan saber nada sobre las remitentes de lotes, estas últimas sí necesitan conocer el destino adonde enviar los mensajes. 

1. En [Azure Portal](https://portal.azure.com) o Visual Studio, cree una aplicación lógica con este nombre: "BatchReceiver" 

2. En el Diseñador de Logic Apps, agregue el desencadenador de **lotes** que inicia el flujo de trabajo de la aplicación lógica. En el cuadro de búsqueda, escriba "lotes" como filtro. Seleccione este desencadenador: **Mensajes de lote**

   ![Adición del desencadenador "Mensajes de lote"](./media/logic-apps-batch-process-send-receive-messages/add-batch-receiver-trigger.png)

3. Establezca las siguientes propiedades para la receptora de lotes: 

   | Propiedad | Descripción | 
   |----------|-------------|
   | **Batch Mode** | - **Inline**: para definir los criterios de versión en el desencadenador de lotes <br>- **Cuenta de integración**: para definir varias configuraciones de criterios de lanzamiento a través de una [cuenta de integración](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md). Con una cuenta de integración, puede mantener todas estas configuraciones en un mismo lugar, en lugar de en aplicaciones lógicas independientes. | 
   | **Batch Name** | El nombre del lote, que es "TestBatch" en este ejemplo, y solo se aplica al modo por lotes **Inline** |  
   | **Release Criteria** | Solo se aplica al modo por lotes **Inline** y selecciona los criterios que deben cumplirse antes de procesar cada lote: <p>- **Basado en el número de mensajes**: se publica el lote en función del número de mensajes recopilados por el lote. <br>- **Basado en el tamaño**: se publica el lote en función del tamaño total en bytes de todos los mensajes recopilados por el lote. <br>- **Programación**: se publica el lote según la periodicidad de la programación, que especifica un intervalo y una frecuencia. En las opciones avanzadas, también puede seleccionar una zona horaria y proporcionar una fecha y hora de inicio. <br>- **Seleccionar todo**: se usan todos los criterios especificados. | 
   | **Message Count** | Número de mensajes que se recopilan en el lote, por ejemplo, 10 mensajes. El límite de un lote es de 8000 mensajes. | 
   | **Tamaño de lote** | Tamaño total en bytes que se recolectará en el lote, por ejemplo, 10 MB. El límite de tamaño de lote es de 80 MB. | 
   | **Programación** | Intervalo y frecuencia entre las publicaciones de lotes, por ejemplo, 10 minutos. La periodicidad mínima es de 60 segundos o 1 minuto. Los minutos fraccionarios se redondean eficazmente hasta 1 minuto. Para especificar una zona horaria o una fecha y hora de inicio, seleccione **Mostrar opciones avanzadas**. | 
   ||| 

   > [!NOTE]
   > 
   > Si cambia los criterios de publicación mientras el desencadenador aún tiene mensajes procesados por lote sin enviar, el desencadenador utiliza los criterios de publicación actualizados para administrar los mensajes no enviados. 

   En el siguiente ejemplo se muestran todos los criterios, pero para realizar sus pruebas, use solo uno:

   ![Proporcionar detalles del desencadenador de lotes](./media/logic-apps-batch-process-send-receive-messages/batch-receiver-criteria.png)

4. Ahora, agregue una o varias acciones que procesen cada lote. 

   Para este ejemplo, agregue una acción que envíe un correo electrónico cuando se active el desencadenador de lotes. 
   El desencadenador se ejecuta y envía un correo electrónico cuando el lote tiene 10 mensajes, llega a 10 MB o pasan 10 minutos.

   1. En el desencadenador de lotes, elija **Nuevo paso**.

   2. En el cuadro de búsqueda, escriba "enviar correo electrónico" como filtro.
   En función de su proveedor de correo electrónico, seleccione un conector de correo electrónico.

      Por ejemplo, si tienes una cuenta personal, como @outlook.com o @hotmail.com, seleccione el conector de Outlook.com. 
      Si tiene una cuenta de Gmail, seleccione el conector de Gmail. 
      En este ejemplo se utiliza Office 365 Outlook. 

   3. Seleccione esta acción: **Enviar un correo electrónico - <*proveedor de correo electrónico*>**

      Por ejemplo:

      ![Seleccione la acción "Enviar un correo electrónico" para el proveedor de correo electrónico](./media/logic-apps-batch-process-send-receive-messages/batch-receiver-send-email-action.png)

5. Si se le pide, inicie sesión en la cuenta de correo electrónico. 

6. Establezca las propiedades de la acción que agregó.

   * En el cuadro **Para**, escriba la dirección de correo electrónico del destinatario. 
   Para realizar pruebas, puede usar su propia dirección de correo electrónico.

   * En el cuadro **Asunto**, cuando aparezca la lista de contenido dinámico, seleccione el campo **Nombre de la partición**.

     ![En la lista de contenido dinámico, seleccione "Nombre de la partición"](./media/logic-apps-batch-process-send-receive-messages/send-email-action-details.png)

     Más adelante, en el remitente de lotes, puede especificar una clave de partición única que divida el lote de destino en subconjuntos lógicos en los que puede enviar mensajes. 
     Cada conjunto tiene un número único generado por la aplicación lógica remitente de lotes. 
     Esta funcionalidad le permite usar un único lote con varios subconjuntos y definir cada subconjunto con el nombre que proporcione.

     > [!IMPORTANT]
     > Una partición tiene un límite de 5000 mensajes o 80 MB. Si se cumple alguna de estas condiciones, Logic Apps puede liberar el lote, incluso cuando no se cumpla la condición de liberación definida.

   * En el cuadro **Cuerpo**, cuando aparezca la lista de contenido dinámico, seleccione el campo **Id. del mensaje**. 

     El diseñador de Logic Apps agrega automáticamente un bucle "Para cada uno" en la acción para enviar correos electrónicos, debido a que esa acción trata el resultado de la acción anterior como una colección, en lugar de un lote. 

     ![En "Cuerpo", seleccione "Id. del mensaje"](./media/logic-apps-batch-process-send-receive-messages/send-email-action-details-for-each.png)

7.  Guarde la aplicación lógica. Ahora ha creado una receptora de lotes.

    ![Guardado de la aplicación lógica](./media/logic-apps-batch-process-send-receive-messages/save-batch-receiver-logic-app.png)

8. Si usa Visual Studio, asegúrese de [implementar la aplicación lógica batch receiver en Azure](../logic-apps/quickstart-create-logic-apps-with-visual-studio.md#deploy-logic-app-to-azure). En caso contrario, no podrá seleccionar la receptora de lotes cuando cree la remitente de lotes.

<a name="batch-sender"></a>

## <a name="create-batch-sender"></a>Creación de la remitente de lotes

Ahora cree una o más aplicaciones lógicas remitentes de lotes que envíen mensajes a la aplicación lógica receptora de lotes. En cada remitente de lotes, especifique el nombre de la receptora de lotes y del lote, el contenido del mensaje, y el resto de las opciones. Opcionalmente, también puede proporcionar una clave de partición única para dividir el lote en subconjuntos lógicos para recopilar mensajes con esa clave. 

* Asegúrese de haber ya [creado la receptora de lotes](#batch-receiver) de modo que, cuando cree la remitente de lotes, pueda seleccionar la receptora de lotes existente como lote de destino. Si bien las receptoras de lotes no necesitan saber nada sobre las remitentes de lotes, estas últimas deben saber adonde enviar los mensajes. 

* Asegúrese de que la receptora de lotes y la remitente de lotes compartan la misma región de Azure *y* suscripción a Azure. Si no es así, no puede seleccionar la receptora de lotes al crear la remitente de lotes, ya que no se verán entre ellas.

1. Cree otra aplicación lógica con este nombre: "BatchSender"

   1. En el cuadro de búsqueda, escriba "periodicidad" para el filtro. 
   Seleccione este desencadenador: **Periodicidad: Programación**

      ![Agregue el desencadenador "Periodicidad: Programación".](./media/logic-apps-batch-process-send-receive-messages/add-schedule-trigger-batch-sender.png)

   2. Establezca la frecuencia y el intervalo para ejecutar la aplicación lógica remitente cada minuto.

      ![Establecer la frecuencia y el intervalo del desencadenador Periodicidad](./media/logic-apps-batch-process-send-receive-messages/recurrence-trigger-batch-sender-details.png)

2. Agregue una nueva acción para enviar mensajes a un lote.

   1. En el desencadenador Periodicidad, elija **Nuevo paso**.

   2. En el cuadro de búsqueda, escriba "lotes" como filtro. 
   En la lista de **acciones**, seleccione la siguiente: **Choose a Logic Apps workflow with batch trigger - Send messages to batch** (Escoger un flujo de trabajo de Logic Apps con el desencadenador de lotes: Enviar mensajes al lote)

      ![Seleccione "Choose a Logic Apps workflow with batch trigger"](./media/logic-apps-batch-process-send-receive-messages/send-messages-batch-action.png)

   3. Seleccione la aplicación lógica receptora de lotes que creó anteriormente.

      ![Seleccionar aplicación lógica "receptora de lotes"](./media/logic-apps-batch-process-send-receive-messages/batch-sender-select-batch-receiver.png)

      > [!NOTE]
      > La lista también muestra todas las aplicaciones lógicas que tienen desencadenadores de lotes. 
      > 
      > Si está utilizando Visual Studio y no ve ninguna receptora de lote para seleccionar, compruebe que ha implementado la receptora de lotes en Azure. Si no lo ha hecho, aprenda cómo [implementar su aplicación de lógica receptora de lotes en Azure](../logic-apps/quickstart-create-logic-apps-with-visual-studio.md#deploy-logic-app-to-azure). 

   4. Seleccione esta acción: **Batch_messages - <*su-receptor-de-lotes*>**

      ![Seleccione esta acción: "Batch_messages - <su-aplicación-lógica>"](./media/logic-apps-batch-process-send-receive-messages/batch-sender-select-batch.png)

3. Establezca las propiedades de la remitente de lotes:

   | Propiedad | Descripción | 
   |----------|-------------| 
   | **Batch Name** | El nombre de lote definido por la aplicación lógica receptora, "TestBatch" en este ejemplo <p>**Importante**: el nombre del lote se valida en tiempo de ejecución y debe coincidir con el nombre especificado por la aplicación lógica receptora. Si cambia el nombre del lote, provocará un error en la remitente de lotes. | 
   | **Message Content** | El contenido del mensaje que desea enviar | 
   ||| 

   En este ejemplo, agregue esta expresión, que inserta la fecha y hora actuales en el contenido del mensaje que envía al lote:

   1. Haga clic en el cuadro **Contenido del mensaje**. 

   2. Cuando aparezca la lista de contenido dinámico, elija **Expresión**. 

   3. Escriba la expresión `utcnow()` y elija **Aceptar**. 

      ![Elija "Expresión" en "Contenido del mensaje", escriba "utcnow()" y haga clic en "Aceptar".](./media/logic-apps-batch-process-send-receive-messages/batch-sender-details.png)

4. Ahora debe configurar una partición para el lote. En la acción "BatchReceiver", elija **Mostrar opciones avanzadas** y establezca estas propiedades:

   | Propiedad | Descripción | 
   |----------|-------------| 
   | **Nombre de la partición** | Una clave de partición única opcional utilizada para dividir el lote de destino en subconjuntos lógicos y recopilar mensajes basándose en esa clave | 
   | **Id. de mensaje** | Un identificador de mensaje opcional que es un identificador único global (GUID) generado cuando está vacío | 
   ||| 

   En este ejemplo, en el cuadro **Nombre de la partición** agregue una expresión que genera un número aleatorio entre uno y cinco. Deje vacío el cuadro **Id. de mensaje**.
   
   1. Haga clic en el cuadro **Nombre de la partición** para que aparezca la lista de contenido dinámico. 

   2. En la lista de contenido dinámico, elija **Expresión**.
   
   3. Escriba la expresión `rand(1,6)` y elija **Aceptar**.

      ![Establecer una partición para el lote de destino](./media/logic-apps-batch-process-send-receive-messages/batch-sender-partition-advanced-options.png)

      Esta función **rand** genera un número comprendido entre uno y cinco. 
      Por tanto, va a dividir este lote en cinco particiones numeradas, que esta expresión establece dinámicamente.

5. Guarde la aplicación lógica. La aplicación lógica remitente tiene ahora un aspecto similar al de este ejemplo:

   ![Guardar la aplicación lógica remitente](./media/logic-apps-batch-process-send-receive-messages/batch-sender-finished.png)

## <a name="test-your-logic-apps"></a>Comprobación de las aplicaciones lógicas

Para probar la solución de procesamiento por lotes, deje las aplicaciones lógicas en ejecución durante unos minutos. Pronto, empezará a recibir mensajes de correo electrónico en grupos de cinco, todos con la misma clave de partición.

La aplicación lógica remitente de lotes se ejecuta cada minuto, genera un número aleatorio entre uno y cinco, y utiliza este número generado como clave de partición para el lote de destino donde se envían los mensajes. Cada vez que el lote tiene cinco elementos con la misma clave de partición, la aplicación lógica receptora de lotes se activa y envía los correos electrónicos para cada mensaje.

> [!IMPORTANT]
> Cuando haya terminado las pruebas, asegúrese de que deshabilita la aplicación lógica BatchSender para detener el envío de mensajes y evitar la sobrecarga de la bandeja de entrada.

## <a name="next-steps"></a>Pasos siguientes

* [Procesamiento por lotes y envío de mensajes EDI](../logic-apps/logic-apps-scenario-edi-send-batch-messages.md)
* [Generar definiciones de aplicación lógica mediante el uso de JSON](../logic-apps/logic-apps-author-definitions.md)
* [Cree una aplicación sin servidor en Visual Studio con Azure Logic Apps y Functions](../logic-apps/logic-apps-serverless-get-started-vs.md)
* [Control de excepciones y registro de errores para aplicaciones lógicas](../logic-apps/logic-apps-scenario-error-and-exception-handling.md)
