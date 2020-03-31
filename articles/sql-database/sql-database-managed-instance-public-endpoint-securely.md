---
title: Protección de puntos de conexión públicos de una instancia administrada
description: Use puntos de conexión públicos de forma segura en Azure con una instancia administrada
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: ''
ms.topic: conceptual
author: srdan-bozovic-msft
ms.author: srbozovi
ms.reviewer: vanto, carlrab
ms.date: 05/08/2019
ms.openlocfilehash: 6dfeab3530445f8f9a102f47039d15b04fdf134a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "73821738"
---
# <a name="use-an-azure-sql-database-managed-instance-securely-with-public-endpoints"></a>Utilice una instancia administrada de Azure SQL Database de forma segura con puntos de conexión públicos

Las instancias administradas de Azure SQL Database pueden proporcionar conectividad de usuario a través de [puntos de conexión públicos](../virtual-network/virtual-network-service-endpoints-overview.md). En este artículo se explica cómo conseguir que esta configuración sea más segura.

## <a name="scenarios"></a>Escenarios

Una instancia administrada de SQL Database proporciona un punto de conexión privado para permitir la conectividad desde dentro de su red virtual. La opción predeterminada es ofrecer un nivel máximo de aislamiento. Sin embargo, hay escenarios en que es necesario proporcionar una conexión de punto de conexión público:

- La instancia administrada debe integrarse con ofertas de plataforma como servicio (PaaS) solo para múltiples inquilinos.
- Necesita un rendimiento de intercambio de datos mayor que el que se obtiene al utilizar una VPN.
- Las directivas de empresa prohíben el uso de PaaS dentro de las redes corporativas.

## <a name="deploy-a-managed-instance-for-public-endpoint-access"></a>Implementación de una instancia administrada para el acceso al punto de conexión público

Aunque no es obligatorio, el modelo de implementación habitual para una instancia administrada con acceso al punto de conexión público consiste en crear la instancia en una red virtual aislada dedicada. En esta configuración, la red virtual se usa solo para aislar los clústeres virtuales. No importa si el espacio de direcciones IP de la instancia administrada se superpone al espacio de direcciones IP de la red corporativa.

## <a name="secure-data-in-motion"></a>Protección de los datos en movimiento

Si el controlador cliente admite el cifrado, siempre se cifra el tráfico de datos de la instancia administrada. Los datos enviados entre la instancia administrada y otras máquinas virtuales o servicios de Azure nunca abandonan la red troncal de Azure. Si hay una conexión entre la instancia administrada y una red local, se recomienda usar Azure ExpressRoute con emparejamiento de Microsoft. ExpressRoute lo ayuda a evitar mover datos a través de la red pública de Internet. Para la conectividad privada de la instancia administrada, solo se puede usar el emparejamiento privado.

## <a name="lock-down-inbound-and-outbound-connectivity"></a>Bloquee la conectividad entrante y saliente

En el diagrama siguiente se muestran las configuraciones de seguridad recomendadas:

![Configuraciones de seguridad para bloquear la conectividad entrante y saliente](media/sql-database-managed-instance-public-endpoint-securely/managed-instance-vnet.png)

Una instancia administrada tiene una [dirección de punto de conexión público dedicada](sql-database-managed-instance-find-management-endpoint-ip-address.md). En el firewall de salida del lado cliente y en las reglas del grupo de seguridad de red, establezca esta dirección IP de punto de conexión público para limitar la conectividad saliente.

Para asegurarse de que el tráfico a la instancia administrada procede de orígenes de confianza, se recomienda conectarse desde orígenes con direcciones IP conocidas. Utilice un grupo de seguridad de red para limitar el acceso al punto de conexión público de la instancia administrada a través del puerto 3342.

Cuando los clientes deban iniciar una conexión desde una red local, asegúrese de que la dirección de origen se convierta en un conjunto conocido de direcciones IP. Si no puede hacerlo (por ejemplo, un caso típico sería un grupo de trabajadores móviles), se recomienda usar [conexiones VPN de punto a sitio y un punto de conexión privado](sql-database-managed-instance-configure-p2s.md).

Si las conexiones se inician desde Azure, se recomienda que el tráfico provenga de una [dirección IP virtual](../virtual-network/virtual-networks-reserved-public-ip.md) asignada conocida (por ejemplo, una máquina virtual). Para facilitar la administración de direcciones IP virtuales (VIP), se recomienda utilizar [prefijos de direcciones IP públicas](../virtual-network/public-ip-address-prefix.md).

## <a name="next-steps"></a>Pasos siguientes

- Obtenga información sobre cómo configurar el punto de conexión público para administrar instancias: [Configuración de un punto de conexión público](sql-database-managed-instance-public-endpoint-configure.md)
