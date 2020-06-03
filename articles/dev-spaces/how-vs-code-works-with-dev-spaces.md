---
title: Cómo funciona Visual Studio Code con Azure Dev Spaces
services: azure-dev-spaces
ms.date: 07/08/2019
ms.topic: conceptual
description: Obtenga información sobre cómo Visual Studio Code y Azure Dev Spaces ayudan a depurar y a iterar rápidamente las aplicaciones de Kubernetes
keywords: Azure Dev Spaces, Dev Spaces, Docker, Kubernetes, Azure, AKS, Azure Kubernetes Service, contenedores
ms.openlocfilehash: 73a2ae1f603626acf7ec4d42838b301c88ee21da
ms.sourcegitcommit: 69156ae3c1e22cc570dda7f7234145c8226cc162
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/03/2020
ms.locfileid: "84307357"
---
# <a name="how-visual-studio-code-works-with-azure-dev-spaces"></a>Cómo funciona Visual Studio Code con Azure Dev Spaces

Puede usar Visual Studio Code y la [extensión Azure Dev Spaces][azds-extension] para preparar, ejecutar y depurar los servicios con Azure Dev Spaces. Con Visual Studio Code y la extensión Azure Dev Spaces, puede:

* Generar recursos para ejecutar y depurar servicios en AKS
* Ejecutar los servicios de Java, Node.js y .NET Core en un espacio de desarrollo
* Depurar directamente los servicios de Java, Node.js y .NET Core que se ejecutan en un espacio de desarrollo

## <a name="generate-assets"></a>Generar recursos

Visual Studio Code y la extensión Azure Dev Spaces generan los siguientes recursos para el proyecto:

* Dockerfiles para aplicaciones de Java con Maven, aplicaciones de Node.js y aplicaciones de .NET Core
* Gráficos de Helm para prácticamente cualquier lenguaje con un Dockerfile
* Un archivo `azds.yaml`, que es el [archivo de configuración de Azure Dev Spaces][azds-yaml] del proyecto
* Una carpeta `.vscode` con la configuración de inicio de Visual Studio Code del proyecto para aplicaciones de Java con Maven, aplicaciones de Node.js y aplicaciones de .NET Core

El Dockerfile, el gráfico de Helm y los archivos `azds.yaml` son los mismos recursos que se generan al ejecutar `azds prep`. Esos archivos también pueden usarse fuera de Visual Studio Code para ejecutar el proyecto en AKS, por ejemplo ejecutar `azds up`. La carpeta `.vscode` solo se usa por parte Visual Studio Code para ejecutar el proyecto en AKS desde Visual Studio Code.

## <a name="run-your-service-in-aks"></a>Ejecutar el servicio en AKS

Después de generar los recursos del proyecto, puede ejecutar los servicios de Java, Node.js y .NET Core en un espacio de desarrollo existente desde Visual Studio Code. En la página *Depurar* de Visual Studio Code, puede invocar la configuración de inicio del directorio `.vscode` para ejecutar el proyecto.

Debe crear el clúster de AKS y habilitar Azure Dev Spaces en el clúster fuera de Visual Studio Code. Puede volver a usar Dockerfiles, gráficos de Helm y archivos `azds.yaml` existentes creados fuera de Visual Studio Code, por ejemplo los recursos generados mediante la ejecución de `azds prep`. Si vuelve a usar recursos generados fuera de Visual Studio Code, aún debe tener un directorio `.vscode`. Visual Studio Code y la extensión Azure Dev Spaces pueden regenerar este directorio `.vscode` sin sobrescribir los recursos existentes.

En proyectos de .NET Core, debe tener instalada la [extensión de C#][csharp-extension] para ejecutar el servicio de .NET desde Visual Studio Code. Asimismo, para los proyectos de Java con Maven, debe tener instalada la [extensión de Java Debugger para Azure Dev Spaces][java-extension] además de [Maven instalado y configurado][maven] para ejecutar el servicio de Java desde Visual Studio Code.

## <a name="debug-your-service-in-aks"></a>Depurar el servicio en AKS

Después de iniciar el proyecto, puede depurar los servicios de Java, Node.js y .NET Core que se ejecutan en un espacio de desarrollo directamente desde Visual Studio Code. La configuración de inicio del directorio `.vscode` proporciona la información de depuración adicional necesaria para ejecutar un servicio con la depuración habilitada en un espacio de desarrollo. Visual Studio Code también se asocia al proceso de depuración del contenedor en ejecución en los espacios de desarrollo, lo que permite establecer puntos de interrupción, inspeccionar variables y realizar otras operaciones de depuración.

## <a name="use-visual-studio-code-with-azure-dev-spaces"></a>Usar Visual Studio Code con Azure Dev Spaces

Puede ver cómo funcionan Visual Studio Code y la extensión Azure Dev Spaces con Azure Dev Spaces en los inicios rápidos siguientes:

* [Iteración y depuración rápidas con Visual Studio Code y Java][quickstart-java]
* [Iteración y depuración rápidas con Visual Studio Code y .NET][quickstart-netcore]
* [Iteración y depuración rápidas con Visual Studio Code y Node.js][quickstart-node]

[azds-extension]: https://marketplace.visualstudio.com/items?itemName=azuredevspaces.azds
[azds-yaml]: how-dev-spaces-works-prep.md#prepare-your-code
[csharp-extension]: https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csharp
[java-extension]: https://marketplace.visualstudio.com/items?itemName=vscjava.vscode-java-debugger-azds
[maven]: https://maven.apache.org
[quickstart-java]: quickstart-java.md
[quickstart-netcore]: quickstart-netcore.md
[quickstart-node]: quickstart-nodejs.md
