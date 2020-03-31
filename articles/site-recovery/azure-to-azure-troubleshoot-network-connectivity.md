---
title: Solución de problemas de la recuperación ante desastres de Azure a Azure con Azure Site Recovery
description: Solución de problemas de conectividad en la recuperación ante desastres de una máquina virtual de Azure
author: sideeksh
manager: rochakm
ms.topic: how-to
ms.date: 08/05/2019
ms.openlocfilehash: b082e1aca094dcb335a7268e4c116376d756fd3b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80292022"
---
# <a name="troubleshoot-azure-to-azure-vm-network-connectivity-issues"></a>Solución de problemas de conectividad de red de máquinas virtuales de Azure en Azure

En este artículo se describen los problemas comunes relacionados con la conectividad de red al replicar y recuperar máquinas virtuales de Azure de una región a otra. Para más información sobre los requisitos de red, consulte los [requisitos de conectividad para la replicación de máquinas virtuales de Azure](azure-to-azure-about-networking.md).

Para que la replicación de Site Recovery funcione, la máquina virtual debe disponer de conectividad saliente a direcciones URL o intervalos IP específicos. Si la máquina virtual está detrás de un firewall o usa reglas de grupo de seguridad de red (NSG) para controlar la conectividad saliente, puede encontrarse alguno de estos problemas.

**URL** | **Detalles**  
--- | ---
*.blob.core.windows.net | Se requiere para que los datos se puedan escribir en la cuenta de almacenamiento de la caché en la región de origen de la máquina virtual. Si conoce todas las cuentas de almacenamiento en caché para las máquinas virtuales, puede incluir en una lista las direcciones URL de las cuentas de almacenamiento específicas permitidas (por ejemplo, cache1.blob.core.windows.net y cache2.blob.core.windows.net) en lugar de *.blob.core.windows.net
login.microsoftonline.com | Se requiere para la autorización y la autenticación de las direcciones URL del servicio Site Recovery.
*.hypervrecoverymanager.windowsazure.com | Se requiere para la comunicación del servicio Site Recovery desde la máquina virtual. Puede usar la "IP de recuperación del sitio" correspondiente si el proxy del firewall es compatible con las IP.
*.servicebus.windows.net | Se requiere para que se puedan escribir datos de supervisión y diagnóstico de Site Recovery desde la máquina virtual. Puede usar la "IP de supervisión de recuperación del sitio" correspondiente si el proxy del firewall es compatible con las IP.

## <a name="outbound-connectivity-for-site-recovery-urls-or-ip-ranges-error-code-151037-or-151072"></a>Conectividad saliente para direcciones URL o intervalos IP de Site Recovery (código de error 151037 o 151072)

## <a name="issue-1-failed-to-register-azure-virtual-machine-with-site-recovery-151195-br"></a><a name="issue-1-failed-to-register-azure-virtual-machine-with-site-recovery-151195-br"></a>Problema 1: no se pudo registrar la máquina virtual de Azure en Site Recovery (151195) </br>
- **Causa posible** </br>
  - No se puede establecer la conexión con los puntos de conexión de Site Recovery por un error de resolución de DNS.
  - Normalmente, este problema se produce al volver a establecer la protección tras una conmutación por error de la máquina virtual y no se puede acceder al servidor DNS desde la región de recuperación ante desastres (DR).

- **Resolución**
   - Si utiliza un DNS personalizado, asegúrese de que el servidor DNS es accesible desde la región de recuperación ante desastres. Para comprobar si el DNS está personalizado, vaya a la máquina virtual > red de recuperación ante desastres > servidores DNS. Intente acceder al servidor DNS desde la máquina virtual. Si no puede, intente que esté accesible conmutando por error el servidor DNS o creando la línea del sitio entre la red de recuperación ante desastres y DNS.

    ![com-error](./media/azure-to-azure-troubleshoot-errors/custom_dns.png)


## <a name="issue-2-site-recovery-configuration-failed-151196"></a>Problema 2: error de configuración de Site Recovery (151196)

