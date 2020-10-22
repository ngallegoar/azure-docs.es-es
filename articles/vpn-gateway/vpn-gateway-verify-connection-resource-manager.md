---
title: 'Azure VPN Gateway: Comprobación de una conexión de puerta de enlace'
description: En este artículo se explica cómo verificar una conexión de VPN Gateway de red virtual.
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: how-to
ms.date: 10/19/2020
ms.author: cherylmc
ms.custom: devx-track-azurecli
ms.openlocfilehash: fef9eb49b10008c86ee044a199ae69a43585f4f3
ms.sourcegitcommit: 8d8deb9a406165de5050522681b782fb2917762d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/20/2020
ms.locfileid: "92217990"
---
# <a name="verify-a-vpn-gateway-connection"></a>Verificación de una conexión de VPN Gateway

En este artículo se muestra cómo comprobar una conexión de puerta de enlace de VPN para los modelos de implementación clásico y de Resource Manager.

## <a name="azure-portal"></a>Portal de Azure

[!INCLUDE [Azure portal](../../includes/vpn-gateway-verify-connection-portal-rm-include.md)]

## <a name="powershell"></a>PowerShell

Para comprobar el modelo de implementación de Resource Manager usado en una conexión de puerta de enlace de VPN con PowerShell, instale la versión más reciente de los [cmdlets de PowerShell de Azure Resource Manager](/powershell/azure/).

[!INCLUDE [PowerShell](../../includes/vpn-gateway-verify-connection-ps-rm-include.md)]

## <a name="azure-cli"></a>Azure CLI

Para comprobar el modelo de implementación de Resource Manager usado en una conexión de puerta de enlace de VPN con la CLI de Azure, instale la versión más reciente de los [comandos de CLI](https://docs.microsoft.com/cli/azure/install-azure-cli) (2.0 o posterior).

[!INCLUDE [CLI](../../includes/vpn-gateway-verify-connection-cli-rm-include.md)]

## <a name="azure-portal-classic"></a>Azure Portal (clásico)

[!INCLUDE [Azure portal](../../includes/vpn-gateway-verify-connection-azureportal-classic-include.md)]

## <a name="powershell-classic"></a>PowerShell (clásico)

Para comprobar el modelo de implementación clásico usado en la conexión de puerta de enlace de VPN con PowerShell, instale las versiones más reciente de los cmdlets de Azure PowerShell. Asegúrese de descargar e instalar el módulo de [Service Management](/powershell/azure/servicemanagement/install-azure-ps?#azure-service-management-cmdlets). Use "Add-AzureAccount" para iniciar sesión en el modelo de implementación clásica.

[!INCLUDE [Classic PowerShell](../../includes/vpn-gateway-verify-connection-ps-classic-include.md)]

## <a name="next-steps"></a>Pasos siguientes

* Puede agregar máquinas virtuales a las redes virtuales. Consulte [Creación de una máquina virtual que ejecuta Windows en el Portal de Azure](../virtual-machines/windows/quick-create-portal.md) para ver los pasos.
