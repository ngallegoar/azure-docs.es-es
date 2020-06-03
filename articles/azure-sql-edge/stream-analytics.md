---
title: Uso de trabajos de Edge de Azure Stream Analytics con Azure SQL Edge (versión preliminar)
description: Más información sobre el uso de trabajos de Stream Analytics en Azure SQL Edge (versión preliminar)
keywords: SQL Edge, stream analytics,
services: sql-edge
ms.service: sql-edge
ms.topic: conceptual
author: SQLSourabh
ms.author: sourabha
ms.reviewer: sstein
ms.date: 05/19/2020
ms.openlocfilehash: 7c15312b48e7118517894d8ffd4807e4892e03a3
ms.sourcegitcommit: f1132db5c8ad5a0f2193d751e341e1cd31989854
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2020
ms.locfileid: "84233136"
---
# <a name="using-azure-stream-analytics-jobs-with-sql-edge"></a>Uso de trabajos de Azure Stream Analytics con SQL Edge

Azure SQL Edge (versión preliminar) es un motor de base de datos relacional optimizado y orientado a implementaciones de IoT y de tipo perimetral. Se basa en las versiones más recientes del motor de base de datos de Microsoft SQL Server, que proporciona funcionalidades de rendimiento, seguridad y procesamiento de consultas líderes en el sector. Junto con las capacidades de administración de bases de datos relacionales líderes del sector de SQL Server, Azure SQL Edge proporciona funcionalidad de streaming integrada para el análisis en tiempo real y el procesamiento de eventos complejos.

Azure SQL Edge tiene una implementación nativa del runtime de Stream Analytics. Esta implementación le permite crear un trabajo de Edge de Azure Stream Analytics e implementar ese trabajo como un trabajo de streaming de SQL Edge. Los trabajos de Azure Stream Analytics se pueden implementar en SQL Edge mediante el parámetro ASAJobInfo expuesto a través de la opción `module twin's desired properties` del módulo de SQL Edge:

```json
{
    "properties.desired":
    {
        "SqlPackage": "<Optional_DACPAC_ZIP_SAS_URL>",
        "ASAJobInfo": "<Optional_ASA_Job_ZIP_SAS_URL>"
    }
}
```

|Campo | Descripción |
|------|-------------|
| SqlPackage | El URI de Azure Blob Storage del archivo *.zip que contiene el paquete DAC de SQL Database.
| ASAJobInfo | El URI de Azure Blob Storage para el trabajo de Edge de ASA.

## <a name="create-an-azure-stream-analytics-edge-job"></a>Creación de un trabajo de Edge de Azure Stream Analytics

1. Vaya a Azure Portal.

2. Cree un trabajo de **Azure Stream Analytics en IoT Edge**. Elija el entorno de hospedaje que tiene como destino **Edge**.