> [!NOTE]
> Si las máquinas virtuales se encuentran detrás de un equilibrador de carga interno **estándar**, no tendría acceso a direcciones IP de O365, es decir, login.microsoftonline.com, de forma predeterminada. Cambie a un tipo de equilibrador de carga interno **básico** o cree acceso de salida como se ha mencionado en el [artículo](https://aka.ms/lboutboundrulescli).

- **Causa posible** </br>
  - No se puede establecer conexión con los puntos de conexión de autenticación e identidad IP4 de Office 365.

- **Resolución**
  - Azure Site Recovery debe tener acceso a los intervalos de direcciones IP de Office 365 para la autenticación.
    Si utiliza un proxy que actúe como firewall o reglas de grupos de seguridad de red (NSG) de Azure para controlar la conectividad de salida de la red en la máquina virtual, no olvide permitir la comunicación con los intervalos de IP de O365. Cree una regla de grupos de seguridad de red basada en la [etiqueta de servicio de Azure Active Directory (Azure AD)](../virtual-network/security-overview.md#service-tags) para permitir el acceso a todas las direcciones IP correspondientes a Azure AD.
      - Si se agregan en el futuro nuevas direcciones a Azure AD, deberá crear nuevas reglas de NSG.

### <a name="example-nsg-configuration"></a>Configuración de NSG de ejemplo

En este ejemplo se muestra cómo configurar reglas de NSG para la replicación de una máquina virtual.

- Si usa reglas de NSG para controlar la conectividad de salida, utilice reglas para permitir el tráfico HTTPS de salida al puerto 443 para todos los intervalos de direcciones IP necesarios.
- En el ejemplo se supone que la ubicación de origen de la máquina virtual es "Este de EE. UU." y la ubicación de destino es "Centro de EE. UU".

### <a name="nsg-rules---east-us"></a>Reglas de NSG: este de EE. UU.

1. Cree una regla de seguridad HTTPS (443) de salida para "Storage.EastUS" en el NSG tal y como se muestra en la captura de pantalla siguiente.

      ![storage-tag](./media/azure-to-azure-about-networking/storage-tag.png)

2. Cree una regla de seguridad HTTPS (443) de salida para "AzureActiveDirectory" en el NSG tal y como se muestra en la captura de pantalla siguiente.

      ![aad-tag](./media/azure-to-azure-about-networking/aad-tag.png)

3. Cree reglas HTTPS (443) de salida para las direcciones IP de Site Recovery que corresponden a la ubicación de destino:

   **Ubicación** | **Dirección IP de Site Recovery** |  **Dirección IP de supervisión de Site Recovery**
    --- | --- | ---
   Centro de EE. UU. | 40.69.144.231 | 52.165.34.144

### <a name="nsg-rules---central-us"></a>Reglas de NSG: centro de EE. UU.

Estas reglas son necesarias para que la replicación se pueda habilitar de la región de destino a la región de origen con posterioridad a la conmutación por error:

1. Cree una regla de seguridad HTTPS (443) de salida saliente para "Storage.CentralUS" en el NSG.

2. Cree una regla de seguridad HTTPS (443) de salida para "AzureActiveDirectory" en el NSG.

3. Cree reglas HTTPS (443) de salida para las direcciones IP de Site Recovery que corresponden a la ubicación de origen:

   **Ubicación** | **Dirección IP de Site Recovery** |  **Dirección IP de supervisión de Site Recovery**
    --- | --- | ---
   Centro de EE. UU. | 13.82.88.226 | 104.45.147.24
## <a name="issue-3-site-recovery-configuration-failed-151197"></a>Problema 3: error de configuración de Site Recovery (151197)
- **Causa posible** </br>
  - No se puede establecer conexión con los puntos de conexión del servicio Azure Site Recovery.

- **Resolución**
  - Azure Site Recovery necesita tener acceso a [intervalos de IP de Site Recovery](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-about-networking#outbound-connectivity-using-service-tags) en función de la región. Asegúrese de que los intervalos de IP están accesibles desde la máquina virtual.


## <a name="issue-4-a2a-replication-failed-when-the-network-traffic-goes-through-on-premises-proxy-server-151072"></a>Problema 4: error de replicación A2A cuando el tráfico de red pasa por el servidor proxy local (151072)
- **Causa posible** </br>
  - La configuración de proxy personalizada no es válida y el agente de Mobility Service de Azure Site Recovery no detectó automáticamente la configuración de proxy de Internet Explorer.


- **Resolución**
  1. El agente de Mobility Service detecta la configuración de proxy de Internet Explorer en Windows y /etc/environment en Linux.
  2. Si prefiere configurar el proxy solo para Mobility Service de Azure Site Recovery, puede proporcionar los detalles de este en ProxyInfo.conf en:</br>
     - ``/usr/local/InMage/config/`` en ***Linux***
     - ``C:\ProgramData\Microsoft Azure Site Recovery\Config`` en ***Windows***
  3. ProxyInfo.conf debe tener la configuración de proxy en el siguiente formato INI.</br>
                *[proxy]*</br>
                *Address=http://1.2.3.4*</br>
                *Port=567*</br>
  4. El agente de Mobility Service de Azure Site Recovery solo admite ***servidores proxy no autenticados***.

### <a name="fix-the-problem"></a>Corrección del problema
Para permitir [las direcciones URL necesarias](azure-to-azure-about-networking.md#outbound-connectivity-for-urls) o los [intervalos IP necesarios](azure-to-azure-about-networking.md#outbound-connectivity-using-service-tags), siga los pasos del [documento de instrucciones para redes](site-recovery-azure-to-azure-networking-guidance.md).


## <a name="next-steps"></a>Pasos siguientes
[Replicación de máquinas virtuales de Azure](site-recovery-replicate-azure-to-azure.md)
