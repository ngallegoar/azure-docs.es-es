---
title: Controles de seguridad para Azure Relay
description: En este artículo se proporciona una lista de comprobación de los controles de seguridad integrados para evaluar Azure Relay.
ms.topic: conceptual
ms.date: 06/23/2020
ms.openlocfilehash: 64abee031bb20e2bdb10bf1cc3cd77e135713550
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85316613"
---
# <a name="security-controls-for-azure-relay"></a>Controles de seguridad para Azure Relay

En este artículo se explican los controles de seguridad integrados en Azure Relay.

[!INCLUDE [Security controls Header](../../includes/security-controls-header.md)]

## <a name="network"></a>Red

| Control de seguridad | Sí/No | Notas | Documentación |
|---|---|--|--|
| Compatibilidad con punto de conexión de servicio| No |  |   |
| Compatibilidad con el aislamiento de red y los firewalls| No |  |   |
| Compatibilidad con la tunelización forzada| N/D | La retransmisión es el túnel de TLS  |   |

## <a name="monitoring--logging"></a>Supervisión y registro

| Control de seguridad | Sí/No | Notas| Documentación |
|---|---|--|--|
| Compatibilidad con la supervisión de Azure (Log Analytics, Application Insights, etc.)| Sí | |   |
| Registro y auditoría del plano de administración y de control| Sí | Mediante [Azure Resource Manager](../azure-resource-manager/index.yml). |   |
| Registro y auditoría del plano de datos| Sí | Conexión correcta o con errores, y errores y registros.  |   |

## <a name="identity"></a>Identidad

| Control de seguridad | Sí/No | Notas| Documentación |
|---|---|--|--|
| Authentication| Sí | Mediante SAS. | [Autenticación y autorización en Azure Relay](relay-authentication-and-authorization.md) |
| Authorization|  Sí | Mediante SAS. | [Autenticación y autorización en Azure Relay](relay-authentication-and-authorization.md) |

## <a name="data-protection"></a>Protección de los datos

| Control de seguridad | Sí/No | Notas | Documentación |
|---|---|--|--|
| Cifrado del lado servidor en reposo: Claves administradas por Microsoft |  N/D | La retransmisión es un socket web y no conserva los datos. |   |
| Cifrado del lado servidor en reposo: claves administradas por el cliente (BYOK) | No | Usa solo los certificados TLS de Microsoft.  |   |
| Cifrado de nivel de columna (Azure Data Services)| N/D | |   |
| Cifrado en tránsito (por ejemplo, cifrado de ExpressRoute, cifrado en la red virtual y cifrado de red virtual a red virtual)| Sí | El servicio requiere TLS. |   |
| Llamadas a API cifradas| Sí | HTTPS. |


## <a name="configuration-management"></a>Administración de configuración

| Control de seguridad | Sí/No | Notas| Documentación |
|---|---|--|--|
| Compatibilidad con la administración de configuración (control de versiones de configuración, etc.)| Sí | Mediante [Azure Resource Manager](../azure-resource-manager/index.yml).|   |

## <a name="next-steps"></a>Pasos siguientes

- Más información sobre los [controles de seguridad integrados en los servicios de Azure](../security/fundamentals/security-controls.md).