---
ms.service: logic-apps
author: ecfan
ms.author: estfan
ms.topic: include
ms.date: 11/08/2019
ms.openlocfilehash: ea0ae1b1527aa1f527c8ac8fbcd3b4e4f6b6fe2f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "74789210"
---
* Si está usando Azure SQL Database, siga los pasos que se indican en [Conexión a Azure SQL Database](#connect-azure-sql-db).

* Si usa SQL Server, siga los pasos que se indican en [Conexión con SQL Server](#connect-sql-server).

<a name="connect-azure-sql-db"></a>

### <a name="connect-to-azure-sql-database"></a>Conexión a Azure SQL Database

Cuando el desencadenador o la acción de SQL le pidan la información de conexión, siga estos pasos (sirven tanto para desencadenadores como para acciones):

1. En **Nombre de la conexión**, cree un nombre para la conexión.

1. En **Nombre de SQL Server**, seleccione el servidor de Azure SQL. Cuando aparezca la lista **SQL Database Name** (Nombre de SQL), seleccione la base de datos. Proporcione el nombre de usuario y la contraseña del servidor de Azure SQL.

   También puede encontrar esta información en Azure Portal en las propiedades de SQL Database o en la cadena de conexión:

   * `User ID=<your-user-name>`
   * `Password=<your-password>`

   ![Creación de una conexión a Azure SQL Database](./media/connectors-create-api-sqlazure/azure-sql-database-create-connection.png)

1. Seleccione **Crear** cuando haya terminado.

1. Después de crear la conexión, siga con los pasos [Incorporación de un desencadenador de SQL](#add-sql-trigger) o [Incorporación de una acción de SQL](#add-sql-action).

<a name="connect-sql-server"></a>

### <a name="connect-to-sql-server"></a>Conectar a SQL Server

Cuando el desencadenador o la acción de SQL le pidan la información de conexión, siga estos pasos (sirven tanto para desencadenadores como para acciones): En los escenarios en los que es necesario instalar la [puerta de enlace de datos local](https://docs.microsoft.com/azure/logic-apps/logic-apps-gateway-install) en un equipo local y [crear el recurso de puerta de enlace de datos de Azure](https://docs.microsoft.com/azure/logic-apps/logic-apps-gateway-connection), asegúrese de completar estos requisitos primero. De lo contrario, no aparecerá su recurso en la lista de puertas de enlace al crear la conexión.

Además, para usar autenticación de Windows con el conector de SQL Server en un [entorno de servicio de integración (ISE)](https://docs.microsoft.com/azure/logic-apps/connect-virtual-network-vnet-isolated-environment-overview), use la versión no ISE del conector y la puerta de enlace de datos local. La versión con etiqueta ISE no admite autenticación de Windows.

1. En **Nombre de la conexión**, cree un nombre para la conexión.

1. En el desencadenador o en la acción, seleccione **Conectar mediante puerta de enlace de datos local** para que aparezcan las opciones de SQL Server.

1. En **Nombre del servidor SQL** y **Nombre de la base de datos SQL**, proporcione la dirección del servidor SQL y asigne un nombre a la base de datos. En **Nombre de usuario** y **Contraseña**, proporcione el nombre de usuario y la contraseña del servidor.

   Esta información se puede encontrar también en la cadena de conexión:

   * `Server=<your-server-address>`
   * `Database=<your-database-name>`
   * `User ID=<your-user-name>`
   * `Password=<your-password>`

   ![Creación de una conexión a SQL Server](./media/connectors-create-api-sqlazure/sql-server-create-connection.png)

1. Si el servidor SQL Server usa la autenticación básica o de Windows, seleccione el **tipo de autenticación**.

1. En **Puertas de enlace**, seleccione la suscripción de Azure asociada a la puerta de enlace de datos local creada anteriormente y seleccione el nombre de la puerta de enlace de datos local.

   Si la puerta de enlace no aparece en la lista, compruebe que la [configuró](https://docs.microsoft.com/azure/logic-apps/logic-apps-gateway-connection) correctamente.

   ![Creación de una conexión a SQL Server finalizada](./media/connectors-create-api-sqlazure/sql-server-create-connection-complete.png)

1. Seleccione **Crear** cuando haya terminado.

1. Después de crear la conexión, continúe con [Incorporación del desencadenador de SQL](#add-sql-trigger) o [Incorporación de acción de SQL](#add-sql-action).
