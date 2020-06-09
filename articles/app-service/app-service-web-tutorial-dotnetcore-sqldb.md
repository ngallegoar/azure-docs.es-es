---
title: 'Tutorial: ASP.NET Core con SQL Database'
description: Aprenda a poner en funcionamiento una aplicación .NET Core en Azure App Service, con conexión a SQL Database.
ms.devlang: dotnet
ms.topic: tutorial
ms.date: 04/23/2020
ms.custom: mvc, cli-validate, seodec18
ms.openlocfilehash: 4b5c78313eddd50441dbd47f556d2dbef03feefc
ms.sourcegitcommit: 69156ae3c1e22cc570dda7f7234145c8226cc162
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/03/2020
ms.locfileid: "84310400"
---
# <a name="tutorial-build-an-aspnet-core-and-sql-database-app-in-azure-app-service"></a>Tutorial: Compilación de una aplicación ASP.NET Core y SQL Database en Azure App Service

> [!NOTE]
> En este artículo se implementa una aplicación en App Service en Windows. Para implementar App Service en _Linux_, consulte [Compilación de una aplicación .NET Core y SQL Database en Azure App Service en Linux](./containers/tutorial-dotnetcore-sqldb-app.md).
>

[App Service](overview.md) proporciona un servicio de hospedaje web muy escalable y con aplicación de revisiones de un modo automático en Azure. En este tutorial se muestra cómo crear una aplicación .NET Core y conectarla a SQL Database. Cuando termine, tendrá una aplicación .NET Core con MVC en ejecución en App Service.

![aplicación que se ejecuta en App Service](./media/app-service-web-tutorial-dotnetcore-sqldb/azure-app-in-browser.png)

En este tutorial, aprenderá a:

> [!div class="checklist"]
>
> * Creación de una base de datos de Azure SQL Database
> * Conectar una aplicación .NET Core a SQL Database
> * Implementar la aplicación en Azure
> * Actualizar el modelo de datos y volver a implementar la aplicación
> * Transmitir registros de diagnóstico desde Azure
> * Administrar la aplicación en Azure Portal

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Requisitos previos

Para completar este tutorial:

* <a href="https://git-scm.com/" target="_blank">Instalación de Git</a>
* <a href="https://dotnet.microsoft.com/download/dotnet-core/3.1" target="_blank">Instalación del SDK más reciente de .NET Core 3.1</a>

## <a name="create-local-net-core-app"></a>Creación de una aplicación .NET Core local

En este paso, configurará el proyecto .NET Core local.

### <a name="clone-the-sample-application"></a>Clonación de la aplicación de ejemplo

En la ventana del terminal, use `cd` para cambiar a un directorio de trabajo.

Ejecute los comandos siguientes para clonar el repositorio de ejemplo y cambiar a su raíz.

```bash
git clone https://github.com/azure-samples/dotnetcore-sqldb-tutorial
cd dotnetcore-sqldb-tutorial
```

