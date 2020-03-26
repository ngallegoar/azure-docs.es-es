---
title: 'Tutorial: Aplicación de Python en Linux con Postgre'
description: Aprenda a poner en funcionamiento una aplicación Python para Linux en Azure App Service, con conexión a una base de datos de PostgreSQL en Azure. En este tutorial se utiliza Django.
ms.devlang: python
ms.topic: tutorial
ms.date: 01/23/2020
ms.custom:
- mvc
- seodec18
- seo-python-october2019
- cli-validate
ms.openlocfilehash: 13431b62e64774a4c31cf95200def3ba77f973d7
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/24/2020
ms.locfileid: "77523949"
---
# <a name="tutorial-run-a-python-django-web-app-with-postgresql-in-azure-app-service"></a>Tutorial: Ejecución de una aplicación web Python (Django) con PostgreSQL en Azure App Service

[Azure App Service](app-service-linux-intro.md) proporciona un servicio de hospedaje web muy escalable y con aplicación de revisiones de un modo automático. En este tutorial se muestra cómo conectar una aplicación web Python Django controlada por datos a una base de datos de Azure Database for PostgreSQL, e implementar y ejecutar la aplicación en Azure App Service.

![Aplicación web Python Django en Azure App Service](./media/tutorial-python-postgresql-app/run-python-django-app-in-azure.png)

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Crear una base de datos de Azure Database for PostgreSQL y conectarle una aplicación web
> * Implementar la aplicación web en Azure App Service
> * Visualización de los registros de diagnóstico
> * Administrar la aplicación web en Azure Portal

Puede seguir los pasos de este artículo en macOS, Linux o Windows. Son similares en la mayoría de los casos, aunque las diferencias no se detallan en este tutorial. La mayoría de los ejemplos siguientes usan una ventana de terminal `bash` en Linux. 

## <a name="prerequisites"></a>Prerrequisitos

Antes de comenzar este tutorial:

