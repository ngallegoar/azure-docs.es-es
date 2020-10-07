---
title: Descubrimiento de dirección IP del punto de conexión de administración
titleSuffix: Azure SQL Managed Instance
description: Aprenda a obtener la dirección IP pública del punto de conexión de administración de Instancia administrada de Azure SQL y compruebe la protección de su firewall integrado
services: sql-database
ms.service: sql-managed-instance
ms.subservice: operations
ms.custom: sqldbrb=1
ms.devlang: ''
ms.topic: how-to
author: srdan-bozovic-msft
ms.author: srbozovi
ms.reviewer: sstein
ms.date: 12/04/2018
ms.openlocfilehash: a9a2b904bd7526f00a8f8a5d013be0c1e42e38a8
ms.sourcegitcommit: 4bebbf664e69361f13cfe83020b2e87ed4dc8fa2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/01/2020
ms.locfileid: "91617373"
---
# <a name="determine-the-management-endpoint-ip-address---azure-sql-managed-instance"></a>Determinación de la dirección IP del punto de conexión de administración: Instancia administrada de Azure SQL 
[!INCLUDE[appliesto-sqlmi](../includes/appliesto-sqlmi.md)]

El clúster virtual de Instancia administrada de Azure SQL contiene un punto de conexión de administración que Azure utiliza para operaciones de administración. El punto de conexión de administración está protegido con un firewall integrado a nivel de red y con comprobación mutua de certificados a nivel de aplicación. Puede determinar la dirección IP del punto de conexión de administración, pero no puede acceder a él.

Para determinar la dirección IP de administración, realice una [búsqueda DNS](/windows-server/administration/windows-commands/nslookup) en el FQDN de su instancia administrada de SQL: `mi-name.zone_id.database.windows.net`. Esto devolverá una entrada DNS como `trx.region-a.worker.vnet.database.windows.net`. Después, puede hacer una búsqueda DNS en este FQDN con ".vnet" eliminado. Esto devolverá la dirección IP de administración. 

Este código de PowerShell lo hará automáticamente si reemplaza \<MI FQDN\> con la entrada DNS de SQL Managed Instance: `mi-name.zone_id.database.windows.net`:
  
``` powershell
  $MIFQDN = "<MI FQDN>"
  resolve-dnsname $MIFQDN | select -first 1  | %{ resolve-dnsname $_.NameHost.Replace(".vnet","")}
```

Para más información acerca de SQL Managed Instance y la conectividad, consulte [Arquitectura de conectividad de Azure SQL Managed Instance](connectivity-architecture-overview.md).
