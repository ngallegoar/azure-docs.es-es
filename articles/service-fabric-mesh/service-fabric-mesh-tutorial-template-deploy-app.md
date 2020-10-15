---
title: 'Tutorial: Implementación de una aplicación en Azure Service Fabric Mesh'
description: En este tutorial, obtendrá información sobre cómo implementar una aplicación en Service Fabric Mesh mediante una plantilla.
author: georgewallace
ms.topic: tutorial
ms.date: 01/11/2019
ms.author: gwallace
ms.custom: mvc, devcenter
ms.openlocfilehash: cc4912545bedb650268b3d8e4a3e9820b70b5fe2
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "91842536"
---
# <a name="tutorial-deploy-an-application-to-service-fabric-mesh-using-a-template"></a>Tutorial: implementar una aplicación en Service Fabric Mesh mediante una plantilla

Este tutorial es la primera parte de una serie. Obtendrá información sobre cómo implementar una aplicación de Azure Service Fabric Mesh mediante una plantilla.  La aplicación se compone de un servicio front-end web ASP.NET y un servicio back-end ASP.NET Core Web API, que se encuentran en Docker Hub.  Extraiga las dos imágenes de contenedor de Docker Hub y luego insértelas en su propio registro privado. A continuación, cree una plantilla de Azure RM para la aplicación e implemente la aplicación desde el registro de contenedor en Service Fabric Mesh. Cuando haya terminado, tendrá una sencilla aplicación de lista de tareas pendientes que se ejecuta en Service Fabric Mesh.

En la primera parte de la serie, se aprende a:

> [!div class="checklist"]
> * Crear una instancia de Azure Container Registry privada
> * Insertar una imagen de contenedor en el registro
> * Crear archivos de plantilla y parámetros de RM
> * Implementar una aplicación en Service Fabric Mesh

En esta serie de tutoriales, se aprende a:
> [!div class="checklist"]
> * Implementar una aplicación en Service Fabric Mesh mediante una plantilla
> * [Escalar servicios en una aplicación que se ejecute en Service Fabric Mesh](service-fabric-mesh-tutorial-template-scale-services.md)
> * [Actualizar una aplicación que se ejecuta en Service Fabric Mesh](service-fabric-mesh-tutorial-template-upgrade-app.md)
> * [Eliminar una aplicación](service-fabric-mesh-tutorial-template-remove-app.md)

[!INCLUDE [preview note](./includes/include-preview-note.md)]

## <a name="prerequisites"></a>Prerequisites

Antes de empezar este tutorial:

