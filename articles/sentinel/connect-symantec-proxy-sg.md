---
title: Conexión de datos de Symantec ProxySG a Azure Sentinel | Microsoft Docs
description: Aprenda a conectar datos de Symantec ProxySG a Azure Sentinel.
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
ms.openlocfilehash: 51a5c5de7bce07aa6a54b9ff81e957c38cfffdd2
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/20/2020
ms.locfileid: "86528205"
---
# <a name="connect-your-symantec-proxysg-to-azure-sentinel"></a>Conexión de Symantec ProxySG a Azure Sentinel

En este artículo se explica cómo conectar el dispositivo de [Symantec ProxySG](https://www.broadcom.com/products/cyber-security/network/gateway/proxy-sg-and-advanced-secure-gateway) con Azure Sentinel. El conector de datos de Symantec ProxySG permite conectar fácilmente los registros de Symantec ProxySG con Azure Sentinel para ver paneles, crear alertas personalizadas y mejorar la investigación. La integración entre Symantec ProxySG y Azure Sentinel usa Syslog.

> [!NOTE]
> Los datos se almacenarán en la ubicación geográfica del área de trabajo en la que Azure Sentinel se ejecute.

## <a name="forward-symantec-proxysg-logs-to-the-syslog-agent"></a>Reenvío de registros de Symantec ProxySG al agente de Syslog  

Configure Symantec ProxySG para reenviar mensajes de Syslog a su área de trabajo de Azure a través del agente de Syslog.

1. En el portal de Azure Sentinel, haga clic en **Data connectors** (Conectores de datos) y seleccione el conector **Symantec ProxySG**.

1. Seleccione **Open connector page** (Abrir página del conector).

1. Siga las instrucciones que aparecen en la página **Symantec ProxySG**.

## <a name="find-your-data"></a>Búsqueda de sus datos

Una vez establecida una conexión correcta, los datos aparecen en Log Analytics bajo Syslog.

## <a name="validate-connectivity"></a>Validar conectividad

Hasta que los registros empiecen a aparecer en Log Analytics, pueden transcurrir más de 20 minutos. 

## <a name="next-steps"></a>Pasos siguientes

En este documento, ha aprendido a conectar Symantec ProxySG a Azure Sentinel. Para más información sobre Azure Sentinel, consulte los siguientes artículos:

- Aprenda a [obtener visibilidad de los datos y de posibles amenazas](quickstart-get-visibility.md).
- Empiece a [detectar amenazas con Azure Sentinel](tutorial-detect-threats-built-in.md).
- [Use libros](tutorial-monitor-your-data.md) para supervisar los datos.