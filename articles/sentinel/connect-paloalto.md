---
title: Conexión de datos de Palo Alto Networks con Azure Sentinel | Microsoft Docs
description: Aprenda a usar el conector de datos de Palo Alto Networks para conectar fácilmente los registros de Palo Alto Networks con Azure Sentinel.
services: sentinel
documentationcenter: na
author: yelevin
manager: rkarlin
editor: ''
ms.assetid: a4b21d67-1a72-40ec-bfce-d79a8707b4e1
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/30/2019
ms.author: yelevin
ms.openlocfilehash: 245db436fc3216fe5c8c8f51c50c0ac03190f9eb
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "85564564"
---
# <a name="connect-palo-alto-networks-to-azure-sentinel"></a>Conexión de Palo Alto Networks a Azure Sentinel



En este artículo se explica cómo conectar el dispositivo Palo Alto Networks a Azure Sentinel. El conector de datos de Palo Alto Networks permite conectar fácilmente los registros de Palo Alto Networks con Azure Sentinel para ver paneles, crear alertas personalizadas y mejorar la investigación. El uso de Palo Alto Networks en Azure Sentinel le proporcionará más información sobre el uso de Internet de su organización y mejorará sus capacidades de operación de seguridad. 


## <a name="forward-palo-alto-networks-logs-to-the-syslog-agent"></a>Reenviar los registros de Palo Alto Networks al agente de Syslog

Configure Palo Alto Networks para reenviar mensajes de Syslog en formato CEF a su área de trabajo de Azure a través del agente de Syslog:
1.  Vaya a [Common Event Format (CEF) Configuration Guides](https://docs.paloaltonetworks.com/resources/cef) (Guías de configuración de Common Event Format [CEF]) y descargue el PDF para su tipo de dispositivo. Siga todas las instrucciones de la guía para configurar su dispositivo de Palo Alto Networks a fin de recopilar eventos CEF. 

1.  Vaya a [Configure Syslog monitoring](https://docs.paloaltonetworks.com/pan-os/8-1/pan-os-admin/monitoring/use-syslog-for-monitoring/configure-syslog-monitoring) (Configuración de supervisión de Syslog) y siga los pasos 2 y 3 para configurar el reenvío de eventos CEF de su dispositivo de Palo Alto Networks a Azure Sentinel.

    1. Asegúrese de establecer el **formato de servidor de Syslog** en **BSD**.

       > [!NOTE]
       > Las operaciones de copiar y pegar desde el archivo PDF pueden cambiar el texto e insertar caracteres aleatorios. Para evitarlo, copie el texto en un editor y quite los caracteres que pueden interrumpir el formato del registro antes de pegarlo, como puede ver en este ejemplo.
 
        ![Problema al copiar texto CEF](./media/connect-cef/paloalto-text-prob1.png)

1. Para usar el esquema correspondiente en Log Analytics para encontrar eventos de Palo Alto Networks, busque **CommonSecurityLog**.

1. Diríjase al [PASO 3: Validación de conectividad](connect-cef-verify.md).




## <a name="next-steps"></a>Pasos siguientes
En este documento, ha aprendido a conectar dispositivos de Palo Alto Networks a Azure Sentinel. Para más información sobre Azure Sentinel, consulte los siguientes artículos:
- Aprenda a [obtener visibilidad de los datos y de posibles amenazas](quickstart-get-visibility.md).
- Empiece a [detectar amenazas con Azure Sentinel](tutorial-detect-threats-built-in.md).
- [Use libros](tutorial-monitor-your-data.md) para supervisar los datos.


