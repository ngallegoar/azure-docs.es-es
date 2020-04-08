---
title: Reglas de firewall de IP
description: Configure reglas de firewall de IP en el nivel de servidor para un firewall de SQL Database o de SQL Data Warehouse. Administre el acceso y configure reglas de firewall de IP en el nivel de base de datos para una base de datos única o agrupada.
services: sql-database
ms.service: sql-database
ms.subservice: security
titleSuffix: Azure SQL Database and SQL Data Warehouse
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: VanMSFT
ms.author: vanto
ms.reviewer: carlrab
ms.date: 03/18/2019
ms.openlocfilehash: af88fdf3378a6290c773c658ea6dd3469d7c92cc
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79531284"
---
# <a name="azure-sql-database-and-azure-sql-data-warehouse-ip-firewall-rules"></a>Reglas de firewall de IP para Azure SQL Database y Azure SQL Data Warehouse

> [!NOTE]
> Este artículo se aplica a Azure SQL Server y tanto a las bases de datos de Azure SQL Database como de Azure SQL Data Warehouse hospedadas en un servidor de Azure SQL. Para simplificar, se utiliza *SQL Database*  para hacer referencia tanto a SQL Database como a SQL Data Warehouse.

> [!IMPORTANT]
> Este artículo *no* es válido para las *instancias administradas de Azure SQL Database*. Para información sobre la configuración de red, consulte [Conexión de una aplicación a Instancia administrada de Azure SQL Database](sql-database-managed-instance-connect-app.md).

Al crear un nuevo servidor de Azure SQL denominado *miServidorSQL*, por ejemplo, el firewall de SQL Database bloquea todo el acceso al punto de conexión público para el servidor (al que se puede acceder en *miServidorSQL.database.windows.net*).

> [!IMPORTANT]
> SQL Data Warehouse solo es compatible con las reglas de firewall de IP de nivel de servidor. No es compatible con las de nivel de base de datos.

## <a name="how-the-firewall-works"></a>Funcionamiento del firewall
Los intentos de conexión desde Internet y Azure deben atravesar el firewall antes de poder alcanzar el servidor o la base de datos SQL, tal y como se muestra en el siguiente diagrama.

   ![Diagrama de configuración del firewall][1]

### <a name="server-level-ip-firewall-rules"></a>Reglas de firewall de IP en el nivel de servidor

  Estas reglas permiten a los clientes obtener acceso a toda la instancia de Azure SQL Server (es decir, a todas las bases de datos que se encuentren en el mismo servidor de SQL Database). Estas reglas se almacenan en la base de datos *maestra*. Puede tener un máximo de 128 reglas de firewall IP de nivel de servidor para una instancia de Azure SQL Server. Si tiene habilitada la opción **Permitir que los servicios y recursos de Azure accedan a este servidor**, cuenta como una sola regla de firewall para Azure SQL Server.
  
  Puede configurar reglas de firewall de IP de nivel de servidor mediante Azure Portal, PowerShell o instrucciones Transact-SQL.
  - Para usar el portal o PowerShell, debe ser propietario o colaborador de la suscripción.
  - Para usar Transact-SQL, debe conectarse a la instancia de SQL Database como el inicio de sesión de la entidad de seguridad a nivel de servidor o como administrador de Azure Active Directory. Antes un usuario con permisos en el nivel de Azure debe crear una regla de firewall de IP en el nivel de servidor.

