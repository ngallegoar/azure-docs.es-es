---
title: Datos que se recopilan al abrir un caso para Microsoft Azure Automation | Microsoft Docs
description: En este artículo se describe la información que debe recopilar antes de abrir un caso para Azure Automation en el Soporte técnico de Microsoft Azure.
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
ms.openlocfilehash: d2f97d0b889186324aef9613847e3eddbfe1eb02
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "83684846"
---
# <a name="data-to-collect-when-opening-a-case-for-microsoft-azure-automation"></a>Datos que se recopilan cuando se abre un caso para Microsoft Azure Automation

En este artículo se describe parte de la información que debe recopilar antes de abrir un caso para Azure Automation con el soporte técnico de Microsoft Azure. Esta información no es necesaria para abrir el caso. Sin embargo, puede ayudar a Microsoft a resolver el problema con más rapidez. Además, es posible que el ingeniero de soporte técnico le pida estos datos después de abrir el caso.

## <a name="basic-data"></a>Datos básicos

Recopile los datos básicos descritos en el artículo de Knowledge Base [4034605 sobre la captura de diagnósticos con scripts de Azure Automation](https://support.microsoft.com/help/4034605/how-to-capture-azure-automation-scripted-diagnostics).

## <a name="data-for-update-management-issues-on-linux"></a>Datos de problemas de Update Management en Linux

1. Además de los elementos que se enumeran en el artículo de KB [4034605](https://support.microsoft.com/help/4034605/how-to-capture-azure-automation-scripted-diagnostics), ejecute la siguiente herramienta de recopilación de registros:

   [Recopilador de registros del Agente de Linux de OMS](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/tools/LogCollector/OMS_Linux_Agent_Log_Collector.md)
 
2. Comprima el contenido de la carpeta **/var/opt/microsoft/omsagent/run/automationworker/** y, a continuación, envíe el archivo comprimido al soporte técnico de Azure.
 
3. Compruebe que el identificador del área de trabajo de la que depende el agente de Log Analytics para Linux es el mismo que el del área de trabajo que se está supervisando para ver si hay actualizaciones.

## <a name="data-for-update-management-issues-on-windows"></a>Datos de problemas de Update Management en Windows

1. Recopile datos de los elementos enumerados en [4034605](https://support.microsoft.com/help/4034605/how-to-capture-azure-automation-scripted-diagnostics).

2. Exporte los siguientes registros de eventos en formato EVTX:

   * Sistema
   * Application
   * Seguridad
   * Operations Manager
   * Microsoft-SMA/Operational

3. Compruebe que el identificador del área de trabajo de la que depende el agente es el mismo que el del área de trabajo que Windows Update está supervisando.

## <a name="data-for-job-issues"></a>Datos de problemas de trabajo

1. Recopile datos de los elementos enumerados en [4034605](https://support.microsoft.com/help/4034605/how-to-capture-azure-automation-scripted-diagnostics).

2. Recopile el identificador del trabajo que tiene un problema:

   1. En Azure Portal, vaya a **Cuentas de Automation**.
   2. Seleccione la cuenta de Automation de la que está solucionando el problema y anote el nombre.
   3. Seleccione **Trabajos**.
   4. Seleccione el trabajo del que está solucionando el problema.
   5. En el panel Resumen del trabajo, busque el valor GUID en **Id. de trabajo**.

   ![Id. de trabajo en el panel de resumen del trabajo](media/collect-data-microsoft-azure-automation-case/job-summary-job-id.png)

3. Recopile un ejemplo del script que está ejecutando.

4. Recopile los archivos de registro:

   1. En Azure Portal, vaya a **Cuentas de Automation**.
   2. Seleccione la cuenta de Automation de la que está solucionando el problema.
   3. Seleccione **Trabajos**.
   4. Seleccione el trabajo del que está solucionando el problema.
   5. Seleccione **Todos los registros**.
   6. En el panel resultante, recopile los datos.

   ![Datos enumerados en Todos los registros](media/collect-data-microsoft-azure-automation-case/all-logs-data.png)

## <a name="data-for-module-issues"></a>Datos de problemas de módulos

Además de los [elementos da datos básicos](#basic-data), recopile la siguiente información:

* Los pasos que siguió para que se pueda reproducir el problema.
* Capturas de pantalla de los mensajes de error.
* Capturas de pantalla de los módulos actuales y sus números de versión.

## <a name="next-steps"></a>Pasos siguientes

Si necesita más ayuda:

* Obtenga respuestas de expertos de Azure en los [foros de Azure](https://azure.microsoft.com/support/forums/).
* Póngase en contacto con [@AzureSupport](https://twitter.com/azuresupport), la cuenta oficial de Microsoft Azure para mejorar la experiencia del cliente, que pone en contacto a la comunidad de Azure con los recursos adecuados: respuestas, soporte técnico y expertos.
* Registrar un incidente de soporte técnico de Azure. Vaya al [sitio de soporte técnico de Azure](https://azure.microsoft.com/support/options/) y seleccione **Obtener soporte**.
