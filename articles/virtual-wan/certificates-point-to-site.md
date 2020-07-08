---
title: Generación y exportación de certificados para conexiones VPN de usuario | Azure Virtual WAN
description: Cree un certificado raíz autofirmado, exporte la clave pública y genere los certificados de cliente mediante PowerShell en Windows 10 o Windows Server 2016.
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: how-to
ms.date: 03/18/2020
ms.author: cherylmc
ms.openlocfilehash: a5361df9e9cfc9f5d299d494ed634dcaaaf2e707
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "84753618"
---
# <a name="generate-and-export-certificates-for-user-vpn-connections"></a>Generación y exportación de certificados para conexiones VPN de usuario

Las conexiones de VPN de usuario (punto a sitio) utilizan certificados para realizar la autenticación. En este artículo, se muestra cómo crear un certificado raíz autofirmado y generar certificados de cliente usando PowerShell en Windows 10 o Windows Server 2016.

Debe realizar los pasos de este artículo en un equipo que ejecute Windows 10 o Windows Server 2016. Los cmdlets de PowerShell que se usan para generar certificados forman parte del sistema operativo y no funcionan en otras versiones de Windows. El equipo con Windows 10 o Windows Server 2016 solo es necesario para generar los certificados. Una vez que se generan los certificados, puede cargarlos o instalarlos en cualquier sistema operativo cliente compatible.

[!INCLUDE [Export public key](../../includes/vpn-gateway-generate-export-certificates-include.md)]

## <a name="next-steps"></a>Pasos siguientes

Continúe con los [Pasos de Virtual WAN para la conexión VPN de usuario](virtual-wan-about.md)
