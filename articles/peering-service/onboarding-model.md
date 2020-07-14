---
title: Modelo de incorporación de Azure Peering Service
description: Aprenda a incorporar Azure Peering Service.
services: peering-service
documentationcenter: na
author: derekolo
ms.service: peering-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: Infrastructure-services
ms.date: 05/18/2020
ms.author: derekol
ms.openlocfilehash: 7a257b4c2bea3bd3384a55c0a6b85d7fdd2ca583
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "84871405"
---
# <a name="onboarding-peering-service-model"></a>Modelo de incorporación de Peering Service

El proceso de incorporación de Peering Service consta de dos modelos (se indican a continuación):

 - Incorporación de una conexión de Peering Service

 - Incorporación de la telemetría de la conexión de Peering Service

A continuación, se describen los planes de acción de los modelos enumerados anteriormente:

| **Step** | **Acción**| **Resultado**|
|-----------|---------|---------|---------|
|1|Cliente para aprovisionar la conectividad de un asociado de conectividad (sin interacción con Microsoft). |Proveedor de Internet que está bien conectado a Microsoft y que satisface los requisitos técnicos de una conectividad buena y confiable con Microsoft.  |
|2 (opcional)|El cliente registra las ubicaciones en Azure Portal. Una ubicación se define mediante: El nombre de ISP/IXP, la ubicación física del sitio de cliente (nivel de estado) y el prefijo IP asignado a la ubicación por el proveedor de servicios o la empresa.  |Telemetría: supervisión de las rutas de Internet, clasificación por orden de prioridad del tráfico desde Microsoft hasta la ubicación POP perimetral más cercana del usuario. |



## <a name="onboarding-peering-service-connection"></a>Incorporación de una conexión de Peering Service

Para incorporar una conexión de Peering Service, haga lo siguiente:

Trabaje con un proveedor de servicios Internet (ISP) o con un asociado de intercambio de Internet (IX) para obtener Peering Service y conectar la red a la red de Microsoft.

Asegúrese de que los [proveedores de conectividad](location-partners.md) de Peering Service estén asociados con Microsoft. 

## <a name="onboarding-peering-service-connection-telemetry"></a>Incorporación de la telemetría de la conexión de Peering Service

Los clientes pueden elegir su telemetría, como el análisis de las rutas BGP, para supervisar la latencia y el rendimiento de la red al acceder a la red de Microsoft. Para ello se puede registrar la conexión en Azure Portal.

Para incorporar la telemetría de la conexión de Peering Service, el cliente debe registrarla en Azure Portal. Consulte [Registro de Peering Service mediante Azure Portal](azure-portal.md) para conocer el procedimiento.

Después, consulte [aquí](measure-connection-telemetry.md) para medir la telemetría.

## <a name="next-steps"></a>Pasos siguientes

Para conocer el procedimiento paso a paso de cómo registrar la conexión de Peering Service, consulte [Registro de Peering Service mediante Azure Portal](azure-portal.md).

Para más información sobre cómo medir la telemetría de la conexión, consulte [Medición de la telemetría de la conexión](measure-connection-telemetry.md).
