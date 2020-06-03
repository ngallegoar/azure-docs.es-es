---
title: Configuración de CI/CD con GitHub Actions
description: Aprenda a implementar su código en Azure App Service desde una canalización de CI/CD con GitHub Actions. Personalice las tareas de compilación y ejecute implementaciones complejas.
ms.devlang: na
ms.topic: article
ms.date: 10/25/2019
ms.author: jafreebe
ms.reviewer: ushan
ms.openlocfilehash: be6b5f0af17aa8343dcb74fd5f0710d44332ce0e
ms.sourcegitcommit: 1f48ad3c83467a6ffac4e23093ef288fea592eb5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/29/2020
ms.locfileid: "84193309"
---
# <a name="deploy-to-app-service-using-github-actions"></a>Implementación de App Service con Acciones de GitHub

[Acciones de GitHub](https://help.github.com/en/articles/about-github-actions) le ofrece la flexibilidad de compilar un flujo de trabajo del ciclo de vida de desarrollo de software automatizado. Gracias a las acciones de Azure App Service para GitHub, puede automatizar el flujo de trabajo para implementar en [Azure App Service](overview.md) con Acciones de GitHub.

> [!IMPORTANT]
> Acciones de GitHub está actualmente en versión beta. Primero debe [registrarse para unirse a la versión preliminar](https://github.com/features/actions) mediante su cuenta de GitHub.
> 

Un archivo YAML (.yml) define un flujo de trabajo en la ruta de acceso `/.github/workflows/` de su repositorio. En esta definición se incluyen los diversos pasos y parámetros que componen el flujo de trabajo.

Para un flujo de trabajo de Azure App Service, el archivo tiene tres secciones:

|Sección  |Tareas  |
|---------|---------|
|**Autenticación** | 1. Definición de una entidad de servicio <br /> 2. Creación de un secreto de GitHub |
|**Compilar** | 1. Configuración del entorno <br /> 2. Compilación de la aplicación web |
|**Implementación** | 1. Implementación de la aplicación web |

## <a name="create-a-service-principal"></a>Creación de una entidad de servicio

Puede crear una [entidad de servicio](../active-directory/develop/app-objects-and-service-principals.md#service-principal-object) mediante el comando [az ad sp create-for-rbac](https://docs.microsoft.com/cli/azure/ad/sp?view=azure-cli-latest#az-ad-sp-create-for-rbac) de la [CLI de Azure](https://docs.microsoft.com/cli/azure/). Puede ejecutar este comando mediante [Azure Cloud Shell](https://shell.azure.com/) en Azure Portal o seleccionando el botón **Pruébelo**.

```azurecli-interactive
az ad sp create-for-rbac --name "myApp" --role contributor --scopes /subscriptions/<subscription-id>/resourceGroups/<group-name>/providers/Microsoft.Web/sites/<app-name> --sdk-auth
```

En este ejemplo, reemplace los marcadores de posición del recurso por su identificador de suscripción, el nombre del grupo de recursos y el nombre de la aplicación. La salida son las credenciales de asignación de roles que proporcionan acceso a su aplicación de App Service. Copie este objeto JSON, el cual puede usar para autenticarse desde GitHub.

> [!NOTE]
> No es necesario crear una entidad de servicio si decide usar el perfil de publicación para la autenticación.

> [!IMPORTANT]
> Siempre es recomendable conceder acceso mínimo. Es por ello que el ámbito del ejemplo anterior se limita a la aplicación específica de App Service y no a todo el grupo de recursos.

## <a name="configure-the-github-secret"></a>Configuración del secreto de GitHub

También puede usar credenciales de nivel de aplicación, es decir, publicar el perfil para la implementación. Siga los pasos para configurar el secreto:

1. Descargue el perfil de publicación para la aplicación de App Service desde el portal con la opción **Obtener perfil de publicación**.

2. En [GitHub](https://github.com/), examine el repositorio y seleccione **Configuración > Secretos > Agregar un nuevo secreto**.

    ![secrets](media/app-service-github-actions/secrets.png)

3. Pegue el contenido del archivo del perfil de publicación descargado en el campo de valor del secreto.

4. Ahora en el archivo de flujo de trabajo de la rama: `.github/workflows/workflow.yml` reemplaza el secreto en el `publish-profile` de entrada de la implementación de la acción Aplicación web de Azure.
    
    ```yaml
        - uses: azure/webapps-deploy@v2
          with:
            publish-profile: ${{ secrets.azureWebAppPublishProfile }}
    ```

5. Una vez definidos, verá los secretos como se muestran a continuación.

    ![secrets](media/app-service-github-actions/app-service-secrets.png)

## <a name="set-up-the-environment"></a>Configuración del entorno

La configuración del entorno puede realizarse mediante una de las acciones de configuración.

|**Lenguaje**  |**Acción de configuración**  |
|---------|---------|
|**.NET**     | `actions/setup-dotnet` |
|**Java**     | `actions/setup-java` |
|**JavaScript** | `actions/setup-node` |
|**Python**     | `actions/setup-python` |

En los siguientes ejemplos se muestra la parte del flujo de trabajo que configura el entorno para los diversos lenguajes admitidos:

**JavaScript**

```yaml
    - name: Setup Node 10.x
      uses: actions/setup-node@v1
      with:
        node-version: '10.x'
```
**Python**

```yaml
    - name: Setup Python 3.6
      uses: actions/setup-python@v1
      with:
        python-version: 3.6
```

**.NET**

```yaml
    - name: Setup Dotnet 2.2.300
      uses: actions/setup-dotnet@v1
      with:
        dotnet-version: '2.2.300'
```

**Java**

```yaml
    - name: Setup Java 1.8.x
      uses: actions/setup-java@v1
      with:
        # If your pom.xml <maven.compiler.source> version is not in 1.8.x
        # Please change the Java version to match the version in pom.xml <maven.compiler.source>
        java-version: '1.8.x'
```

## <a name="build-the-web-app"></a>Compilación de la aplicación web

Esto depende del lenguaje y, para los lenguajes admitidos por Azure App Service, esta sección debe comprender los pasos de compilación estándar de cada lenguaje.

En los siguientes ejemplos se muestra la parte del flujo de trabajo que compila la aplicación web, en los diversos lenguajes admitidos:

**JavaScript**

```yaml
    - name: 'Run npm'
      shell: bash
      run: |
        # If your web app project is not located in your repository's root
        # Please change your directory for npm in pushd
        pushd .
        npm install
        npm run build --if-present
        npm run test --if-present
        popd
```

**Python**

```yaml
    - name: 'Run pip'
      shell: bash
      run: |
        # If your web app project is not located in your repository's root
        # Please change your directory for pip in pushd
        pushd .
        python -m pip install --upgrade pip
        pip install -r requirements.txt --target=".python_packages/lib/python3.6/site-packages"
        popd
```

**.NET**

```yaml
    - name: 'Run dotnet build'
      shell: bash
      run: |
        # If your web app project is not located in your repository's root
        # Please consider using pushd to change your path
        pushd .
        dotnet build --configuration Release --output ./output
        popd
```

**Java**

```yaml
    - uses: actions/checkout@v1
    - name: Set up JDK 1.8
      uses: actions/setup-java@v1
      with:
        java-version: 1.8
    - name: Build with Maven
      run: mvn -B package --file pom.xml
```
## <a name="deploy-to-app-service"></a>Implementación en App Service

Para implementar el código en una aplicación de App Service, utilice la acción `azure/webapps-deploy@v2`. Esta acción tiene cuatro parámetros:

| **Parámetro**  | **Explicación**  |
|---------|---------|
| **app-name** | (Obligatorio) Especifique el nombre de la aplicación de App Service. | 
| **publish-profile** | (Opcional) Publique el contenido del archivo de perfil con secretos de Web Deploy |
| **package** | (Opcional) Ruta de acceso al paquete o la carpeta. *.zip, *.war, *.jar o una carpeta para implementar |
| **slot-name** | (Opcional) Especifique un espacio existente que no sea el de producción. |

### <a name="deploy-using-publish-profile"></a>Implementación con el perfil de publicación

A continuación se muestra el flujo de trabajo de ejemplo para compilar e implementar una aplicación Node.js en Azure con el perfil de publicación.

```yaml
# File: .github/workflows/workflow.yml

on: push

jobs:
  build-and-deploy:
    runs-on: ubuntu-latest
    steps:
    # checkout the repo
    - name: 'Checkout GitHub Action' 
      uses: actions/checkout@master
    
    - name: Setup Node 10.x
      uses: actions/setup-node@v1
      with:
        node-version: '10.x'
    - name: 'npm install, build, and test'
      run: |
        npm install
        npm run build --if-present
        npm run test --if-present
       
    - name: 'Run Azure webapp deploy action using publish profile credentials'
          uses: azure/webapps-deploy@v2
          with: 
            app-name: node-rn
            publish-profile: ${{ secrets.azureWebAppPublishProfile }}
```

### <a name="deploy-using-azure-service-principal"></a>Implementación de una entidad de servicio de Azure

A continuación se muestra el flujo de trabajo de ejemplo para compilar e implementar una aplicación Node.js en Azure con una entidad de servicio de Azure.

```yaml
on: [push]

name: Node.js

jobs:
  build-and-deploy:
    runs-on: ubuntu-latest
    steps:
    # checkout the repo
    - name: 'Checkout GitHub Action' 
      uses: actions/checkout@master
   
    - uses: azure/login@v1
      with:
        creds: ${{ secrets.AZURE_CREDENTIALS }}
        
    - name: Setup Node 10.x
      uses: actions/setup-node@v1
      with:
        node-version: '10.x'
    
    - name: 'npm install, build, and test'
      run: |
        npm install
        npm run build --if-present
        npm run test --if-present
               
    # deploy web app using Azure credentials
    - uses: azure/webapps-deploy@v2
      with:
        app-name: 'node-rn'

    # Azure logout 
    - name: logout
      run: |
        az logout
```

## <a name="next-steps"></a>Pasos siguientes

Puede encontrar nuestro conjunto de acciones agrupadas en distintos repositorios de GitHub, cada uno con documentación y ejemplos que le ayudarán a usar GitHub con CI/CD y a implementar sus aplicaciones en Azure.

- [Flujo de trabajo de acciones para implementar en Azure](https://github.com/Azure/actions-workflow-samples)

- [Inicio de sesión de Azure](https://github.com/Azure/login)

- [Azure WebApp](https://github.com/Azure/webapps-deploy)

- [Azure WebApp for containers](https://github.com/Azure/webapps-container-deploy)

- [Inicio y cierre de sesión de Docker](https://github.com/Azure/docker-login)

- [Eventos que desencadenan flujos de trabajo](https://help.github.com/en/articles/events-that-trigger-workflows)

- [Implementación de K8s](https://github.com/Azure/k8s-deploy)

- [Flujos de trabajo de inicio](https://github.com/actions/starter-workflows)
