---
title: Conexión de datos de F5 BIG-IP con Azure Sentinel | Microsoft Docs
description: Aprenda a conectar datos F5 BIG-IP a Azure Sentinel.
services: sentinel
documentationcenter: na
author: yelevin
manager: rkarlin
editor: ''
ms.assetid: d068223f-395e-46d6-bb94-7ca1afd3503c
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/30/2019
ms.author: yelevin
ms.openlocfilehash: ae361c74b261bdd6a5673040f868392282b573ef
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "77588287"
---
# <a name="connect-your-f5-big-ip-appliance"></a>Conexión del dispositivo de F5 BIG-IP 

> [!IMPORTANT]
> El conector de datos de F5 BIG-IP en Azure Sentinel se encuentra actualmente en versión preliminar pública.
> Esta característica se ofrece sin contrato de nivel de servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas. Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

El conector de datos de F5 BIG-IP permite conectar fácilmente todos los registros de F5 BIG-IP con Azure Sentinel para ver libros, crear alertas personalizadas y mejorar la investigación. de modo que dispondrá de más información sobre la red de la organización y de mejores capacidades de seguridad. La integración entre F5 BIG-IP y Azure Sentinel usa la API de REST.


> [!NOTE]
> Los datos se almacenarán en la ubicación geográfica del área de trabajo en la que Azure Sentinel se ejecute.

## <a name="configure-and-connect-f5-big-ip"></a>Configuración y conexión de F5 BIG-IP 

F5 BIG-IP puede integrar y exportar los registros directamente a Azure Sentinel.

1. En el portal de Azure Sentinel, haga clic en **Data connectors** (Conectores de datos) y seleccione **F5 BIG-IP** y, luego, **Open connector page** (Abrir página de conectores). 
1. Para conectar F5 BIG-IP, tiene que publicar una declaración JSON en el punto de conexión de la API del sistema. Para obtener instrucciones sobre cómo hacerlo, consulte [Integración de F5 BIG-IP con Azure Sentinel](https://devcentral.f5.com/s/articles/Integrating-the-F5-BIGIP-with-Azure-Sentinel).
8. En la página de conectores de F5 BIG-IP, copie el identificador del área de trabajo y la clave principal y péguelos como se indica en [Transmisión de datos a Azure Log Analytics](https://devcentral.f5.com/s/articles/Integrating-the-F5-BIGIP-with-Azure-Sentinel#streaming-data-to-azure-log-analytics).
1. Después de completar las instrucciones de F5 BIG-IP, en la página de conectores de Azure Sentinel, verá los tipos de datos conectados.
1. Para usar el esquema pertinente en Log Analytics para los eventos F5 BIG-IP, busque **F5Telemetry_LTM_CL**, **F5Telemetry_system_CL** y **F5Telemetry_ASM_CL**.


## <a name="validate-connectivity"></a>Validar conectividad

Hasta que los registros empiecen a aparecer en Log Analytics, pueden transcurrir más de 20 minutos. 



## <a name="next-steps"></a>Pasos siguientes
En este documento, ha aprendido a conectar F5 BIG-IP a Azure Sentinel. Para más información sobre Azure Sentinel, consulte los siguientes artículos:
- Aprenda a [obtener visibilidad de los datos y de posibles amenazas](quickstart-get-visibility.md).
- Empiece a [detectar amenazas con Azure Sentinel](tutorial-detect-threats-built-in.md).
- [Use libros](tutorial-monitor-your-data.md) para supervisar los datos.


