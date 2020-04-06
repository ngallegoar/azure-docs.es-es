---
title: Creación de una función de Azure con Java e IntelliJ
description: Aprenda a crear y publicar una aplicación sencilla sin servidor desencadenada por HTTP en Azure con Java e IntelliJ.
author: jeffhollan
ms.topic: conceptual
ms.date: 07/01/2018
ms.author: jehollan
ms.custom: mvc, devcenter
ms.openlocfilehash: 7003dc19a7bfc405809de91534028aba8e0416c5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79136857"
---
# <a name="create-your-first-azure-function-with-java-and-intellij"></a>Creación de la primera función de Azure con Java e IntelliJ

Este artículo le muestra:
- Cómo crear un proyecto de función [sin servidor](https://azure.microsoft.com/overview/serverless-computing/) con IntelliJ IDEA y Apache Maven
- Pasos para probar y depurar la función en el entorno de desarrollo integrado (IDE) en su propio equipo
- Instrucciones para implementar el proyecto de función en Azure Functions

<!-- TODO ![Access a Hello World function from the command line with cURL](media/functions-create-java-maven/hello-azure.png) -->

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="set-up-your-development-environment"></a>Configurado su entorno de desarrollo

Para desarrollar una función con Java e IntelliJ, instale el software siguiente:

- [Kit de desarrolladores de Java](https://www.azul.com/downloads/zulu/) (JDK), versión 8
- [Apache Maven](https://maven.apache.org), versión 3.0 o posterior
- [IntelliJ IDEA](https://www.jetbrains.com/idea/download), versión Community o Ultimate con Maven
- [CLI de Azure](https://docs.microsoft.com/cli/azure)

> [!IMPORTANT]
> La variable de entorno JAVA_HOME se debe establecer en la ubicación de instalación del JDK para completar los pasos de este artículo.

 Se recomienda instalar [Azure Functions Core Tools, versión 2](functions-run-local.md#v2), que proporciona un entorno de desarrollo local para escribir, ejecutar y depurar Azure Functions.

## <a name="create-a-functions-project"></a>Creación de un proyecto de Functions

1. En IntelliJ IDEA, seleccione **Create New Project** (Crear nuevo proyecto).  
1. En la ventana **New Project** (Nuevo proyecto), seleccione **Maven** en el panel izquierdo.
1. Active la casilla **Create from archetype** (Crear desde arquetipo) y, a continuación, seleccione **Add Archetype** (Agregar arquetipo) para [azure-functions-archetype](https://mvnrepository.com/artifact/com.microsoft.azure/azure-functions-archetype).
1. En la ventana **Add Archetype** (Agregar arquetipo), complete los campos como se indica a continuación:
    - _GroupId_: com.microsoft.azure
    - _ArtifactId_: azure-functions-archetype
    - _Versión_: compruebe la versión más reciente y úsela en el [repositorio central](https://mvnrepository.com/artifact/com.microsoft.azure/azure-functions-archetype)
    ![Creación de un proyecto de Maven a partir del arquetipo en IntelliJ IDEA](media/functions-create-first-java-intellij/functions-create-intellij.png)  
1. Seleccione **Aceptar** y después **Siguiente**.
1. Especifique los detalles del proyecto actual y seleccione **Finish** (Finalizar).

Maven crea los archivos del proyecto en una carpeta nueva con el mismo nombre que el valor _ArtifactId_. El código generado en el proyecto es una función [desencadenada por HTTP](/azure/azure-functions/functions-bindings-http-webhook) simple que devuelve el cuerpo de la solicitud HTTP desencadenadora.

## <a name="run-functions-locally-in-the-ide"></a>Ejecución local de funciones en el IDE

> [!NOTE]
> Para ejecutar y depurar localmente las funciones, asegúrese de que [Azure Functions Core Tools, versión 2](functions-run-local.md#v2) está instalado.

1. Importe los cambios manualmente o habilite la [importación automática](https://www.jetbrains.com/help/idea/creating-and-optimizing-imports.html).
1. Abra la barra de herramientas **Maven Projects** (Proyectos de Maven).
1. Expanda **Lifecycle** (Ciclo de vida) y, a continuación, abra **package** (paquete). La solución se compila y empaqueta en un directorio de destino recién creado.
1. Expanda **Plugins** > **azure-functions** (Complementos > azure-functions) y abra **azure-functions:run** para iniciar el entorno de ejecución local de Azure Functions.  
  ![Barra de herramientas de Maven para Azure Functions](media/functions-create-first-java-intellij/functions-intellij-java-maven-toolbar.png)  

1. Cierre el cuadro de diálogo de ejecución cuando termine de probar la función. Solo puede haber un host de función activo y en ejecución en el entorno local a la vez.

## <a name="debug-the-function-in-intellij"></a>Depuración de la función en IntelliJ

1. Para iniciar el host de función en modo de depuración, agregue **-DenableDebug** como argumento al ejecutar la función. Puede cambiar la configuración en [maven goals](https://www.jetbrains.com/help/idea/maven-support.html#run_goal) (objetivos de maven) o ejecutar el siguiente comando en una ventana de terminal:  

   ```
   mvn azure-functions:run -DenableDebug
   ```

   Este comando hace que el host de función abra un puerto de depuración en 5005.

1. En el menú **Run** (Ejecutar), seleccione **Edit Configurations** (Editar configuraciones).
1. Seleccione **(+)** para agregar un elemento **Remote** (Remoto).
1. Complete los campos _Name_ (Nombre) y _Settings_ (Configuración) y, a continuación, seleccione **OK** para guardar la configuración.
1. Después de la configuración, seleccione **Debug < Remote Configuration Name >** (Depurar <Nombre de configuración remota>) o presione MAYÚS + F9 en el teclado para iniciar la depuración.

1. Cuando termine, detenga el depurador y el proceso en ejecución. Solo puede haber un host de función activo y en ejecución en el entorno local a la vez.

## <a name="deploy-the-function-to-azure"></a>Implementación de la función en Azure

1. Para poder implementar su función en Azure, debe [iniciar sesión mediante la CLI de Azure](/cli/azure/authenticate-azure-cli?view=azure-cli-latest).

   ``` azurecli
   az login
   ```

1. Implemente el código en una función nueva con el destino de Maven `azure-functions:deploy`. También puede seleccionar la opción **azure-functions:deploy** en la ventana de proyectos de Maven.

   ```
   mvn azure-functions:deploy
   ```

1. Busque la dirección URL de la función en la salida de la CLI de Azure después de que la función se haya implementado correctamente.

   ``` output
   [INFO] Successfully deployed Function App with package.
   [INFO] Deleting deployment package from Azure Storage...
   [INFO] Successfully deleted deployment package fabrikam-function-20170920120101928.20170920143621915.zip
   [INFO] Successfully deployed Function App at https://fabrikam-function-20170920120101928.azurewebsites.net
   [INFO] ------------------------------------------------------------------------
   ```

## <a name="next-steps"></a>Pasos siguientes

- Revise la [guía para desarrolladores de Java Functions](functions-reference-java.md) para más información sobre cómo desarrollar funciones de Java.
- Agregue funciones adicionales con desencadenadores distintos en el proyecto con el destino de Maven `azure-functions:add`.
