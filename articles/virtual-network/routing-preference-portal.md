---
title: 'Configuración de la preferencia de enrutamiento para una dirección IP pública: Azure Portal'
description: Información sobre cómo crear una dirección IP pública con una preferencia de enrutamiento de tráfico de Internet
services: virtual-network
documentationcenter: na
author: KumudD
manager: mtillman
ms.service: virtual-network
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 12/02/2020
ms.author: mnayak
ms.openlocfilehash: ef1e33a2e43f26dcaf794b2ed81f27d39639b52d
ms.sourcegitcommit: 5b93010b69895f146b5afd637a42f17d780c165b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/02/2020
ms.locfileid: "96533985"
---
# <a name="configure-routing-preference-for-a-public-ip-address-using-the-azure-portal"></a>Configuración de la preferencia de enrutamiento para una dirección IP pública mediante Azure Portal

En este artículo se muestra cómo configurar las [preferencias de enrutamiento](https://docs.microsoft.com/azure/virtual-network/routing-preference-overview) a través de la red de ISP (opción de **Internet**) para una dirección IP pública. Después de crear la dirección IP pública, puede asociarla a los siguientes recursos de Azure para el tráfico entrante y saliente de Internet:

* Máquina virtual
* Conjunto de escalado de máquina virtual
* Azure Kubernetes Service (AKS)
* Equilibrador de carga accesible desde Internet
* Application Gateway
* Azure Firewall

De manera predeterminada, la preferencia de enrutamiento de la dirección IP pública se establece en la red global de Microsoft para todos los servicios de Azure y puede asociarse a cualquier servicio de Azure.

> [!IMPORTANT]
> La preferencia de enrutamiento se encuentra actualmente en versión preliminar pública.
> Esta versión preliminar se ofrece sin Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas. Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) ahora.

## <a name="create-a-public-ip-address-with-a-routing-preference"></a>Creación de una dirección IP pública con preferencia de enrutamiento
1. Inicie sesión en [Azure Portal](https://preview.portal.azure.com/).
2. Seleccione **Crear un recurso**. 
3. En el cuadro de búsqueda, escriba *Dirección IP pública*.
3. En los resultados de la búsqueda, escriba **Dirección IP pública**. A continuación, en la página **Dirección IP pública** seleccione **Crear**.
3. En las opciones de **Preferencias de enrutamiento**, seleccione **Internet**.

      ![Creación de una dirección IP pública](./media/routing-preference-portal/pip-new.png)

    > [!NOTE]
    > Las direcciones IP públicas se crean con una dirección IPv4 o IPv6. Sin embargo, la preferencia de enrutamiento solo admite IPV4 actualmente.

Puede asociar la dirección IP pública creada anteriormente con una máquina virtual de [Windows](../virtual-machines/windows/overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json) o [Linux](../virtual-machines/linux/overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json). Use la sección CLI en la siguiente página del tutorial: [asociación de una dirección IP pública a una máquina virtual](associate-public-ip-address-vm.md#azure-cli) para asociar la dirección IP pública a la VM. Asimismo, puede asociar una dirección IP pública que haya creado con anterioridad con una instancia de [Azure Load Balancer](../load-balancer/load-balancer-overview.md) asignándola a la configuración de **front-end** del equilibrador de carga. La dirección IP pública actúa como dirección IP virtual (VIP) de carga equilibrada.

## <a name="next-steps"></a>Pasos siguientes
- Obtenga más información sobre las [direcciones IP públicas con preferencia de enrutamiento](routing-preference-overview.md).
- [Configuración de la preferencia de enrutamiento de una VM](tutorial-routing-preference-virtual-machine-portal.md).
- [Configuración de la preferencia de enrutamiento para una dirección IP pública mediante PowerShell](routing-preference-powershell.md).
- Obtenga más información acerca de las [direcciones IP públicas](virtual-network-ip-addresses-overview-arm.md#public-ip-addresses) en Azure.
- Obtenga más información acerca de toda la [configuración de direcciones IP públicas](virtual-network-public-ip-address.md#create-a-public-ip-address).
