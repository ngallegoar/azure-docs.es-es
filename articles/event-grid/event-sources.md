---
title: Orígenes de eventos de Azure Event Grid
description: Suscripciones de Azure, Container Registry, temas personalizados, Event Hubs, IoT Hub, Key Vault, Media Services, grupos de recursos, Service Bus, Storage, Mapa, App Configuration, Signal R, Machine Learning.
services: event-grid
author: spelluru
ms.service: event-grid
ms.topic: conceptual
ms.date: 02/12/2019
ms.author: spelluru
ms.openlocfilehash: d4a426ea1432d0266b7ae9344afefe8ddac1d030
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79231324"
---
# <a name="event-sources-in-azure-event-grid"></a>Orígenes de eventos de Azure Event Grid

Un origen de evento es donde se produce el evento. Varios servicios de Azure se configuran automáticamente para enviar eventos. También puede crear aplicaciones personalizadas que envían eventos. Las aplicaciones personalizadas no necesitan estar hospedadas en Azure para usar Event Grid para la distribución de eventos.

Este artículo contiene vínculos a contenido para cada origen de eventos.

## <a name="azure-subscriptions"></a>Suscripciones de Azure

Suscríbase a eventos de suscripciones de Azure para responder a los cambios en los recursos de una suscripción de Azure.

|Título |Descripción  |
|---------|---------|
| [Tutorial: Integración de Azure Automation con Event Grid y Microsoft Teams](ensure-tags-exists-on-new-virtual-machines.md) |Cree una máquina virtual, que envía un evento. El evento desencadena un runbook de Automation que etiqueta la máquina virtual y genera un mensaje que se envía a un canal de Equipos de Microsoft. |
| [Suscripción a eventos mediante el portal](subscribe-through-portal.md) | Uso del portal para la suscripción a eventos desde una suscripción de Azure. |
| [CLI de Azure: suscripción a eventos desde una suscripción de Azure](./scripts/event-grid-cli-azure-subscription.md) |Script de ejemplo que crea una suscripción de Event Grid para una suscripción de Azure y envía eventos a un webhook. |
| [PowerShell: suscripción a eventos desde una suscripción de Azure](./scripts/event-grid-powershell-azure-subscription.md)| Script de ejemplo que crea una suscripción de Event Grid para una suscripción de Azure y envía eventos a un webhook. |
| [Esquema del evento](event-schema-subscriptions.md) | Muestra los campos en los eventos de suscripción de Azure. |

## <a name="container-registry"></a>Container Registry

Suscríbase a los eventos de Container Registry para responder a los cambios en las imágenes.

|Título |Descripción  |
|---------|---------|
| [Guía de inicio rápido: Envío de eventos de Container Registry](../container-registry/container-registry-event-grid-quickstart.md?toc=%2fazure%2fevent-grid%2ftoc.json) | Explica cómo utilizar la CLI de Azure para enviar eventos de Container Registry. |
| [Esquema del evento](event-schema-container-registry.md) | Describe los campos de los eventos de Container Registry. |

## <a name="custom-topics"></a>Temas personalizados

Suscríbase a temas personalizados para responder a eventos de la aplicación.

