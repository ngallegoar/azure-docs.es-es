---
author: robinsh
manager: philmea
ms.author: robinsh
ms.topic: include
ms.date: 05/20/2019
ms.openlocfilehash: c164433efc6a34a3a06676a3145feb18d3de80b9
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "66249062"
---
## <a name="add-a-consumer-group-to-your-iot-hub"></a>Adición de un grupo de consumidores a IoT Hub

Los [grupos de consumidores](https://docs.microsoft.com/azure/event-hubs/event-hubs-features#event-consumers) proporcionan vistas independientes en la secuencia de eventos que permiten a las aplicaciones y a los servicios de Azure consumir datos de forma independiente desde el mismo punto de conexión del centro de eventos. En esta sección, agregará un grupo de consumidores al punto de conexión integrado de su instancia de IoT Hub que se usará posteriormente en este tutorial para extraer datos del punto de conexión.

Para agregar un grupo de consumidores a su centro de IoT, siga estos pasos:

1. En [Azure Portal](https://portal.azure.com/), abra su centro de IoT.

2. En el panel izquierdo, seleccione **Puntos de conexión integrados**, luego **Eventos** en el panel superior derecho y escriba un nombre en **Grupos de consumidores**. Seleccione **Guardar**.

   ![Creación de un grupo de consumidores para el IoT Hub](./media/iot-hub-get-started-create-consumer-group/iot-hub-create-consumer-group-azure.png)
