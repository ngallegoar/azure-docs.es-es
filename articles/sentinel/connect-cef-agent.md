---
title: Implementar el agente para conectar CEF a la versión preliminar de Azure Sentinel | Microsoft Docs
description: Aprenda a implementar el agente para conectar datos CEF a Azure Sentinel.
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
ms.date: 11/26/2019
ms.author: yelevin
ms.openlocfilehash: b0c9335357cb793ea76e1dbe68575f716a50372a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "77588474"
---
# <a name="step-1-deploy-the-agent"></a>Paso 1: Implementar el agente


En este paso, debe seleccionar la máquina Linux que actuará como proxy entre Azure Sentinel y la solución de seguridad. Tendrá que ejecutar un script en el equipo proxy que:
- Instala el agente de Log Analytics y lo configura según sea necesario para escuchar los mensajes de Syslog.
- Configura el demonio de Syslog para escuchar mensajes de Syslog mediante el puerto TCP 514 y, a continuación, reenvía solo los mensajes CEF al agente de Log Analytics mediante el puerto TCP 25226.
- Ajuste el agente de Syslog para que recopile los datos y los envíe de forma segura a Azure Sentinel, donde se analizan y enriquecen.
 
## <a name="deploy-the-agent"></a>Implementar el agente
 
1. En el portal de Azure Sentinel, haga clic en **Data connectors** (Conectores de datos) y seleccione **Common Event Format (CEF)** y, luego, **Open connector page** (Abrir página de conectores). 

1. En **Instalar y configurar el agente de Syslog**, seleccione el tipo de máquina, ya sea en Azure, en otra nube o local. 
   > [!NOTE]
   > Dado que el script del paso siguiente instala el agente de Log Analytics y conecta el equipo al área de trabajo de Azure Sentinel, asegúrese de que esta máquina no esté conectada a ninguna otra área de trabajo.
1. Debe tener permisos elevados (sudo) en la máquina. Asegúrese de que tiene Python en la máquina con el siguiente comando: `python –version`.

1. Ejecute el siguiente script en la máquina proxy.
   `sudo wget https://raw.githubusercontent.com/Azure/Azure-Sentinel/master/DataConnectors/CEF/cef_installer.py&&sudo python cef_installer.py [WorkspaceID] [Workspace Primary Key]`
1. Mientras se ejecuta el script, asegúrese de que no recibe ningún mensaje de error o de advertencia.

Diríjase al [PASO 2: Configurar la solución de seguridad para reenviar mensajes CEF](connect-cef-solution-config.md).


## <a name="next-steps"></a>Pasos siguientes
En este documento, ha aprendido a conectar dispositivos CEF a Azure Sentinel. Para más información sobre Azure Sentinel, consulte los siguientes artículos:
- Aprenda a [obtener visibilidad de los datos y de posibles amenazas](quickstart-get-visibility.md).
- Empiece a [detectar amenazas con Azure Sentinel](tutorial-detect-threats.md).

