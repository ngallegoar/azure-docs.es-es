---
title: Configuración de reglas de aplicación de Azure Firewall con nombres de dominio completos de SQL
description: En este artículo se aprende a configurar nombres de dominio completos (FQDN) de SQL en reglas de aplicación de Azure Firewall.
services: firewall
author: vhorne
ms.service: firewall
ms.topic: article
ms.date: 07/19/2019
ms.author: victorh
ms.openlocfilehash: 858cfc9a8c15f1e33e688bb5086a58f194e7173f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79501500"
---
# <a name="configure-azure-firewall-application-rules-with-sql-fqdns"></a>Configuración de reglas de aplicación de Azure Firewall con nombres de dominio completos de SQL

> [!IMPORTANT]
> Las reglas de aplicación de Azure Firewall con FQDN de SQL están actualmente en versión preliminar pública.
> Esta versión preliminar se ofrece sin Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas.
> Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Ahora es posible configurar reglas de aplicación de Azure Firewall con FQDN de SQL. Esto permite limitar el acceso desde las redes virtuales a las instancias de SQL server especificadas.

Con los FQDN de SQL, puede filtrar el tráfico:

- Desde las redes virtuales a una instancia de Azure SQL Database o Azure SQL Data Warehouse. Por ejemplo: permitir el acceso a *sql-server1.database.windows.net* únicamente.
- Desde el entorno local a instancias administradas de Azure SQL o SQL IaaS que se ejecutan en las redes virtuales.
- Desde spoke-to-spoke a instancias administradas de Azure SQL o SQL IaaS que se ejecutan en las redes virtuales.

Durante la versión preliminar pública, el filtrado por nombre de dominio completo de SQL se admite solo en [modo de proxy](https://docs.microsoft.com/azure/sql-database/sql-database-connectivity-architecture#connection-policy) (puerto 1433). Si usa SQL en el modo de redirección predeterminado, puede filtrar el acceso mediante la etiqueta de servicio de SQL como parte de [reglas de red](overview.md#network-traffic-filtering-rules).
Si usa puertos no predeterminados para el tráfico de SQL IaaS, puede configurar esos puertos en las reglas de aplicación del firewall.

Las reglas de aplicación con FQDN de SQL están disponibles actualmente en todas las regiones a través de Azure Portal, la CLI de Azure, REST y plantillas.

## <a name="configure-using-azure-cli"></a>Configuración mediante la CLI de Azure

1. Implemente [Azure Firewall mediante la CLI de Azure](deploy-cli.md).
2. Si filtra el tráfico a Azure SQL Database, SQL Data Warehouse o Instancia administrada de SQL, asegúrese de que el modo de conectividad SQL esté establecido en **Proxy**. Para obtener información sobre cómo cambiar el modo de conectividad SQL, consulte [Configuración de conectividad de Azure SQL](https://docs.microsoft.com/azure/sql-database/sql-database-connectivity-settings#change-connection-policy-via-azure-cli).

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
2. Si filtra el tráfico a Azure SQL Database, SQL Data Warehouse o Instancia administrada de SQL, asegúrese de que el modo de conectividad SQL esté establecido en **Proxy**. Para obtener información sobre cómo cambiar el modo de conectividad SQL, consulte [Configuración de conectividad de Azure SQL](https://docs.microsoft.com/azure/sql-database/sql-database-connectivity-settings#change-connection-policy-via-azure-cli).  

   > [!NOTE]
   > El modo de *proxy* de SQL puede traducirse en una mayor latencia en comparación con el modo de *redirección*. Si quiere seguir usando el modo de redirección, que es el valor predeterminado para los clientes que se conectan desde dentro de Azure, puede filtrar el acceso mediante la [etiqueta de servicio](service-tags.md) de SQL en [reglas de red](tutorial-firewall-deploy-portal.md#configure-a-network-rule) del firewall.
3. Agregue la regla de aplicación con el protocolo, el puerto y el FQDN de SQL adecuados y luego seleccione **Guardar**.
   ![regla de aplicación con FQDN de SQL](media/sql-fqdn-filtering/application-rule-sql.png)
4. Acceda a SQL desde una máquina virtual de una red virtual que filtre el tráfico a través del firewall. 
5. Valide que los [registros de Azure Firewall](log-analytics-samples.md) muestran que se permite el tráfico.

## <a name="next-steps"></a>Pasos siguientes

Para obtener información sobre los modos de proxy y redirección de SQL, vea [Arquitectura de conectividad de Azure SQL Database](../sql-database/sql-database-connectivity-architecture.md).