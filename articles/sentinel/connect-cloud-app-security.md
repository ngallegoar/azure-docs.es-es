---
title: Conexión de datos de Cloud App Security a Azure Sentinel | Microsoft Docs
description: Aprenda a conectar datos de Cloud App Security a Azure Sentinel.
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
ms.date: 10/23/2019
ms.author: yelevin
ms.openlocfilehash: 348576fbbdd1037f9e2e792218b96bbbecf36668
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "77588372"
---
# <a name="connect-data-from-microsoft-cloud-app-security"></a>Conectar datos de Microsoft Cloud App Security 



Se pueden transmitir registros desde [Cloud App Security](https://docs.microsoft.com/cloud-app-security/what-is-cloud-app-security) a Azure Sentinel con tan solo un clic. Esta conexión permite transmitir las alertas de Cloud App Security a Azure Sentinel. 

## <a name="prerequisites"></a>Prerequisites

- Un usuario que sea administrador global o que tenga permisos de administrador de seguridad
- Para transmitir registros de Cloud Discovery a Azure Sentinel, [habilite Azure Sentinel como SIEM en Microsoft Cloud App Security](https://aka.ms/AzureSentinelMCAS).

> [!IMPORTANT]
> La ingesta de registros de Cloud Discovery se encuentra actualmente en versión preliminar pública.
> Esta característica se ofrece sin contrato de nivel de servicio y no se recomienda para cargas de trabajo de producción.
> Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).
 
## <a name="connect-to-cloud-app-security"></a>Conectar a Cloud App Security

Si ya tiene Cloud App Security, asegúrese de que está [habilitado en la red](https://docs.microsoft.com/cloud-app-security/getting-started-with-cloud-app-security).
Si Cloud App Security ya está implementado e ingiriendo datos, los datos de alerta se pueden transmitir a Azure Sentinel muy fácilmente.


1. En Azure Sentinel, seleccione **Data connectors** (Conectores de datos), haga clic en el icono de **Cloud App Security** y seleccione **Open connector page** (abrir la página de conectores).

1. Seleccione qué registros quiere transmitir a Azure Sentinel. Aquí puede elegir **Alertas** y **Registros de Cloud Discovery** (versión preliminar). 

1. Haga clic en **Conectar**.

1. Para usar el esquema correspondiente en Log Analytics para encontrar alertas de Cloud App Security, busque **SecurityAlert**.




## <a name="next-steps"></a>Pasos siguientes
En este documento, ha aprendido a conectar Microsoft Cloud App Security a Azure Sentinel. Para más información sobre Azure Sentinel, consulte los siguientes artículos:
- Aprenda a [obtener visibilidad de los datos y de posibles amenazas](quickstart-get-visibility.md).
- Empiece a [detectar amenazas con Azure Sentinel](tutorial-detect-threats.md).
