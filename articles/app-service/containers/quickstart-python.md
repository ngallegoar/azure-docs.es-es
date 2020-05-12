---
title: 'Inicio rápido: Creación de una aplicación en Python para Linux'
description: Para empezar a trabajar con aplicaciones de Linux en Azure App Service, debe implementar su primera aplicación de Python en un contenedor Linux en App Service.
ms.topic: quickstart
ms.date: 04/03/2020
ms.custom: seo-python-october2019, cli-validate
ms.openlocfilehash: 5b055c3ed93d5f093295b52c7a28a73e242bfe75
ms.sourcegitcommit: 366e95d58d5311ca4b62e6d0b2b47549e06a0d6d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82690877"
---
# <a name="quickstart-create-a-python-app-in-azure-app-service-on-linux"></a>Inicio rápido: Creación de una aplicación de Python en Azure App Service en Linux

En este inicio rápido se implementa una aplicación web de Python en [App Service en Linux](app-service-linux-intro.md), un servicio de Azure de hospedaje de sitios web muy escalable y con aplicación automática de revisiones. Puede usar la [interfaz de la línea de comandos (CLI) de Azure](/cli/azure/install-azure-cli) local en un equipo Mac, Linux o Windows. La aplicación web que va a configurar usa un nivel de App Service gratuito, por lo que no incurrirá en ningún gasto en el transcurso de este artículo.

Si prefiere implementar aplicaciones mediante un entorno de desarrollo integrado, consulte [Deploy Python apps to App Service from Visual Studio Code](/azure/python/tutorial-deploy-app-service-on-linux-01) (Implementación de aplicaciones de Python en App Service desde Visual Studio Code).

## <a name="prerequisites"></a>Prerrequisitos

- Una suscripción a Azure: [cree una cuenta gratuita](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio)
- <a href="https://www.python.org/downloads/" target="_blank">Python 3.7</a> (también se admite Python 3.6)
- <a href="https://git-scm.com/downloads" target="_blank">Git</a>
- <a href="https://docs.microsoft.com/cli/azure/install-azure-cli" target="_blank">CLI de Azure</a> 2.0.80 o superior. Ejecute `az --version` para comprobar la versión.

## <a name="download-the-sample"></a>Descarga del ejemplo

En una ventana de terminal, ejecute el siguiente comando para clonar la aplicación de ejemplo en la máquina local. 

```terminal
git clone https://github.com/Azure-Samples/python-docs-hello-world
```

A continuación, vaya a esa carpeta:

```terminal
cd python-docs-hello-world
```

El repositorio contiene un archivo *application.py*, que indica a App Service que el código contiene una aplicación de Flask. Para más información, consulte el artículo sobre [las personalizaciones y el proceso de inicio del contenedor](how-to-configure-python.md).

## <a name="run-the-sample"></a>Ejecución del ejemplo

En una ventana de terminal, use los comandos siguientes (los que sean más adecuados para su sistema operativo) para instalar las dependencias necesarias e iniciar el servidor de desarrollo integrado. 

# <a name="bash"></a>[Bash](#tab/bash)

```bash
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt
export FLASK_APP=application.py
flask run
```

# <a name="powershell"></a>[PowerShell](#tab/powershell)

```powershell
py -3 -m venv env
env\scripts\activate
pip install -r requirements.txt
Set-Item Env:FLASK_APP ".\application.py"
flask run
```

# <a name="cmd"></a>[Cmd](#tab/cmd)

```cmd
py -3 -m venv env
env\scripts\activate
pip install -r requirements.txt
SET FLASK_APP=application.py
flask run
```

---

Abra un explorador web y vaya a la aplicación de ejemplo en `http://localhost:5000/`. La aplicación muestra el mensaje **Hola mundo**.

![Ejecución de una aplicación de Python de ejemplo en el entorno local](./media/quickstart-python/run-hello-world-sample-python-app-in-browser-localhost.png)

En la ventana de terminal, presione **Ctrl**+**C** para salir del servidor web.

## <a name="sign-in-to-azure"></a>Inicio de sesión en Azure

La CLI de Azure proporciona muchos comandos útiles que puede usar desde un terminal local para aprovisionar y administrar recursos de Azure desde la línea de comandos. Puede usar comandos para realizar las mismas tareas que llevaría a cabo en Azure Portal en un explorador. También puede usar comandos de la CLI en scripts para automatizar los procesos de administración.

