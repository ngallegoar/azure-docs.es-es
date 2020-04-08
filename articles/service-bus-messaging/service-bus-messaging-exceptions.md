---
title: Guía para la solución de problemas de Azure Service Bus | Microsoft Docs
description: En este artículo se proporciona una lista de las excepciones de mensajería de Azure Service Bus y las acciones sugeridas para cuando se produce la excepción.
services: service-bus-messaging
documentationcenter: na
author: axisc
manager: timlt
editor: spelluru
ms.assetid: 3d8526fe-6e47-4119-9f3e-c56d916a98f9
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/23/2020
ms.author: aschhab
ms.openlocfilehash: fb27befadcf8e6d201d020e758cfd1ef9b695f41
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80240801"
---
# <a name="troubleshooting-guide-for-azure-service-bus"></a>Guía para la solución de problemas de Azure Service Bus
En este artículo se proporcionan algunas de las excepciones de .NET que generan las API de .NET Framework de Service Bus y también otras sugerencias para solucionar problemas. 

## <a name="service-bus-messaging-exceptions"></a>Excepciones de mensajería de Service Bus
En esta sección se enumeran las excepciones de .NET generadas por API de .NET Framework. 

### <a name="exception-categories"></a>Categorías de excepciones
Las API de mensajería generan excepciones que pueden dividirse en las siguientes categorías, junto con la acción asociada que puede realizar para intentar corregirlas. El significado y las causas de una excepción pueden variar en función del tipo de entidad de mensajería:

