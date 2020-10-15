---
title: Telemetría de conexión de Azure Peering Service
description: Obtenga información sobre la telemetría de conexión de Microsoft Azure Peering Service
services: peering-service
author: derekolo
ms.service: peering-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 05/18/2020
ms.author: derekol
ms.openlocfilehash: 0e54b20cc6aefa69226c24f557ae02166dc4b4b6
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "84871305"
---
# <a name="peering-service-connection-telemetry"></a>Telemetría de conexión de Peering Service

La telemetría de conexión proporciona información recopilada sobre la conectividad entre la ubicación del cliente y la red de Microsoft. Los clientes pueden obtener la telemetría de la conexión de Azure Peering Service si registran la conexión en Azure Portal. Esta característica proporciona seguridad de prefijo e información sobre el rendimiento de la red.


La telemetría de conexión se compone de los siguientes ámbitos:

### <a name="latency-measurement"></a>Medida de latencia

 La latencia se mide desde el cliente hasta el PoP de Microsoft Edge de los prefijos registrados en Peering Service.

### <a name="route-prefix-monitoring-and-protection"></a>Supervisión y protección de prefijos de ruta

Las rutas de acceso de enrutamiento se supervisan para detectar cualquier actividad sospechosa que luego se captura en registros de eventos. Por ejemplo, los registros de eventos se crean en caso de alguno de estos factores:

- Secuestros de prefijo
- Retirada de prefijo
- Pérdidas de rutas

## <a name="next-steps"></a>Pasos siguientes

- Para más información sobre la conexión de Peering Service, consulte [Conexión de Peering Service](connection.md).
- Para incorporar una conexión de Peering Service, vea [Incorporación de un modelo de Peering Service](onboarding-model.md).
- Para medir la telemetría, consulte [Medición de la telemetría de la conexión](measure-connection-telemetry.md).
