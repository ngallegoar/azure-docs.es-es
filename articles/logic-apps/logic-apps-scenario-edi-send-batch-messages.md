---
title: Procesamiento por lotes de los mensajes EDI como grupo
description: Enviar y recibir mensajes EDI como lotes, grupos o colecciones mediante el procesamiento por lotes en Azure Logic Apps
services: logic-apps
author: divyaswarnkar
ms.author: divswa
ms.reviewer: estfan, logicappspm
ms.topic: article
ms.date: 08/19/2018
ms.openlocfilehash: 6fc0833f70e3e9cd98100f193b52e5a1bfa4d651
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "75666676"
---
# <a name="exchange-edi-messages-as-batches-or-groups-between-trading-partners-in-azure-logic-apps"></a>Intercambiar mensajes EDI como lotes o grupos entre socios comerciales en Azure Logic Apps

En escenarios de negocio a negocio (B2B), los asociados a menudo intercambian mensajes en grupos o *lotes*. Al compilar una solución de procesamiento por lotes con Logic Apps, puede enviar mensajes a los socios comerciales y procesar esos mensajes en lotes. En este artículo se muestra cómo se pueden procesar por lotes mensajes EDI, con X12 como ejemplo, mediante la creación de una aplicación lógica "batch sender" (remitente de lotes) y otra "batch receiver" (receptora de lotes). 

El procesamiento por lotes de mensajes X12 funciona como el procesamiento por lotes de otros mensajes; se usa un desencadenador por lotes que recopila los mensajes en un lote y realiza una acción por lotes que envía los mensajes al lote. Además, procesamiento por lotes de X12 incluye un paso de codificación de X12 antes de que los mensajes partan al socio comercial o a otro destino. Para más información sobre el desencadenador y la acción por lotes, consulte el artículo de [procesamiento de mensajes en lote](../logic-apps/logic-apps-batch-process-send-receive-messages.md).

En este artículo se crea una solución de procesamiento por lotes mediante la creación de dos aplicaciones lógicas en la misma suscripción a Azure y región de Azure, y siguiendo este orden específico:

