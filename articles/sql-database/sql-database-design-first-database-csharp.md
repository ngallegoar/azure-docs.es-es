---
title: Diseño de la primera base de datos relacional con C#
description: Aprenda a diseñar su primera base de datos relacional en una base de datos única en Azure SQL Database con C# mediante ADO.NET.
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.custom: seo-lt-2019
ms.topic: tutorial
author: MightyPen
ms.author: genemi
ms.reviewer: carlrab
ms.date: 07/29/2019
ms.openlocfilehash: 0f1140bbefc7508666e763fcd4f1a04ba48cdfdd
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/24/2020
ms.locfileid: "75354953"
---
# <a name="tutorial-design-a-relational-database-in-a-single-database-within-azure-sql-database-cx23-and-adonet"></a>Tutorial: Diseño de una base de datos relacional en una base de datos única en Azure SQL Database con C# y ADO.NET

Azure SQL Database es una base de datos como servicio (DBaaS) relacional en Microsoft Cloud (Azure). En este tutorial se aprenderá a usar Azure Portal y ADO.NET con Visual Studio para:

> [!div class="checklist"]
> * Crear una base de datos única mediante Azure Portal*
> * Configurar una regla de firewall por IP de nivel de servidor mediante Azure Portal
> * Conectarse a la base de datos con ADO.NET y Visual Studio
> * Crear tablas con ADO.NET
> * Insertar, actualizar y eliminar datos con ADO.NET
> * Consultar datos con ADO.NET

