---
title: 'Introducción a los centros de eventos dedicados: Azure Event Hubs | Microsoft Docs'
description: En este artículo, se proporciona información general sobre Azure Event Hubs dedicado, que permite realizar implementaciones de centros de eventos con un único inquilino.
services: event-hubs
documentationcenter: na
author: ShubhaVijayasarathy
manager: timlt
editor: ''
ms.assetid: ''
ms.service: event-hubs
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.custom: seodec18
ms.date: 12/06/2018
ms.author: shvija
ms.openlocfilehash: f67be1d31125b21048deca4d9cafcc76f4ffc3b1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "72516747"
---
# <a name="overview-of-event-hubs-dedicated"></a>Introducción a Event Hubs dedicado

Los *clústeres de Event Hubs* disponen de implementaciones con un único inquilino, ideales para aquellos clientes con las necesidades de streaming más exigentes. Esta oferta de inquilino único tiene un acuerdo de nivel de servicio garantizado del 99,99 % y solo está disponible en el plan de tarifa Dedicado. Los clústeres de Event Hubs pueden admitir la entrada de millones de eventos por segundo con una capacidad garantizada y una latencia inferior a un segundo. Los espacios de nombres y los centros de eventos creados en el clúster Dedicado incluyen todas las características de la oferta Estándar y muchas otras, pero sin imponer límites de entrada. También incluye la popular característica [Event Hubs Capture](event-hubs-capture-overview.md) sin costo adicional, que permite crear lotes de flujos de datos y transmitirlos a Azure Storage o Azure Data Lake de forma automática. 

Los clústeres se aprovisionan y facturan por **unidades de capacidad (CU)** , una cantidad de recursos de CPU y memoria asignada previamente. Puede adquirir 1, 2, 4, 8, 12, 16 o 20 CU para cada clúster. La cantidad de datos que se puede ingerir y transmitir en cada unidad de capacidad depende de diversos factores, como el número de productores y consumidores, la forma de la carga y la velocidad de salida (consulte los resultados de las pruebas comparativas a continuación para más información). 

> [!NOTE]
> De forma predeterminada, todos los clústeres de Event Hubs están habilitados para Kafka y admiten puntos de conexión de Kafka que se pueden usar en las aplicaciones existentes basadas en esta plataforma. El hecho de tener habilitado Kafka en el clúster no afectará a otros casos de uso que no estén relacionados con Kafka, por lo que no existe la opción ni la necesidad de deshabilitar Kafka en un clúster.

## <a name="why-dedicated"></a>¿Por qué el plan de tarifa Dedicado?

Event Hubs dedicado brinda tres atractivas ventajas para los clientes que necesitan capacidad de nivel empresarial:

#### <a name="single-tenancy-guarantees-capacity-for-better-performance"></a>Al haber un único inquilino, se asegura de que hay capacidad suficiente para mejorar el rendimiento.

Los clústeres dedicados garantizan la capacidad con el máximo nivel de escalado y pueden admitir hasta gigabytes de datos de streaming de entrada con un almacenamiento de máxima duración y una latencia inferior a un segundo para acomodar ráfagas de tráfico de todo tipo. 

#### <a name="inclusive-and-exclusive-access-to-features"></a>Acceso incluido y exclusivo a características 
La oferta Dedicado incluye algunas características como Capture sin ningún costo adicional y proporciona acceso exclusivo a otras características que se publicarán próximamente, como Bring Your Own Key (BYOK). El servicio también administra el equilibrio de carga, las actualizaciones del sistema operativo, las revisiones de seguridad y la creación de particiones para el cliente, por lo que puede dedicar menos tiempo al mantenimiento de la infraestructura y más tiempo a la creación de características en el lado cliente.  

#### <a name="cost-savings"></a>Ahorro de costos
Cuando los volúmenes de entrada son altos (> 100 TU), el costo por hora de un clúster es significativamente inferior a la cantidad equivalente de unidades de procesamiento de la oferta Estándar.


## <a name="event-hubs-dedicated-quotas-and-limits"></a>Cuotas y límites de Event Hubs dedicado

La oferta de Event Hubs dedicado se factura aplicando una tarifa mensual fija con un uso mínimo de 4 horas. El nivel Dedicado ofrece todas las características del plan Estándar, pero con una capacidad de escalado de nivel empresarial para aquellos clientes que tienen cargas de trabajo muy exigentes. 

| Característica | Estándar | Dedicado |
| --- |:---:|:---:|
| Ancho de banda | 20 TU (hasta 40 TU) | 20 CU |
| Espacios de nombres |  1 | 50 por CU |
| Event Hubs |  10 por espacio de nombres | 1000 por espacio de nombres |
| Eventos de entrada | Pago por millones de eventos | Se incluye |
| Tamaño de los mensajes | 1 millón de bytes | 1 millón de bytes |
| Particiones | 32 por centro de eventos | 1024 por centro de eventos |
| Grupos de consumidores | 20 por centro de eventos | Sin límite por CU, 1000 por centro de eventos |
| Conexiones asincrónicas | 1000 incluidas, 5000 como máximo | 100 000 incluidos y como máximo |
| Retención de mensajes | 7 días, 84 GB incluidas por TU | 90 días, 10 TB incluidas por TU |
| Capturar | Pago por hora | Se incluye |

