---
title: 'Recuperación ante desastres geográfica: Azure Event Hubs| Microsoft Docs'
description: Cómo usar regiones geográficas para conmutar por error y llevar a cabo una recuperación ante desastres en Azure Event Hubs
services: event-hubs
documentationcenter: ''
author: ShubhaVijayasarathy
manager: timlt
editor: ''
ms.service: event-hubs
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.custom: seodec18
ms.date: 12/06/2018
ms.author: shvija
ms.openlocfilehash: 61318fbccdf92c6502aa8b2236d8b234cec67668
ms.sourcegitcommit: 34a6fa5fc66b1cfdfbf8178ef5cdb151c97c721c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2020
ms.locfileid: "82209152"
---
# <a name="azure-event-hubs---geo-disaster-recovery"></a>Azure Event Hubs: recuperación ante desastres geográfica 

Cuando hay regiones de Azure completas o centros de datos (si no se utilizan [zonas de disponibilidad](../availability-zones/az-overview.md)) que experimentan un tiempo de inactividad, es crucial que el procesamiento de datos siga funcionando en otra región o centro de datos. De esta forma, la *recuperación ante desastres con localización geográfica* y la *replicación geográfica* son características importantes para cualquier empresa. Azure Event Hubs admite tanto la recuperación ante desastres con localización geográfica como la replicación geográfica, en el nivel de espacio de nombres. 

