---
title: Conexión de Azure Security Center para IoT con Azure Sentinel | Microsoft Docs
description: Aprenda a conectar los datos de Azure Security Center para IoT a Azure Sentinel.
services: sentinel
documentationcenter: na
author: yelevin
manager: rkarlin
editor: ''
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/18/2020
ms.author: yelevin
ms.openlocfilehash: 3af51110a4c4604444573f62be65077c786db606
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "77588644"
---
# <a name="connect-your-data-from-azure-security-center-for-iot-to-azure-sentinel"></a>Conexión de los datos de Azure Security Center para IoT con Azure Sentinel 


> [!IMPORTANT]
> El conector de datos de Azure Security Center para IoT se encuentra actualmente en versión preliminar pública. Esta característica se ofrece sin contrato de nivel de servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas. Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Use el conector de Azure Security Center para IoT para transmitir todos los eventos de Azure Security Center para IoT a Azure Sentinel. 

## <a name="prerequisites"></a>Prerequisites

- Permisos de **lectura** y **escritura** en el área de trabajo donde está implementado Azure Sentinel.
- **Azure Security Center para IoT** debe estar **habilitado** en las instancias pertinentes de IoT Hub.
- Permisos de **lectura** y **escritura** en la instancia de **Azure IoT Hub** que quiere conectar.
- Permisos de **lectura** y **escritura** en el **grupo de recursos de Azure IoT Hub**.

> [!NOTE]
> Aunque debe habilitar la licencia del nivel **Estándar** de Azure Security Center en su suscripción para transmitir las alertas de recursos de IoT a Azure Sentinel, solo tiene que habilitar la licencia del nivel **Gratis** de Azure Security Center en su suscripción para ver alertas de Azure Security Center para IoT en Azure Sentinel. 

## <a name="connect-to-azure-security-center-for-iot"></a>Conexión a Azure Security Center para IoT

1. En Azure Sentinel, seleccione **Conectores de datos** y, luego, haga clic en el mosaico **Azure Security Center para IoT**.
1. Desde el panel inferior derecho, haga clic en **Abrir la página del conector**. 
1. Haga clic en **Conectar** junto a cada suscripción de IoT Hub cuyas alertas y alertas de dispositivo quiera transmitir a Azure Sentinel. 
    - Si Azure Security Center para IoT no está habilitado en esa instancia de Hub, verá el mensaje de advertencia **Habilitar**. Haga clic en el enlace **Habilitar** para iniciar el servicio. 
1. Puede decidir si quiere que las alertas de Azure Security Center para IoT generen incidentes automáticamente en Azure Sentinel. En **Creación de incidentes**, seleccione **Habilitar** para habilitar la regla analítica predeterminada que crea incidentes automáticamente a partir de alertas generadas en el servicio de seguridad conectado. Esta regla se puede cambiar o editar en **Análisis** > **Activar** reglas.

> [!NOTE]
> Puede tardar cierto tiempo en actualizar la lista de centros de conectividad tras realizar cambios en la conexión. 

## <a name="log-analytics-alert-display"></a>Visualización de alertas de Log Analytics

Para usar el esquema correspondiente en Log Analytics para mostrar las alertas de Azure Security Center para IoT:

1. Abra **Registros** > **SecurityInsights** > **SecurityAlert** o busque **SecurityAlert**. 
2. Use el siguiente filtro kql para ver solo las alertas generadas por Azure Security Center para IoT:

```kusto
SecurityAlert | where ProductName == "Azure Security Center for IoT"
``` 

### <a name="service-notes"></a>Notas del servicio

Tras conectar una instancia de IoT Hub, los datos del centro de conectividad están disponibles en Azure Sentinel aproximadamente 15 minutos más tarde.


## <a name="next-steps"></a>Pasos siguientes

En este documento, ha aprendido a conectar datos de Azure Security Center para IoT a Azure Sentinel. Para más información sobre Azure Sentinel, consulte los siguientes artículos:
- Aprenda a [obtener visibilidad de los datos y de posibles amenazas](quickstart-get-visibility.md).
- Empiece a [detectar amenazas con Azure Sentinel](tutorial-detect-threats-built-in.md).
- [Use libros](tutorial-monitor-your-data.md) para supervisar los datos.
