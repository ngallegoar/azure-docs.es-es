---
title: Desarrollo de Azure Functions con Visual Studio
description: Obtenga información sobre cómo desarrollar y probar Azure Functions mediante Herramientas de Azure Functions para Visual Studio 2019.
ms.custom: vs-azure
ms.topic: conceptual
ms.date: 08/21/2019
ms.openlocfilehash: b3d683153a1e70f4c65dcc5e401f4ee702af8b49
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79235048"
---
# <a name="develop-azure-functions-using-visual-studio"></a>Desarrollo de Azure Functions con Visual Studio  

Visual Studio le permite desarrollar, probar e implementar funciones de la biblioteca de clases C# en Azure. Si esta es su primera experiencia con Azure Functions, puede obtener más información en [Introducción a Azure Functions](functions-overview.md).

Visual Studio proporcionan las siguientes ventajas al desarrollar funciones: 

* Editar, compilar y ejecutar funciones en el equipo de desarrollo local. 
* Publique su proyecto de Azure Functions directamente en Azure y cree recursos de Azure según sea necesario. 
* Use atributos de C# para declarar enlaces de función directamente en el código de C#.
* Desarrollar e implementar funciones de C# compiladas previamente. Las funciones compiladas previamente proporcionan un mejor rendimiento de arranque en frío que las funciones basadas en scripts de C#. 
* Programar las funciones en C# a la vez que se tienen todos los beneficios del desarrollo de Visual Studio. 

En este artículo se dan detalles sobre cómo usar Visual Studio para desarrollar funciones de biblioteca de clases C# y publicarlas en Azure. Antes de leer este artículo, debe completar la [guía de inicio rápido de Functions para Visual Studio](functions-create-your-first-function-visual-studio.md). 

A menos que se indique lo contrario, los procedimientos y los ejemplos que se muestran son para Visual Studio 2019. 

## <a name="prerequisites"></a>Prerrequisitos

Las herramientas de Azure Functions forman parte de la carga de trabajo de desarrollo de Azure de Visual Studio a partir de Visual Studio 2017. Asegúrese de incluir la carga de trabajo de **desarrollo de Azure** en la instalación de Visual Studio.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

Los demás recursos que necesita, como una cuenta de Azure Storage, se crean en la suscripción durante el proceso de publicación.

