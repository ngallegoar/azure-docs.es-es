---
title: Recuperación ante desastres con localización geográfica de Azure Service Bus | Microsoft Docs
description: Cómo usar regiones geográficas para conmutar por error y llevar a cabo una recuperación ante desastres en Azure Service Bus
services: service-bus-messaging
author: axisc
manager: timlt
editor: spelluru
ms.service: service-bus-messaging
ms.topic: article
ms.date: 04/29/2020
ms.author: aschhab
ms.openlocfilehash: a5a1e7a7ef73825b4b13d2f36c1c8554fdc2a9b6
ms.sourcegitcommit: fdec8e8bdbddcce5b7a0c4ffc6842154220c8b90
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/19/2020
ms.locfileid: "83647861"
---
# <a name="azure-service-bus-geo-disaster-recovery"></a>Recuperación ante desastres con localización geográfica de Azure Service Bus

Cuando hay regiones de Azure completas o centros de datos (si no se utilizan [zonas de disponibilidad](../availability-zones/az-overview.md)) que experimentan un tiempo de inactividad, es crucial que el procesamiento de datos siga funcionando en otra región o centro de datos. Por ello, la *recuperación ante desastres geográfica* es una característica importante para cualquier empresa. Azure Service Bus admite la recuperación ante desastres geográfica en el nivel del espacio de nombres.

La característica de recuperación ante desastres con localización geográfica está disponible globalmente para la SKU Premium de Service Bus. 

>[!NOTE]
> La recuperación ante desastres geográfica solo garantiza actualmente que los metadatos (colas, temas, suscripciones, filtros) se copiarán desde el espacio de nombres principal al espacio de nombres secundario cuando se emparejen.

## <a name="outages-and-disasters"></a>Interrupciones y desastres

Es importante tener en cuenta la distinción entre "interrupciones" y "desastres". 

Una *interrupción* es la falta de disponibilidad temporal de Azure Service Bus y puede afectar a algunos componentes del servicio, como un almacén de mensajes o incluso todo el centro de datos. Sin embargo, una vez corregido el problema, Service Bus vuelve a estar disponible. Normalmente, una interrupción no provoca la pérdida de mensajes ni otros datos. Un ejemplo de una interrupción de este tipo podría ser un error de corriente en el centro de datos. Algunas interrupciones son solo breves pérdidas de conexión debido a problemas transitorios o de red. 

Un *desastre* se define como la pérdida permanente o a largo plazo de un clúster, una región de Azure o un centro de datos de Service Bus. La región o el centro de datos no volverá necesariamente a estar disponible, o puede que esté fuera de servicio durante horas o días. Algunos ejemplos de esos desastres son los incendios, las inundaciones o los terremotos. Un desastre que se convierte en permanente podría provocar la pérdida de algunos mensajes, eventos u otros datos. Sin embargo, en la mayoría de los casos, no debe producirse una pérdida de datos y se pueden recuperar los mensajes una vez que se realiza la copia de seguridad del centro de datos.

La característica de recuperación ante desastres con localización geográfica de Azure Service Bus es una solución de recuperación ante desastres. Los conceptos y el flujo de trabajo descritos en este artículo se aplican a situaciones catastróficas y no a interrupciones transitorias o temporales. Para obtener una explicación detallada de la recuperación ante desastres en Microsoft Azure, consulte [este artículo](/azure/architecture/resiliency/disaster-recovery-azure-applications).   

## <a name="basic-concepts-and-terms"></a>Términos y conceptos básicos

La característica de recuperación ante desastres implementa la recuperación ante desastres de metadatos y depende de espacios de nombres de recuperación ante desastres principales y secundarios. Tenga en cuenta que la característica de recuperación ante desastres con localización geográfica solo está disponible para [SKU Premium](service-bus-premium-messaging.md). No es necesario realizar ningún cambio de la cadena de conexión, ya que la conexión se realiza a través de un alias.

Los siguientes términos se utilizan en este artículo:

-  *Alias*: el nombre para una configuración de recuperación ante desastres que ha configurado. El alias proporciona una sola cadena de conexión estable de nombre de dominio completo (FQDN). Las aplicaciones usan esta cadena de conexión de alias para conectarse a un espacio de nombres. El uso de un alias garantiza que la cadena de conexión permanecerá sin modificación cuando se desencadene la conmutación por error.

