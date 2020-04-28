---
title: Managed instance connect application (Aplicación conectada a una instancia administrada)
description: En este artículo se describe la conexión de la aplicación a Instancia administrada de Azure SQL Database.
services: sql-database
ms.service: sql-database
ms.subservice: managed-instance
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: srdan-bozovic-msft
ms.author: srbozovi
ms.reviewer: sstein, bonova, carlrab, vanto
ms.date: 11/09/2018
ms.openlocfilehash: 8d920fb7815e5a9fe30d8f3b4e40f36133d83222
ms.sourcegitcommit: 31ef5e4d21aa889756fa72b857ca173db727f2c3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/16/2020
ms.locfileid: "81538093"
---
# <a name="connect-your-application-to-azure-sql-database-managed-instance"></a>Conexión de la aplicación a una instancia administrada de Azure SQL Database

Hoy en día puede elegir entre varias opciones sobre cuándo y cómo hospedar la aplicación.

Puede elegir hospedar la aplicación en la nube mediante Azure App Service o algunas de las opciones de red virtual integradas de Azure, como Azure App Service Environment, Virtual Machine o Virtual Machine Scale Set. También podría elegir un enfoque de nube híbrida y mantener las aplicaciones en local.

Sea cual sea su opción, se podrá realizar una conexión a una Instancia administrada.  

![alta disponibilidad](./media/sql-database-managed-instance/application-deployment-topologies.png)

## <a name="connect-an-application-inside-the-same-vnet"></a>Conexión de una aplicación dentro de la misma red virtual

Este escenario es el más sencillo. Las máquinas virtuales dentro de la red virtual pueden conectarse directamente entre sí aunque se encuentren en subredes diferentes. Esto significa que para conectar la aplicación dentro de una instancia de Azure Application Environment o de Virtual Machine solo tiene que establecer correctamente la cadena de conexión.  

## <a name="connect-an-application-inside-a-different-vnet"></a>Conexión de una aplicación dentro de una red virtual distinta

Este escenario es un poco más complejo, ya que Instancia administrada tiene la dirección IP privada en su propia red virtual. Para conectarse, una aplicación necesita acceso a la red virtual donde Instancia administrada está implementada. Por lo tanto, primero debe establecer una conexión entre la aplicación y la red virtual de Instancia administrada. Las redes virtuales no tienen por qué estar en la misma suscripción para que este escenario funcione.

Para conectar redes virtuales hay dos opciones:

- [Emparejamiento de Azure Virtual Network](../virtual-network/virtual-network-peering-overview.md)
- Puerta de enlace VPN de red virtual a red virtual ([Azure Portal](../vpn-gateway/vpn-gateway-howto-vnet-vnet-resource-manager-portal.md), [PowerShell](../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md), [CLI de Azure](../vpn-gateway/vpn-gateway-howto-vnet-vnet-cli.md))

La opción de emparejamiento es la preferida, porque se usa la red troncal de Microsoft y, por lo tanto, desde la perspectiva de la conectividad, no hay diferencias notables de latencia entre máquinas virtuales de la red virtual emparejada y la propia. El emparejamiento de VNet está restringido a las redes de la misma región.  

