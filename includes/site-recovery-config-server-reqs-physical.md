---
title: archivo de inclusión
description: archivo de inclusión
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: include
ms.date: 09/03/2018
ms.author: raynew
ms.custom: include file
ms.openlocfilehash: 3a8a7be6f437687a4de31ce8e0ac62588f64e2eb
ms.sourcegitcommit: c95e2d89a5a3cf5e2983ffcc206f056a7992df7d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/24/2020
ms.locfileid: "96016917"
---
**Requisitos del servidor de configuración o de procesos para la replicación de servidores físicos**

**Componente** | **Requisito** 
--- | ---
**CONFIGURACIÓN DE HARDWARE** | 
Núcleos de CPU | 8 
RAM | 16 GB
Número de discos | 3, incluidos el disco del sistema operativo, el disco de memoria caché del servidor de procesos, la unidad de retención para la conmutación por recuperación 
Espacio libre en el disco (caché del servidor de procesos) | 600 GB
Espacio libre en el disco (disco de retención) | 600 GB
 | 
**CONFIGURACIÓN DE SOFTWARE** | 
Sistema operativo | Windows Server 2012 R2 <br> Windows Server 2016
Configuración regional del sistema operativo | Español (es-es)
Roles de Windows Server | No habilite estos roles: <br> - Active Directory Domain Services <br>- Internet Information Services <br> - Hyper-V 
Directivas de grupo | No habilite estas directivas de grupo: <br> - Impedir el acceso al símbolo del sistema. <br> - Impedir el acceso a herramientas de edición del Registro. <br> - Confiar en la lógica de datos adjuntos de archivos. <br> - Activar la ejecución de scripts. <br> [Más información](/previous-versions/windows/it-pro/windows-7/gg176671(v=ws.10))
IIS | - Ningún sitio web predeterminado debe existir previamente <br> - Ningún sitio web o aplicación que escuche en el puerto 443 deben existir previamente <br>- Habilitar la [autenticación anónima](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc731244(v=ws.10)) <br> - Habilitar la configuración de [FastCGI](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc753077(v=ws.10)).
Tipo de dirección IP | estática 
| 
**CONFIGURACIÓN DE ACCESO** | 
MYSQL | MySQL debe instalarse en el servidor de configuración. Lo puede instalar manualmente o Site Recovery puede instalarlo durante la implementación. Para que Site Recovery lo instale, compruebe que la máquina puede acceder a http://cdn.mysql.com/archives/mysql-5.5/mysql-5.5.37-win32.msi.
URLs | El servidor de configuración necesita acceder a estas direcciones URL (directamente o mediante proxy):<br/><br/> Azure AD: `login.microsoftonline.com`; `login.microsoftonline.us`; `*.accesscontrol.windows.net`<br/><br/> Transferencia de datos de replicación: `*.backup.windowsazure.com`; `*.backup.windowsazure.us`<br/><br/> Administración de replicación: `*.hypervrecoverymanager.windowsazure.com`; `*.hypervrecoverymanager.windowsazure.us`; `https://management.azure.com`; `*.services.visualstudio.com`<br/><br/> Acceso al almacenamiento: `*.blob.core.windows.net`; `*.blob.core.usgovcloudapi.net`<br/><br/> Sincronización de la hora: `time.nist.gov`; `time.windows.com`<br/><br/> Datos de telemetría (opcional): `dc.services.visualstudio.com`
Firewall | Las reglas de firewall basadas en direcciones IP deben permitir la comunicación con las direcciones URL de Azure. Para simplificar y limitar los intervalos de direcciones IP, se recomienda realizar el filtrado de URL.<br/><br/>**Para direcciones IP comerciales:**<br/><br/>- Permita los [intervalos de direcciones IP del centro de datos de Azure](https://www.microsoft.com/download/confirmation.aspx?id=41653) y el puerto HTTPS (443).<br/><br/> - Permita los intervalos de direcciones IP correspondientes al Oeste de EE. UU. (se usan para Access Control y para la administración de identidades).<br/><br/> - Permita los intervalos de direcciones IP para la región de Azure de su suscripción de modo que se admitan las direcciones URL necesarias para Azure Active Directory, copia de seguridad, replicación y almacenamiento.<br/><br/> **Para direcciones IP de gobierno:**<br/><br/> - Permita los intervalos de direcciones IP del centro de datos de Azure Government y el puerto HTTPS (443).<br/><br/> - Permita los intervalos de direcciones IP para todas las regiones de US Gov (Virginia, Texas, Arizona y Iowa) de modo que se admitan las direcciones URL necesarias para Azure Active Directory, copia de seguridad, replicación y almacenamiento.
Puertos | Permita el 443 (orquestación del canal de control)<br/><br/> Permita el 9443 (transporte de datos) 


**Requisitos de tamaño del servidor de proceso/configuración**

**CPU** | **Memoria** | **Disco de caché** | **Frecuencia de cambio de datos** | **Máquinas replicadas**
--- | --- | --- | --- | ---
8 vCPU<br/><br/> 2 sockets * 4 núcleos \@ 2,5 GHz | 16 GB | 300 GB | 500 GB o menos | < 100 máquinas
12 vCPU<br/><br/> 2 socks * 6 núcleos \@ 2,5 GHz | 18 GB | 600 GB | 500 GB - 1 TB | De 100 a 150 máquinas
16 vCPU<br/><br/> 2 socks * 8 núcleos \@ 2,5 GHz | 32 GB | 1 TB | 1 - 2 TB | De 150 a 200 máquinas