-  *Espacio de nombres principal o secundario*: los espacio de nombres que corresponden al alias. El espacio de nombres principal es "activo" y recibe mensajes (puede ser un espacio de nombres ya existente o uno nuevo). El espacio de nombres secundario es "pasivo" y no recibe mensajes. Los metadatos entre ambos están sincronizados, por lo que ambos pueden aceptar sin problemas mensajes sin ningún cambio de código de la aplicación o cadena de conexión. Para asegurarse de que solo el espacio de nombres activo recibe mensajes, tiene que utilizar el alias. 

-  *Metadatos*: entidades como colas, temas y suscripciones, y sus propiedades del servicio que están asociadas con el espacio de nombres. Tenga en cuenta que solo las entidades y sus valores se replican automáticamente. Los mensajes no se replican.

-  *Conmutación por error*: el proceso de activación del espacio de nombres secundario.

## <a name="setup"></a>Configurar

La siguiente sección es una introducción a la configuración del emparejamiento entre los espacios de nombres.

![1][]

El proceso de configuración es el siguiente:

1. Cree un espacio de nombres prémium de Service Bus ***principal***.

2. Aprovisione un espacio de nombres prémium de Service Bus ***secundario*** en una región *diferente de aquella en la que se aprovisiona el espacio de nombres principal*. Esto ayudará con el aislamiento de errores entre regiones de centro de datos diferentes.

3. Cree un emparejamiento entre el espacio de nombres principal y el secundario para obtener el ***alias***.

    >[!NOTE] 
    > Si ha [migrado el espacio de nombres de Azure Service Bus Standard a Azure Service Bus Premium](service-bus-migrate-standard-premium.md), debe utilizar el alias ya existente (es decir, la cadena de conexión de espacio de nombres de Service Bus Standard) para crear la configuración de recuperación ante desastres a través de la **PS/CLI** o la **API REST**.
    >
    >
    > Esto es porque, durante la migración, el nombre de DNS o la cadena de conexión de espacio de nombres de Azure Service Bus Standard se convierte automáticamente en un alias para el espacio de nombres de Azure Service Bus Premium.
    >
    > Las aplicaciones cliente deben utilizar este alias (es decir, la cadena de conexión de espacio de nombres de Azure Service Bus Standard) para conectarse al espacio de nombres Premium si se ha configurado el emparejamiento de recuperación ante desastres.
    >
    > Si usa el portal para realizar la configuración de recuperación ante desastres, se encargará de hacerlo de forma automática.


4. Use el ***alias*** obtenido en el paso 3 para conectar las aplicaciones cliente al espacio de nombres principal con recuperación ante desastres geográfica. Inicialmente, el alias apunta al espacio de nombres principal.

5. (Opcional) Agregue alguna supervisión para detectar si es necesario realizar una conmutación por error.

## <a name="failover-flow"></a>Flujo de conmutación por error

El cliente (y nunca Azure) es el que desencadena manualmente una conmutación por error (ya sea explícitamente mediante un comando o mediante una lógica de negocios propiedad del cliente que desencadena el comando). Esto otorga al cliente una propiedad y visibilidad total para la resolución de interrupciones en la red troncal de Azure.

![4][]

Después de que se desencadena la conmutación por error:

1. La cadena de conexión del ***alias*** se actualiza para que apunte al espacio de nombres prémium secundario.

2. Los clientes (remitentes y receptores) se conectan automáticamente al espacio de nombres secundario.

3. Se ha roto el emparejamiento existente entre el espacio de nombres prémium principal y el secundario.

Una vez que se inicia la conmutación por error:

1. En caso de otra interrupción, tiene que poder volver a realizar la conmutación por error. Por lo tanto, configure un segundo espacio de nombres pasivo y actualice el emparejamiento. 

2. Extraiga mensajes del espacio de nombres anteriormente principal una vez que vuelva a estar disponible. Después de eso, utilice ese espacio de nombres para la mensajería regular fuera de la configuración de recuperación con localización geográfica, o elimine el espacio de nombres principal antiguo.

> [!NOTE]
> Se admite solo la semántica de conmutación de reenvío. En este escenario, se realiza la conmutación por error y, a continuación, se vuelve a emparejar con un nuevo espacio de nombres. No se admite la conmutación por recuperación, por ejemplo en un clúster de SQL. 