*Si no tiene una suscripción a Azure, [cree una cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.

> [!TIP]
> El siguiente módulo de Microsoft Learn le ayuda a aprender gratis cómo [desarrollar y configurar una aplicación de ASP.net que consulta una instancia de Azure SQL Database](https://docs.microsoft.com/learn/modules/develop-app-that-queries-azure-sql/), incluida la creación de una base de datos simple.

## <a name="prerequisites"></a>Prerrequisitos

Una instalación de [Visual Studio 2019](https://www.visualstudio.com/downloads/) o posterior.

## <a name="create-a-blank-single-database"></a>Crear una base de datos única en blanco

Se crea una base de datos única en Azure SQL Database con un conjunto definido de recursos de proceso y almacenamiento. La base de datos se crea dentro de un [grupo de recursos de Azure](../azure-resource-manager/management/overview.md) y se administra mediante un [servidor de bases de datos](sql-database-servers.md).

Siga estos pasos para crear una base de datos única en blanco.

1. Haga clic en **Crear un recurso** en la esquina superior izquierda de Azure Portal.
2. En la página **Nuevo**, seleccione **Bases de datos** en la sección de Microsoft Azure Marketplace y, a continuación, haga clic en **SQL Database** en la sección **Destacados**.

   ![crear una base de datos en blanco](./media/sql-database-design-first-database/create-empty-database.png)

3. Rellene el formulario de **SQL Database** con la siguiente información, como se muestra en la imagen anterior:

    | Configuración       | Valor sugerido | Descripción |
    | ------------ | ------------------ | ------------------------------------------------- |
    | **Nombre de la base de datos** | *yourDatabase* | Para conocer los nombres de base de datos válidos, consulte [Identificadores de base de datos](/sql/relational-databases/databases/database-identifiers). |
    | **Suscripción** | *yourSubscription*  | Para más información acerca de sus suscripciones, consulte [Suscripciones](https://account.windowsazure.com/Subscriptions). |
    | **Grupos de recursos** | *yourResourceGroup* | Para conocer cuáles son los nombres de grupo de recursos válidos, consulte el artículo [Convenciones de nomenclatura](/azure/architecture/best-practices/resource-naming). |
    | **Seleccionar origen** | Base de datos en blanco | Especifica que se debe crear una base de datos en blanco. |

4. Haga clic en **Servidor** para usar un servidor de bases de datos existente o para crear y configurar uno nuevo. Seleccione un servidor existente o haga clic en **Crear un nuevo servidor** y rellene el formulario **Nuevo servidor** con la información siguiente:

    | Configuración       | Valor sugerido | Descripción |
    | ------------ | ------------------ | ------------------------------------------------- |
    | **Nombre del servidor** | Cualquier nombre globalmente único | Para conocer cuáles son los nombres de servidor válidos, consulte el artículo [Naming conventions](/azure/architecture/best-practices/resource-naming) (Convenciones de nomenclatura). |
    | **Inicio de sesión del administrador del servidor** | Cualquier nombre válido | Para conocer los nombres de inicio de sesión válidos, consulte [Identificadores de base de datos](/sql/relational-databases/databases/database-identifiers). |
    | **Contraseña** | Cualquier contraseña válida | La contraseña debe tener un mínimo de ocho caracteres y debe usar caracteres de tres de las siguientes categorías: caracteres en mayúsculas, caracteres en minúsculas, números y caracteres no alfanuméricos. |
    | **Ubicación** | Cualquier ubicación válida | Para obtener información acerca de las regiones, consulte [Regiones de Azure](https://azure.microsoft.com/regions/). |

    ![create database-server](./media/sql-database-design-first-database/create-database-server.png)

5. Haga clic en **Seleccionar**.
6. Haga clic en **Plan de tarifa** para especificar el nivel de servicio, el número de DTU o de núcleos virtuales y la cantidad de almacenamiento. Puede explorar las opciones del número de DTU o núcleos virtuales, y la cantidad de almacenamiento que están a su disposición para cada nivel de servicio.

    Después de seleccionar el nivel de servicio, el número de DTU o núcleos virtuales y la cantidad de almacenamiento, haga clic en **Aplicar**.

7. Introduzca una **intercalación** para la base de datos en blanco (para este tutorial, use el valor predeterminado). Para más información sobre las intercalaciones, vea [Collations](/sql/t-sql/statements/collations) (Intercalaciones)

8. Una vez completado el formulario de **SQL Database**, haga clic en **Crear** para aprovisionar la base de datos única. Esta operación puede tardar unos minutos.

9. En la barra de herramientas, haga clic en **Notificaciones** para supervisar el proceso de implementación.

   ![notificación](./media/sql-database-design-first-database/notification.png)

## <a name="create-a-server-level-ip-firewall-rule"></a>Creación de una regla de firewall de IP de nivel de servidor

El servicio SQL Database crea un firewall por IP en el nivel de servidor. Este firewall evita que las herramientas y aplicaciones externas se conecten al servidor o a las bases de datos de este, a menos que una regla de firewall permita sus direcciones IP. Para habilitar la conectividad externa a la base de datos única, primero debe agregar una regla de firewall para la dirección IP (o un intervalo de direcciones IP). Siga estos pasos para crear una [regla de firewall de IP de nivel de servidor de SQL Database](sql-database-firewall-configure.md).

> [!IMPORTANT]
> El servicio SQL Database se comunica a través del puerto 1433. Si intenta conectarse a este servicio desde dentro de una red corporativa, es posible que el firewall de la red no permita el tráfico de salida a través del puerto 1433. En ese caso, no puede conectarse a la base de datos única, salvo que el administrador abra el puerto 1433.

1. Cuando se haya finalizado la implementación, haga clic en **Bases de datos SQL** en el menú de la izquierda y, después, haga clic en *yourDatabase* en la página **Bases de datos SQL**. Se abre la página de información general de la base de datos, que muestra el **nombre del servidor completo** (por ejemplo, *sample-svr.database.windows.net*) y proporciona opciones para otras configuraciones.

2. Copie el nombre completo del servidor para conectarse a su servidor y a sus bases de datos de SQL Server Management Studio.

   ![nombre del servidor](./media/sql-database-design-first-database/server-name.png)

3. Haga clic en **Establecer el firewall del servidor** en la barra de herramientas. Se abrirá la página **Configuración del firewall** del servidor de SQL Database.

   ![Regla de firewall de IP en el nivel de servidor](./media/sql-database-design-first-database/server-firewall-rule.png)

4. Haga clic en **Agregar IP de cliente** en la barra de herramientas para agregar la dirección IP actual a la nueva regla de firewall por IP. La regla de firewall de IP puede abrir el puerto 1433 para una única dirección IP o un intervalo de direcciones IP.

5. Haga clic en **Save**(Guardar). Se crea una regla de firewall de IP en el nivel de servidor para el puerto 1433 de la dirección IP actual en el servidor de SQL Database.

6. Haga clic en **Aceptar** y después cierre la página **Configuración de firewall**.

Ahora la dirección IP puede pasar a través del firewall de IP; además, puede conectarse a la base de datos única mediante SQL Server Management Studio u otra herramienta que elija. Asegúrese de usar la cuenta de administración de servidor que creó anteriormente.

> [!IMPORTANT]
> De forma predeterminada, el acceso a través del firewall por IP de SQL Database está habilitado para todos los servicios de Azure. Haga clic en **OFF** en esta página para deshabilitar todos los servicios de Azure.

[!INCLUDE [sql-database-csharp-adonet-create-query-2](../../includes/sql-database-csharp-adonet-create-query-2.md)]

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, aprendió tareas básicas de las bases de datos como crear una base de datos y tablas, conectarse a la base de datos, cargar datos y ejecutar consultas. Ha aprendido a:

> [!div class="checklist"]
> * Crear una base de datos
> * Configurar una regla de firewall
> * Conectarse a la base de datos con [Visual Studio y C#](sql-database-connect-query-dotnet-visual-studio.md)
> * Crear tablas.
> * Insertar, actualizar, eliminar y consultar los datos

Prosiga con el tutorial siguiente para aprender sobre la migración de datos.

> [!div class="nextstepaction"]
> [Migración de SQL Server a Azure SQL Database sin conexión mediante DMS](../dms/tutorial-sql-server-to-azure-sql.md)
