---
title: DNS personalizado
titleSuffix: Azure SQL Managed Instance
description: En este tema se describen las opciones de configuración de un DNS personalizado con Instancia administrada de Azure SQL.
services: sql-database
ms.service: sql-database
ms.subservice: managed-instance
ms.custom: sqldbrb=1
ms.devlang: ''
ms.topic: conceptual
author: srdan-bozovic-msft
ms.author: srbozovi
ms.reviewer: sstein, bonova, carlrab
ms.date: 07/17/2019
ms.openlocfilehash: 5f442eb5b48611d39e9a123a495f8f42095c8017
ms.sourcegitcommit: 053e5e7103ab666454faf26ed51b0dfcd7661996
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/27/2020
ms.locfileid: "84031296"
---
# <a name="configuring-a-custom-dns-for-azure-sql-managed-instance"></a>Configuración de un DNS personalizado para Instancia administrada de Azure SQL
[!INCLUDE[appliesto-sqlmi](../includes/appliesto-sqlmi.md)]

Instancia administrada de Azure SQL debe implementarse dentro de una [red virtual (VNet)](../../virtual-network/virtual-networks-overview.md) de Azure. Hay algunos escenarios (por ejemplo, correo electrónico de base de datos o servidores vinculados a otras instancias de SQL en el entorno híbrido o de nube) que requieren que los nombres de host privados se resuelvan en la instancia administrada de SQL. En este caso, debe configurar un DNS personalizado dentro de Azure. 

Como Instancia administrada de SQL usa el mismo DNS para su funcionamiento interno, configure el servidor DNS personalizado para que pueda resolver los nombres de dominio públicos.

> [!IMPORTANT]
> Utilice siempre nombres de dominio completos (FQDN) para los servidores de correo electrónico, las instancias de SQL Server y otros servicios, aunque se encuentren en la zona DNS privada. Por ejemplo, `smtp.contoso.com` para el servidor de correo, ya que `smtp` no se resolverá correctamente. La creación de un servidor vinculado o una replicación que haga referencia a las máquinas virtuales de SQL de la misma red virtual también requiere un nombre de dominio completo y un sufijo DNS predeterminado. Por ejemplo, `SQLVM.internal.cloudapp.net`. Para más información, consulte [Resolución de nombres con su propio servidor DNS](https://docs.microsoft.com/azure/virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances#name-resolution-that-uses-your-own-dns-server).

> [!IMPORTANT]
> La actualización de los servidores DNS de una red virtual no afectará de inmediato a Instancia administrada de SQL. La configuración de DNS de Instancia administrada se actualiza una vez que la concesión de DHCP expira o la plataforma se actualiza, lo que sea que ocurra primero. **Se recomienda que los usuarios establezcan la configuración de DNS de una red virtual antes de crear su primera Instancia administrada.**

## <a name="next-steps"></a>Pasos siguientes

- Para información general, consulte [¿Qué es Instancia administrada de SQL Database (versión preliminar)?](sql-managed-instance-paas-overview.md)
- Para ver un tutorial que muestra cómo crear una instancia administrada, consulte el artículo de [creación de instancias administradas](instance-create-quickstart.md).
- Para información sobre cómo configurar una red virtual para una instancia administrada, consulte el artículo sobre la [configuración de la red virtual para las instancias administradas](connectivity-architecture-overview.md).
