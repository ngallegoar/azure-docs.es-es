---
title: Conexión con el servidor IBM MQ
description: Envío y recuperación de mensajes con un servidor de Azure o servidor IBM MQ local y Azure Logic Apps
services: logic-apps
ms.suite: integration
author: ChristopherHouser
ms.author: chrishou
ms.reviewer: valthom, logicappspm
ms.topic: article
ms.date: 03/31/2020
tags: connectors
ms.openlocfilehash: 737c5b90b216156ca08346f4a64fd0b421ad6c19
ms.sourcegitcommit: 27bbda320225c2c2a43ac370b604432679a6a7c0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/31/2020
ms.locfileid: "80410273"
---
# <a name="connect-to-an-ibm-mq-server-from-azure-logic-apps"></a>Conectarse a un servidor IBM MQ desde Azure Logic Apps

El conector IBM MQ envía y recupera mensajes almacenados en un servidor IBM MQ local o en Azure. Este conector incluye un cliente de Microsoft MQ que se comunica con un servidor IBM MQ remoto a través de una red TCP/IP. En este artículo se proporciona una guía de inicio para usar el conector MQ. Puede empezar por examinar un único mensaje en una cola y luego intentar otras acciones.

El conector IBM MQ incluye estas acciones, pero no proporciona ningún desencadenador:

- Examinar un único mensaje sin eliminarlo del servidor IBM MQ.
- Examinar un lote de mensajes sin eliminarlos del servidor IBM MQ.
- Recibir un único mensaje y eliminarlo del servidor IBM MQ.
- Recibir un lote de mensajes y eliminarlos del servidor IBM MQ.
- Enviar un único mensaje al servidor IBM MQ.

Estas son las versiones oficialmente compatibles de IBM WebSphere MQ:

  * MQ 7.5
  * MQ 8.0
  * MQ 9.0

## <a name="prerequisites"></a>Prerrequisitos

* Si usa un servidor MQ local, [instale la puerta de enlace de datos local](../logic-apps/logic-apps-gateway-install.md) en un servidor de la red. El servidor donde está instalada la puerta de enlace de datos local también debe tener .NET Framework 4.6 instalado para que el conector de MQ funcione.

  Al acabar de instalar la puerta de enlace, también debe crear un recurso de Azure para la puerta de enlace de datos local. Para más información, consulte [Configuración de una conexión de puerta de enlace de datos](../logic-apps/logic-apps-gateway-connection.md).

  Si el servidor MQ está disponible públicamente o en Azure, no debe usar la puerta de enlace de datos.

* La aplicación lógica en la que desea agregar la acción de MQ. Esta aplicación lógica debe usar la misma ubicación que la conexión de puerta de enlace de datos local y debe tener un desencadenador que inicia el flujo de trabajo.

  El conector de MQ no tiene ningún desencadenador, por lo que debe agregar primero un desencadenador a la aplicación lógica. Por ejemplo, puede usar el desencadenador de periodicidad. Si no está familiarizado con el uso de aplicaciones lógicas, pruebe este [inicio rápido para crear su primera aplicación lógica](../logic-apps/quickstart-create-first-logic-app-workflow.md).

<a name="create-connection"></a>

## <a name="create-mq-connection"></a>Creación de una conexión de MQ

Si aún no tiene ninguna conexión de MQ al agregar una acción de MQ, se le pedirá que la cree; por ejemplo:

![Proporcionar la información de conexión](media/connectors-create-api-mq/connection-properties.png)

1. Si se conecta a un servidor MQ local, seleccione **Conectarse mediante una puerta de enlace de datos local**.

