---
title: 'Tutorial: Implementación de Python (Django) con Postgres'
description: Aprenda a crear una aplicación de Python con una base de datos de PostgreSQL y a implementarla en Azure App Service en Linux. En el tutorial se usa una aplicación de ejemplo Django para la demostración.
ms.devlang: python
ms.topic: tutorial
ms.date: 04/14/2020
ms.custom:
- mvc
- seodec18
- seo-python-october2019
- cli-validate
ms.openlocfilehash: 0c9329b46d096df1afab6f7e457d143f9c6504be
ms.sourcegitcommit: 09a124d851fbbab7bc0b14efd6ef4e0275c7ee88
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2020
ms.locfileid: "82085763"
---
# <a name="tutorial-deploy-a-python-django-web-app-with-postgresql-in-azure-app-service"></a>Tutorial: Implementación de una aplicación web Python (Django) con PostgreSQL en Azure App Service

En este tutorial se muestra cómo implementar una aplicación web Python (Django) controlada por datos en [Azure App Service](app-service-linux-intro.md) y cómo conectarla a una base de datos de Azure Database for PostgreSQL. App Service proporciona un servicio de hospedaje web muy escalable y con aplicación de revisiones de un modo automático.

![Despliegue de una aplicación web Python (Django) en Azure App Service](./media/tutorial-python-postgresql-app/deploy-python-django-app-in-azure.png)

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Crear una base de datos de Azure Database for PostgreSQL
> * Implementar código en Azure App Service y conectarse a Postgres
> * Actualizar el código y volver a implementarlo
> * Visualización de los registros de diagnóstico
> * Administrar la aplicación web en Azure Portal

Puede seguir los pasos de este artículo en macOS, Linux o Windows.

## <a name="install-dependencies"></a>Instalar dependencias

Antes de comenzar este tutorial:

