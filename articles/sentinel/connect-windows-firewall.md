---
title: Conectar datos de Firewall de Windows a Azure Sentinel| Microsoft Docs
description: Aprenda a conectar datos de Firewall de Windows a Azure Sentinel.
services: sentinel
documentationcenter: na
author: yelevin
manager: rkarlin
editor: ''
ms.assetid: 0e41f896-8521-49b8-a244-71c78d469bc3
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/23/2019
ms.author: yelevin
ms.openlocfilehash: 5d2f68261143c3fc5bbcda0b739af17251eeee63
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "77588066"
---
# <a name="connect-windows-firewall"></a>Conexión del firewall de Windows



El conector de Firewall de Windows le permite conectarse fácilmente a los registros de los Firewall de Windows, si están conectados al área de trabajo Azure Sentinel. Esta conexión le permite ver paneles, crear alertas personalizadas y mejorar la investigación, de modo que dispondrá de más información sobre la red de la organización y de mejores capacidades de seguridad. La solución recopila eventos de firewall de Windows de las máquinas de Windows en las que está instalado un agente de Log Analytics. 


> [!NOTE]
> - Los datos se almacenarán en la ubicación geográfica del área de trabajo en la que Azure Sentinel se ejecute.
> - Si Azure Sentinel y Azure Security Center se recopilan en la misma área de trabajo, no es necesario habilitar la solución de Firewall de Windows a través de este conector. Si de todas formas lo habilitó, no se generarán datos duplicados. 

## <a name="enable-the-connector"></a>Habilitar el conector 

1. En el portal de Azure Sentinel, seleccione **Data connectors** (Conectores de datos) y, después, haga clic en el icono de **Firewall de Windows**. 
1.  Si las máquinas Windows están en Azure:
    1. Haga clic en **Install agent on Azure Windows virtual machine** (Instalar el agente en la máquina virtual Windows de Azure).
    1. En la lista **Máquinas virtuales**, seleccione la máquina Windows que quiera transmitir a Azure Sentinel. Asegúrese de que se trata de una máquina virtual Windows.
    1. En la ventana de la máquina virtual que se abre, haga clic en **Connect**(Conectar).  
    1. Haga clic en **Habilitar** en la ventana **Windows firewall connector** (Conector de Firewall de Windows). 

2. Si la máquina Windows no es una máquina virtual de Azure:
    1. Haga clic en **Install agent on non-Azure machines** (Instalar el agente en máquinas que no son de Azure).
    1. En la ventana **Direct agent** (Agente directo), seleccione **Download Windows agent (64 bit)** (Descargar agente de Windows de [64 bits]) o **Download Windows agent (32 bit)** (Descargar agente de Windows de [32 bits]).
    1. Instale el agente en su máquina Windows. Copie los valores de **Workspace ID** (Identificador de área de trabajo), **Primary key** (Clave principal) y **Secondary key** (Clave secundaria) y úselos cuando se le solicite durante la instalación.

4. Seleccione los tipos de datos que quiera transmitir.
5. Haga clic en **Install solution** (Instalar solución).
6. Para usar el esquema correspondiente en Log Analytics para encontrar Firewall de Windows, busque **SecurityEvent**.

## <a name="validate-connectivity"></a>Validar conectividad

Hasta que los registros empiecen a aparecer en Log Analytics, pueden transcurrir más de 20 minutos. 



## <a name="next-steps"></a>Pasos siguientes
En este documento, ha aprendido a conectar Firewall de Windows a Azure Sentinel. Para más información sobre Azure Sentinel, consulte los siguientes artículos:
- Aprenda a [obtener visibilidad de los datos y de posibles amenazas](quickstart-get-visibility.md).
- Empiece a [detectar amenazas con Azure Sentinel](tutorial-detect-threats-built-in.md).

