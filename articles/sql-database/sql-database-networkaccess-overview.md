---
title: Controles de acceso a la red
description: Información general sobre los controles de acceso a la red para Azure SQL Database y Data Warehouse para administrar el acceso y configurar una base de datos única o agrupada.
services: sql-database
ms.service: sql-database
ms.subservice: security
titleSuffix: Azure SQL Database and SQL Data Warehouse
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: rohitnayakmsft
ms.author: rohitna
ms.reviewer: vanto
ms.date: 03/09/2020
ms.openlocfilehash: 822fab5c00501d415c3c184587141e869523e417
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "78945393"
---
# <a name="azure-sql-database-and-data-warehouse-network-access-controls"></a>Controles de acceso a la red para Azure SQL Database y Data Warehouse

> [!NOTE]
> Este artículo se aplica a Azure SQL Server y tanto a las bases de datos de SQL Database como a SQL Data Warehouse que se crean en el servidor de Azure SQL. Para simplificar, SQL Database se utiliza cuando se hace referencia tanto a SQL Database como a SQL Data Warehouse.

> [!IMPORTANT]
> Este artículo *no* es válido para las **instancias administradas de Azure SQL Database**. Para más información sobre la configuración de red, consulte el artículo de [conexión a una instancia administrada](sql-database-managed-instance-connect-app.md).

Al crear una nueva instancia de Azure SQL Server en [Azure Portal](sql-database-single-database-get-started.md), el resultado es un punto de conexión público con el formato *nombreDelServidor.database.windows.net*.

Puede usar los siguientes controles de acceso a la red para permitir el acceso a la base de datos SQL de forma selectiva mediante el punto de conexión público:
- Allow Azure Services (Permitir servicios de Azure): cuando se establece en ACTIVADO, otros recursos dentro de los límites de Azure, por ejemplo, una máquina virtual de Azure, pueden acceder a SQL Database.

- Reglas de firewall de IP: use esta característica para permitir explícitamente las conexiones desde una dirección IP específica, por ejemplo, desde máquinas locales.

También puede permitir el acceso privado a SQL Database desde [redes virtuales](../virtual-network/virtual-networks-overview.md) a través de:
- Reglas de firewall de la red virtual: use esta característica para permitir el tráfico desde una red virtual específica dentro de los límites de Azure.

- Private Link: Use esta característica para crear un punto de conexión privado para Azure SQL Server dentro de una red virtual específica



