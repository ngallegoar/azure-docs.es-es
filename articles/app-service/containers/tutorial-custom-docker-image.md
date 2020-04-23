---
title: 'Tutorial: Creación y ejecución de una imagen personalizada'
description: Aprenda a crear una imagen personalizada de Linux que se pueda ejecutar en Azure App Service, a implementarla en Azure Container Registry y a ejecutarla en App Service.
keywords: azure app service, aplicación web, linux, docker, contenedor
author: msangapu-msft
ms.assetid: b97bd4e6-dff0-4976-ac20-d5c109a559a8
ms.topic: tutorial
ms.date: 03/27/2019
ms.author: msangapu
ms.custom: mvc, seodec18
ms.openlocfilehash: 2609ff908b3c2f872cb63d3dcd7dcd481d316484
ms.sourcegitcommit: 09a124d851fbbab7bc0b14efd6ef4e0275c7ee88
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2020
ms.locfileid: "82085866"
---
# <a name="tutorial-build-a-custom-image-and-run-in-app-service-from-a-private-registry"></a>Tutorial: Compilación de una imagen personalizada para ejecutarla en App Service a partir de un registro privado

[App Service](app-service-linux-intro.md) proporciona imágenes integradas de Docker en Linux con compatibilidad para versiones específicas, como PHP 7.3 y Node.js 10.14. App Service usa la tecnología de contenedores de Docker para hospedar imágenes integradas e imágenes personalizadas como plataforma como servicio. En este tutorial aprenderá a compilar una imagen personalizada y a ejecutarla en App Service. Este patrón es útil cuando las imágenes integradas no incluyen el lenguaje elegido o cuando la aplicación requiere una configuración específica que no se proporciona en las imágenes integradas.

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Implementar una imagen personalizada en un registro de contenedor privado
> * Ejecutar la imagen personalizada en App Service
> * Configuración de las variables de entorno
> * Actualizar y volver a implementar la imagen
> * Acceso a los registros de diagnóstico
> * Conexión al contenedor con SSH

