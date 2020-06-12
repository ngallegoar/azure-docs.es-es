---
title: Descubrimiento de dirección IP del punto de conexión de administración
titleSuffix: Azure SQL Managed Instance
description: Aprenda a obtener la dirección IP pública del punto de conexión de administración de Instancia administrada de Azure SQL y compruebe la protección de su firewall integrado
services: sql-database
ms.service: sql-database
ms.subservice: managed-instance
ms.custom: sqldbrb=1
ms.devlang: ''
ms.topic: conceptual
author: srdan-bozovic-msft
ms.author: srbozovi
ms.reviewer: sstein, carlrab
ms.date: 12/04/2018
ms.openlocfilehash: 88965c25702917f17a226cfa51de662703136aae
ms.sourcegitcommit: 053e5e7103ab666454faf26ed51b0dfcd7661996
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/27/2020
ms.locfileid: "84031166"
---
# <a name="determine-the-management-endpoint-ip-address---azure-sql-managed-instance"></a>Determinación de la dirección IP del punto de conexión de administración: Instancia administrada de Azure SQL 
[!INCLUDE[appliesto-sqlmi](../includes/appliesto-sqlmi.md)]

El clúster virtual de Instancia administrada de Azure SQL contiene un punto de conexión de administración que Azure utiliza para operaciones de administración. El punto de conexión de administración está protegido con un firewall integrado a nivel de red y con comprobación mutua de certificados a nivel de aplicación. Puede determinar la dirección IP del punto de conexión de administración, pero no puede acceder a él.

Para determinar la dirección IP de administración, realice una [búsqueda DNS](/windows-server/administration/windows-commands/nslookup) en el FQDN de su instancia administrada de SQL: `mi-name.zone_id.database.windows.net`. Esto devolverá una entrada DNS como `trx.region-a.worker.vnet.database.windows.net`. Después, puede hacer una búsqueda DNS en este FQDN con ".vnet" eliminado. Esto devolverá la dirección IP de administración. 

Esta instancia de PowerShell lo hará automáticamente si reemplaza \<MI FQDN\> con la entrada DNS de su instancia administrada de SQL: `mi-name.zone_id.database.windows.net`:
  
``` powershell
  $MIFQDN = "<MI FQDN>"
  resolve-dnsname $MIFQDN | select -first 1  | %{ resolve-dnsname $_.NameHost.Replace(".vnet","")}
```

Para más información acerca de instancias administradas de SQL y conectividad, consulte [Arquitectura de conectividad de Instancia administrada de Azure SQL](connectivity-architecture-overview.md).
