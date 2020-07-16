---
title: Conexión de datos de detección de IA Vectra con Azure Sentinel | Microsoft Docs
description: Obtenga información sobre cómo conectar datos de detección de IA Vectra con Azure Sentinel.
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
ms.date: 06/21/2020
ms.author: yelevin
ms.openlocfilehash: 964bab2b81f6ce69e77fd69cab55f6d1894e4edb
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85368421"
---
# <a name="connect-ai-vectra-detect-to-azure-sentinel"></a>Conexión de la detección de IA Vectra con Azure Sentinel

En este artículo se explica cómo conectar el dispositivo [Detección de IA Vectra](https://www.vectra.ai/product/cognito-detect) con Azure Sentinel. El conector de datos de detección de IA Vectra permite incorporar fácilmente los datos de registro de detección de IA Vectra a Azure Sentinel, de modo que pueda verlos en los libros, usarlos para crear alertas personalizadas e incorporarlos para mejorar la investigación.

> [!NOTE]
> Los datos se almacenarán en la ubicación geográfica del área de trabajo en la que Azure Sentinel se ejecute.

## <a name="configure-your-ai-vectra-detect-appliance-to-send-cef-messages"></a>Configuración del dispositivo de detección de IA Vectra para enviar mensajes de CEF  

Configure la detección de IA Vectra para reenviar mensajes de Syslog con formato CEF al área de trabajo de Log Analytics a través del reenviador de Syslog que configuró en [PASO 1: Implementación del reenviador de registros](connect-cef-agent.md).

1. En la interfaz de Vectra, vaya a Configuración > Notificaciones y, después, elija Editar configuración de Syslog. Siga estas instrucciones para configurar la conexión:

    - Agregue un nuevo destino (el nombre de host del [reenviador de registros](connect-cef-agent.md))
    - Establezca el puerto como **514**.
    - Establezca el protocolo como **UDP**.
    - Establezca el formato en **CEF**.
    - Establezca los tipos de registro (seleccione todos los tipos disponibles).
    - Haga clic en **Guardar**.

2. Puede hacer clic en el botón **Test** (Probar) para forzar el envío de algunos eventos de prueba al reenviador de registros.

3. Si quiere usar el esquema correspondiente en Log Analytics para encontrar eventos de detección de IA Vectra, busque **CommonSecurityLog**.

4. Diríjase al [PASO 3: Validación de conectividad](connect-cef-verify.md).

## <a name="next-steps"></a>Pasos siguientes

En este documento, ha aprendido a conectar dispositivos de detección de IA Vectra con Azure Sentinel. Para sacar el máximo partido de las capacidades integradas en este conector de datos, haga clic en la pestaña **Pasos siguientes** de la página del conector de datos. Allí encontrará algunas consultas de ejemplo preparadas para que pueda empezar a buscar información útil.

Para más información sobre Azure Sentinel, consulte los siguientes artículos:

- Aprenda a [obtener visibilidad de los datos y de posibles amenazas](quickstart-get-visibility.md).
- Empiece a [detectar amenazas con Azure Sentinel](tutorial-detect-threats-built-in.md).
- [Use libros](tutorial-monitor-your-data.md) para supervisar los datos.