* La aplicación lógica ["batch receiver"](#receiver), que acepta y recopila los mensajes en un lote hasta que se cumplan los criterios especificados para liberar y procesar esos mensajes. En este escenario, la receptora de lotes también codifica los mensajes del lote mediante el acuerdo X12 especificado o las entidades asociadas.

  Asegúrese de crear primero la receptora de lotes para que más tarde pueda seleccionar el destino de los lotes cuando cree la remitente de lotes.

* Una o aplicación lógica ["batch sender"](#sender), que envíe los mensajes a la receptora de lotes creada anteriormente. 

Asegúrese de que la receptora de lotes y la remitente de lotes compartan la misma suscripción a Azure *y* región de Azure. Si no es así, no puede seleccionar la receptora de lotes al crear la remitente de lotes, ya que no se verán entre ellas.

## <a name="prerequisites"></a>Prerrequisitos

Para seguir este ejemplo, necesita estos elementos:

* Suscripción a Azure. Si no tiene una suscripción, puede [comenzar con una cuenta de Azure gratuita](https://azure.microsoft.com/free/). También puede [registrarse para obtener una suscripción de pago por uso](https://azure.microsoft.com/pricing/purchase-options/).

* Conocimientos básicos acerca de [cómo crear aplicaciones lógicas](../logic-apps/quickstart-create-first-logic-app-workflow.md)

* Una [cuenta de integración](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md) existente asociada a la suscripción de Azure y vinculada a las aplicaciones lógicas

* Al menos dos [asociados](../logic-apps/logic-apps-enterprise-integration-partners.md) existentes en la cuenta de integración. Los asociados deben utilizar el calificador X12 (Standard Carrier Alpha Code) en las propiedades del asociado como identidad empresarial.

* Un [acuerdo X12](../logic-apps/logic-apps-enterprise-integration-x12.md) existente en la cuenta de integración

* Para usar Visual Studio, en lugar de Azure Portal, asegúrese de [configurar Visual Studio para trabajar con Logic Apps](../logic-apps/quickstart-create-logic-apps-with-visual-studio.md).

<a name="receiver"></a>

## <a name="create-x12-batch-receiver"></a>Creación de la receptora de lotes de X12

Antes de poder enviar mensajes a un lote, ese lote debe existir como el destino adonde enviar esos mensajes. Primero, debe crear la aplicación lógica "batch receiver", que comienza con el desencadenador **Batch**. De este modo, al crear la aplicación lógica "batch sender", podrá seleccionar la aplicación lógica batch receiver. La receptora de lotes continúa recopilando los mensajes hasta que se cumplan los criterios especificados para liberar y procesar esos mensajes. Si bien las receptoras de lotes no necesitan saber nada sobre las remitentes de lotes, estas últimas sí necesitan conocer el destino adonde enviar los mensajes. 

Para la receptora de lotes, especifique el modo del lote, el nombre, los criterios de liberación, el acuerdo X12 y el resto de opciones. 

1. En [Azure Portal](https://portal.azure.com) o Visual Studio, cree una aplicación lógica con este nombre: "BatchX12Messages"

2. [Vincule la aplicación lógica a la cuenta de integración](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md#link-account).

3. En el Diseñador de Logic Apps, agregue el desencadenador de **lotes** que inicia el flujo de trabajo de la aplicación lógica. En el cuadro de búsqueda, escriba "lotes" como filtro. Seleccione este desencadenador: **Mensajes de lote**

   ![Agregar desencadenador de lotes](./media/logic-apps-scenario-EDI-send-batch-messages/add-batch-receiver-trigger.png)

4. Establezca las propiedades de la receptora de lotes: 

   | Propiedad | Value | Notas | 
   |----------|-------|-------|
   | **Batch Mode** | En línea |  |  
   | **Batch Name** | TestBatch | Disponible solo con Batch Mode **Inline** | 
   | **Release Criteria** | Depende de Message count based y Schedule based | Disponible solo con Batch Mode **Inline** | 
   | **Message Count** | 10 | Disponible solo con Release Criteria **Message count based** | 
   | **Intervalo** | 10 | Disponible solo con Release Criteria **Schedule based** | 
   | **Frecuencia** | minute | Disponible solo con Release Criteria **Schedule based** | 
   ||| 

   ![Incorporación de los detalles del desencadenador en lote](./media/logic-apps-scenario-EDI-send-batch-messages/batch-receiver-release-criteria.png)

   > [!NOTE]
   > En este ejemplo no se configura una partición para el lote, por lo que los lotes usan la misma clave de partición. Para más información acerca de las particiones, consulte [Mensajes de procesamiento por lotes](../logic-apps/logic-apps-batch-process-send-receive-messages.md#batch-sender).

5. Ahora, agregue una acción que codifique cada lote: 

   1. En el desencadenador de lotes, elija **Nuevo paso**.

   2. En el cuadro de búsqueda, escriba "X12 batch" como filtro y seleccione esta acción (para cualquier versión): **Batch encode <*versión*> - X12** 

      ![Seleccione la acción de codificación por lotes X12.](./media/logic-apps-scenario-EDI-send-batch-messages/add-batch-encode-action.png)

   3. Si aún no se había conectado a la cuenta de integración, cree la conexión ahora. Proporcione un nombre para la conexión, seleccione la cuenta de integración que desee y elija **Crear**.

      ![Creación de la conexión entre la cuenta de integración y el codificador por lotes](./media/logic-apps-scenario-EDI-send-batch-messages/batch-encoder-connect-integration-account.png)

   4. Establezca estas propiedades para la acción del codificador por lotes:

      | Propiedad | Descripción |
      |----------|-------------|
      | **Name of X12 agreement** | Abra la lista y seleccione el acuerdo existente. <p>Si la lista está vacía, asegúrese de [vincular la aplicación lógica a la cuenta de integración](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md#link-account) que tenga el acuerdo que desee. | 
      | **BatchName** | Haga clic en este cuadro y, cuando aparezca la lista de contenido dinámico, seleccione el token **Batch Name**. | 
      | **PartitionName** | Haga clic en este cuadro y, cuando aparezca la lista de contenido dinámico, seleccione el token **Partition Name**. | 
      | **Items** | Cierre el cuadro de detalles del elemento y haga clic en este cuadro. Cuando aparezca la lista de contenido dinámico, seleccione el token **Batched Items**. | 
      ||| 

      ![Detalles de la acción de codificación por lotes](./media/logic-apps-scenario-EDI-send-batch-messages/batch-encode-action-details.png)

      Para el cuadro **Items**:

      ![Elementos de la acción de codificación por lotes](./media/logic-apps-scenario-EDI-send-batch-messages/batch-encode-action-items.png)

6. Guarde la aplicación lógica. 

7. Si usa Visual Studio, asegúrese de [implementar la aplicación lógica batch receiver en Azure](../logic-apps/quickstart-create-logic-apps-with-visual-studio.md#deploy-logic-app-to-azure). En caso contrario, no podrá seleccionar la receptora de lotes cuando cree la remitente de lotes.

### <a name="test-your-logic-app"></a>Comprobación de la aplicación lógica

Para asegurarse de que la receptora de lotes funciona según lo previsto, puede agregar una acción HTTP con fines de prueba y enviar un mensaje por lotes al [servicio RequestBin](https://requestbin.com/). 

1. En la acción de codificación de X12, elija **Nuevo paso**. 

2. En el cuadro de búsqueda, escriba "http" como filtro. Seleccione esta acción: **HTTP - HTTP**
    
   ![Selección de la acción de HTTP](./media/logic-apps-scenario-EDI-send-batch-messages/batch-receiver-add-http-action.png)

3. Establezca las propiedades de la acción HTTP:

   | Propiedad | Descripción | 
   |----------|-------------|
   | **Método** | Seleccione **POST** en la lista. | 
   | **Uri** | Genere un identificador URI para la instancia de RequestBin y escríbalo en este cuadro. | 
   | **Cuerpo** | Haga clic en este cuadro y, cuando se abra la lista de contenido dinámico, seleccione el token **Body**, que aparece en la sección **Batch encode by agreement name** (Codificación en lotes por nombre de acuerdo). <p>Si no ve el token **Body**, junto a **Batch encode by agreement name** (Codificación en lotes por nombre de acuerdo), seleccione **Ver más**. | 
   ||| 

   ![Especificación de los detalles de la acción HTTP](./media/logic-apps-scenario-EDI-send-batch-messages/batch-receiver-add-http-action-details.png)

4. Guarde la aplicación lógica. 

   La aplicación lógica batch receiver se parecerá a este ejemplo: 

   ![Guardado de la aplicación lógica batch receiver](./media/logic-apps-scenario-EDI-send-batch-messages/batch-receiver-finished.png)

<a name="sender"></a>

## <a name="create-x12-batch-sender"></a>Creación de la remitente de lotes de X12

Ahora cree una o más aplicaciones lógicas que envíen mensajes a la aplicación lógica batch receiver. En cada batch sender, especifique el nombre de la aplicación lógica batch receiver y del lote, el contenido del mensaje, y el resto de las opciones. Opcionalmente, también puede proporcionar una clave de partición única para dividir el lote en subconjuntos para recopilar mensajes con esa clave. 

* Asegúrese de haber ya [creado la receptora de lotes](#receiver) de modo que, cuando cree la remitente de lotes, pueda seleccionar la receptora de lotes existente como lote de destino. Si bien las receptoras de lotes no necesitan saber nada sobre las remitentes de lotes, estas últimas deben saber adonde enviar los mensajes. 

* Asegúrese de que la receptora de lotes y la remitente de lotes compartan la misma región de Azure *y* suscripción a Azure. Si no es así, no puede seleccionar la receptora de lotes al crear la remitente de lotes, ya que no se verán entre ellas.

1. Cree otra aplicación lógica con este nombre: "SendX12MessagesToBatch" 

2. En el cuadro de búsqueda, escriba "when a http request" como filtro. Seleccione este desencadenador: **Cuando se recibe una solicitud HTTP** 
   
   ![Adición del desencadenador Request](./media/logic-apps-scenario-EDI-send-batch-messages/add-request-trigger-sender.png)

3. Agregue una acción para enviar mensajes a un lote.

   1. En la acción de solicitud HTTP, elija **Nuevo paso**.

   2. En el cuadro de búsqueda, escriba "lotes" como filtro. 
   En la lista de **acciones**, seleccione la siguiente: **Choose a Logic Apps workflow with batch trigger - Send messages to batch** (Escoger un flujo de trabajo de Logic Apps con el desencadenador de lotes: Enviar mensajes al lote)

      ![Seleccione "Choose a Logic Apps workflow with batch trigger"](./media/logic-apps-scenario-EDI-send-batch-messages/batch-sender-select-batch-trigger.png)

   3. Ahora seleccione la aplicación lógica "BatchX12Messages" que creó anteriormente.

      ![Seleccionar aplicación lógica "receptora de lotes"](./media/logic-apps-scenario-EDI-send-batch-messages/batch-sender-select-batch-receiver.png)

   4. Seleccione esta acción: **Batch_messages - <*su-receptor-de-lotes*>**

      ![Selección de la acción "Batch_messages"](./media/logic-apps-scenario-EDI-send-batch-messages/batch-sender-select-batch-messages-action.png)

4. Establezca las propiedades de la remitente de lotes.

   | Propiedad | Descripción | 
   |----------|-------------| 
   | **Batch Name** | El nombre de lote definido por la aplicación lógica receptora, "TestBatch" en este ejemplo <p>**Importante**: el nombre del lote se valida en tiempo de ejecución y debe coincidir con el nombre especificado por la aplicación lógica receptora. Si cambia el nombre del lote, provocará un error en la remitente de lotes. | 
   | **Message Content** | Contenido del mensaje que desea enviar, que es el token **Body** en este ejemplo | 
   ||| 
   
   ![Establecimiento de las propiedades de lote](./media/logic-apps-scenario-EDI-send-batch-messages/batch-sender-set-batch-properties.png)

5. Guarde la aplicación lógica. 

   La aplicación lógica batch sender se parecerá a este ejemplo:

   ![Guarde la aplicación lógica batch sender](./media/logic-apps-scenario-EDI-send-batch-messages/batch-sender-finished.png)

## <a name="test-your-logic-apps"></a>Comprobación de las aplicaciones lógicas

Para probar la solución de procesamiento por lotes, publique mensajes X12 en la aplicación lógica batch sender desde [Postman](https://www.getpostman.com/postman) o una herramienta similar. Pronto debería empezar a recibir mensajes X12, ya sea en la instancia de RequestBin, cada 10 minutos o en lotes de 10, todos con la misma clave de partición.

## <a name="next-steps"></a>Pasos siguientes

* [Envío, recepción y procesamiento por lotes de mensajes en Logic Apps](../logic-apps/logic-apps-batch-process-send-receive-messages.md) 
