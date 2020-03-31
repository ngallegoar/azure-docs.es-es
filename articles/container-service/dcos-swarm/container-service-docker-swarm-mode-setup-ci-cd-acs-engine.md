---
title: (EN DESUSO) CI/CD con Azure Container Service Engine y modo Swarm
description: Uso de Azure Container Service Engine con modo Docker Swarm, Azure Container Registry y Azure DevOps para proporcionar de forma continua una aplicación .NET Core de varios contenedores
author: diegomrtnzg
ms.service: container-service
ms.topic: conceptual
ms.date: 05/27/2017
ms.author: dimart
ms.custom: mvc
ms.openlocfilehash: 1ec7ece6f5afd1bbd2613ae08af04b82e8a156b2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "76277926"
---
# <a name="deprecated-full-cicd-pipeline-to-deploy-a-multi-container-application-on-azure-container-service-with-acs-engine-and-docker-swarm-mode-using-azure-devops"></a>(EN DESUSO) Canalización completa de CI/CD para implementar una aplicación de varios contenedores en Azure Container Service con ACS Engine y modo Docker Swarm mediante Azure DevOps

[!INCLUDE [ACS deprecation](../../../includes/container-service-deprecation.md)]

*Este artículo se basa en el documento [Canalización completa de CI/CD para implementar una aplicación de varios contenedores en Azure Container Service con Docker Swarm mediante Azure DevOps](container-service-docker-swarm-setup-ci-cd.md)*

En la actualidad, uno de los mayores desafíos al desarrollar aplicaciones modernas para la nube es la posibilidad de proporcionar estas aplicaciones de forma continua. En este artículo, aprenderá a implementar una integración completa continua y la canalización de la implementación (CI/CD) mediante: 
* Azure Container Service Engine con modo Docker Swarm
* Azure Container Registry
* Azure DevOps


![Aplicación de ejemplo MyShop](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/myshop-application.png)

El objetivo es entregar continuamente esta aplicación en un clúster en modo Docker Swarm, mediante Azure DevOps. La ilustración siguiente describe esta canalización de entrega continua:

![Aplicación de ejemplo MyShop](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/full-ci-cd-pipeline.png)

Esta es una breve explicación de los pasos:

1. Los cambios de código se realizan en el repositorio de código fuente (aquí, GitHub). 
2. GitHub desencadena una compilación en Azure DevOps. 
3. Azure DevOps obtiene la última versión del código fuente y compila todas las imágenes que componen la aplicación. 
4. Azure DevOps inserta cada imagen en un registro de Docker que se creó mediante el servicio Azure Container Registry. 
5. Azure DevOps desencadena una nueva versión. 
6. La versión ejecuta algunos comandos mediante SSH en el nodo maestro del clúster de Azure Container Service. 
7. Modo Docker Swarm en el clúster extrae la versión más reciente de las imágenes 
8. La nueva versión de la aplicación se implementa mediante Docker Stack 

## <a name="prerequisites"></a>Prerequisites

Antes de comenzar este tutorial, debe realizar las siguientes tareas:

