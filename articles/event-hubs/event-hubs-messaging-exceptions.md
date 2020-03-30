---
title: Guía de solución de problemas para Azure Event Hubs | Microsoft Docs
description: En este artículo se proporciona una lista de las excepciones de mensajería y acciones sugeridas de Azure Event Hubs.
services: event-hubs
documentationcenter: na
author: ShubhaVijayasarathy
manager: timlt
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.custom: seodec18
ms.date: 01/16/2020
ms.author: shvija
ms.openlocfilehash: de5b95bd10bf72f60ba5d63c4b3a799556fcce33
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "76309785"
---
# <a name="troubleshooting-guide-for-azure-event-hubs"></a>Guía de solución de problemas para Azure Event Hubs
En este artículo se proporcionan algunas de las excepciones de .NET que generan las API de .NET Framework de Event Hubs y también otras sugerencias para solucionar problemas. 

## <a name="event-hubs-messaging-exceptions---net"></a>Excepciones de mensajería de Event Hubs: .NET
En esta sección se enumeran las excepciones de .NET generadas por API de .NET Framework. 

### <a name="exception-categories"></a>Categorías de excepciones

Las API de .NET de Event Hubs generan excepciones que pueden dividirse en las siguientes categorías, junto con la acción asociada que puede realizar para intentar corregirlas.

