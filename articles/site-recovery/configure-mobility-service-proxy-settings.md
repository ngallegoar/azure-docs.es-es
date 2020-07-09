---
title: Configuración del proxy del servicio Mobility para Azure en la recuperación ante desastres de Azure | Microsoft Docs
description: Proporciona detalles sobre cómo configurar el servicio Mobility cuando los clientes usan un proxy en su entorno de origen.
services: site-recovery
author: sideeksh
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 03/18/2020
ms.author: sideeksh
ms.openlocfilehash: 429ffcab147142ae2e96de13b7c9e1e5ee1ac7ba
ms.sourcegitcommit: e995f770a0182a93c4e664e60c025e5ba66d6a45
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/08/2020
ms.locfileid: "86133218"
---
# <a name="configure-mobility-service-proxy-settings-for-azure-to-azure-disaster-recovery"></a>Configuración del proxy del servicio Mobility para Azure en la recuperación ante desastres de Azure

En este artículo se proporciona una guía para personalizar las configuraciones de red en la máquina virtual de Azure de destino cuando se replican y recuperan máquinas virtuales de Azure de una región a otra mediante [Azure Site Recovery](site-recovery-overview.md).

El fin de este documento es proporcionar los pasos necesarios para configurar los valores de proxy del servicio Mobility de Azure Site Recovery en el escenario de recuperación ante desastres de Azure a Azure. 

Los servidores proxy son puertas de enlace de red que permiten o deniegan conexiones de red a puntos de conexión. Normalmente, un proxy es una máquina que está fuera de la máquina cliente y que intenta acceder a los puntos de conexión de la red. Las listas de omisión permiten al cliente realizar conexiones directamente a los puntos de conexión sin atravesar el proxy. Los administradores de red pueden establecer opcionalmente un nombre de usuario y una contraseña para un proxy, con el fin de que los clientes autenticados puedan usar el proxy. 

## <a name="before-you-start"></a>Antes de comenzar

Obtenga información sobre cómo Site Recovery proporciona recuperación ante desastres para [este escenario](azure-to-azure-architecture.md).
Conozca la [guía de redes](azure-to-azure-about-networking.md) cuando vaya a replicar y recuperar máquinas virtuales de Azure de una región a otra mediante [Azure Site Recovery](site-recovery-overview.md).
Asegúrese de que el proxy está configurado correctamente en función de las necesidades de su organización.

## <a name="configure-the-mobility-service"></a>Configuración del servicio Mobility

El servicio Mobility solo admite servidores proxy no autenticados. Proporciona dos maneras de especificar los detalles del proxy para la comunicación con los puntos de conexión de Site Recovery. 

### <a name="method-1-auto-detection"></a>Método 1: detección automática

El servicio Mobility detecta automáticamente la configuración del proxy desde la configuración del entorno o de Internet Explorer (solo Windows) durante la habilitación de la replicación. 

- SO Windows: durante la habilitación de la replicación, el servicio Mobility detecta la configuración del proxy tal como está configurada en Internet Explorer para el usuario Sistema local. Para configurar un proxy para la cuenta Sistema local, el administrador puede usar psexec para iniciar un símbolo del sistema y, después, Internet Explorer. 
- SO Windows: Los valores del proxy se configuran como las variables de entorno http_proxy y no_proxy. 
- SO Linux: Los valores del proxy se configuran en/etc/profile o /etc/environment como variables de entorno http_proxy y no_proxy. 
- La configuración del proxy detectada automáticamente se guarda en el archivo de configuración del proxy del servicio Mobility, ProxyInfo.conf 
- Ubicación predeterminada de ProxyInfo.conf 
    - Windows: C:\ProgramData\Microsoft Azure Site Recovery\Config\ProxyInfo.conf 
    - Linux: /usr/local/InMage/config/ProxyInfo.conf


### <a name="method-2-provide-custom-application-proxy-settings"></a>Método 2: especificar la configuración personalizada del proxy de aplicación

En este caso, el cliente proporciona la configuración personalizada del proxy de aplicación en el archivo de configuración del servicio Mobility ProxyInfo.conf. Este método permite a los clientes proporcionar el proxy solo para el servicio Mobility o un proxy diferente para el servicio Mobility de Azure Site Recovery que es distinto del proxy del resto de las aplicaciones del equipo (o ningún proxy).

## <a name="proxy-template"></a>Plantilla de proxy
ProxyInfo.conf contiene la siguiente plantilla [proxy] Address=http://1.2.3.4 Port=5678 BypassList=hypervrecoverymanager.windowsazure.com,login.microsoftonline.com,blob.core.windows.net. BypassList no admite caracteres comodín como "*. windows.net", pero escribir windows.net es suficiente para la omisión. 

## <a name="next-steps"></a>Pasos siguientes:
- Obtenga más información en las [instrucciones sobre redes](./azure-to-azure-about-networking.md) para replicar máquinas virtuales de Azure.
- Implemente la recuperación ante desastres mediante [la replicación de máquinas virtuales de Azure](./azure-to-azure-quickstart.md).