## <a name="how-to-onboard"></a>¿Cómo incorporarlo?

La experiencia de autoservicio para [crear un clúster de Event Hubs](event-hubs-dedicated-cluster-create-portal.md) en [Azure Portal](https://aka.ms/eventhubsclusterquickstart) está ahora en versión preliminar. Si tiene alguna pregunta o necesita ayuda para empezar a utilizar Event Hubs dedicado, póngase en contacto con el [equipo de Event Hubs](mailto:askeventhubs@microsoft.com).

## <a name="faqs"></a>Preguntas más frecuentes

#### <a name="what-can-i-achieve-with-a-cluster"></a>¿Qué puedo conseguir con un clúster?

En un clúster de Event Hubs, la cantidad de datos que puede ingerir y transmitir depende de varios factores, como los productores, los consumidores, la velocidad a la que se ingiere y procesa, etc. 

En la tabla siguiente se muestran los resultados del banco de pruebas que hemos obtenido durante nuestras pruebas:

| Forma de la carga | Receptores | Ancho de banda de entrada| Mensajes de entrada | Ancho de banda de salida | Mensajes de salida | TU totales | TU por CU |
| ------------- | --------- | ---------------- | ------------------ | ----------------- | ------------------- | --------- | ---------- |
| Lotes de 100 x 1 KB | 2 | 400 MB/s | 400 000 mensajes/s | 800 MB/s | 800 000 mensajes/s | 400 TU | 100 TU | 
| Lotes de 10 x 10 KB | 2 | 666 MB/s | 66 600 mensajes/s | 1,33 GB/s | 133 000 mensajes/s | 666 TU | 166 TU |
| Lotes de 6 x 32 KB | 1 | 1,05 GB/s | 34 000 mensajes/s | 1,05 GB/s | 34 000 mensajes/s | 1000 TU | 250 TU |

En las pruebas, se usaron los siguientes criterios:

- Se usó un clúster de Event Hubs dedicado con cuatro unidades de capacidad (CU). 
- El centro de eventos usado para la ingesta tenía 200 particiones. 
- Dos aplicaciones receptoras recibieron los datos ingeridos de todas las particiones.

#### <a name="can-i-scale-updown-my-cluster"></a>¿Se puede escalar o reducir verticalmente el clúster?

Una vez creados los clústeres, se factura un mínimo de 4 horas de uso. En la versión preliminar de la experiencia de autoservicio, puede enviar una [solicitud de soporte técnico](https://ms.portal.azure.com/#create/Microsoft.Support) al equipo de Event Hubs en *Técnico > Cuota > Request to Scale Up or Scale Down Dedicated Cluster (Solicitud para escalar o reducir verticalmente el clúster dedicado)* para escalar o reducir verticalmente el clúster. La solicitud para reducir verticalmente el clúster puede tardar hasta 7 días en completarse. 

#### <a name="how-will-geo-dr-work-with-my-cluster"></a>¿Cómo va a funcionar la recuperación ante desastres con localización geográfica en mi clúster?

Puede emparejar geográficamente un espacio de nombres de un clúster del nivel Dedicado con otro espacio de nombres de un clúster del mismo nivel. No es recomendable emparejar un espacio de nombres del nivel Dedicado con otro de la oferta Estándar, ya que el límite de rendimiento sería incompatible y se producirían errores. 

#### <a name="can-i-migrate-my-standard-namespaces-to-belong-to-a-dedicated-tier-cluster"></a>¿Puedo migrar espacios de nombres del nivel Estándar para que pertenezcan a un clúster del nivel Dedicado?
Actualmente, no se admite ningún proceso automático para migrar los datos de los centros de eventos de un espacio de nombres Estándar a otro Dedicado. 

## <a name="next-steps"></a>Pasos siguientes

Póngase en contacto con un representante de ventas o con el Soporte técnico de Microsoft para obtener detalles adicionales sobre Event Hubs dedicado. También puede crear un clúster u obtener más información sobre los planes de tarifa de Event Hubs en los vínculos siguientes:

- [Creación de un clúster de Event Hubs con Azure Portal](https://aka.ms/eventhubsclusterquickstart) 
- [Precios de Event Hubs dedicado](https://azure.microsoft.com/pricing/details/event-hubs/). También puede ponerse en contacto con su representante de ventas de Microsoft o el Servicio de soporte técnico de Microsoft para obtener detalles adicionales sobre la capacidad dedicada de Event Hubs.
- En [Preguntas más frecuentes sobre Event Hubs](event-hubs-faq.md) se proporciona información sobre los precios, así como respuestas a algunas preguntas frecuentes acerca de Event Hubs.
