---
title: Conectar datos de Fortinet a Azure Sentinel| Microsoft Docs
description: Conecte un dispositivo Fortinet a Azure Sentinel para ver los paneles, crear alertas personalizadas y mejorar la investigación. 
services: sentinel
documentationcenter: na
author: yelevin
manager: rkarlin
editor: ''
ms.assetid: add92907-0d7c-42b8-a773-f570f2d705ff
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/30/2019
ms.author: yelevin
ms.openlocfilehash: 72ba4acd694933db503b01611e63867af99a129f
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85555955"
---
# <a name="connect-fortinet-to-azure-sentinel"></a>Conexión de Fortinet a Azure Sentinel



En este artículo se explica cómo conectar el dispositivo Fortinet a Azure Sentinel. El conector de datos de Fortinet permite conectar fácilmente los registros de Fortinet con Azure Sentinel para ver paneles, crear alertas personalizadas y mejorar la investigación. El uso de Fortinet en Azure Sentinel le proporcionará más información sobre el uso de Internet de su organización y mejorará sus capacidades de operación de seguridad. 


 
## <a name="forward-fortinet-logs-to-the-syslog-agent"></a>Reenviar los registros de Fortinet al agente de Syslog

Configure Fortinet para reenviar mensajes de Syslog en formato CEF a su área de trabajo de Azure a través del agente de Syslog.

1. Abra la CLI en su dispositivo Fortinet y ejecute los siguientes comandos:

        config log syslogd setting
        set format cef
        set port 514
        set server <ip_address_of_Receiver>
        set status enable
        end

    - Reemplace la **dirección IP** del servidor por la dirección IP del agente.
    - Establezca el **puerto de syslog** en **514** o el puerto establecido en el agente.
    - Para habilitar el formato CEF en las primeras versiones de FortiOS, es posible que deba ejecutar el conjunto de comandos **csv disable**.
 
   > [!NOTE] 
   > Para obtener más información, vaya a la [biblioteca de documentos de Fortinet](https://aka.ms/asi-syslog-fortinet-fortinetdocumentlibrary). Seleccione su versión y use el **manual** y la **referencia de mensajes de registro**.

1. Para usar el esquema correspondiente en Log Analytics de Azure Monitor para encontrar eventos de Fortinet, busque `CommonSecurityLog`.

1. Diríjase al [PASO 3: Validación de conectividad](connect-cef-verify.md).


## <a name="next-steps"></a>Pasos siguientes
En este artículo, ha aprendido a conectar dispositivos Fortinet a Azure Sentinel. Para más información sobre Azure Sentinel, consulte los siguientes artículos:
- Aprenda a [obtener visibilidad de los datos y de posibles amenazas](quickstart-get-visibility.md).
- Empiece a [detectar amenazas con Azure Sentinel](tutorial-detect-threats-built-in.md).
- [Use libros](tutorial-monitor-your-data.md) para supervisar los datos.


