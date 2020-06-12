---
title: Comprobación de la seguridad de puerto en el firewall integrado
description: Aprenda a comprobar la protección de firewall integrado en Instancia administrada de Azure SQL.
services: sql-database
ms.service: sql-database
ms.subservice: managed-instance
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: srdan-bozovic-msft
ms.author: srbozovi
ms.reviewer: sstein, carlrab
ms.date: 12/04/2018
ms.openlocfilehash: 04a630e51a68fab8f6c60262fbd14fad2e4aef1c
ms.sourcegitcommit: 053e5e7103ab666454faf26ed51b0dfcd7661996
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/27/2020
ms.locfileid: "84031156"
---
# <a name="verify-the-azure-sql-managed-instance-built-in-firewall"></a>Comprobación del firewall integrado de Instancia administrada de Azure SQL
[!INCLUDE[appliesto-sqlmi](../includes/appliesto-sqlmi.md)]

Las [reglas de seguridad de entrada obligatorias](connectivity-architecture-overview.md#mandatory-inbound-security-rules) de Instancia administrada de Azure SQL requieren la apertura de los puertos administrados 9000, 9003, 1438, 1440 o 1452 desde **Cualquier origen** en el grupo de seguridad de red (NSG) que protege la Instancia administrada de SQL. Aunque estos puertos están abiertos en el nivel de NSG, el firewall integrado los protege en el nivel de red.

## <a name="verify-firewall"></a>Comprobar el firewall

Para comprobar estos puertos, use cualquier herramienta de escáner de seguridad para probar estos puertos. La captura de pantalla siguiente muestra cómo usar una de estas herramientas.

![Comprobación del firewall integrado](./media/management-endpoint-verify-built-in-firewall/03_verify_firewall.png)

## <a name="next-steps"></a>Pasos siguientes

Para más información acerca de Instancias administradas de SQL y conectividad, consulte [Arquitectura de conectividad de Instancia administrada de Azure SQL](connectivity-architecture-overview.md).