Vea el vídeo siguiente para obtener una explicación de alto nivel sobre estos controles de acceso y lo que hacen:
> [!VIDEO https://channel9.msdn.com/Shows/Data-Exposed/Data-Exposed--SQL-Database-Connectivity-Explained/player?WT.mc_id=dataexposed-c9-niner]


## <a name="allow-azure-services"></a>Allow Azure Services (Permitir servicios de Azure) 
Durante la creación de una nueva instancia de Azure SQL Server [en Azure Portal](sql-database-single-database-get-started.md), esta configuración se deja desactivada.



También puede cambiar esta configuración desde el panel de firewall después de crear la instancia de Azure SQL Server como sigue.
  
 ![Captura de pantalla de la administración del firewall del servidor][2]

Cuando se establece en **ACTIVADO**, Azure SQL Server permite la comunicación de todos los recursos dentro de los límites de Azure, que pueden o no formar parte de la suscripción.

En muchos casos, el valor **ACTIVADO** es más permisivo que el que desean la mayoría de los clientes. Es posible que deseen establecer esta opción en **DESACTIVADO** y reemplazarla por reglas de firewall de IP más restrictivas o reglas de firewall de red virtual. Esta acción afecta a las siguientes características que se ejecutan en máquinas virtuales de Azure que no forman parte de la red virtual y, por tanto, se conectan a SQL Database a través de una dirección IP de Azure.

### <a name="import-export-service"></a>Import Export Service
Import Export Service no funciona, **Permitir que los servicios de Azure accedan al servidor** está establecido en OFF. Sin embargo, puede solucionar el problema [al ejecutar manualmente sqlpackage.exe desde una máquina virtual de Azure o realizar la exportación](https://docs.microsoft.com/azure/sql-database/import-export-from-vm) directamente en el código mediante la API de DACFx.

### <a name="data-sync"></a>Sincronización de datos
Para usar la característica de sincronización de datos con **Permitir que los servicios de Azure accedan al servidor** establecido en OFF, debe crear entradas de reglas de firewall individuales para [agregar direcciones IP](sql-database-server-level-firewall-rule.md) de la **etiqueta de servicio SQL** para la región que hospeda la base de datos **central**.
Agregue estas reglas de firewall de nivel de servidor a los servidores lógicos que hospedan las bases de datos **central** y **miembro** (que pueden estar en regiones diferentes).

Use el siguiente script de PowerShell para generar las direcciones IP correspondientes a la etiqueta de servicio de SQL para la región Oeste de EE. UU.
```powershell
PS C:\>  $serviceTags = Get-AzNetworkServiceTag -Location eastus2
PS C:\>  $sql = $serviceTags.Values | Where-Object { $_.Name -eq "Sql.WestUS" }
PS C:\> $sql.Properties.AddressPrefixes.Count
70
PS C:\> $sql.Properties.AddressPrefixes
13.86.216.0/25
13.86.216.128/26
13.86.216.192/27
13.86.217.0/25
13.86.217.128/26
13.86.217.192/27
```

> [!TIP]
> Get-AzNetworkServiceTag devuelve el intervalo global de la etiqueta de servicio SQL a pesar de que se especificó el parámetro Location. Asegúrese de filtrar para usarlo en la región que hospeda la base de datos central que usa el grupo de sincronización.

Tenga en cuenta que los resultados del script de PowerShell están en la notación Enrutamiento de interdominios sin clases (CIDR) y deben convertirse a un formato de dirección IP inicial y final con [Get-IPrangeStartEnd.ps1](https://gallery.technet.microsoft.com/scriptcenter/Start-and-End-IP-addresses-bcccc3a9) de la siguiente forma
```powershell
PS C:\> Get-IPrangeStartEnd -ip 52.229.17.93 -cidr 26                                                                   
start        end
-----        ---
52.229.17.64 52.229.17.127
```

Realice los siguientes pasos adicionales para convertir todas las direcciones IP de CIDR al formato de dirección IP inicial y final.

```powershell
PS C:\>foreach( $i in $sql.Properties.AddressPrefixes) {$ip,$cidr= $i.split('/') ; Get-IPrangeStartEnd -ip $ip -cidr $cidr;}                                                                                                                
start          end
-----          ---
13.86.216.0    13.86.216.127
13.86.216.128  13.86.216.191
13.86.216.192  13.86.216.223
```
Ahora puede agregarlas como reglas de firewall independientes y, a continuación, establecer la opción **Permitir que los servicios de Azure accedan al servidor** en OFF.


## <a name="ip-firewall-rules"></a>Reglas de firewall de IP
El firewall basado en IP es una característica de Azure SQL Server que impide todo acceso al servidor de bases de datos hasta que se agregan explícitamente las [direcciones IP](sql-database-server-level-firewall-rule.md) de las máquinas cliente.


## <a name="virtual-network-firewall-rules"></a>reglas de firewall de red virtual

Además de las reglas de IP, el firewall de Azure SQL Server permite definir *reglas de red virtual*.  
Para más información, consulte [Uso de reglas y puntos de conexión de servicio de red virtual para Azure SQL Database y SQL Data Warehouse](sql-database-vnet-service-endpoint-rule-overview.md) o vea este vídeo:

> [!VIDEO https://channel9.msdn.com/Shows/Data-Exposed/Data-Exposed--Demo--Vnet-Firewall-Rules-for-SQL-Database/player?WT.mc_id=dataexposed-c9-niner]

 ### <a name="azure-networking-terminology"></a>Terminología de redes en Azure  
Tenga en cuenta los siguientes términos de redes en Azure a medida que explora las reglas de firewall de red virtual

**Red virtual:** puede tener redes virtuales asociadas a la suscripción de Azure. 

**Subred:** una red virtual contiene **subredes**. Cualquier máquina virtual (VM) de Azure que tenga se asignará a las subredes. Una subred puede contener varias máquinas virtuales u otros nodos de proceso. Los nodos de proceso que se encuentran fuera de la red virtual no pueden tener acceso a su red virtual a menos que configure la seguridad para que permita el acceso.

**Punto de conexión de servicio de red virtual:** un [punto de conexión de servicio de red virtual](../virtual-network/virtual-network-service-endpoints-overview.md) es una subred cuyos valores de propiedad incluyen uno o más nombres formales de tipo de servicio de Azure. En este artículo nos interesa el nombre de tipo de **Microsoft.Sql**, que hace referencia al servicio de Azure denominado SQL Database.

**Regla de red virtual:** una regla de red virtual para el servidor de SQL Database es una subred que se muestra en la lista de control de acceso (ACL) del servidor de SQL Database. Para estar en la ACL de su SQL Database, la subred debe contener el nombre de tipo **Microsoft.Sql**. Una regla de red virtual indica a su servidor de SQL Database que acepte las comunicaciones procedentes de todos los nodos que están en la subred.


## <a name="ip-vs-virtual-network-firewall-rules"></a>Reglas de IP frente a reglas de firewall de red virtual

El firewall de Azure SQL Server le permite especificar intervalos de direcciones IP desde los que se aceptan las comunicaciones en SQL Database. Este enfoque es preciso para las direcciones IP estables que están fuera de la red privada de Azure. Sin embargo, las máquinas virtuales de la red privada de Azure se configuran con direcciones IP *dinámicas*. Las direcciones IP dinámicas pueden cambiar cuando se reinicia la máquina virtual y, a su vez, invalidar la regla de firewall basada en IP. Sería una locura especificar una dirección IP dinámica en una regla de firewall, en un entorno de producción.

Para superar esta limitación, puede obtener una dirección IP *estática* para la máquina virtual. Para más información, consulte [Configuración de direcciones IP privadas para una máquina virtual mediante Azure Portal](../virtual-network/virtual-networks-static-private-ip-arm-pportal.md). Sin embargo, el enfoque de IP estática puede resultar difícil de administrar, y es costoso si se realiza a escala. 

Las reglas de red virtual son alternativas más fáciles de establecer y su acceso, de administrar desde una subred específica que contenga las máquinas virtuales.

> [!NOTE]
> Aún no se puede disponer de SQL Database en una subred. Si su servidor de Azure SQL Database fuera un nodo de una subred de la red virtual, todos los nodos de la red virtual podrían comunicarse con su instancia de SQL Database. En este caso, las máquinas virtuales podrían comunicarse con SQL Database sin necesitar ninguna regla IP ni regla de red virtual.

## <a name="private-link"></a>Private Link 
Private Link le permite conectarse a Azure SQL Server mediante un **punto de conexión privado**. Un punto de conexión privado es una dirección IP privada dentro de una [red virtual](../virtual-network/virtual-networks-overview.md) y una subred específicas.

## <a name="next-steps"></a>Pasos siguientes

- Para consultar un inicio rápido sobre cómo crear una regla de firewall por IP de nivel de servidor, consulte [Creación de una base de datos de Azure SQL](sql-database-single-database-get-started.md).

- Para un inicio rápido sobre la creación de una regla de firewall de red virtual de nivel de servidor, consulte [Puntos de conexión de servicio de red virtual y reglas para Azure SQL Database](sql-database-vnet-service-endpoint-rule-overview.md).

- Si desea obtener ayuda para conectarse a una base de datos de Azure SQL desde aplicaciones de código abierto o de terceros, consulte [Ejemplos de código de inicio rápido de cliente para SQL Database](https://msdn.microsoft.com/library/azure/ee336282.aspx).

- Para información sobre los puertos adicionales que puede necesitar abrir, vea la sección **SQL Database: fuera frente a dentro**: de la sección [Puertos más allá de 1433 para ADO.NET 4.5 y SQL Database](sql-database-develop-direct-route-ports-adonet-v12.md)

- Para información general sobre la connectividad de Azure SQL Database, consulte [Arquitectura de conectividad de Azure SQL](sql-database-connectivity-architecture.md).

- Para obtener información general sobre la seguridad de Azure SQL Database, vea [Protección de bases de datos SQL](sql-database-security-overview.md).

<!--Image references-->
[1]: ./media/sql-database-get-started-portal/new-server2.png
[2]: ./media/sql-database-get-started-portal/manage-server-firewall.png

