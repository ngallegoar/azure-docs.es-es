---
title: 'Reglas de firewall: Azure Database for MariaDB'
description: Obtenga información sobre el uso de reglas de firewall para habilitar conexiones a su servidor de Azure Database for MariaDB.
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.topic: conceptual
ms.date: 3/18/2020
ms.openlocfilehash: 743e3f50d747993250399493d97fc2becab19319
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79532049"
---
# <a name="azure-database-for-mariadb-server-firewall-rules"></a>Reglas de firewall del servidor de Azure Database for MariaDB
Los firewalls impiden todo acceso al servidor de bases de datos, excepto a aquellos equipos a los que haya concedido permiso. Asimismo, otorgan acceso al servidor según la dirección IP de origen de cada solicitud.

Para configurar un firewall, cree reglas de firewall que especifiquen intervalos de direcciones IP aceptables. Puede crear reglas de firewall a nivel de servidor.

**Reglas de firewall:** estas reglas permiten a los clientes tener acceso a todo el servidor de Azure Database for MariaDB; es decir, a todas las bases de datos que se encuentren en el mismo servidor lógico. Las reglas de firewall de nivel de servidor pueden configurarse a través de Azure Portal o los comandos de la CLI de Azure. Para crear reglas de firewall en el nivel del servidor, debe ser propietario o colaborador de la suscripción.

## <a name="firewall-overview"></a>Información general de firewalls
De manera predeterminada, el firewall bloquea todos los accesos de base de datos al servidor de Azure Database for MariaDB. Para comenzar a usar el servidor desde otro equipo, debe especificar una o varias reglas de firewall a nivel de servidor para habilitar el acceso a su servidor. Use las reglas de firewall para especificar los intervalos de direcciones IP de Internet que se permitirán. La acción de las reglas de firewall no repercute sobre el acceso al propio sitio web de Azure Portal.

Los intentos de conexión desde Internet y Azure deben atravesar primero el firewall antes de alcanzar la base de datos de Azure Database for MariaDB, tal y como se muestra en el siguiente diagrama:

![Ejemplo de flujo de cómo funciona el firewall](./media/concepts-firewall-rules/1-firewall-concept.png)

## <a name="connecting-from-the-internet"></a>Conexión desde Internet
Las reglas de firewall a nivel de servidor se aplican a todas las bases de datos del servidor de Azure Database for MariaDB.

Si la dirección IP de la solicitud se encuentra dentro de uno de los intervalos especificados en las reglas de firewall de nivel de servidor, la conexión se concede.

Si la dirección IP de la solicitud se encuentra fuera de los intervalos especificados en cualquiera de las reglas de firewall de nivel de base de datos o de servidor, la solicitud de conexión genera un error.

## <a name="connecting-from-azure"></a>Conexión desde Azure
Se recomienda encontrar la dirección IP saliente de cualquier aplicación o servicio, y permitir explícitamente el acceso a las direcciones IP individuales o a los rangos. Por ejemplo, puede encontrar la dirección IP saliente de Azure App Service, o bien usar una IP pública asociada a una máquina virtual u otro recurso (a continuación encontrará información acerca de cómo conectar con la IP privada de una máquina virtual a través de puntos de conexión de servicio). 

Si no hay ninguna dirección IP saliente fija disponible para su servicio de Azure, puede habilitar las conexiones de todas las direcciones IP del centro de datos de Azure. Este valor se puede habilitar desde Azure Portal. Para ello, en la opción **Permitir el acceso a servicios de Azure**, seleccione **Activado** en el panel **Seguridad de conexión** y pulse **Guardar**. En la CLI de Azure, un valor de regla de firewall en que la dirección inicial y final sean 0.0.0.0 surte el mismo efecto. Si no se permite el intento de conexión, la solicitud no llega al servidor Azure Database for MariaDB.

> [!IMPORTANT]
> La opción **Permitir el acceso a servicios de Azure** configura el firewall para permitir todas las conexiones desde Azure, lo que incluye conexiones desde las suscripciones de otros clientes. Al seleccionar esta opción, asegúrese de que los permisos de usuario y el inicio de sesión limiten el acceso solamente a los usuarios autorizados.
> 

![Configurar el permiso de acceso a los servicios de Azure en el portal](./media/concepts-firewall-rules/allow-azure-services.png)

### <a name="connecting-from-a-vnet"></a>Conexión desde una red virtual
Para conectarse de forma segura a Azure Database for MariaDB desde una red virtual, considere la posibilidad de usar [puntos de conexión de servicio de red virtual](./concepts-data-access-security-vnet.md). 

## <a name="programmatically-managing-firewall-rules"></a>Administración mediante programación de reglas de firewall
Además de Azure Portal, las reglas de firewall se pueden administrar mediante programación con la CLI de Azure. 

Consulte también [Creación y administración de reglas de firewall de Azure Database for MariaDB mediante la CLI de Azure](./howto-manage-firewall-cli.md).

## <a name="troubleshooting-firewall-issues"></a>Solución de problemas de firewall
Tenga en cuenta los siguientes puntos cuando el acceso al servicio del servidor de Microsoft Azure Database for MariaDB no se comporte de la manera prevista:

* **Los cambios en la lista de permitidos aún no se han aplicado:** puede que se produzca un retraso de hasta cinco minutos hasta que se apliquen los cambios de configuración del firewall del servidor de Azure Database for MariaDB.

* **La cuenta de usuario no está autorizada o se ha usado una contraseña incorrecta:** si una cuenta de inicio de sesión no tiene los permisos en el servidor de Azure Database for MariaDB o la contraseña usada es incorrecta, se denegará la conexión al servidor de Azure Database for MariaDB. La creación de una configuración de firewall solo ofrece a los clientes una oportunidad de intentar conectarse al servidor; cada cliente debe ofrecer las credenciales de seguridad necesarias.

* **Dirección IP dinámica:** si tiene una conexión a Internet con direccionamiento IP dinámico y tiene problemas con el firewall, puede probar una de las soluciones siguientes:

   * Pida a su proveedor de acceso a Internet (ISP) el intervalo de direcciones IP asignado a los equipos cliente que acceden al servidor de Azure Database for MariaDB y agréguelo como regla de firewall.

   * Obtenga el direccionamiento IP estático en su lugar para los equipos cliente y luego agregue las direcciones IP como reglas de firewall.

* **La dirección IP del servidor parece pública:** Las conexiones con el servidor de Azure Database for MariaDB se enrutan mediante una puerta de enlace de Azure de acceso público. Sin embargo, la dirección IP real del servidor está protegida por el firewall. Para más información, consulte el [artículo de arquitectura de conectividad](concepts-connectivity-architecture.md). 

## <a name="next-steps"></a>Pasos siguientes
- [Create and manage Azure Database for MariaDB firewall rules using the Azure portal](./howto-manage-firewall-portal.md) (Creación y administración de reglas de firewall de Azure Database for MariaDB mediante Azure Portal)
- [Create and manage Azure Database for MariaDB firewall rules using Azure CLI](./howto-manage-firewall-cli.md) (Creación y administración de reglas de firewall de Azure Database for MariaDB mediante la CLI de Azure)
- [Puntos de conexión de servicio de red virtual en Azure Database for MariaDB](./concepts-data-access-security-vnet.md)