- [Crear un clúster del modo Swarm en Azure Container Service con ACS Engine](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acsengine-swarmmode)
- [Conectar con el clúster de Swarm en Azure Container Service (servicio Contenedor de Azure)](../container-service-connect.md)
- [Crear una instancia de Azure Container Registry](../../container-registry/container-registry-get-started-portal.md)
- [Tener una organización y un proyecto de Azure DevOps creados](https://docs.microsoft.com/azure/devops/organizations/accounts/create-organization-msa-or-work-student)
- [Bifurcar el repositorio de GitHub a su cuenta de GitHub](https://github.com/jcorioland/MyShop/tree/docker-linux)

>[!NOTE]
> El orquestador de Docker Swarm de Azure Container Service utiliza Swarm, que es independiente y heredado. Actualmente, el modo [Swarm](https://docs.docker.com/engine/swarm/) integrado (en Docker 1.12 y versiones posteriores) no es un orquestador admitido en Azure Container Service. Por eso, usamos [ACS Engine](https://github.com/Azure/acs-engine/blob/master/docs/swarmmode.md), una [plantilla de inicio rápido](https://azure.microsoft.com/resources/templates/101-acsengine-swarmmode/) a la que ha contribuido la comunidad, o una solución de Docker en [Azure Marketplace](https://azuremarketplace.microsoft.com).
>

## <a name="step-1-configure-your-azure-devops-organization"></a>Paso 1: Configuración de la organización de Azure DevOps 

En esta sección, se va a configurar una organización de Azure DevOps. Para configurar los puntos de conexión de Azure DevOps Services, en el proyecto de Azure DevOps, haga clic en el icono **Configuración** de la barra de herramientas y seleccione **Servicios**.

![Abrir punto de conexión de servicios](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/services-vsts.PNG)

### <a name="connect-azure-devops-and-azure-account"></a>Conexión de Azure DevOps con la cuenta de Azure

Configure una conexión entre el proyecto de Azure DevOps y la cuenta de Azure.

1. En el lado izquierdo, haga clic en **Nuevo punto de conexión de servicio** > **Azure Resource Manager**.
2. Para autorizar a Azure DevOps para que trabaje con su cuenta de Azure, seleccione su **suscripción** y haga clic en **Aceptar**.

    ![Azure DevOps (autorizar a Azure)](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-azure.PNG)

### <a name="connect-azure-devops-and-github"></a>Conexión entre Azure DevOps y GitHub

Configure una conexión entre el proyecto de Azure DevOps y su cuenta de GitHub.

1. En el lado izquierdo, haga clic en **New Service Endpoint** > **GitHub** (Nuevo punto de conexión de servicio > GitHub).
2. Para autorizar a Azure DevOps para trabajar con su cuenta de GitHub, haga clic en **Autorizar** y siga el procedimiento en la ventana que se abre.

    ![Azure DevOps (autorizar GitHub)](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-github.png)

### <a name="connect-azure-devops-to-your-azure-container-service-cluster"></a>Conexión de Azure DevOps a un clúster de Azure Container Service

Los últimos pasos antes de acceder a la canalización de CI/CD son configurar conexiones externas a un clúster de Docker Swarm en Azure. 

1. Para el clúster de Docker Swarm, agregue un punto de conexión de tipo **SSH**. Luego, escriba la información de la conexión de SSH de su clúster de Swarm (nodo maestro).

    ![Azure DevOps (SSH)](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-ssh.png)

Ahora ya está lista toda la configuración. En los siguientes pasos, se crea la canalización de CI/CD que compila e implementa la aplicación en el clúster de Docker Swarm. 

## <a name="step-2-create-the-build-pipeline"></a>Paso 2: Creación de la canalización de compilación

En este paso, se va a configurar una canalización de compilación para el proyecto de Azure DevOps y también se va a definir el flujo de trabajo de compilación de las imágenes de contenedor.

### <a name="initial-pipeline-setup"></a>Configuración inicial de la canalización

1. Para crear una canalización de compilación, conéctese al proyecto de Azure DevOps y haga clic en **Compilación y versión**. En la sección **Build definitions** (Definiciones de compilación), haga clic en **+ Nuevo**. 

    ![Azure DevOps (nueva canalización de compilación)](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/create-build-vsts.PNG)

2. Seleccione el **proceso vacío**.

    ![Azure DevOps (nueva canalización de compilación vacía)](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/create-empty-build-vsts.PNG)

4. Luego, haga clic en la pestaña **Variables** y cree dos nuevas variables: **RegistryURL** y **AgentURL**. Pegue los valores de su registro y DNS de agentes de clúster.

    ![Azure DevOps (configuración de las variables de compilación)](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-build-variables.png)

5. En la página **Definiciones de compilación**, abra la pestaña **Desencadenadores** y configure la compilación para la integración continua con la bifurcación del proyecto MyShop que creó en los requisitos previos. Luego, seleccione **Cambios del lote**. Asegúrese de que selecciona *docker-linux* en **Especificación de rama**.

    ![Azure DevOps (configuración del repositorio de compilación)](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-github-repo-conf.PNG)


6. Por último, haga clic en la pestaña **Opciones** y en Cola de agente predeterminada, seleccione **Hosted Linux Preview**.

    ![Azure DevOps (configuración del agente del host)](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-build-agent.png)

### <a name="define-the-build-workflow"></a>Definición del flujo de trabajo de compilación
Los pasos siguientes definen el flujo de trabajo de compilación. En primer lugar, es preciso que configure el origen del código. Para hacerlo, seleccione **GitHub** y su **repositorio** y **rama** (docker-linux).

![Azure DevOps (configuración del origen del código)](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-source-code.png)

Hay cinco imágenes de contenedor para compilar para la aplicación *MyShop*. Cada imagen se compila mediante el Dockerfile ubicado en las carpetas del proyecto:

* ProductsApi
* Proxy
* RatingsApi
* RecommendationsApi
* ShopFront

Necesita dos pasos de Docker para cada imagen, uno para compilarla y otro para insertar la imagen en Azure Registry Container. 

1. Para agregar un paso en el flujo de trabajo de compilación, haga clic en **+ Add build step** (+Agregar paso de compilación) y seleccione **Docker**.

    ![Azure DevOps (adición de pasos de compilación)](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-build-add-task.png)

2. Para cada imagen, configure un paso que use el comando `docker build`.

    ![Azure DevOps (compilación de Docker)](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-docker-build.png)

    Para la operación de compilación, seleccione su instancia de Azure Container Registry, la acción **Build an image** (Compilar una imagen) y el Dockerfile que define cada imagen. En **Directorio de trabajo**, seleccione el directorio raíz de Dockerfile, rellene el campo **Nombre de la imagen** y seleccione **Incluir etiqueta " latest"** .
    
    El valor de Nombre de la imagen debe estar en este formato: ```$(RegistryURL)/[NAME]:$(Build.BuildId)```. Reemplace **[NAME]** por el nombre de la imagen:
    - ```proxy```
    - ```products-api```
    - ```ratings-api```
    - ```recommendations-api```
    - ```shopfront```

3. Para cada imagen, configure un segundo paso que use el comando `docker push`.

    ![Azure DevOps (inserción de Docker)](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-docker-push.png)

    Para la operación de inserción, seleccione su instancia de Azure Container Registry, la acción **Push an image** (Insertar una imagen), especifique el valor de **Nombre de la imagen** y que se compiló en el paso anterior y seleccione **Incluir etiqueta " latest"** .

4. Después de configurar los pasos de compilación e inserción de cada una de las cinco imágenes, agregue tres pasos más al flujo de trabajo de compilación.

   ![Azure DevOps (adición de una tarea de la línea de comandos)](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-build-command-task.png)

   1. Una tarea de la línea de comandos que usa un script de bash para reemplazar la aparición de *BuildNumber* en el archivo docker-compose.yml por la variable RegistryURL. 
    
       ```-c "sed -i 's/RegistryUrl/$(RegistryURL)/g' src/docker-compose-v3.yml"```

       ![Azure DevOps (actualización del archivo de Compose con la dirección URL del registro)](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-build-replace-registry.png)

   2. Una tarea de la línea de comandos que usa un script de bash para reemplazar la aparición de *AgentURL* en el archivo docker-compose.yml por la variable AgentURL.
  
       ```-c "sed -i 's/AgentUrl/$(AgentURL)/g' src/docker-compose-v3.yml"```

      1. Una tarea que quita el archivo de Compose actualizado como un artefacto de compilación para que se pueda usar en la versión. Consulte la siguiente pantalla para más información.

      ![Azure DevOps (publicación del artefacto)](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-publish.png) 

      ![Azure DevOps (publicación del archivo de Compose)](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-publish-compose.png) 

5. Haga clic en **Guardar y poner en cola** para probar la canalización de compilación.

   ![Azure DevOps (Guardar y poner en cola)](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-build-save.png) 

   ![Azure DevOps (Nueva cola)](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-build-queue.png) 

6. Si la **compilación** es correcta, tiene que verá esta pantalla:

   ![Azure DevOps (Compilación correcta)](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-build-succeeded.png) 

## <a name="step-3-create-the-release-pipeline"></a>Paso 3: Creación de la canalización de versión

Azure DevOps permite [administrar versiones entre entornos](https://www.visualstudio.com/team-services/release-management/). Puede habilitar la implementación continua para asegurarse de que la aplicación se implementa en sus diferentes entornos (por ejemplo, desarrollo, prueba, preproducción y producción) sin problemas. Puede crear un entorno que represente el clúster del modo Docker Swarm de Azure Container Service.

![Azure DevOps (versión para ACS)](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-release-acs.png) 

### <a name="initial-release-setup"></a>Configuración inicial de la versión.

1. Para crear una canalización de versión, haga clic en **Versiones** >  **+ Versión**.

2. Para configurar el origen de artefacto, haga clic en **Artefactos** > **Link an artifact source** (Vincular un origen de artefacto). Aquí, vincule esta nueva canalización de versión a la compilación definida en el paso anterior. A partir de ese momento, el archivo docker-compose.yml estará disponible en el proceso de versión.

    ![Azure DevOps (artefactos de versión)](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-release-artefacts.png) 

3. Para configurar el desencadenador de versión, haga clic en **Desencadenadores** y seleccione **Implementación continua**. Establezca el desencadenador en el mismo origen de artefacto. Este valor garantiza que cuando finaliza la compilación correctamente, se inicia una nueva versión.

    ![Azure DevOps (desencadenadores de versión)](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-release-trigger.png) 

4. Para configurar las variables de la versión, haga clic en **Variables** y seleccione **+ Variable** para crear tres nuevas variables con la información del registro: **docker.username**, **docker.password** y **docker.registry**. Pegue los valores de su registro y DNS de agentes de clúster.

    ![Azure DevOps (configuración del repositorio de compilación)](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-release-variables.png)

    >[!IMPORTANT]
    > Como se muestra en la pantalla anterior, haga clic en la casilla **Bloquear** de docker.password. Este valor es importante restringir la contraseña.
    >

### <a name="define-the-release-workflow"></a>Definición del flujo de trabajo de versión

El flujo de trabajo de versión se compone de dos tareas que agrega.

1. Configure una tarea para que copie de forma segura el archivo de Compose en una carpeta de *implementación* en el nodo maestro de Docker Swarm, mediante la conexión SSH configurada anteriormente. Consulte la siguiente pantalla para más información.
    
    Carpeta de origen:```$(System.DefaultWorkingDirectory)/MyShop-CI/drop```

    ![Azure DevOps (SCP de versión)](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-release-scp.png)

2. Configure una segunda tarea para que ejecute un comando de bash para ejecutar los comandos `docker` y `docker stack deploy` en el nodo maestro. Consulte la siguiente pantalla para más información.

    ```
    docker login -u $(docker.username) -p $(docker.password) $(docker.registry) && export DOCKER_HOST=:2375 && cd deploy && docker stack deploy --compose-file docker-compose-v3.yml myshop --with-registry-auth
    ```

    ![Azure DevOps (Bash de versión)](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-release-bash.png)

    El comando ejecutado en el maestro usa la CLI de Docker y la CLI de Docker-Compose para realizar las siguientes tareas:

   - Inicie sesión en Azure Container Registry (usa tres variables de compilación que se definen en la pestaña **Variables**)
   - Defina la variable **DOCKER_HOST** para que funcione con el punto de conexión de Swarm (:2375)
   - Vaya a la carpeta de *implementación* que se creó en la tarea de copia segura anterior y que contiene el archivo docker-compose.yml. 
   - Ejecute comandos `docker stack deploy` que extraigan las nuevas imágenes y creen los contenedores.

     >[!IMPORTANT]
     > Como se muestra en la pantalla anterior, deje la casilla **Error en STDERR** desactivada. Este valor nos permite completar el proceso de lanzamiento, porque `docker-compose` imprime varios mensajes de diagnóstico (por ejemplo, los contenedores se están deteniendo o se están eliminando), en la salida de error estándar. Si activa la casilla, Azure DevOps informa de que se produjeron errores durante la versión, incluso si todo va bien.
     >
3. Guarde esta nueva canalización de versión.

## <a name="step-4-test-the-cicd-pipeline"></a>Paso 4: Prueba de la canalización de CI/CD

Ahora que ha terminado con la configuración, es hora de probar esta nueva canalización de CI/CD. La manera más fácil de hacerlo es actualizar el código fuente y confirmar los cambios en el repositorio de GitHub. Unos pocos segundos después de insertar el código, verá una nueva compilación que se ejecuta en Azure DevOps. Una vez completada correctamente, se desencadena una nueva versión de la aplicación, que se implementa en el clúster de Azure Container Service.

## <a name="next-steps"></a>Pasos siguientes

* Para más información sobre CI/CD con Azure DevOps, consulte el artículo sobre la [documentación de Azure Pipelines](/azure/devops/pipelines/?view=azure-devops).
* Para más información acerca de ACS Engine, consulte el [repositorio de GitHub de ACS Engine](https://github.com/Azure/acs-engine).
* Para más información acerca del modo de Docker Swarm, consulte la [introducción al modo de Docker Swarm](https://docs.docker.com/engine/swarm/).
