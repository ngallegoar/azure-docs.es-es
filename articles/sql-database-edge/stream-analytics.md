---
title: Uso de paquetes DAC de SQL Database y trabajos de Stream Analytics con Azure SQL Database Edge | Microsoft Docs
description: Aprenda sobre el uso de trabajos de Stream Analytics en SQL Database Edge
keywords: sql database edge, stream analytics, sqlpackage
services: sql-database-edge
ms.service: sql-database-edge
ms.topic: conceptual
author: SQLSourabh
ms.author: sourabha
ms.reviewer: sstein
ms.date: 11/04/2019
ms.openlocfilehash: 21a8bb6953fd879b17816361f536596571678697
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "74384159"
---
# <a name="using-sql-database-dac-packages-and-stream-analytics-jobs-with-sql-database-edge"></a>Uso de paquetes DAC de SQL Database y trabajos de Stream Analytics con SQL Database Edge | Microsoft Docs

La versión preliminar de Azure SQL Database Edge es un motor de base de datos relacional optimizado orientado a implementaciones de IoT y bordes. Se basa en las versiones más recientes del motor de base de datos de Microsoft SQL Server, que proporciona funcionalidades de rendimiento, seguridad y procesamiento de consultas líderes en el sector. Junto con las capacidades de administración de bases de datos relacionales líderes del sector de SQL Server, Azure SQL Database Edge proporciona funcionalidad de streaming integrada para el análisis en tiempo real y el procesamiento de eventos complejos.

