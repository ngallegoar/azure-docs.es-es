---
title: Conexión de datos de Check Point con Azure Sentinel | Microsoft Docs
description: Aprenda a conectar los datos de Check Point a Azure Sentinel.
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
ms.openlocfilehash: 70836ec557eff1be035d92e8e7db30a882e05fc6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "77588423"
---
# <a name="connect-check-point-to-azure-sentinel"></a>Conexión de Check Point a Azure Sentinel



En este artículo se explica cómo conectar el dispositivo Check Point a Azure Sentinel. El conector de datos de Check Point permite conectar fácilmente los registros de Check Point con Azure Sentinel para ver paneles, crear alertas personalizadas y mejorar la investigación. El uso de Check Point en Azure Sentinel le proporcionará más información sobre el uso de Internet de su organización y mejorará sus capacidades de operación de seguridad. 

## <a name="forward-check-point-logs-to-the-syslog-agent"></a>Reenvío de los registros de Check Point al agente de Syslog

Configure el dispositivo Check Point para reenviar mensajes de Syslog en formato CEF a su área de trabajo de Azure a través del agente de Syslog.

1. Vaya a [Check Point Log Export](https://aka.ms/asi-syslog-checkpoint-forwarding) (Exportación del registro de Check Point).
1. Desplácese hacia abajo hasta **Implementación básica** y siga las instrucciones para configurar la conexión según lo que se indica a continuación:
   - Establezca el **puerto de Syslog** en **514** o en el puerto que establezca en el agente.
     - Reemplace el **nombre** y la **dirección IP del servidor de destino** en la CLI con la dirección IP y el nombre del agente de Syslog.
     - Establezca el formato en **CEF**.
1. Si usa la versión R77.30 o R80.10, desplácese hacia arriba hasta **Instalaciones** y siga las instrucciones para instalar un exportador de registros para su versión.
1. Diríjase al [PASO 3: Validación de conectividad](connect-cef-verify.md).
 

## <a name="next-steps"></a>Pasos siguientes
En este documento, ha aprendido a conectar dispositivos de Check Point a Azure Sentinel. Para más información sobre Azure Sentinel, consulte los siguientes artículos:
- [Validación de conectividad](connect-cef-verify.md).
- Empiece a [detectar amenazas con Azure Sentinel](tutorial-detect-threats-built-in.md).
- [Use libros](tutorial-monitor-your-data.md) para supervisar los datos.