- [!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]
- Instale [Git](https://git-scm.com/).
- Instale [Python 3](https://www.python.org/downloads/).
- Instale y ejecute [PostgreSQL](https://www.postgresql.org/download/).

## <a name="test-postgresql-installation-and-create-a-database"></a>Prueba de la instalación de PostgreSQL y creación de una base de datos

En primer lugar, conéctese al servidor local de PostgreSQL y cree una base de datos: 

En una ventana de terminal local, ejecute `psql` para conectarse al servidor local de PostgreSQL como el usuario `postgres` integrado.

```bash
sudo su - postgres
psql
```
or
```PowerShell
psql -U postgres
```

Si la conexión se realiza correctamente, significa que la base de datos PostgreSQL está en ejecución. De lo contrario, asegúrese de que se ha iniciado la base de datos PostgreSQL local, para lo que debe seguir las instrucciones para su sistema operativo que se indican en [Downloads - PostgreSQL Core Distribution](https://www.postgresql.org/download/) (Descargas - Distribución del núcleo de PostgreSQL).

Cree una base de datos denominada *pollsdb* y configure un usuario de base de datos denominado *manager* cuya contraseña será *supersecretpass*:

```sql
CREATE DATABASE pollsdb;
CREATE USER manager WITH PASSWORD 'supersecretpass';
GRANT ALL PRIVILEGES ON DATABASE pollsdb TO manager;
```

Escriba `\q` para salir del cliente de PostgreSQL.

<a name="step2"></a>
## <a name="create-and-run-the-local-python-app"></a>Creación y ejecución de la aplicación Python local

A continuación, configure y ejecute la aplicación web Python Django de ejemplo.

El repositorio de ejemplo [djangoapp](https://github.com/Azure-Samples/djangoapp) contiene la aplicación [Django](https://www.djangoproject.com/) de sondeos controlada por datos que se obtiene al seguir [Creación de la primera aplicación Django](https://docs.djangoproject.com/en/2.1/intro/tutorial01/) de la documentación de Django.

### <a name="clone-the-sample-app"></a>Clonación de la aplicación de ejemplo

En una ventana de terminal, ejecute los siguientes comandos para clonar el repositorio de la aplicación de ejemplo y cambiar al nuevo directorio de trabajo:

```bash
git clone https://github.com/Azure-Samples/djangoapp.git
cd djangoapp
```

### <a name="configure-the-python-virtual-environment"></a>Configuración del entorno virtual de Python

Cree y active un entorno virtual de Python donde ejecutar la aplicación.

```bash
python3 -m venv venv
source venv/bin/activate
```
or
```PowerShell
py -3 -m venv venv
venv\scripts\activate
```

En el entorno `venv`, ejecute *env.sh* o *env.ps1* para establecer las variables de entorno que *azuresite/setting.py* usará para la configuración de conexión de base de datos.

```bash
source ./env.sh
```
or
```PowerShell
.\env.ps1
```

Instale los paquetes necesarios de *requirements.txt*, ejecute [migraciones de Django](https://docs.djangoproject.com/en/2.1/topics/migrations/) y [cree un usuario administrador](https://docs.djangoproject.com/en/2.1/intro/tutorial02/#creating-an-admin-user):

```bash
pip install -r requirements.txt
python manage.py migrate
python manage.py createsuperuser
```

### <a name="run-the-web-app"></a>Ejecución de la aplicación web

Una vez creado el usuario administrador, ejecute el servidor de Django.

```bash
python manage.py runserver
```

Cuando la aplicación web Django se haya cargado completamente, verá algo similar al siguiente mensaje:

```bash
Performing system checks...

System check identified no issues (0 silenced).
December 13, 2019 - 10:54:59
Django version 2.1.2, using settings 'azuresite.settings'
Starting development server at http://127.0.0.1:8000/
Quit the server with CONTROL-C.
```

Vaya a *http:\//localhost:8000* mediante un explorador. Debería ver el mensaje **No polls are available** (No hay sondeos disponibles). 

Vaya a *http:\//localhost:8000/admin* e inicie sesión con el usuario administrador que creó en el último paso. Seleccione **Agregar** junto a **Preguntas** y cree una pregunta de sondeo con opciones.

![Ejecución local de la aplicación Python Django en App Services](./media/tutorial-python-postgresql-app/run-python-django-app-locally.png)

Vaya a *http:\//localhost: 8000* de nuevo para ver la pregunta de sondeo y responderla. La aplicación Django de ejemplo local escribe y almacena los datos del usuario en la base de datos PostgreSQL local.

Para detener el servidor de Django, escriba Ctrl+C en el terminal.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

En la mayoría de los pasos restantes de este artículo se usan comandos de la CLI de Azure para Azure Cloud Shell. 

## <a name="create-and-connect-to-azure-database-for-postgresql"></a>Creación y conexión a Azure Database for PostgreSQL

En esta sección creará una base de datos y un servidor de Azure Database for PostgreSQL y conectará su aplicación web al servicio. Al implementar la aplicación web en Azure App Service, esta usará esta base de datos en la nube. 

### <a name="create-a-resource-group"></a>Crear un grupo de recursos

Puede crear un grupo de recursos para el servidor de Azure Database for PostgreSQL o usar uno existente. 

[!INCLUDE [Create resource group](../../../includes/app-service-web-create-resource-group-linux-no-h.md)]

### <a name="create-an-azure-database-for-postgresql-server"></a>Creación de un servidor de Azure Database for PostgreSQL

En Cloud Shell, con el comando [az postgres server create](/cli/azure/postgres/server?view=azure-cli-latest#az-postgres-server-create) se crea un servidor de PostgreSQL.

> [!NOTE]
> Antes de crear un servidor de Azure Database for PostgreSQL, consulte [qué generación de proceso](/azure/postgresql/concepts-pricing-tiers#compute-generations-and-vcores) está disponible en su región. Si su región no es compatible con el hardware Gen4, cambie *--sku-name* de la siguiente línea de comandos por un valor compatible con su región, como Gen5. 

En el siguiente comando, reemplace *\<nombreDePostgresql>* por un nombre de servidor único. El nombre del servidor forma parte del punto de conexión de PostgreSQL *https://\<nombreDePostgresql>.postgres.datablase.azure.com*, por lo que debe ser único en todos los servidores en Azure. 

Reemplace *\<nombreDelGrupoDeRecursos>* y *\<región>* por el nombre y la región del grupo de recursos que desea utilizar. Para *\<nombreDeUsuarioAdministrador>* y *\<contraseñaDeAdministración>* , cree las credenciales de usuario de la cuenta de administrador de la base de datos. Recuerde los valores de *\<nombreDeUsuarioAdministrador>* y *\<contraseñaDeAdministración>* para usarlos más adelante en el inicio de sesión en el servidor y las bases de datos de PostgreSQL.

```azurecli-interactive
az postgres server create --resource-group <resourcegroup-name> --name <postgresql-name> --location "<region>" --admin-user <admin-username> --admin-password <admin-password> --sku-name B_Gen5_1
```

Cuando se crea el servidor de Azure Database for PostgreSQL, la CLI de Azure muestra código JSON similar al del siguiente ejemplo:

```json
{
  "administratorLogin": "myusername",
  "earliestRestoreDate": "2020-01-22T19:02:15.727000+00:00",
  "fullyQualifiedDomainName": "myservername.postgres.database.azure.com",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myresourcegroup/providers/Microsoft.DBforPostgreSQL/servers/myservername",
  "location": "westeurope",
  "masterServerId": "",
  "name": "myservername",
  "replicaCapacity": 5,
  "replicationRole": "None",
  "resourceGroup": "myresourcegroup",
  "sku": {
    "capacity": 1,
    "family": "Gen5",
    "name": "B_Gen5_1",
    "size": null,
    "tier": "Basic"
  },
  < JSON data removed for brevity. >
}
```

### <a name="create-firewall-rules-for-the-azure-database-for-postgresql-server"></a>Creación de reglas de firewall para el servidor de Azure Database for PostgreSQL

Ejecute el comando [az postgres server firewall-rule create](/cli/azure/postgres/server/firewall-rule#az-postgres-server-firewall-rule-create) para permitir el acceso a la base de datos desde los recursos de Azure. Reemplace los marcadores de posición *\<nombreDePostgresql>* y *\<nombreDelGrupoDeRecursos>* por sus valores.

```azurecli-interactive
az postgres server firewall-rule create --resource-group <resourcegroup-name> --server-name <postgresql-name> --start-ip-address=0.0.0.0 --end-ip-address=0.0.0.0 --name AllowAllAzureIPs
```

> [!NOTE]
> La configuración anterior permite las conexiones de red desde todas las direcciones IP dentro de la red de Azure. Para su uso en producción, pruebe a configurar las reglas de firewall más restrictivas posibles al [permitir solo las direcciones IP de salida que utiliza la aplicación](../overview-inbound-outbound-ips.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json#find-outbound-ips).

Vuelva a ejecutar el comando `firewall-rule create` para permitir el acceso desde el equipo local. Reemplace *\<suDirecciónIp>* por [si dirección IP IPv4 local](https://www.whatsmyip.org/). Reemplace los marcadores de posición *\<nombreDePostgresql>* y *\<nombreDelGrupoDeRecursos>* por sus propios valores.

```azurecli-interactive
az postgres server firewall-rule create --resource-group <resourcegroup-name> --server-name <postgresql-name> --start-ip-address=<your-ip-address> --end-ip-address=<your-ip-address> --name AllowLocalClient
```

### <a name="create-and-connect-to-the-azure-database-for-postgresql-database"></a>Creación y conexión a la base de datos de Azure Database for PostgreSQL

Ejecute el comando siguiente para conectarse al servidor de Azure Database for PostgreSQL. Use sus propios valores para *\<nombreDePostgresql>* y *\<nombreDeUsuarioAdministrador>* e inicie sesión con la contraseña que ha creado.

```bash
psql -h <postgresql-name>.postgres.database.azure.com -U <admin-username>@<postgresql-name> postgres
```

Tal como hizo en el servidor local de PostgreSQL, cree una base de datos y un usuario en el servidor de Azure Database for PostgreSQL:

```sql
CREATE DATABASE pollsdb;
CREATE USER manager WITH PASSWORD 'supersecretpass';
GRANT ALL PRIVILEGES ON DATABASE pollsdb TO manager;
```

> [!NOTE]
> Es un procedimiento recomendado crear usuarios de bases de datos con permisos restringidos para aplicaciones específicas, en lugar de utilizar el usuario administrador. El usuario `manager` tiene plenos privilegios *solo* para la base de datos `pollsdb`.

Escriba `\q` para salir del cliente de PostgreSQL.

### <a name="test-app-connectivity-to-the-azure-postgresql-database"></a>Prueba de la conectividad de la aplicación con la base de datos de Azure PostgreSQL

Edite el archivo local *env.sh* o *env.ps1* para que apunte a la base de datos de PostgreSQL en la nube; para ello, reemplace *\<nombreDePostgresql>* por el nombre del servidor de Azure Database for PostgreSQL.

```bash
export DBHOST="<postgresql-name>.postgres.database.azure.com"
export DBUSER="manager@<postgresql-name>"
export DBNAME="pollsdb"
export DBPASS="supersecretpass"
```
or
```powershell
$Env:DBHOST = "<postgresql-name>.postgres.database.azure.com"
$Env:DBUSER = "manager@<postgresql-name>"
$Env:DBNAME = "pollsdb"
$Env:DBPASS = "supersecretpass"
```

En el entorno `venv` de la ventana de terminal local, ejecute los archivos *env.sh* o *env.ps1* editados. 
```bash
source ./env.sh
```
or
```PowerShell
.\env.ps1
```

Ejecute la migración de Django a la base de datos de Azure y cree un usuario administrador.

```bash
python manage.py migrate
python manage.py createsuperuser
```

Una vez creado el usuario administrador, ejecute el servidor de Django.

```bash
python manage.py runserver
```

En un explorador, vaya a *http:\//localhost:8000* para volver a ver el mensaje **No polls are available** (No hay sondeos disponibles). 

Vaya a *http:\//localhost:8000/admin* e inicie sesión con el usuario administrador de antes y cree una pregunta de sondeo, como ya hizo anteriormente.

![Ejecución local de la aplicación Python Django en App Services](./media/tutorial-python-postgresql-app/run-python-django-app-locally.png)

Vaya a *http:\//localhost:8000* de nuevo y verá la pregunta de sondeo. La aplicación ya está escribiendo datos en la base de datos de Azure Database for PostgreSQL.

Para detener el servidor de Django, escriba Ctrl+C en el terminal.

## <a name="deploy-the-web-app-to-azure-app-service"></a>Implementar la aplicación web en Azure App Service

En este paso se implementa la aplicación Python conectada a la base de datos de Azure Database for PostgreSQL en Azure App Service.

### <a name="configure-repository"></a>Configuración del repositorio

Dado que en este tutorial se usa un ejemplo de Django, debe cambiar y agregar algunas opciones de configuración en el archivo *djangoapp/azuresite/settings.py* para que funcione con Azure App Service. 

1. Django valida el encabezado `HTTP_HOST` en las solicitudes entrantes. Para que la aplicación web Django funcione en App Service, debe agregar el nombre de dominio completo de la aplicación a los hosts permitidos. 
   
   Edite *azuresite/settings.py* para cambiar la línea de `ALLOWED_HOSTS` de la siguiente manera:
   
   ```python
   ALLOWED_HOSTS = [os.environ['WEBSITE_SITE_NAME'] + '.azurewebsites.net', '127.0.0.1'] if 'WEBSITE_SITE_NAME' in os.environ else []
   ```
   
1. Django no admite [servir archivos estáticos en producción](https://docs.djangoproject.com/en/2.1/howto/static-files/deployment/). En este tutorial se usa [WhiteNoise](https://whitenoise.evans.io/en/stable/) para permitir servir los archivos. El paquete WhiteNoise ya está instalado en el archivo *requirements.txt*. 
   
   Para configurar Django para que use WhiteNoise, en *azuresite/settings.py*, busque el valor `MIDDLEWARE` y agregue `whitenoise.middleware.WhiteNoiseMiddleware` a la lista, justo después de la línea `django.middleware.security.SecurityMiddleware`. El valor `MIDDLEWARE` debe tener este aspecto:
   
   ```python
   MIDDLEWARE = [
       'django.middleware.security.SecurityMiddleware',
       'whitenoise.middleware.WhiteNoiseMiddleware',
       ...
   ]
   ```
   
1. Al final de *azuresite/settings.py*, agregue las líneas siguientes:
   
   ```python
   STATICFILES_STORAGE = 'whitenoise.storage.CompressedManifestStaticFilesStorage'
   STATIC_ROOT = os.path.join(BASE_DIR, 'staticfiles')
   ```
   
   Para más información sobre la configuración de WhiteNoise, consulte la [documentación de WhiteNoise](https://whitenoise.evans.io/en/stable/).

> [!IMPORTANT]
> La sección de configuración de la base de datos ya sigue el procedimiento recomendado de seguridad que aconseja usar variables de entorno. Para todas las recomendaciones de implementación, consulte [Documentación de Django: Lista de comprobación de implementación](https://docs.djangoproject.com/en/2.1/howto/deployment/checklist/).

Confirme los cambios en la bifurcación del repositorio *djangoapp*:

```bash
git commit -am "configure for App Service"
```

### <a name="configure-a-deployment-user"></a>Configuración de un usuario de implementación

[!INCLUDE [Configure deployment user](../../../includes/configure-deployment-user-no-h.md)]

### <a name="create-app-service-plan"></a>Creación de un plan de App Service

[!INCLUDE [Create app service plan](../../../includes/app-service-web-create-app-service-plan-linux-no-h.md)]

### <a name="create-a-web-app"></a>Creación de una aplicación web

[!INCLUDE [Create web app](../../../includes/app-service-web-create-web-app-python-linux-no-h.md)]

### <a name="configure-environment-variables"></a>Configuración de las variables de entorno

En este mismo tutorial, ya ha definido variables de entorno para conectarse a su base de datos de PostgreSQL.

En Azure App Service, las variables de entorno se establecen como *valores de aplicación* mediante el comando [az webapp config appsettings set](/cli/azure/webapp/config/appsettings?view=azure-cli-latest#az-webapp-config-appsettings-set).

En Azure Cloud Shell, ejecute el siguiente comando para especificar los detalles de la conexión de base de datos como valores de aplicación. Reemplace *\<nombreDeLaAplicación>* , *\<nombreDel GrupoDeRecursos>* y *\<nombreDePostgresql>* por sus propios valores.

```azurecli-interactive
az webapp config appsettings set --name <app-name> --resource-group <resourcegroup-name> --settings DBHOST="<postgresql-name>.postgres.database.azure.com" DBUSER="manager@<postgresql-name>" DBPASS="supersecretpass" DBNAME="pollsdb"
```

Para información sobre cómo el código accede a estos valores de aplicación, consulte [Acceso a variables de entorno](how-to-configure-python.md#access-environment-variables).

### <a name="push-to-azure-from-git"></a>Inserción en Azure desde Git

[!INCLUDE [app-service-plan-no-h](../../../includes/app-service-web-git-push-to-azure-no-h.md)]

```bash 
Counting objects: 60, done.
Delta compression using up to 8 threads.
Compressing objects: 100% (51/51), done.
Writing objects: 100% (60/60), 15.37 KiB | 749.00 KiB/s, done.
Total 60 (delta 9), reused 0 (delta 0)
remote: Deploy Async
remote: Updating branch 'master'.
remote: Updating submodules.
remote: Preparing deployment for commit id '06f3f7c0cb'.
remote: Repository path is /home/site/repository
remote: Running oryx build...
remote: Build orchestrated by Microsoft Oryx, https://github.com/Microsoft/Oryx
remote: You can report issues at https://github.com/Microsoft/Oryx/issues
. 
. 
. 
remote: Done in 100 sec(s).
remote: Running post deployment command(s)...
remote: Triggering recycle (preview mode disabled).
remote: Deployment successful.
remote: Deployment Logs : 'https://<app-name>.scm.azurewebsites.net/newui/jsonviewer?view_url=/api/deployments/06f3f7c0cb52ce3b4aff85c2b5099fbacb65ab94/log'
To https://<app-name>.scm.azurewebsites.net/<app-name>.git 
 * [new branch]      master -> master
```  

El servidor de implementación de App Service ve el archivo *requirements.txt* en la raíz del repositorio y ejecuta la administración de paquetes de Python automáticamente después de `git push`.

### <a name="browse-to-the-azure-app"></a>Navegación hasta la aplicación de Azure

Vaya a la aplicación implementada con la dirección URL *http:\//\<nombreDeLaAplicación>.azurewebsites.net*. Tarda algún tiempo en iniciarse porque el contenedor se debe descargar y ejecutar la primera vez que se solicita la aplicación. Si la página agota el tiempo de espera o muestra un mensaje de error, espere unos minutos y actualice la página.

Debería ver las preguntas de sondeo que creó anteriormente. 

App Service detecta un proyecto de Django en el repositorio mediante la búsqueda de un archivo *wsgi.py* en cada subdirectorio, archivo que `manage.py startproject` crea de forma predeterminada. Cuando encuentra el archivo, carga la aplicación web Django. Para más información sobre cómo carga App Service aplicaciones Python, consulte [Configuración de una imagen de Python integrada](how-to-configure-python.md).

Vaya a *http:\//\<nombreDeLaAplicación>.azurewebsites.net/admin* e inicie sesión con el usuario administrador que ha creado. Si lo desea, cree alguna pregunta de sondeo más.

![Ejecución de la aplicación Python Django en App Services en Azure](./media/tutorial-python-postgresql-app/run-python-django-app-in-azure.png)

**¡Enhorabuena!** Va a ejecutar una aplicación web Python (Django) en Azure App Service para Linux.

## <a name="stream-diagnostic-logs"></a>Transmisión de registros de diagnóstico

[!INCLUDE [Access diagnostic logs](../../../includes/app-service-web-logs-access-no-h.md)]

## <a name="manage-your-app-in-the-azure-portal"></a>Administración de una aplicación en Azure Portal

En [Azure Portal](https://portal.azure.com), busque y seleccione la aplicación que ha creado.

![Vaya a la aplicación Python Django en Azure Portal](./media/tutorial-python-postgresql-app/navigate-to-django-app-in-app-services-in-the-azure-portal.png)

De manera predeterminada, el portal muestra la página **Información general** de la aplicación. Esta página proporciona una visión del funcionamiento de la aplicación. En ella puede también realizar tareas de administración básicas como examinar, detener, reiniciar y eliminar. Las pestañas del lado izquierdo de la página muestran las diferentes páginas de configuración que puede abrir.

![Administración de la aplicación Python Django en la página de información general de Azure Portal](./media/tutorial-python-postgresql-app/manage-django-app-in-app-services-in-the-azure-portal.png)

[!INCLUDE [cli-samples-clean-up](../../../includes/cli-samples-clean-up.md)]

## <a name="next-steps"></a>Pasos siguientes

Vaya al siguiente tutorial para aprender a asignar un nombre DNS personalizado a la aplicación:

> [!div class="nextstepaction"]
> [Tutorial: Asignar un nombre DNS personalizado a la aplicación](../app-service-web-tutorial-custom-domain.md)

O bien, eche un vistazo a otros recursos:

> [!div class="nextstepaction"]
> [Configuración de una aplicación de Python](how-to-configure-python.md)
