---
title: Configuración de reglas de aplicación de Azure Firewall con nombres de dominio completos de SQL
description: En este artículo se aprende a configurar nombres de dominio completos (FQDN) de SQL en reglas de aplicación de Azure Firewall.
services: firewall
author: vhorne
ms.service: firewall
ms.topic: how-to
ms.date: 06/18/2020
ms.author: victorh
ms.openlocfilehash: 7256f94b8e8376cf98a279d085a131a4ce84826f
ms.sourcegitcommit: 8e7316bd4c4991de62ea485adca30065e5b86c67
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/17/2020
ms.locfileid: "94658629"
---
# <a name="configure-azure-firewall-application-rules-with-sql-fqdns"></a>Configuración de reglas de aplicación de Azure Firewall con nombres de dominio completos de SQL

Ahora es posible configurar reglas de aplicación de Azure Firewall con FQDN de SQL. Esto permite limitar el acceso desde las redes virtuales a las instancias de SQL server especificadas.

Con los FQDN de SQL, puede filtrar el tráfico:

- Desde las redes virtuales a una instancia de Azure SQL Database o Azure Synapse Analytics. Por ejemplo: permitir el acceso a *sql-server1.database.windows.net* únicamente.
- Desde el entorno local a instancias administradas de Azure SQL o SQL IaaS que se ejecutan en las redes virtuales.
- Desde spoke-to-spoke a instancias administradas de Azure SQL o SQL IaaS que se ejecutan en las redes virtuales.

El filtrado por nombre de dominio completo de SQL se admite solo en [modo de proxy](../azure-sql/database/connectivity-architecture.md#connection-policy) (puerto 1433). Si usa SQL en el modo de redirección predeterminado, puede filtrar el acceso mediante la etiqueta de servicio de SQL como parte de [reglas de red](features.md#network-traffic-filtering-rules).
Si usa puertos no predeterminados para el tráfico de SQL IaaS, puede configurar esos puertos en las reglas de aplicación del firewall.

## <a name="configure-using-azure-cli"></a>Configuración mediante la CLI de Azure

1. Implemente [Azure Firewall mediante la CLI de Azure](deploy-cli.md).
2. Si filtra el tráfico a Azure SQL Database, Azure Synapse Analytic o SQL Managed Instance, asegúrese de que el modo de conectividad SQL esté establecido en **Proxy**. Para obtener información sobre cómo cambiar el modo de conectividad SQL, consulte [Configuración de conectividad de Azure SQL](../azure-sql/database/connectivity-settings.md#change-the-connection-policy-via-the-azure-cli).

   > [!NOTE]
   > El modo de *proxy* de SQL puede traducirse en una mayor latencia en comparación con el modo de *redirección*. Si quiere seguir usando el modo de redirección, que es el valor predeterminado para los clientes que se conectan desde dentro de Azure, puede filtrar el acceso mediante la [etiqueta de servicio](service-tags.md) de SQL en [reglas de red](tutorial-firewall-deploy-portal.md#configure-a-network-rule) del firewall.

3. Configure una regla de aplicación con FQDN de SQL para permitir el acceso a SQL Server:

   ```azurecli
   az extension add -n azure-firewall

   az network firewall application-rule create \
   -g FWRG \
   -f azfirewall \
   -c FWAppRules \
   -n srule \
   --protocols mssql=1433 \
   --source-addresses 10.0.0.0/24 \
   --target-fqdns sql-serv1.database.windows.net
   ```

## <a name="configure-using-the-azure-portal"></a>Configurar mediante Azure Portal
1. Implemente [Azure Firewall mediante la CLI de Azure](deploy-cli.md).
2. Si filtra el tráfico a Azure SQL Database, Azure Synapse Analytic o SQL Managed Instance, asegúrese de que el modo de conectividad SQL esté establecido en **Proxy**. Para obtener información sobre cómo cambiar el modo de conectividad SQL, consulte [Configuración de conectividad de Azure SQL](../azure-sql/database/connectivity-settings.md#change-the-connection-policy-via-the-azure-cli).  

   > [!NOTE]
   > El modo de *proxy* de SQL puede traducirse en una mayor latencia en comparación con el modo de *redirección*. Si quiere seguir usando el modo de redirección, que es el valor predeterminado para los clientes que se conectan desde dentro de Azure, puede filtrar el acceso mediante la [etiqueta de servicio](service-tags.md) de SQL en [reglas de red](tutorial-firewall-deploy-portal.md#configure-a-network-rule) del firewall.
3. Agregue la regla de aplicación con el protocolo, el puerto y el FQDN de SQL adecuados y luego seleccione **Guardar**.
   ![regla de aplicación con FQDN de SQL](media/sql-fqdn-filtering/application-rule-sql.png)
4. Acceda a SQL desde una máquina virtual de una red virtual que filtre el tráfico a través del firewall. 
5. Valide que los [registros de Azure Firewall](log-analytics-samples.md) muestran que se permite el tráfico.

## <a name="next-steps"></a>Pasos siguientes

Para información sobre los modos de redirección y proxy SQL, consulte [Arquitectura de conectividad de Azure SQL Database](../azure-sql/database/connectivity-architecture.md).