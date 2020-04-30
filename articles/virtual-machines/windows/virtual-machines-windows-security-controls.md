---
title: Controles de seguridad para Windows Virtual Machines en Azure
description: Lista de comprobación de los controles de seguridad que se usan para evaluar Windows Virtual Machines en Azure
ms.service: virtual-machines
ms.subservice: security
author: msmbaldwin
manager: barbkess
ms.topic: conceptual
ms.date: 09/05/2019
ms.author: mbaldwin
ms.openlocfilehash: ac1ed9ac25d65d0391175fc6d43b48048da74926
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2020
ms.locfileid: "82101593"
---
# <a name="security-controls-for-windows-virtual-machines"></a>Controles de seguridad para Windows Virtual Machines

En este artículo, se documentan los controles de seguridad integrados en Windows Virtual Machines.

[!INCLUDE [Security controls header](../../../includes/security-controls-header.md)]

## <a name="network"></a>Red

| Control de seguridad | Sí/No | Notas |
|---|---|--|
| Compatibilidad con punto de conexión de servicio| Sí | |
| Compatibilidad con la inserción de redes virtuales| Sí | |
| Compatibilidad con el aislamiento de red y los firewalls| Sí |  |
| Compatibilidad con la tunelización forzada| Sí | Consulte [Configuración de la tunelización forzada mediante el modelo de implementación de Azure Resource Manager](/azure/vpn-gateway/vpn-gateway-forced-tunneling-rm). |

## <a name="monitoring--logging"></a>Supervisión y registro

| Control de seguridad | Sí/No | Notas|
|---|---|--|
| Compatibilidad con la supervisión de Azure (Log Analytics, Application Insights, etc.)| Sí | [Supervisión y actualización de una máquina virtual Windows en Azure](tutorial-monitoring.md). |
| Registro y auditoría del plano de administración y de control| Sí |  |
| Registro y auditoría del plano de datos | No |  |

## <a name="identity"></a>Identidad

| Control de seguridad | Sí/No | Notas|
|---|---|--|
| Authentication| Sí |  |
| Authorization| Sí |  |

## <a name="data-protection"></a>Protección de los datos

| Control de seguridad | Sí/No | Notas |
|---|---|--|
| Cifrado del lado servidor en reposo: Claves administradas por Microsoft | Sí | Consulte [Cifrado de discos virtuales en una máquina virtual con Windows](/azure/virtual-machines/windows/disk-encryption-overview). |
| Cifrado en tránsito (por ejemplo, cifrado de ExpressRoute, cifrado en la red virtual y cifrado de red virtual a red virtual)| Sí | Azure Virtual Machines admite [ExpressRoute](/azure/expressroute) y el cifrado de red virtual. Consulte [Cifrado en tránsito en máquinas virtuales](/azure/security/security-azure-encryption-overview#in-transit-encryption-in-vms). |
| Cifrado del lado servidor en reposo: claves administradas por el cliente (BYOK) | Sí | Las claves administradas por el cliente es un escenario de cifrado admitidos de Azure; consulte [Introducción al cifrado de Azure](/azure/security/security-azure-encryption-overview#in-transit-encryption-in-vms).|
| Cifrado de nivel de columna (Azure Data Services)| N/D | |
| Llamadas a API cifradas| Sí | A través de HTTPS y TLS. |



## <a name="configuration-management"></a>Administración de configuración

| Control de seguridad | Sí/No | Notas|
|---|---|--|
| Compatibilidad con la administración de configuración (control de versiones de configuración, etc.)| Sí |  | 

## <a name="next-steps"></a>Pasos siguientes

- Más información sobre los [controles de seguridad integrados en los servicios de Azure](../../security/fundamentals/security-controls.md).
