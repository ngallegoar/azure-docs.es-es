---
title: 'Generación y exportación de certificados para conexiones de punto a sitio: Linux: CLI'
description: Cree un certificado raíz autofirmado, exporte la clave pública y genere los certificados de cliente mediante la CLI de Linux (strongSwan).
titleSuffix: Azure VPN Gateway
services: vpn-gateway
author: anzaman
ms.service: vpn-gateway
ms.topic: article
ms.date: 08/14/2019
ms.author: alzam
ms.openlocfilehash: a0f996ff2805da4dd5af400642eef2506c228d33
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "75779763"
---
# <a name="generate-and-export-certificates"></a>Generación y exportación de certificados

Las conexiones de punto a sitio utilizan certificados para realizar la autenticación. En este artículo, se muestra cómo crear un certificado raíz autofirmado y generar certificados de cliente con la CLI de Linux y strongSwan. Si busca otras instrucciones de certificado, vea los artículos sobre [Powershell](vpn-gateway-certificates-point-to-site.md) o [MakeCert](vpn-gateway-certificates-point-to-site-makecert.md). Para obtener información sobre cómo instalar strongSwan mediante la GUI en lugar de la CLI, vea los pasos descritos en el artículo [Configuración del cliente](point-to-site-vpn-client-configuration-azure-cert.md#install).

## <a name="install-strongswan"></a>Instalación de strongSwan

[!INCLUDE [strongSwan Install](../../includes/vpn-gateway-strongswan-install-include.md)]

## <a name="generate-and-export-certificates"></a>Generación y exportación de certificados

[!INCLUDE [strongSwan certificates](../../includes/vpn-gateway-strongswan-certificates-include.md)]

## <a name="next-steps"></a>Pasos siguientes

Continúe con la configuración de punto a sitio para [crear e instalar archivos de configuración de cliente de VPN](point-to-site-vpn-client-configuration-azure-cert.md#linuxinstallcli).
