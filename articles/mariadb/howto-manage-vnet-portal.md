---
title: Administración de puntos de conexión de red virtual de Azure Database for MariaDB mediante Azure Portal
description: Crear y administrar las reglas y puntos de conexión del servicio de red virtual de Azure Database for MariaDB mediante Azure Portal
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.topic: conceptual
ms.date: 3/18/2020
ms.openlocfilehash: 61a8337536f55ceda9bef5b7eaa67a37644d2aca
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79530604"
---
# <a name="create-and-manage-azure-database-for-mariadb-vnet-service-endpoints-and-vnet-rules-by-using-the-azure-portal"></a>Crear y administrar las reglas y puntos de conexión del servicio de red virtual de Azure Database for MariaDB mediante Azure Portal

Las reglas y los puntos de conexión de los servicios de red virtual (VNet) amplían el espacio de direcciones privadas de una red virtual al servidor de Azure Database for MariaDB. Para obtener información general sobre los puntos de conexión de servicio de red virtual de Azure Database for MariaDB, incluidas las limitaciones, consulte [Puntos de conexión de servicio de red virtual del servidor de Azure Database for MariaDB](concepts-data-access-security-vnet.md). Los puntos de conexión del servicio de red virtual están disponibles en todas las regiones admitidas para Azure Database for MariaDB.

> [!NOTE]
> La compatibilidad con puntos de conexión de servicio de red virtual solo existe para servidores de uso general y optimizados para memoria.

## <a name="create-a-vnet-rule-and-enable-service-endpoints"></a>Crear una regla de red virtual y habilitar los puntos de conexión de servicio

1. En la página del servidor de MariaDB, en el encabezado Configuración, haga clic en **Seguridad de conexión** para abrir la página de seguridad de conexión de Azure Database for MariaDB.

2. Asegúrese de que el control Permitir el acceso a los servicios de Azure está configurado en**Desactivado**.

> [!Important]
> Si lo establece en Activado, el servidor de Azure MariaDB Database acepta la comunicación desde cualquier subred. Si deja el control establecido en Activado, el número de accesos podría ser excesivo desde un punto de vista de seguridad. La característica de punto de conexión del servicio Microsoft Azure Virtual Network, junto con la característica de la regla de red virtual de Azure Database for MariaDB, pueden reducir el área expuesta de seguridad.

3. A continuación, haga clic en **+ Agregar una red virtual existente**. Si no tiene ninguna red virtual, puede hacer clic en **+ Crear nueva red virtual** para crear una. Consulte [Guía de inicio rápido: Creación de una red virtual mediante Azure Portal](../virtual-network/quick-create-portal.md)

   ![Azure Portal: haga clic en Seguridad de conexión](./media/howto-manage-vnet-portal/1-connection-security.png)

4. Escriba un nombre de regla de red virtual, seleccione la suscripción, el nombre de la subred y red virtual y, luego, haga clic en **Habilitar**. Esto habilita automáticamente puntos de conexión de servicio de red virtual en la subred mediante la etiqueta de servicio **Microsoft.SQL**.

   ![Azure Portal: configuración de red virtual](./media/howto-manage-vnet-portal/2-configure-vnet.png)

   La cuenta debe tener todos los permisos necesarios para crear una red virtual y un punto de conexión de servicio.

   Los puntos de conexión de servicio pueden configurarse en redes virtuales de forma independiente por un usuario con acceso de escritura a la red virtual.
    
   Para proteger los recursos de servicio de Azure en una red virtual, el usuario debe tener permiso en "Microsoft.Network/virtualNetworks/subnets/joinViaServiceEndpoint/" para las subredes que se agregan. De forma predeterminada, este permiso se incluye en los roles de administrador de servicios integrado y puede modificarse mediante la creación de roles personalizados.
    
   Obtenga más información sobre los [roles integrados](https://docs.microsoft.com/azure/active-directory/role-based-access-built-in-roles) y la asignación de permisos específicos a [roles personalizados](https://docs.microsoft.com/azure/active-directory/role-based-access-control-custom-roles).
    
   Las redes virtuales y los recursos de servicio de Azure pueden encontrarse en la misma o en diferentes suscripciones. Si los recursos de servicio de Azure y de red virtual se encuentran en distintas suscripciones, los recursos deben estar en el mismo inquilino de Active Directory (AD). Asegúrese de que ambas suscripciones tengan registrado el proveedor de recursos **Microsoft.Sql**. Para más información, consulte [resource-manager-registration][resource-manager-portal].

   > [!IMPORTANT]
   > Se recomienda encarecidamente leer este artículo sobre las configuraciones y las consideraciones de puntos de conexión de servicio antes de configurarlos. **punto de conexión de servicio de red virtual:** un [punto de conexión de servicio de red virtual](../virtual-network/virtual-network-service-endpoints-overview.md) es una subred cuyos valores de propiedad incluyen uno o más nombres formales de tipo de servicio de Azure. Los puntos de conexión de servicio de red virtual usan el nombre de tipo de servicio **Microsoft.Sql**, que hace referencia al servicio de Azure denominado SQL Database. Esta etiqueta de servicio también se aplica a los servicios Azure SQL Database, Azure Database for MariaDB, PostgreSQL y MySQL. Es importante tener en cuenta que, al aplicar la etiqueta de servicio **Microsoft.Sql** a un punto de conexión del servicio de red virtual, se configura el tráfico del punto de conexión de servicio de todos los servicios de Azure Database, incluidos los servidores de Azure SQL Database, Azure Database for PostgreSQL, Azure Database for MariaDB y Azure Database for MySQL de la subred.
   > 

5. Una vez habilitada, haga clic en **Aceptar** y verá que se habilitan los puntos de conexión de servicio de red virtual junto con una regla de red virtual.

   ![Se han habilitado los puntos de conexión de servicio de red virtual y se ha creado la regla de red virtual](./media/howto-manage-vnet-portal/3-vnet-service-endpoints-enabled-vnet-rule-created.png)

## <a name="next-steps"></a>Pasos siguientes
- Obtenga más información sobre [configurar SSL en Azure Database for MariaDB](howto-configure-ssl.md).
- De forma similar, puede crear scripts para [habilitar puntos de conexión de servicio de red virtual y crear una regla de red virtual para Azure Database for MariaDB mediante la CLI de Azure](howto-manage-vnet-cli.md).

<!-- Link references, to text, Within this same GitHub repo. --> 
[resource-manager-portal]: ../azure-resource-manager/management/resource-providers-and-types.md