Puede automatizar la conmutación por error con la supervisión de sistemas, o con soluciones de supervisión personalizadas. Sin embargo, dicha automatización necesita planeamiento y trabajo extra que se encuentran fuera del ámbito de este artículo.

![2][]

## <a name="management"></a>Administración

Si ha cometido algún error; por ejemplo, ha emparejado regiones incorrectas durante la configuración inicial, puede interrumpir el emparejamiento de los dos espacios de nombres en cualquier momento. Si desea usar los espacios de nombres emparejados como espacios de nombres normales, elimine el alias.

## <a name="use-existing-namespace-as-alias"></a>Uso de espacio de nombres existente como alias

Si tiene un escenario en el que no se pueden cambiar las conexiones de productores y consumidores, puede reutilizar el nombre del espacio de nombres como nombre de alias. Consulte el [código de ejemplo en GitHub aquí](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/GeoDR/SBGeoDR2/SBGeoDR_existing_namespace_name).

## <a name="samples"></a>Ejemplos

Los [ejemplos en GitHub](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/GeoDR/SBGeoDR2/) muestran cómo configurar e iniciar una conmutación por error. Estos ejemplos demuestran los conceptos siguientes:

- Una muestra de .NET y la configuración necesarias en Azure Active Directory para usar Azure Resource Manager con Service Bus para configurar y habilitar la recuperación ante desastres con localización geográfica.
- Pasos necesarios para ejecutar el código de ejemplo.
- Uso de un espacio de nombres existente como un alias.
- Pasos para habilitar la recuperación ante desastres con localización geográfica de manera alternativa a través de PowerShell o CLI.
- [Envío y recepción](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/GeoDR/TestGeoDR/ConsoleApp1) desde el espacio de nombres principal o secundario actual usando el alias.

## <a name="considerations"></a>Consideraciones

Tenga en cuenta y recuerde las siguientes consideraciones para esta versión:

1. En el planeamiento de la conmutación por error, también debe considerar el factor de tiempo. Por ejemplo, si se pierde la conectividad durante más de 15 a 20 minutos, puede decidir iniciar la conmutación por error.

2. El hecho de que no se replican datos significa que las sesiones activas en la actualidad no se replican. Además, la detección de duplicados y mensajes programados puede no funcionar. Funcionarán las nuevas sesiones, los mensajes programados nuevos y los duplicados nuevos. 

