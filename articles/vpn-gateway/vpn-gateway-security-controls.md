---
title: Controles de seguridad para Azure VPN Gateway
description: Lista de comprobación de los controles de seguridad para evaluar Azure VPN Gateway
services: sql-database
author: msmbaldwin
manager: rkarlin
ms.service: vpn-gateway
ms.topic: conceptual
ms.date: 09/06/2019
ms.author: mbaldwin
ms.openlocfilehash: cdf616b29a93e786ef26af83b5d3b3541f94d67c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "75972290"
---
# <a name="security-controls-for-azure-vpn-gateway"></a>Controles de seguridad para Azure VPN Gateway

En este artículo, se explican los controles de seguridad integrados en Azure VPN Gateway.

[!INCLUDE [Security controls Header](../../includes/security-controls-header.md)]

## <a name="network"></a>Red

| Control de seguridad | Sí/No | Notas |
|---|---|--|
| Compatibilidad con punto de conexión de servicio| N/D | |
| Compatibilidad con la inserción de redes virtuales| N/D | |
| Compatibilidad con el aislamiento de red y los firewalls| Sí | Las instancias de VPN Gateway son instancias de VM dedicadas para cada red virtual de cliente  |
| Compatibilidad con la tunelización forzada| Sí |  |

## <a name="monitoring--logging"></a>Supervisión y registro

| Control de seguridad | Sí/No | Notas|
|---|---|--|
| Compatibilidad con la supervisión de Azure (Log Analytics, Application Insights, etc.)| Sí | Consulte [Registros de diagnósticos de Azure Monitor/alerta](vpn-gateway-howto-setup-alerts-virtual-network-gateway-log.md) & [Métricas de Azure Monitor/alerta](vpn-gateway-howto-setup-alerts-virtual-network-gateway-metric.md).  |
| Registro y auditoría del plano de administración y de control| Sí | Registro de actividad de Azure Resource Manager. |
| Registro y auditoría del plano de datos | Sí | [Registros de diagnóstico de Azure Monitor](../azure-resource-manager/management/view-activity-logs.md) para el registro y la auditoría de la conectividad de VPN. |

## <a name="identity"></a>Identidad

| Control de seguridad | Sí/No | Notas|
|---|---|--|
| Authentication| Sí | [Azure Active Directory](../active-directory/fundamentals/active-directory-whatis.md) para administrar el servicio y configurar Azure VPN Gateway. |
| Authorization| Sí | Admitir la autorización a través de [RBAC](../role-based-access-control/overview.md). |

## <a name="data-protection"></a>Protección de los datos

| Control de seguridad | Sí/No | Notas |
|---|---|--|
| Cifrado del lado servidor en reposo: Claves administradas por Microsoft | N/D | Datos de clientes en tránsito de VPN Gateway, NO se almacenan los datos de los clientes |
| Cifrado en tránsito (por ejemplo, cifrado de ExpressRoute, cifrado en la red virtual y cifrado de red virtual a red virtual)| Sí | Paquetes de clientes cifrados de VPN Gateway entre las instancias de Azure VPN Gateway y los clientes de VPN (P2S) o los dispositivos de VPN (S2S) locales de los clientes. Las instancias de Azure VPN Gateway también admiten el cifrado entre redes virtuales. |
| Cifrado del lado servidor en reposo: claves administradas por el cliente (BYOK) | No | Las claves precompartidas especificadas por el cliente se cifran en reposo; pero aún no se integran con CMK. |
| Cifrado de nivel de columna (Azure Data Services)| N/D | |
| Llamadas a API cifradas| Sí | Mediante [Azure Resource Manager](../azure-resource-manager/index.yml) y HTTPS  |

## <a name="configuration-management"></a>Administración de configuración

| Control de seguridad | Sí/No | Notas|
|---|---|--|
| Compatibilidad con la administración de configuración (control de versiones de configuración, etc.)| Sí | Para las operaciones de administración, el estado de una configuración de Azure VPN Gateway se puede exportar como una plantilla de Azure Resource Manager y con control de versiones a lo largo del tiempo. |

## <a name="next-steps"></a>Pasos siguientes

- Más información sobre los [controles de seguridad integrados en los servicios de Azure](../security/fundamentals/security-controls.md).
