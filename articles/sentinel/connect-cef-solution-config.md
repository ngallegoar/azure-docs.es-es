---
title: Configuración de la solución de seguridad para conectar datos de CEF a la versión preliminar de Azure Sentinel | Microsoft Docs
description: Obtenga información sobre cómo configurar la solución de seguridad para conectar datos de CEF a Azure Sentinel.
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
ms.date: 12/30/2019
ms.author: yelevin
ms.openlocfilehash: e2ed3680a0867ab8f7e2ad41603883f07a4be427
ms.sourcegitcommit: 8e7316bd4c4991de62ea485adca30065e5b86c67
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/17/2020
ms.locfileid: "94655755"
---
# <a name="step-2-configure-your-security-solution-to-send-cef-messages"></a>PASO 2: Configurar la solución de seguridad para enviar mensajes CEF

En este paso, realizará los cambios de configuración necesarios en la solución de seguridad para enviar registros al agente de CEF.

## <a name="configure-a-solution-with-a-connector"></a>Configuración de una solución con un conector

Si la solución de seguridad ya tiene un conector existente, use las instrucciones específicas del conector como se indica a continuación:

- [AI Vectra Detect](connect-ai-vectra-detect.md)
- [Check Point](connect-checkpoint.md)
- [Cisco](connect-cisco.md)
- [Reveal(x) de ExtraHop](connect-extrahop.md)
- [F5 ASM](connect-f5.md)  
- [Fortinet](connect-fortinet.md)
- [One Identity Safeguard](connect-one-identity.md)
- [Palo Alto Networks](connect-paloalto.md)
- [Trend Micro Deep Security](connect-trend-micro.md)
- [Zscaler](connect-zscaler.md)   

## <a name="configure-any-other-solution"></a>Configurar cualquier otra solución

Si no existe un conector para su solución de seguridad específica, use las siguientes instrucciones genéricas para reenviar los registros al agente de CEF.

1. Vaya al artículo de configuración específico para conocer los pasos para configurar la solución para enviar mensajes CEF. Si su solución no aparece, debe establecer estos valores en el dispositivo para que envíe los registros necesarios en el formato necesario al agente de Syslog de Azure Sentinel, según el agente de Log Analytics. Puede modificar estos parámetros en su dispositivo, siempre y cuando modifique también el demonio de Syslog en el agente de Azure Sentinel.
    - Protocolo = TCP
    - Puerto = 514
    - Formato = CEF
    - Dirección IP: Asegúrese de enviar los mensajes CEF a la dirección IP de la máquina virtual que ha dedicado para tales efectos.

   > [!NOTE]
   > Esta solución admite RFC 3164 o RFC 5424 de Syslog.

1. Para usar el esquema correspondiente en Log Analytics para los eventos CEF, busque `CommonSecurityLog`.

1. Diríjase al PASO 3: [validación de conectividad](connect-cef-verify.md).

## <a name="next-steps"></a>Pasos siguientes

En este documento, ha aprendido a conectar dispositivos CEF a Azure Sentinel. Para más información sobre Azure Sentinel, consulte los siguientes artículos:
- Aprenda a [obtener visibilidad de los datos y de posibles amenazas](quickstart-get-visibility.md).
- Empiece a [detectar amenazas con Azure Sentinel](./tutorial-detect-threats-built-in.md).