|Título  |Descripción  |
|---------|---------|
| [Guía de inicio rápido: creación y enrutamiento de eventos personalizados con la CLI de Azure](custom-event-quickstart.md) | Muestra cómo utilizar la CLI de Azure para enviar eventos personalizados. |
| [Guía de inicio rápido: creación y enrutamiento de eventos personalizados con Azure PowerShell](custom-event-quickstart-powershell.md) | Muestra cómo utilizar Azure PowerShell para enviar eventos personalizados. |
| [Guía de inicio rápido: creación y enrutamiento de eventos personalizados con Azure Portal](custom-event-quickstart-portal.md) | Muestra cómo utilizar Azure Portal para enviar eventos personalizados. |
| [Guía de inicio rápido: enrutamiento de eventos personalizados a Azure Queue Storage](custom-event-to-queue-storage.md) | Describe cómo enviar eventos personalizados a una instancia de Queue Storage. |
| [Publicación en tema personalizado](post-to-custom-topic.md) | Se describe cómo publicar un evento en un tema personalizado. |
| [CLI de Azure: creación de un tema personalizado de Event Grid](./scripts/event-grid-cli-create-custom-topic.md)|Script de ejemplo que crea un tema personalizado. El script recupera el punto de conexión y una clave.|
| [CLI de Azure: suscripción a eventos de un tema personalizado](./scripts/event-grid-cli-subscribe-custom-topic.md)|Script de ejemplo que crea una suscripción a un tema personalizado. Envía eventos a un webhook.|
| [PowerShell: Creación de un tema personalizado de Event Grid](./scripts/event-grid-powershell-create-custom-topic.md)|Script de ejemplo que crea un tema personalizado. El script recupera el punto de conexión y una clave.|
| [PowerShell: suscripción a eventos de un tema personalizado](./scripts/event-grid-powershell-subscribe-custom-topic.md)|Script de ejemplo que crea una suscripción a un tema personalizado. Envía eventos a un webhook.|
| [Plantilla de Resource Manager: tema personalizado y punto de conexión del webhook](https://github.com/Azure/azure-quickstart-templates/tree/master/101-event-grid) | Plantilla de Resource Manager que crea un tema personalizado y una suscripción a ese tema personalizado. Envía eventos a un webhook. |
|
| [Plantilla de Resource Manager: tema personalizado y punto de conexión a Event Hubs](https://github.com/Azure/azure-quickstart-templates/tree/master/101-event-grid-event-hubs-handler)| Plantilla de Resource Manager que crea una suscripción a un tema personalizado. Envía eventos a una instancia de Azure Event Hubs. |
| [Esquema del evento](event-schema.md) | Muestra los campos de eventos personalizados. |

## <a name="event-hubs"></a>Event Hubs

Suscríbase a eventos de Event Hubs para responder a eventos de archivos de captura. Event Hubs puede actuar como origen de eventos o como controlador de eventos. En los siguientes artículos se explica cómo utilizar Event Hubs como origen.

|Título  |Descripción  |
|---------|---------|
| [Tutorial: transmisión de macrodatos a un almacén de datos](event-grid-event-hubs-integration.md) | Cuando Event Hubs crea un archivo de captura, Event Grid envía un evento a una aplicación de función. La aplicación recupera el archivo de captura y migra los datos a un almacenamiento de datos. |
| [Esquema del evento](event-schema-event-hubs.md) | Muestra los campos de los eventos de Event Hubs. |

Para ejemplos de Event Hubs como controlador, consulte este artículo sobre el [controlador de Event Hubs](event-handlers.md#event-hubs).

## <a name="iot-hub"></a>IoT Hub

Suscríbase a eventos de IoT Hub para responder a eventos de telemetría y de dispositivo creado, eliminado, conectado y desconectado.

|Título  |Descripción  |
|---------|---------|
| [Envío de notificaciones por correo electrónico sobre eventos de Azure IoT Hub mediante Logic Apps](publish-iot-hub-events-to-logic-apps.md) | Una aplicación lógica envía un correo electrónico de notificación cada vez que se agrega un dispositivo a IoT Hub. |
| [Reacción a eventos de IoT Hub usando Event Grid para desencadenar acciones](../iot-hub/iot-hub-event-grid.md) | Información general sobre la integración de IoT Hub con Event Grid. |
| [Esquema del evento](event-schema-iot-hub.md) | Muestra los campos de los eventos de IoT Hub. |
| [Ordenación de los eventos de dispositivo conectado y dispositivo desconectado](../iot-hub/iot-hub-how-to-order-connection-state-events.md) | Muestra cómo ordenar los eventos de estado de conexión del dispositivo. |

## <a name="key-vault-preview"></a>Key Vault (versión preliminar)

La integración de Key Vault con Event Grid se encuentra actualmente en versión preliminar. 

Suscríbase a los eventos de Key Vault para recibir notificaciones cuando un secreto está a punto de expirar, expira o tiene una nueva versión disponible. 

|Título  |Descripción  |
|---------|---------|
| [Supervisión de eventos de Key Vault con Azure Event Grid](../key-vault/event-grid-overview.md) | Información general sobre la integración de Key Vault con Event Grid. |
| [Tutorial: Creación y supervisión de eventos de Key Vault con Event Grid](../key-vault/event-grid-tutorial.md) | Obtenga información sobre cómo configurar notificaciones de Event Grid para Key Vault. |
| [Esquema del evento](event-schema-key-vault.md) | Muestra los campos de los eventos de Key Vault. |

## <a name="media-services"></a>Media Services

Suscríbase a eventos de Media Services para responder a eventos de estado de trabajo.

|Título  |Descripción  |
|---------|---------|
| [Introducción: reacción ante eventos de Media Services](../media-services/latest/reacting-to-media-services-events.md) | Información general de la integración de Media Services con Event Grid. |
| [Tutorial: enrutamiento de eventos de Azure Media Services a un punto de conexión web personalizado mediante la CLI](../media-services/latest/job-state-events-cli-how-to.md?toc=%2fazure%2fevent-grid%2ftoc.json) | Muestra cómo enviar eventos desde Media Services. |
| [Esquema del evento](../media-services/latest/media-services-event-schemas.md?toc=%2fazure%2fevent-grid%2ftoc.json) | Muestra los campos de los eventos de Media Services. |

## <a name="resource-groups"></a>Grupos de recursos

Suscríbase a eventos de grupos de recursos para responder a los cambios de los recursos de un grupo de recursos.

|Título  |Descripción  |
|---------|---------|
| [Tutorial: supervisión de los cambios en máquinas virtuales con Azure Event Grid y Logic Apps](monitor-virtual-machine-changes-event-grid-logic-app.md) | Una aplicación lógica supervisa los cambios realizados en una máquina virtual y envía mensajes de correo electrónico sobre dichos cambios. |
| [CLI de Azure: suscripción a eventos para un grupo de recursos](./scripts/event-grid-cli-resource-group.md)| Script de ejemplo que se suscribe a eventos para un grupo de recursos. Envía eventos a un webhook. |
| [CLI de Azure: suscripción a eventos de un grupo de recursos y filtrado de los eventos de un recurso](./scripts/event-grid-cli-resource-group-filter.md) | Script de ejemplo que se suscribe a eventos para un grupo de recursos y filtra los eventos para un recurso. |
| [PowerShell: suscripción a eventos para un grupo de recursos](./scripts/event-grid-powershell-resource-group.md) | Script de ejemplo que se suscribe a eventos para un grupo de recursos. Envía eventos a un webhook. |
| [PowerShell: suscripción a eventos para un grupo de recursos y filtrado de los eventos para un recurso](./scripts/event-grid-powershell-resource-group-filter.md) | Script de ejemplo que se suscribe a eventos para un grupo de recursos y filtra los eventos para un recurso. |
| [Plantilla de Resource Manager: suscripción de recursos](https://github.com/Azure/azure-quickstart-templates/tree/master/101-event-grid-resource-events-to-webhook) | Se suscribe a eventos para una suscripción o grupo de recursos de Azure. Envía eventos a un webhook. |
| [Esquema del evento](event-schema-resource-groups.md) | Muestra los campos de eventos de grupos de recursos. |

## <a name="service-bus"></a>Azure Service Bus

Suscríbase a eventos de Service Bus para responder a mensajes sin un agente de escucha activo.

|Título  |Descripción  |
|---------|---------|
| [Tutorial: Ejemplos de integración de Azure Service Bus en Azure Event Grid](../service-bus-messaging/service-bus-to-event-grid-integration-example.md?toc=%2fazure%2fevent-grid%2ftoc.json) | Event Grid envía mensajes de temas de Service Bus a la aplicación de función y a la aplicación lógica. |
| [Información general: Integración de Azure Service Bus en Event Grid](../service-bus-messaging/service-bus-to-event-grid-integration-concept.md) | Información general de la integración de Service Bus con Event Grid. |
| [Esquema del evento](event-schema-service-bus.md) | Muestra los campos de los eventos de Service Bus. |

## <a name="storage"></a>Storage

Suscríbase a eventos de Blob Storage para responder a eventos creados y eliminados del blob.

>[!NOTE]
> Solo las cuentas de almacenamiento de tipo **StorageV2 (uso general v2)** y **BlobStorage** admiten la integración de eventos. **Storage (uso general v1)** *no* admite la integración con Event Grid.

|Título  |Descripción  |
|---------|---------|
| [Guía de inicio rápido: enrutamiento de eventos de Blob Storage a un punto de conexión web personalizado con la CLI de Azure](../storage/blobs/storage-blob-event-quickstart.md?toc=%2fazure%2fevent-grid%2ftoc.json) | Muestra cómo utilizar la CLI de Azure para enviar eventos de Blob Storage a un webhook. |
| [Guía de inicio rápido: enrutamiento de eventos de Blob Storage a un punto de conexión web personalizado con PowerShell](../storage/blobs/storage-blob-event-quickstart-powershell.md?toc=%2fazure%2fevent-grid%2ftoc.json) | Muestra cómo utilizar Azure PowerShell para enviar eventos de Blob Storage a un webhook. |
| [Guía de inicio rápido: creación y enrutamiento de eventos de Blob Storage con Azure Portal](blob-event-quickstart-portal.md) | Muestra cómo utilizar el portal para enviar eventos de Blob Storage a un webhook. |
| [CLI de Azure: suscripción a eventos de una cuenta de Blob Storage](./scripts/event-grid-cli-blob.md) | Script de ejemplo que se suscribe a un evento para una cuenta de Blob Storage. Envía el evento a un webhook. |
| [PowerShell: suscripción a eventos de una cuenta de Blob Storage](./scripts/event-grid-powershell-blob.md) | Script de ejemplo que se suscribe a un evento para una cuenta de Blob Storage. Envía el evento a un webhook. |
| [Plantilla de Resource Manager: Creación de almacenamiento y suscripción de blobs](https://github.com/Azure/azure-quickstart-templates/tree/master/101-event-grid-subscription-and-storage) | Implementa una cuenta de Azure Blob Storage y crea una suscripción a eventos de dicha cuenta. Envía eventos a un webhook. |
| [Introducción: reacción ante eventos de Blob Storage](../storage/blobs/storage-blob-event-overview.md) | Información general de la integración de Blob Storage con Event Grid. |
| [Esquema del evento](event-schema-blob-storage.md) | Muestra los campos de los eventos de Blob Storage. |

## <a name="maps"></a>Mapas
Suscribirse a eventos de Azure Maps para responder a eventos de la geovalla. Por ejemplo, una aplicación puede entregar una notificación por correo electrónico cada vez que un dispositivo entra o sale de una geovalla.

|Título  |Descripción  |
|---------|---------|
| [Reaccionar a eventos de Azure Maps mediante Event Grid](../azure-maps/azure-maps-event-grid-integration.md?toc=%2fazure%2fevent-grid%2ftoc.json) | Información general de la integración de Azure Maps con Event Grid. |
| [Tutorial: configuración de una geovalla](../azure-maps/tutorial-geofence.md?toc=%2fazure%2fevent-grid%2ftoc.json) | Este tutorial le guiará por los pasos básicos para configurar la geovalla con Azure Maps. Usará Azure Event Grid para transmitir los resultados de la geovalla y establecer una notificación basada en los resultados de esa geovalla. |
| [Esquema del evento](event-schema-azure-maps.md) | Muestra los campos en los eventos de Azure Maps. |

## <a name="app-configuration"></a>Configuración de la aplicación
Suscríbase a eventos Azure App Configuration para responder a los eventos de modificación de pares clave-valor.

|Título | Descripción |
|---------|---------|
| [Reacción a eventos de Azure App Configuration con Event Grid](../azure-app-configuration/concept-app-configuration-event.md?toc=%2fazure%2fevent-grid%2ftoc.json) | Información general de la integración de Azure App Configuration con Event Grid. |
| [Inicio rápido: Enrutamiento de eventos de Azure App Configuration a un punto de conexión web personalizado con la CLI de Azure](../azure-app-configuration/howto-app-configuration-event.md?toc=%2fazure%2fevent-grid%2ftoc.json) | Muestra cómo utilizar la CLI de Azure para enviar eventos de Azure App Configuration a un webhook. |
| [Esquema del evento](event-schema-app-configuration.md) | Muestra los campos de eventos de Azure App Configuration. |

## <a name="azure-signalr"></a>Azure SignalR
Suscríbase a eventos de Azure SignalR Service para responder a eventos de conexión de cliente.

|Título | Descripción |
|---------|---------|
| [Reaccionar a eventos de Azure SignalR Service mediante Event Grid](../azure-signalr/signalr-concept-event-grid-integration.md) | Información general sobre la integración de Azure SignalR Service con Event Grid. |
| [Cómo enviar eventos de Azure SignalR Service a Event Grid](../azure-signalr/signalr-howto-event-grid-integration.md) | Muestra cómo enviar eventos de Azure SignalR Service a una aplicación a través de Event Grid. |
| [Esquema del evento](event-schema-azure-signalr.md) | Muestra campos de eventos de Azure SignalR Service. |

## <a name="azure-machine-learning"></a>Azure Machine Learning

Suscríbase a eventos de área de trabajo de Azure Machine Learning para responder al registro del modelo.

| Título | Descripción |
| ----- | ----- |
| [Consumo de eventos de Azure Machine Learning](../machine-learning/concept-event-grid-integration.md) | Información general de la integración de Azure Machine Learning con Event Grid. |
| [Esquema de eventos de Azure Event Grid para Azure Machine Learning](event-schema-machine-learning.md) | Muestra los campos de los eventos de Azure Machine Learning. |

## <a name="next-steps"></a>Pasos siguientes

* Para obtener una introducción a Event Grid, vea [Acerca de Event Grid](overview.md).
* Para comenzar a usar rápidamente Event Grid, vea [Creación y enrutamiento de eventos personalizados con Azure Event Grid](custom-event-quickstart.md).