3. Conmutar por error una compleja infraestructura distribuida debe [ensayarse](/azure/architecture/reliability/disaster-recovery#disaster-recovery-plan) al menos una vez.

4. La sincronización de entidades puede tardar algún tiempo, aproximadamente 50-100 entidades por minuto. Las suscripciones y reglas también cuentan como entidades.

## <a name="availability-zones"></a>Zonas de disponibilidad

La SKU de Service Bus Premium es compatible con [Availability Zones](../availability-zones/az-overview.md), lo que proporciona ubicaciones con aislamiento de errores dentro de una región de Azure.

> [!NOTE]
> La compatibilidad de Availability Zones con Azure Service Bus Premium solo está disponible en aquellas [regiones de Azure](../availability-zones/az-region.md) en las que hay zonas de disponibilidad.

Solo puede habilitar Availability Zones en los espacios de nombres nuevos mediante Azure Portal. Service Bus no admite la migración de espacios de nombres existentes. No se puede deshabilitar la redundancia de zona después de habilitarla en el espacio de nombres.

![3][]

## <a name="private-endpoints"></a>Puntos de conexión privados
En esta sección se proporcionan consideraciones adicionales cuando se usa la recuperación ante desastres geográfica con espacios de nombres que utilizan puntos de conexión privados. Para obtener información sobre el uso de puntos de conexión privados con Service Bus en general, consulte [Integración de Azure Service Bus con Azure Private Link](private-link-service.md).

### <a name="new-pairings"></a>Nuevos emparejamientos
Si intenta crear un emparejamiento entre un espacio de nombres principal con un punto de conexión privado y un espacio de nombres secundario sin un punto de conexión privado, se producirá un error en el emparejamiento. El emparejamiento solo se realizará correctamente si los espacios de nombres principal y secundario tienen puntos de conexión privados. Se recomienda utilizar las mismas configuraciones en los espacios de nombres principal y secundario y en las redes virtuales en las que se creen los puntos de conexión privados. 

> [!NOTE]
> Al intentar emparejar el espacio de nombres principal con un punto de conexión privado y el espacio de nombres secundario, el proceso de validación solo comprueba si existe un punto de conexión privado en el espacio de nombres secundario. No comprueba si el punto de conexión funciona ahora o después de la conmutación por error. Es su responsabilidad asegurarse de que el espacio de nombres secundario con el punto de conexión privado funcionará según lo esperado después de la conmutación por error.
>
> Para probar que las configuraciones del punto de conexión privado son iguales, envíe una solicitud [Get queues](/rest/api/servicebus/queues/get) al espacio de nombres secundario desde fuera de la red virtual y compruebe que recibe un mensaje de error del servicio.

### <a name="existing-pairings"></a>Emparejamientos existentes
Si ya existe un emparejamiento entre el espacio de nombres principal y el secundario, se producirá un error en la creación del punto de conexión privado en el espacio de nombres principal. Para resolverlo, cree primero un punto de conexión privado en el espacio de nombres secundario y, a continuación, cree uno para el espacio de nombres principal.

> [!NOTE]
> Aunque se permite el acceso de solo lectura al espacio de nombres secundario, no se permiten las actualizaciones de las configuraciones de punto de conexión privado. 

### <a name="recommended-configuration"></a>Configuración recomendada
Al crear una configuración de recuperación ante desastres para la aplicación y Service Bus, debe crear puntos de conexión privados para los espacios de nombres de Service Bus principal y secundario en las redes virtuales que hospeden las instancias principales y secundarias de la aplicación.

Supongamos que tiene dos redes virtuales, VNET-1 y VNET-2, y estos espacios de nombres principal y secundario: ServiceBus-Namespace1-Primary, ServiceBus-Namespace2-Secondary. Debe seguir estos pasos: 

- En ServiceBus-Namespace1-Primary, cree dos puntos de conexión privados que usen subredes de VNET-1 y VNET-2.
- En ServiceBus-Namespace2-Secondary, cree dos puntos de conexión privados que usen las mismas subredes de VNET-1 y VNET-2. 

![Puntos de conexión privados y redes virtuales](./media/service-bus-geo-dr/private-endpoints-virtual-networks.png)


La ventaja de este enfoque es que la conmutación por error puede producirse en el nivel de aplicación independiente del espacio de nombres de Service Bus. Considere los casos siguientes: 

**Conmutación por error solo de la aplicación:** en este caso, la aplicación no existirá en VNET-1, sino que se moverá a VNET-2. Como ambos puntos de conexión privados están configurados tanto en VNET-1 como en VNET-2 para los espacios de nombres principal y secundario, la aplicación funcionará. 

**Conmutación por error solo del espacio de nombres de Service Bus**: en este caso, dado que los dos puntos de conexión privados están configurados en ambas redes virtuales para los espacios de nombres principal y secundario, la aplicación funcionará. 

> [!NOTE]
> Para obtener instrucciones sobre la recuperación ante desastres con localización geográfica de una red virtual, consulte [Virtual Network: continuidad del negocio](../virtual-network/virtual-network-disaster-recovery-guidance.md).

## <a name="next-steps"></a>Pasos siguientes

- Consulte la [referencia de la API de REST](/rest/api/servicebus/disasterrecoveryconfigs) de la recuperación ante desastres con localización geográfica aquí.
- Ejecute el [ejemplo de recuperación](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/GeoDR/SBGeoDR2/SBGeoDR2) ante desastres con localización geográfica en GitHub.
- Consulte el [ejemplo en el que se envían mensajes a un alias](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/GeoDR/TestGeoDR/ConsoleApp1) de la recuperación ante desastres con localización geográfica.

Para más información sobre la mensajería de Service Bus, consulte los siguientes artículos:

* [Colas, temas y suscripciones de Service Bus](service-bus-queues-topics-subscriptions.md)
* [Introducción a las colas de Service Bus](service-bus-dotnet-get-started-with-queues.md)
* [Uso de temas y suscripciones de Service Bus](service-bus-dotnet-how-to-use-topics-subscriptions.md)
* [API DE REST](/rest/api/servicebus/) 

[1]: ./media/service-bus-geo-dr/geodr_setup_pairing.png
[2]: ./media/service-bus-geo-dr/geo2.png
[3]: ./media/service-bus-geo-dr/az.png
[4]: ./media/service-bus-geo-dr/geodr_failover_alias_update.png
