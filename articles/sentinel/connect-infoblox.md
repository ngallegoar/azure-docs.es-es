---
title: Conexión de datos de Infoblox Network Identity Operating System (NIOS) a Azure Sentinel | Microsoft Docs
description: Aprenda a conectar datos de Infoblox Network Identity Operating System (NIOS) a Azure Sentinel.
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
ms.openlocfilehash: ed4f2d769dbda3dec7b353fddfd1e5e0f3d00f9b
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/20/2020
ms.locfileid: "86528262"
---
# <a name="connect-your-infoblox-nios-to-azure-sentinel"></a>Conexión de Infoblox NIOS a Azure Sentinel

En este artículo se explica cómo conectar su [dispositivo de Infoblox Network Identity Operating System (NIOS)](https://www.infoblox.com/glossary/network-identity-operating-system-nios/) a Azure Sentinel. El conector de datos de Infoblox NIOS permite conectar fácilmente los registros de Infoblox con Azure Sentinel para ver paneles, crear alertas personalizadas y mejorar la investigación. La integración entre Infoblox NIOS y Azure Sentinel usa Syslog.

> [!NOTE]
> Los datos se almacenarán en la ubicación geográfica del área de trabajo en la que Azure Sentinel se ejecute.

## <a name="forward-infoblox-logs-to-the-syslog-agent"></a>Reenvío de los registros de Infoblox al agente de Syslog  

Configure Infoblox para reenviar mensajes de Syslog a su área de trabajo de Azure a través del agente de Syslog.

1. En el portal de Azure Sentinel, haga clic en **Data connectors** (Conectores de datos) y seleccione el conector **Infoblox NIOS**.

1. Seleccione **Open connector page** (Abrir página del conector).

1. Siga las instrucciones de la página **Infoblox NIOS**.

## <a name="find-your-data"></a>Búsqueda de sus datos

Una vez establecida una conexión correcta, los datos aparecen en Log Analytics bajo Syslog.

## <a name="validate-connectivity"></a>Validar conectividad

Hasta que los registros empiecen a aparecer en Log Analytics, pueden transcurrir más de 20 minutos. 

## <a name="next-steps"></a>Pasos siguientes

En este documento, ha aprendido a conectar Infoblox NIOS a Azure Sentinel. Para más información sobre Azure Sentinel, consulte los siguientes artículos:

- Aprenda a [obtener visibilidad de los datos y de posibles amenazas](quickstart-get-visibility.md).
- Empiece a [detectar amenazas con Azure Sentinel](tutorial-detect-threats-built-in.md).
- [Use libros](tutorial-monitor-your-data.md) para supervisar los datos.