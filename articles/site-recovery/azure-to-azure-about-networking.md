---
title: Acerca de las redes en la recuperación ante desastres de máquinas virtuales de Azure con Azure Site Recovery
description: Proporciona información general de las redes para la replicación de máquinas virtuales de Azure mediante Azure Site Recovery.
services: site-recovery
author: sujayt
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 3/13/2020
ms.author: sutalasi
ms.openlocfilehash: f9e2d82130ae188d269847d0e0236ea0e33d00dc
ms.sourcegitcommit: e995f770a0182a93c4e664e60c025e5ba66d6a45
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/08/2020
ms.locfileid: "86131382"
---
# <a name="about-networking-in-azure-vm-disaster-recovery"></a>Acerca de las redes para la recuperación ante desastres de máquinas virtuales de Azure



En este artículo se proporciona la guía de redes cuando se van a replicar y recuperar máquinas virtuales de Azure de una región a otra, mediante [Azure Site Recovery](site-recovery-overview.md).

## <a name="before-you-start"></a>Antes de comenzar

Obtenga información sobre cómo Site Recovery proporciona recuperación ante desastres para [este escenario](azure-to-azure-architecture.md).

## <a name="typical-network-infrastructure"></a>Infraestructura de red típica

El siguiente diagrama representa el entorno de Azure típico para aplicaciones que se ejecutan en máquinas virtuales de Azure:

![Entorno de cliente](./media/site-recovery-azure-to-azure-architecture/source-environment.png)

Si usa Azure ExpressRoute o una conexión VPN desde su red local a Azure, el entorno tendrá este aspecto:

![Entorno de cliente](./media/site-recovery-azure-to-azure-architecture/source-environment-expressroute.png)

Normalmente, los clientes protegen sus redes mediante firewalls y grupos de seguridad de red (NSG). Los firewalls usan la inclusión en lista blanca basada en direcciones URL o IP para controlar la conectividad de red. Los NSG proporcionan reglas que usan intervalos de direcciones IP para controlar la conectividad de red.

>[!IMPORTANT]
> Site Recovery no admite el uso de un proxy autenticado para controlar la conectividad de red y la replicación no se podrá habilitar.


## <a name="outbound-connectivity-for-urls"></a>Conectividad de salida para las direcciones URL

Si usa un proxy de firewall basado en la dirección URL para controlar la conectividad de salida, admita estas direcciones URL de Site Recovery:


**URL** | **Detalles**
--- | ---
*.blob.core.windows.net | Se requiere para que los datos se puedan escribir en la cuenta de almacenamiento de la caché en la región de origen de la máquina virtual. Si conoce todas las cuentas de almacenamiento en caché de las máquinas virtuales, puede permitir el acceso a las direcciones URL de las cuentas de almacenamiento específicas (por ejemplo, cache1.blob.core.windows.net y cache2.blob.core.windows.net) en lugar de *.blob.core.windows.net
login.microsoftonline.com | Se requiere para la autorización y la autenticación de las direcciones URL del servicio Site Recovery.
*.hypervrecoverymanager.windowsazure.com | Se requiere para la comunicación del servicio Site Recovery desde la máquina virtual.
*.servicebus.windows.net | Se requiere para que se puedan escribir datos de supervisión y diagnóstico de Site Recovery desde la máquina virtual.
*.vault.azure.net | Permite el acceso para habilitar la replicación de máquinas virtuales habilitadas para ADE a través del portal.
*.automation.ext.azure.com | Permite habilitar la actualización automática del agente de movilidad para un elemento replicado a través del portal.

## <a name="outbound-connectivity-using-service-tags"></a>Conectividad saliente mediante etiquetas de servicio

Si utiliza un grupo de seguridad de red para controlar la conectividad de salida, deben permitirse estas etiquetas de servicio.

