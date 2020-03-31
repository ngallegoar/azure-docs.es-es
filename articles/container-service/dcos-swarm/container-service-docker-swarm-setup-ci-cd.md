---
title: (EN DESUSO) CI/CD con Azure Container Service y Swarm
description: Uso de Azure Container Service con Docker Swarm, Azure Container Registry y Azure DevOps para proporcionar de forma continua una aplicación .NET Core de varios contenedores
author: jcorioland
ms.service: container-service
ms.topic: conceptual
ms.date: 12/08/2016
ms.author: jucoriol
ms.custom: mvc
ms.openlocfilehash: 11a6debe735459b617f6f93c3f67a32350dd4623
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "76549060"
---
# <a name="deprecated-full-cicd-pipeline-to-deploy-a-multi-container-application-on-azure-container-service-with-docker-swarm-using-azure-devops-services"></a>(EN DESUSO) Canalización completa de CI/CD para implementar una aplicación de varios contenedores en Azure Container Service con Docker Swarm mediante Azure DevOps Services

[!INCLUDE [ACS deprecation](../../../includes/container-service-deprecation.md)]

Uno de los mayores desafíos al desarrollar aplicaciones modernas para la nube es la posibilidad de proporcionar estas aplicaciones de forma continua. En este artículo, aprenderá a implementar una canalización completa de integración e implementación (CI/CD) continuas mediante Azure Container Service con la administración de Docker Swarm, Azure Container Registry y Azure Pipelines.


![Aplicación de ejemplo MyShop](./media/container-service-docker-swarm-setup-ci-cd/myshop-application.png)

El objetivo es entregar continuamente esta aplicación en un clúster de Docker Swarm, mediante Azure DevOps Services. La ilustración siguiente describe esta canalización de entrega continua:

![Aplicación de ejemplo MyShop](./media/container-service-docker-swarm-setup-ci-cd/full-ci-cd-pipeline.png)

Esta es una breve explicación de los pasos:

1. Los cambios de código se realizan en el repositorio de código fuente (aquí, GitHub). 
1. GitHub desencadena una compilación en Azure DevOps Services. 
1. Azure DevOps Services obtiene la última versión del código fuente y compila todas las imágenes que componen la aplicación. 
1. Azure DevOps Services inserta cada imagen en un registro de Docker que se creó mediante el servicio Azure Container Registry. 
1. Azure DevOps Services desencadena una nueva versión. 
1. La versión ejecuta algunos comandos mediante SSH en el nodo maestro del clúster de Azure Container Service. 
1. Docker Swarm en el clúster extrae la última versión de las imágenes. 
1. La nueva versión de la aplicación se implementa con Docker Compose. 

## <a name="prerequisites"></a>Prerrequisitos

Antes de comenzar este tutorial, debe realizar las siguientes tareas:

