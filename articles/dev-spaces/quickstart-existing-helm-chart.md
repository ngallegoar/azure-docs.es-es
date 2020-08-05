---
title: Desarrollo de una aplicación con un gráfico de Helm existente en Kubernetes
services: azure-dev-spaces
ms.date: 04/21/2020
ms.topic: quickstart
description: En este inicio rápido se muestra cómo usar Azure Dev Spaces y la línea de comandos para desarrollar una aplicación con un gráfico de Helm existente en Azure Kubernetes Service.
keywords: Docker, Kubernetes, Azure, AKS, Azure Kubernetes Service, contenedores, Helm, service mesh, enrutamiento de service mesh, kubectl, k8s
manager: gwallace
ms.custom: devx-track-javascript
ms.openlocfilehash: 7d1d782203401607d3909d79d1f0f3a499712c32
ms.sourcegitcommit: e71da24cc108efc2c194007f976f74dd596ab013
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/29/2020
ms.locfileid: "87414316"
---
# <a name="quickstart-develop-an-application-with-an-existing-helm-chart-on-kubernetes---azure-dev-spaces"></a>Inicio rápido: Desarrollo de una aplicación con un gráfico de Helm existente en Kubernetes (Azure Dev Spaces)
En esta guía, aprenderá a:

- Configurar Azure Dev Spaces con un clúster de Kubernetes administrado en Azure.
- Ejecute una aplicación con un gráfico de Helm existente en AKS mediante Azure Dev Spaces en la línea de comandos.

## <a name="prerequisites"></a>Prerrequisitos

