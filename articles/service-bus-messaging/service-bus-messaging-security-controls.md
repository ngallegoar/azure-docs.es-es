---
title: Controles de seguridad para la mensajería de Azure Service Bus
description: Lista de comprobación de los controles de seguridad para evaluar la mensajería de Azure Service Bus
ms.topic: conceptual
ms.date: 06/23/2020
ms.openlocfilehash: 3130150a227076befae3f58f65e00a36578b68d5
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "85341637"
---
# <a name="security-controls-for-azure-service-bus-messaging"></a>Controles de seguridad para la mensajería de Azure Service Bus

En este artículo se explican los controles de seguridad integrados en la mensajería de Azure Service Bus.

[!INCLUDE [Security controls Header](../../includes/security-controls-header.md)]

## <a name="network"></a>Red

| Control de seguridad | Sí/No | Notas | Documentación |
|---|---|--|--|
| Compatibilidad con punto de conexión de servicio| Sí (solo el nivel Premium) | Se admiten los puntos de conexión de servicio de red virtual solo para el [nivel Premium de Service Bus](service-bus-premium-messaging.md). |  |
| Compatibilidad con la inserción de redes virtuales| No | |  |
| Compatibilidad con el aislamiento de red y los firewalls| Sí (solo el nivel Premium) |  |  |
| Compatibilidad con la tunelización forzada| No |  |  |

## <a name="monitoring--logging"></a>Supervisión y registro

| Control de seguridad | Sí/No | Notas| Documentación |
|---|---|--|--|
| Compatibilidad con la supervisión de Azure (Log Analytics, Application Insights, etc.)| Sí | Compatible mediante [Azure Monitor y las alertas](service-bus-metrics-azure-monitor.md). |  |
| Registro y auditoría del plano de administración y de control| Sí | Hay registros de operaciones disponibles.  | [Registros de diagnóstico de Service Bus](service-bus-diagnostic-logs.md) |
| Registro y auditoría del plano de datos| No |  |

## <a name="identity"></a>Identidad

| Control de seguridad | Sí/No | Notas| Documentación |
|---|---|--|--|
| Authentication| Sí | Administrado mediante [Azure Active Directory Managed Service Identity](service-bus-managed-service-identity.md).| [Autenticación y autorización de Service Bus](service-bus-authentication-and-authorization.md). |
| Authorization| Sí | Admite la autorización a través de [RBAC](authenticate-application.md) y el token de SAS. | [Autenticación y autorización de Service Bus](service-bus-authentication-and-authorization.md). |

## <a name="data-protection"></a>Protección de los datos

| Control de seguridad | Sí/No | Notas | Documentación |
|---|---|--|--|
| Cifrado del lado servidor en reposo: Claves administradas por Microsoft |  Sí, para el cifrado en reposo en el servidor, de forma predeterminada. |  |  |
| Cifrado del lado servidor en reposo: claves administradas por el cliente (BYOK) | Sí. | Se puede usar una clave administrada por el cliente en Azure Key Vault para cifrar los datos en el espacio de nombres de Service Bus en reposo. | [Configuración de claves administradas por el cliente para cifrar datos en reposo de Azure Service Bus mediante Azure Portal](configure-customer-managed-key.md)  |
| Cifrado de nivel de columna (Azure Data Services)| N/D | |   |
| Cifrado en tránsito (por ejemplo, cifrado de ExpressRoute, cifrado en la red virtual y cifrado de red virtual a red virtual)| Sí | Compatible con el mecanismo estándar de HTTPS/TLS. |   |
| Llamadas a API cifradas| Sí | Las llamadas de API se realizan mediante [Azure Resource Manager](../azure-resource-manager/index.yml) y HTTPS. |   |

## <a name="configuration-management"></a>Administración de configuración

| Control de seguridad | Sí/No | Notas| Documentación |
|---|---|--|--|
| Compatibilidad con la administración de configuración (control de versiones de configuración, etc.)| Sí | Compatible con el control de versiones del proveedor de recursos mediante la [API de Azure Resource Manager](/rest/api/resources/).|   |

## <a name="next-steps"></a>Pasos siguientes

- Más información sobre los [controles de seguridad integrados en los servicios de Azure](../security/fundamentals/security-controls.md).