- [Crear un clúster de Swarm en Azure Container Service (servicio Contenedor de Azure)](container-service-deployment.md)
- [Conectar con el clúster de Swarm en Azure Container Service (servicio Contenedor de Azure)](../container-service-connect.md)
- [Crear una instancia de Azure Container Registry](../../container-registry/container-registry-get-started-portal.md)
- [Tener una organización y un proyecto de Azure DevOps Services creados](https://docs.microsoft.com/azure/devops/organizations/accounts/create-organization-msa-or-work-student)
- [Bifurcar el repositorio de GitHub a su cuenta de GitHub](https://github.com/jcorioland/MyShop/)

[!INCLUDE [container-service-swarm-mode-note](../../../includes/container-service-swarm-mode-note.md)]

También se necesita una máquina Ubuntu (14.04 o 16.04) que tenga instalado Docker. Azure DevOps Services utiliza esta máquina durante los procesos de Azure Pipelines. Una manera de crear esta máquina es usar la imagen disponible en Azure Marketplace. 

## <a name="step-1-configure-your-azure-devops-services-organization"></a>Paso 1: Configuración de la organización de Azure DevOps Services 

En esta sección, se va a configurar una organización de Azure DevOps Services.

### <a name="configure-an-azure-devops-services-linux-build-agent"></a>Configuración de un agente de compilación para Linux en Azure DevOps Services

Para crear imágenes de Docker e insertarlas en una instancia de Azure Container Registry desde una compilación de Azure DevOps Services, debe registrar un agente para Linux. Dispone de estas opciones de instalación:

* [Implementar un agente en Linux](https://www.visualstudio.com/docs/build/admin/agents/v2-linux)

* [Usar Docker para ejecutar al agente de Azure DevOps Services](https://hub.docker.com/r/microsoft/vsts-agent)

### <a name="install-the-docker-integration-azure-devops-services-extension"></a>Instalación de la extensión Docker Integration para Azure DevOps Services

Microsoft proporciona una extensión para Azure DevOps Services a fin de poder trabajar con Docker en los procesos de Azure Pipelines. Esta extensión está disponible en [Azure DevOps Services Marketplace](https://marketplace.visualstudio.com/items?itemName=ms-vscs-rm.docker). Haga clic en **Install** (Instalar) para agregar esta extensión a la organización de Azure DevOps Services:

![Instalar la integración con Docker](./media/container-service-docker-swarm-setup-ci-cd/install-docker-vsts.png)

Se le pedirá que se conecte a la organización de Azure DevOps Services con sus credenciales. 

### <a name="connect-azure-devops-services-and-github"></a>Conexión entre Azure DevOps Services y GitHub

Configure una conexión entre el proyecto de Azure DevOps Services y su cuenta de GitHub.

1. En el proyecto de Azure DevOps Services, haga clic en el icono **Configuración** de la barra de herramientas y seleccione **Servicios**.

    ![Azure DevOps Services (conexión externa)](./media/container-service-docker-swarm-setup-ci-cd/vsts-services-menu.png)

1. En el lado izquierdo, haga clic en **New Service Endpoint** > **GitHub** (Nuevo punto de conexión de servicio > GitHub).

    ![Azure DevOps Services (GitHub)](./media/container-service-docker-swarm-setup-ci-cd/vsts-github.png)

1. Para autorizar a Azure DevOps Services para trabajar con su cuenta de GitHub, haga clic en **Autorizar** y siga el procedimiento en la ventana que se abre.

    ![Azure DevOps Services (Autorizar GitHub)](./media/container-service-docker-swarm-setup-ci-cd/vsts-github-authorize.png)

### <a name="connect-azure-devops-services-to-your-azure-container-registry-and-azure-container-service-cluster"></a>Conexión de Azure DevOps Services a la instancia de Azure Container Registry y al clúster de Azure Container Service

Los últimos pasos antes de entrar en la canalización de CI/CD son configurar conexiones externas al registro de contenedores y al clúster de Docker Swarm en Azure. 

1. En la configuración **Servicios** del proyecto de Azure DevOps Services, agregue un punto de conexión de servicio de tipo **Docker Registry**. 

1. En la ventana emergente que se abre, escriba la dirección URL y las credenciales de su instancia de Azure Container Registry.

    ![Azure DevOps Services (Docker Registry)](./media/container-service-docker-swarm-setup-ci-cd/vsts-registry.png)

1. Para el clúster de Docker Swarm, agregue un punto de conexión de tipo **SSH**. A continuación, escriba la información de conexión de SSH de su clúster de Swarm.

    ![Azure DevOps Services (SSH)](./media/container-service-docker-swarm-setup-ci-cd/vsts-ssh.png)

Ahora ya está lista toda la configuración. En los siguientes pasos, se crea la canalización de CI/CD que compila e implementa la aplicación en el clúster de Docker Swarm. 

## <a name="step-2-create-the-build-pipeline"></a>Paso 2: Creación de la canalización de compilación

En este paso, se va a configurar una canalización de compilación para el proyecto de Azure DevOps Services y también se va a definir el flujo de trabajo de compilación de las imágenes de contenedor.

### <a name="initial-pipeline-setup"></a>Configuración inicial de la canalización

1. Para crear una canalización de compilación, conéctese al proyecto de Azure DevOps Services y haga clic en **Compilación y versión**. 

1. En la sección **Build definitions** (Definiciones de compilación), haga clic en **+ Nuevo**. Seleccione la plantilla **Vacía**.

    ![Azure DevOps (nueva canalización de compilación)](./media/container-service-docker-swarm-setup-ci-cd/create-build-vsts.png)

1. Configure la nueva compilación con un origen de repositorio de GitHub, marque **Integración continua** y seleccione la cola de agentes donde registró el agente de Linux. Haga clic en **Crear** para crear la canalización de compilación.

    ![Azure DevOps Services (crear canalización de compilación)](./media/container-service-docker-swarm-setup-ci-cd/vsts-create-build-github.png)

1. En la página **Build Definitions** (Definiciones de compilación), abra primero la pestaña **Repositorio** y configure la compilación para usar la bifurcación del proyecto MyShop que creó en los requisitos previos. Asegúrese de que selecciona *acs-docs* como la **bifurcación predeterminada**.

    ![Azure DevOps Services (configuración del repositorio de compilación)](./media/container-service-docker-swarm-setup-ci-cd/vsts-github-repo-conf.png)

1. En la pestaña **Desencadenadores**, configure la compilación que se desencadenará después de cada confirmación. Seleccione **Integración continua** y **Batch changes** (Cambios en lote).

    ![Azure DevOps Services (configuración del desencadenador de compilación)](./media/container-service-docker-swarm-setup-ci-cd/vsts-github-trigger-conf.png)

### <a name="define-the-build-workflow"></a>Definición del flujo de trabajo de compilación
Los pasos siguientes definen el flujo de trabajo de compilación. Hay cinco imágenes de contenedor para compilar para la aplicación *MyShop*. Cada imagen se compila mediante el Dockerfile ubicado en las carpetas del proyecto:

* ProductsApi
* Proxy
* RatingsApi
* RecommendationsApi
* ShopFront

Deberá agregar dos pasos de Docker para cada imagen, uno para compilar la imagen y otro para insertar la imagen en Azure Registry Container. 

1. Para agregar un paso en el flujo de trabajo de compilación, haga clic en **+ Add build step** (+Agregar paso de compilación) y seleccione **Docker**.

    ![Azure DevOps Services (adición de pasos de compilación)](./media/container-service-docker-swarm-setup-ci-cd/vsts-build-add-task.png)

1. Para cada imagen, configure un paso que use el comando `docker build`.

    ![Azure DevOps Services (compilación de Docker)](./media/container-service-docker-swarm-setup-ci-cd/vsts-docker-build.png)

    Para la operación de compilación, seleccione su instancia de Azure Container Registry, la acción **Build an image** (Compilar una imagen) y el Dockerfile que define cada imagen. Establezca el **contexto de compilación** como el directorio raíz y defina el **nombre de la imagen**. 
    
    Como se ha mostrado en la pantalla anterior, inicie el nombre de la imagen con el URI de Azure Container Registry. (También puede usar una variable de compilación para parametrizar la etiqueta de la imagen, por ejemplo, el identificador de la compilación en este ejemplo).

1. Para cada imagen, configure un segundo paso que use el comando `docker push`.

    ![Azure DevOps Services (inserción de Docker)](./media/container-service-docker-swarm-setup-ci-cd/vsts-docker-push.png)

    Para la operación de inserción, seleccione su instancia de Azure Container Registry, la acción **Push an image** (Insertar una imagen) y escriba el **nombre de la imagen** que se compiló en el paso anterior.

1. Después de configurar los pasos de compilación e inserción para cada una de las cinco imágenes, agregue dos pasos más en el flujo de trabajo de compilación.

    a. Una tarea de la línea de comandos que usa un script de bash para reemplazar la aparición de *BuildNumber* en el archivo docker-compose.yml por el identificador de la compilación actual. Consulte la siguiente pantalla para más información.

    ![Azure DevOps Services (actualización de archivo de Compose)](./media/container-service-docker-swarm-setup-ci-cd/vsts-build-replace-build-number.png)

    b. Una tarea que quita el archivo de Compose actualizado como un artefacto de compilación para que se pueda usar en la versión. Consulte la siguiente pantalla para más información.

    ![Azure DevOps Services (publicación de archivo de Compose)](./media/container-service-docker-swarm-setup-ci-cd/vsts-publish-compose.png) 

1. Haga clic en **Guardar** y asigne un nombre a la canalización de compilación.

## <a name="step-3-create-the-release-pipeline"></a>Paso 3: Creación de la canalización de versión

Azure DevOps Services permite [administrar versiones entre entornos](https://www.visualstudio.com/team-services/release-management/). Puede habilitar la implementación continua para asegurarse de que la aplicación se implementa en sus diferentes entornos (por ejemplo, desarrollo, prueba, preproducción y producción) sin problemas. Puede crear un nuevo entorno que representa el clúster de Docker Swarm de Azure Container Service.

![Azure DevOps Services (versión para ACS)](./media/container-service-docker-swarm-setup-ci-cd/vsts-release-acs.png) 

### <a name="initial-release-setup"></a>Configuración inicial de la versión.

1. Para crear una canalización de versión, haga clic en **Versiones** >  **+ Versión**.

1. Para configurar el origen de artefacto, haga clic en **Artefactos** > **Link an artifact source** (Vincular un origen de artefacto). Aquí, vincule esta nueva canalización de versión a la compilación definida en el paso anterior. De esta forma, el archivo docker-compose.yml estará disponible en el proceso de versión.

    ![Azure DevOps Services (artefactos de versión)](./media/container-service-docker-swarm-setup-ci-cd/vsts-release-artefacts.png) 

1. Para configurar el desencadenador de versión, haga clic en **Desencadenadores** y seleccione **Implementación continua**. Establezca el desencadenador en el mismo origen de artefacto. Este valor garantiza que tan pronto como finaliza la compilación correctamente, se inicia una nueva versión.

    ![Azure DevOps Services (desencadenadores de versión)](./media/container-service-docker-swarm-setup-ci-cd/vsts-release-trigger.png) 

### <a name="define-the-release-workflow"></a>Definición del flujo de trabajo de versión

El flujo de trabajo de versión se compone de dos tareas que agrega.

1. Configure una tarea para que copie de forma segura el archivo de Compose en una carpeta de *implementación* en el nodo maestro de Docker Swarm, mediante la conexión SSH configurada anteriormente. Consulte la siguiente pantalla para más información.

    ![Azure DevOps Services (SCP de versión)](./media/container-service-docker-swarm-setup-ci-cd/vsts-release-scp.png)

1. Configure una segunda tarea para que ejecute un comando de bash para ejecutar los comandos `docker` y `docker-compose` en el nodo maestro. Consulte la siguiente pantalla para más información.

    ![Azure DevOps Services (Bash de versión)](./media/container-service-docker-swarm-setup-ci-cd/vsts-release-bash.png)

    El comando ejecutado en el nodo maestro usa la CLI de Docker y la CLI de Docker-Compose para realizar las siguientes tareas:

   - Inicie sesión en Azure Container Registry (usa tres variables de compilación que se definen en la pestaña **Variables**).
   - Defina la variable **DOCKER_HOST** para que funcione con el punto de conexión de Swarm (:2375)
   - Vaya a la carpeta de *implementación* que se creó en la tarea de copia segura anterior y que contiene el archivo docker-compose.yml. 
   - Ejecute comandos `docker-compose` que extraigan las nuevas imágenes, detengan los servicios, quiten los servicios y creen los contenedores.

     >[!IMPORTANT]
     > Como se muestra en la pantalla anterior, deje la casilla **Fail on STDERR** (Error en STDERR) desactivada. Se trata de una opción importante, porque `docker-compose` imprime varios mensajes de diagnóstico (por ejemplo, los contenedores se están deteniendo o se están eliminando), en la salida de error estándar. Si activa la casilla, Azure DevOps Services informa de que se produjeron errores durante la versión, incluso si todo va bien.
     >
1. Guarde esta nueva canalización de versión.


>[!NOTE]
>Esta implementación incluye algún tiempo de inactividad porque se detendrán los servicios antiguos y se ejecutará el nuevo. Para evitar esto, realice una implementación Blue-Green.
>

## <a name="step-4-test-the-cicd-pipeline"></a>Paso 4. Prueba de la canalización de CI/CD

Ahora que ha terminado con la configuración, es hora de probar esta nueva canalización de CI/CD. La manera más fácil de hacerlo es actualizar el código fuente y confirmar los cambios en el repositorio de GitHub. Unos pocos segundos después de insertar el código, verá una nueva compilación que se ejecuta en Azure DevOps Services. Una vez completada correctamente, se desencadenará una nueva versión y se implementará la nueva versión de la aplicación en el clúster de Azure Container Service.

## <a name="next-steps"></a>Pasos siguientes

* Para más información sobre CI/CD con Azure DevOps Services, consulte el artículo sobre la [documentación de Azure Pipelines](/azure/devops/pipelines/?view=azure-devops).
