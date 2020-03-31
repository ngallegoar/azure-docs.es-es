---
title: Conexión de datos de ATP de Microsoft Defender con Azure Sentinel | Microsoft Docs
description: Aprenda a conectar los datos de Protección contra amenazas avanzada de Microsoft Defender con Azure Sentinel.
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
ms.date: 10/13/2019
ms.author: yelevin
ms.openlocfilehash: ee2c917d02a442ebabc0e1b40bc245982af794f8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "77588219"
---
# <a name="connect-alerts-from-microsoft-defender-advanced-threat-protection"></a>Conexión de alertas desde Protección contra amenazas avanzada de Microsoft Defender 


> [!IMPORTANT]
> La ingesta de los registros de Protección contra amenazas avanzada de Microsoft Defender se encuentra actualmente en versión preliminar pública.
> Esta característica se ofrece sin contrato de nivel de servicio y no se recomienda para cargas de trabajo de producción.
> Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).
 

Se pueden transmitir registros desde [Protección contra amenazas avanzada de Microsoft Defender](https://docs.microsoft.com/windows/security/threat-protection/microsoft-defender-atp/microsoft-defender-advanced-threat-protection) a Azure Sentinel con tan solo un clic. Esta conexión permite transmitir las alertas de Protección contra amenazas avanzada de Microsoft Defender a Azure Sentinel. 

## <a name="prerequisites"></a>Prerequisites

- Una licencia válida para Protección contra amenazas avanzada de Microsoft Defender habilitada, tal como se describe en [Validación del aprovisionamiento de licencias y configuración completa para Protección contra amenazas avanzada de Microsoft Defender](https://docs.microsoft.com/windows/security/threat-protection/microsoft-defender-atp/licensing). 
- Debe ser un administrador o un administrador de seguridad en el inquilino de Azure Sentinel.


## <a name="connect-to-microsoft-defender-advanced-threat-protection"></a>Conexión a Protección contra amenazas avanzada de Microsoft Defender

Si Protección contra amenazas avanzada de Microsoft Defender ya está implementada e ingiriendo datos, las alertas se pueden transmitir a Azure Sentinel muy fácilmente.


1. En Azure Sentinel, seleccione **Data connectors** (Conectores de datos), haga clic en el icono **Protección contra amenazas avanzada de Microsoft Defender** y seleccione **Open connector page** (Abrir la página del conector).
1. Haga clic en **Conectar**. 
1. Para usar el esquema correspondiente en Log Analytics para encontrar alertas de ATP de Defender, busque **SecurityAlert** y que el **nombre del proveedor** sea **MDATP**.




## <a name="next-steps"></a>Pasos siguientes
En este documento, ha aprendido a conectar ATP de Microsoft Defender a Azure Sentinel. Para más información sobre Azure Sentinel, consulte los siguientes artículos:
- Aprenda a [obtener visibilidad de los datos y de posibles amenazas](quickstart-get-visibility.md).
- Empiece a [detectar amenazas con Azure Sentinel](tutorial-detect-threats.md).