3. Defina una entrada y salida para el trabajo de Azure Stream Analytics. Cada salida de SQL, que configurará aquí, está asociada a una sola tabla de la base de datos. Si necesita transmitir datos a varias tablas, deberá crear varias salidas de SQL Database. Puede configurar las salidas SQL para que apunten a diferentes bases de datos.

    **Entrada**. Elija EdgeHub como entrada para el trabajo de Edge y proporcione la información de recursos.

    **Salida**. Seleccione SQL Database como salida. Seleccione **Indicar manualmente la configuración de Base de datos SQL**. Proporcione los detalles de configuración de la base de datos y la tabla.

    |Campo      | Descripción |
    |---------------|-------------|
    |Alias de salida | Nombre del alias de salida.|
    |Base de datos | El nombre de la base de datos SQL. Debe ser un nombre de base de datos válido que exista en la instancia de SQL Edge.|
    |Nombre de servidor | Detalles de nombre (o de dirección IP) y número de puerto de la instancia de SQL. En el caso de una implementación de SQL Edge, puede usar **tcp:.,1433** como nombre del servidor.|
    |Nombre de usuario | Cuenta de inicio de sesión de SQL que tiene acceso de lectura y escritura de datos a la base de datos que especificó anteriormente.|
    |Contraseña | Contraseña de la cuenta de inicio de sesión de SQL que especificó anteriormente.|
    |Tabla | Nombre de la tabla que se generará para el trabajo de streaming.|
    |Heredar creación de particiones| Habilita la herencia del esquema de partición de la entrada o el paso anterior de la consulta. Con esta opción habilitada, al escribir en una tabla basada en disco y tener una topología totalmente paralela para su trabajo, puede esperar un rendimiento superior.|
    |Tamaño de lote| El número máximo de registros que se envía con cada transacción de inserción masiva.|

    Esta es una configuración de muestra de entrada y salida:

    ```txt
        Input:
            Input from EdgeHub
            Input alias: input
            Event serialization format: JSON
            Encoding: UTF-8
            Event compression type: None

        Output:
            Output alias: output
            Database:  MeasurementsDB
            Server name: tcp:.,1433
            Username: sa
            Password: <Password>
            Table: TemperatureMeasurements
            Inherit Partitioning: Merge all input partitions into a single writer
            Max batch count: 10000
    ```

    > [!NOTE]
    > Para más información sobre el adaptador de salida de SQL para Azure Stream Analytics, vea [Salida de Azure Stream Analytics a Azure SQL Database](../stream-analytics/stream-analytics-sql-output-perf.md).

4. Defina la consulta de trabajo ASA para el trabajo perimetral. Esta consulta debe utilizar los alias de entrada/salida definidos como nombres de entrada y salida en la consulta. Para más información, consulte [Referencia del lenguaje de consulta de Stream Analytics](https://docs.microsoft.com/stream-analytics-query/stream-analytics-query-language-reference).

5. Establezca la configuración de la cuenta de almacenamiento para el trabajo en Edge. La cuenta de almacenamiento se usa como el destino de publicación para el trabajo perimetral.

6. En **Configurar**, seleccione **Publicar** y seleccione el botón **Publicar**. Guarde el URI de SAS para usarlo con el módulo de SQL Edge.

## <a name="deploy-azure-stream-analytics-edge-job-to-sql-edge"></a>Implementación del trabajo de Edge de Azure Stream Analytics en SQL Edge

Para implementar el trabajo de streaming en el módulo de SQL Edge, actualice la configuración del módulo de SQL Edge para incluir el URI de SAS del trabajo de streaming del paso anterior. Para actualizar el módulo de SQL Edge:

1. En Azure Portal, vaya a la implementación de IoT Hub.

2. En el panel izquierdo, seleccione **IoT Edge**.

3. En la página de **IoT Edge**, busque y seleccione la instancia de IoT Edge en la que se implementa el módulo de SQL Edge.

4. En la página de **Dispositivo de IoT Edge**, seleccione **Establecer módulo**.

5. En la página **Establecer módulos**, seleccione **Configurar** en el módulo de SQL Edge.

6. En el panel **Módulos personalizados de IoT Edge**, seleccione **Establecer propiedades deseadas de gemelo del módulo**. Actualice las propiedades deseadas para incluir el URI para la opción `ASAJobInfo`, como se muestra en el ejemplo siguiente.

    > [!NOTE]
    > El URI de SAS del siguiente archivo JSON es solo un ejemplo. Reemplace el URI por el URI real de la implementación.

    ```json
        {
            "properties.desired":
            {
                "ASAJobInfo": "<<<SAS URL For the published ASA Edge Job>>>>"
            }
        }
    ```

7. Seleccione **Guardar**.

8. En la página **Establecer módulos**, seleccione **Siguiente**.

9. En la página **Establecer módulos**, seleccione **Siguiente** y, después, **Enviar**.

10. Después de la actualización del módulo, el archivo del trabajo de Stream Analytics se descarga, se descomprime y se implementa en la instancia de SQL Edge.

## <a name="next-steps"></a>Pasos siguientes

- [Implementación de SQL Edge mediante Azure Portal](deploy-portal.md).

- [Datos de streaming](stream-data.md)

- [Aprendizaje automático e IA con ONNX en SQL Edge (versión preliminar)](onnx-overview.md)
