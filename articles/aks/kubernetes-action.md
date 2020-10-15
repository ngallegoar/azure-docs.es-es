---
title: Compilación, prueba e implementación de contenedores en Azure Kubernetes Service mediante Acciones de GitHub
description: Más información sobre cómo usar Acciones de GitHub para implementar el contenedor en Kubernetes
services: container-service
author: azooinmyluggage
ms.topic: article
ms.date: 11/04/2019
ms.author: atulmal
ms.openlocfilehash: 7743a3a8d6e77affd6229b648ab79b5b2f07a0af
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "90564107"
---
# <a name="github-actions-for-deploying-to-kubernetes-service"></a>Acciones de GitHub para la implementación en el servicio Kubernetes

[Acciones de GitHub](https://help.github.com/en/articles/about-github-actions) le ofrece la flexibilidad de compilar un flujo de trabajo del ciclo de vida de desarrollo de software automatizado. La acción [azure/aks-set-context@v1](https://github.com/Azure/aks-set-context) de Kubernetes facilita las implementaciones en los clústeres de Azure Kubernetes Service. La acción establece el contexto de clúster de AKS de destino, que pueden usar otras acciones como [azure/k8s-deploy](https://github.com/Azure/k8s-deploy/tree/master), [azure/k8s-create-secret](https://github.com/Azure/k8s-create-secret/tree/master) etc. o ejecute cualquier comando de kubectl.

Un archivo YAML (.yml) define un flujo de trabajo en la ruta de acceso `/.github/workflows/` de su repositorio. En esta definición se incluyen los diversos pasos y parámetros que componen el flujo de trabajo.

Para un flujo de trabajo que tiene como destino AKS, el archivo tiene tres secciones:

|Sección  |Tareas  |
|---------|---------|
|**Autenticación** | Inicio de sesión en un registro de contenedor privado (ACR) |
|**Compilar** | Compilación e inserción de la imagen de contenedor en el registro  |
|**Implementación** | 1. Establezca el clúster de AKS de destino |
| |2. Cree un secreto genérico o del registro de Docker en el clúster de Kubernetes  |
||3. Implemente el clúster de Kubernetes|

## <a name="create-a-service-principal"></a>Creación de una entidad de servicio

Puede crear una [entidad de servicio](../active-directory/develop/app-objects-and-service-principals.md#service-principal-object) mediante el comando [az ad sp create-for-rbac](/cli/azure/ad/sp?view=azure-cli-latest#az-ad-sp-create-for-rbac) de la [CLI de Azure](/cli/azure/). Puede ejecutar este comando mediante [Azure Cloud Shell](https://shell.azure.com/) en Azure Portal o seleccionando el botón **Pruébelo**.

```azurecli-interactive
az ad sp create-for-rbac --name "myApp" --role contributor --scopes /subscriptions/<SUBSCRIPTION_ID>/resourceGroups/<RESOURCE_GROUP> --sdk-auth
```

En el comando anterior, reemplace los marcadores de posición por el identificador de la suscripción y el grupo de recursos. La salida son las credenciales de asignación de roles que proporcionan acceso a su aplicación. El comando debe generar un objeto JSON similar a este.

```json
  {
    "clientId": "<GUID>",
    "clientSecret": "<GUID>",
    "subscriptionId": "<GUID>",
    "tenantId": "<GUID>",
    (...)
  }
```
Copie este objeto JSON, el cual puede usar para autenticarse desde GitHub.

## <a name="configure-the-github-secrets"></a>Configuración de los secretos de GitHub

Siga los pasos para configurar los secretos:

1. En [GitHub](https://github.com/), examine el repositorio y seleccione **Configuración > Secretos > Agregar un nuevo secreto**.

    ![Captura de pantalla que muestra el vínculo Agregar un nuevo secreto para un repositorio.](media/kubernetes-action/secrets.png)

2. Pegue el contenido del comando `az cli` anterior como valor de la variable secreta. Por ejemplo, `AZURE_CREDENTIALS`.

3. Del mismo modo, defina los siguientes secretos adicionales para las credenciales del registro de contenedor y establézcalos en la acción de inicio de sesión de Docker. 

    - REGISTRY_USERNAME
    - REGISTRY_PASSWORD

4. Verá los secretos como se muestran a continuación una vez definidos.

    ![Captura de pantalla que muestra los secretos existentes de un repositorio.](media/kubernetes-action/kubernetes-secrets.png)

##  <a name="build-a-container-image-and-deploy-to-azure-kubernetes-service-cluster"></a>Creación de una imagen de contenedor e implementación en el clúster de Azure Kubernetes Service

La compilación y la extracción de las imágenes de contenedor se realizan mediante la acción `Azure/docker-login@v1`. Para implementar una imagen de contenedor en AKS, debe usar la acción `Azure/k8s-deploy@v1`. Esta acción tiene cinco parámetros:

| **Parámetro**  | **Explicación**  |
|---------|---------|
| **namespace** | (Opcional) Elija el espacio de nombres de Kubernetes de destino. Si no se proporciona el espacio de nombres, los comandos se ejecutarán en el espacio de nombres predeterminado. | 
| **manifests** |  (Obligatorio) Ruta de acceso a los archivos de manifiesto que se usará para la implementación. |
| **images** | (Obligatorio) Dirección URL completa de recursos de las imágenes que se van a usar para sustituciones sobre los archivos de manifiesto. |
| **imagepullsecrets** | (Opcional) Nombre de un secreto del registro de Docker que ya se ha configurado dentro del clúster. Cada uno de estos nombres de secreto se agrega en el campo imagePullSecrets para las cargas de trabajo que se encuentran en los archivos de manifiesto de entrada. |
| **kubectl-version** | (Opcional) Instala una versión específica del archivo binario kubectl. |

### <a name="deploy-to-azure-kubernetes-service-cluster"></a>Implementación en un clúster de Azure Kubernetes Service

Flujo de trabajo integral para compilar imágenes de contenedor e implementar en un clúster de Azure Kubernetes Service.

```yaml
on: [push]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@master
    
    - uses: Azure/docker-login@v1
      with:
        login-server: contoso.azurecr.io
        username: ${{ secrets.REGISTRY_USERNAME }}
        password: ${{ secrets.REGISTRY_PASSWORD }}
    
    - run: |
        docker build . -t contoso.azurecr.io/k8sdemo:${{ github.sha }}
        docker push contoso.azurecr.io/k8sdemo:${{ github.sha }}
      
    # Set the target AKS cluster.
    - uses: Azure/aks-set-context@v1
      with:
        creds: '${{ secrets.AZURE_CREDENTIALS }}'
        cluster-name: contoso
        resource-group: contoso-rg
        
    - uses: Azure/k8s-create-secret@v1
      with:
        container-registry-url: contoso.azurecr.io
        container-registry-username: ${{ secrets.REGISTRY_USERNAME }}
        container-registry-password: ${{ secrets.REGISTRY_PASSWORD }}
        secret-name: demo-k8s-secret

    - uses: Azure/k8s-deploy@v1
      with:
        manifests: |
          manifests/deployment.yml
          manifests/service.yml
        images: |
          demo.azurecr.io/k8sdemo:${{ github.sha }}
        imagepullsecrets: |
          demo-k8s-secret
```

## <a name="next-steps"></a>Pasos siguientes

Puede encontrar nuestro conjunto de acciones en distintos repositorios de GitHub, cada uno con documentación y ejemplos para ayudarlo a usar GitHub para CI/CD e implementar sus aplicaciones en Azure.

- [setup-kubectl](https://github.com/Azure/setup-kubectl)

- [k8s-set-context](https://github.com/Azure/k8s-set-context)

- [aks-set-context](https://github.com/Azure/aks-set-context)

- [k8s-create-secret](https://github.com/Azure/k8s-create-secret)

- [k8s-deploy](https://github.com/Azure/k8s-deploy)

- [webapps-container-deploy](https://github.com/Azure/webapps-container-deploy)

- [actions-workflow-samples](https://github.com/Azure/actions-workflow-samples)
