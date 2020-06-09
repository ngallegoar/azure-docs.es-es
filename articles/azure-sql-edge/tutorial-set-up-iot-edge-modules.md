---
title: Configuración de módulos de IoT Edge en Azure SQL Edge
description: En la segunda parte de este tutorial de tres partes de Azure SQL Edge para predecir impurezas de mineral de hierro, configurará módulos y conexiones de IoT Edge.
keywords: ''
services: sql-edge
ms.service: sql-edge
ms.topic: tutorial
author: VasiyaKrishnan
ms.author: vakrishn
ms.reviewer: sstein
ms.date: 05/19/2020
ms.openlocfilehash: a4087ef56712e098443009bd0457029394ea7b51
ms.sourcegitcommit: f1132db5c8ad5a0f2193d751e341e1cd31989854
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2020
ms.locfileid: "84235037"
---
# <a name="set-up-iot-edge-modules-and-connections"></a>Configuración de módulos y conexiones de IoT Edge

En la segunda parte de este tutorial de tres partes para predecir impurezas de mineral de hierro en Azure SQL Edge, configurará los siguientes módulos de IoT Edge:

- Azure SQL Edge
- Módulo IoT Edge del generador de datos

## <a name="create-azure-stream-analytics-module"></a>Creación de un módulo de Azure Stream Analytics

Cree un módulo de Azure Stream Analytics que se usará en este tutorial. Para más información sobre el uso de trabajos de streaming con SQL Edge, consulte [Uso de trabajos de streaming con SQL Edge](stream-analytics.md).

Una vez creado el trabajo de Azure Stream Analytics con el entorno de hospedaje establecido como Edge, configure las entradas y salidas para el tutorial.

1. Para crear la **entrada**, haga clic en **+Agregar entrada de flujo**. Rellene la sección de detalles con la información siguiente:

   Campo|Value
   -----|-----
   Formato de serialización de eventos|JSON
   Encoding|UTF-8
   Tipo de compresión de eventos|None

2. Para crear la **salida**, haga clic en **+Agregar** y elija SQL Database. Rellene la sección de detalles con la información siguiente.

   > [!NOTE]
   > La contraseña especificada en esta sección debe especificarse para la contraseña de administrador del sistema de SQL al implementar el módulo de SQL Edge en la sección **"Implementación del módulo de Azure SQL Edge"** .

   Campo|Value
   -----|-----
   Base de datos|IronOreSilicaPrediction
   Nombre de servidor|tcp:.,1433
   Nombre de usuario|sa
   Contraseña|Especifique una contraseña segura.
   Tabla|IronOreMeasurements1

3. Vaya a la sección **Consulta** y configure la consulta de la siguiente manera:

   `SELECT * INTO <name_of_your_output_stream> FROM <name_of_your_input_stream>`
   
4. En **Configurar**, seleccione **Publicar** y seleccione el botón **Publicar**. Guarde el URI de SAS para usarlo con el módulo SQL Database Edge.

## <a name="specify-container-registry-credentials"></a>Especificación de las credenciales de registro de contenedor

Debe especificar las credenciales en los registros de contenedor que hospedan las imágenes de módulo. Se pueden encontrar en el registro de contenedor que se creó en el grupo de recursos. Vaya a la sección **Claves de acceso**. Anote los campos siguientes:

- Nombre de registro
- Servidor de inicio de sesión
- Nombre de usuario
- Contraseña

Ahora, especifique las credenciales del contenedor en el módulo IoT Edge.

1. Vaya al centro de IoT que creó en el grupo de recursos.

2. En la sección **IoT Edge**, en **Administración automática de dispositivos**, haga clic en **Id. de dispositivo**. Para este tutorial, el identificador es `IronOrePredictionDevice`.

3. Seleccione la sección **Establecer módulos**.

4. En **Credenciales de Container Registry**, escriba los valores siguientes:

   _Campo_|_Valor_
   -------|-------
   Nombre|Nombre de registro
   Dirección|Servidor de inicio de sesión
   Nombre de usuario|Nombre de usuario
   Contraseña|Contraseña
  
## <a name="deploy-the-data-generator-module"></a>Implementación del módulo de generador de datos

1. En la sección **Módulos IoT Edge**, haga clic en **+ AGREGAR** y seleccione **Módulo IoT Edge**.

2. Proporcione un nombre de módulo IoT Edge y el URI de la imagen.
   El URI de la imagen se puede encontrar en el registro de contenedor del grupo de recursos. Seleccione la sección **Repositorios** en **Servicios**. En este tutorial, elija el repositorio denominado `silicaprediction`. Seleccione la etiqueta apropiada. El URI de la imagen tendrá el formato siguiente:

   *servidor de inicio de sesión del registro de contenedor*/*nombre del repositorio*:*nombre de la etiqueta*

   Por ejemplo:

   ```
   ASEdemocontregistry.azurecr.io/silicaprediction:amd64
   ```

3. Haga clic en **Agregar**.

## <a name="deploy-the-azure-sql-edge-module"></a>Implementación del módulo de Azure SQL Edge

1. Siga los pasos indicados en [Implementación de Azure SQL Edge (versión preliminar)](https://docs.microsoft.com/azure/azure-sql-edge/deploy-portal) para implementar el módulo de Azure SQL Edge.

2. En la opción **Especificar ruta** de la página **Establecer módulos**, especifique las rutas del módulo para la comunicación del Centro de IoT Edge como se indica a continuación. 

   ```
   FROM /messages/modules/<your_data_generator_module>/outputs/<your_output_stream_name> INTO
   BrokeredEndpoint("/modules/<your_azure_sql_edge_module>/inputs/<your_input_stream_name>")
   ```

   Por ejemplo:

   ```
   FROM /messages/modules/ASEDataGenerator/outputs/IronOreMeasures INTO BrokeredEndpoint("/modules/AzureSQLEdge/inputs/Input1")
   ```

3. En la configuración **módulo gemelo**, asegúrese de que SQLPackage y ASAJonInfo se actualizan con las direcciones URL de SAS relevantes que guardó anteriormente en el tutorial.

   ```json
       {
         "properties.desired":
         {
           "SqlPackage": "<Optional_DACPAC_ZIP_SAS_URL>",
           "ASAJobInfo": "<Optional_ASA_Job_ZIP_SAS_URL>"
         }
       }
   ```

## <a name="next-steps"></a>Pasos siguientes

- [Implementación del modelo de ML en Azure SQL Edge mediante ONNX](tutorial-run-ml-model-on-sql-edge.md)
