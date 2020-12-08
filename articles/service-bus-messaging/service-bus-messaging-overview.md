---
title: Información general sobre la mensajería de Azure Service Bus | Microsoft Docs
description: En este artículo se proporciona información general de alto nivel sobre Azure Service Bus, un agente de mensajes de integración empresarial totalmente administrado.
ms.topic: overview
ms.date: 11/20/2020
ms.openlocfilehash: ca04b4a41489fba61e7c0ee2bb3f4f7f0b8b80f4
ms.sourcegitcommit: d60976768dec91724d94430fb6fc9498fdc1db37
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/02/2020
ms.locfileid: "96489486"
---
# <a name="what-is-azure-service-bus"></a>Qué es Azure Service Bus
Microsoft Azure Service Bus es un agente de mensajes empresarial totalmente administrado que incluye colas de mensajes y temas de suscripción pública. Service Bus se usa para desacoplar aplicaciones de servicios, lo que proporciona las siguientes ventajas:

- Equilibrio de carga del trabajo entre trabajadores paralelos
- Enrutamiento y transferencia de datos de forma segura y control entre los límites de aplicaciones y servicios
- Coordinación del trabajo transaccional que requiere un alto grado de confiabilidad 

## <a name="overview"></a>Información general
Los datos se transfieren entre distintas aplicaciones y servicios mediante *mensajes*. Un mensaje es un contenedor decorado con metadatos que contiene datos. Los datos pueden ser cualquier tipo de información, como datos estructurados codificados con los formatos comunes, como los siguientes: JSON, XML, Apache Avro, texto sin formato.

Algunos escenarios de mensajería comunes son:

