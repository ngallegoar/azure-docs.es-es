---
title: Uso de Azure Service Bus Explorer para realizar operaciones de datos en Service Bus (versión preliminar)
description: En este artículo se proporciona información sobre cómo usar la instancia de Azure Service Bus Explorer basada en el portal para acceder a los datos de Azure Service Bus.
ms.topic: conceptual
ms.date: 06/23/2020
ms.openlocfilehash: e5e97c6860c2cc01048f4f7caf9f40f9e07592d0
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/25/2020
ms.locfileid: "91295607"
---
# <a name="use-service-bus-explorer-to-perform-data-operations-on-service-bus-preview"></a>Uso de Azure Service Bus Explorer para realizar operaciones de datos en Service Bus (versión preliminar)

## <a name="overview"></a>Información general

Azure Service Bus permite que las aplicaciones cliente del remitente y del receptor desacoplen su lógica de negocios con el uso de la conocida semántica de punto a punto (cola) y publicación-suscripción (tema-suscripción).

Las operaciones realizadas en un espacio de nombres de Azure Service Bus son de dos tipos: 
   * Operaciones de administración: creación, actualización, eliminación de espacio de nombres de Service Bus, colas, temas y suscripciones.
   * Operaciones de datos: envío y recepción de mensajes de colas, temas y suscripciones.

Azure Service Bus Explorer amplía la funcionalidad del portal más allá de las operaciones de administración para admitir las operaciones de datos (envío, recepción, inspección) en las colas, temas y suscripciones (y sus subentidades de mensajes fallidos), directamente desde el propio Azure Portal.

