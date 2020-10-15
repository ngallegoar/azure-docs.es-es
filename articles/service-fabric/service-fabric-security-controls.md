---
title: Controles de seguridad para Azure Service Fabric
description: Obtenga información sobre los controles de seguridad para Azure Service Fabric. Incluye una lista de comprobación de controles de seguridad integrados.
author: msmbaldwin
ms.topic: conceptual
ms.date: 09/04/2019
ms.author: mbaldwin
ms.openlocfilehash: a8bb49e20ec5812a4882966c6918cf2bd59f36a0
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "75645436"
---
# <a name="security-controls-for-azure-service-fabric"></a>Controles de seguridad para Azure Service Fabric

En este artículo se explican los controles de seguridad integrados en Azure Service Fabric. 

[!INCLUDE [Security controls Header](../../includes/security-controls-header.md)]

## <a name="network"></a>Red

| Control de seguridad | Sí/No | Notas |
|---|---|--|
| Compatibilidad con punto de conexión de servicio| Sí |  |
| Compatibilidad con la inserción de redes virtuales| Sí |  |
| Compatibilidad con el aislamiento de red y los firewalls| Sí | Usando grupos de seguridad de red (NSG). |
| Compatibilidad con la tunelización forzada| Sí | Las redes de Azure proporcionan la tunelización forzada. |

## <a name="monitoring--logging"></a>Supervisión y registro

| Control de seguridad | Sí/No | Notas|
|---|---|--|
| Compatibilidad con la supervisión de Azure (Log Analytics, Application Insights, etc.)| Sí | Con la compatibilidad de supervisión de Azure y la compatibilidad de las soluciones de terceros. |
| Registro y auditoría del plano de administración y de control| Sí | Todas las operaciones del plano de control se ejecutan a través de procesos de auditoría y aprobaciones. |
| Registro y auditoría del plano de datos| N/D | El cliente es propietario del clúster.  |

## <a name="identity"></a>Identidad

| Control de seguridad | Sí/No | Notas|
|---|---|--|
| Authentication| Sí | La autenticación se realiza a través de Azure Active Directory. |
| Authorization| Sí | Administración de identidad y acceso (IAM) para llamadas a través de SFRP. Las llamadas directamente al punto de conexión del clúster son compatibles con dos roles: usuario y administrador. El cliente puede asignar las API a cualquiera de estos roles. |

## <a name="data-protection"></a>Protección de los datos

| Control de seguridad | Sí/No | Notas |
|---|---|--|
| Cifrado del lado servidor en reposo: Claves administradas por Microsoft | Sí | El cliente posee el clúster y el conjunto de escalado de máquinas virtuales en el que se basa el clúster. El cifrado de discos de Azure se puede habilitar en el conjunto de escalado de máquinas virtuales. |
| Cifrado del lado servidor en reposo: claves administradas por el cliente (BYOK) | Sí | El cliente posee el clúster y el conjunto de escalado de máquinas virtuales en el que se basa el clúster. El cifrado de discos de Azure se puede habilitar en el conjunto de escalado de máquinas virtuales. |
| Cifrado de nivel de columna (Azure Data Services)| N/D |  |
| Cifrado en tránsito (por ejemplo, cifrado de ExpressRoute, cifrado en la red virtual y cifrado de red virtual a red virtual)| Sí |  |
| Llamadas a API cifradas| Sí | Las llamadas de API de Service Fabric se hacen a través de Azure Resource Manager. Se requiere un token web de JSON (JWT) válido. |

## <a name="configuration-management"></a>Administración de configuración

| Control de seguridad | Sí/No | Notas|
|---|---|--|
| Compatibilidad con la administración de configuración (control de versiones de configuración, etc.)| Sí | |

## <a name="next-steps"></a>Pasos siguientes

- Más información sobre los [controles de seguridad integrados en los servicios de Azure](../security/fundamentals/security-controls.md).