- Suscripción a Azure. Si no tiene una suscripción a Azure, puede crear una [cuenta gratuita](https://azure.microsoft.com/free).
- [La CLI de Azure instalada](/cli/azure/install-azure-cli?view=azure-cli-latest).

## <a name="create-an-azure-kubernetes-service-cluster"></a>Creación de un clúster de Azure Kubernetes Service

Debe crear un clúster de AKS en una [región admitida][supported-regions]. Los siguientes comandos permiten crear un grupo de recursos llamado *MyResourceGroup* y un clúster de AKS denominado *MyAKS*.

```azurecli
az group create --name MyResourceGroup --location eastus
az aks create -g MyResourceGroup -n MyAKS --location eastus --generate-ssh-keys
```

## <a name="enable-azure-dev-spaces-on-your-aks-cluster"></a>Habilitar Azure Dev Spaces en el clúster de AKS

Use el comando `use-dev-spaces` para habilitar Dev Spaces en el clúster de AKS y siga las indicaciones. El siguiente comando permite habilitar Dev Spaces en el clúster *MyAKS* del grupo *MyResourceGroup* y crea un espacio de desarrollo llamado *dev*.

> [!NOTE]
> El comando `use-dev-spaces` también instalará la CLI de Azure Dev Spaces si aún no se ha instalado. La CLI de Azure Dev Spaces no se puede instalar en Azure Cloud Shell.

```azurecli
az aks use-dev-spaces -g MyResourceGroup -n MyAKS --space dev --yes
```

## <a name="get-sample-application-code"></a>Obtención del código de la aplicación de ejemplo

En este artículo, puede usar la [aplicación de ejemplo de Azure Dev Spaces](https://github.com/Azure/dev-spaces) para demostrar el uso de Azure Dev Spaces.

Clone la aplicación desde GitHub y vaya al directorio *dev-spaces/samples/python/getting-started/webfrontend*:

```cmd
git clone https://github.com/Azure/dev-spaces
cd dev-spaces/samples/python/getting-started/webfrontend
```

## <a name="prepare-the-application"></a>Preparar la aplicación

Para ejecutar la aplicación en Azure Dev Spaces, necesita un archivo Dockerfile y un gráfico de Helm. Para algunos lenguajes, como [Java][java-quickstart], [.NET Core][netcore-quickstart] y [Node.js][nodejs-quickstart], las herramientas de cliente de Azure Dev Spaces pueden generar todos los recursos que necesita. Para muchos otros lenguajes como Go, PHP y Python, las herramientas de cliente podrán generar el gráfico de Helm siempre y cuando pueda proporcionar un archivo Dockerfile válido. En este caso, la aplicación de ejemplo tiene un Dockerfile y un gráfico de Helm existentes.

Genere la configuración para ejecutar la aplicación con Azure Dev Spaces con el gráfico de Helm y el Dockerfile existentes mediante el comando `azds prep`:

```cmd
azds prep --enable-ingress --chart webfrontend/
```

Debe ejecutar el comando `prep` desde el directorio *dev-spaces/samples/python/getting-started/webfrontend* y especificar la ubicación del gráfico de Helm mediante `--chart`.

> [!NOTE]
> Puede que vea la advertencia: *ADVERTENCIA: Dockerfile could not be generated due to unsupported language* (No se pudo generar un Dockerfile debido a un lenguaje no admitido) al ejecutar `azds prep`. El comando `azds prep` intenta generar [un Dockerfile y un gráfico de Helm](how-dev-spaces-works-prep.md#prepare-your-code) para el proyecto, pero no sobrescribirá los archivos Dockerfile o gráficos de Helm existentes.

## <a name="build-and-run-code-in-kubernetes"></a>Compilación y ejecución de código en Kubernetes

Compile y ejecute el código en AKS mediante el comando `azds up`:

```cmd
$ azds up
Using dev space 'dev' with target 'MyAKS'
Synchronizing files...14s
Installing Helm chart...2s
Waiting for container image build...3s
Building container image...
Step 1/7 : FROM python:3
Step 2/7 : WORKDIR /python/webfrontend
Step 3/7 : RUN pip install flask
Step 4/7 : COPY webfrontend.py webfrontend.py
Step 5/7 : COPY public/ public/
Step 6/7 : EXPOSE 80
Step 7/7 : CMD ["python", "./webfrontend.py"]
Built container image in 45s
Waiting for container...25s
Service 'azds-543eae-dev-webfrontend' port 'http' is available at http://dev.service.1234567890abcdef1234.eus.azds.io/
Service 'azds-543eae-dev-webfrontend' port 80 (http) is available via port forwarding at http://localhost:52382
Press Ctrl+C to detach
...
```

Para ver el servicio que se está ejecutando, abra la dirección URL pública que aparece en la salida del comando `azds up`. En este ejemplo, la dirección URL pública es `http://dev.service.1234567890abcdef1234.eus.azds.io/`.

> [!NOTE]
> Al ir al servicio mientras se ejecuta `azds up`, los seguimientos de solicitudes HTTP también se muestran en la salida del comando `azds up`. Estos seguimientos pueden ayudarle a solucionar problemas y depurar el servicio. Puede deshabilitar estos seguimientos con `--disable-http-traces` al ejecutar `azds up`.

Si detiene el comando `azds up` mediante *Ctrl + c*, el servicio continuará ejecutándose en AKS y la dirección URL pública seguirá estando disponible.

## <a name="update-code"></a>Actualización del código

Para implementar una versión actualizada del servicio, puede actualizar cualquier archivo del proyecto y volver a ejecutar el comando `azds up`. Por ejemplo:

1. Si `azds up` se está todavía ejecutando, presione *Ctrl + c*.
1. Actualice la [línea 13 en `webfrontend.py`](https://github.com/Azure/dev-spaces/blob/master/samples/python/getting-started/webfrontend/webfrontend.py#L13) a:
    
    ```javascript
        res.send('Hello from webfrontend in Azure');
    ```

1. Guarde los cambios.
1. Vuelva a ejecutar el comando `azds up`:

    ```cmd
    $ azds up
    Using dev space 'dev' with target 'MyAKS'
    Synchronizing files...11s
    Installing Helm chart...3s
    Waiting for container image build...
    ...    
    ```

1. Vaya al servicio en ejecución y observe los cambios.
1. Presione *Ctrl + C* para detener el comando `azds up`.

## <a name="clean-up-your-azure-resources"></a>Limpieza de los recursos de Azure

```azurecli
az group delete --name MyResourceGroup --yes --no-wait
```

## <a name="next-steps"></a>Pasos siguientes

Obtenga información acerca de la forma en que Azure Dev Spaces le ayuda a desarrollar aplicaciones más complejas en varios contenedores y cómo puede simplificar el desarrollo en colaboración mediante el uso de distintas versiones o bifurcaciones del código en distintos espacios.

> [!div class="nextstepaction"]
> [Desarrollo en equipo en Azure Dev Spaces][team-quickstart]

[java-quickstart]: quickstart-java.md
[nodejs-quickstart]: quickstart-nodejs.md
[netcore-quickstart]: quickstart-netcore.md
[team-quickstart]: quickstart-team-development.md
[supported-regions]: https://azure.microsoft.com/global-infrastructure/services/?products=kubernetes-service