> [!IMPORTANT]
> El escenario de emparejamiento de VNet de la instancia administrada está restringido a las redes de la misma región debido a las [restricciones de emparejamiento de redes virtuales globales](../virtual-network/virtual-network-manage-peering.md#requirements-and-constraints). Consulte también la sección correspondiente del artículo [Preguntas más frecuentes (P+F) acerca de Azure Virtual Network](https://docs.microsoft.com/azure/virtual-network/virtual-networks-faq#what-are-the-constraints-related-to-global-vnet-peering-and-load-balancers) para obtener más información. 

## <a name="connect-an-on-premises-application"></a>Conexión de una aplicación local

A Instancia administrada solo se puede acceder mediante dirección IP privada. Para acceder a la instancia desde la máquina local, debe establecer una conexión de sitio a sitio entre la aplicación y la red virtual de Instancia administrada.

Hay dos opciones de conexión local a una red virtual de Azure:

- Conexión VPN de sitio a sitio ([Azure Portal](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md), [PowerShell](../vpn-gateway/vpn-gateway-create-site-to-site-rm-powershell.md), [CLI de Azure](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-cli.md))
- Conexión de [ExpressRoute](../expressroute/expressroute-introduction.md)  

Si ha establecido la conexión local a Azure correctamente y no puede establecer la conexión con Instancia administrada, compruebe si el firewall tiene abierta la conexión saliente en el puerto 1433 de SQL, así como los puertos 11000-11999 para el redireccionamiento.

## <a name="connect-an-application-on-the-developers-box"></a>Conexión de una aplicación en el cuadro de desarrollo

A Instancia administrada se puede acceder solo mediante una dirección IP privada, por ello, para acceder desde el cuadro de desarrollo, primero debe realizar una conexión entre el cuadro de desarrollo y la red virtual de Instancia administrada. Para ello, configure una conexión de punto a sitio a una red virtual mediante la autenticación de certificados de Azure nativa. Para obtener más información, consulte [Configure a point-to-site connection to connect to an Azure SQL Database Managed Instance from on-premises computer](sql-database-managed-instance-configure-p2s.md) (Configuración de una conexión de punto a sitio para conectarse a una Instancia administrada de Azure SQL Database desde el equipo local).

## <a name="connect-from-on-premises-with-vnet-peering"></a>Conexión desde un entorno local con el emparejamiento de VNET

Otro escenario implementado por los clientes es en el que VPN Gateway está instalado en una red virtual independiente y en el que una suscripción de este servicio hospeda Instancia administrada. Entonces, las dos redes virtuales se emparejan. El siguiente diagrama de arquitectura de ejemplo muestra cómo se puede realizar esta implementación.

![Emparejamiento de VNET](./media/sql-database-managed-instance-connect-app/vnet-peering.png)

Una vez configurada la infraestructura básica, deberá modificar alguna configuración para que VPN Gateway pueda ver las direcciones IP en la red virtual que hospeda Instancia administrada. Para ello, realice los siguientes cambios específicos en la **Configuración de emparejamiento**.

1. En la red virtual que hospeda VPN Gateway, vaya a **Emparejamientos**, a continuación, a la Instancia administrada emparejada con la conexión de red virtual y, luego, haga clic en **Permitir tránsito de puerta de enlace**.
2. En la red virtual que hospeda Instancia administrada, vaya a **Emparejamientos**, a continuación, a la instancia de VPN Gateway emparejada con la conexión de red virtual y, luego, haga clic en **Usar puertas de enlace remotas**.

## <a name="connect-an-azure-app-service-hosted-application"></a>Conexión de una aplicación hospedada en Azure App Service

A Instancia administrada se puede acceder solo mediante una dirección IP privada, por ello, para acceder desde Azure App Service, primero debe realizar una conexión entre la aplicación y la red virtual de Instancia administrada. Consulte [Integración de su aplicación con una instancia de Azure Virtual Network](../app-service/web-sites-integrate-with-vnet.md).  

Para la solución de problemas, consulte la sección de [solución de problemas con redes virtuales y aplicaciones](../app-service/web-sites-integrate-with-vnet.md#troubleshooting). Si no se puede establecer una conexión, intente [sincronizar la configuración de red](sql-database-managed-instance-sync-network-configuration.md).

Un caso especial de conexión de Azure App Service a Instancia administrada es cuando la instancia de Azure App Service se ha integrado en una red virtual emparejada a otra red virtual de Instancia administrada. Ese caso requiere la siguiente configuración:

- La red virtual de Instancia administrada NO debe tener puerta de enlace  
- La red virtual de Instancia administrada debe tener la opción Usar puertas de enlace remotas establecida
- La red virtual emparejada debe tener la opción Permitir tránsito de puerta de enlace establecida

Este escenario se ilustra en el diagrama siguiente:

![emparejamiento de aplicación integrada](./media/sql-database-managed-instance/integrated-app-peering.png)

>[!NOTE]
>La característica Integración con red virtual no integra una aplicación con una red virtual que tiene una puerta de enlace de ExpressRoute. Incluso si la puerta de enlace de ExpressRoute está configurada en modo de coexistencia, la integración de red virtual no funcionará. Si necesita acceder a los recursos mediante una conexión de ExpressRoute, puede utilizar una instancia de App Service Environment que se ejecute en la red virtual.

## <a name="troubleshooting-connectivity-issues"></a>Solución de problemas de conectividad

Para solucionar problemas de conectividad, consulte la siguiente información:

- Si no puede conectarse a la Instancia administrada desde una máquina virtual de Azure dentro de la misma red virtual, pero desde una subred diferente, compruebe si tiene un grupo de seguridad de red establecido en la subred de la máquina virtual que podría estar bloqueando el acceso. Además, tenga en cuenta que deberá abrir una conexión saliente en el puerto 1433 de SQL, así como los puertos en el intervalo 11000-11999, ya que son necesarios para establecer conexión a través de un redireccionamiento dentro del límite de Azure.
- Asegúrese de que la propagación de BGP está establecida en **Habilitado** para la tabla de rutas asociada a la red virtual.
- Si utiliza VPN de P2S, compruebe la configuración en Azure Portal para ver si detecta números de **entrada/salida**. Los números distintos de cero indican que Azure enruta el tráfico hacia y desde un entorno local.

   ![Números de entrada/salida](./media/sql-database-managed-instance-connect-app/ingress-egress-numbers.png)

- Compruebe que la máquina cliente (que ejecuta el cliente VPN) tenga entradas de ruta para todas las redes virtuales a las que debe acceder. Las rutas se almacenan en `%AppData%\ Roaming\Microsoft\Network\Connections\Cm\<GUID>\routes.txt`.

   ![route.txt](./media/sql-database-managed-instance-connect-app/route-txt.png)

   Como se muestra en esta imagen, hay dos entradas para cada red virtual implicada y una tercera entrada para el punto de conexión VPN que está configurada en el portal.

   Otra manera de comprobar las rutas es mediante el comando siguiente. La salida muestra las rutas a las diversas subredes:

   ```cmd
   C:\ >route print -4
   ===========================================================================
   Interface List
   14...54 ee 75 67 6b 39 ......Intel(R) Ethernet Connection (3) I218-LM
   57...........................rndatavnet
   18...94 65 9c 7d e5 ce ......Intel(R) Dual Band Wireless-AC 7265
   1...........................Software Loopback Interface 1
   Adapter===========================================================================

   IPv4 Route Table
   ===========================================================================
   Active Routes:
   Network Destination        Netmask          Gateway       Interface  Metric
          0.0.0.0          0.0.0.0       10.83.72.1     10.83.74.112     35
         10.0.0.0    255.255.255.0         On-link       172.26.34.2     43
         10.4.0.0    255.255.255.0         On-link       172.26.34.2     43
   ===========================================================================
   Persistent Routes:
   None
   ```

- Si usa el emparejamiento de redes virtuales, asegúrese de que ha seguido las instrucciones de configuración [Permitir tránsito de puerta de enlace y Usar puertas de enlace remotas](#connect-from-on-premises-with-vnet-peering).

- Si usa el emparejamiento de redes virtuales para conectar una aplicación hospedada de Azure App Service, y la red virtual de Instancia administrada tiene un intervalo de direcciones IP públicas, asegúrese de que la configuración de la aplicación hospedada permita que el tráfico saliente se enrute a redes con direcciones IP públicas. Siga las instrucciones de [Integración con red virtual regional](../app-service/web-sites-integrate-with-vnet.md#regional-vnet-integration).

## <a name="required-versions-of-drivers-and-tools"></a>Versiones necesarias de los controladores y las herramientas

Si desea conectarse a Instancia administrada, se recomiendan las siguientes versiones mínimas de las herramientas y los controladores:

| Controlador/Herramienta | Versión |
| --- | --- |
|.NET Framework | 4.6.1 o .NET Core |
|Controlador ODBC| v17 |
|Controlador PHP| 5.2.0 |
|Controlador JDBC| 6.4.0 |
|Controlador de Node.js| 2.1.1 |
|Controlador de OLEDB| 18.0.2.0 |
|SSMS| 18.0 o [superior](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) |
|[SMO](https://docs.microsoft.com/sql/relational-databases/server-management-objects-smo/sql-server-management-objects-smo-programming-guide) | [150](https://www.nuget.org/packages/Microsoft.SqlServer.SqlManagementObjects) o superior |

## <a name="next-steps"></a>Pasos siguientes

- Para más información acerca de Instancia administrada, consulte [¿Qué es Instancia administrada de SQL Database (versión preliminar)?](sql-database-managed-instance.md).
- Para un tutorial que muestra cómo crear una instancia administrada, consulte el artículo de [creación de instancias administradas](sql-database-managed-instance-get-started.md).