* Si no tiene una suscripción a Azure, puede [crear una cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

* [Instalación de Docker](service-fabric-mesh-howto-setup-developer-environment-sdk.md#install-docker)

* [Instale localmente la CLI de Azure y la CLI de Service Fabric Mesh](service-fabric-mesh-howto-setup-cli.md#install-the-azure-service-fabric-mesh-cli).

## <a name="create-a-container-registry"></a>Creación de un Registro de contenedor

Las imágenes de contenedor asociadas con los servicios en la aplicación de Service Fabric Mesh deben almacenarse en un registro de contenedor.  En este tutorial, utilizaremos una instancia de Azure Container Registry (ACR) privada. 

Siga los pasos siguientes para crear una instancia de ACR.  Si ya tiene una instalación de una instancia de ACR, puede omitir este paso y pasar directamente al siguiente.

### <a name="sign-in-to-azure"></a>Inicio de sesión en Azure

Inicie sesión en Azure y establezca la suscripción activa.

```azurecli
az login
az account set --subscription "<subscriptionName>"
```

### <a name="create-a-resource-group"></a>Crear un grupo de recursos

Un grupo de recursos de Azure es un contenedor lógico en el que se implementan y se administran los recursos de Azure. Utilice el comando siguiente para crear un grupo de recursos denominado *myResourceGroup* en la ubicación *eastus*.

```azurecli
az group create --name myResourceGroup --location eastus
```

### <a name="create-the-container-registry"></a>Creación de un registro de contenedor

Cree una instancia de ACR mediante el comando `az acr create`. El nombre del registro debe ser único dentro de Azure y contener entre 5 y 50 caracteres alfanuméricos. En el ejemplo siguiente, se usa el nombre *myContainerRegistry*. Si se produce un error que el nombre del registro está en uso, elija un nombre diferente.

```azurecli
az acr create --resource-group myResourceGroup --name myContainerRegistry --sku Basic
```

Al crearse el registro, verá un resultado similar al siguiente:

```json
{
  "adminUserEnabled": false,
  "creationDate": "2018-09-13T19:43:57.388203+00:00",
  "id": "/subscriptions/<subscription>/resourceGroups/myResourceGroup/providers/Microsoft.ContainerRegistry/registries/myContainerRegistry",
  "location": "eastus",
  "loginServer": "mycontainerregistry.azurecr.io",
  "name": "myContainerRegistry",
  "provisioningState": "Succeeded",
  "resourceGroup": "myResourceGroup",
  "sku": {
    "name": "Basic",
    "tier": "Basic"
  },
  "status": null,
  "storageAccount": null,
  "tags": {},
  "type": "Microsoft.ContainerRegistry/registries"
}
```

## <a name="push-the-images-to-azure-container-registry"></a>Insertar las imágenes en Azure Container Registry

En este tutorial, la aplicación de ejemplo de lista de tareas pendientes se utiliza como ejemplo.  El contenedor de imágenes para los servicios [WebFrontEnd](https://hub.docker.com/r/seabreeze/azure-mesh-todo-webfrontend/) y [ToDoService](https://hub.docker.com/r/seabreeze/azure-mesh-todo-service/) puede encontrarse en Docker Hub. Para obtener información acerca de cómo compilar la aplicación en Visual Studio, consulte [Compilación de una aplicación web de Service Fabric Mesh](service-fabric-mesh-tutorial-create-dotnetcore.md). Service Fabric Mesh puede ejecutar contenedores Docker de Linux o Windows.  Si está trabajando con contenedores de Linux, seleccione **Cambiar a contenedores de Linux** en Docker.  Si está trabajando con contenedores de Windows, seleccione **Cambiar a contenedores de Windows** en Docker.

Para insertar una imagen en una instancia de ACR, primero debe tener una imagen de contenedor. Si aún no tiene ninguna imagen de contenedor local, use el comando [docker pull](https://docs.docker.com/engine/reference/commandline/pull/) para extraer las imágenes [WebFrontEnd](https://hub.docker.com/r/seabreeze/azure-mesh-todo-webfrontend/) y [ToDoService](https://hub.docker.com/r/seabreeze/azure-mesh-todo-service/) de Docker Hub.

Extraiga las imágenes de Windows:

```bash
docker pull seabreeze/azure-mesh-todo-webfrontend:1.0-nanoserver-1709
docker pull seabreeze/azure-mesh-todo-service:1.0-nanoserver-1709
```

Para poder insertar una imagen en el registro, debe etiquetarla con el nombre completo del servidor de inicio de sesión de ACR.

Ejecute el siguiente comando para obtener el nombre del servidor de inicio de sesión completo de la instancia de ACR.

```azurecli
az acr list --resource-group myResourceGroup --query "[].{acrLoginServer:loginServer}" --output table

AcrLoginServer
---------------------------------
mycontainerregistry.azurecr.io
```

Ahora, etiquete la imagen de Docker mediante el comando [docker tag](https://docs.docker.com/engine/reference/commandline/tag/). En el ejemplo siguiente se etiquetan las imágenes seabreeze/azure-mesh-todo-service:1.0-nanoserver-1709 y seabreeze/azure-mesh-todo-webfrontend:1.0-nanoserver-1709. Si usa diferentes imágenes, sustituya esos nombres de imagen en el siguiente comando.

```bash
docker tag seabreeze/azure-mesh-todo-webfrontend:1.0-nanoserver-1709 mycontainerregistry.azurecr.io/seabreeze/azure-mesh-todo-webfrontend:1.0-nanoserver-1709
docker tag seabreeze/azure-mesh-todo-service:1.0-nanoserver-1709 mycontainerregistry.azurecr.io/seabreeze/azure-mesh-todo-service:1.0-nanoserver-1709
```

Inicie sesión en Azure Container Registry.

```azurecli
az acr login -n myContainerRegistry
```

Inserte la imagen en la instancia de ACR con el comando [docker push](https://docs.docker.com/engine/reference/commandline/push/):

```bash
docker push mycontainerregistry.azurecr.io/seabreeze/azure-mesh-todo-webfrontend:1.0-nanoserver-1709
docker push mycontainerregistry.azurecr.io/seabreeze/azure-mesh-todo-service:1.0-nanoserver-1709
```

### <a name="list-container-images"></a>Lista de imágenes de contenedor

Para ver los repositorios en la instancia de ACR, ejecute lo siguiente:

```azurecli
az acr repository list --name myContainerRegistry --output table

Result
-------------------------------
seabreeze/azure-mesh-todo-webfrontend
seabreeze/azure-mesh-todo-service
```

En el ejemplo siguiente se muestran las etiquetas del repositorio **azure-mesh-todo-service**.

```azurecli
az acr repository show-tags --name myContainerRegistry --repository seabreeze/azure-mesh-todo-service --output table

Result
--------
1.0-nanoserver-1709
```

La salida anterior confirma la presencia de `azure-mesh-todo-service:1.0-nanoserver-1709` en el registro de contenedor privado.  Compruebe también la presencia de `azure-mesh-todo-webfrontend:1.0-nanoserver-1709`.

## <a name="retrieve-credentials-for-the-registry"></a>Recuperar las credenciales para el registro

> [!IMPORTANT]
> En los escenarios de producción, no se recomienda habilitar al usuario administrador en una instancia de ACR. Se hace aquí para mayor comodidad. En los escenarios de producción, use una [entidad de servicio](../container-registry/container-registry-auth-service-principal.md) para la autenticación del usuario y del sistema.

Para implementar una instancia de contenedor del registro que se ha creado mediante una plantilla, debe proporcionar las credenciales de registro durante la implementación. En primer lugar, habilite al usuario administrador en el registro con el siguiente comando:

```azurecli
az acr update --name myContainerRegistry --admin-enabled true
```

A continuación, obtenga el nombre del servidor de inicio de sesión en el registro, el nombre de usuario y la contraseña mediante los siguientes comandos:

```azurecli
az acr list --resource-group myResourceGroup --query "[].{acrLoginServer:loginServer}" --output table
az acr credential show --name myContainerRegistry --query username
az acr credential show --name myContainerRegistry --query "passwords[0].value"
```

Use el nombre del servidor de inicio de sesión en ACR devuelto, el nombre de usuario y los valores de contraseña al crear los archivos de plantilla y parámetros de RM en la sección siguiente.

## <a name="download-and-explore-the-template-and-parameters-files"></a>Descargar y explorar los archivos de plantilla y parámetros

Una aplicación de Service Fabric Mesh es un recurso de Azure que puede implementar y administrar mediante plantillas de Azure Resource Manager (RM). Si no está familiarizado con los conceptos asociados a la implementación y administración de las soluciones de Azure, consulte [Introducción a Azure Resource Manager](../azure-resource-manager/management/overview.md) y [Definición de la estructura y la sintaxis de las plantillas de RM](../azure-resource-manager/templates/template-syntax.md).

En este tutorial, el ejemplo de la lista de tareas pendientes se utiliza como ejemplo.  En lugar de crear los nuevos archivos de plantilla y parámetros, descargue los archivos [mesh_rp.windows.json deployment template](https://github.com/Azure-Samples/service-fabric-mesh/blob/master/templates/todolist/mesh_rp.windows.json) y [mesh_rp.windows.parameter.json parameters](https://github.com/Azure-Samples/service-fabric-mesh/blob/master/templates/todolist/mesh_rp.windows.parameters.json).

### <a name="parameters"></a>Parámetros
Cuando haya valores en la plantilla que prevea cambiar una vez implementada la aplicación o si le gustaría tener la opción de cambiar a una base según implementación (si tiene previsto volver a usar esta plantilla para otras implementaciones), el procedimiento recomendado es parametrizar los valores. La manera adecuada de hacerlo es crear una sección "Parámetros" en la parte superior de la plantilla de implementación, donde puede especificar los nombres y las propiedades de los parámetros, a los que se hace referencia más adelante en la plantilla de implementación. La definición de cada parámetro incluye las secciones *type*, *defaultValue* y *metadata*, esta última opcional, con una *descripción*.

La sección de parámetros se define en la parte superior de la plantilla de implementación, justo antes de la sección *Recursos*:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json",
    "contentVersion": "1.0.0.0",
    "parameters": {
      ...
    },
    "resources": [
```

En la plantilla [mesh_rp.windows.json deployment template](https://github.com/Azure-Samples/service-fabric-mesh/blob/master/templates/todolist/mesh_rp.windows.json), se declaran los parámetros siguientes: location, registryPassword, registryUserName, registryServer, frontEndImage, serviceImage, frontEndCpu, serviceCpu, frontEndMemory, serviceMemory, frontEndReplicaCount, serviceReplicaCount.  Puede encontrar descripciones para cada parámetro en el archivo de plantilla de implementación.

Estos parámetros se usan en el archivo [mesh_rp.windows.parameter.json parámetros](https://github.com/Azure-Samples/service-fabric-mesh/blob/master/templates/todolist/mesh_rp.windows.parameters.json).  Mediante un archivo de parámetros independiente, puede cambiar los valores de los parámetros de una implementación a otra sin actualizar la plantilla de implementación.

### <a name="overview-of-the-application-and-services"></a>Información general de los servicios y las aplicaciones

Los servicios se especifican en la plantilla como propiedades del recurso de aplicación.  Las aplicaciones se implementan en una red privada, que se declara como un recurso en la plantilla.  Los servicios pueden usar volúmenes para conservar los datos, que se declaran como recursos en la plantilla.  Para cada servicio, el tipo de sistema operativo, los paquetes de código, el número de réplicas y la red se especifican como propiedades del servicio.  Para cada paquete de código, especifique la imagen de contenedor, los puntos de conexión, los recursos de memoria y CPU y las credenciales del repositorio de imágenes. A un nivel alto, la plantilla para una aplicación de Service Fabric Mesh con varios servicios tiene el aspecto siguiente:

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json",
  "contentVersion": "1.0.0.0",
  "parameters": {
    ...
  },
  "resources": [
    {
      "apiVersion": "2018-09-01-preview",
      "name": "MyMeshApplication",
      "type": "Microsoft.ServiceFabricMesh/applications",
      "location": "[parameters('location')]",
      "dependsOn": [
        "Microsoft.ServiceFabricMesh/networks/MeshAppNetwork",
        "Microsoft.ServiceFabricMesh/volumes/ServiceAVolume"
      ],
      "properties": {
        "services": [
          {
            "name": "ServiceA",
            "properties": {
              "description": "ServiceA description.",
              "osType": "Linux",
              "codePackages": [
                {
                  "name": "ServiceA",
                  "image": "[parameters('frontEndImage')]",
                  "volumeRefs": [
                    {
                      "name": "[resourceId('Microsoft.ServiceFabricMesh/volumes', 'ServiceAVolume')]",
                      "destinationPath": "/app/data"
                    }
                  ],
                  "endpoints": [
                    {
                      "name": "ServiceAListener",
                      "port": 80
                    }
                  ],
                  "resources": {
                    "requests": {
                      "cpu": "[parameters('frontEndCpu')]",
                      "memoryInGB": "[parameters('frontEndMemory')]"
                    }
                  },
                  "imageRegistryCredential": {
                    "server": "[parameters('registryServer')]",
                    "username": "[parameters('registryUserName')]",
                    "password": "[parameters('registryPassword')]"
                  }
                }
              ],
              "replicaCount": "[parameters('frontEndReplicaCount')]",
              "networkRefs": [
                {
                  "name": "[resourceId('Microsoft.ServiceFabricMesh/networks', 'MeshAppNetwork')]"
                }
              ]
            }
          },
          {
            "name": "ServiceB",
            ...
          }
        ],
        "description": "Application description."
      }
    },
    {
      "apiVersion": "2018-07-01-preview",
      "name": "MeshAppNetwork",
      "type": "Microsoft.ServiceFabricMesh/networks",
      "location": "[parameters('location')]",
      "dependsOn": [],
      "properties": {
        "description": "MeshAppNetwork description.",
        "addressPrefix": "10.0.0.4/22",
        "ingressConfig": {
          "layer4": [
            {
              "name": "ServiceAIngress",
              "publicPort": "20001",
              "applicationName": "MyMeshApplication",
              "serviceName": "ServiceA",
              "endpointName": "ServiceAListener"
            }
          ]
        }
      }
    },
    {
      "apiVersion": "2018-09-01-preview",
      "name": "ServiceAVolume",
      "type": "Microsoft.ServiceFabricMesh/volumes",
      "location": "[parameters('location')]",
      "dependsOn": [],
      "properties": {
        "description": "Azure Files storage volume for counter App.",
        "provider": "SFAzureFile",
        "azureFileParameters": {
          "shareName": "[parameters('fileShareName')]",
          "accountName": "[parameters('storageAccountName')]",
          "accountKey": "[parameters('storageAccountKey')]"
        }
      }
    }
  ]
}
```

Consulte el archivo [mesh_rp.windows.json deployment template](https://github.com/Azure-Samples/service-fabric-mesh/blob/master/templates/todolist/mesh_rp.windows.json) para conocer las características de la aplicación de lista de tareas pendientes.

## <a name="deploy-the-application-to-service-fabric-mesh"></a>Implementar la aplicación en Service Fabric Mesh
Cree la aplicación y los recursos relacionados mediante el siguiente comando, y proporcione las credenciales del paso anterior [Recuperar las credenciales del registro](#retrieve-credentials-for-the-registry).

En el archivo de parámetros, actualice los siguientes valores de los parámetros:

|Parámetro|Value|
|---|---|
|ubicación|La región donde se implementará la aplicación.  Por ejemplo: "eastus".|
|registryPassword|La contraseña que ha obtenido anteriormente en [Recuperar las credenciales del registro](#retrieve-credentials-for-the-registry). Este parámetro en la plantilla es una cadena segura y no se mostrará en el estado de implementación ni en los comandos `az mesh service show`.|
|registryUserName|El nombre de usuario que ha obtenido en [Recuperar las credenciales del registro](#retrieve-credentials-for-the-registry).|
|registryServer|El nombre de servidor de registro que ha obtenido en [Recuperar las credenciales del registro](#retrieve-credentials-for-the-registry).|
|frontEndImage|La imagen de contenedor para el servicio front-end.  Por ejemplo, `<myregistry>.azurecr.io/seabreeze/azure-mesh-todo-webfrontend:1.0-nanoserver-1709`.|
|serviceImage|La imagen de contenedor para el servicio back-end.  Por ejemplo, `<myregistry>.azurecr.io/seabreeze/azure-mesh-todo-service:1.0-nanoserver-1709`.|

Para implementar la aplicación, ejecute lo siguiente:

```azurecli
az mesh deployment create --resource-group myResourceGroup --template-file c:\temp\mesh_rp.windows.json --parameters c:\temp\mesh_rp.windows.parameters.json
```

Este comando producirá un fragmento de código de JSON que se muestra a continuación. En la sección ```outputs``` de la salida de JSON, copie la propiedad ```publicIPAddress```.

```json
"outputs": {
    "publicIPAddress": {
    "type": "String",
    "value": "40.83.78.216"
    }
}
```

Esta información proviene de la sección ```outputs``` de la plantilla ARM. Como se muestra a continuación, esta sección hace referencia al recurso de la puerta de enlace para capturar la dirección IP pública. 

```json
  "outputs": {
    "publicIPAddress": {
      "value": "[reference('todolistappGateway').ipAddress]",
      "type": "string"
    }
  }
```

## <a name="open-the-application"></a>Abrir la aplicación

Una vez que la aplicación se ha implementado correctamente, obtenga la dirección IP pública del punto de conexión de servicio. El comando de implementación devuelve la dirección IP pública del punto de conexión de servicio. Si lo desea, también puede consultar el recurso de red para buscar la dirección IP pública del punto de conexión de servicio. El nombre de los recursos de red de esta aplicación es `todolistappNetwork`; puede obtener información sobre él mediante el siguiente comando. 

```azurecli
az mesh gateway show --resource-group myResourceGroup --name todolistappGateway
```

Vaya a la dirección IP en un explorador web.

## <a name="check-application-status"></a>Comprobar el estado de la aplicación

Puede comprobar el estado de la aplicación mediante el comando "app show". El nombre de aplicación para la aplicación implementada es "todolistapp", por lo que puede obtener sus detalles.

```azurecli
az mesh app show --resource-group myResourceGroup --name todolistapp
```

Examine los registros de la aplicación implementada mediante el comando `az mesh code-package-log get`:
```azurecli
az mesh code-package-log get --resource-group myResourceGroup --application-name todolistapp --service-name WebFrontEnd --replica-name 0 --code-package-name WebFrontEnd
```

## <a name="next-steps"></a>Pasos siguientes

En esta parte del tutorial, ha aprendido a:

> [!div class="checklist"]
> * Crear un registro de contenedor privado
> * Insertar una imagen de contenedor en el registro
> * Crear archivos de plantilla y parámetros
> * Implementar una aplicación en Service Fabric Mesh

Avance hasta el siguiente tutorial:
> [!div class="nextstepaction"]
> [Escalar una aplicación que se ejecuta en Service Fabric Mesh](service-fabric-mesh-tutorial-template-scale-services.md)