> [!NOTE]
> En este artículo se resalta la funcionalidad de Azure Service Bus Explorer que reside en Azure Portal.
>
> La herramienta Azure Service Bus Explorer ***no*** es la herramienta OSS propiedad de la comunidad [Service Bus Explorer](https://github.com/paolosalvatori/ServiceBusExplorer).
>

## <a name="prerequisites"></a>Prerrequisitos

Para usar la herramienta Azure Service Bus Explorer, deberá aprovisionar un espacio de nombres de Azure Service Bus. 

Una vez que se haya aprovisionado el espacio de nombres de Service Bus, debe crear una cola para enviar y recibir mensajes, o un tema con una suscripción para probar la funcionalidad.

Para más información sobre cómo crear colas, temas y suscripciones, consulte los vínculos siguientes:
   * [Inicio rápido: Creación de colas](service-bus-quickstart-portal.md)
   * [Inicio rápido: Creación de temas](service-bus-quickstart-topics-subscriptions-portal.md)


## <a name="using-the-service-bus-explorer"></a>Uso de Azure Service Bus Explorer

Para usar Azure Service Bus Explorer, debe ir al espacio de nombres de Service Bus en el que desee realizar las operaciones de envío, inspección y recepción.

Si desea realizar operaciones en una cola, seleccione **Colas** en el menú de navegación. Si desea realizar operaciones en un tema (y en sus suscripciones relacionadas), seleccione **Temas**. 

:::image type="content" source="./media/service-bus-explorer/queue-topics-left-navigation.png"alt-text="Selección de entidad":::

Después de seleccionar **Colas** o **Temas**, seleccione la cola o el tema específico.

Seleccione **Azure Service Bus Explorer (versión preliminar)** en el menú de navegación izquierdo.

:::image type="content" source="./media/service-bus-explorer/left-navigation-menu-selected.png" alt-text="Selección de entidad":::

### <a name="sending-a-message-to-a-queue-or-topic"></a>Envío de un mensaje a una cola o un tema

Para enviar un mensaje a una **cola** o un **tema**, haga clic en la pestaña ***Enviar*** de Azure Service Bus Explorer.

Para redactar un mensaje aquí: 

1. Elija el **tipo de contenido** para que sea "Text/Plain", "Application/XML" o "Application/JSON".
2. Agregue el **contenido** del mensaje. Asegúrese de que coincida con el **tipo de contenido** establecido anteriormente.
3. Establezca las **propiedades avanzadas** (opcional): entre estas se incluyen el identificador de correlación, el identificador de mensaje, la etiqueta, ReplyTo, el período de vida (TTL) y el tiempo de puesta en cola programado (para los mensajes programados).
4. Establezca las **propiedades personalizadas**: puede ser cualquier propiedad de usuario establecida en una clave de diccionario.

Una vez redactado el mensaje, presione Enviar.

:::image type="content" source="./media/service-bus-explorer/send-experience.png" alt-text="Selección de entidad":::

Si la operación de envío se completa correctamente, 

* si se envía a la cola, se incrementará el contador de la métrica **Mensajes activos**.

    :::image type="content" source="./media/service-bus-explorer/queue-after-send-metrics.png" alt-text="Selección de entidad":::

* Si envía al tema, se incrementará el contador de la métrica **Mensajes activos** en la suscripción a la que se enrutó el mensaje.

    :::image type="content" source="./media/service-bus-explorer/topic-after-send-metrics.png" alt-text="Selección de entidad":::

### <a name="receiving-a-message-from-a-queue"></a>Recepción de un mensaje de una cola

La función de recepción de Service Bus Explorer permite recibir un solo mensaje a la vez. La operación de recepción se realiza con el modo **ReceiveAndDelete**.

> [!IMPORTANT]
> Tenga en cuenta que la operación de recepción realizada por Azure Service Bus Explorer es ***destructiva***, es decir, el mensaje se quita de la cola cuando se muestra en la herramienta Azure Service Bus Explorer.
>
> Para examinar los mensajes sin quitarlos de la cola, considere la posibilidad de usar la funcionalidad de ***inspección***.
>

Para recibir un mensaje de una cola (o su subcola de mensajes fallidos) 

1. Haga clic en la pestaña ***Recibir*** de Azure Service Bus Explorer.
2. Compruebe en las métricas si hay **mensajes activos** o **mensajes no enviados** por recibir.

    :::image type="content" source="./media/service-bus-explorer/queue-after-send-metrics.png" alt-text="Selección de entidad":::

3. Elija entre la subcola ***Cola*** o ***Mensajes fallidos***.

    :::image type="content" source="./media/service-bus-explorer/queue-or-deadletter.png" alt-text="Selección de entidad":::

4. Haga clic en el botón ***Recibir*** y después en ***Sí*** para confirmar la operación Recibir y eliminar.


Cuando la operación de recepción se realice correctamente, los detalles del mensaje se verán en la cuadrícula como se muestra a continuación. Puede seleccionar el mensaje de la cuadrícula para mostrar sus detalles.

:::image type="content" source="./media/service-bus-explorer/receive-message-from-queue-2.png" alt-text="Selección de entidad":::


### <a name="peeking-a-message-from-a-queue"></a>Inspección de un mensaje de una cola

Con la funcionalidad de inspección, puede usar Azure Service Bus Explorer para ver los primeros 32 mensajes de una cola o de la cola de mensajes fallidos.

1. Para inspeccionar el mensaje de una cola, haga clic en la pestaña ***Ver*** de Azure Service Bus Explorer.

    :::image type="content" source="./media/service-bus-explorer/peek-tab-selected.png" alt-text="Selección de entidad":::

2. Compruebe en las métricas si hay **mensajes activos** o **mensajes fallidos** que inspeccionar.

    :::image type="content" source="./media/service-bus-explorer/queue-after-send-metrics.png" alt-text="Selección de entidad":::

3. A continuación, elija entre las subcolas ***Cola*** o ***Mensajes fallidos***.

    :::image type="content" source="./media/service-bus-explorer/queue-or-deadletter.png" alt-text="Selección de entidad":::

4. Haga clic en el botón ***Ver***. 

Una vez completada la operación de inspección, se mostrarán hasta 32 mensajes en la cuadrícula, como se muestra a continuación. Para ver los detalles de un mensaje determinado, selecciónelo en la cuadrícula. 

:::image type="content" source="./media/service-bus-explorer/peek-message-from-queue-2.png" alt-text="Selección de entidad":::

> [!NOTE]
>
> Dado que la inspección no es una operación destructiva, el mensaje **no** se quitará de la cola.
>

### <a name="receiving-a-message-from-a-subscription"></a>Recepción de un mensaje de una suscripción

Al igual que con una cola, la operación de ***recepción*** se puede realizar en una suscripción (o en su entidad de mensajes fallidos). Sin embargo, puesto que una suscripción reside en el contexto del tema, para llevar a cabo la operación de recepción, hay que ir a Azure Service Bus Explorer para un tema determinado.

> [!IMPORTANT]
> Tenga en cuenta que la operación de recepción realizada por Azure Service Bus Explorer es ***destructiva***, es decir, el mensaje se quita de la cola cuando se muestra en la herramienta Azure Service Bus Explorer.
>
> Para examinar los mensajes sin quitarlos de la cola, considere la posibilidad de usar la funcionalidad de ***inspección***.
>

1. Haga clic en la pestaña ***Recibir*** y seleccione la ***suscripción*** específica en el selector desplegable.

    :::image type="content" source="./media/service-bus-explorer/receive-subscription-tab-selected.png" alt-text="Selección de entidad":::

2. Elija entre la subentidad ***Suscripción*** o ***Mensajes fallidos***.

    :::image type="content" source="./media/service-bus-explorer/subscription-or-deadletter.png" alt-text="Selección de entidad":::

3. Haga clic en el botón ***Recibir*** y después en ***Sí*** para confirmar la operación Recibir y eliminar.

Si la operación de recepción se realiza correctamente, el mensaje recibido se mostrará en la cuadrícula como se indica a continuación. Para ver los detalles del mensaje, haga clic en él.

:::image type="content" source="./media/service-bus-explorer/receive-message-from-subscription.png" alt-text="Selección de entidad":::

### <a name="peeking-a-message-from-a-subscription"></a>Inspección de un mensaje desde una suscripción

Para examinar simplemente los mensajes de una suscripción o de su subentidad de mensajes fallidos, también se puede usar la funcionalidad de ***inspección*** en la suscripción.

1. Haga clic en la pestaña ***Ver*** y seleccione la ***suscripción*** específica en el selector desplegable.

    :::image type="content" source="./media/service-bus-explorer/peek-subscription-tab-selected.png" alt-text="Selección de entidad":::

2. Elija entre la ***suscripción*** o la subentidad de ***mensajes fallidos***.

    :::image type="content" source="./media/service-bus-explorer/subscription-or-deadletter.png" alt-text="Selección de entidad":::

3. Haga clic en el botón ***Ver***.

Una vez completada la operación de inspección, se mostrarán hasta 32 mensajes en la cuadrícula, como se muestra a continuación. Para ver los detalles de un mensaje determinado, selecciónelo en la cuadrícula. 

:::image type="content" source="./media/service-bus-explorer/peek-message-from-subscription.png" alt-text="Selección de entidad":::

> [!NOTE]
>
> Dado que la inspección no es una operación destructiva, el mensaje **no** se quitará de la cola.
>

## <a name="next-steps"></a>Pasos siguientes

   * Más información sobre las [colas](service-bus-queues-topics-subscriptions.md#queues) y los [temas](service-bus-queues-topics-subscriptions.md#topics-and-subscriptions) de Service Bus.
   * Más información sobre la creación de [colas de Service Bus mediante Azure Portal](service-bus-quickstart-portal.md)
   * Más información sobre la creación de [temas y suscripciones de Service Bus mediante Azure Portal](service-bus-quickstart-topics-subscriptions-portal.md)