Para ejecutar comandos de Azure en la CLI de Azure, primero debe iniciar sesión con el comando `az login`. Este comando abre un explorador que recopila las credenciales.

```azurecli
az login
```

## <a name="deploy-the-sample"></a>Implementación del ejemplo

El comando [`az webapp up`](/cli/azure/webapp#az-webapp-up) crea la aplicación web en App Service e implementa el código.

En la carpeta *python-docs-hello-world* que contiene el código de ejemplo, ejecute el siguiente comando `az webapp up`. Reemplace `<app-name>` por un nombre de aplicación único global (*los caracteres válidos son `a-z`, `0-9` y `-`* ).


```azurecli
az webapp up --sku F1 -n <app-name>
```
> [!CAUTION]
> Si utiliza la **versión 2.5.0 de la CLI de Azure** hay una regresión en `az webapp up` en la que se producirán errores en ciertos escenarios si no se incluye el parámetro `-l <location-name>`. El seguimiento del problema se[realiza aquí](https://github.com/Azure/azure-cli/issues/13257).  
> 
>Puede comprobar qué versión de la CLI de Azure usa con el comando `az --version`.
>

El argumento `--sku F1` crea la aplicación web en el plan de tarifa Gratuito. Puede omitir este argumento para usar en su lugar un nivel Premium, lo que supondrá un costo por hora.

También tiene la opción de incluir el argumento `-l <location-name>`, donde `<location_name>` es una región de Azure, como **centralus**, **eastasia**, **westeurope**, **koreasouth**, **brazilsouth**, **centralindia**, etc. Puede recuperar una lista de las regiones permitidas para su cuenta de Azure mediante la ejecución del comando [`az account list-locations`](/cli/azure/appservice?view=azure-cli-latest.md#az-appservice-list-locations).

El comando `az webapp up` puede tardar varios minutos en ejecutarse. Durante la ejecución, muestra información similar a la del ejemplo siguiente, donde `<app-name>` será el nombre que proporcionó anteriormente:

<pre>
Creating Resource group 'appsvc_rg_Linux_centralus' ...
Resource group creation complete
Creating App service plan 'appsvc_asp_Linux_centralus' ...
App service plan creation complete
Creating app '&lt;app-name&gt;' ....
Configuring default logging for the app, if not already enabled
Creating zip with contents of dir D:\Examples\python-docs-hello-world ...
Getting scm site credentials for zip deployment
Starting zip deployment. This operation can take a while to complete ...
Deployment endpoint responded with status code 202
You can launch the app at http://&lt;app-name&gt;.azurewebsites.net
{
  "URL": "http://&lt;app-name&gt;.net",
  "appserviceplan": "appsvc_asp_Linux_centralus",
  "location": "eastus",
  "name": "&lt;app-name&gt;",
  "os": "Linux",
  "resourcegroup": "appsvc_rg_Linux_centralus",
  "runtime_version": "python|3.7",
  "runtime_version_detected": "-",
  "sku": "FREE",
  "src_path": "D:\\Examples\\python-docs-hello-world"
}
</pre>

[!INCLUDE [AZ Webapp Up Note](../../../includes/app-service-web-az-webapp-up-note.md)]

## <a name="browse-to-the-app"></a>Navegación hasta la aplicación

Vaya a la aplicación implementada mediante el explorador web en la dirección URL `http://<app-name>.azurewebsites.net`.

El código de ejemplo de Python se ejecuta en un contenedor de Linux en App Service con una imagen integrada.

![Ejecución de una aplicación de Python de ejemplo en Azure](./media/quickstart-python/run-hello-world-sample-python-app-in-browser.png)

**¡Enhorabuena!** Ha implementado su primera aplicación de Python en App Service en Linux.

## <a name="redeploy-updates"></a>Reimplementación de actualizaciones

En el editor de código que prefiera, abra *application.py* y actualice la función `hello` como se indica a continuación. Este cambio agrega una instrucción `print` para generar la salida de registro con la que trabajará en la sección siguiente. 

```python
def hello():
    print("Handling request to home page.")
    return "Hello Azure!"
```

Guarde los cambios y salga del editor. 

Vuelva a implementar la aplicación con el comando `az webapp up`:

```azurecli
az webapp up
```

Este comando emplea valores que se almacenan en caché en el archivo *.azure/config*, incluido el nombre de la aplicación, el grupo de recursos y el plan de App Service.

Una vez finalizada la implementación, vuelva a la ventana del explorador abierta en `http://<app-name>.azurewebsites.net` y actualice la página; ahora debería aparecer el mensaje modificado:

![Ejecución de una aplicación de Python de ejemplo actualizada en Azure](./media/quickstart-python/run-updated-hello-world-sample-python-app-in-browser.png)

> [!TIP]
> Visual Studio Code proporciona extensiones eficaces para Python y Azure App Service que simplifican el proceso de implementación de aplicaciones web de Python en App Service. Para más información, consulte [Deploy Python apps to App Service from Visual Studio Code](/azure/python/tutorial-deploy-app-service-on-linux-01) (Implementación de aplicaciones de Python en App Service desde Visual Studio Code).

## <a name="stream-logs"></a>Transmisión de registros

Puede acceder a los registros de consola generados desde dentro de la aplicación y del contenedor en que esta se ejecuta. Los registros incluyen todas las salidas generadas mediante instrucciones `print`.

Para transmitir registros, ejecute el siguiente comando:

```azurecli
az webapp log tail
```

Actualice la aplicación en el explorador para que se generen registros de la consola que incluyan líneas similares al texto siguiente. Si no ve la salida inmediatamente, inténtelo de nuevo en 30 segundos.

<pre>
2020-04-03T22:54:04.236405938Z Handling request to home page.
2020-04-03T22:54:04.236497641Z 172.16.0.1 - - [03/Apr/2020:22:54:04 +0000] "GET / HTTP/1.1" 200 12 "-" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/81.0.4044.83 Safari/537.36 Edg/81.0.416.41"
</pre>

También puede inspeccionar los archivos de registro desde el explorador en `https://<app-name>.scm.azurewebsites.net/api/logs/docker`.

Para detener el streaming del registro en cualquier momento, escriba `Ctrl`+`C`.

## <a name="manage-the-azure-app"></a>Administración de la aplicación de Azure

Vaya a <a href="https://portal.azure.com" target="_blank">Azure Portal</a> para administrar la aplicación que ha creado. Busque y seleccione **App Services**.

![Ir a App Services en Azure Portal](./media/quickstart-python/navigate-to-app-services-in-the-azure-portal.png)

Seleccione el nombre de la aplicación de Azure.

![Aplicación de Python en App Services desde Azure Portal](./media/quickstart-python/navigate-to-app-in-app-services-in-the-azure-portal.png)

Verá la página de información general de la aplicación. En este caso, puede realizar tareas de administración básicas como examinar, detener, iniciar, reiniciar y eliminar.

![Administración de la aplicación de Python en la página de información general de Azure Portal](./media/quickstart-python/manage-an-app-in-app-services-in-the-azure-portal.png)

El menú de App Service proporciona distintas páginas para configurar la aplicación.

## <a name="clean-up-resources"></a>Limpieza de recursos

En los pasos anteriores, creó recursos de Azure en un grupo de recursos. El grupo de recursos tiene un nombre como "appsvc_rg_Linux_CentralUS", en función de su ubicación. Si usa una SKU de App Service que sea de un nivel distinto del F1 gratuito, estos recursos incurrirán en costos continuos (consulte [Precios de App Service](https://azure.microsoft.com/pricing/details/app-service/linux/)).

Si considera que no necesitará estos recursos en el futuro, elimine el grupo de recursos mediante el comando siguiente, reemplazando `<resource-group-name>` por el grupo de recursos que se muestra en la salida del comando `az webapp up`, como "appsvc_rg_Linux_centralus". Este comando puede tardar hasta un minuto en completarse.

```azurecli
az group delete -n <resource-group-name>
```

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Tutorial: Aplicación web Python (Django) con PostgreSQL](tutorial-python-postgresql-app.md)

> [!div class="nextstepaction"]
> [Incorporación del inicio de sesión de un usuario a una aplicación web de Python](../../active-directory/develop/quickstart-v2-python-webapp.md)

> [!div class="nextstepaction"]
> [Configuración de una aplicación de Python](how-to-configure-python.md)

> [!div class="nextstepaction"]
> [Tutorial: Ejecución de la aplicación Python en un contenedor personalizado](tutorial-custom-docker-image.md)
