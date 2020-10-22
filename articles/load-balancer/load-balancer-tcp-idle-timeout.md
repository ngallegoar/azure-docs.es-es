---
title: Configuración del tiempo de espera de inactividad y restablecimiento de TCP de Load Balancer en Azure
titleSuffix: Azure Load Balancer
description: En este artículo, aprenderá a configurar Azure Load Balancer tiempo de espera de inactividad de TCP.
services: load-balancer
documentationcenter: na
author: asudbring
ms.custom: seodec18
ms.service: load-balancer
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/09/2020
ms.author: allensu
ms.openlocfilehash: 26c4c01aaf6abe6b9c9ac6daf6836d7b660ba21e
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "91649871"
---
# <a name="configure-tcp-idle-timeout-for-azure-load-balancer"></a>Configuración del tiempo de espera de inactividad de TCP de Azure Load Balancer

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Si decide instalar y usar PowerShell de forma local, para realizar los pasos de este artículo necesita la versión 5.4.1 del módulo de Azure PowerShell o cualquier versión posterior. Ejecute `Get-Module -ListAvailable Az` para buscar la versión instalada. Si necesita actualizarla, consulte [Instalación del módulo de Azure PowerShell](/powershell/azure/install-Az-ps). Si PowerShell se ejecuta localmente, también debe ejecutar `Connect-AzAccount` para crear una conexión con Azure.

Azure Load Balancer tiene una configuración de tiempo de espera de inactividad de 4 a 120 minutos. De forma predeterminada, se establece en 4 minutos. Si un período de inactividad es mayor que el valor de tiempo de espera, no hay ninguna garantía de que todavía exista la sesión TCP o HTTP entre el cliente y el servicio en la nube. Obtenga más información sobre el [tiempo de espera de inactividad de TCP](load-balancer-tcp-reset.md).

Las secciones siguientes describen cómo cambiar la configuración de tiempo de espera de inactividad para los recursos de IP pública y equilibrador de carga.


## <a name="configure-the-tcp-idle-timeout-for-your-public-ip"></a>Configuración del tiempo de espera de inactividad de TCP para la IP pública

```azurepowershell-interactive
$publicIP = Get-AzPublicIpAddress -Name MyPublicIP -ResourceGroupName MyResourceGroup
$publicIP.IdleTimeoutInMinutes = "15"
Set-AzPublicIpAddress -PublicIpAddress $publicIP
```

`IdleTimeoutInMinutes` es opcional. Si no se establece, el tiempo de espera predeterminado es de 4 minutos. El intervalo de tiempo de espera aceptable está entre 4 y 120 minutos.

## <a name="set-the-tcp-idle-timeout-on-rules"></a>Establecimiento del tiempo de espera de inactividad de TCP en reglas

Para establecer el tiempo de espera de inactividad de un equilibrador de carga, se establece el valor de "IdleTimeoutInMinutes" en la regla de carga equilibrada. Por ejemplo:

```azurepowershell-interactive
$lb = Get-AzLoadBalancer -Name "MyLoadBalancer" -ResourceGroup "MyResourceGroup"
$lb | Set-AzLoadBalancerRuleConfig -Name myLBrule -IdleTimeoutInMinutes 15
```

## <a name="next-steps"></a>Pasos siguientes

[Información general sobre el equilibrador de carga interno](load-balancer-internal-overview.md)

[Introducción a la creación de un equilibrador de carga orientado a Internet](quickstart-load-balancer-standard-public-powershell.md)

[Configuración de un modo de distribución del equilibrador de carga](load-balancer-distribution-mode.md)