El proyecto de ejemplo contiene una aplicación básica CRUD (crear, leer, actualizar, eliminar) que usa [Entity Framework Core](https://docs.microsoft.com/ef/core/).

### <a name="run-the-application"></a>Ejecución de la aplicación

Ejecute los comandos siguientes para instalar los paquetes necesarios, ejecutar las migraciones de bases de datos e iniciar la aplicación.

```bash
dotnet tool install -g dotnet-ef
dotnet ef database update
dotnet run
```

Vaya a `http://localhost:5000` en un explorador. Seleccione el vínculo **Crear nuevo** y cree un par de elementos de _tareas pendientes_.

![Se conecta correctamente a SQL Database](./media/app-service-web-tutorial-dotnetcore-sqldb/local-app-in-browser.png)

Para detener .NET Core en cualquier momento, presione `Ctrl+C` en el terminal.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="create-a-database-in-azure-sql-database"></a>Creación de una base de datos de Azure SQL Database

En este paso, creará una base de datos en [Azure SQL Database](/azure/sql-database/). Cuando la aplicación se implementa en Azure, utiliza esta base de datos.

### <a name="create-a-resource-group"></a>Crear un grupo de recursos

[!INCLUDE [Create resource group](../../includes/app-service-web-create-resource-group-no-h.md)]

### <a name="create-a-server-in-azure-sql-database"></a>Creación de un servidor en Azure SQL Database

En Cloud Shell, cree un [servidor](../azure-sql/database/logical-servers.md) en Azure SQL Database mediante el comando [`az sql server create`](/cli/azure/sql/server?view=azure-cli-latest#az-sql-server-create). Un servidor es una estructura lógica que contiene un conjunto de bases de datos administradas como un grupo.

Reemplace el marcador de posición *\<server-name>* por un nombre *único*. Este nombre se usa como parte del punto de conexión SQL Database único global `<server-name>.database.windows.net`. Los caracteres válidos son `a`-`z`, `0`-`9`, `-`. Reemplace también *\<db-username>* y *\<db-password>* por un nombre de usuario y una contraseña de su elección.

```azurecli-interactive
az sql server create --name <server-name> --resource-group myResourceGroup --location "West Europe" --admin-user <db-username> --admin-password <db-password>
```

Cuando se crea el servidor, la CLI de Azure muestra información similar a la del siguiente ejemplo:

<pre>
{
  "administratorLogin": "&lt;db-username&gt;",
  "administratorLoginPassword": null,
  "fullyQualifiedDomainName": "&lt;server-name&gt;.database.windows.net",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Sql/servers/&lt;server-name&gt;",
  "identity": null,
  "kind": "v12.0",
  "location": "westeurope",
  "name": "&lt;server-name&gt;",
  "resourceGroup": "myResourceGroup",
  "state": "Ready",
  "tags": null,
  "type": "Microsoft.Sql/servers",
  "version": "12.0"
}
</pre>

### <a name="configure-a-server-firewall-rule"></a>Configuración de una regla de firewall del servidor

Creación de una [regla de firewall en el nivel de servidor](../azure-sql/database/firewall-configure.md) mediante el comando [`az sql server firewall create`](/cli/azure/sql/server/firewall-rule?view=azure-cli-latest#az-sql-server-firewall-rule-create). Cuando tanto la dirección IP de inicio como final están establecidas en 0.0.0.0., el firewall solo se abre para otros recursos de Azure.

```azurecli-interactive
az sql server firewall-rule create --resource-group myResourceGroup --server <server-name> --name AllowAzureIps --start-ip-address 0.0.0.0 --end-ip-address 0.0.0.0
```

> [!TIP]
> Puede ser incluso más restrictivo con su regla de firewall [usando solo las direcciones IP de salida que utiliza su aplicación](overview-inbound-outbound-ips.md#find-outbound-ips).

En Cloud Shell, ejecute de nuevo el comando para permitir el acceso desde el equipo local y reemplace *\<your-ip-address>* por su [dirección IP IPv4 local](https://www.whatsmyip.org/).

```azurecli-interactive
az sql server firewall-rule create --name AllowLocalClient --server <server_name> --resource-group myResourceGroup --start-ip-address=<your-ip-address> --end-ip-address=<your-ip-address>
```

### <a name="create-a-database-in-azure-sql-database"></a>Creación de una base de datos de Azure SQL Database

Cree una base de datos con un [nivel de rendimiento S0](../azure-sql/database/service-tiers-dtu.md) en el servidor con el comando [`az sql db create`](/cli/azure/sql/db?view=azure-cli-latest#az-sql-db-create).

```azurecli-interactive
az sql db create --resource-group myResourceGroup --server <server-name> --name coreDB --service-objective S0
```

### <a name="create-connection-string"></a>Creación de la cadena de conexión

Obtenga la cadena de conexión mediante el comando [`az sql db show-connection-string`](/cli/azure/sql/db?view=azure-cli-latest#az-sql-db-show-connection-string).

```azurecli-interactive
az sql db show-connection-string --client ado.net --server <server-name> --name coreDB
```

En la salida del comando, reemplace *\<username>* y *\<password>* por las credenciales de administrador de base de datos que usó anteriormente.

Esta es la cadena de conexión de la aplicación .NET Core. Cópiela para usarla más adelante.

### <a name="configure-app-to-connect-to-the-database-in-azure"></a>Configuración de la aplicación para conectarse a la base de datos de Azure

En el repositorio local, abra Startup.cs y busque el código siguiente:

```csharp
services.AddDbContext<MyDatabaseContext>(options =>
        options.UseSqlite("Data Source=localdatabase.db"));
```

Reemplácelo por el código que verá a continuación.

```csharp
services.AddDbContext<MyDatabaseContext>(options =>
        options.UseSqlServer(Configuration.GetConnectionString("MyDbConnection")));
```

> [!IMPORTANT]
> Para las aplicaciones de producción que necesiten escalar horizontalmente, siga los procedimientos recomendados en [Aplicación de migraciones en producción](/aspnet/core/data/ef-rp/migrations#applying-migrations-in-production).

### <a name="run-database-migrations-to-the-database-in-azure"></a>Ejecución de migraciones de bases de datos a la base de datos de Azure

Actualmente, la aplicación se conecta a una base de datos SQLite local. Ahora que ha configurado una instancia de Azure SQL Database, vuelva a crear la migración inicial para establecerla como destino.

En la raíz del repositorio, ejecute los siguientes comandos. Reemplace *\<connection-string>* por la cadena de conexión que creó anteriormente.

```
# Delete old migrations
rm Migrations -r
# Recreate migrations
dotnet ef migrations add InitialCreate

# Set connection string to production database
# PowerShell
$env:ConnectionStrings:MyDbConnection="<connection-string>"
# CMD (no quotes)
set ConnectionStrings:MyDbConnection=<connection-string>
# Bash (no quotes)
export ConnectionStrings__MyDbConnection=<connection-string>

# Run migrations
dotnet ef database update
```

### <a name="run-app-with-new-configuration"></a>Ejecución de la aplicación con una configuración nueva

Ahora que las migraciones de base de datos se ejecutan en la base de datos de producción, pruebe la aplicación mediante la ejecución de:

```
dotnet run
```

Vaya a `http://localhost:5000` en un explorador. Seleccione el vínculo **Crear nuevo** y cree un par de elementos de _tareas pendientes_. La aplicación ahora lee y escribe datos en la base de datos de producción.

Guarde los cambios y confírmelos en el repositorio de Git.

```bash
git add .
git commit -m "connect to SQLDB in Azure"
```

Ahora está preparado para implementar el código.

## <a name="deploy-app-to-azure"></a>Implementación de la aplicación en Azure

En este paso, va a implementar la aplicación .NET Core conectada a SQL Database en App Service.

### <a name="configure-local-git-deployment"></a>Configuración de la implementación de Git local

[!INCLUDE [Configure a deployment user](../../includes/configure-deployment-user-no-h.md)]

### <a name="create-an-app-service-plan"></a>Creación de un plan de App Service

[!INCLUDE [Create app service plan](../../includes/app-service-web-create-app-service-plan-no-h.md)]

### <a name="create-a-web-app"></a>Creación de una aplicación web

[!INCLUDE [Create web app](../../includes/app-service-web-create-web-app-dotnetcore-win-no-h.md)]

### <a name="configure-connection-string"></a>Configuración de la cadena de conexión

Para establecer las cadenas de conexión de la aplicación de Azure, use el comando [`az webapp config appsettings set`](/cli/azure/webapp/config/appsettings?view=azure-cli-latest#az-webapp-config-appsettings-set) en Cloud Shell. En el comando siguiente, reemplace *\<app-name>* y el parámetro *\<connection-string>* por la cadena de conexión que creó anteriormente.

```azurecli-interactive
az webapp config connection-string set --resource-group myResourceGroup --name <app-name> --settings MyDbConnection="<connection-string>" --connection-string-type SQLAzure
```

En ASP.NET Core, puede usar esta cadena de conexión con nombre (`MyDbConnection`) con el patrón estándar, como cualquier cadena de conexión especificada en *appsettings.jon*. En este caso, `MyDbConnection` también se define en *appsettings.json*. Cuando se ejecuta en App Service, la cadena de conexión definida en App Service tiene prioridad sobre la definida en *appsettings.json*. El código usa el valor de *appsettings.json* durante el desarrollo local y el mismo código usa el valor de App Service cuando se implementa.

Para ver cómo se hace referencia a la cadena de conexión en el código, consulte [Configuración de la aplicación para conectarse a la base de datos de producción](#configure-app-to-connect-to-the-database-in-azure).

### <a name="push-to-azure-from-git"></a>Inserción en Azure desde Git

[!INCLUDE [push-to-azure-no-h](../../includes/app-service-web-git-push-to-azure-no-h.md)]

<pre>
Enumerating objects: 268, done.
Counting objects: 100% (268/268), done.
Compressing objects: 100% (171/171), done.
Writing objects: 100% (268/268), 1.18 MiB | 1.55 MiB/s, done.
Total 268 (delta 95), reused 251 (delta 87), pack-reused 0
remote: Resolving deltas: 100% (95/95), done.
remote: Updating branch 'master'.
remote: Updating submodules.
remote: Preparing deployment for commit id '64821c3558'.
remote: Generating deployment script.
remote: Project file path: .\DotNetCoreSqlDb.csproj
remote: Generating deployment script for ASP.NET MSBuild16 App
remote: Generated deployment script files
remote: Running deployment command...
remote: Handling ASP.NET Core Web Application deployment with MSBuild16.
remote: .
remote: .
remote: .
remote: Finished successfully.
remote: Running post deployment command(s)...
remote: Triggering recycle (preview mode disabled).
remote: App container will begin restart within 10 seconds.
To https://&lt;app-name&gt;.scm.azurewebsites.net/&lt;app-name&gt;.git
 * [new branch]      master -> master
</pre>

### <a name="browse-to-the-azure-app"></a>Navegación hasta la aplicación de Azure

Vaya a la aplicación implementada mediante el explorador web.

```bash
http://<app-name>.azurewebsites.net
```

Agregue algunos elementos de tareas pendientes.

![aplicación que se ejecuta en App Service](./media/app-service-web-tutorial-dotnetcore-sqldb/azure-app-in-browser.png)

**¡Enhorabuena!** Está ejecutando una aplicación .NET Core controlada por datos en App Service.

## <a name="update-locally-and-redeploy"></a>Actualización local y nueva implementación

En este paso, modificará el esquema de la base de datos y lo publicará en Azure.

### <a name="update-your-data-model"></a>Actualización del modelo de datos

Abra _Models\Todo.cs_ en el editor de código. Agregue la siguiente propiedad a la clase `ToDo`:

```csharp
public bool Done { get; set; }
```

### <a name="rerun-database-migrations"></a>Nueva ejecución de migraciones de base de datos

Ejecute algunos comandos para realizar actualizaciones en la base de datos de producción.

```bash
dotnet ef migrations add AddProperty
dotnet ef database update
```

### <a name="use-the-new-property"></a>Uso de la nueva propiedad

Realice algunos cambios en el código para usar la propiedad `Done`. Para simplificar, en este tutorial va a cambiar las vistas `Index` y `Create` para ver la propiedad en acción.

Abra _Controllers\TodosController.cs_.

Busque el método `Create([Bind("ID,Description,CreatedDate")] Todo todo)` y agregue `Done` a la lista de propiedades en el atributo `Bind`. Cuando haya terminado, la firma del método `Create()` se parecerá al código siguiente:

```csharp
public async Task<IActionResult> Create([Bind("ID,Description,CreatedDate,Done")] Todo todo)
```

Abra _Views\Todos\Create.cshtml_.

En el código Razor debería ver un elemento `<div class="form-group">` para `Description` y, luego, otro elemento `<div class="form-group">` para `CreatedDate`. Inmediatamente después de estos dos elementos, agregue otro elemento `<div class="form-group">` para `Done`:

```csharp
<div class="form-group">
    <label asp-for="Done" class="col-md-2 control-label"></label>
    <div class="col-md-10">
        <input asp-for="Done" class="form-control" />
        <span asp-validation-for="Done" class="text-danger"></span>
    </div>
</div>
```

Abra _Views\Todos\Index.cshtml_.

Busque el elemento vacío `<th></th>`. Justo encima de este elemento, agregue el siguiente código Razor:

```csharp
<th>
    @Html.DisplayNameFor(model => model.Done)
</th>
```

Busque el elemento `<td>` que contiene los asistentes de etiquetas `asp-action`. Justo encima de este elemento, agregue el siguiente código Razor:

```csharp
<td>
    @Html.DisplayFor(modelItem => item.Done)
</td>
```

Eso es todo lo que necesita para ver los cambios en las vistas `Index` y `Create`.

### <a name="test-your-changes-locally"></a>Prueba de los cambios localmente

Ejecute localmente la aplicación.

```bash
dotnet run
```

Abra el explorador y vaya a `http://localhost:5000/`. Ahora puede agregar una tarea pendiente y marcar **Listo**. A continuación se debería mostrar en su página principal como un elemento completado. Recuerde que la vista `Edit` no muestra el campo `Done`, dado que no cambió la vista `Edit`.

### <a name="publish-changes-to-azure"></a>Publicación de los cambios en Azure

```bash
git add .
git commit -m "added done field"
git push azure master
```

Una vez que `git push` esté completo, vaya a la aplicación de App Service e intente agregar un elemento de tarea y active **Listo**.

![Aplicación de Azure después de Migraciones de Code First](./media/app-service-web-tutorial-dotnetcore-sqldb/this-one-is-done.png)

Aún se muestran todas las tareas pendientes existentes. Cuando vuelva a publicar la aplicación ASP.NET Core, no se perderán los datos existentes en la instancia de SQL Database. Además, las migraciones de Entity Framework Core solo cambia el esquema de datos y deja intactos los datos existentes.

## <a name="stream-diagnostic-logs"></a>Transmisión de registros de diagnóstico

Mientras la aplicación ASP.NET Core se ejecuta en Azure App Service, puede hacer que los registros de la consola se canalicen a Cloud Shell. De este modo, puede obtener los mismos mensajes de diagnóstico para ayudarle a depurar errores de la aplicación.

El proyecto de ejemplo ya sigue las instrucciones indicadas en [Registro de ASP.NET Core en Azure](https://docs.microsoft.com/aspnet/core/fundamentals/logging#azure-app-service-provider) con dos cambios de configuración:

* Incluye una referencia a `Microsoft.Extensions.Logging.AzureAppServices` en *DotNetCoreSqlDb.csproj*.
* Llama a `loggerFactory.AddAzureWebAppDiagnostics()` en *Program.cs*.

Para establecer el [nivel de registro](https://docs.microsoft.com/aspnet/core/fundamentals/logging#log-level) de ASP.NET Core en App Service en `Information` desde el nivel predeterminado `Error`, utilice el comando [`az webapp log config`](/cli/azure/webapp/log?view=azure-cli-latest#az-webapp-log-config) en Cloud Shell.

```azurecli-interactive
az webapp log config --name <app-name> --resource-group myResourceGroup --application-logging true --level information
```

> [!NOTE]
> El nivel de registro del proyecto ya está establecido en `Information` en *appsettings.json*.

Para iniciar la transmisión del registro, use el comando [`az webapp log tail`](/cli/azure/webapp/log?view=azure-cli-latest#az-webapp-log-tail) en Cloud Shell.

```azurecli-interactive
az webapp log tail --name <app-name> --resource-group myResourceGroup
```

Cuando las secuencias de registro se inicien, actualice la aplicación de Azure en el explorador para obtener tráfico web. Ahora puede ver los registros de la consola canalizados al terminal. Si no ve los registros de la consola de inmediato, vuelve a comprobarlo en 30 segundos.

Para detener el streaming del registro en cualquier momento, escriba `Ctrl`+`C`.

Para más información acerca de cómo personalizar los registros de ASP.NET Core, consulte [Registro en ASP.NET Core](https://docs.microsoft.com/aspnet/core/fundamentals/logging).

## <a name="manage-your-azure-app"></a>Administración de la aplicación de Azure

Para ver la aplicación que ha creado, en [Azure Portal](https://portal.azure.com), busque y seleccione **App Services**.

![Selección de App Services en Azure Portal](./media/app-service-web-tutorial-dotnetcore-sqldb/app-services.png)

En la página **App Services**, seleccione el nombre de la aplicación de Azure.

![Navegación en el portal a la aplicación de Azure](./media/app-service-web-tutorial-dotnetcore-sqldb/access-portal.png)

De manera predeterminada, el portal muestra la página **Información general** de la aplicación. Esta página proporciona una visión del funcionamiento de la aplicación. En este caso, también puede realizar tareas de administración básicas como examinar, detener, iniciar, reiniciar y eliminar. Las pestañas del lado izquierdo de la página muestran las diferentes páginas de configuración que puede abrir.

![Página de App Service en Azure Portal](./media/app-service-web-tutorial-dotnetcore-sqldb/web-app-blade.png)

[!INCLUDE [cli-samples-clean-up](../../includes/cli-samples-clean-up.md)]

<a name="next"></a>

## <a name="next-steps"></a>Pasos siguientes

¿Qué ha aprendido?

> [!div class="checklist"]
>
> * Creación de una base de datos de Azure SQL Database
> * Conectar una aplicación .NET Core a SQL Database
> * Implementar la aplicación en Azure
> * Actualizar el modelo de datos y volver a implementar la aplicación
> * Transmitir registros desde Azure a un terminal
> * Administrar la aplicación en Azure Portal

Vaya al siguiente tutorial para aprender a asignar un nombre DNS personalizado a una aplicación web.

> [!div class="nextstepaction"]
> [Tutorial: Asignar un nombre DNS personalizado a la aplicación](app-service-web-tutorial-custom-domain.md)