> [!NOTE]
> En Visual Studio 2017, la carga de trabajo de desarrollo de Azure instala las herramientas de Azure Functions como una extensión independiente. Al actualizar Visual Studio 2017, también debe asegurarse de que está usando la [versión más reciente](#check-your-tools-version) de las herramientas de Azure Functions. En las secciones siguientes se muestra cómo comprobar y, si es necesario, actualizar la extensión de herramientas de Azure Functions en Visual Studio 2017. 
>
> Omita esta sección al usar Visual Studio 2019.

### <a name="check-your-tools-version-in-visual-studio-2017"></a><a name="check-your-tools-version"></a>Comprobar la versión de las herramientas en Visual Studio 2017

1. En el menú **Herramientas**, seleccione **Extensiones y actualizaciones**. Expanda **Herramientas** > **instaladas** y elija **Azure Functions and Web Jobs Tools**.

    ![Comprobación de la versión de herramientas de Functions](./media/functions-develop-vs/functions-vstools-check-functions-tools.png)

1. Observe la **versión** instalada. Puede comparar esta versión con la versión más reciente que aparece [en las notas de la versión](https://github.com/Azure/Azure-Functions/blob/master/VS-AzureTools-ReleaseNotes.md). 

1. Si su versión es anterior, actualice las herramientas en Visual Studio como se muestra en la siguiente sección.

### <a name="update-your-tools-in-visual-studio-2017"></a>Actualización de las herramientas en Visual Studio 2017

1. En el cuadro de diálogo **Extensiones y actualizaciones**, expanda **Actualizaciones** > **Visual Studio Marketplace**, elija **Azure Functions and Web Jobs Tools** y seleccione **Actualizar**.

    ![Actualización de la versión de herramientas de Functions](./media/functions-develop-vs/functions-vstools-update-functions-tools.png)   

1. Después de descargar la actualización de las herramientas, cierre Visual Studio para desencadenar la actualización de las herramientas mediante el programa de instalación de VSIX.

1. En el programa de instalación, elija **Aceptar** para iniciar y, a continuación, **Modificar** para actualizar las herramientas. 

1. Una vez completada la actualización, elija **Cerrar** y reinicie Visual Studio.

> [!NOTE]  
> En Visual Studio 2019 y versiones posteriores, la extensión de herramientas de Azure Functions se actualiza como parte de Visual Studio.  

## <a name="create-an-azure-functions-project"></a>Creación de un proyecto de Azure Functions

[!INCLUDE [Create a project using the Azure Functions](../../includes/functions-vstools-create.md)]

La plantilla de proyecto crea un proyecto de C#, instala el paquete NuGet `Microsoft.NET.Sdk.Functions` y establece la plataforma de destino. El proyecto nuevo contiene los archivos siguientes:

* **host.json**: permite configurar el host de Functions. Esta configuración se aplica tanto cuando se ejecuta localmente como en Azure. Para más información, consulte la [referencia sobre host.json](functions-host-json.md).

* **local.settings.json**: mantiene la configuración que se usa cuando se ejecutan localmente las funciones. Esta configuración no se usa cuando se ejecuta en Azure. Para más información, consulte [Archivo de configuración local](#local-settings-file).

    >[!IMPORTANT]
    >Debido a que el archivo local.settings.json puede contener secretos, debe excluirlo del control de origen del proyecto. La opción **Copiar en el directorio de salida** para este archivo siempre debe ser **Copiar si es más nuevo**. 

Para más información, consulte [Proyecto de biblioteca de clases de Functions](functions-dotnet-class-library.md#functions-class-library-project).

[!INCLUDE [functions-local-settings-file](../../includes/functions-local-settings-file.md)]

La configuración en local.settings.json no se carga automáticamente cuando publica el proyecto. Para asegurarse de que esta configuración también existe en la aplicación de funciones de Azure, debe cargarla después de publicar el proyecto. Para más información, consulte [Function app settings](#function-app-settings) (Configuración de la aplicación de funciones).

Los valores de **ConnectionStrings** nunca se publican.

Esta configuración de la aplicación de función también se puede leer en el código como variables de entorno. Para más información, consulte [Variables de entorno](functions-dotnet-class-library.md#environment-variables).

## <a name="configure-the-project-for-local-development"></a>Configuración del proyecto para el desarrollo local

El entorno de tiempo de ejecución de Functions usa internamente una cuenta de Azure Storage. Para todos los tipos de desencadenadores distintos de HTTP y webhooks, debe establecer la clave **Values.AzureWebJobsStorage** en una cadena de conexión de cuenta de Azure Storage válida. La aplicación de función puede usar el [Emulador de Azure Storage](../storage/common/storage-use-emulator.md) para la configuración de conexión **AzureWebJobsStorage** que el proyecto requiere. Para usar el emulador, establezca el valor de **AzureWebJobsStorage** en `UseDevelopmentStorage=true`. Cambie esta configuración a una cadena de conexión de la cuenta de almacenamiento real antes de la implementación.

Para establecer la cadena de conexión de cuenta de almacenamiento:

1. En Visual Studio, abra **Cloud Explorer**, expanda **Cuenta de almacenamiento** > **Su cuenta de almacenamiento** y, a continuación, en la pestaña **Propiedades**, copie el valor **Cadena de conexión principal**.

2. En el proyecto, abra el archivo local.settings.json y establezca el valor de la clave **AzureWebJobsStorage** en la cadena de conexión que copió.

3. Repita el paso anterior para agregar claves únicas a la matriz de **valores** para cualquier otra conexión que requieran sus funciones. 

## <a name="add-a-function-to-your-project"></a>Incorporación de una función al proyecto

En las funciones de biblioteca de clases C#, los enlaces que la función usa se definen mediante la aplicación de atributos en el código. Al crear los desencadenadores de funciones a partir de las plantillas proporcionadas, los atributos del desencadenador se aplican automáticamente. 

1. En el **Explorador de soluciones**, haga clic con el botón derecho en el nodo del proyecto y seleccione **Agregar** > **Nuevo elemento**. Seleccione **Función de Azure**, escriba un **nombre** para la clase y, luego, haga clic en **Agregar**.

2. Elija el desencadenador, establezca las propiedades de enlace y haga clic en **Crear**. En el ejemplo siguiente se muestra la configuración cuando se crea una función desencadenada de Queue Storage. 

    ![Crear una función desencadenada por la cola](./media/functions-develop-vs/functions-vstools-create-queuetrigger.png)

    Este ejemplo de desencadenador usa una cadena de conexión con una clave denominada **QueueStorage**. Esta configuración de la cadena de conexión se debe definir en el [archivo local.settings.json](functions-run-local.md#local-settings-file).

3. Examine la clase recién agregada. Verá un método **Run** estático, que cuenta con el atributo **FunctionName**. Este atributo indica que el método es el punto de entrada de la función.

    Por ejemplo, la clase C# siguiente representa una función desencadenada de Queue Storage:

    ```csharp
    using System;
    using Microsoft.Azure.WebJobs;
    using Microsoft.Azure.WebJobs.Host;
    using Microsoft.Extensions.Logging;

    namespace FunctionApp1
    {
        public static class Function1
        {
            [FunctionName("QueueTriggerCSharp")]
            public static void Run([QueueTrigger("myqueue-items", 
                Connection = "QueueStorage")]string myQueueItem, ILogger log)
            {
                log.LogInformation($"C# Queue trigger function processed: {myQueueItem}");
            }
        }
    }
    ```

    Se aplica un atributo específico de enlace a cada parámetro de enlace que se suministra al método de punto de entrada. El atributo toma la información de enlace como parámetros. En el ejemplo anterior, el primer parámetro tiene aplicado un atributo **QueueTrigger**, que indica la función desencadenada de cola. El nombre de la configuración de cadena de conexión y el nombre de cola se pasan como parámetros en el atributo **QueueTrigger**. Para más información, consulte [Enlaces de Azure Queue Storage para Azure Functions](functions-bindings-storage-queue-trigger.md).

Puede utilizar el procedimiento anterior para agregar más funciones a su proyecto de aplicación de funciones. Cada función del proyecto puede tener un desencadenador diferente, pero una función no puede tener más de un desencadenador. Para más información, consulte [Conceptos básicos sobre los enlaces y desencadenadores de Azure Functions](functions-triggers-bindings.md).

## <a name="add-bindings"></a>Adición de enlaces

Al igual que sucede con los desencadenadores, los enlaces de entrada y salida se agregan a la función como atributos de enlace. Agregue los enlaces a una función como se indica a continuación:

1. Asegúrese de que ha [configurado el proyecto para desarrollo local](#configure-the-project-for-local-development).

2. Agregue el paquete de extensión de NuGet adecuado para el enlace concreto. Para más información, consulte [Desarrollo local con C# mediante Visual Studio](./functions-bindings-register.md#local-csharp) en el artículo Conceptos básicos sobre los enlaces y desencadenadores de Azure Functions. Los requisitos del paquete NuGet específico del enlace se encuentran en el artículo de referencia del enlace. Por ejemplo, buscar los requisitos del paquete para el desencadenador de Event Hubs en el [artículo de referencia Enlaces de Azure Event Hubs](functions-bindings-event-hubs.md).

3. Si hay valores de la aplicación que necesite el enlace, agréguelos a la colección **Valores** en el [archivo de configuración local](functions-run-local.md#local-settings-file). Estos valores se utilizan cuando la función se ejecuta localmente. Cuando la función se ejecuta en la aplicación de funciones en Azure, se usan los [valores de la aplicación de aplicaciones](#function-app-settings).

4. Agregue el atributo de enlace apropiado para la firma del método. En el ejemplo siguiente, un mensaje de la cola desencadena la función y el enlace de salida, se crea un nuevo mensaje de la cola con el mismo texto en otra cola.

    ```csharp
    public static class SimpleExampleWithOutput
    {
        [FunctionName("CopyQueueMessage")]
        public static void Run(
            [QueueTrigger("myqueue-items-source", Connection = "AzureWebJobsStorage")] string myQueueItem, 
            [Queue("myqueue-items-destination", Connection = "AzureWebJobsStorage")] out string myQueueItemCopy,
            ILogger log)
        {
            log.LogInformation($"CopyQueueMessage function processed: {myQueueItem}");
            myQueueItemCopy = myQueueItem;
        }
    }
    ```
   La conexión con el almacenamiento en la cola se obtiene del valor `AzureWebJobsStorage`. Para más información, consulte el artículo de referencia del enlace concreto. 

[!INCLUDE [Supported triggers and bindings](../../includes/functions-bindings.md)]

## <a name="testing-functions"></a>Funciones de prueba

Azure Functions Core Tools le permite ejecutar un proyecto de Azure Functions en el equipo de desarrollo local. Se le solicita que instale estas herramientas la primera vez que inicie una función de Visual Studio.

Para probar la función, presione F5. Si se le solicita, acepte la solicitud de Visual Studio para descargar e instalar las herramientas de Azure Functions Core (CLI). También es preciso que habilite una excepción de firewall para que las herramientas para controlen las solicitudes de HTTP.

Con el proyecto en ejecución, puede probar el código tal como probaría la función implementada. Para más información, consulte [Estrategias para probar el código en Azure Functions](functions-test-a-function.md). Cuando se ejecuta en modo de depuración, los puntos de interrupción se alcanzan en Visual Studio tal como se esperaba. 

<!---
For an example of how to test a queue triggered function, see the [queue triggered function quickstart tutorial](functions-create-storage-queue-triggered-function.md#test-the-function).  
-->

Para más información sobre cómo usar Azure Functions Core Tools, consulte [Codificación y comprobación de las funciones de Azure en un entorno local](functions-run-local.md).

## <a name="publish-to-azure"></a>Publicación en Azure

Al publicar desde Visual Studio, se usa uno de dos métodos de implementación:

* [Web Deploy](functions-deployment-technologies.md#web-deploy-msdeploy): empaqueta e implementa aplicaciones Windows en cualquier servidor IIS.
* [Implementación desde archivo ZIP con la ejecución desde un archivo de paquete habilitada](functions-deployment-technologies.md#zip-deploy): se recomienda para las implementaciones de Azure Functions.

Siga estos pasos para publicar el proyecto en una aplicación de funciones de Azure.

[!INCLUDE [Publish the project to Azure](../../includes/functions-vstools-publish.md)]

## <a name="function-app-settings"></a>Configuración de Function App

Cualquier configuración que agregue en local.settings.json debe agregarse a la aplicación de función en Azure. Esta configuración no se carga automáticamente cuando publica el proyecto.

La manera más sencilla de cargar la configuración requerida en la aplicación de función en Azure es usar el vínculo **Administrar configuración de la aplicación...** que aparece una vez que publica correctamente el proyecto.

![](./media/functions-develop-vs/functions-vstools-app-settings.png)

Con esto aparece el cuadro de diálogo **Configuración de la aplicación** para la aplicación de función, donde puede agregar una configuración de la aplicación nueva o modificar alguna existente.

![](./media/functions-develop-vs/functions-vstools-app-settings2.png)

**Local** representa un valor de configuración en el archivo local.settings.json y **Remoto** es el valor actual de la aplicación de función de Azure.  Elija **Agregar configuración** para crear una nueva configuración de aplicación. Use el vínculo **Insertar un valor desde Local** para copiar un valor de configuración para el campo **Remoto**. Los cambios pendientes se escriben en el archivo de configuración local y la aplicación de función cuando se selecciona **Aceptar**.

> [!NOTE]
> De forma predeterminada, el archivo local.settings.json no se ha insertado en el control de código fuente. Esto significa que al clonar un proyecto de funciones locales desde el control de código fuente, el proyecto no tiene el archivo local.settings.json. En este caso, debe crear manualmente el archivo local.settings.json en la raíz del proyecto para que el cuadro de diálogo **Configuración de la aplicación** funcione según lo previsto. 

También puede administrar la configuración de la aplicación en una de estas otras maneras:

* [Uso de Azure Portal](functions-how-to-use-azure-function-app-settings.md#settings).
* [Uso de la opción de publicación de `--publish-local-settings` en Herramientas principales de Azure Functions](functions-run-local.md#publish).
* [Uso de la CLI de Azure](/cli/azure/functionapp/config/appsettings#az-functionapp-config-appsettings-set).

## <a name="monitoring-functions"></a>Supervisión de funciones

La forma recomendada de supervisar la ejecución de las funciones es mediante la aplicación de función con Azure Application Insights. De forma predeterminada, cuando crea una aplicación de función en Azure Portal, esta integración se realiza automáticamente. Sin embargo, cuando se crea la aplicación de función durante la publicación de Visual Studio, no se lleva a cabo su integración en Azure.

Para habilitar Application Insights para la aplicación de función:

[!INCLUDE [functions-connect-new-app-insights.md](../../includes/functions-connect-new-app-insights.md)]

Para más información, consulte [Supervisión de Azure Functions](functions-monitoring.md).

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre Azure Functions Core Tools, consulte [Codificación y comprobación de las funciones de Azure en un entorno local](functions-run-local.md).

Para más información sobre el desarrollo de funciones como las bibliotecas de clases de .NET, consulte [Referencia para desarrolladores de C# de Azure Functions](functions-dotnet-class-library.md). Este artículo también tiene vínculos a ejemplos de cómo usar atributos para declarar los distintos tipos de vínculos compatibles con Azure Functions.    