> [!NOTE]
> La característica de recuperación ante desastres geográfica solo está disponible para las [SKU estándar y dedicadas](https://azure.microsoft.com/pricing/details/event-hubs/).  

## <a name="outages-and-disasters"></a>Interrupciones y desastres

Es importante tener en cuenta la distinción entre "interrupciones" y "desastres". Una *interrupción* es la falta de disponibilidad temporal de Azure Event Hubs y puede afectar a algunos componentes del servicio, como un almacén de mensajes o incluso todo el centro de datos. Sin embargo, después de corregir el problema, Event Hubs está de nuevo disponible. Normalmente, una interrupción no provoca la pérdida de mensajes ni otros datos. Un ejemplo de una interrupción de este tipo podría ser un error de corriente en el centro de datos. Algunas interrupciones son solo breves pérdidas de conexión debido a problemas transitorios o de red. 

Un *desastre* se define como la pérdida permanente o a largo plazo de un clúster de Event Hubs, una región de Azure o un centro de datos. La región o el centro de datos no volverá necesariamente a estar disponible, o puede que esté fuera de servicio durante horas o días. Algunos ejemplos de esos desastres son los incendios, las inundaciones o los terremotos. Un desastre que se convierte en permanente podría provocar la pérdida de algunos mensajes, eventos u otros datos. Sin embargo, en la mayoría de los casos, no debe producirse una pérdida de datos y se pueden recuperar los mensajes una vez que se realiza la copia de seguridad del centro de datos.

La característica de recuperación ante desastres con localización geográfica de Azure Event Hubs es una solución de recuperación ante desastres. Los conceptos y el flujo de trabajo descritos en este artículo se aplican a situaciones catastróficas y no a interrupciones transitorias o temporales. Para obtener una explicación detallada de la recuperación ante desastres en Microsoft Azure, consulte [este artículo](/azure/architecture/resiliency/disaster-recovery-azure-applications).

## <a name="basic-concepts-and-terms"></a>Términos y conceptos básicos

La característica de recuperación ante desastres implementa la recuperación ante desastres de metadatos y depende de espacios de nombres de recuperación ante desastres principales y secundarios. 

La característica de recuperación ante desastres geográfica solo está disponible para las [SKU estándar y dedicadas](https://azure.microsoft.com/pricing/details/event-hubs/). No es necesario realizar ningún cambio de la cadena de conexión, ya que la conexión se realiza a través de un alias.

Los siguientes términos se utilizan en este artículo:

-  *Alias*: el nombre para una configuración de recuperación ante desastres que ha configurado. El alias proporciona una sola cadena de conexión estable de nombre de dominio completo (FQDN). Las aplicaciones usan esta cadena de conexión de alias para conectarse a un espacio de nombres. 

-  *Espacio de nombres principal o secundario*: los espacios de nombres que corresponden al alias. El espacio de nombres principal es "activo" y recibe mensajes (puede ser un espacio de nombres ya existente o uno nuevo). El espacio de nombres secundario es "pasivo" y no recibe mensajes. Los metadatos entre ambos están sincronizados, por lo que ambos pueden aceptar sin problemas mensajes sin ningún cambio de código de la aplicación o cadena de conexión. Para asegurarse de que solo el espacio de nombres activo recibe mensajes, tiene que utilizar el alias. 

-  *Metadatos*: entidades como centros de eventos y grupos de consumidores; y sus propiedades del servicio que están asociadas con el espacio de nombres. Tenga en cuenta que solo las entidades y sus valores se replican automáticamente. No se replican los mensajes ni los eventos. 

-  *Conmutación por error*: El proceso de activación del espacio de nombres secundario.

## <a name="supported-namespace-pairs"></a>Pares de espacios de nombres admitidos
Se admiten las siguientes combinaciones de espacios de nombres principales y secundarios:  

| Espacio de nombres principal | Espacio de nombres secundario | Compatible | 
| ----------------- | -------------------- | ---------- |
| Estándar | Estándar | Sí | 
| Estándar | Dedicado | Sí | 
| Dedicado | Dedicado | Sí | 
| Dedicado | Estándar | No | 

> [!NOTE]
> No se pueden emparejar espacios de nombres que se encuentran en el mismo clúster dedicado. Puede emparejar espacios de nombres que se encuentran en clústeres independientes. 

## <a name="setup-and-failover-flow"></a>Flujo de conmutación por error y configuración

La siguiente sección contiene información general del proceso de conmutación por error y explica cómo configurar la conmutación por error inicial. 

![1][]

### <a name="setup"></a>Configurar

En primer lugar cree un espacio de nombres principal o use uno ya existente, y un nuevo espacio de nombres secundario, luego emparéjelos. Este emparejamiento le proporciona un alias que puede usar para conectarse. Al usar un alias, no es necesario que cambie las cadenas de conexión. Solo pueden agregarse nuevos espacios de nombres al emparejamiento de la conmutación por error. Por último, debe agregar alguna supervisión para detectar si es necesario realizar una conmutación por error. En la mayoría de los casos, el servicio forma parte de un ecosistema mayor, por lo tanto las conmutaciones por error automáticas raramente son posibles, ya que a menudo las conmutaciones por error tienen que realizarse en sincronía con el subsistema o infraestructura restantes.

### <a name="example"></a>Ejemplo

En un ejemplo de este escenario, se considera una solución de punto de venta (POS) que emite mensajes o eventos. Event Hubs pasa esos eventos a alguna solución de asignación o formato, que reenvía los datos asignados a otros sistema para continuar el procesamiento. En ese momento, todos estos sistemas se pueden hospedar en la misma región de Azure. La decisión sobre cuándo y en qué parte se realizará la conmutación por error depende del flujo de datos en su infraestructura. 

Puede automatizar la conmutación por error con la supervisión de sistemas, o con soluciones de supervisión personalizadas. Sin embargo, dicha automatización necesita planeamiento y trabajo extra que se encuentran fuera del ámbito de este artículo.

### <a name="failover-flow"></a>Flujo de conmutación por error

Si inicia la conmutación por error, se requieren dos pasos:

1. En caso de otra interrupción, tiene que poder volver a realizar la conmutación por error. Por lo tanto, configure un segundo espacio de nombres pasivo y actualice el emparejamiento. 

2. Extraiga mensajes del espacio de nombres anteriormente principal una vez que vuelva a estar disponible. Después de eso, utilice ese espacio de nombres para la mensajería regular fuera de la configuración de recuperación con localización geográfica, o elimine el espacio de nombres principal antiguo.

> [!NOTE]
> Se admite solo la semántica de conmutación de reenvío. En este escenario, se realiza la conmutación por error y, a continuación, se vuelve a emparejar con un nuevo espacio de nombres. No se admite la conmutación por recuperación, por ejemplo en un clúster de SQL. 

![2][]

## <a name="management"></a>Administración

Si ha cometido algún error; por ejemplo, ha emparejado regiones incorrectas durante la configuración inicial, puede interrumpir el emparejamiento de los dos espacios de nombres en cualquier momento. Si desea usar los espacios de nombres emparejados como espacios de nombres normales, elimine el alias.

## <a name="samples"></a>Ejemplos

En el [ejemplo de GitHub](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/GeoDRClient) se muestra cómo configurar e iniciar una conmutación por error. En este ejemplo se demuestran los siguientes conceptos:

- La configuración necesaria en Azure Active Directory para usar Azure Resource Manager con Event Hubs. 
- Pasos necesarios para ejecutar el código de ejemplo. 
- Envío y recepción desde el espacio de nombres principal actual. 

## <a name="considerations"></a>Consideraciones

Tenga en cuenta y recuerde las siguientes consideraciones para esta versión:

1. Por motivos de diseño, la recuperación ante desastres geográfica de Event Hubs no replica datos y, por lo tanto, no se puede volver a usar el valor de desplazamiento anterior del centro de eventos principal en el centro de eventos secundario. Se recomienda reiniciar el receptor de eventos con uno de los siguientes métodos:

- *EventPosition.FromStart()* : si quiere leer todos los datos en el centro de eventos secundario.
- *EventPosition.FromEnd()* : si quiere leer todos los datos nuevos desde el momento de conexión con el centro de eventos secundario.
- *EventPosition.FromEnqueuedTime(dateTime)* : si quiere leer todos los datos recibidos en el centro de eventos secundario a partir de una hora y fecha determinadas.

2. En el planeamiento de la conmutación por error, también debe considerar el factor de tiempo. Por ejemplo, si se pierde la conectividad durante más de 15 a 20 minutos, puede decidir iniciar la conmutación por error. 
 
3. El hecho de que no se replican datos significa que las sesiones activas en la actualidad no se replican. Además, la detección de duplicados y mensajes programados puede no funcionar. Funcionarán las nuevas sesiones, los mensajes programados y los duplicados nuevos. 

4. Conmutar por error una compleja infraestructura distribuida debe [ensayarse](/azure/architecture/reliability/disaster-recovery#disaster-recovery-plan) al menos una vez. 

5. La sincronización de entidades puede tardar algún tiempo, aproximadamente 50-100 entidades por minuto.

## <a name="availability-zones"></a>Zonas de disponibilidad 

La SKU de Event Hubs estándar es compatible con [Availability Zones](../availability-zones/az-overview.md), lo que proporciona ubicaciones con aislamiento de errores dentro de una región de Azure. 

> [!NOTE]
> La compatibilidad de Availability Zones con Azure Event Hubs estándar solo está disponible en aquellas [regiones de Azure](../availability-zones/az-region.md) en las que hay zonas de disponibilidad.

Solo puede habilitar Availability Zones en los espacios de nombres nuevos mediante Azure Portal. Event Hubs no admite la migración de espacios de nombres existentes. No se puede deshabilitar la redundancia de zona después de habilitarla en el espacio de nombres.

![3][]

## <a name="next-steps"></a>Pasos siguientes

* El [ejemplo en GitHub](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/GeoDRClient) le guía a través de un flujo de trabajo simple que crea un emparejamiento geográfico e inicia una conmutación por error para un escenario de recuperación ante desastres.
* La [referencia de la API de REST](/rest/api/eventhub/disasterrecoveryconfigs) describe las API para llevar a cabo la configuración de recuperación de desastres con localización geográfica.

Para obtener más información acerca de Event Hubs, visite los vínculos siguientes:

- Introducción a Event Hubs
    - [.NET Core](get-started-dotnet-standard-send-v2.md)
    - [Java](get-started-java-send-v2.md)
    - [Python](get-started-python-send-v2.md)
    - [JavaScript](get-started-java-send-v2.md)
* [Preguntas más frecuentes sobre Event Hubs](event-hubs-faq.md)
* [Aplicaciones de ejemplo que usan Event Hubs](https://github.com/Azure/azure-event-hubs/tree/master/samples)

[1]: ./media/event-hubs-geo-dr/geo1.png
[2]: ./media/event-hubs-geo-dr/geo2.png
[3]: ./media/event-hubs-geo-dr/eh-az.png
