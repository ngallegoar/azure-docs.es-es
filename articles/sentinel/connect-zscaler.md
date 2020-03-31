---
title: Conexión de datos de Zscaler a Azure Sentinel | Microsoft Docs
description: Aprenda a conectar datos de Zscaler a Azure Sentinel.
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
ms.openlocfilehash: cc784afe5db64ccc4aad13fae7a2fa748e4befa3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "77587998"
---
# <a name="connect-zscaler-internet-access-to-azure-sentinel"></a>Conectar Zscaler Internet Access a Azure Sentinel

> [!IMPORTANT]
> El conector de datos de Zscaler en Azure Sentinel se encuentra actualmente en versión preliminar pública.
> Esta característica se ofrece sin contrato de nivel de servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas. Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

En este artículo se explica cómo conectar el dispositivo Zscaler Internet Access a Azure Sentinel. El conector de datos de Zscaler permite conectar fácilmente los registros de Zscaler Internet Access (ZIA) con Azure Sentinel para ver paneles, crear alertas personalizadas y mejorar la investigación. El uso de Zscaler en Azure Sentinel le proporcionará más información sobre el uso de Internet de su organización y mejorará sus capacidades de operación de seguridad. 


## <a name="configure-your-zscaler-to-send-cef-messages"></a>Configurar Zscaler para enviar mensajes de CEF

1. Debe establecer estos valores en el dispositivo Zscaler para que envíe los registros necesarios en el formato necesario al agente de Syslog de Azure Sentinel, según el agente de Log Analytics. Puede modificar estos parámetros en su dispositivo, siempre y cuando modifique también el demonio de Syslog en el agente de Azure Sentinel.
    - Protocolo = TCP
    - Puerto = 514
    - Formato = CEF
    - Dirección IP: Asegúrese de enviar los mensajes CEF a la dirección IP de la máquina virtual que ha dedicado para tales efectos.
 Para más información, consulte la [Guía de implementación de Zscaler y Azure Sentinel](https://aka.ms/ZscalerCEFInstructions).
 
   > [!NOTE]
   > Esta solución admite RFC 3164 o RFC 5424 de Syslog.


1. Para usar el esquema correspondiente en Log Analytics para los eventos CEF, busque `CommonSecurityLog`.
1. Vaya al [PASO 3: Validación de conectividad](connect-cef-verify.md).


## <a name="next-steps"></a>Pasos siguientes
En este documento, ha aprendido a conectar Zscaler Internet Access a Azure Sentinel. Para más información sobre Azure Sentinel, consulte los siguientes artículos:
- Aprenda a [obtener visibilidad de los datos y de posibles amenazas](quickstart-get-visibility.md).
- Empiece a [detectar amenazas con Azure Sentinel](tutorial-detect-threats.md).
- [Use libros](tutorial-monitor-your-data.md) para supervisar los datos.


