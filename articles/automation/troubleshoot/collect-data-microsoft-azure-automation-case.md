---
title: Datos que se recopilan al abrir un caso para Microsoft Azure Automation | Microsoft Docs
description: En este artículo se describe parte de la información básica que debe recopilar antes de abrir un caso para Azure Automation con el soporte técnico de Microsoft Azure.
services: virtual-machines-windows, azure-resource-manager
documentationcenter: ''
author: v-miegge
manager: dcscontentpm
editor: ''
tags: top-support-issue, azure-resource-manager
ms.service: virtual-machines-windows
ms.workload: na
ms.tgt_pltfrm: vm-windows
ms.topic: troubleshooting
ms.date: 09/23/2019
ms.author: v-miegge
ms.openlocfilehash: 4839ce7a0188c782656fd3a4c42cbdd116b165e9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "74849384"
---
# <a name="data-to-collect-when-you-open-a-case-for-microsoft-azure-automation"></a>Datos que se recopilan al abrir un caso para Microsoft Azure Automation

En este artículo se describe parte de la información básica que debe recopilar antes de abrir un caso para Azure Automation con el soporte técnico de Microsoft Azure. Esta información no es necesaria para abrir el caso. Sin embargo, puede ayudar a Microsoft a resolver el problema con más rapidez. Además, es posible que el ingeniero de soporte técnico le pida estos datos después de abrir el caso.

## <a name="collect-more-information"></a>Recopilación de más información

Antes de abrir un caso para el soporte técnico de Microsoft Azure Automation, se recomienda recopilar la siguiente información.

### <a name="basic-data-collection"></a>Recopilación de datos básicos

Recopile los datos descritos en el siguiente artículo de Knowledge Base:

* [4034605](https://support.microsoft.com/help/4034605/how-to-capture-azure-automation-scripted-diagnostics) Cómo capturar diagnósticos generados por script de Azure Automation

## <a name="collect-data-depending-on-issue"></a>Recopilación de datos en función del problema
 
### <a name="for-issues-that-affect-update-management-on-linux"></a>Para problemas que afectan a Update Management en Linux

1. Además de los elementos que se enumeran en el artículo de KB [4034605](https://support.microsoft.com/help/4034605/how-to-capture-azure-automation-scripted-diagnostics), ejecute la siguiente herramienta de recopilación de registros:

   [Recopilador de registros del Agente de Linux de OMS](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/tools/LogCollector/OMS_Linux_Agent_Log_Collector.md)
 
2. Comprima el contenido de la siguiente carpeta y, a continuación, envíe el archivo comprimido al soporte técnico de Azure:

   ``/var/opt/microsoft/omsagent/run/automationworker/``
 
3. Compruebe si el id. del área de trabajo de la que está informando el Agente de Linux de OMS es el mismo que el del área de trabajo que se está supervisando para las actualizaciones.

### <a name="for-issues-that-affect-update-management-on-windows"></a>Para problemas que afectan a Update Management en Windows

Además de los elementos que se enumeran en [4034605](https://support.microsoft.com/help/4034605/how-to-capture-azure-automation-scripted-diagnostics), haga lo siguiente:

1. Exporte los siguientes registros de eventos en formato EVTX:

   * Sistema
   * Application
   * Seguridad
   * Operations Manager
   * Microsoft-SMA/Operational

2. Compruebe si el id. del área de trabajo de la que está informando el agente es el mismo que el del área de trabajo que está supervisando Windows Update.

### <a name="for-issues-that-affect-a-job"></a>Para problemas que afectan a un trabajo

Además de los elementos que se enumeran en [4034605](https://support.microsoft.com/help/4034605/how-to-capture-azure-automation-scripted-diagnostics), recopile la siguiente información:

1. Recopile el número de **JobID** (para el trabajo que está experimentando un problema):

   1. En Azure Portal, vaya a la hoja **Cuentas de Automation**.
   2. Seleccione la **cuenta de Automation** de la que está solucionando el problema.
   3. Seleccione **Trabajos**.
   4. Seleccione el trabajo del que está solucionando el problema.
   5. En **Resumen del trabajo** y busque un **Id. de trabajo** (GUID).

   ![Id. de trabajo en el panel de resumen del trabajo](media/collect-data-microsoft-azure-automation-case/job-summary-job-id.png)

2. Recopile el nombre de la cuenta:

   1. En Azure Portal, vaya a la hoja **Cuentas de Automation**.
   2. Obtenga el nombre de la **cuenta de Automation** de la que está solucionando el problema.

3. Recopile el ejemplo del script que está ejecutando.

4. Recopile los archivos de registro:

   1. En Azure Portal, vaya a la hoja **Cuentas de Automation**.
   2. Seleccione la **cuenta de Automation** de la que está solucionando el problema.
   3. Seleccione **Trabajos**.
   4. Seleccione el trabajo del que está solucionando el problema.
   5. Seleccione **Todos los registros**.
   6. En la hoja resultante, recopile los datos.

   ![Datos enumerados en Todos los registros](media/collect-data-microsoft-azure-automation-case/all-logs-data.png)

### <a name="for-issues-that-affect-modules"></a>Para problemas que afectan a los módulos

Además de los elementos incluidos en "Recopilación de datos básicos", recopile la siguiente información:

* Los pasos que siguió para poder reproducir el problema.
* Una captura de pantalla de cualquier mensaje de error.
* Una captura de pantalla de los módulos actuales y sus números de versión.

## <a name="next-steps"></a>Pasos siguientes

Si necesita más ayuda con cualquier aspecto de este artículo, póngase en contacto con los expertos de Azure en los [foros de MSDN Azure o Stack Overflow](https://azure.microsoft.com/support/forums/).

Como alternativa, puede registrar un incidente de soporte técnico de Azure. Vaya al [sitio de soporte técnico de Azure](https://azure.microsoft.com/support/options/) y seleccione **Obtener soporte**.