Azure SQL Database Edge también proporciona una implementación nativa de SqlPackage.exe que permite a los usuarios implementar un paquete [DAC de SQL Database](https://docs.microsoft.com/sql/relational-databases/data-tier-applications/data-tier-applications) durante la implementación de SQL Database Edge.

Azure SQL Database Edge expone dos parámetros opcionales mediante la opción `module twin's desired properties` del módulo IoT Edge:

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
| ASAJobInfo | El URI de Azure Blob Storage para el trabajo de Edge de ASA. Para más información, consulte [Publicación de un trabajo de Edge de ASA para SQL Database Edge](/azure/sql-database-edge/stream-analytics#using-streaming-jobs-with-sql-database-edge).

## <a name="using-sql-database-dac-packages-with-sql-database-edge"></a>Uso de paquetes DAC de SQL Database con SQL Database Edge

Para usar un paquete DAC de SQL Database (*.dacpac) con SQL Database Edge, siga estos pasos:

1. Cree o extraiga un paquete DAC de SQL Database. Consulte [Extracción de un paquete DAC de una base de datos](/sql/relational-databases/data-tier-applications/extract-a-dac-from-a-database/) para más información sobre cómo generar un paquete DAC para una base de datos de SQL Server existente.

2. Comprima el archivo *.dacpac y cárguelo en una cuenta de Azure Blob Storage. Para más información sobre la carga de archivos en Azure Blob Storage, consulte [Carga, descarga y enumeración de blobs con Azure Portal](../storage/blobs/storage-quickstart-blobs-portal.md).

3. Genere una firma de acceso compartido para el archivo ZIP mediante Azure Portal. Para más información, consulte [Delegación de acceso con firmas de acceso compartidas (SAS)](../storage/common/storage-sas-overview.md).

4. Actualice la configuración del módulo SQL Database Edge para incluir el URI de acceso compartido para el paquete DAC. Para actualizar el módulo SQL Database Edge, realice estos pasos:

    1. En Azure Portal, vaya a la implementación de IoT Hub.

    2. En el panel izquierdo, seleccione **IoT Edge**.

    3. En la página de **IoT Edge**, busque y seleccione la instancia de IoT Edge en la que se implementa el módulo de SQL Database Edge.

    4. En la página de **Dispositivo de IoT Edge**, seleccione **Establecer módulo**.

    5. En la página **Establecer módulos**, seleccione **Configurar** en el módulo de SQL Database Edge.

    6. En el panel **Módulos personalizados de IoT Edge**, seleccione **Establecer propiedades deseadas de gemelo del módulo**. Actualice las propiedades deseadas para incluir el URI para la opción `SQLPackage`, como se muestra en el ejemplo siguiente.

        > [!NOTE]
        > El URI de SAS del siguiente archivo JSON es solo un ejemplo. Reemplace el URI por el URI real de la implementación.

        ```json
            {
                "properties.desired":
                {
                    "SqlPackage": "https://StorageAccountName.blob.core.windows.net/SQLpackageFiles/MeasurementsDB.zip?sp=r&st=2019-09-01T00:00:00Z&se=2019-09-30T00:00:00Z&spr=https&sv=2019-02-02&sr=b&sig=Uh8X0E50qzlxkiKVBJKU3ccVQYwF235qTz7AXp4R3gI%3D",
                }
            }
        ```

    7. Seleccione **Guardar**.

    8. En la página **Establecer módulos**, seleccione **Siguiente**.

    9. En la página **Establecer módulos**, seleccione **Siguiente** y, después, **Enviar**.

5. Después de la actualización del módulo, el archivo de paquete DAC se descarga, se descomprime y se implementa en la instancia de SQL Database Edge.

## <a name="using-streaming-jobs-with-sql-database-edge"></a>Uso de trabajos de streaming con SQL Database Edge

Azure SQL Database Edge tiene una implementación nativa del tiempo de ejecución del análisis de transmisiones. Esta implementación le permite crear un trabajo de Azure Stream Analytics Edge e implementar ese trabajo como un trabajo de streaming de SQL Database Edge. Para crear un trabajo de Stream Analytics Edge, realice estos pasos:

1. Vaya a Azure Portal mediante la versión preliminar de la [URL](https://portal.azure.com/?microsoft_azure_streamanalytics_edgeadapterspreview=true). Esta versión preliminar de la URL le permite a los usuarios configurar la salida de SQL Database para un trabajo en Edge de Stream Analytics.

2. Cree un trabajo de **Azure Stream Analytics en IoT Edge**. Elija el entorno de hospedaje que tiene como destino **Edge**.

3. Defina una entrada y salida para el trabajo de Azure Stream Analytics. Cada salida de SQL, que configurará aquí, está asociada a una sola tabla de la base de datos. Si necesita transmitir datos a varias tablas, deberá crear varias salidas de SQL Database. Puede configurar las salidas SQL para que apunten a diferentes bases de datos.

    **Entrada**. Elija EdgeHub como entrada para el trabajo de Edge y proporcione la información de recursos.

    **Salida**. Seleccione SQL Database como salida. Seleccione **Indicar manualmente la configuración de Base de datos SQL**. Proporcione los detalles de configuración de la base de datos y la tabla.

    |Campo      | Descripción |
    |---------------|-------------|
    |Alias de salida | Nombre del alias de salida.|
    |Base de datos | El nombre de la base de datos SQL. Debe ser un nombre de base de datos válido que exista en la instancia de SQL Database Edge.|
    |Nombre de servidor | Detalles de nombre (o de dirección IP) y número de puerto de la instancia de SQL. En el caso de una implementación de SQL Database Edge, puede usar **tcp:.,1433** como nombre del servidor.|
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

6. En **Configurar**, seleccione **Publicar** y seleccione el botón **Publicar**. Guarde el URI de SAS para usarlo con el módulo SQL Database Edge.

### <a name="deploy-the-stream-analytics-edge-job-to-sql-database-edge"></a>Implementación del trabajo de Stream Analytics en dispositivos perimetrales en SQL Database Edge

Para implementar el trabajo de streaming en el módulo SQL Database Edge, actualice la configuración del módulo de SQL Database Edge para incluir el URI de SAS del trabajo de streaming del paso anterior. Para actualizar el módulo SQL Database Edge:

1. En Azure Portal, vaya a la implementación de IoT Hub.

2. En el panel izquierdo, seleccione **IoT Edge**.

3. En la página de **IoT Edge**, busque y seleccione la instancia de IoT Edge en la que se implementa el módulo de SQL Database Edge.

4. En la página de **Dispositivo de IoT Edge**, seleccione **Establecer módulo**.

5. En la página **Establecer módulos**, seleccione **Configurar** en el módulo de SQL Database Edge.

6. En el panel **Módulos personalizados de IoT Edge**, seleccione **Establecer propiedades deseadas de gemelo del módulo**. Actualice las propiedades deseadas para incluir el URI para la opción `ASAJobInfo`, como se muestra en el ejemplo siguiente.

    > [!NOTE]
    > El URI de SAS del siguiente archivo JSON es solo un ejemplo. Reemplace el URI por el URI real de la implementación.

    ```json
        {
            "properties.desired":
            {
                "ASAJobInfo": "https://storageAccountName.blob.core.windows.net/asajobs/ASAEdgeJobs/5a9b34db-7a5c-4606-adae-4c8609eaa1c7/d85b5aa6-4d38-4703-bb34-af7f0bd7916d/ASAEdgeJobDefinition.zip?sv=2018-03-28&sr=b&sig=HH%2BFMsEy378RaTxIy2Xo6rM6wDaqoBaZ5zFDBqdZiS0%3D&st=2019-09-01T22%3A24%3A34Z&se=2019-09-30T22%3A34%3A34Z&sp=r"   
            }
        }
    ```

7. Seleccione **Guardar**.

8. En la página **Establecer módulos**, seleccione **Siguiente**.

9. En la página **Establecer módulos**, seleccione **Siguiente** y, después, **Enviar**.

10. Después de la actualización del módulo, el archivo del trabajo de Stream Analytics se descarga, se descomprime y se implementa en la instancia de SQL Database Edge.

## <a name="next-steps"></a>Pasos siguientes

- Para más información sobre los precios y los detalles sobre la disponibilidad, consulte [Azure SQL Database Edge](https://azure.microsoft.com/services/sql-database-edge/).
- Solicite que habiliten Azure SQL Database Edge para su suscripción.
- Para empezar, consulte [Implementación de SQL Database Edge mediante Azure Portal](deploy-portal.md).