1. Proporcione la información de conexión para el servidor MQ.

   * Para **Servidor**, especifique el nombre del servidor MQ, o escriba la dirección IP seguida de dos puntos y el número del puerto.

   * Para usar la Capa de sockets seguros (SSL), seleccione **¿Quiere habilitar SSL?** .

     Actualmente, el conector de MQ solo admite la autenticación de servidor, no la de cliente. Para obtener más información, consulte [Connection and authentication problems](#connection-problems) (Problemas de conexión y autenticación).

1. En la sección **Puerta de enlace**, siga estos pasos:

   1. En la lista **Suscripción**, seleccione la suscripción de Azure asociada con el recurso de la puerta de enlace de Azure.

   1. En la lista **Puerta de enlace de conexión**, seleccione el recurso de puerta de enlace de Azure que quiera usar.

1. Seleccione **Crear** cuando haya terminado.

<a name="connection-problems"></a>

### <a name="connection-and-authentication-problems"></a>Problemas de conexión y autenticación

Cuando la aplicación lógica intente conectarse al servidor MQ local, puede que aparezca este error:

`"MQ: Could not Connect the Queue Manager '<queue-manager-name>': The Server was expecting an SSL connection."`

* Si usa el conector de MQ directamente en Azure, el servidor MQ debe usar un certificado emitido por una [entidad de certificación](https://www.ssl.com/faqs/what-is-a-certificate-authority/) de confianza.

* Si usa la puerta de enlace de datos local, intente usar un certificado emitido por una [entidad de certificación](https://www.ssl.com/faqs/what-is-a-certificate-authority/) de confianza siempre que sea posible. Sin embargo, si esta opción no es posible, puede usar un certificado autofirmado, que no está emitido por una [entidad de certificación](https://www.ssl.com/faqs/what-is-a-certificate-authority/) de confianza, lo que se considera menos seguro.

  Para instalar el certificado autofirmado del servidor, puede usar la herramienta **Administrador de certificación de Windows** (certmgr.msc). En este escenario, en el equipo local en el que se ejecuta el servicio de puerta de enlace de datos local, debe instalar el certificado en el almacén de certificados del **equipo local** en el nivel de **entidades de certificación raíz de confianza**.

  1. En el equipo en el que se ejecuta el servicio de puerta de enlace de datos local, abra el menú Inicio, y busque y seleccione **Administrar certificados de usuario**.

  1. Una vez abierta la herramienta Administrador de certificación de Windows, vaya a la carpeta **Certificados: Equipo local** >  **Entidades de certificación raíz de confianza** e instale el certificado.

     > [!IMPORTANT]
     > Asegúrese de instalar el certificado en el almacén **Certificados: Equipo local** > **Entidades de certificación raíz de confianza**.

* El servidor MQ requiere que defina la especificación de cifrado que quiere usar para las conexiones SSL. Sin embargo, SsLStream en .NET no permite especificar el orden de las especificaciones de cifrado. Para evitar esta limitación, puede cambiar la configuración del servidor MQ para que coincida con la primera especificación de cifrado del conjunto que el conector envía en la negociación SSL.

  Al intentar la conexión, el servidor MQ registra un mensaje de evento que indica que se produjo un error en la conexión porque el otro extremo usaba una especificación de cifrado incorrecta. El mensaje de evento contiene la especificación de cifrado que aparece en primer lugar en la lista. Actualice la especificación de cifrado en la configuración del canal para que coincida con la especificación de cifrado del mensaje de evento.

## <a name="browse-single-message"></a>Exploración de un único mensaje

1. En la aplicación lógica, en el desencadenador u otra acción, seleccione **Nuevo paso**.

1. En el cuadro de búsqueda, escriba `mq` y seleccione la acción **Buscar mensaje**.

   ![Selección de la acción "Buscar mensaje"](media/connectors-create-api-mq/browse-message.png)

1. Si aún no ha creado ninguna conexión de MQ, se le pedirá que [la cree](#create-connection).

1. Una vez creada, configure las propiedades de la acción **Buscar mensaje**:

   | Propiedad | Descripción |
   |----------|-------------|
   | **Cola** | Si es diferente de la cola especificada en la conexión, especifique esa cola. |
   | **MessageId**, **CorrelationId**, **GroupId** y otras propiedades | Busque un mensaje en función de distintas propiedades de mensaje de MQ. |
   | **IncludeInfo** | Para incluir información de mensaje adicional en el resultado, seleccione **true**. Para omitir información de mensaje adicional en el resultado, seleccione **false**. |
   | **Tiempo de espera** | especifique un valor para determinar cuánto tiempo esperar a que llegue un mensaje a una cola vacía. Si no se especifica nada, se recupera el primer mensaje de la cola y no hay tiempo de espera para que aparezca un mensaje. |
   |||

   Por ejemplo:

   ![Propiedades de la acción "Buscar mensaje"](media/connectors-create-api-mq/browse-message-properties.png)

1. Cuando esté listo, seleccione **Guardar** en la barra de herramientas del diseñador. Para probar la aplicación, seleccione **Ejecutar**.

   Una vez finalizada la ejecución, el diseñador muestra los pasos del flujo de trabajo y su estado para que pueda revisar la salida.

1. Para ver los detalles de cada paso, haga clic en la barra de título del paso. Para revisar más información sobre la salida del paso, seleccione **Mostrar salidas sin procesar**.

   ![Salida de Buscar mensaje](media/connectors-create-api-mq/browse-message-output.png)

   Presentamos algunas salidas sin procesar de ejemplo:

   ![Salida sin procesar de Browse message](media/connectors-create-api-mq/browse-message-raw-output.png)

1. Si define **IncludeInfo** como **true**, se mostrará una salida adicional:

   ![IncludeInfo de Buscar mensaje](media/connectors-create-api-mq/browse-message-include-info.png)

## <a name="browse-multiple-messages"></a>Examen de varios mensajes

La acción **Examinar mensajes** incluye la opción **BatchSize** para indicar cuántos mensajes se deben devolver de la cola. Si no se especifica ningún valor para **BatchSize**, se devuelven todos los mensajes. El resultado devuelto es una matriz de mensajes.

1. Siga los pasos anteriores, pero agregue la acción **Examinar mensajes**.

1. Si aún no ha creado ninguna conexión de MQ, se le pedirá que [la cree](#create-connection). De lo contrario, de forma predeterminada, se usa la primera conexión configurada previamente. Para crear una nueva conexión, seleccione **Cambiar conexión**. O bien, seleccione una conexión diferente.

1. Proporcione la información para la acción.

1. Guarde y ejecute la aplicación lógica.

   Una vez que termina la ejecución de la aplicación lógica, presentamos algunos resultados de ejemplo de la acción **Examinar mensajes**:

   ![Salida de muestra de la acción "Examinar mensajes"](media/connectors-create-api-mq/browse-messages-output.png)

## <a name="receive-single-message"></a>Recibir un mensaje único

La acción **Receive message** tiene las mismas entradas y salidas que la acción **Browse message**. Cuando se usa **Recibir mensaje**, el mensaje se elimina de la cola.

## <a name="receive-multiple-messages"></a>Recepción de varios mensajes

La acción **Receive messages** tiene las mismas entradas y salidas que la acción **Browse messages**. Cuando se usa **Recibir mensajes**, los mensajes se eliminan de la cola.

> [!NOTE]
> Al ejecutar una acción de exploración o recepción en una cola sin mensajes, la acción devuelve este error:
>
> ![Error de MQ que indica que no hay mensajes](media/connectors-create-api-mq/mq-no-message-error.png)

## <a name="send-message"></a>Enviar mensaje

1. Siga los pasos anteriores, pero agregue la acción **Enviar mensaje** en su lugar.

1. Si aún no ha creado ninguna conexión de MQ, se le pedirá que [la cree](#create-connection). De lo contrario, de forma predeterminada, se usa la primera conexión configurada previamente. Para crear una nueva conexión, seleccione **Cambiar conexión**. O bien, seleccione una conexión diferente.

1. Proporcione la información para la acción. En **MessageType**, seleccione un tipo de mensaje válido: **Datagrama**, **Responder** o **Solicitud**

   ![Propiedades de la acción "Enviar mensaje"](media/connectors-create-api-mq/send-message-properties.png)

1. Guarde y ejecute la aplicación lógica.

   Cuando finalice la aplicación lógica, presentamos algunos resultados de ejemplo de la acción **Enviar mensaje**:

   ![Salida de muestra de la acción "Enviar mensaje"](media/connectors-create-api-mq/send-message-output.png)

## <a name="connector-reference"></a>Referencia de conectores

Para obtener información técnica acerca de las acciones y los límites, que se detallan en la descripción de Swagger del conector, consulte la [página de referencia del conector](/connectors/mq/).

## <a name="next-steps"></a>Pasos siguientes

* Obtenga más información sobre otros [conectores de Logic Apps](../connectors/apis-list.md)