### <a name="database-level-ip-firewall-rules"></a>Reglas de firewall de IP en el nivel de base de datos

  Estas reglas permiten a los clientes acceder a determinadas bases de datos (seguras) dentro del mismo servidor de SQL Database. Se crean para cada base de datos (incluida la base datos *maestra*) y se almacenan en las bases de datos individuales.
  
  Las reglas de firewall de IP de nivel de base de datos para las bases de datos maestra y de usuario solo se pueden crear y administrar mediante instrucciones Transact-SQL y solo después de haber configurado la primera regla de firewall de nivel de servidor.
  
  Si especifica un intervalo de direcciones IP en la regla de firewall de IP de nivel de base de datos que se encuentra fuera del intervalo indicado en la regla de firewall de IP de nivel de servidor, solo los clientes que tengan direcciones IP en el intervalo de nivel de base de datos pueden tener acceso a la base de datos.
  
  Puede tener un máximo de 128 reglas de firewall de IP de nivel de base de datos para una base de datos. Para más información sobre la configuración de reglas de firewall de IP de nivel de base de datos, consulte el ejemplo que aparece más adelante en este artículo y [sp_set_database_firewall_rule (Azure SQL Database)](https://msdn.microsoft.com/library/dn270010.aspx).

### <a name="recommendations-for-how-to-set-firewall-rules"></a>Recomendaciones para establecer reglas de firewall

Se recomienda usar reglas de firewall de IP de nivel de base de datos siempre que sea posible. Este procedimiento mejora la seguridad y consigue que la base de datos sea más portátil. Use reglas de firewall de IP de nivel de servidor para los administradores. También puede usarlas cuando tenga muchas bases de datos con los mismos requisitos de acceso y no quiera configurar individualmente cada una de ellas.

> [!NOTE]
> Para más información acerca de bases de datos portátiles en el contexto de la continuidad empresarial, consulte [Requisitos de autenticación para la recuperación ante desastres](sql-database-geo-replication-security-config.md).

## <a name="server-level-versus-database-level-ip-firewall-rules"></a>Reglas de firewall de IP de nivel de servidor frente a nivel de base de datos

*¿Los usuarios de una base de datos deben estar completamente aislados de otra base de datos?*

En caso *afirmativo*, use reglas de firewall de IP de nivel de base de datos para conceder acceso. Este método evita el uso de reglas de firewall de IP de nivel de servidor, que permiten el acceso a través del firewall a todas las bases de datos. Esto reduce la solidez de sus defensas.

*¿Los usuarios de las direcciones IP necesitan acceso a todas las bases de datos?*

En caso *afirmativo*, utilice reglas de firewall de IP de nivel de servidor para reducir el número de veces que tiene que configurar este tipo de reglas.

*¿La persona o el equipo que configura las reglas de firewall de IP solo tiene acceso mediante Azure Portal, PowerShell o la API REST?*

En caso afirmativo, debe usar reglas de firewall de IP de nivel de servidor. Las reglas de firewall de IP de nivel de base de datos solo pueden configurarse mediante Transact-SQL.  

*¿Está prohibido que la persona o el equipo que configura las reglas de firewall de IP tenga permisos elevados en el nivel de base de datos?*

En caso afirmativo, use reglas de firewall de IP de nivel de servidor. Necesita al menos el permiso *CONTROL DATABASE* en el nivel de base de datos para configurar reglas de firewall de IP de nivel de base de datos mediante Transact-SQL.  

*¿La persona o el equipo que configura o audita las reglas de firewall de IP administra de forma centralizada las reglas de firewall de IP para muchas bases de datos (quizás cientos)?*

En este escenario, los procedimientos recomendados se determinan en función de las necesidades y el entorno. Las reglas de firewall de IP de nivel de servidor pueden ser más fáciles de configurar, pero con scripting se pueden configurar reglas en el nivel de base de datos. Incluso si usa las reglas de firewall de IP de nivel de servidor, podría necesitar auditar las reglas de firewall de IP de nivel de base de datos para ver si los usuarios con permiso *CONTROL* en la base de datos crean reglas de firewall de IP en este nivel.

*¿Puedo usar una combinación de reglas de firewall de IP de nivel de servidor y de base de datos?*

Sí. Algunos usuarios, por ejemplo, los administradores, pueden necesitar reglas de firewall de IP de nivel de servidor. Otros usuarios, como los usuarios de una aplicación de base de datos, pueden necesitar reglas de firewall de IP de nivel de base de datos.

### <a name="connections-from-the-internet"></a>Conexiones desde Internet

Cuando un equipo intenta conectarse al servidor de bases de datos desde Internet, el firewall comprueba la dirección IP de origen de la solicitud con las reglas de firewall de IP de nivel de base de datos, para la base de datos que se solicita en la conexión.

- Si la dirección está comprendida en uno de los intervalos especificados en las reglas de firewall de IP de nivel de base de datos, se concede la conexión a la base de datos SQL que contiene la regla.
- Si la dirección no está comprendida en uno de los intervalos de las reglas de IP de nivel de base de datos, el firewall comprueba las reglas de firewall de IP de nivel de servidor. Si la dirección está incluida en uno de los intervalos de estas reglas, se concede la conexión. Las reglas de firewall de IP de nivel de servidor se aplican a todas las instancias de SQL Database en el servidor SQL de Azure.  
- Si la dirección no se encuentra dentro de los intervalos especificados en cualquiera de las reglas de firewall de IP de nivel de base de datos o de servidor, la solicitud de conexión genera un error.

> [!NOTE]
> Para acceder a SQL Database desde el equipo local, asegúrese de que el firewall de su red y el equipo local permiten la comunicación saliente en el puerto TCP 1433.

### <a name="connections-from-inside-azure"></a>Conexiones desde dentro de Azure

Para permitir que las aplicaciones hospedadas en Azure se conecten al servidor SQL, deben habilitarse las conexiones de Azure. Cuando una aplicación intenta conectarse desde Azure al servidor de bases de datos, el firewall comprueba que se permiten las conexiones de Azure. Una configuración de firewall con las direcciones IP inicial y final iguales a *0.0.0.0* indica que se permiten las conexiones de Azure. Puede activarse directamente desde la hoja de Azure Portal mediante el establecimiento de reglas de firewall, así como si se cambia la opción **Permitir que los servicios y recursos de Azure accedan a este servidor** a **Activada** en la configuración **Firewalls y redes virtuales**. Si no se permite la conexión, la solicitud no alcanza al servidor de SQL Database.

> [!IMPORTANT]
> Esta opción configura el firewall para permitir todas las conexiones desde Azure, incluidas las procedentes de las suscripciones de otros clientes. Si selecciona esta opción, asegúrese de que los permisos de usuario y el inicio de sesión solo dejan acceder a los usuarios autorizados.

## <a name="create-and-manage-ip-firewall-rules"></a>Creación y administración de reglas de firewall de IP

La primera configuración del firewall de nivel de servidor se crea mediante [Azure Portal](https://portal.azure.com/) o bien mediante programación con [Azure PowerShell](https://docs.microsoft.com/powershell/module/az.sql), la [CLI de Azure](https://docs.microsoft.com/cli/azure/sql/server/firewall-rule) o una [API REST](https://docs.microsoft.com/rest/api/sql/firewallrules/createorupdate) de Azure. Las reglas de firewall de IP de nivel de servidor adicionales se crean y se administran mediante estos métodos o con Transact-SQL.

> [!IMPORTANT]
> Las reglas de firewall de IP de nivel de base de datos solo pueden crearse y administrarse mediante Transact-SQL.

Para mejorar el rendimiento, las reglas de firewall de IP de nivel de servidor se almacenan temporalmente en caché en el nivel de base de datos. Para actualizar la memoria caché, consulte [DBCC FLUSHAUTHCACHE](https://msdn.microsoft.com/library/mt627793.aspx).

> [!TIP]
> Puede usar [Auditoría de SQL Database](sql-database-auditing.md) para auditar cambios en el firewall a nivel de servidor y de base de datos.

### <a name="use-the-azure-portal-to-manage-server-level-ip-firewall-rules"></a>Uso de Azure Portal para administrar las reglas de firewall de IP de nivel de servidor

Para establecer una regla de firewall de IP de nivel de servidor en Azure Portal, vaya a la página de información general de Azure SQL Database o del servidor de SQL Database.

> [!TIP]
> Para consultar un tutorial, vea [Creación de una instancia de Azure SQL Database en Azure Portal](sql-database-single-database-get-started.md).

#### <a name="from-the-database-overview-page"></a>Desde la página de información general de la base de datos

1. Para establecer una regla de firewall de IP de nivel de servidor desde la página de información general de la base de datos, seleccione **Establecer el firewall del servidor** en la barra de herramientas, como se muestra en la siguiente imagen. 

    ![Regla de firewall de IP del servidor](./media/sql-database-get-started-portal/sql-database-server-set-firewall-rule.png)

    Se abrirá la página **Configuración del firewall** del servidor de SQL Database.

2. Seleccione **Agregar IP de cliente** en la barra de herramientas, para agregar la dirección IP del equipo que está usando y, a continuación, seleccione **Guardar**. Se creará una regla de firewall de IP de nivel de servidor para la dirección IP actual.

    ![Establecimiento de regla de firewall de IP de nivel de servidor](./media/sql-database-get-started-portal/sql-database-server-firewall-settings.png)

#### <a name="from-the-server-overview-page"></a>Desde la página de información general del servidor

Se abre la página de información general del servidor. Muestra el nombre completo del servidor (por ejemplo, *mynewserver20170403.database.windows.net*) y proporciona opciones para otras configuraciones.

1. Para establecer una regla de nivel de servidor desde esta página, seleccione **Firewall** en el menú **Configuración** del lado izquierdo.

2. Seleccione **Agregar IP de cliente** en la barra de herramientas, para agregar la dirección IP del equipo que está usando y, a continuación, seleccione **Guardar**. Se creará una regla de firewall de IP de nivel de servidor para la dirección IP actual.

### <a name="use-transact-sql-to-manage-ip-firewall-rules"></a>Uso de Transact-SQL para administrar reglas de firewall de IP

| Vista de catálogo o procedimiento almacenado | Nivel | Descripción |
| --- | --- | --- |
| [sys.firewall_rules](https://msdn.microsoft.com/library/dn269980.aspx) |Server |Muestra las reglas de firewall de IP de nivel de servidor actuales |
| [sp_set_firewall_rule](https://msdn.microsoft.com/library/dn270017.aspx) |Server |Crea o actualiza las reglas de firewall de IP de nivel de servidor |
| [sp_delete_firewall_rule](https://msdn.microsoft.com/library/dn270024.aspx) |Server |Quita las reglas de firewall de IP de nivel de servidor |
| [sys.database_firewall_rules](https://msdn.microsoft.com/library/dn269982.aspx) |Base de datos |Muestra las reglas de firewall de IP de nivel de base de datos actuales |
| [sp_set_database_firewall_rule](https://msdn.microsoft.com/library/dn270010.aspx) |Base de datos |Crea o actualiza las reglas de firewall de IP de nivel de base de datos |
| [sp_delete_database_firewall_rule](https://msdn.microsoft.com/library/dn270030.aspx) |Bases de datos |Quita las reglas de firewall de IP de nivel de base de datos |

En el ejemplo siguiente se revisan las reglas existentes, se habilita un intervalo de direcciones IP en el servidor *Contoso* y se elimina una regla de firewall de IP:

```sql
SELECT * FROM sys.firewall_rules ORDER BY name;
```

A continuación, agregue una regla de firewall de IP de nivel de servidor.

```sql
EXECUTE sp_set_firewall_rule @name = N'ContosoFirewallRule',
   @start_ip_address = '192.168.1.1', @end_ip_address = '192.168.1.10'
```

Para eliminar una regla de firewall de IP de nivel de servidor, ejecute el procedimiento almacenado *sp_delete_firewall_rule*. En el ejemplo siguiente se elimina la regla *ContosoFirewallRule*:

```sql
EXECUTE sp_delete_firewall_rule @name = N'ContosoFirewallRule'
```

### <a name="use-powershell-to-manage-server-level-ip-firewall-rules"></a>Uso de PowerShell para administrar reglas de firewall de IP de nivel de servidor 

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]
> [!IMPORTANT]
> El módulo Azure Resource Manager para PowerShell todavía es compatible con Azure SQL Database, pero todo el desarrollo se realiza ahora para el módulo Az.Sql. Para estos cmdlets, consulte [AzureRM.Sql](https://docs.microsoft.com/powershell/module/AzureRM.Sql/). Los argumentos para los comandos de los módulos Az y AzureRm son esencialmente idénticos.

| Cmdlet | Nivel | Descripción |
| --- | --- | --- |
| [Get-AzSqlServerFirewallRule](/powershell/module/az.sql/get-azsqlserverfirewallrule) |Server |Devuelve las reglas de firewall de nivel de servidor actuales |
| [New-AzSqlServerFirewallRule](/powershell/module/az.sql/new-azsqlserverfirewallrule) |Server |Crear una regla de firewall de nivel de servidor |
| [Set-AzSqlServerFirewallRule](/powershell/module/az.sql/set-azsqlserverfirewallrule) |Server |Actualiza las propiedades de una regla de firewall de nivel de servidor existente |
| [Remove-AzSqlServerFirewallRule](/powershell/module/az.sql/remove-azsqlserverfirewallrule) |Server |Elimina las reglas de firewall de nivel de servidor |

En el ejemplo siguiente se usa PowerShell para establecer una regla de firewall de IP de nivel de servidor:

```powershell
New-AzSqlServerFirewallRule -ResourceGroupName "myResourceGroup" `
    -ServerName $servername `
    -FirewallRuleName "ContosoIPRange" -StartIpAddress "192.168.1.0" -EndIpAddress "192.168.1.255"
```
> [!TIP]
> Para $servername, especifique el nombre del servidor y no el nombre DNS completo; por ejemplo, especifique **mysqldbserver** en lugar de **mysqldbserver.database.windows.net**.

> [!TIP]
> Para consultar ejemplos de PowerShell en el contexto de un inicio rápido, consulte [Creación de una base de datos con PowerShell](sql-database-powershell-samples.md) y [Creación de una base de datos única y configuración de una regla de firewall de IP de nivel de servidor de SQL Database mediante PowerShell](scripts/sql-database-create-and-configure-database-powershell.md).

### <a name="use-cli-to-manage-server-level-ip-firewall-rules"></a>Uso de la CLI para administrar reglas de firewall de IP de nivel de servidor

| Cmdlet | Nivel | Descripción |
| --- | --- | --- |
|[az sql server firewall-rule create](/cli/azure/sql/server/firewall-rule#az-sql-server-firewall-rule-create)|Server|Crea una regla de firewall de IP del servidor|
|[az sql server firewall-rule list](/cli/azure/sql/server/firewall-rule#az-sql-server-firewall-rule-list)|Server|Enumera las reglas de firewall de IP en un servidor|
|[az sql server firewall-rule show](/cli/azure/sql/server/firewall-rule#az-sql-server-firewall-rule-show)|Server|Muestra los detalles de una regla de firewall de IP.|
|[az sql server firewall-rule update](/cli/azure/sql/server/firewall-rule##az-sql-server-firewall-rule-update)|Server|Actualiza una regla de firewall de IP.|
|[az sql server firewall-rule delete](/cli/azure/sql/server/firewall-rule#az-sql-server-firewall-rule-delete)|Server|Elimina una regla de firewall de IP.|

En el ejemplo siguiente se usa la CLI para establecer una regla de firewall de IP de nivel de servidor:

```azurecli-interactive
az sql server firewall-rule create --resource-group myResourceGroup --server $servername \
-n ContosoIPRange --start-ip-address 192.168.1.0 --end-ip-address 192.168.1.255
```
> [!TIP]
> Para $servername, especifique el nombre del servidor y no el nombre DNS completo; por ejemplo, especifique **mysqldbserver** en lugar de **mysqldbserver.database.windows.net**.

> [!TIP]
> Para consultar un ejemplo de la CLI en el contexto de un inicio rápido, consulte [Creación de una base de datos con la CLI de Azure](sql-database-cli-samples.md) y [Creación de una base de datos única y configuración de una regla de firewall de IP de SQL Database con la CLI de Azure](scripts/sql-database-create-and-configure-database-cli.md).

### <a name="use-a-rest-api-to-manage-server-level-ip-firewall-rules"></a>Uso de API REST para administrar reglas de firewall de IP de nivel de servidor

| API | Nivel | Descripción |
| --- | --- | --- |
| [Enumerar reglas de firewall](https://docs.microsoft.com/rest/api/sql/firewallrules/listbyserver) |Server |Muestra las reglas de firewall de IP de nivel de servidor actuales |
| [Crear o actualizar regla de firewall](https://docs.microsoft.com/rest/api/sql/firewallrules/createorupdate) |Server |Crea o actualiza las reglas de firewall de IP de nivel de servidor |
| [Eliminar reglas de firewall](https://docs.microsoft.com/rest/api/sql/firewallrules/delete) |Server |Quita las reglas de firewall de IP de nivel de servidor |
| [Obtener reglas de firewall](https://docs.microsoft.com/rest/api/sql/firewallrules/get) | Server | Obtiene las reglas de firewall de IP de nivel de servidor |

## <a name="troubleshoot-the-database-firewall"></a>Solución de problemas del firewall de base de datos

Tenga en cuenta los siguientes puntos cuando el acceso al servicio de SQL Database no se comporte de la manera prevista.

- **Configuración del firewall local:**

  Antes de que el equipo pueda acceder a SQL Database, es posible que necesite crear una excepción del firewall en el equipo para el puerto TCP 1433. Para realizar conexiones dentro del límite de la nube de Azure, es posible que tenga que abrir puertos adicionales. Para más información, consulte la sección "Fuera o dentro" de [Puertos más allá de 1433 para ADO.NET 4.5 y SQL Database](sql-database-develop-direct-route-ports-adonet-v12.md).

- **Traducción de direcciones de red:**

  Debido a la traducción de direcciones de red (NAT), la dirección IP usada por el equipo para conectarse a SQL Database puede diferir de la que se muestra en los valores de la configuración de IP del equipo. Para ver la dirección IP que usa el equipo para conectarse a Azure:
    1. Inicie sesión en el portal.
    1. Vaya a la pestaña **Configurar** en el servidor que hospeda la base de datos.
    1. En la sección **Direcciones IP permitidas** se muestra **Dirección IP del cliente actual**. Seleccione **Agregar** en la opción **Direcciones IP permitidas** para permitir que este equipo acceda al servidor.

- **Los cambios en la lista de permitidos aún no se han aplicado:**

  Puede que se produzca un retraso de hasta cinco minutos en la aplicación de los cambios de configuración del firewall de SQL Database.

- **El inicio de sesión no está autorizado o se ha usado una contraseña incorrecta:**

  Si un inicio de sesión no tiene los permisos en el servidor de SQL Database o la contraseña es incorrecta, se denegará la conexión al servidor. La creación de una configuración de firewall solo ofrece a los clientes la *oportunidad* de intentar conectarse al servidor. El cliente todavía debe proporcionar las credenciales de seguridad necesarias. Para más información sobre la preparación de inicios de sesión, consulte [Control y concesión de acceso de base de datos a SQL Database y SQL Data Warehouse](sql-database-manage-logins.md).

- **Dirección IP dinámica:**

  Si tiene una conexión a Internet que usa direccionamiento IP dinámico y tiene problemas para atravesar el firewall, pruebe una de las soluciones siguientes:
  
  - Pida a su proveedor de acceso a Internet el intervalo de direcciones IP asignado a los equipos cliente que acceden al servidor de SQL Database. Agregue este intervalo como una regla de firewall de IP.
  - Obtenga el direccionamiento IP estático en su lugar para los equipos cliente. Agregue las direcciones IP como reglas de firewall de IP.

## <a name="next-steps"></a>Pasos siguientes

- Confirme que su entorno de red corporativo permite la comunicación entrante desde los intervalos de direcciones IP de proceso (incluidos los intervalos SQL) usados por los centros de datos de Azure. Es posible que tenga que agregar esas direcciones IP a la lista de permitidos. Consulte [Intervalos de direcciones IP del centro de datos de Microsoft Azure](https://www.microsoft.com/download/details.aspx?id=41653).  
- Para consultar un inicio rápido sobre la creación de una regla de firewall de IP de nivel de servidor, consulte [Creación de una base de datos de Azure SQL](sql-database-single-database-get-started.md).
- Si desea obtener ayuda para conectarse a una base de datos de Azure SQL desde aplicaciones de código abierto o de terceros, consulte [Ejemplos de código de inicio rápido de cliente para SQL Database](https://msdn.microsoft.com/library/azure/ee336282.aspx).
- Para información sobre los puertos adicionales que puede necesitar abrir, vea la sección "Fuera o dentro" de [Puertos más allá de 1433 para ADO.NET 4.5 y SQL Database](sql-database-develop-direct-route-ports-adonet-v12.md).
- Para obtener información general sobre la seguridad de Azure SQL Database, consulte [Protección de bases de datos](sql-database-security-overview.md).

<!--Image references-->
[1]: ./media/sql-database-firewall-configure/sqldb-firewall-1.png