- Para las cuentas de almacenamiento en la región de origen:
    - Cree una regla de NSG basada en la [etiqueta del servicio Storage](../virtual-network/security-overview.md#service-tags) para la región de origen.
    - Permita estas direcciones para que los datos se puedan escribir en la cuenta de almacenamiento en caché, desde la máquina virtual.
- Cree una regla de grupos de seguridad de red basada en la [etiqueta de servicio de Azure Active Directory (AAD)](../virtual-network/security-overview.md#service-tags) para permitir el acceso a todas las direcciones IP correspondientes a AAD.
- Cree una regla de NSG basada en una etiqueta de servicio EventsHub para la región de destino, lo que permite el acceso a la supervisión de Site Recovery.
- Cree una regla de NSG basada en una etiqueta de servicio AzureSiteRecovery para permitir el acceso al servicio Site Recovery en cualquier región.
- Cree una regla de NSG basada en una etiqueta de servicio de AzureKeyVault. Esto solo es necesario para habilitar la replicación de máquinas virtuales habilitadas para ADE a través del portal.
- Cree una regla de NSG basada en una etiqueta de servicio de GuestAndHybridManagement. Esto es necesario solo para habilitar la actualización automática del agente de movilidad para un elemento replicado a través del portal.
- Se recomienda crear las reglas de NSG necesarias en un grupo NSG de NSG de prueba y comprobar que no haya ningún problema antes de crear las reglas en un grupo de NSG de producción.

## <a name="example-nsg-configuration"></a>Configuración de NSG de ejemplo

En este ejemplo se muestra cómo configurar reglas de NSG para la replicación de una máquina virtual.

- Si usa reglas de NSG para controlar la conectividad de salida, utilice reglas para permitir el tráfico HTTPS de salida al puerto 443 para todos los intervalos de direcciones IP necesarios.
- En el ejemplo se supone que la ubicación de origen de la máquina virtual es "Este de EE. UU." y la ubicación de destino es "Centro de EE. UU".

### <a name="nsg-rules---east-us"></a>Reglas de NSG: este de EE. UU.

1. Cree una regla de seguridad HTTPS (443) de salida para "Storage.EastUS" en el NSG tal y como se muestra en la captura de pantalla siguiente.

      ![storage-tag](./media/azure-to-azure-about-networking/storage-tag.png)

2. Cree una regla de seguridad HTTPS (443) de salida para "AzureActiveDirectory" en el NSG tal y como se muestra en la captura de pantalla siguiente.

      ![aad-tag](./media/azure-to-azure-about-networking/aad-tag.png)

3. De forma similar a las reglas de seguridad anteriores, cree una regla de seguridad HTTPS (443) de salida para ""EventHub.CentralUS" en el NSG que corresponda a la ubicación de destino. Esto permite el acceso a la supervisión de Site Recovery.

4. Cree una regla de seguridad HTTPS (443) de salida para "AzureSiteRecovery" en el NSG. Esto permite el acceso al servicio Site Recovery en cualquier región.

### <a name="nsg-rules---central-us"></a>Reglas de NSG: centro de EE. UU.

Estas reglas son necesarias para que la replicación se pueda habilitar de la región de destino a la región de origen con posterioridad a la conmutación por error:

1. Cree una regla de seguridad HTTPS (443) de salida saliente para "Storage.CentralUS" en el NSG.

2. Cree una regla de seguridad HTTPS (443) de salida para "AzureActiveDirectory" en el NSG.

3. De forma similar a las reglas de seguridad anteriores, cree una regla de seguridad HTTPS (443) de salida para "EventHub.EastUS" en el NSG que corresponda a la ubicación de origen. Esto permite el acceso a la supervisión de Site Recovery.

4. Cree una regla de seguridad HTTPS (443) de salida para "AzureSiteRecovery" en el NSG. Esto permite el acceso al servicio Site Recovery en cualquier región.

## <a name="network-virtual-appliance-configuration"></a>Configuración de la aplicación virtual de red

Si usa aplicaciones virtuales de red (NVA) para controlar el tráfico de red saliente de las máquinas virtuales, el dispositivo podría verse limitado si todo el tráfico de replicación pasa a través de la NVA. Se recomienda crear un punto de conexión de servicio de red en la red virtual de "Storage" para que el tráfico de replicación no se dirija a la NVA.

### <a name="create-network-service-endpoint-for-storage"></a>Crear el punto de conexión de servicio de red de Storage
Puede crear un punto de conexión de servicio de red en la red virtual de "Storage" para que el tráfico de replicación no sobrepase el límite de Azure.

- Seleccione la red virtual de Azure y haga clic en "Puntos de conexión de servicio".

    ![storage-endpoint](./media/azure-to-azure-about-networking/storage-service-endpoint.png)

- Haga clic en "Agregar" y se abrirá la pestaña "Agregar extremos del servicio".
- Seleccione "Microsoft.Storage" en "Servicio", elija las subredes necesarias en el campo "Subredes" y haga clic en "Agregar".

>[!NOTE]
>No restrinja el acceso de red virtual a las cuentas de almacenamiento que usa para ASR. Debe permitir el acceso desde todas las redes.

### <a name="forced-tunneling"></a>Tunelización forzada

Puede invalidar la ruta del sistema predeterminada de Azure para el prefijo de dirección 0.0.0.0/0 con una [ruta personalizada](../virtual-network/virtual-networks-udr-overview.md#custom-routes) y desviar el tráfico de la máquina virtual a una aplicación virtual de red (NVA) local, pero esta configuración no se recomienda para la replicación de Site Recovery. Si va a usar rutas personalizadas, debe [crear un punto de conexión de servicio de red virtual](azure-to-azure-about-networking.md#create-network-service-endpoint-for-storage) en su red virtual de "Storage" para que el tráfico de replicación no salga de los límites de Azure.

## <a name="next-steps"></a>Pasos siguientes
- Comience a proteger las cargas de trabajo mediante la [replicación de máquinas virtuales de Azure](./azure-to-azure-quickstart.md).
- Más información sobre la [retención de direcciones IP](site-recovery-retain-ip-azure-vm-failover.md) en la conmutación por error de máquinas virtuales de Azure.
- Más información sobre la recuperación ante desastres de [máquinas virtuales de Azure con ExpressRoute](azure-vm-disaster-recovery-with-expressroute.md).
