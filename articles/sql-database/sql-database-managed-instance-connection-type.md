---
title: Tipos de conexión de la instancia administrada
description: Información acerca de los tipos de conexión de la instancia administrada
services: sql-database
ms.service: sql-database
ms.subservice: operations
ms.topic: conceptual
author: srdan-bozovic-msft
ms.author: srbozovi
ms.reviewer: vanto
ms.date: 10/07/2019
ms.openlocfilehash: a7df26590ec1514edcab24a1af9d85048b332d92
ms.sourcegitcommit: 318d1bafa70510ea6cdcfa1c3d698b843385c0f6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/21/2020
ms.locfileid: "83773637"
---
# <a name="azure-sql-database-managed-instance-connection-types"></a>Tipos de conexión de la instancia administrada de Azure SQL Database

En este artículo se explica cómo se conectan los clientes a la instancia administrada de Azure SQL Database en función del tipo de conexión. A continuación se proporcionan ejemplos de script para cambiar los tipos de conexión y consideraciones sobre la modificación de la configuración predeterminada de la conectividad.

## <a name="connection-types"></a>Tipos de conexión

La instancia administrada de Azure SQL Database admite los dos tipos de conexión siguientes:

- **Redirigir (recomendado):** los clientes establecen conexiones directamente con el nodo que hospeda la base de datos. Para habilitar la conectividad mediante el redireccionamiento, debe abrir los firewalls y los grupos de seguridad de red (NSG) para permitir el acceso a los puertos 1433 y 11000-11999. Los paquetes van directamente a la base de datos y, por lo tanto, hay mejoras de rendimiento de latencia y capacidad de proceso mediante el redireccionamiento en proxy.
- **Proxy (predeterminado)** : en este modo, todas las conexiones utilizan un componente de puerta de enlace de proxy. Para habilitar la conectividad, solo es necesario abrir el puerto 1433 para las redes privadas y el puerto 3342 para la conexión pública. La elección de este modo puede producir una latencia mayor y un rendimiento inferior, según la naturaleza de la carga de trabajo. Es muy recomendable utilizar la directiva de conexión Redirigir para obtener la menor latencia y el mayor rendimiento.

## <a name="redirect-connection-type"></a>Tipo de conexión: redireccionamiento

En este tipo de conexión, una vez establecida la sesión TCP en el motor de SQL, la sesión de cliente obtiene la dirección IP virtual de destino del nodo de clúster virtual del equilibrador de carga. Los paquetes posteriores fluyen directamente al nodo de clúster virtual y omiten la puerta de enlace. En el siguiente diagrama, se ilustra este flujo de tráfico.

![redirect.png](media/sql-database-managed-instance-connection-types/redirect.png)

> [!IMPORTANT]
> El redireccionamiento solo funciona actualmente para los puntos de conexión privados. Independientemente de la configuración del tipo de conexión, las conexiones que llegan por el punto de conexión público se realizarían mediante proxy.

## <a name="proxy-connection-type"></a>Tipo de conexión: proxy

En este tipo de conexión, la sesión TCP se establece con la puerta de enlace y todos los paquetes posteriores fluyen por esta. En el siguiente diagrama, se ilustra este flujo de tráfico.

![proxy.png](media/sql-database-managed-instance-connection-types/proxy.png)

## <a name="script-to-change-connection-type-settings-using-powershell"></a>Script para cambiar la configuración del tipo de conexión mediante PowerShell

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

El siguiente script de PowerShell muestra cómo cambiar el tipo de conexión a redireccionamiento para una instancia administrada.

```powershell
Install-Module -Name Az
Import-Module Az.Accounts
Import-Module Az.Sql

Connect-AzAccount
# Get your SubscriptionId from the Get-AzSubscription command
Get-AzSubscription
# Use your SubscriptionId in place of {subscription-id} below
Select-AzSubscription -SubscriptionId {subscription-id}
# Replace {rg-name} with the resource group for your managed instance, and replace {mi-name} with the name of your managed instance
$mi = Get-AzSqlInstance -ResourceGroupName {rg-name} -Name {mi-name}
$mi = $mi | Set-AzSqlInstance -ProxyOverride "Redirect" -force
```

## <a name="next-steps"></a>Pasos siguientes

- [Restauración de una base de datos en una instancia administrada](sql-database-managed-instance-get-started-restore.md)
- Aprenda a [configurar un punto de conexión público en una instancia administrada](sql-database-managed-instance-public-endpoint-configure.md)
- Más información sobre la [arquitectura de conectividad de la instancia administrada](sql-database-managed-instance-connectivity-architecture.md)