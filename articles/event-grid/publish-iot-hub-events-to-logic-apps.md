---
title: 'Tutorial: Uso de eventos de IoT Hub para desencadenar Azure Logic Apps'
description: En este tutorial se indica cómo usar el servicio de enrutamiento de eventos de Azure Event Grid para crear procesos automatizados que lleven a cabo acciones de Azure Logic Apps basadas en eventos de IoT Hub.
services: iot-hub, event-grid
author: philmea
ms.service: iot-hub
ms.topic: tutorial
ms.date: 09/14/2020
ms.author: philmea
ms.custom: devx-track-azurecli
ms.openlocfilehash: 5092aa0b5b23f04af1f49933bca234815f03f454
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "90604602"
---
# <a name="tutorial-send-email-notifications-about-azure-iot-hub-events-using-event-grid-and-logic-apps"></a>Tutorial: Envío de notificaciones por correo electrónico sobre eventos de Azure IoT Hub mediante Event Grid y Logic Apps

Azure Event Grid permite reaccionar a los eventos en IoT Hub mediante el desencadenamiento de acciones en las aplicaciones empresariales de bajada.

En este artículo se ofrece orientación mediante una configuración de ejemplo que usa IoT Hub y Event Grid. Al final, tendrá una aplicación lógica de Azure configurada para enviar una notificación por correo electrónico cada vez que un dispositivo se conecta o se desconecta del centro de IoT. Se puede utilizar Event Grid para recibir notificaciones puntuales sobre la desconexión de los dispositivos críticos. Las métricas y los diagnósticos pueden tardar varios minutos (es decir, 20 o más, sin precisar un número) en aparecer en los registros y las alertas. Esto puede ser inaceptable para la infraestructura crítica.

## <a name="prerequisites"></a>Requisitos previos