1. Error de codificación de usuario ([System.ArgumentException](https://msdn.microsoft.com/library/system.argumentexception.aspx), [System.InvalidOperationException](https://msdn.microsoft.com/library/system.invalidoperationexception.aspx), [System.OperationCanceledException](https://msdn.microsoft.com/library/system.operationcanceledexception.aspx), [System.Runtime.Serialization.SerializationException](https://msdn.microsoft.com/library/system.runtime.serialization.serializationexception.aspx)). Acción general: intente corregir el código antes de continuar.
2. Error de instalación/configuración ([Microsoft.ServiceBus.Messaging.MessagingEntityNotFoundException](/dotnet/api/microsoft.azure.servicebus.messagingentitynotfoundexception) y [System.UnauthorizedAccessException](https://msdn.microsoft.com/library/system.unauthorizedaccessexception.aspx)). Acción general: revise la configuración y cámbiela si es necesario.
3. Excepciones transitorias ([Microsoft.ServiceBus.Messaging.MessagingException](/dotnet/api/microsoft.servicebus.messaging.messagingexception), [Microsoft.ServiceBus.Messaging.ServerBusyException](/dotnet/api/microsoft.azure.servicebus.serverbusyexception), [Microsoft.ServiceBus.Messaging.MessagingCommunicationException](/dotnet/api/microsoft.servicebus.messaging.messagingcommunicationexception)). Acción general: intente realizar de nuevo la operación o informe a los usuarios. La clase `RetryPolicy` del SDK de cliente puede configurarse para controlar reintentos automáticamente. Consulte [Instrucciones de reintento](/azure/architecture/best-practices/retry-service-specific#service-bus) para más información.
4. Otras excepciones ([System.Transactions.TransactionException](https://msdn.microsoft.com/library/system.transactions.transactionexception.aspx), [System.TimeoutException](https://msdn.microsoft.com/library/system.timeoutexception.aspx), [Microsoft.ServiceBus.Messaging.MessageLockLostException](/dotnet/api/microsoft.azure.servicebus.messagelocklostexception), [Microsoft.ServiceBus.Messaging.SessionLockLostException](/dotnet/api/microsoft.azure.servicebus.sessionlocklostexception)). Acción general: específica del tipo de excepción; consulte la tabla de la siguiente sección: 

### <a name="exception-types"></a>Tipos de excepciones
En la tabla siguiente se describen los tipos de excepción de mensajería, sus causas y las acciones sugeridas que puede realizar.

| **Tipo de excepción** | **Descripción/causa/ejemplos** | **Acción sugerida** | **Nota sobre el reintento automático o inmediato** |
| --- | --- | --- | --- |
| [TimeoutException](https://msdn.microsoft.com/library/system.timeoutexception.aspx) |El servidor no respondió a la operación solicitada en el tiempo especificado, que está controlado por [OperationTimeout](/dotnet/api/microsoft.servicebus.messaging.messagingfactorysettings). Puede que el servidor haya completado la operación solicitada. Esto puede producirse debido a un retraso de red o a otros retrasos de infraestructura. |Compruebe la coherencia del estado del sistema y vuelva a intentarlo si es necesario. Consulte [Excepciones de tiempo de espera](#timeoutexception). |El reintento podría resultar útil en algunos casos; agregue lógica de reintento al código. |
| [InvalidOperationException](https://msdn.microsoft.com/library/system.invalidoperationexception.aspx) |La operación del usuario solicitada no está permitida en el servidor o servicio. Consulte el mensaje de excepción para obtener detalles. Por ejemplo, [Complete()](/dotnet/api/microsoft.azure.servicebus.queueclient.completeasync) genera esta excepción si el mensaje se recibió en el modo [ReceiveAndDelete](/dotnet/api/microsoft.azure.servicebus.receivemode) . |Compruebe el código y la documentación. Asegúrese de que la operación solicitada sea válida. |Los reintentos no funcionan. |
| [OperationCanceledException](https://msdn.microsoft.com/library/system.operationcanceledexception.aspx) |Se realiza un intento para invocar una operación en un objeto que ya se ha cerrado, anulado o eliminado. En raras ocasiones, la transacción de ambiente ya se ha eliminado. |Compruebe el código y asegúrese de que no invoca operaciones de un objeto desechado. |Los reintentos no funcionan. |
| [UnauthorizedAccessException](https://msdn.microsoft.com/library/system.unauthorizedaccessexception.aspx) |El objeto [TokenProvider](/dotnet/api/microsoft.servicebus.tokenprovider) no pudo adquirir un token, el token no es válido o no contiene las notificaciones necesarias para realizar la operación. |Asegúrese de que el proveedor de tokens se cree con los valores correctos. Compruebe la configuración de Access Control Service. |El reintento podría resultar útil en algunos casos; agregue lógica de reintento al código. |
| [ArgumentException](https://msdn.microsoft.com/library/system.argumentexception.aspx)<br /> [ArgumentNullException](https://msdn.microsoft.com/library/system.argumentnullexception.aspx)<br />[ArgumentOutOfRangeException](https://msdn.microsoft.com/library/system.argumentoutofrangeexception.aspx) |Uno o varios de los argumentos proporcionados para el método no son válidos.<br /> El URI proporcionado a [NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager) o [Create](/dotnet/api/microsoft.servicebus.messaging.messagingfactory) contiene segmentos de ruta de acceso.<br /> El esquema de URI proporcionado a [NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager) o [Create](/dotnet/api/microsoft.servicebus.messaging.messagingfactory) no es válido. <br />El valor de la propiedad es mayor de 32 KB. |Compruebe el código de llamada y asegúrese de que los argumentos sean correctos. |Los reintentos no funcionan. |
| [MessagingEntityNotFoundException](/dotnet/api/microsoft.azure.servicebus.messagingentitynotfoundexception) |La entidad asociada a la operación no existe o se eliminó. |Asegúrese de que la entidad exista. |Los reintentos no funcionan. |
| [MessageNotFoundException](/dotnet/api/microsoft.servicebus.messaging.messagenotfoundexception) |Se intenta recibir un mensaje con una secuencia numérica concreta. Este mensaje no se encuentra. |Asegúrese de que aún no se recibió el mensaje. Compruebe la cola de correo devuelto para ver si el mensaje se procesó como correo devuelto. |Los reintentos no funcionan. |
| [MessagingCommunicationException](/dotnet/api/microsoft.servicebus.messaging.messagingcommunicationexception) |El cliente no puede establecer una conexión a Service Bus. |Asegúrese de que el nombre de host proporcionado sea correcto y que este sea accesible. |El reintento podría resultar útil si hay problemas de conectividad intermitente. |
| [ServerBusyException](/dotnet/api/microsoft.azure.servicebus.serverbusyexception) |El servicio no puede procesar la solicitud en este momento. |El cliente puede esperar durante un período de tiempo y volver a intentar realizar la operación. |El cliente puede volver a intentarlo tras un determinado intervalo de tiempo. Si el reintento genera otra excepción, compruebe el comportamiento de reintento de esa excepción. |
| [MessageLockLostException](/dotnet/api/microsoft.azure.servicebus.messagelocklostexception) |El token de bloqueo asociado al mensaje expiró o no se encuentra. |Deseche el mensaje. |Los reintentos no funcionan. |
| [SessionLockLostException](/dotnet/api/microsoft.azure.servicebus.sessionlocklostexception) |Se perdió el bloqueo asociado a esta sesión. |Anule el objeto [MessageSession](/dotnet/api/microsoft.servicebus.messaging.messagesession) . |Los reintentos no funcionan. |
| [MessagingException](/dotnet/api/microsoft.servicebus.messaging.messagingexception) |Excepción de mensajería genérica que puede producirse en los siguientes casos:<p>Se intentó crear [QueueClient](/dotnet/api/microsoft.azure.servicebus.queueclient) con un nombre o una ruta de acceso que pertenece a un tipo de entidad diferente (por ejemplo, un tema).</p><p>Intento de enviar un mensaje mayor de 256 KB. </p>El servidor o servicio encontró un error durante el procesamiento de la solicitud. Consulte el mensaje de excepción para obtener detalles. Por lo general, se trata de una excepción transitoria.</p><p>La solicitud finalizó porque la entidad se está limitando. Código de error: 50001, 50002, 50008. </p> | Compruebe el código y asegúrese de que solo se usan objetos serializables en el cuerpo del mensaje (o use un serializador personalizado). <p>Consulte la documentación de los tipos de valor de las propiedades admitidos y use solo los tipos compatibles.</p><p> Compruebe la propiedad [IsTransient](/dotnet/api/microsoft.servicebus.messaging.messagingexception) . Si es **true**, puede volver a intentar la operación. </p>| Si la excepción se debe a una limitación, espere unos segundos y vuelva a intentar la operación. El comportamiento de reintento es indefinido y quizá no resulte útil en otros escenarios.|
| [MessagingEntityAlreadyExistsException](/dotnet/api/microsoft.servicebus.messaging.messagingentityalreadyexistsexception) |Se intenta crear una entidad con un nombre que ya se usa en otra entidad de ese espacio de nombres de servicio. |Elimine la entidad existente o elija un nombre diferente para la entidad que quiere crear. |Los reintentos no funcionan. |
| [QuotaExceededException](/dotnet/api/microsoft.azure.servicebus.quotaexceededexception) |La entidad de mensajería ha alcanzado su tamaño máximo permitido, o bien se superó el número máximo de conexiones en un espacio de nombres. |Cree espacio en la entidad recibiendo mensajes de esta o de sus subcolas. Consulte [QuotaExceededException](#quotaexceededexception). |El reintento podría resultar útil si los mensajes se eliminan mientras este se lleva a cabo. |
| [RuleActionException](/dotnet/api/microsoft.servicebus.messaging.ruleactionexception) |Service Bus devuelve esta excepción si se intenta crear una acción de regla no válida. Si se produjo un error al procesar la acción de regla de un mensaje, Service Bus adjunta esta excepción a dicho mensaje. |Compruebe si la acción de regla es correcta. |Los reintentos no funcionan. |
| [FilterException](/dotnet/api/microsoft.servicebus.messaging.filterexception) |Service Bus devuelve esta excepción si se intenta crear un filtro no válido. Si se produjo un error al procesar el filtro de un mensaje, Service Bus adjunta esta excepción a dicho mensaje. |Compruebe si el filtro es correcto. |Los reintentos no funcionan. |
| [SessionCannotBeLockedException](/dotnet/api/microsoft.servicebus.messaging.sessioncannotbelockedexception) |Se intenta aceptar una sesión con un id. de sesión específico, pero actualmente esta se encuentra bloqueada por otro cliente. |Asegúrese de que otros clientes desbloqueen la sesión. |El reintento podría ser útil si la sesión se publica mientras este se lleva a cabo. |
| [TransactionSizeExceededException](/dotnet/api/microsoft.servicebus.messaging.transactionsizeexceededexception) |Hay demasiadas operaciones que forman parte de la transacción. |Reduzca el número de operaciones que forman parte de esta transacción. |Los reintentos no funcionan. |
| [MessagingEntityDisabledException](/dotnet/api/microsoft.azure.servicebus.messagingentitydisabledexception) |Solicitud para realizar una operación en tiempo de ejecución en una entidad deshabilitada. |Active la entidad. |El reintento podría ser útil si la entidad se activa mientras este se lleva a cabo. |
| [NoMatchingSubscriptionException](/dotnet/api/microsoft.servicebus.messaging.nomatchingsubscriptionexception) |Service Bus devuelve esta excepción si envía un mensaje a un tema que tiene el filtrado previo habilitado y dicho mensaje no coincide con ninguno de los filtros. |Asegúrese de que coincida con al menos un filtro. |Los reintentos no funcionan. |
| [MessageSizeExceededException](/dotnet/api/microsoft.servicebus.messaging.messagesizeexceededexception) |Una carga de mensaje supera el límite de 256 KB. El límite de 256 KB es el tamaño total del mensaje, que puede incluir las propiedades del sistema y cualquier sobrecarga .NET. |Reduzca el tamaño de la carga del mensaje y vuelva a intentar la operación. |Los reintentos no funcionan. |
| [TransactionException](https://msdn.microsoft.com/library/system.transactions.transactionexception.aspx) |La transacción de ambiente (*Transaction.Current*) no es válida. Puede se haya completado o anulado. La excepción interna puede proporcionar información adicional. | |Los reintentos no funcionan. |
| [TransactionInDoubtException](https://msdn.microsoft.com/library/system.transactions.transactionindoubtexception.aspx) |Se intenta realizar una operación en una transacción dudosa o se intenta confirmar una transacción y esta se convierte en dudosa. |La aplicación debe controlar esta excepción (como caso especial), porque puede que ya se haya confirmado la transacción. |- |

### <a name="quotaexceededexception"></a>QuotaExceededException
[QuotaExceededException](/dotnet/api/microsoft.azure.servicebus.quotaexceededexception) indica que se ha superado una cuota de una entidad específica.

#### <a name="queues-and-topics"></a>Colas y temas
Para las colas y los temas, suele tratarse del tamaño de la cola. La propiedad de mensaje de error contiene más detalles, como en el ejemplo siguiente:

```Output
Microsoft.ServiceBus.Messaging.QuotaExceededException
Message: The maximum entity size has been reached or exceeded for Topic: 'xxx-xxx-xxx'. 
    Size of entity in bytes:1073742326, Max entity size in bytes:
1073741824..TrackingId:xxxxxxxxxxxxxxxxxxxxxxxxxx, TimeStamp:3/15/2013 7:50:18 AM
```

El mensaje indica que el tema superaba su límite de tamaño, en este caso 1 GB (el límite de tamaño predeterminado). 

#### <a name="namespaces"></a>Espacios de nombres

Para espacios de nombres, [QuotaExceededException](/dotnet/api/microsoft.azure.servicebus.quotaexceededexception) puede indicar que una aplicación ha superado el número máximo de conexiones en un espacio de nombres. Por ejemplo:

```Output
Microsoft.ServiceBus.Messaging.QuotaExceededException: ConnectionsQuotaExceeded for namespace xxx.
<tracking-id-guid>_G12 ---> 
System.ServiceModel.FaultException`1[System.ServiceModel.ExceptionDetail]: 
ConnectionsQuotaExceeded for namespace xxx.
```

#### <a name="common-causes"></a>Causas comunes
Hay dos causas comunes de este error: la cola de mensajes fallidos y receptores de mensajes que no funcionan.

1. **[Cola de mensajes fallidos](service-bus-dead-letter-queues.md)** Un lector no puede completar los mensajes, que se devuelven a la cola o tema al expirar el bloqueo. Esto puede suceder si el lector encuentra una excepción que impide que llame a [BrokeredMessage.Complete](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.complete). Una vez que un mensaje se ha leído 10 veces, se mueve a la cola de mensajes fallidos de forma predeterminada. La propiedad [QueueDescription.MaxDeliveryCount](/dotnet/api/microsoft.servicebus.messaging.queuedescription.maxdeliverycount) controla este comportamiento, que tiene un valor predeterminado de 10. Al acumularse los mensajes en la cola de mensajes fallidos, estos ocupan espacio.
   
    Para resolver el problema, lea y complete los mensajes de la cola de mensajes fallidos, igual que haría si se encontraran en cualquier otra cola. Puede usar el método [FormatDeadLetterPath](/dotnet/api/microsoft.azure.servicebus.entitynamehelper.formatdeadletterpath) para ayudar a dar formato a la ruta de acceso de la cola de mensajes fallidos.
2. **Receptor detenido**. Un receptor ha dejado de recibir mensajes de una cola o suscripción. Para identificar este aspecto, eche un vistazo a la propiedad [QueueDescription.MessageCountDetails](/dotnet/api/microsoft.servicebus.messaging.messagecountdetails) , que muestra el desglose completo de los mensajes. Si la propiedad [ActiveMessageCount](/dotnet/api/microsoft.servicebus.messaging.messagecountdetails.activemessagecount) es alta o creciente, los mensajes no se leen tan rápido como se escriben.

### <a name="timeoutexception"></a>TimeoutException
[TimeoutException](https://msdn.microsoft.com/library/system.timeoutexception.aspx) indica que la operación iniciada por el usuario está superando el tiempo de espera. 

Debe comprobar el valor de la propiedad [ServicePointManager.DefaultConnectionLimit](https://msdn.microsoft.com/library/system.net.servicepointmanager.defaultconnectionlimit), porque si alcanza este límite también puede causar una excepción [TimeoutException](https://msdn.microsoft.com/library/system.timeoutexception.aspx).

#### <a name="queues-and-topics"></a>Colas y temas
Para las colas y los temas, el tiempo de espera se especifica en la propiedad [MessagingFactorySettings.OperationTimeout](/dotnet/api/microsoft.servicebus.messaging.messagingfactorysettings) como parte de la cadena de conexión o mediante [ServiceBusConnectionStringBuilder](/dotnet/api/microsoft.azure.servicebus.servicebusconnectionstringbuilder). El propio mensaje de error puede variar, pero siempre contiene el valor de tiempo de espera especificado para la operación actual. 

## <a name="connectivity-certificate-or-timeout-issues"></a>Problemas de conectividad, certificados o tiempo de espera
Los pasos siguientes pueden ayudarle a solucionar problemas de conectividad, certificados y tiempo de espera para todos los servicios de *.servicebus.windows.net. 

- Desplácese a `https://<yournamespace>.servicebus.windows.net/` o use [wget](https://www.gnu.org/software/wget/) ir allí. Le ayudará a comprobar si tiene problemas de cadena de certificados (lo más común al usar el SDK de Java), filtrado IP o red virtual.

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
- Ejecute el siguiente comando para comprobar si hay algún puerto bloqueado en el firewall. Los puertos empleados son 443 (HTTPS), 5671 (AMQP) y 9354 (mensajería .NET o SBMP). En función de la biblioteca que use, también se usan otros puertos. Este es el comando de ejemplo que comprueba si el puerto 5671 está bloqueado. 

    ```powershell
    tnc <yournamespacename>.servicebus.windows.net -port 5671
    ```

    En Linux:

    ```shell
    telnet <yournamespacename>.servicebus.windows.net 5671
    ```
- Si hay problemas de conectividad intermitentes, ejecute el siguiente comando para comprobar si hay paquetes descartados. Este comando intentará establecer 25 conexiones TCP diferentes cada segundo con el servicio. A continuación, puede comprobar cuántas de ellas se han realizado correctamente y cuántas han fallado y, además, ver la latencia de conexión TCP. Puede descargar la herramienta `psping` desde [aquí](/sysinternals/downloads/psping).

    ```shell
    .\psping.exe -n 25 -i 1 -q <yournamespace>.servicebus.windows.net:5671 -nobanner     
    ```
    Puede usar comandos equivalentes si utiliza otras herramientas como `tnc`, `ping`, etc. 
- Realice un seguimiento de red si los pasos anteriores no ayudan y analícelo con herramientas como [Wireshark](https://www.wireshark.org/). Si lo necesita, póngase en contacto con el [soporte técnico de Microsoft](https://support.microsoft.com/). 

## <a name="issues-that-may-occur-with-service-upgradesrestarts"></a>Problemas que se pueden producir con las actualizaciones o reinicios de servicios
Las actualizaciones y los reinicios de servicios back-end pueden tener el siguiente impacto en las aplicaciones:

- Puede que las solicitudes se limiten momentáneamente.
- Puede haber una caída en la llegada de mensajes o solicitudes entrantes.
- El archivo de registro puede contener mensajes de error.
- Puede que las aplicaciones se desconecten del servicio durante unos segundos.

Si el código de aplicación usa SDK, la directiva de reintento ya está integrada y activa. La aplicación se volverá a conectar sin un impacto significativo en la aplicación o el flujo de trabajo.

## <a name="next-steps"></a>Pasos siguientes

Para obtener la referencia completa de la API de .NET para Service Bus, consulte la [referencia de la API de .NET de Azure](/dotnet/api/overview/azure/service-bus).

Para más información sobre [Service Bus](https://azure.microsoft.com/services/service-bus/), vea los artículos siguientes:

* [Introducción a la mensajería de Service Bus](service-bus-messaging-overview.md)
* [Arquitectura de Service Bus](service-bus-architecture.md)

