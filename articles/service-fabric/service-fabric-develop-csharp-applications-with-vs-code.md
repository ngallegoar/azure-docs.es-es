---
title: Desarrollo de aplicaciones de .NET Core con Visual Studio Code
description: En este artículo se muestra cómo compilar, implementar y depurar aplicaciones de .NET Core para Service Fabric con Visual Studio Code.
author: peterpogorski
ms.topic: article
ms.date: 06/29/2018
ms.author: pepogors
ms.openlocfilehash: 1d7478e6b81ef2c53ca6194197336e91d3ff250b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "75614530"
---
# <a name="develop-c-service-fabric-applications-with-visual-studio-code"></a>Desarrollo de aplicaciones C# de Service Fabric con Visual Studio Code

La [extensión Service Fabric Reliable Services para VS Code](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-service-fabric-reliable-services) facilita la tarea de compilar aplicaciones de .NET Core para Service Fabric en sistemas operativos Windows, Linux y macOS.

En este artículo se muestra cómo compilar, implementar y depurar una aplicación de .NET Core para Service Fabric con Visual Studio Code.

## <a name="prerequisites"></a>Prerrequisitos

En este artículo se da por supuesto que ya ha instalado VS Code, la extensión Service Fabric Reliable Services para VS Code y todas las dependencias necesarias para el entorno de desarrollo. Para obtener más información, consulte la [introducción](./service-fabric-get-started-vs-code.md#prerequisites).

## <a name="download-the-sample"></a>Descarga del ejemplo
En este artículo se usa la aplicación CounterService en el [repositorio de GitHub de ejemplos de .NET Core para Service Fabric](https://github.com/Azure-Samples/service-fabric-dotnet-core-getting-started). 

Para clonar el repositorio en la máquina de desarrollo, ejecute el siguiente comando desde una ventana de terminal (ventana de comandos de Windows):

```
git clone https://github.com/Azure-Samples/service-fabric-dotnet-core-getting-started.git
```

## <a name="open-the-application-in-vs-code"></a>Abrir la aplicación en VS Code

### <a name="windows"></a>Windows
Haga clic con el botón derecho en el icono de VS Code en el menú Inicio y seleccione **Ejecutar como administrador**. Para asociar el depurador a los servicios, debe ejecutar VS Code como administrador.

### <a name="linux"></a>Linux
Con el terminal, vaya a la ruta de acceso /service-fabric-dotnet-core-getting-started/Services/CounterService desde el directorio a la que se clonó la aplicación localmente.

Ejecute el siguiente comando para abrir VS Code como usuario raíz de modo que el depurador pueda asociarse a los servicios.
```
sudo code . --user-data-dir='.'
```

La aplicación ahora debería aparecer en el área de trabajo de VS Code.

![Aplicación CounterService en el área de trabajo](./media/service-fabric-develop-csharp-applications-with-vs-code/counter-service-application-in-workspace.png)

## <a name="build-the-application"></a>Compilar la aplicación
1. Presione Ctrl+Mayús+P para abrir la **paleta de comandos** en VS Code.
2. Busque y seleccione el comando **Service Fabric: Build Application**. La salida de compilación se envía al terminal integrado.

   ![Comando de compilación de la aplicación en VS Code](./media/service-fabric-develop-csharp-applications-with-vs-code/sf-build-application.png)

## <a name="deploy-the-application-to-the-local-cluster"></a>Implementación de la aplicación en el clúster local
Después de haber compilado la aplicación, se puede implementar en el clúster local. 

1. En la **paleta de comandos**, seleccione el comando **Service Fabric: Deploy Application (Localhost)** . La salida del proceso de instalación se envía al terminal integrado.

   ![Comando de implementación de la aplicación en VS Code](./media/service-fabric-develop-csharp-applications-with-vs-code/sf-deploy-application.png)

4. Una vez completada la implementación, inicie un explorador y abra Service Fabric Explorer: http:\//localhost:19080/Explorer. Debería ver que la aplicación se está ejecutando. Esto puede tardar algún tiempo, tenga paciencia. 

   ![Aplicación CounterService en Service Fabric Explorer](./media/service-fabric-develop-csharp-applications-with-vs-code/sfx-verify-deploy.png)

4. Después de haber comprobado que la aplicación se está ejecutando, inicie un explorador y abra esta página: http:\//localhost:31002. Se trata del front-end web de la aplicación. Actualice la página para ver el valor actual del contador a medida que aumenta.

   ![Aplicación CounterService en el explorador](./media/service-fabric-develop-csharp-applications-with-vs-code/counter-service-running.png)

## <a name="publish-the-application-to-an-azure-service-fabric-cluster"></a>Publicación de una aplicación en un clúster de Azure Service Fabric
Además de implementar la aplicación en el clúster local, también puede publicar la aplicación en un clúster remoto de Azure Service Fabric. 

1. Asegúrese de haber compilado la aplicación según las instrucciones anteriores. Actualice el archivo de configuración generado `Cloud.json` con los detalles del clúster remoto en el que desea realizar la publicación.

2. En la **paleta de comandos**, seleccione el comando **Service Fabric: Publicación de aplicación**. La salida del proceso de instalación se envía al terminal integrado.

   ![Comando Publicar aplicación en VS Code](./media/service-fabric-develop-csharp-applications-with-vs-code/sf-publish-application.png)

3. Una vez completada la implementación, inicie un explorador y abra Service Fabric Explorer: `https:<clusterurl>:19080/Explorer`. Debería ver que la aplicación se está ejecutando. Esto puede tardar algún tiempo, tenga paciencia. 

## <a name="debug-the-application"></a>Depuración de la aplicación
Al depurar aplicaciones en VS Code, debe ejecutar la aplicación en un clúster local. A continuación, se pueden agregar puntos de interrupción en el código.

Para establecer un punto de interrupción y depurar, complete los pasos siguientes:
1. En el explorador, abra el archivo */src/CounterServiceApplication/CounterService/CounterService.cs* y establezca un punto de interrupción en la línea 62 dentro del método `RunAsync`.
3. Haga clic en el icono Depurar en la **barra de actividades** para abrir la vista depurador de VS Code. Haga clic en el icono de engranaje en la parte superior de la vista de depurador y seleccione **.NET Core** en el menú desplegable del entorno. Se abre el archivo launch.json. Puede cerrar este archivo. Ahora debería ver las opciones de configuración en el menú de configuración de depuración situado junto al botón de ejecución (flecha verde).

   ![Icono Depurar en el área de trabajo de VS Code](./media/service-fabric-develop-csharp-applications-with-vs-code/debug-icon-workspace.png)

2. Seleccione **.NET Core Attach** en el menú de configuración de depuración.

   ![Icono Depurar en el área de trabajo de VS Code](./media/service-fabric-develop-csharp-applications-with-vs-code/debug-start.png)

3. Abra Service Fabric Explorer en un explorador: http:\//localhost:19080/Explorer. Haga clic en **Applications** (Aplicaciones) y explore en profundidad para determinar el nodo principal en que se ejecuta CounterService. En la imagen siguiente, del nodo principal para CounterService es el nodo 0.

   ![Nodo principal de CounterService](./media/service-fabric-develop-csharp-applications-with-vs-code/counter-service-primary-node.png)

4. En VS Code, haga clic en el icono de ejecución (flecha verde) junto a la configuración de depuración **.NET Core Attach**. En el cuadro de diálogo de selección de procesos, seleccione el proceso CounterService que se ejecuta en el nodo principal que identificó en el paso 4.

   ![Proceso principal](./media/service-fabric-develop-csharp-applications-with-vs-code/select-process.png)

5. El punto de interrupción en el archivo *CounterService.cs* se alcanzarán muy rápidamente. A continuación, puede explorar los valores de las variables locales. Utilice la barra de herramientas de depuración en la parte superior de VS Code para continuar la ejecución, recorrer paso a paso las líneas por procedimientos, depurar métodos paso a paso por instrucciones o recorrer paso a paso para salir del método actual. 

   ![Valores de depuración](./media/service-fabric-develop-csharp-applications-with-vs-code/breakpoint-hit.png)

6. Para finalizar la sesión de depuración, haga clic en el icono de enchufe de la barra de herramientas de depuración en la parte superior de VS Code.
   
   ![Desconexión del depurador](./media/service-fabric-develop-csharp-applications-with-vs-code/debug-bar-disconnect.png)
       
7. Cuando la depuración haya terminado, puede usar el comando **Service Fabric: Remove Application** para quitar la aplicación CounterService del clúster local. 

## <a name="next-steps"></a>Pasos siguientes

* Obtenga más información sobre el [desarrollo y la depuración de aplicaciones de Java para Service Fabric con VS Code](./service-fabric-develop-java-applications-with-vs-code.md).