* Una suscripción de Azure activa. Si no tiene una suscripción, puede crear [una cuenta gratuita de Azure](https://azure.microsoft.com/pricing/free-trial/).

* Una cuenta de correo electrónico de cualquier proveedor de correo electrónico que sea compatible con Azure Logic Apps, como Office 365 Outlook u Outlook.com. Esta cuenta de correo electrónico se usa para enviar las notificaciones de eventos. 

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="create-an-iot-hub"></a>Creación de un centro de IoT

Puede crear rápidamente un nuevo centro de IoT mediante el terminal de Azure Cloud Shell en el portal.

1. Inicie sesión en [Azure Portal](https://portal.azure.com). 

1. En la parte superior derecha de la página, seleccione el botón Cloud Shell.

   ![Botón Cloud Shell](./media/publish-iot-hub-events-to-logic-apps/portal-cloud-shell.png)

1. Ejecute el comando siguiente para crear un nuevo grupo de recursos:

   ```azurecli
   az group create --name {your resource group name} --location westus
   ```
    
1. Ejecute el comando siguiente para crear un centro de IoT:

   ```azurecli
   az iot hub create --name {your iot hub name} --resource-group {your resource group name} --sku S1 
   ```

1. Minimice el terminal de Cloud Shell. Volverá al shell más adelante en el tutorial.

## <a name="create-a-logic-app"></a>Creación de una aplicación lógica

A continuación, cree una aplicación lógica y agregue un desencadenador HTTP de Event Grid que procese las solicitudes de IoT Hub. 

### <a name="create-a-logic-app-resource"></a>Creación de un recurso de aplicación lógica

1. En [Azure Portal](https://portal.azure.com), seleccione **Crear un recurso** y, a continuación, escriba "aplicación lógica" en el cuadro de búsqueda y presione Entrar. Seleccione **Aplicación lógica** en los resultados.

   ![Creación de la aplicación lógica](./media/publish-iot-hub-events-to-logic-apps/select-logic-app.png)

1. En la pantalla siguiente, seleccione **Crear**. 

1. Asigne un nombre a la aplicación lógica que sea único en su suscripción; a continuación, seleccione la misma suscripción, el grupo de recursos y la ubicación en que se encuentra la instancia de IoT Hub. 

   ![Campos para crear una aplicación lógica](./media/publish-iot-hub-events-to-logic-apps/create-logic-app-fields.png)

1. Seleccione **Revisar + crear**.

1. Compruebe la configuración y, a continuación, seleccione **Crear**.

1. Cuando se cree el nuevo recurso, seleccione **Ir al recurso**. 

1. En el diseñador de aplicaciones lógicas, desplácese hacia abajo para ver **Plantillas**. Elija **Aplicación lógica en blanco** para que pueda crear la aplicación lógica desde el principio.

### <a name="select-a-trigger"></a>Selección de un desencadenador

Un desencadenador es un evento específico que inicia la aplicación lógica. Para este tutorial, el desencadenador que activa el flujo de trabajo recibe una solicitud a través de HTTP.  

1. En la barra de búsqueda de conectores y desencadenadores, escriba **HTTP**.

1. Desplácese por los resultados y seleccione **Solicitud: Cuando se recibe una solicitud HTTP** como desencadenador. 

   ![Selección del desencadenador de solicitud HTTP](./media/publish-iot-hub-events-to-logic-apps/http-request-trigger.png)

1. Seleccione **Usar una carga de ejemplo para generar el esquema**. 

   ![Uso de la carga de ejemplo](./media/publish-iot-hub-events-to-logic-apps/sample-payload.png)

1. Pegue el código JSON del *esquema del evento dispositivo conectado* en el cuadro de texto y, después, seleccione **Listo**:

   ```json
     [{  
      "id": "f6bbf8f4-d365-520d-a878-17bf7238abd8",
      "topic": "/SUBSCRIPTIONS/<subscription ID>/RESOURCEGROUPS/<resource group name>/PROVIDERS/MICROSOFT.DEVICES/IOTHUBS/<hub name>",
      "subject": "devices/LogicAppTestDevice",
      "eventType": "Microsoft.Devices.DeviceConnected",
      "eventTime": "2018-06-02T19:17:44.4383997Z",
      "data": {
          "deviceConnectionStateEventInfo": {
            "sequenceNumber":
              "000000000000000001D4132452F67CE200000002000000000000000000000001"
          },
        "hubName": "egtesthub1",
        "deviceId": "LogicAppTestDevice",
        "moduleId" : "DeviceModuleID"
      }, 
      "dataVersion": "1",
      "metadataVersion": "1"
    }]
   ```

   este evento se publica cuando un dispositivo se conecta a un centro de IoT.

> [!NOTE]
> Puede recibir una notificación emergente que dice: **Recuerde incluir un encabezado Content-Type establecido en application/json en la solicitud.** Puede ignorar esta sugerencia sin ningún problema y pasar a la sección siguiente. 

### <a name="create-an-action"></a>Creación de una acción

Las acciones son los pasos que se producen después de que el desencadenador inicia el flujo de trabajo de la aplicación lógica. En este tutorial, la acción consiste en enviar una notificación por correo electrónico desde su proveedor de correo electrónico. 

1. Seleccione **Nuevo paso**. Se abrirá una ventana para **elegir una acción**.

1. Busque **Outlook**.

1. En función de su proveedor de correo electrónico, busque y seleccione el conector correspondiente. Este tutorial usa **Outlook.com**. Los pasos para otros proveedores de correo electrónico son similares. 

   ![Selección del conector del proveedor de correo electrónico](./media/publish-iot-hub-events-to-logic-apps/outlook-step.png)

1. Seleccione la acción **Enviar correo electrónico (V2)**. 

1. Seleccione **Iniciar sesión** e inicie sesión en su cuenta de correo electrónico. Seleccione **Sí** para permitir que la aplicación acceda a su información.

1. Compile su plantilla de correo electrónico. 

   * **Para**: escriba la dirección de correo electrónico para recibir los correos electrónicos de notificación. En este tutorial, use una cuenta de correo electrónico a la que pueda acceder para realizar pruebas. 

   * **Firmante**: Rellene el texto del asunto. Al hacer clic en el cuadro de texto Asunto, puede seleccionar contenido dinámico para incluirlo. Por ejemplo, en este tutorial se usa `IoT Hub alert: {eventType}`. Si no puede ver el contenido dinámico, seleccione el hipervínculo **Agregar contenido dinámico**: con esto lo puede activar o desactivar.

   * **Cuerpo**: escriba el texto del correo electrónico. Seleccione las propiedades JSON de la herramienta del selector para incluir contenido dinámico basado en los datos de los eventos. Si no puede ver el contenido dinámico, seleccione el hipervínculo **Agregar contenido dinámico** situado en el cuadro de texto **Cuerpo**. Si no aparecen los campos que desea, haga clic en el signo *más* en la pantalla Contenido dinámico para que se incluyan los campos de la acción anterior.

   La plantilla de correo electrónico puede tener un aspecto similar al de este ejemplo:

   ![Rellenar la información del correo electrónico](./media/publish-iot-hub-events-to-logic-apps/email-content.png)

1. Seleccione **Guardar** en el Diseñador de aplicaciones lógicas.  

### <a name="copy-the-http-url"></a>Copia de la dirección URL HTTP

Antes de cerrar el Diseñador de Logic Apps, copie la dirección URL en la que se realizan las escuchas de un desencadenador de las aplicaciones lógicas. Use esta dirección URL para configurar Event Grid. 

1. Haga clic en el cuadro de configuración del desencadenador **Cuando se recibe una solicitud HTTP** para expandirlo. 

1. Seleccione el botón de copia que se encuentra junto al valor **Dirección URL de HTTP POST** para copiarlo. 

   ![Copia de la dirección URL de HTTP POST](./media/publish-iot-hub-events-to-logic-apps/copy-url.png)

1. Guarde esta dirección URL para poder hacer referencia a ella en la siguiente sección. 

## <a name="configure-subscription-for-iot-hub-events"></a>Configurar la suscripción de eventos de IoT Hub

En esta sección, va a configurar IoT Hube para publicar eventos cuando se produzcan. 

1. En Azure Portal, vaya hasta el centro de IoT. Para ello, seleccione **Grupos de recursos**, a continuación, seleccione el grupo de recursos para este tutorial y, finalmente, seleccione el centro de IoT de la lista de recursos.

1. Seleccione **Eventos**.

   ![Abrir los detalles de Event Grid](./media/publish-iot-hub-events-to-logic-apps/event-grid.png)

1. Seleccione **Suscripción de eventos**. 

   ![Crear una suscripción de eventos](./media/publish-iot-hub-events-to-logic-apps/event-subscription.png)

1. Cree la suscripción de eventos con los siguientes valores: 

   1. En la sección **Detalles de suscripciones de eventos**:
      1. Especifique un **nombre** para la suscripción de eventos. 
      2. Seleccione **Esquema de Event Grid** en **Esquema de eventos**. 
   2. En la sección **Detalles del tema**:
      1. Confirme que en **Tipo de tema** está seleccionado **IoT Hub**. 
      2. Confirme que el nombre del centro de IoT se establece como valor del campo **Recurso de origen**. 
      3. Escriba el nombre del **tema del sistema** que se creará. Para obtener información acerca de los temas del sistema, consulte [Introducción a los temas del sistema](system-topics.md).
   3. En la sección **Tipos de evento**:
      1. Seleccione el desplegable **Filtro para tipos de evento**.
      1. Anule la selección de las casillas **Dispositivo creado** y **Dispositivo eliminado**, y deje seleccionadas solo las casillas **Dispositivo conectado** y **Dispositivo desconectado**.

         ![Seleccionar tipos de evento de suscripción](./media/publish-iot-hub-events-to-logic-apps/subscription-event-types.png)
   
   4. En la sección **Detalles del punto de conexión**: 
       1. En **Tipo de punto de conexión**, seleccione **Webhook**.
       2. Haga clic en **Seleccionar punto de conexión** y pegue la dirección URL que copió de la aplicación lógica y confirme la selección.

         ![seleccionar dirección URL de punto de conexión](./media/publish-iot-hub-events-to-logic-apps/endpoint-webhook.png)

         Una vez realizadas las acciones anteriores, el panel debe tener un aspecto similar al del ejemplo siguiente: 

         ![Formulario de ejemplo de suscripción de eventos](./media/publish-iot-hub-events-to-logic-apps/subscription-form.png)

1.  Seleccione **Crear**.

## <a name="simulate-a-new-device-connecting-and-sending-telemetry"></a>Simulación de la conexión de un nuevo dispositivo y del envío de datos de telemetría

Pruebe la aplicación lógica mediante la simulación rápida de una conexión de dispositivo mediante la CLI de Azure. 

1. Seleccione el botón Cloud Shell para volver a abrir el terminal.

1. Ejecute el siguiente comando para crear una identidad de dispositivo simulado:
    
     ```azurecli 
    az iot hub device-identity create --device-id simDevice --hub-name {YourIoTHubName}
    ```

1. Ejecute el siguiente comando para simular la conexión del dispositivo a IoT Hub y el envío de datos de telemetría:

    ```azurecli
    az iot device simulate -d simDevice -n {YourIoTHubName}
    ```

1. Cuando el dispositivo simulado se conecta a IoT Hub, recibirá un correo electrónico que le notificará el evento "DeviceConnected".

1. Cuando finaliza la simulación, recibirá un correo electrónico que le notificará el evento "DeviceDisconnected". 

    ![Correo electrónico de alerta de ejemplo](./media/publish-iot-hub-events-to-logic-apps/alert-mail.png)

## <a name="clean-up-resources"></a>Limpieza de recursos

En este tutorial se usaron recursos que generan gastos en su suscripción de Azure. Cuando haya terminado de probar el tutorial y los resultados de las pruebas, deshabilite o elimine los recursos que no desea conservar. 

Para eliminar todos los recursos que ha creado en este tutorial, elimine el grupo de recursos. 

1. Seleccione **Grupos de recursos** y, luego, el grupo de recursos que creó para este tutorial.

2. En la página del grupo de recursos, seleccione **Eliminar grupo de recursos**. Se le pedirá que escriba el nombre del grupo de recursos y, a continuación, podrá eliminarlo. También se quitarán todos los recursos que contiene.

## <a name="next-steps"></a>Pasos siguientes

* Obtenga más información sobre [reacción a eventos de IoT Hub mediante Event Grid para desencadenar acciones](../iot-hub/iot-hub-event-grid.md).
* [Aprenda a ordenar eventos conectados y desconectados de dispositivos](../iot-hub/iot-hub-how-to-order-connection-state-events.md)
* Obtenga información sobre qué más puede hacer con [Event Grid](overview.md).

Puede encontrar una lista completa de los conectores de Logic Apps admitidos en [Introducción a los conectores](/connectors/).
