---
title: 'Tutorial: Deserializadores de .NET personalizados para trabajos en la nube de Azure Stream Analytics'
description: En este tutorial se muestra cómo crear un deserializador de .NET personalizado para un trabajo en la nube de Azure Stream Analytics mediante Visual Studio.
author: mamccrea
ms.author: mamccrea
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: tutorial
ms.date: 05/06/2019
ms.openlocfilehash: 1fffeec1434cb066487bf383589554edec2e6a86
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "75443689"
---
# <a name="tutorial-custom-net-deserializers-for-azure-stream-analytics"></a>Tutorial: Deserializadores de .NET personalizados para Azure Stream Analytics

Azure Stream Analytics tiene [compatibilidad integrada con tres formatos de datos](stream-analytics-parsing-json.md): JSON, CSV y Avro. Con los deserializadores de .NET personalizados, puede leer datos de otros formatos, como el [búfer de protocolo](https://developers.google.com/protocol-buffers/), [Bond](https://github.com/Microsoft/bond) y otros definidos por el usuario para trabajos en la nube y perimetrales.

En este tutorial se muestra cómo crear un deserializador de .NET personalizado para un trabajo en la nube de Azure Stream Analytics mediante Visual Studio. 

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Crear un deserializador personalizado para el búfer de protocolo.
> * Crear un trabajo de Azure Stream Analytics en Visual Studio.
> * Configurar el trabajo de Stream Analytics para usar el deserializador personalizado.
> * Ejecutar de forma local el trabajo de Stream Analytics para probar el deserializador personalizado.

## <a name="prerequisites"></a>Prerrequisitos

* Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

* Instale [Visual Studio 2017](https://www.visualstudio.com/downloads/) o [Visual Studio 2015](https://www.visualstudio.com/vs/older-downloads/). Se admiten las ediciones Enterprise (Ultimate y Premium), Professional y Community. No se admite la edición Express.

* [Instale las herramientas de Stream Analytics para Visual Studio](stream-analytics-tools-for-visual-studio-install.md) o actualice a la versión más reciente. Las siguientes versiones de Visual Studio son compatibles:
   * Visual Studio 2015
   * Visual Studio 2017

* Abra **Cloud Explorer** en Visual Studio e inicie sesión en su suscripción de Azure.

* Cree un contenedor en una cuenta de Azure Storage.
El contenedor que cree se usará para almacenar los recursos relacionados con el trabajo de Stream Analytics. Si ya tiene una cuenta de almacenamiento con los contenedores existentes, puede usarlos. De lo contrario, tendrá que [crear un nuevo contenedor](../storage/blobs/storage-quickstart-blobs-portal.md).

## <a name="create-a-custom-deserializer"></a>Creación de un deserializador personalizado

1. Abra Visual Studio y seleccione **Archivo > Nuevo > Proyecto**. Busque **Stream Analytics** y seleccione **Azure Stream Analytics Custom Deserializer Project (.NET)** [Proyecto de deserializador personalizado de Azure Stream Analytics (.NET)]. Asigne un nombre al proyecto, como **Deserializador protobuf**.

   ![Creación de un proyecto de biblioteca de clases estándar de dotnet para Visual Studio](./media/custom-deserializer/create-dotnet-library-project.png)

2. En el Explorador de soluciones, haga clic con el botón derecho en el proyecto **Deserializador protobuf** y seleccione **Administrar paquetes NuGet** en el menú. Después, instale los paquetes NuGet **Microsoft.Azure.StreamAnalytics** y **Google.Protobuf**.

3. Agregue la clase [MessageBodyProto](https://github.com/Azure/azure-stream-analytics/blob/master/CustomDeserializers/Protobuf/MessageBodyProto.cs) y la clase [MessageBodyDeserializer](https://github.com/Azure/azure-stream-analytics/blob/master/CustomDeserializers/Protobuf/MessageBodyDeserializer.cs) al proyecto.

4. Compile el proyecto **Deserializador protobuf**.

## <a name="add-an-azure-stream-analytics-project"></a>Incorporación de un proyecto de Azure Stream Analytics

1. En el Explorador de soluciones, haga clic con el botón derecho en la solución **Deserializador protobuf** y seleccione **Agregar > Nuevo proyecto**. En **Azure Stream Analytics > Stream Analytics**, elija **Aplicación Azure Stream Analytics**. Asígnele el nombre **ProtobufCloudDeserializer** y seleccione **Aceptar**. 

2. Haga clic con el botón derecho en **Referencias** en el proyecto de Azure Stream Analytics **ProtobufCloudDeserializer**. En **Proyectos**, agregue **Deserializador protobuf**. Se rellenará automáticamente.

## <a name="configure-a-stream-analytics-job"></a>Configuración de un trabajo de Stream Analytics

1. Haga doble clic en **JobConfig.json**. Use las configuraciones predeterminadas, excepto las siguientes:

   |Configuración|Valor sugerido|
   |-------|---------------|
   |Recurso de configuración de almacenamiento global|Selección del origen de datos desde la cuenta actual|
   |Suscripción de configuración de almacenamiento global| < su suscripción >|
   |Cuenta de almacenamiento de la configuración de almacenamiento global| < su cuenta de almacenamiento >|
   |Recurso de configuración de almacenamiento de código personalizado|Selección del origen de datos desde la cuenta actual|
   |Cuenta de almacenamiento de configuración de almacenamiento de código personalizado|< su cuenta de almacenamiento >|
   |Contenedor de configuración de almacenamiento de código personalizado|< su contenedor de almacenamiento >|

2. En **Entradas**, haga doble clic en **Input.json**. Use las configuraciones predeterminadas, excepto las siguientes:

   |Configuración|Valor sugerido|
   |-------|---------------|
   |Source|Blob Storage|
   |Resource|Selección del origen de datos desde la cuenta actual|
   |Subscription|< su suscripción >|
   |Cuenta de almacenamiento|< su cuenta de almacenamiento >|
   |Contenedor|< su contenedor de almacenamiento >|
   |Formato de serialización de eventos|Otro (protobuf, XML, propietario...)|
   |Resource|Carga desde Referencia de proyecto ASA o de CodeBehind|
   |Nombre del ensamblado CSharp|ProtobufDeserializer.dll|
   |Class Name (Nombre de clase)|MessageBodyProto.MessageBodyDeserializer|
   |Tipo de compresión de eventos|None|

3. Agregue la siguiente consulta al archivo **Script.asaql**.

   ```sql
   SELECT * FROM Input
   ```

4. Descargue el [archivo de entrada de ejemplo protobuf](https://github.com/Azure/azure-stream-analytics/blob/master/CustomDeserializers/Protobuf/SimulatedTemperatureEvents.protobuf). En la carpeta **Inputs**, haga clic con el botón derecho en **Inputs.json** y seleccione **Agregar entrada local**. A continuación, haga doble clic en **local_Input.json** y configure las siguientes opciones:

   |Configuración|Valor sugerido|
   |-------|---------------|
   |Alias de entrada|Entrada|
   |Tipo de origen|Flujo de datos|
   |Formato de serialización de eventos|Otro (protobuf, XML, propietario...)|
   |Nombre del ensamblado CSharp|ProtobufDeserializer.dll|
   |Class Name (Nombre de clase)|MessageBodyProto.MessageBodyDeserializer|
   |La ruta de acceso del archivo de entrada local|< la ruta de acceso del archivo de entrada de protobuf de ejemplo descargado >|

## <a name="execute-the-stream-analytics-job"></a>Ejecución del trabajo de Stream Analytics

1. Abra **script.asaql** y seleccione **Ejecutar localmente**.

2. Observe los resultados en los **resultados de la ejecución local de Stream Analytics**.

Ha implementado correctamente un deserializador personalizado para el trabajo de Stream Analytics. En este tutorial, ha probado el deserializador personalizado localmente. Para los datos reales, configuraría correctamente la entrada y la salida. Después, envíe el trabajo a Azure desde Visual Studio para ejecutar el trabajo en la nube con el deserializador personalizado recién implementado.

## <a name="debug-your-deserializer"></a>Depuración del deserializador

Puede depurar el deserializador de .NET localmente de la misma manera que el código de .NET estándar. 

1. Agregue puntos de interrupción a la función.

2. Pulse **F5** para iniciar la depuración. El programa se detendrá en los puntos de interrupción según lo previsto.

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando no los necesite, elimine el grupo de recursos, el trabajo de streaming y todos los recursos relacionados. La eliminación del trabajo evita la facturación de las unidades de streaming utilizadas por el trabajo. Si piensa utilizar el trabajo en el futuro, puede detenerlo y volver a iniciarlo más adelante cuando sea necesario. Si no va a seguir usando este trabajo, siga estos pasos para eliminar todos los recursos creados en este tutorial:

1. En el menú de la izquierda de Azure Portal, seleccione **Grupos de recursos** y luego el nombre del recurso que creó.  

2. En la página del grupo de recursos, seleccione **Eliminar**, escriba el nombre del recurso que quiere eliminar en el cuadro de texto y, luego, seleccione **Eliminar**.

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, aprendió a implementar un deserializador de .NET personalizado para la serialización de entrada del búfer de protocolo. Para más información sobre la creación de deserializadores personalizados, continúe con el siguiente artículo:

> [!div class="nextstepaction"]
> [Creación de deserializadores de .NET personalizados para trabajos de Azure Stream Analytics](custom-deserializer-examples.md)
