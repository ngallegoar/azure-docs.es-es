---
title: Temas del sistema en Azure Event Grid
description: Describe los temas del sistema en Azure Event Grid.
services: event-grid
author: spelluru
ms.service: event-grid
ms.topic: conceptual
ms.date: 06/02/2020
ms.author: spelluru
ms.openlocfilehash: 67746ebd8a16eb02b8f02d238b0e3c0125989189
ms.sourcegitcommit: 69156ae3c1e22cc570dda7f7234145c8226cc162
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/03/2020
ms.locfileid: "84308275"
---
# <a name="system-topics-in-azure-event-grid"></a>Temas del sistema en Azure Event Grid
El servicio Azure Event Grid crea temas del sistema cuando se crea una primera suscripción de evento para un origen del evento de Azure. Actualmente, Event Grid no crea temas del sistema para los orígenes de tema que se crearon antes del 15 de marzo de 2020. En el caso de todos los orígenes de tema que se hayan creado en esta fecha o con posterioridad a la misma, Event Grid crea automáticamente temas del sistema. En este artículo se describen los **temas del sistema** en Azure Event Grid.

> [!NOTE]
> Esta característica no está habilitada actualmente para la nube de Azure Government. 

## <a name="overview"></a>Información general
Al crear una primera suscripción de evento para un origen del evento de Azure, como una cuenta de Azure Storage, el proceso de aprovisionamiento de la suscripción crea un recurso adicional de tipo **Microsoft. EventGrid/systemTopics**. Al eliminar la última suscripción de evento al origen del evento de Azure, el tema del sistema se elimina automáticamente.

El tema del sistema no es aplicable a los escenarios de temas personalizados, es decir, los temas de Event Grid y los dominios de Event Grid. 

## <a name="name"></a>Nombre 
Anteriormente, cuando creó una suscripción para un evento generado por orígenes de Azure, el servicio Event Grid creó automáticamente un tema del sistema con un **nombre generado de forma aleatoria**. Ahora, puede especificar un nombre para el tema del sistema al crear el tema en Azure Portal. Este recurso de tema del sistema se puede usar para detectar métricas y registros de diagnóstico.

## <a name="location"></a>Location
En el caso de los orígenes del evento de Azure que se encuentran en una región o ubicación específica, el tema del sistema se crea en la misma ubicación que el origen del evento de Azure. Por ejemplo, si crea una suscripción de evento para un almacenamiento de blobs de Azure en la región Este de EE. UU., el tema del sistema se crea en Este de EE. UU. En el caso de los orígenes del evento globales de Azure, como las suscripciones y grupos de recursos de Azure o Azure Maps, Event Grid crea el tema del sistema en una ubicación **global**. 

## <a name="resource-group"></a>Resource group 
En general, el tema del sistema se crea en el mismo grupo de recursos en el que se encuentra el origen del evento de Azure. En el caso de las suscripciones de evento creadas en el ámbito de la suscripción de Azure, el tema del sistema se crea en el grupo de recursos **Default-EventGrid**. Si el grupo de recursos no existe, Azure Event Grid lo crea antes de crear el tema del sistema. 

Si intentar eliminar el grupo de recursos con la cuenta de almacenamiento, verá el tema del sistema en la lista de recursos afectados.  

![Eliminación de un grupo de recursos](./media/system-topics/delete-resource-group.png)


## <a name="next-steps"></a>Pasos siguientes
Consulte el artículo siguiente: [Creación, visualización y administración de temas del sistema](create-view-manage-system-topics.md).