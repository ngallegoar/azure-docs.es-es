---
title: Conexión de datos de Pulse Connect Secure a Azure Sentinel | Microsoft Docs
description: Aprenda a conectar datos de Pulse Connect Secure a Azure Sentinel.
services: sentinel
documentationcenter: na
author: yelevin
manager: rkarlin
editor: ''
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/17/2020
ms.author: yelevin
ms.openlocfilehash: 01d41b20c543262280e215cf4e31fd0cccce877f
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/20/2020
ms.locfileid: "86528192"
---
# <a name="connect-your-pulse-connect-secure-to-azure-sentinel"></a>Conexión de Pulse Connect Secure a Azure Sentinel

En este artículo se explica cómo conectar el dispositivo de [Pulse Connect Secure](https://www.pulsesecure.net/products/pulse-connect-secure/) a Azure Sentinel. El conector de datos de Pulse Connect Secure permite conectar fácilmente los registros de Pulse Connect Secure con Azure Sentinel para ver paneles, crear alertas personalizadas y mejorar la investigación. La integración entre Pulse Connect Secure y Azure Sentinel usa Syslog.

> [!NOTE]
> Los datos se almacenarán en la ubicación geográfica del área de trabajo en la que Azure Sentinel se ejecute.

## <a name="forward-pulse-connect-secure-logs-to-the-syslog-agent"></a>Reenvío de registros de Pulse Connect Secure al agente de Syslog  

Configure Pulse Connect Secure para reenviar mensajes de Syslog a su área de trabajo de Azure a través del agente de Syslog.

1. En el portal de Azure Sentinel, haga clic en **Data connectors** (Conectores de datos) y seleccione el conector **Pulse Connect Secure**.

1. Seleccione **Open connector page** (Abrir página del conector).

1. Siga las instrucciones que aparecen en la página **Pulse Connect Secure**.

## <a name="find-your-data"></a>Búsqueda de sus datos

Una vez establecida una conexión correcta, los datos aparecen en Log Analytics bajo Syslog.

## <a name="validate-connectivity"></a>Validar conectividad

Hasta que los registros empiecen a aparecer en Log Analytics, pueden transcurrir más de 20 minutos. 

## <a name="next-steps"></a>Pasos siguientes

En este documento, ha aprendido a conectar Pulse Connect Secure a Azure Sentinel. Para más información sobre Azure Sentinel, consulte los siguientes artículos:

- Aprenda a [obtener visibilidad de los datos y de posibles amenazas](quickstart-get-visibility.md).
- Empiece a [detectar amenazas con Azure Sentinel](tutorial-detect-threats-built-in.md).
- [Use libros](tutorial-monitor-your-data.md) para supervisar los datos.