* *Mensajería*. Transferir datos empresariales, como pedidos de ventas o compras, diarios o movimientos de inventario.
* *Desacoplar aplicaciones*. Mejore la confiabilidad y la escalabilidad de las aplicaciones y los servicios. No es necesario que el productor y el consumidor estén en línea o disponibles al mismo tiempo. La [carga se redistribuye](/azure/architecture/patterns/queue-based-load-leveling), de tal forma que los picos de tráfico no sobrecarguen un servicio. 
* *Equilibrio de carga*. Permite que varios [consumidores paralelos](/azure/architecture/patterns/competing-consumers) lean una cola al mismo tiempo y que cada uno obtenga de forma segura la propiedad exclusiva de mensajes específicos. 
* *Temas y suscripciones*. Permite relaciones 1:*n* entre [publicadores y suscriptores](/azure/architecture/patterns/publisher-subscriber), lo que hace que los suscriptores puedan seleccionar mensajes concretos de una secuencia de mensajes publicados.
* *Transacciones*. Permite realizar varias operaciones, todas ellas en el ámbito de una transacción atómica. Por ejemplo, se pueden realizar las siguientes operaciones en el ámbito de una transacción.  

    1. Obtener un mensaje de una cola.
    2. Publicar resultados de procesamiento en una o varias colas diferentes.
    3. Mover el mensaje de entrada desde la cola original. 
    
    Los resultados se hacen visibles para los consumidores de nivel inferior solo si son correctos, incluida la liquidación correcta del mensaje de entrada, lo que permite una semántica de procesamiento de una sola vez. Este modelo de transacción es una base sólida para el patrón de [transacciones de compensación](https://docs.microsoft.com/azure/architecture/patterns/compensating-transaction.md) en el contexto de una solución mayor. 
* *Sesiones de mensajes*. Implemente la coordinación a gran escala de flujos de trabajo y transferencias multiplexadas que requieran un ordenamiento o un aplazamiento de mensajes estricto.

Si está familiarizado con otros agentes de mensajes, como Apache ActiveMQ, los conceptos de Service Bus son parecidos a los que ya conoce. Como Service Bus es una oferta de plataforma como servicio (PaaS), una diferencia clave es que no es necesario preocuparse por las siguientes acciones. Azure se encarga de esas tareas. 

- Colocar los registros y administrar el espacio en disco.
- Administrar las copias de seguridad.
- Mantener revisados los sistemas operativos o productos.
- Preocuparse por los errores de hardware. 
- Conmutar por error a una máquina de reserva.

## <a name="compliance-with-standards-and-protocols"></a>Cumplimiento de estándares y protocolos

El protocolo de conexión principal de Service Bus es [Advanced Message Queuing Protocol (AMQP) 1.0](service-bus-amqp-overview.md), un estándar abierto ISO/IEC. Este protocolo permite a los clientes escribir aplicaciones que funcionan en agentes de Service Bus y locales, como ActiveMQ o RabbitMQ. En la [guía del protocolo AMQP](service-bus-amqp-protocol-guide.md) se proporciona información detallada en caso de que quiera crear una abstracción de este tipo.

[Service Bus Premium](service-bus-premium-messaging.md) es totalmente compatible con la API [Java Message Service (JMS) 2.0](how-to-use-java-message-service-20.md) de Java/Jakarta EE. Además, Service Bus Standard admite el subconjunto JMS 1.1 centrado en las colas. JMS es una abstracción común de los agentes de mensajes y se integra con muchas aplicaciones y marcos, incluido el conocido marco Spring. Para pasar de otros agentes a Azure Service Bus, solo tiene que volver a crear la topología de colas y temas y cambiar la configuración y las dependencias del proveedor del cliente. Puede encontrar un ejemplo en la [guía de migración de ActiveMQ](migrate-jms-activemq-to-servicebus.md).

## <a name="concepts-and-terminology"></a>Conceptos y terminología 
En esta sección se describen los conceptos y la terminología de Service Bus.

### <a name="namespaces"></a>Espacios de nombres
Un espacio de nombres es un contenedor para todos los componentes de mensajería. Varias colas y temas pueden estar en un solo espacio de nombres, y los espacios de nombres suelen servir como contenedores de aplicación. 

Un espacio de nombres puede compararse con un "servidor" en la terminología de otros agentes, pero los conceptos no son directamente equivalentes. Un espacio de nombres de Service Bus es su propio segmento de capacidad de un clúster grande compuesto por docenas de máquinas virtuales activas. Opcionalmente, puede abarcar tres [zonas de disponibilidad de Azure](../availability-zones/az-overview.md). Por lo tanto, obtiene todas las ventajas de disponibilidad y solidez de la ejecución del agente de mensajes a gran escala. Además, no es necesario preocuparse por las complejidades subyacentes. Service Bus es la mensajería "sin servidor".

### <a name="queues"></a>Colas
Los mensajes se envían y se reciben desde *colas*. Las colas almacenan mensajes hasta que la aplicación receptora está disponible para recibirlos y procesarlos.

![Cola](./media/service-bus-messaging-overview/about-service-bus-queue.png)

Los mensajes de las colas se ordenan y se les asigna una marca de tiempo a su llegada. Una vez aceptados por el agente, el mensaje siempre se mantiene de forma duradera en un almacenamiento con redundancia triple, distribuido entre las zonas de disponibilidad si el espacio de nombres está habilitado para zonas. Service Bus nunca deja mensajes en memoria o almacenamiento volátil una vez que se han notificado al cliente como aceptados.

Los mensajes se entregan en modo de *extracción* y solo entregan mensajes cuando se solicitan. A diferencia del modelo de sondeo de estado ocupado de otras colas de nube, la operación de extracción puede ser de larga duración y completarse solamente una vez que un mensaje esté disponible. 

### <a name="topics"></a>Temas

También puede usar *temas* para enviar y recibir mensajes. Mientras que una cola se utiliza a menudo para la comunicación punto a punto, los temas son útiles en escenarios de publicación y suscripción.

![Tema](./media/service-bus-messaging-overview/about-service-bus-topic.png)

Los temas pueden tener varias suscripciones independientes asociadas; por lo demás, funcionan exactamente igual que las colas del lado receptor. Un suscriptor a un tema puede recibir una copia de cada mensaje enviado a ese tema. Las suscripciones son entidades con nombre. De forma predeterminada, las suscripciones son duraderas, pero se pueden configurar para que expiren y se eliminen luego automáticamente. Mediante la API de JMS, Service Bus Premium también permite crear suscripciones volátiles que existen mientras dura la conexión.

Puede definir reglas en una suscripción. Una regla de suscripción tiene un *filtro* para definir una condición del mensaje que se va a copiar en la suscripción, y una acción *opcional* que puede modificar los metadatos del mensaje. Para más información, consulte [Filtros y acciones de temas](topic-filters.md). Esta característica es útil en los escenarios siguientes:

- No quiere que una suscripción reciba todos los mensajes enviados a un tema.
- Quiere marcar los mensajes con metadatos adicionales cuando pasan a través de una suscripción.

## <a name="advanced-features"></a>Características avanzadas

Service Bus incluye características avanzadas que le permiten resolver problemas de mensajería más complejos. En las secciones siguientes se describen algunas de estas características.

### <a name="message-sessions"></a>Sesiones de mensajes

Para crear una garantía primero en entrar/primero en salir (FIFO) en Service Bus, use sesiones. Las sesiones de mensajes permiten la administración ordenada y exclusiva de secuencias de mensajes relacionados no enlazadas. Para permitir la administración de sesiones en sistemas de alta disponibilidad y a gran escala, la característica de sesión también permite almacenar el estado de la sesión, lo que hace que las sesiones se muevan de forma segura entre controladores. Para más información, consulte [Sesiones de mensajes: primera en entrar, primero en salir (FIFO)](message-sessions.md).

### <a name="autoforwarding"></a>Artículo reenvío automático

La característica de reenvío automático encadena una cola o suscripción a otra cola o tema dentro del mismo espacio de nombres. Al usar esta característica, Service Bus mueve automáticamente los mensajes de una cola o suscripción a una cola o tema de destino. Todos estos movimientos se realizan de manera transaccional. Para más información, consulte [Encadenamiento de Service Bus entidades con el reenvío](service-bus-auto-forwarding.md).

### <a name="dead-letter-queue"></a>Cola de mensajes fallidos

Todas las colas y suscripciones a temas de Service Bus llevan asociada una cola de mensajes con problemas de entrega (DLQ). Esta cola contiene mensajes que cumplen los criterios siguientes: 

- No se pueden entregar correctamente a ningún receptor.
- Se les ha agotado el tiempo de espera.
- La aplicación receptora los deja a un lado explícitamente. 

Los mensajes de la cola de mensajes con problemas de entrega se anotan con el motivo de que se colocaran allí. La cola de mensajes con problemas de entregas tiene un punto de conexión especial, pero, por lo demás, funciona como cualquier cola normal. Una aplicación o herramienta puede examinar una cola de mensajes con problemas de entrega o quitarlos de esta. También puede reenviar automáticamente una cola de mensajes con problemas de entrega. Para más información, consulte [Introducción a las colas de mensajes fallidos de Service Bus](service-bus-dead-letter-queues.md).

### <a name="scheduled-delivery"></a>Entrega programada

Puede enviar mensajes a una cola o tema para procesarlos más tarde y establecer una hora a la que el mensaje estará disponible para su consumo. Los mensajes programados también se pueden cancelar. Para más información, consulte [Mensajes programados](message-sequencing.md#scheduled-messages).

### <a name="message-deferral"></a>Aplazamiento de mensajes

El cliente de una cola o suscripción puede aplazar la recuperación de un mensaje recibido para otro momento. Es posible que el mensaje se haya publicado fuera del orden esperado y que el cliente desee esperar hasta recibir otro mensaje. Los mensajes aplazados permanecen en la cola o suscripción y se deben reactivar explícitamente mediante el número de secuencia asignado por el servicio. Para más información, consulte [ Aplazamiento de mensajes](message-deferral.md).

### <a name="batching"></a>Lotes

El procesamiento por lotes del lado cliente permite que el cliente de una cola o tema acumule un conjunto de mensajes y los transfiera juntos. Este sistema se suele emplear para ahorrar ancho de banda o para aumentar el rendimiento. Para más información, consulte [Procesamiento por lotes del lado cliente](service-bus-performance-improvements.md#client-side-batching).

### <a name="transactions"></a>Transacciones

Una transacción agrupa dos o más operaciones en un *ámbito de ejecución*. Service Bus permite agrupar las operaciones en varias entidades de mensajería dentro del ámbito de una única transacción. Una entidad de mensaje puede ser una cola, un tema o una suscripción. Para más información, consulte [Información general sobre el procesamiento de transacciones de Service Bus](service-bus-transactions.md).

### <a name="autodelete-on-idle"></a>Eliminación automática en estado inactivo

La eliminación automática en modo inactivo le permite especificar un intervalo de inactividad después del cual se elimina automáticamente la suscripción a una cola o tema. La duración mínima es de 5 minutos. Para más información, consulte la [propiedadQueueDescription.AutoDeleteOnIdle](/dotnet/api/microsoft.servicebus.messaging.queuedescription.autodeleteonidle).

### <a name="duplicate-detection"></a>Detección de duplicados
La característica de detección de duplicados permite al remitente volver a enviar el mismo mensaje y al agente quitar un posible duplicado. Esta característica se basa en el seguimiento de la propiedad `message-id` de un mensaje, lo que significa que la aplicación debe tener cuidado de usar el mismo valor al reenviar el mensaje; este valor podría obtenerse directamente de algún contexto específico de la aplicación. Para más información, consulte [Detección de duplicados](duplicate-detection.md).

### <a name="geo-disaster-recovery"></a>Recuperación ante desastres geográfica

Cuando una región de Azure experimenta tiempo de inactividad, la característica de recuperación ante desastres permite que el procesamiento de datos siga funcionando en otra región o centro de datos. La característica mantiene un reflejo estructural de un espacio de nombres disponible en la región secundaria y permite que la identidad del espacio de nombres cambie al espacio de nombres secundario. Los mensajes que ya se han publicado permanecen en el espacio de nombres principal anterior a la espera de recuperarse una vez que mejora el incidente de disponibilidad. Para obtener más información, consulte [Recuperación ante desastres con localización geográfica de Azure Service Bus](service-bus-geo-dr.md).

### <a name="security"></a>Seguridad

Service Bus admite los protocolos estándar [AMQP 1,0](service-bus-amqp-overview.md) y [HTTP o REST](/rest/api/servicebus/) y sus respectivas utilidades de seguridad, incluida la seguridad de nivel de transporte (TLS). Los clientes pueden tener acceso autorizado mediante [Firma de acceso compartido](service-bus-sas.md) o la seguridad basada en roles de [Azure Active Directory](service-bus-authentication-and-authorization.md). 

Para proteger contra el tráfico no deseado, Service Bus proporciona [características de seguridad](network-security.md), como el firewall de IP y la integración con redes virtuales. 

## <a name="client-libraries"></a>Bibliotecas de clientes

Todas bibliotecas cliente de Service Bus están disponibles a través de Azure SDK.

- [Azure Service Bus para .NET](/dotnet/api/overview/azure/service-bus?preserve-view=true&view=azure-dotnet)
- [Bibliotecas de Azure Service Bus para Java](/java/api/overview/azure/servicebus?preserve-view=true&view=azure-java-stable)
- [Proveedor de Azure Service Bus para Java JMS 2.0](how-to-use-java-message-service-20.md)
- [Módulos de Azure Service Bus para JavaScript y TypeScript](/javascript/api/overview/azure/service-bus?preserve-view=true&view=azure-node-latest)
- [Bibliotecas de Azure Service Bus para Python](/python/api/overview/azure/servicebus?preserve-view=true&view=azure-python)

El [protocolo principal de Azure Service Bus es AMQP 1.0](service-bus-amqp-overview.md) y se puede usar desde cualquier cliente compatible con este protocolo. Varios clientes de AMQP de código abierto tienen ejemplos que demuestran explícitamente la interoperabilidad de Service Bus. Revise la [guía del protocolo AMQP 1.0](service-bus-amqp-protocol-guide.md) para aprender a usar las características de Service Bus directamente con los clientes de AMQP 1.0.

[!INCLUDE [messaging-oss-amqp-stacks.md](../../includes/messaging-oss-amqp-stacks.md)]

## <a name="integration"></a>Integración

Service Bus se integra totalmente con muchos servicios de Microsoft y Azure, por ejemplo:

* [Event Grid](https://azure.microsoft.com/services/event-grid/)
* [Logic Apps](https://azure.microsoft.com/services/logic-apps/)
* [Funciones de Azure](https://azure.microsoft.com/services/functions/)
* [Power Platform](https://powerplatform.microsoft.com/)
* [Dynamics 365](https://dynamics.microsoft.com)
* [Azure Stream Analytics](https://azure.microsoft.com/services/stream-analytics/)

## <a name="next-steps"></a>Pasos siguientes

Para comenzar a trabajar con la mensajería de Service Bus, consulte los siguientes artículos:

* Para comparar los servicios de mensajería de Azure, consulte [Comparación de los servicios](../event-grid/compare-messaging-services.md?toc=%2fazure%2fservice-bus-messaging%2ftoc.json&bc=%2fazure%2fservice-bus-messaging%2fbreadcrumb%2ftoc.json).
* Pruebe las guías de inicio rápido para [.NET](service-bus-dotnet-get-started-with-queues.md), [Java](service-bus-java-how-to-use-queues.md) o [JMS](service-bus-java-how-to-use-jms-api-amqp.md).
* Para administrar los recursos de Service Bus, consulte [Explorador de Service Bus](https://github.com/paolosalvatori/ServiceBusExplorer/releases).
* Más información sobre los niveles Estándar y Premium y sus precios, consulte [precios de Service Bus](https://azure.microsoft.com/pricing/details/service-bus/).
* Para más información sobre el rendimiento y la latencia del nivel Premium, consulte [Mensajería Premium](https://techcommunity.microsoft.com/t5/Service-Bus-blog/Premium-Messaging-How-fast-is-it/ba-p/370722).