1. Error de codificación de usuario: [System.ArgumentException](https://msdn.microsoft.com/library/system.argumentexception.aspx), [System.InvalidOperationException](https://msdn.microsoft.com/library/system.invalidoperationexception.aspx), [System.OperationCanceledException](https://msdn.microsoft.com/library/system.operationcanceledexception.aspx), [System.Runtime.Serialization.SerializationException](https://msdn.microsoft.com/library/system.runtime.serialization.serializationexception.aspx). Acción general: intente corregir el código antes de continuar.
2. Error de instalación/configuración: [Microsoft.ServiceBus.Messaging.MessagingEntityNotFoundException](/dotnet/api/microsoft.servicebus.messaging.messagingentitynotfoundexception), [Microsoft.Azure.EventHubs.MessagingEntityNotFoundException](/dotnet/api/microsoft.azure.eventhubs.messagingentitynotfoundexception), [System.UnauthorizedAccessException](https://msdn.microsoft.com/library/system.unauthorizedaccessexception.aspx). Acción general: revise la configuración y cámbiela si es necesario.
3. Excepciones transitorias: [Microsoft.ServiceBus.Messaging.MessagingException](/dotnet/api/microsoft.servicebus.messaging.messagingexception), [Microsoft.ServiceBus.Messaging.ServerBusyException](#serverbusyexception), [Microsoft.Azure.EventHubs.ServerBusyException](#serverbusyexception), [Microsoft.ServiceBus.Messaging.MessagingCommunicationException](/dotnet/api/microsoft.servicebus.messaging.messagingcommunicationexception). Acción general: intente realizar de nuevo la operación o informe a los usuarios.
4. Otras excepciones: [System.Transactions.TransactionException](https://msdn.microsoft.com/library/system.transactions.transactionexception.aspx), [System.TimeoutException](#timeoutexception), [Microsoft.ServiceBus.Messaging.MessageLockLostException](/dotnet/api/microsoft.servicebus.messaging.messagelocklostexception), [Microsoft.ServiceBus.Messaging.SessionLockLostException](/dotnet/api/microsoft.servicebus.messaging.sessionlocklostexception). Acción general: específica del tipo de excepción; consulte la tabla de la siguiente sección. 

### <a name="exception-types"></a>Tipos de excepciones
En la tabla siguiente se describen los tipos de excepción de mensajería, sus causas y las acciones sugeridas que puede realizar.

| Tipo de excepción | Descripción/causa/ejemplos | Acción sugerida | Nota sobre el reintento automático o inmediato |
| -------------- | -------------------------- | ---------------- | --------------------------------- |
| [TimeoutException](https://msdn.microsoft.com/library/system.timeoutexception.aspx) |El servidor no respondió a la operación solicitada en el tiempo especificado, que está controlado por [OperationTimeout](/dotnet/api/microsoft.servicebus.messaging.messagingfactorysettings). Puede que el servidor haya completado la operación solicitada. Esta excepción se puede producir debido a un retraso de red o a otros retrasos de infraestructura. |Compruebe la coherencia del estado del sistema y vuelva a intentarlo si es necesario.<br /> Consulte [TimeoutException](#timeoutexception). | El reintento podría resultar útil en algunos casos; agregue lógica de reintento al código. |
| [InvalidOperationException](https://msdn.microsoft.com/library/system.invalidoperationexception.aspx) |La operación del usuario solicitada no está permitida en el servidor o servicio. Consulte el mensaje de excepción para obtener detalles. Por ejemplo, [Complete](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage) genera esta excepción si el mensaje se recibió en el modo [ReceiveAndDelete](/dotnet/api/microsoft.servicebus.messaging.receivemode) . | Compruebe el código y la documentación. Asegúrese de que la operación solicitada sea válida. | Los reintentos no funcionan. |
| [OperationCanceledException](https://msdn.microsoft.com/library/system.operationcanceledexception.aspx) | Se realiza un intento para invocar una operación en un objeto que ya se ha cerrado, anulado o eliminado. En raras ocasiones, la transacción de ambiente ya se ha eliminado. | Compruebe el código y asegúrese de que no invoca operaciones de un objeto desechado. | Los reintentos no funcionan. |
| [UnauthorizedAccessException](https://msdn.microsoft.com/library/system.unauthorizedaccessexception.aspx) | El objeto [TokenProvider](/dotnet/api/microsoft.servicebus.tokenprovider) no pudo adquirir un token, el token no es válido o no contiene las notificaciones necesarias para realizar la operación. | Asegúrese de que el proveedor de tokens se cree con los valores correctos. Compruebe la configuración de Access Control Service. | El reintento podría resultar útil en algunos casos; agregue lógica de reintento al código. |
| [ArgumentException](https://msdn.microsoft.com/library/system.argumentexception.aspx)<br /> [ArgumentNullException](https://msdn.microsoft.com/library/system.argumentnullexception.aspx)<br />[ArgumentOutOfRangeException](https://msdn.microsoft.com/library/system.argumentoutofrangeexception.aspx) | Uno o varios de los argumentos proporcionados para el método no son válidos. El URI proporcionado a [NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager) o [Create](/dotnet/api/microsoft.servicebus.messaging.messagingfactory) contiene segmentos de ruta de acceso. El esquema de URI proporcionado a [NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager) o [Create](/dotnet/api/microsoft.servicebus.messaging.messagingfactory) no es válido. El valor de la propiedad es mayor de 32 KB. | Compruebe el código de llamada y asegúrese de que los argumentos sean correctos. | El reintento no le será de ayuda. |
| [Microsoft.ServiceBus.Messaging MessagingEntityNotFoundException](/dotnet/api/microsoft.servicebus.messaging.messagingentitynotfoundexception) <br /><br/> [Microsoft.Azure.EventHubs MessagingEntityNotFoundException](/dotnet/api/microsoft.azure.eventhubs.messagingentitynotfoundexception) | La entidad asociada a la operación no existe o se eliminó. | Asegúrese de que la entidad exista. | El reintento no le será de ayuda. |
| [MessagingCommunicationException](/dotnet/api/microsoft.servicebus.messaging.messagingcommunicationexception) | El cliente no puede establecer una conexión al Centro de eventos. |Asegúrese de que el nombre de host proporcionado sea correcto y que este sea accesible. | El reintento podría resultar útil si hay problemas de conectividad intermitente. |
| [Microsoft.ServiceBus.Messaging ServerBusyException](/dotnet/api/microsoft.servicebus.messaging.serverbusyexception) <br /> <br/>[Microsoft.Azure.EventHubs ServerBusyException](/dotnet/api/microsoft.azure.eventhubs.serverbusyexception) | El servicio no puede procesar la solicitud en este momento. | El cliente puede esperar durante un período de tiempo y volver a intentar realizar la operación. <br /> Consulte [ServerBusyException](#serverbusyexception). | El cliente puede volver a intentarlo tras un determinado intervalo de tiempo. Si el reintento genera otra excepción, compruebe el comportamiento de reintento de esa excepción. |
| [MessagingException](/dotnet/api/microsoft.servicebus.messaging.messagingexception) | Excepción de mensajería genérica que puede producirse en los siguientes casos: intento de crear [QueueClient](/dotnet/api/microsoft.servicebus.messaging.queueclient) con un nombre o una ruta de acceso que pertenece a un tipo de entidad diferente (por ejemplo, un tema). Intento de enviar un mensaje mayor de 1 MB. El servidor o servicio encontró un error durante el procesamiento de la solicitud. Consulte el mensaje de excepción para obtener detalles. Por lo general, se trata de una excepción transitoria. | Compruebe el código y asegúrese de que solo se usan objetos serializables en el cuerpo del mensaje (o use un serializador personalizado). Consulte la documentación de los tipos de valor de las propiedades admitidos y use solo los tipos compatibles. Compruebe la propiedad [IsTransient](/dotnet/api/microsoft.servicebus.messaging.messagingexception) . Si es **true**, puede volver a intentar la operación. | El comportamiento de reintento es indefinido y quizá no resulte útil. |
| [MessagingEntityAlreadyExistsException](/dotnet/api/microsoft.servicebus.messaging.messagingentityalreadyexistsexception) | Se intenta crear una entidad con un nombre que ya se usa en otra entidad de ese espacio de nombres de servicio. | Elimine la entidad existente o elija un nombre diferente para la entidad que quiere crear. | El reintento no le será de ayuda. |
| [QuotaExceededException](/dotnet/api/microsoft.servicebus.messaging.quotaexceededexception) | La entidad de mensajería alcanzó su tamaño máximo permitido. Esta excepción puede suceder si ya ha abierto el número máximo de destinatarios (que es cinco) en un nivel de grupo por consumidor. | Cree espacio en la entidad recibiendo mensajes de esta o de sus subcolas. <br /> Consulte [QuotaExceededException](#quotaexceededexception). | El reintento podría resultar útil si los mensajes se eliminan mientras este se lleva a cabo. |
| [MessagingEntityDisabledException](/dotnet/api/microsoft.servicebus.messaging.messagingentitydisabledexception) | Solicitud para realizar una operación en tiempo de ejecución en una entidad deshabilitada. |Active la entidad. | El reintento podría ser útil si la entidad se activa mientras este se lleva a cabo. |
| [Microsoft.ServiceBus.Messaging MessageSizeExceededException](/dotnet/api/microsoft.servicebus.messaging.messagesizeexceededexception) <br /><br/> [Microsoft.Azure.EventHubs MessageSizeExceededException](/dotnet/api/microsoft.azure.eventhubs.messagesizeexceededexception) | Carga de mensaje que supera el límite de 1 MB. Este límite de 1 MB es para el mensaje total, que puede incluir propiedades del sistema y cualquier sobrecarga .NET. | Reduzca el tamaño de la carga del mensaje y vuelva a intentar la operación. |El reintento no le será de ayuda. |

### <a name="quotaexceededexception"></a>QuotaExceededException
[QuotaExceededException](/dotnet/api/microsoft.servicebus.messaging.quotaexceededexception) indica que se ha superado una cuota de una entidad específica.

Esta excepción suceder si ya ha abierto el número máximo de destinatarios (cinco) en un nivel de grupo por consumidor.

#### <a name="event-hubs"></a>Event Hubs
Event Hubs tiene un límite de 20 grupos de consumidores por Centro de eventos. Cuando intenta crear más, recibe [QuotaExceededException](/dotnet/api/microsoft.servicebus.messaging.quotaexceededexception). 

### <a name="timeoutexception"></a>TimeoutException
[TimeoutException](https://msdn.microsoft.com/library/system.timeoutexception.aspx) indica que la operación iniciada por el usuario está superando el tiempo de espera. 

Para Event Hubs, el tiempo de espera se especifica como parte de la cadena de conexión o a través de [ServiceBusConnectionStringBuilder](/dotnet/api/microsoft.servicebus.servicebusconnectionstringbuilder). El propio mensaje de error puede variar, pero siempre contiene el valor de tiempo de espera especificado para la operación actual. 

#### <a name="common-causes"></a>Causas comunes
Hay dos causas comunes de este error: configuración incorrecta o un error de servicio transitorio.

1. **Configuración incorrecta** : es posible que el tiempo de espera de la operación sea demasiado breve para la condición operativa. El valor predeterminado para el tiempo de espera de la operación en el SDK de cliente es 60 segundos. Compruebe si su código tiene el valor establecido en algo demasiado pequeño. La condición de la red y el uso de CPU pueden afectar al tiempo que tarda en completarse una operación particular, por lo que el tiempo de espera de esta no debe establecerse en un valor pequeño.
2. **Error de servicio transitorio**: a veces, el servicio Event Hubs puede experimentar retrasos en el procesamiento de solicitudes; por ejemplo, durante períodos de tráfico elevado. En tales casos, puede reintentar su operación después de un retraso hasta que la operación se realice correctamente. Si la misma operación aún experimenta errores después de varios intentos, visite el [sitio de estado del servicio de Azure](https://azure.microsoft.com/status/) para ver si hay interrupciones del servicio conocidas.

### <a name="serverbusyexception"></a>ServerBusyException

Una excepción [Microsoft.ServiceBus.Messaging.ServerBusyException](/dotnet/api/microsoft.servicebus.messaging.serverbusyexception) o [Microsoft.Azure.EventHubs.ServerBusyException](/dotnet/api/microsoft.azure.eventhubs.serverbusyexception) indica que un servidor está sobrecargado. Hay dos códigos de error relevantes para esta excepción.

#### <a name="error-code-50002"></a>Código de error 50002

Este error puede producirse por uno de estos dos motivos:

1. La carga no se distribuye uniformemente entre todas las particiones del centro de eventos y una partición ha alcanzado la limitación de unidades de rendimiento local.
    
    Solución: revisar la estrategia de distribución de particiones o probar [EventHubClient.Send(eventDataWithOutPartitionKey)](/dotnet/api/microsoft.servicebus.messaging.eventhubclient) puede que le sirva de ayuda.

2. El espacio de nombres de Event Hubs no tiene suficientes unidades de rendimiento (puede comprobar la pantalla **Métrica** en la ventana de espacio de nombres de Event Hubs en [Azure Portal](https://portal.azure.com) para confirmar). El portal muestra información agregada (un minuto), pero el rendimiento se mide en tiempo real, por lo que esto es solo una estimación.

    Solución: aumentar las unidades de rendimiento en el espacio de nombres puede resultar útil. Puede hacer esta operación en este portal, en la ventana **Escala** de la pantalla de espacio de nombres de Event Hubs. O bien, puede usar el [inflado automático](event-hubs-auto-inflate.md).

#### <a name="error-code-50001"></a>Código de error 50001

Este error se produce raras veces. Se produce cuando el contenedor que ejecuta el código para su espacio de nombres hace un uso bajo de la CPU: no más de unos pocos segundos antes de que comience el equilibrador de carga de Event Hubs.

#### <a name="limit-on-calls-to-the-getruntimeinformation-method"></a>Límite a las llamadas al método GetRuntimeInformation
Azure Event Hubs admite hasta 50 llamadas por segundo al método GetRuntimeInfo. Una vez que se alcanza el límite, puede recibir una excepción similar a esta:

```
ExceptionId: 00000000000-00000-0000-a48a-9c908fbe84f6-ServerBusyException: The request was terminated because the namespace 75248:aaa-default-eventhub-ns-prodb2b is being throttled. Error code : 50001. Please wait 10 seconds and try again.
```

## <a name="connectivity-certificate-or-timeout-issues"></a>Problemas de conectividad, certificados o tiempo de espera
Los pasos siguientes pueden ayudarle a solucionar problemas de conectividad, certificados y tiempo de espera para todos los servicios de *.servicebus.windows.net. 

- Desplácese a [ o use ](https://www.gnu.org/software/wget/)wget`https://<yournamespacename>.servicebus.windows.net/` ir allí. Le ayudará a comprobar si tiene problemas de cadena de certificados (lo más común al usar el SDK de Java), filtrado IP o red virtual.

    Un ejemplo de mensaje correcto:
    
    ```xml
    <feed xmlns="http://www.w3.org/2005/Atom"><title type="text">Publicly Listed Services</title><subtitle type="text">This is the list of publicly-listed services currently available.</subtitle><id>uuid:27fcd1e2-3a99-44b1-8f1e-3e92b52f0171;id=30</id><updated>2019-12-27T13:11:47Z</updated><generator>Service Bus 1.1</generator></feed>
    ```
    
    Un ejemplo de mensaje de error:

    ```json
    <Error>
        <Code>400</Code>
        <Detail>
            Bad Request. To know more visit https://aka.ms/sbResourceMgrExceptions. . TrackingId:b786d4d1-cbaf-47a8-a3d1-be689cda2a98_G22, SystemTracker:NoSystemTracker, Timestamp:2019-12-27T13:12:40
        </Detail>
    </Error>
    ```
- Ejecute el siguiente comando para comprobar si hay algún puerto bloqueado en el firewall. Los puertos usados son 443 (HTTPS), 5671 (AMQP) y 9093 (Kafka). En función de la biblioteca que use, también se usan otros puertos. Este es el comando de ejemplo que comprueba si el puerto 5671 está bloqueado.

    ```powershell
    tnc <yournamespacename>.servicebus.windows.net -port 5671
    ```

    En Linux:

    ```shell
    telnet <yournamespacename>.servicebus.windows.net 5671
    ```
- Si hay problemas de conectividad intermitentes, ejecute el siguiente comando para comprobar si hay paquetes descartados. Este comando intentará establecer 25 conexiones TCP diferentes cada segundo con el servicio. A continuación, puede comprobar cuántas de ellas se han realizado correctamente y cuántas han fallado y, además, ver la latencia de conexión TCP. Puede descargar la herramienta `psping` desde [aquí](/sysinternals/downloads/psping).

    ```shell
    .\psping.exe -n 25 -i 1 -q <yournamespacename>.servicebus.windows.net:5671 -nobanner     
    ```
    Puede usar comandos equivalentes si utiliza otras herramientas como `tnc`, `ping`, etc. 
- Realice un seguimiento de red si los pasos anteriores no ayudan y analícelo con herramientas como [Wireshark](https://www.wireshark.org/). Si lo necesita, póngase en contacto con el [soporte técnico de Microsoft](https://support.microsoft.com/). 

## <a name="next-steps"></a>Pasos siguientes

Para más información acerca de Event Hubs, visite los vínculos siguientes:

* [Información general de Event Hubs](event-hubs-what-is-event-hubs.md)
* [Creación de un Centro de eventos](event-hubs-create.md)
* [Preguntas más frecuentes sobre Event Hubs](event-hubs-faq.md)
