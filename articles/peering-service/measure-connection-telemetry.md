---
title: 'Azure Peering Service: Medición de la telemetría de la conexión '
description: En este tutorial aprenderá a medir la telemetría de la conexión.
services: peering-service
author: derekolo
ms.service: peering-service
ms.topic: tutorial
ms.date: 05/18/2020
ms.author: derekol
Customer intent: Customer wants to measure their connection telemetry per prefix to Microsoft services with Azure Peering Service.
ms.openlocfilehash: abbe69ebbaed56ed416f85fafa7b77a1740fabe7
ms.sourcegitcommit: a43a59e44c14d349d597c3d2fd2bc779989c71d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/25/2020
ms.locfileid: "96022503"
---
# <a name="tutorial-measure-peering-service-connection-telemetry"></a>Tutorial: Medición de la telemetría de conexión de Peering Service

 En este tutorial, aprenderá a medir la telemetría de las conexiones de Peering Service.
 
 La telemetría de conexión proporciona información recopilada sobre la conectividad entre la ubicación del cliente y la red de Microsoft. En este artículo, aprenderá a ver el informe de latencia de una conexión de Azure Peering Service específica. 

Para medir la telemetría de la conexión de Peering Service, debe registrar una conexión de Peering Service en Azure Portal. Para obtener información sobre cómo registrar una conexión, consulte [Registro de una conexión de Peering Service: Azure Portal](azure-portal.md).


## <a name="view-a-latency-report"></a>Visualización de un informe de latencia

Para ver un informe de latencia para una conexión de Peering Service específica, siga estos pasos.

1. Seleccione **Todos los recursos** en el panel izquierdo y seleccione la conexión de Peering Service. A continuación, seleccione **Open** (Abrir) en **Prefixes** (Prefijos). 

   ![Selección de la conexión de Peering Service](./media/peering-service-measure/peering-service-measure-menu.png)

2. Aparece una página del informe de latencia de todos los prefijos asociados a esa conexión de Peering Service. 

      ![Página del informe de latencia](./media/peering-service-measure/peering-service-latency-report.png)

3. De forma predeterminada, el informe se actualiza cada hora que se muestra en esta página. Para ver el informe para diferentes escalas de tiempo, elija la opción adecuada en **Show data for last** (Mostrar datos del último período de). 

4. Para ver los eventos de un prefijo específico, seleccione el nombre del prefijo y seleccione **Prefix Events** (Eventos de prefijo) en el panel izquierdo. Se muestran los eventos capturados.


   ![Eventos de prefijo](./media/peering-service-measure/peering-service-prefix-event.png)

 Aquí se muestran algunos de los posibles eventos que se capturan en la lista **Eventos de prefijo**.

| **Eventos de prefijo** | **Tipo de evento**|**Razonamiento**|
|-----------|---------|---------|
| PrefixAnnouncementEvent |Información|Se recibió el anuncio del prefijo|
| PrefixWithdrawalEvent|Advertencia| Se recibió la retirada del prefijo |
| PrefixBackupRouteAnnouncementEvent |Información|Se recibió el anuncio de la ruta de copia de seguridad del prefijo |
| PrefixBackupRouteWithdrawalEvent|Advertencia|Se recibió la retirada de la ruta de copia de seguridad del prefijo |
| PrefixActivePath |Información| Ruta activa actual del prefijo   |
| PrefixBackupPath | Información|Ruta de copia de seguridad actual del prefijo   |
| PrefixOriginAsChangeEvent|Crítico| Prefijo exacto recibido con un número de sistema autónomo de origen diferente (para la ruta activa)| 
| PrefixBackupRouteOriginAsChangeEvent  | Error|Prefijo recibido con un número de sistema autónomo de origen diferente (para la ruta de copia de seguridad)  |

## <a name="next-steps"></a>Pasos siguientes

- Para más información sobre la conexión de Peering Service, consulte [Conexión de Peering Service](connection.md).
- Para más información sobre la telemetría de la conexión de Peering Service, consulte [Telemetría de la conexión de Peering Service](connection-telemetry.md).