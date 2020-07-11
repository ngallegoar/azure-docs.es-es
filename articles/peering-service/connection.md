---
title: Conexión de Azure Peering Service
description: Obtenga información sobre la conexión de Microsoft Azure Peering Service
services: peering-service
author: derekolo
ms.service: peering-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: Infrastructure-services
ms.date: 05/18/2020
ms.author: derekol
ms.openlocfilehash: df337736a676c40f1345310afb9a2e685ddc1d5c
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "84871325"
---
# <a name="peering-service-connection"></a>Conexión de Peering Service

Una conexión normalmente hace referencia a un conjunto de información lógico que identifica a una instancia de Peering Service. Se define al especificar los atributos siguientes:

- Nombre lógico
- Asociado de conectividad
- Ubicación física del cliente
- Prefijos IP

El cliente puede establecer una sola conexión o varias conexiones según los requisitos. Una conexión también se usa como una unidad de recopilación de telemetría. Por ejemplo, para recibir alertas de telemetría, el cliente debe definir la conexión que se va a supervisar.

> [!Note]
> Al suscribirse a Peering Service, se analiza la telemetría de Windows y Office 365 con el fin de proporcionar medidas de latencia para los prefijos seleccionados.
>Para obtener más información sobre la telemetría de conexión, vea [Telemetría de conexión de Peering Service](connection-telemetry.md).
>

>## <a name="how-to-register-a-connection"></a>Registro de una conexión

**Escenario**: imagine una sucursal distribuida entre diferentes ubicaciones geográficas, como se muestra en la ilustración siguiente. Aquí, el cliente debe proporcionar un nombre lógico, el nombre del proveedor de servicios (SP), la ubicación física del cliente y los prefijos IP que haya (propiedad del cliente o asignados por el proveedor de servicios) asociados a una única conexión. Este proceso debe repetirse para registrar Peering Service para conexiones con redundancia geográfica independientes.

![Conexiones con redundancia geográfica](./media/peering-service-connection/peering-service-connections.png)

> [!Note]
> El filtrado de nivel de estado se tiene en cuenta para la ubicación física del cliente cuando la conexión se encuentra geográficamente en los Estados Unidos.
>

## <a name="next-steps"></a>Pasos siguientes

Para conocer el procedimiento paso a paso para registrar la conexión de Peering Service, vea [Registro de Peering Service mediante Azure Portal](azure-portal.md).

Para más información sobre la telemetría de la conexión de Peering Service, consulte [Telemetría de la conexión de Peering Service](connection-telemetry.md).

Para medir la telemetría, consulte [Medición de la telemetría de la conexión](measure-connection-telemetry.md).