- [!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]
- Instale la [CLI de Azure](/cli/azure/install-azure-cli).
- Instale [Git](https://git-scm.com/).
- Instale [Python 3](https://www.python.org/downloads/).

## <a name="clone-the-sample-app"></a>Clonación de la aplicación de ejemplo

En una ventana de terminal, ejecute los siguientes comandos para clonar el repositorio de la aplicación de ejemplo y cambiar a la raíz del repositorio:

```
git clone https://github.com/Azure-Samples/djangoapp
cd djangoapp
```

El repositorio de ejemplo djangoapp contiene la aplicación [Django](https://www.djangoproject.com/) de sondeos controlada por datos que se obtiene al seguir [Creación de la primera aplicación Django](https://docs.djangoproject.com/en/2.1/intro/tutorial01/) de la documentación de Django. Se proporciona aquí para su comodidad.

## <a name="prepare-app-for-app-service"></a>Preparación de la aplicación para App Service

Al igual que sucede con muchos marcos web de Python, Django [requiere ciertos cambios antes de que se puedan ejecutar en un servidor de producción](https://docs.djangoproject.com/en/2.1/howto/deployment/checklist/); sucede lo mismo con App Service. Para que la aplicación funcione después de implementarla en App Service, debe cambiar y agregar algunas opciones de configuración en el archivo predeterminado *azuresite/settings.py*. 

Examine *azuresite/production.py*, que realiza la configuración necesaria para App Service. En resumen, lleva a cabo las operaciones siguientes:

- Hereda toda la configuración de *azuresite/settings.py*.
- Agrega el nombre de dominio completo de la aplicación de App Service a los hosts permitidos. 
- Usa [WhiteNoise](https://whitenoise.evans.io/en/stable/) para habilitar el servicio de archivos estáticos en producción, ya que Django de forma predeterminada no atiende a estos archivos. El paquete WhiteNoise ya está incluido en el archivo *requirements.txt*.
- Agrega la configuración de la base de datos PostgreSQL. De forma predeterminada, Django usa Sqlite3 como base de datos, pero esta opción no es adecuada para las aplicaciones de producción. El paquete [psycopg2-binary](https://pypi.org/project/psycopg2-binary/) ya está incluido en el archivo *requirements.txt*.
- La configuración de Postgres usa variables de entorno. Más adelante, verá cómo establecer variables de entorno en App Service.

El archivo *azuresite/production.py* se incluye en el repositorio por comodidad, pero todavía no se usa en la aplicación. Para asegurarse de que su configuración se usa en App Service, debe configurar dos archivos, *manage.py* y *azuresite/wsgi.py*, de forma que accedan a él.

- En *manage.py*, cambie la línea siguiente:

    <pre>
    os.environ.setdefault('DJANGO_SETTINGS_MODULE', 'azuresite.settings')
    </pre>

    Para el código siguiente:

    ```python
    if os.environ.get('DJANGO_ENV') == 'production':
        os.environ.setdefault('DJANGO_SETTINGS_MODULE', 'azuresite.production')
    else:
        os.environ.setdefault('DJANGO_SETTINGS_MODULE', 'azuresite.settings')
    ```

    Podrá establecer la variable de entorno `DJANGO_ENV` más adelante, cuando configure la aplicación de App Service.

- En *azuresite/wsgi.py*, realice el mismo cambio que en el paso anterior.

    En App Service, se usa *manage.py* para ejecutar migraciones de base de datos, mientras que App Service usa *azuresite/wsgi.py* para ejecutar la aplicación Django en producción. El cambio en ambos archivos garantiza que la configuración de producción se use en ambos casos.

## <a name="sign-in-to-azure-cli"></a>Inicio de sesión en la CLI de Azure

Debe tener ya instalada la CLI de Azure. La [CLI de Azure](/cli/azure/what-is-azure-cli) permite trabajar con recursos de Azure desde el terminal de línea de comandos. 

Para iniciar sesión en Azure, ejecute el comando [`az login`](/cli/azure/reference-index#az-login):

```azurecli
az login
```

Siga las instrucciones del terminal para iniciar sesión en la cuenta de Azure. Cuando haya terminado, se mostrarán las suscripciones en formato JSON en la salida de terminal.

## <a name="create-postgres-database-in-azure"></a>Creación de una base de datos de Postgres en Azure

<!-- > [!NOTE]
> Before you create an Azure Database for PostgreSQL server, check which [compute generation](/azure/postgresql/concepts-pricing-tiers#compute-generations-and-vcores) is available in your region. If your region doesn't support Gen4 hardware, change *--sku-name* in the following command line to a value that's supported in your region, such as B_Gen4_1.  -->

En esta sección, creará un servidor y una base de datos de Azure Database for PostgreSQL. Para empezar, instale la extensión `db-up` con el siguiente comando:

```azurecli
az extension add --name db-up
```

Cree la base de datos Postgres en Azure con el comando [`az postgres up`](/cli/azure/ext/db-up/postgres#ext-db-up-az-postgres-up), como se muestra en el ejemplo siguiente. Reemplace *\<postgresql-name>* con un nombre *único* (el punto de conexión del servidor es *https://\<postgresql-name>.postgres.database.azure.com*). Como valores de *\<admin-username >* y *\<admin-password >* , especifique las credenciales para crear un usuario administrador de este servidor Postgres.

<!-- Issue: without --location -->
```azurecli
az postgres up --resource-group myResourceGroup --location westus2 --server-name <postgresql-name> --database-name pollsdb --admin-user <admin-username> --admin-password <admin-password> --ssl-enforcement Enabled
```

Este comando puede tardar un poco porque está llevando a cabo las siguientes operaciones:

- Crea un [grupo de recursos](../../azure-resource-manager/management/overview.md#terminology) denominado `myResourceGroup`, si no existe. Cada recurso de Azure debe estar incluido en uno de ellos. `--resource-group` es opcional.
- Crea un servidor de Postgres con el usuario administrativo.
- Crea una base de datos `pollsdb`.
- Permite el acceso desde la dirección IP local.
- Permite el acceso desde servicios de Azure.
- Crea un usuario de base de datos con acceso a la base de datos `pollsdb`.

Puede realizar todos los pasos por separado con otros comandos de `az postgres` y `psql`, pero `az postgres up` realiza todos ellos de una vez.

Cuando finalice el comando, busque las líneas de salida que empiecen por `Ran Database Query:`. Muestran el usuario de base de datos que se crea automáticamente, con el nombre de usuario `root` y la contraseña `Pollsdb1`. Los usará más adelante para conectar la aplicación a la base de datos.

<!-- not all locations support az postgres up -->
> [!TIP]
> Para especificar la ubicación del servidor de Postgres, incluya el argumento `--location <location-name>`, donde `<location_name>` es una de las [regiones de Azure](https://azure.microsoft.com/global-infrastructure/regions/). Puede obtener las regiones disponibles para su suscripción con el comando [`az account list-locations`](/cli/azure/account#az-account-list-locations).

## <a name="deploy-the-app-service-app"></a>Implementación de la aplicación de App Service

En esta sección, creará la aplicación de App Service. Conectará esta aplicación a la base de datos Postgres que ha creado e implementará el código.

### <a name="create-the-app-service-app"></a>Creación de la aplicación de App Service

<!-- validation error: Parameter 'ResourceGroup.location' can not be None. -->
<!-- --resource-group is not respected at all -->

Asegúrese de volver a la raíz del repositorio (`djangoapp`), ya que la aplicación se implementará desde este directorio.

Cree una aplicación de App Service con el comando [`az webapp up`](/cli/azure/webapp#az-webapp-up), como se muestra en el ejemplo siguiente. Reemplace *\<app-name>* con un nombre *único* (el punto de conexión del servidor es *https://\<app-name>.azurewebsites.net*). Los caracteres permitidos para *\<app-name>* son `A`-`Z`, `0`-`9` y `-`.

```azurecli
az webapp up --plan myAppServicePlan --sku B1 --name <app-name>
```
<!-- !!! without --sku creates PremiumV2 plan!! -->

Este comando puede tardar un poco porque está llevando a cabo las siguientes operaciones:

<!-- - Create the resource group if it doesn't exist. `--resource-group` is optional. -->
<!-- No it doesn't. az webapp up doesn't respect --resource-group -->
- Genera un [grupo de recursos](../../azure-resource-manager/management/overview.md#terminology) automáticamente.
- Crea el [plan de App Service](../overview-hosting-plans.md) *myAppServicePlan* en el plan de tarifa Básico (B1), si no existe. `--plan` y `--sku` son opcionales.
- Crea la aplicación de App Service, si no existe.
- Habilita el registro predeterminado de la aplicación, si aún no está habilitado.
- Carga el repositorio mediante la implementación de ZIP con la automatización de compilación habilitada.

Una vez finalizada la implementación, verá una salida JSON similar a la siguiente:

<pre>
{
  "URL": "http://&lt;app-name&gt;.azurewebsites.net",
  "appserviceplan": "myAppServicePlan",
  "location": "westus",
  "name": "&lt;app-name&gt;",
  "os": "Linux",
  "resourcegroup": "&lt;app-resource-group&gt;",
  "runtime_version": "python|3.7",
  "runtime_version_detected": "-",
  "sku": "BASIC",
  "src_path": "//var//lib//postgresql//djangoapp"
}
</pre>

Copie el valor de *\<app-resource-group>* . Lo necesitará para configurar la aplicación más adelante. 

> [!TIP]
> Puede usar el mismo comando más adelante para implementar cualquier cambio y habilitar inmediatamente los registros de diagnóstico con la siguiente línea:
> 
> ```azurecli
> az webapp up --name <app-name>
> ```

El código de ejemplo ya está implementado, pero la aplicación no se conecta todavía a la base de datos Postgres en Azure. Se ocupará de esto a continuación.

### <a name="configure-environment-variables"></a>Configuración de las variables de entorno

Cuando ejecuta la aplicación localmente, puede establecer las variables de entorno en la sesión de terminal. En App Service, las variables de entorno se establecen como *valores de aplicación* mediante el comando [az webapp config appsettings set](/cli/azure/webapp/config/appsettings#az-webapp-config-appsettings-set).

Ejecute el siguiente comando para especificar los detalles de la conexión de base de datos como valores de aplicación. Reemplace *\<app-name>* , *\<app-resource-group>* y *\<postgresql-name>* por sus propios valores. Recuerde que el comando `az postgres up` creó automáticamente las credenciales de usuario `root` y `Pollsdb1`.

```azurecli
az webapp config appsettings set --name <app-name> --resource-group <app-resource-group> --settings DJANGO_ENV="production" DBHOST="<postgresql-name>.postgres.database.azure.com" DBUSER="root@<postgresql-name>" DBPASS="Pollsdb1" DBNAME="pollsdb"
```

Para información sobre cómo el código accede a estos valores de aplicación, consulte [Acceso a variables de entorno](how-to-configure-python.md#access-environment-variables).

### <a name="run-database-migrations"></a>Ejecución de migraciones de base de datos

Para ejecutar migraciones de base de datos en App Service, abra una sesión de SSH en el explorador. Para ello, vaya a *https://\<app-name>.scm.azurewebsites.net/webssh/host*:

<!-- doesn't work when container not started -->
<!-- ```azurecli
az webapp ssh --resource-group myResourceGroup --name <app-name>
``` -->

En la sesión de SSH, ejecute los siguientes comandos:

```bash
cd site/wwwroot

# Activate default virtual environment in App Service container
source /antenv/bin/activate
# Install requirements in environment
pip install -r requirements.txt
# Run database migrations
python manage.py migrate
# Create the super user (follow prompts)
python manage.py createsuperuser
```

### <a name="browse-to-the-azure-app"></a>Navegación hasta la aplicación de Azure

Vaya a la aplicación implementada escribiendo la dirección URL *http:\//\<app-name>.azurewebsites.net* en un explorador. Debería ver el mensaje **No polls are available** (No hay sondeos disponibles). 

Vaya a *http:\//\<app-name>.azurewebsites.net/admin* e inicie sesión con el usuario administrador que creó en el último paso. Seleccione **Agregar** junto a **Preguntas** y cree una pregunta de sondeo con opciones.

Vaya a la aplicación implementada con la dirección URL *http:\//\<app-name>.azurewebsites.net/admin* y cree algunas preguntas de sondeo. Puede ver las preguntas en *http:\//\<app-name>.azurewebsites.net/* . 

![Ejecución de la aplicación Python Django en App Services en Azure](./media/tutorial-python-postgresql-app/deploy-python-django-app-in-azure.png)

Vaya a la aplicación implementada con la dirección URL *http:\//\<app-name>.azurewebsites.net* de nuevo para ver la pregunta de sondeo y responderla.

App Service detecta un proyecto de Django en el repositorio mediante la búsqueda de un archivo *wsgi.py* en cada subdirectorio, archivo que `manage.py startproject` crea de forma predeterminada. Cuando encuentra el archivo, carga la aplicación web Django. Para más información sobre cómo carga App Service aplicaciones Python, consulte [Configuración de una imagen de Python integrada](how-to-configure-python.md).

**¡Enhorabuena!** Va a ejecutar una aplicación web Python (Django) en Azure App Service para Linux.

## <a name="develop-app-locally-and-redeploy"></a>Desarrollo local de la aplicación y nueva implementación

En esta sección, desarrollará la aplicación en el entorno local y volverá a implementar el código en App Service.

### <a name="set-up-locally-and-run"></a>Configuración local y ejecución

Para configurar el entorno de desarrollo local y ejecutar la aplicación de ejemplo por primera vez, ejecute los siguientes comandos:

# <a name="bash"></a>[bash](#tab/bash)

```bash
# Configure the Python virtual environment
python3 -m venv venv
source venv/bin/activate

# Install packages
pip install -r requirements.txt
# Run Django migrations
python manage.py migrate
# Create Django superuser (follow prompts)
python manage.py createsuperuser
# Run the dev server
python manage.py runserver
```

# <a name="powershell"></a>[PowerShell](#tab/powershell)

```powershell
# Configure the Python virtual environment
py -3 -m venv venv
Set-ExecutionPolicy -Scope CurrentUser -ExecutionPolicy RemoteSigned -Force
venv\scripts\activate

# Install packages
pip install -r requirements.txt
# Run Django migrations
python manage.py migrate
# Create Django superuser (follow prompts)
python manage.py createsuperuser
# Run the dev server
python manage.py runserver
```

# <a name="cmd"></a>[CMD](#tab/cmd)

```CMD
:: Configure the Python virtual environment
py -3 -m venv venv
venv\scripts\activate

:: Install packages
pip install -r requirements.txt
:: Run Django migrations
python manage.py migrate
:: Create Django superuser (follow prompts)
python manage.py createsuperuser
:: Run the dev server
python manage.py runserver
```
---

Cuando la aplicación web Django se haya cargado completamente, verá algo similar al siguiente mensaje:

<pre>
Performing system checks...

System check identified no issues (0 silenced).
December 13, 2019 - 10:54:59
Django version 2.1.2, using settings 'azuresite.settings'
Starting development server at http://127.0.0.1:8000/
Quit the server with CONTROL-C.
</pre>

Vaya a *http:\//localhost:8000* mediante un explorador. Debería ver el mensaje **No polls are available** (No hay sondeos disponibles). 

Vaya a *http:\//localhost:8000/admin* e inicie sesión con el usuario administrador que creó en el último paso. Seleccione **Agregar** junto a **Preguntas** y cree una pregunta de sondeo con opciones.

![Ejecución local de la aplicación Python Django en App Services](./media/tutorial-python-postgresql-app/run-python-django-app-locally.png)

Vaya a *http:\//localhost: 8000* de nuevo para ver la pregunta de sondeo y responderla. La aplicación de ejemplo Django local escribe y almacena los datos de usuario en una base de datos Sqlite3 local, por lo que no debe preocuparse por posibles incidencias en la base de datos de producción. Para que el entorno de desarrollo coincida con el entorno de Azure, considere la posibilidad de usar una base de datos de Postgres localmente.

Para detener el servidor de Django, escriba Ctrl+C.

### <a name="update-the-app"></a>Actualización de la aplicación

Solo para ver cómo funcionan las actualizaciones de la aplicación, realice un pequeño cambio en el archivo `polls/models.py`. Busque la línea:

<pre>
choice_text = models.CharField(max_length=200)
</pre>

Cámbiela a:

```python
choice_text = models.CharField(max_length=100)
```

Al cambiar el modelo de datos, debe crear una nueva migración de Django. Hágalo con el siguiente comando:

```
python manage.py makemigrations
```

Puede probar los cambios localmente ejecutando migraciones, ejecutando el servidor de desarrollo y navegando a *http:\//localhost:8000/admin*:

```
python manage.py migrate
python manage.py runserver
```

### <a name="redeploy-code-to-azure"></a>Nueva implementación del código en Azure

Para volver a implementar los cambios, ejecute el siguiente comando desde la raíz del repositorio:

```azurecli
az webapp up --name <app-name>
```

App Service detecta que la aplicación existe y solo implementa el código.

### <a name="rerun-migrations-in-azure"></a>Nueva ejecución de migraciones en Azure

Dado que ha realizado cambios en el modelo de datos, debe volver a ejecutar las migraciones de base de datos en App Service. Abra una sesión de SSH en el explorador. Para ello, vaya a *https://\<app-name>.scm.azurewebsites.net/webssh/host*. Ejecute los comandos siguientes:

```
cd site/wwwroot

# Activate default virtual environment in App Service container
source /antenv/bin/activate
# Run database migrations
python manage.py migrate
```

### <a name="review-app-in-production"></a>Revisión de la aplicación en producción

Vaya a *http:\//\<app-name>.azurewebsites.net* y vea cómo se ejecutan los cambios en producción. 

## <a name="stream-diagnostic-logs"></a>Transmisión de registros de diagnóstico

Puede acceder a los registros de consola generados desde dentro del contenedor.

> [!TIP]
> `az webapp up` activa el registro predeterminado. Por motivos de rendimiento, este registro se desactiva después de un tiempo, pero vuelve a activarse cada vez que se vuelve a ejecutar `az webapp up`. Para activarlo manualmente, ejecute el siguiente comando:
>
> ```azurecli
> az webapp log config --name <app-name> --resource-group <app-resource-group> --docker-container-logging filesystem
> ```

Ejecute el siguiente comando de la CLI de Azure para ver la secuencia de registro:

```azurecli
az webapp log tail --name <app-name> --resource-group <app-resource-group>
```

Si no ve los registros de la consola de inmediato, vuelve a comprobarlo en 30 segundos.

> [!NOTE]
> También puede inspeccionar los archivos de registro desde el explorador en `https://<app-name>.scm.azurewebsites.net/api/logs/docker`.

Para detener el streaming del registro en cualquier momento, escriba `Ctrl`+`C`.

## <a name="manage-your-app-in-the-azure-portal"></a>Administración de una aplicación en Azure Portal

En [Azure Portal](https://portal.azure.com), busque y seleccione la aplicación que ha creado.

![Vaya a la aplicación Python Django en Azure Portal](./media/tutorial-python-postgresql-app/navigate-to-django-app-in-app-services-in-the-azure-portal.png)

De manera predeterminada, el portal muestra la página **Información general** de la aplicación. Esta página proporciona una visión del funcionamiento de la aplicación. En ella puede también realizar tareas de administración básicas como examinar, detener, reiniciar y eliminar. Las pestañas del lado izquierdo de la página muestran las diferentes páginas de configuración que puede abrir.

![Administración de la aplicación Python Django en la página de información general de Azure Portal](./media/tutorial-python-postgresql-app/manage-django-app-in-app-services-in-the-azure-portal.png)

## <a name="clean-up-resources"></a>Limpieza de recursos

Si prevé que no necesitará estos recursos en el futuro, ejecute los siguientes comandos para eliminar el grupo de recursos:

```azurecli
az group delete --name myResourceGroup
az group delete --name <app-resource-group>
```

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, ha aprendido:

> [!div class="checklist"]
> * Crear una base de datos de Azure Database for PostgreSQL
> * Implementar código en Azure App Service y conectarse a Postgres
> * Actualizar el código y volver a implementarlo
> * Visualización de los registros de diagnóstico
> * Administrar la aplicación web en Azure Portal

Vaya al siguiente tutorial para aprender a asignar un nombre DNS personalizado a la aplicación:

> [!div class="nextstepaction"]
> [Tutorial: Asignar un nombre DNS personalizado a la aplicación](../app-service-web-tutorial-custom-domain.md)

O bien, eche un vistazo a otros recursos:

> [!div class="nextstepaction"]
> [Configuración de una aplicación de Python](how-to-configure-python.md)
