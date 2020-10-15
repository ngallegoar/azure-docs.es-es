---
title: Recomendaciones de Endpoint Protection en Azure Security Center
description: Cómo se detectan las soluciones de Endpoint Protection y cómo se determina que tienen un estado correcto.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: 2730a2f5-20bc-4027-a1c2-db9ed0539532
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/29/2019
ms.author: memildin
ms.openlocfilehash: 7a9541eb3b7c662b43de0d3a609ecec4fe2621ca
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "86519412"
---
# <a name="endpoint-protection-assessment-and-recommendations-in-azure-security-center"></a>Valoración y recomendaciones de Endpoint Protection en Azure Security Center

Azure Security Center proporciona evaluaciones de estado de las versiones [admitidas](security-center-services.md#endpoint-supported) de las soluciones de Endpoint Protection. En este artículo se explican los escenarios que llevan a Security Center a generar las dos recomendaciones siguientes:

* **Instalar soluciones de Endpoint Protection en la máquina virtual**
* **Resolver problemas de mantenimiento de Endpoint Protection en las máquinas**

## <a name="windows-defender"></a>Windows Defender

* Security Center recomienda **"Instalar soluciones de Endpoint Protection en la máquina virtual"** cuando se ejecuta [Get-MpComputerStatus](https://docs.microsoft.com/powershell/module/defender/get-mpcomputerstatus?view=win10-ps) y el resultado es **AMServiceEnabled: False**

* Security Center recomiendan **"Resolver problemas de mantenimiento de Endpoint Protection en las máquinas"** cuando se ejecuta [Get-MpComputerStatus](https://docs.microsoft.com/powershell/module/defender/get-mpcomputerstatus?view=win10-ps) y ocurre algo de lo siguiente:

  * Todas estas propiedades son false:

    **AMServiceEnabled**

    **AntispywareEnabled**

    **RealTimeProtectionEnabled**

    **BehaviorMonitorEnabled**

    **IoavProtectionEnabled**

    **OnAccessProtectionEnabled**

  * Si una de las siguientes propiedades, o las dos, es mayor o igual que 7.

    **AntispywareSignatureAge**

    **AntivirusSignatureAge**

## <a name="microsoft-system-center-endpoint-protection"></a>Microsoft System Center Endpoint Protection

* Security Center recomienda **"Instalar soluciones de Endpoint Protection en la máquina virtual"** cuando la importación de **SCEPMpModule ("$env:ProgramFiles\Microsoft Security Client\MpProvider\MpProvider.psd1" )** y la ejecución de **Get-MProtComputerStatus** da como resultado **AMServiceEnabled = false**.

* Security Center recomienda **"Resolver problemas de mantenimiento de Endpoint Protection en las máquinas"** cuando se ejecuta **Get-MprotComputerStatus** y ocurre algo de lo siguiente:

  * Al menos una de las propiedades siguientes es false:

    * **AMServiceEnabled**
    * **AntispywareEnabled**
    * **RealTimeProtectionEnabled**
    * **BehaviorMonitorEnabled**
    * **IoavProtectionEnabled**
    * **OnAccessProtectionEnabled**

  * Si una de las siguientes actualizaciones de firma, o las dos, es mayor o igual que 7. 

    * **AntispywareSignatureAge**
    * **AntivirusSignatureAge**

## <a name="trend-micro"></a>Trend Micro

* Security Center recomienda **"Instalar soluciones de Endpoint Protection en la máquina virtual"** si no se satisface ninguna de las siguientes comprobaciones:
    * **HKLM:\SOFTWARE\TrendMicro\Deep Security Agent** existe.
    * **HKLM:\SOFTWARE\TrendMicro\Deep Security Agent\InstallationFolder** existe.
    * El archivo **dsa_query.cmd** se encuentra en la carpeta de instalación.
    * La ejecución de **dsa_query.cmd** da como resultado **Component.AM.mode: on - Trend Micro Deep Security Agent detected**.

## <a name="symantec-endpoint-protection"></a>Symantec Endpoint Protection
Security Center recomienda **"Instalar soluciones de Endpoint Protection en la máquina virtual"** si no se satisface ninguna de las siguientes comprobaciones:

* **HKLM:\Software\Symantec\Symantec Endpoint Protection\CurrentVersion\PRODUCTNAME = "Symantec Endpoint Protection"**

* **HKLM:\Software\Symantec\Symantec Endpoint Protection\CurrentVersion\public-opstate\ASRunningStatus = 1**

Or

* **HKLM:\Software\Wow6432Node\Symantec\Symantec Endpoint Protection\CurrentVersion\PRODUCTNAME = "Symantec Endpoint Protection"**

* **HKLM:\Software\Wow6432Node\Symantec\Symantec Endpoint Protection\CurrentVersion\public-opstate\ASRunningStatus = 1**

Security Center recomienda **"Resolver problemas de estado de protección de puntos de conexión en las máquinas"** si no se satisface ninguna de las siguientes comprobaciones:

* Comprobar que la versión de Symantec es >= 12: Ubicación del Registro: **HKLM:\Software\Symantec\Symantec Endpoint Protection\CurrentVersion" -Value "PRODUCTVERSION"**

* Comprobar el estado de protección en tiempo real: **HKLM:\Software\Wow6432Node\Symantec\Symantec Endpoint Protection\AV\Storages\Filesystem\RealTimeScan\OnOff == 1**

* Comprobar el estado de actualización de firma: **HKLM\Software\Symantec\Symantec Endpoint Protection\CurrentVersion\public-opstate\LatestVirusDefsDate <= 7 days**

* Comprobar el estado de examen completo: **HKLM:\Software\Symantec\Symantec Endpoint Protection\CurrentVersion\public-opstate\LastSuccessfulScanDateTime <= 7 days**

* Buscar la ruta del número de versión de firma para la versión de firma para Symantec 12: **Rutas de acceso del Registro + "CurrentVersion\SharedDefs" -Value "SRTSP"** 

* Ruta de acceso a la versión de firma para Symantec 14: **Rutas de acceso del Registro + "CurrentVersion\SharedDefs\SDSDefs" -Value "SRTSP"**

Rutas de acceso del Registro:

* **"HKLM:\Software\Symantec\Symantec Endpoint Protection" + $Path;**
* **"HKLM:\Software\Wow6432Node\Symantec\Symantec Endpoint Protection" + $Path**

## <a name="mcafee-endpoint-protection-for-windows"></a>McAfee Endpoint Protection para Windows

Security Center recomienda **"Instalar soluciones de Endpoint Protection en la máquina virtual"** si no se satisface ninguna de las siguientes comprobaciones:

* **HKLM:\SOFTWARE\McAfee\Endpoint\AV\ProductVersion** existe.

* **HKLM:\SOFTWARE\McAfee\AVSolution\MCSHIELDGLOBAL\GLOBAL\enableoas = 1**

Security Center recomienda **"Resolver problemas de estado de protección de puntos de conexión en las máquinas"** si no se satisface ninguna de las siguientes comprobaciones:

* Versión de McAfee: **HKLM:\SOFTWARE\McAfee\Endpoint\AV\ProductVersion >= 10**

* Buscar la versión de la firma: **HKLM:\Software\McAfee\AVSolution\DS\DS -Value "dwContentMajorVersion"**

* Buscar la fecha de la firma: **HKLM:\Software\McAfee\AVSolution\DS\DS -Value "szContentCreationDate" >= 7 days**

* Buscar la fecha del examen: **HKLM:\Software\McAfee\Endpoint\AV\ODS -Value "LastFullScanOdsRunTime" >= 7 days**

## <a name="mcafee-endpoint-security-for-linux-threat-prevention"></a>McAfee Endpoint Security for Linux Threat Prevention 

Security Center recomienda **"Instalar soluciones de Endpoint Protection en la máquina virtual"** si no se satisface ninguna de las siguientes comprobaciones:

- El archivo **/opt/isec/ens/threatprevention/bin/isecav** se cierra 

- El resultado de **"/opt/isec/ens/threatprevention/bin/isecav --version"** es: **El nombre de McAfee = McAfee Endpoint Security for Linux Threat Prevention y la versión de McAfee >= 10**

Security Center recomienda **"Resolver problemas de estado de protección de puntos de conexión en las máquinas"** si no se satisface ninguna de las siguientes comprobaciones:

- **"/opt/ISEC/ENS/threatprevention/bin/isecav--listtask"** devuelve **Quick scan, Full scan** (Examen rápido, examen completo) y ambos exámenes < = 7 días

- **"/opt/ISEC/ENS/threatprevention/bin/isecav--listtask"** devuelve **DAT and engine Update time** (Motor DAT y hora de actualización) y ambos exámenes < = 7 días

- **"/opt/ISEC/ENS/threatprevention/bin/isecav--getoasconfig--summary"** devuelve el estado **On Access Scan** (En el examen de acceso)

## <a name="sophos-antivirus-for-linux"></a>Antivirus Sophos para Linux 

Security Center recomienda **"Instalar soluciones de Endpoint Protection en la máquina virtual"** si no se satisface ninguna de las siguientes comprobaciones:

- El archivo **/opt/Sophos-AV/bin/savdstatus** se cierra o busca la ubicación **"readlink $(which savscan)"**

- **"/opt/sophos-av/bin/savdstatus --version"** devuelve el nombre de Sophos = **Sophos Anti-Virus y la versión de Sophos >= 9**

Security Center recomienda **"Resolver problemas de estado de protección de puntos de conexión en las máquinas"** si no se satisface ninguna de las siguientes comprobaciones:

- **"/opt/sophos-av/bin/savlog --maxage=7 | grep -i "Scheduled scan .\* completed" | tail -1"** , devuelve un valor

- **"/opt/sophos-av/bin/savlog --maxage=7 | grep "scan finished"** | tail -1", devuelve un valor

- **"/opt/sophos-av/bin/savdstatus --lastupdate"** devuelve lastUpdate, que debe ser <= 7 días 

- - **"/opt/sophos-av/bin/savdstatus -v"** es igual a **"On-access scanning is running"** (se ejecuta el examen en el acceso) 

- **"/opt/sophos-av/bin/savconfig get LiveProtection"** devuelve enabled (habilitado)

## <a name="troubleshoot-and-support"></a>Solución de problemas y asistencia

### <a name="troubleshoot"></a>Solución de problemas

Los registros de extensión de Microsoft Antimalware están disponibles en **%Systemdrive%\WindowsAzure\Logs\Plugins\Microsoft.Azure.Security.IaaSAntimalware(o PaaSAntimalware)\1.5.5.x(version#)\CommandExecution.log**

### <a name="support"></a>Soporte técnico

Si necesita más ayuda, póngase en contacto con los expertos de Azure en los [foros de MSDN Azure y Stack Overflow](https://azure.microsoft.com/support/forums/). También puede registrar un incidente de soporte técnico de Azure. Vaya al [sitio de soporte técnico de Azure](https://azure.microsoft.com/support/options/) y seleccione Obtener soporte. Para obtener información sobre el uso del soporte técnico, lea las [Preguntas más frecuentes de soporte técnico de Microsoft Azure](https://azure.microsoft.com/support/faq/).