[!INCLUDE [Free trial note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Prerrequisitos

Para completar este tutorial, necesita:

* [Git](https://git-scm.com/downloads)
* [Docker](https://docs.docker.com/get-started/#setup)

## <a name="download-the-sample"></a>Descarga del ejemplo

En una ventana de terminal, ejecute el siguiente comando para clonar el repositorio de la aplicación de ejemplo en el equipo local y, luego, cambie al directorio que contiene el código de ejemplo.

```bash
git clone https://github.com/Azure-Samples/docker-django-webapp-linux.git --config core.autocrlf=input
cd docker-django-webapp-linux
```

## <a name="build-the-image-from-the-docker-file"></a>Compilación de la imagen desde el archivo de Docker

En el repositorio de Git, examine _Dockerfile_. Este archivo describe el entorno de Python que se requiere para ejecutar la aplicación. Además, la imagen configura un servidor [SSH](https://www.ssh.com/ssh/protocol/) para una comunicación segura entre el contenedor y el host. La última línea del archivo _Dockerfile_, `ENTRYPOINT ["init.sh"]`, invoca a `init.sh` para iniciar el servicio SSH y el servidor de Python.

```Dockerfile
FROM python:3.4

RUN mkdir /code
WORKDIR /code
ADD requirements.txt /code/
RUN pip install -r requirements.txt
ADD . /code/

# ssh
ENV SSH_PASSWD "root:Docker!"
RUN apt-get update \
        && apt-get install -y --no-install-recommends dialog \
        && apt-get update \
    && apt-get install -y --no-install-recommends openssh-server \
    && echo "$SSH_PASSWD" | chpasswd 

COPY sshd_config /etc/ssh/
COPY init.sh /usr/local/bin/
    
RUN chmod u+x /usr/local/bin/init.sh
EXPOSE 8000 2222

#service SSH start
#CMD ["python", "/code/manage.py", "runserver", "0.0.0.0:8000"]
ENTRYPOINT ["init.sh"]
```

Compile la imagen de Docker con el comando `docker build`.

```bash
docker build --tag mydockerimage .
```

Ejecute el contenedor de Docker para comprobar que la compilación funciona. Emita el comando [`docker run`](https://docs.docker.com/engine/reference/commandline/run/) y pásele el nombre y la etiqueta de la imagen. Asegúrese de especificar el puerto con el argumento `-p`.

```bash
docker run -p 8000:8000 mydockerimage
```

Compruebe que la aplicación web y el contenedor funcionan correctamente, para lo que debe examinar `http://localhost:8000`.

![Prueba local de la aplicación web](./media/app-service-linux-using-custom-docker-image/app-service-linux-browse-local.png)

[!INCLUDE [Try Cloud Shell](../../../includes/cloud-shell-try-it.md)]

## <a name="deploy-app-to-azure"></a>Implementación de la aplicación en Azure

Para crear una aplicación que use la imagen que acaba de crear, ejecute los comandos de la CLI de Azure que crean un grupo de recursos, insertan la imagen y crean la aplicación web del plan de App Service para ejecutarla.

### <a name="create-a-resource-group"></a>Crear un grupo de recursos

[!INCLUDE [Create resource group](../../../includes/app-service-web-create-resource-group-linux-no-h.md)] 

### <a name="create-an-azure-container-registry"></a>Creación de una instancia de Azure Container Registry

En Cloud Shell, use el comando [`az acr create`](/cli/azure/acr?view=azure-cli-latest#az-acr-create) para crear una instancia de Azure Container Registry.

```azurecli-interactive
az acr create --name <azure-container-registry-name> --resource-group myResourceGroup --sku Basic --admin-enabled true
```

### <a name="sign-in-to-azure-container-registry"></a>Inicio de sesión en Azure Container Registry

Para insertar una imagen en el registro, deberá autenticarse con el registro privado. En Cloud Shell, use el comando [`az acr show`](/cli/azure/acr?view=azure-cli-latest#az-acr-show) para recuperar las credenciales del registro que creó.

```azurecli-interactive
az acr credential show --name <azure-container-registry-name>
```

El resultado revela dos contraseñas junto con el nombre de usuario.

<pre>
{
  "passwords": [
    {
      "name": "password",
      "value": "{password}"
    },
    {
      "name": "password2",
      "value": "{password}"
    }
  ],
  "username": "&lt;registry-username&gt;"
}
</pre>

En la ventana del terminal local, inicie sesión en Azure Container Registry mediante el comando `docker login`, como se muestra en el siguiente ejemplo. Reemplace *\<azure-container-registry-name>* y *\<registry-username>* por los valores de su registro. Cuando se le solicite, escriba una de las contraseñas del paso anterior.

```bash
docker login <azure-container-registry-name>.azurecr.io --username <registry-username>
```

Confirme que el inicio de sesión se realiza correctamente.

### <a name="push-image-to-azure-container-registry"></a>Inserción de imágenes en Azure Container Registry

Etiquete la imagen local en Azure Container Registry. Por ejemplo:
```bash
docker tag mydockerimage <azure-container-registry-name>.azurecr.io/mydockerimage:v1.0.0
```

Para insertar una imagen, use el comando `docker push`. Etiquete la imagen con el nombre del registro, seguido del nombre y la etiqueta de la imagen.

```bash
docker push <azure-container-registry-name>.azurecr.io/mydockerimage:v1.0.0
```

De nuevo en Cloud Shell, compruebe que la inserción sea correcta.

```azurecli-interactive
az acr repository list -n <azure-container-registry-name>
```

Debería obtener la siguiente salida.

<pre>
[
  "mydockerimage"
]
</pre>

### <a name="create-app-service-plan"></a>Creación de un plan de App Service

[!INCLUDE [Create app service plan](../../../includes/app-service-web-create-app-service-plan-linux-no-h.md)]

### <a name="create-web-app"></a>Crear una aplicación web

En Cloud Shell, cree una [aplicación web](app-service-linux-intro.md) en el plan de App Service `myAppServicePlan` con el comando [`az webapp create`](/cli/azure/webapp?view=azure-cli-latest#az-webapp-create). Reemplace _\<app-name>_ por un nombre de aplicación único y _\<azure-container-registry-name>_ , por el nombre de su registro.

```azurecli-interactive
az webapp create --resource-group myResourceGroup --plan myAppServicePlan --name <app-name> --deployment-container-image-name <azure-container-registry-name>.azurecr.io/mydockerimage:v1.0.0
```

Cuando se haya creado la aplicación web, la CLI de Azure mostrará información similar a la del ejemplo siguiente:

<pre>
{
  "availabilityState": "Normal",
  "clientAffinityEnabled": true,
  "clientCertEnabled": false,
  "cloningInfo": null,
  "containerSize": 0,
  "dailyMemoryTimeQuota": 0,
  "defaultHostName": "&lt;app-name&gt;.azurewebsites.net",
  "deploymentLocalGitUrl": "https://&lt;username&gt;@&lt;app-name&gt;.scm.azurewebsites.net/&lt;app-name&gt;.git",
  "enabled": true,
  &lt; JSON data removed for brevity. &gt;
}
</pre>

### <a name="configure-registry-credentials-in-web-app"></a>Configuración de las credenciales del registro en la aplicación web

Para que App Service extraiga la imagen privada, necesita información sobre el registro y la imagen. En Cloud Shell, esta se proporciona con el comando [`az webapp config container set`](/cli/azure/webapp/config/container?view=azure-cli-latest#az-webapp-config-container-set). Reemplace *\<app-name>* , *\<azure-container-registry-name>* , _\<registry-username>_ y _\<password>_ .

```azurecli-interactive
az webapp config container set --name <app-name> --resource-group myResourceGroup --docker-custom-image-name <azure-container-registry-name>.azurecr.io/mydockerimage:v1.0.0 --docker-registry-server-url https://<azure-container-registry-name>.azurecr.io --docker-registry-server-user <registry-username> --docker-registry-server-password <password>
```

> [!NOTE]
> Al usar un registro que no sea de Docker Hub, `--docker-registry-server-url` debe tener el formato `https://`, seguido del nombre de dominio completo del registro.
>

### <a name="configure-environment-variables"></a>Configuración de las variables de entorno

La mayoría de las imágenes de Docker usan variables de entorno personalizadas, como un puerto distinto de 80. Para indicar a App Service el puerto que usa la imagen se utiliza la configuración de la aplicación `WEBSITES_PORT`. La página de GitHub que contiene el [ejemplo de Python de este tutorial](https://github.com/Azure-Samples/docker-django-webapp-linux) muestra que es preciso establecer `WEBSITES_PORT` en _8000_.

Para establecer la configuración de la aplicación, utilice el comando [`az webapp config appsettings set`](/cli/azure/webapp/config/appsettings?view=azure-cli-latest#az-webapp-config-appsettings-set) en Cloud Shell. La configuración de la aplicación distingue mayúsculas de minúsculas y la separación se realiza mediante espacios.

```azurecli-interactive
az webapp config appsettings set --resource-group myResourceGroup --name <app-name> --settings WEBSITES_PORT=8000
```

### <a name="test-the-web-app"></a>Prueba de la aplicación web

Para comprobar que la aplicación web funciona desplácese hasta ella (`http://<app-name>.azurewebsites.net`).

> [!NOTE]
> La primera vez que acceda a la aplicación esta puede tardar, ya que App Service necesita extraer la imagen completa. Si el tiempo del explorador se agota, simplemente actualice la página.

![Prueba de la configuración del puerto de la aplicación web](./media/app-service-linux-using-custom-docker-image/app-service-linux-browse-azure.png)

## <a name="change-web-app-and-redeploy"></a>Cambio de la aplicación web y nueva implementación

En el repositorio de Git local, abra app/templates/app/index.html. Busque el primer elemento HTML y cambie a:

```python
<nav class="navbar navbar-inverse navbar-fixed-top">
    <div class="container">
      <div class="navbar-header">
        <a class="navbar-brand" href="#">Azure App Service - Updated Here!</a>
      </div>
    </div>
  </nav>
```

Una vez que modifique y guarde el archivo Python, debe recompilar e insertar la imagen de Docker nueva. Luego reinicie la aplicación web para que los cambios surtan efecto. Use los mismos comandos que usó anteriormente en el tutorial. Puede consultar [Compilación de la imagen desde el archivo de Docker](#build-the-image-from-the-docker-file) e [Inserción de imágenes en Azure Container Registry](#push-image-to-azure-container-registry). Pruebe la aplicación web con las instrucciones que aparecen en [Prueba de la aplicación web](#test-the-web-app).

## <a name="access-diagnostic-logs"></a>Acceso a los registros de diagnóstico

[!INCLUDE [Access diagnostic logs](../../../includes/app-service-web-logs-access-no-h.md)]

## <a name="enable-ssh-connections"></a>Habilitación de las conexiones SSH

SSH habilita la comunicación segura entre un contenedor y un cliente. Para habilitar la conexión SSH en su contenedor, la imagen personalizada debe configurarse para ello. Echemos un vistazo al repositorio de ejemplo que ya tiene la configuración necesaria.

* En [Dockerfile](https://github.com/Azure-Samples/docker-django-webapp-linux/blob/master/Dockerfile), el siguiente código instala el servidor SSH y establece las credenciales de inicio de sesión.

    ```Dockerfile
    ENV SSH_PASSWD "root:Docker!"
    RUN apt-get update \
            && apt-get install -y --no-install-recommends dialog \
            && apt-get update \
      && apt-get install -y --no-install-recommends openssh-server \
      && echo "$SSH_PASSWD" | chpasswd 
    ```

    > [!NOTE]
    > Esta configuración no permite realizar conexiones externas al contenedor. SSH solo está disponible en el sitio de Kudu/SCM. El sitio de Kudu/SCM se autentica con la cuenta de Azure.

* [Dockerfile](https://github.com/Azure-Samples/docker-django-webapp-linux/blob/master/Dockerfile#L18) copia el archivo [sshd_config](https://github.com/Azure-Samples/docker-django-webapp-linux/blob/master/sshd_config) del repositorio al directorio */etc/ssh/* .

    ```Dockerfile
    COPY sshd_config /etc/ssh/
    ```

* [Dockerfile](https://github.com/Azure-Samples/docker-django-webapp-linux/blob/master/Dockerfile#L22) expone el puerto 2222 en el contenedor. Se trata de un puerto interno al que solo pueden acceder los contenedores que se encuentren en la red puente de una red privada virtual. 

    ```Dockerfile
    EXPOSE 8000 2222
    ```

* El [script de entrada](https://github.com/Azure-Samples/docker-django-webapp-linux/blob/master/init.sh#L5) inicia el servidor SSH.

    ```bash
    #!/bin/bash
    service ssh start
    ```

### <a name="open-ssh-connection-to-container"></a>Apertura de una conexión SSH a un contenedor

La conexión SSH solo está disponible mediante el sitio de Kudu, al que se puede acceder en `https://<app-name>.scm.azurewebsites.net`.

Para conectarse, vaya a `https://<app-name>.scm.azurewebsites.net/webssh/host` e inicie sesión con su cuenta de Azure.

Luego se le redirigirá a una página que muestra una consola interactiva.

Puede que desee comprobar que ciertas aplicaciones están en ejecución en el contenedor. Para inspeccionar el contenedor y comprobar los procesos en ejecución, emita el comando `top` en el símbolo del sistema.

```bash
top
```

El comando `top` expone todos los procesos en ejecución de un contenedor.

```
PID USER      PR  NI    VIRT    RES    SHR S %CPU %MEM     TIME+ COMMAND
 1 root      20   0  945616  35372  15348 S  0.0  2.1   0:04.63 node
20 root      20   0   55180   2776   2516 S  0.0  0.2   0:00.00 sshd
42 root      20   0  944596  33340  15352 S  0.0  1.9   0:05.80 node /opt/s+
56 root      20   0   59812   5244   4512 S  0.0  0.3   0:00.93 sshd
58 root      20   0   20228   3128   2664 S  0.0  0.2   0:00.00 bash
62 root      20   0   21916   2272   1944 S  0.0  0.1   0:03.15 top
63 root      20   0   59812   5344   4612 S  0.0  0.3   0:00.03 sshd
65 root      20   0   20228   3140   2672 S  0.0  0.2   0:00.00 bash
71 root      20   0   59812   5380   4648 S  0.0  0.3   0:00.02 sshd
73 root      20   0   20228   3160   2696 S  0.0  0.2   0:00.00 bash
77 root      20   0   21920   2304   1972 R  0.0  0.1   0:00.00 top
```

Felicidades. Ha configurado un contenedor Linux personalizado en App Service.

[!INCLUDE [Clean-up section](../../../includes/cli-script-clean-up.md)]

## <a name="next-steps"></a>Pasos siguientes

¿Qué ha aprendido?

> [!div class="checklist"]
> * Implementar una imagen personalizada en un registro de contenedor privado
> * Ejecutar la imagen personalizada en App Service
> * Configuración de las variables de entorno
> * Actualizar y volver a implementar la imagen
> * Acceso a los registros de diagnóstico
> * Conexión al contenedor con SSH

Vaya al siguiente tutorial para aprender a asignar un nombre DNS personalizado a una aplicación web.

> [!div class="nextstepaction"]
> [Tutorial: Asignar un nombre DNS personalizado a la aplicación](../app-service-web-tutorial-custom-domain.md)

O bien, eche un vistazo a otros recursos:

> [!div class="nextstepaction"]
> [Configuración de un contenedor personalizado](configure-custom-container.md)

> [!div class="nextstepaction"]
> [Tutorial: Aplicación de WordPress con varios contenedores](tutorial-multi-container-app.md)
