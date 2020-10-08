---
title: Creación de un panel en tiempo real con Azure Cosmos DB, Azure Analysis Services y Power BI
description: Aprenda a crear un panel meteorológico en directo en Power BI con Azure Cosmos DB y Azure Analysis Services.
author: SnehaGunda
ms.author: sngun
ms.service: cosmos-db
ms.topic: how-to
ms.date: 09/04/2019
ms.reviewer: sngun
ms.openlocfilehash: 681929928e6e6b28c7950c8aeeadc8b181491f46
ms.sourcegitcommit: 23aa0cf152b8f04a294c3fca56f7ae3ba562d272
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/07/2020
ms.locfileid: "91804136"
---
# <a name="create-a-real-time-dashboard-using-azure-cosmos-db-and-power-bi"></a>Creación de un panel en tiempo real mediante Azure Cosmos DB y Power BI

En este artículo se describen los pasos necesarios para crear un panel meteorológico en directo en Power BI con Azure Cosmos DB y Azure Analysis Services. En el panel de Power BI se mostrarán los gráficos para mostrar información en tiempo real sobre la temperatura y las precipitaciones de una región.

## <a name="reporting-scenarios"></a>Escenarios de informes

Hay varias maneras de configurar paneles de informes en los datos almacenados en Azure Cosmos DB. En función de los requisitos de obsolescencia y del tamaño de los datos, en la tabla siguiente se describe la configuración de informes para cada escenario:


|Escenario |Configurar |
|---------|---------|
|1. Generación de informes ad hoc (sin actualización)    |  [Conector de Azure Cosmos DB con Power BI con el modo de importación](powerbi-visualize.md)       |
|2. Generación de informes ad hoc con actualización periódica   |  [Conector de Azure Cosmos DB con Power BI con el modo de importación (actualización periódica programada)](powerbi-visualize.md)       |
|3. Generación de informes en grandes conjuntos de datos (< 10 GB)     |  Conector de Azure Cosmos DB con Power BI con actualización incremental       |
|4. Informes en tiempo real en grandes conjuntos de datos    |  Conector de Azure Analysis Services con Power BI con consulta directa y Azure Analysis Services (conector de Azure Cosmos DB)       |
|5. Informes sobre datos en directo con agregados     |  [Conector de Spark con Power BI con la consulta directa, Azure Databricks y conector Spark de Cosmos DB.](https://github.com/Azure/azure-cosmosdb-spark/wiki/Connecting-Cosmos-DB-with-PowerBI-using-spark-and-databricks-premium)       |
|6. Informes sobre datos en directo con agregados en grandes conjuntos de datos   |  Conector de Azure Analysis Services con Power BI con consulta directa, Azure Analysis Services, Azure Databricks y conector de Spark con Cosmos DB.       |

Los escenarios 1 y 2 se pueden configurar fácilmente mediante el conector de Power BI con Azure Cosmos DB. En este artículo se describen las configuraciones de los escenarios 3 y 4.

### <a name="power-bi-with-incremental-refresh"></a>Power BI con actualización incremental

Power BI tiene un modo en el que se puede configurar la actualización incremental. Este modo elimina la necesidad de crear y administrar particiones de Azure Analysis Services. La actualización incremental puede configurarse para filtrar únicamente las actualizaciones más recientes de grandes conjuntos de datos. Sin embargo, este modo solo funciona con el servicio Power BI Premium que tiene una limitación del conjunto de datos de 10 GB.

### <a name="power-bi-azure-analysis-connector--azure-analysis-services"></a>Conector de Azure Analysis de Power BI + Azure Analysis Services

Azure Analysis Services proporciona una plataforma completamente administrada como un servicio que hospeda modelos de datos en la nube de nivel empresarial. Los conjuntos de datos masivos se pueden cargar de Azure Cosmos DB en Azure Analysis Services. Para evitar consultar constantemente el conjunto de datos completo, los conjuntos de datos se pueden subdividir en particiones de Azure Analysis Services, que se pueden actualizar de forma independiente en distintas frecuencias.

## <a name="power-bi-incremental-refresh"></a>Actualización incremental de Power BI

### <a name="ingest-weather-data-into-azure-cosmos-db"></a>Ingesta de datos meteorológicos en Azure Cosmos DB

Configure una canalización de ingesta para cargar [datos meteorológicos](https://catalog.data.gov/dataset/local-weather-archive) en Azure Cosmos DB. Puede configurar un trabajo de [Azure Data Factory (ADF)](../data-factory/connector-azure-cosmos-db.md) para cargar periódicamente los datos meteorológicos más recientes en Azure Cosmos DB mediante el receptor de Cosmos DB y de origen HTTP.


### <a name="connect-power-bi-to-azure-cosmos-db"></a>Conexión de Power BI con Azure Cosmos DB

1. **Conectar la cuenta de Azure Cosmos con Power BI**: abra Power BI Desktop y use el conector de Azure Cosmos DB para seleccionar la base de datos y el contenedor correctos.

   :::image type="content" source="./media/create-real-time-weather-dashboard-powerbi/cosmosdb-powerbi-connector.png" alt-text="Conector de Power BI de Azure Cosmos DB":::

1. **Configurar actualización incremental**: siga los pasos descritos en el artículo [Actualización incremental con Power BI](/power-bi/service-premium-incremental-refresh) para configurar la actualización incremental para el conjunto de datos. Agregue los parámetros **RangeStart** y **RangeEnd**, tal y como se muestra en la siguiente captura de pantalla:

   :::image type="content" source="./media/create-real-time-weather-dashboard-powerbi/configure-range-parameters.png" alt-text="Conector de Power BI de Azure Cosmos DB" = Table.SelectRows(#"Expanded Document", each [Document.date] > DateTime.ToText(RangeStart,"yyyy-MM-dd") and [Document.date] < DateTime.ToText(RangeEnd,"yyyy-MM-dd"))
   ```
   
   En función de la columna y el tipo de datos que se encuentren en el conjunto de datos de origen, puede cambiar los campos RangeStart y RangeEnd en consecuencia.

   
   |Propiedad  |Tipo de datos  |Filter  |
   |---------|---------|---------|
   |_ts     |   Numeric      |  [_ts] > Duration.TotalSeconds(RangeStart - #datetime(1970, 1, 1, 0, 0, 0)) y [_ts] < Duration.TotalSeconds(RangeEnd - #datetime(1970, 1, 1, 0, 0, 0)))       |
   |Fecha (por ejemplo:- 2019-08-19)     |   String      | [Document.date]> DateTime.ToText(RangeStart,"yyyy-MM-dd") y [Document.date] < DateTime.ToText(RangeEnd,"yyyy-MM-dd")        |
   |Fecha (por ejemplo:- 2019-08-11 12:00:00)   |  String       |  [Document.date]> DateTime.ToText(RangeStart," yyyy-mm-dd HH:mm:ss") y [Document.date] < DateTime.ToText(RangeEnd,"yyyy-mm-dd HH:mm:ss")       |


1. **Definir la directiva de actualización**: defina la directiva de actualización; para ello, vaya a la pestaña **Actualización incremental** en el menú **contextual** de la tabla. Establezca la directiva de actualización para que se actualice **cada día** y almacene los datos del último mes.

   :::image type="content" source="./media/create-real-time-weather-dashboard-powerbi/define-refresh-policy.png" alt-text="Conector de Power BI de Azure Cosmos DB":::

   Omita la advertencia que indica *que la consulta M no se puede confirmar que se vaya a incorporar*. El conector de Azure Cosmos DB incorpora las consultas de filtro.

1. **Cargar los datos y generar los informes**: mediante el uso de los datos cargados anteriormente, cree los gráficos para informar de la temperatura y las precipitaciones.

   :::image type="content" source="./media/create-real-time-weather-dashboard-powerbi/load-data-generate-report.png" alt-text="Conector de Power BI de Azure Cosmos DB":::

1. **Publicar el informe en Power BI Premium**: como la actualización incremental es una característica solo Premium, el cuadro de diálogo Publicar únicamente permite seleccionar un área de trabajo en la funcionalidad Premium. La primera actualización puede tardar más tiempo en importar los datos históricos. Las actualizaciones de datos posteriores son mucho más rápidas porque utilizan la actualización incremental.


## <a name="power-bi-azure-analysis-connector--azure-analysis-services"></a>Conector de Azure Analysis de Power BI + Azure Analysis Services 

### <a name="ingest-weather-data-into-azure-cosmos-db"></a>Ingesta de datos meteorológicos en Azure Cosmos DB 

Configure una canalización de ingesta para cargar [datos meteorológicos](https://catalog.data.gov/dataset/local-weather-archive) en Azure Cosmos DB. Puede configurar un trabajo de Azure Data Factory (ADF) para cargar periódicamente los datos meteorológicos más recientes en Azure Cosmos DB mediante el receptor de Cosmos DB y de origen HTTP.

### <a name="connect-azure-analysis-services-to-azure-cosmos-account"></a>Conexión de Azure Analysis Services con la cuenta de Azure Cosmos

1. **Cree un nuevo clúster de Azure Analysis Services** - [Cree una instancia de Azure Analysis Services](../analysis-services/analysis-services-create-server.md) en la misma región que la cuenta de Azure Cosmos y el clúster de Databricks.

1. **Cree un nuevo proyecto tabular de Analysis Services en Visual Studio** -  [Instale SQL Server Data Tools (SSDT)](/sql/ssdt/download-sql-server-data-tools-ssdt?view=sql-server-2017&preserve-view=true) y cree un proyecto tabular de Analysis Services en Visual Studio.

   :::image type="content" source="./media/create-real-time-weather-dashboard-powerbi/create-analysis-services-project.png" alt-text="Conector de Power BI de Azure Cosmos DB":::

   Elija la instancia del **área de trabajo integrada** y establezca el nivel de compatibilidad en **SQL Server 2017/Azure Analysis Services (1400)** .

   :::image type="content" source="./media/create-real-time-weather-dashboard-powerbi/tabular-model-designer.png" alt-text="Conector de Power BI de Azure Cosmos DB":::

1. **Agregue el origen de datos de Azure Cosmos DB**: vaya a **Modelos**> **Orígenes de datos** > **Origen de datos nuevo** y agregue el origen de datos de Azure Cosmos DB, tal como se muestra en la siguiente captura de pantalla:

   :::image type="content" source="./media/create-real-time-weather-dashboard-powerbi/add-data-source.png" alt-text="Conector de Power BI de Azure Cosmos DB":::

   Conéctese a Azure Cosmos DB proporcionando el **URI de la cuenta**, el **nombre de la base de datos** y el **nombre del contenedor**. Ahora puede ver que los datos del contenedor de Azure Cosmos se importan en Power BI.

   :::image type="content" source="./media/create-real-time-weather-dashboard-powerbi/preview-cosmosdb-data.png" alt-text="Conector de Power BI de Azure Cosmos DB":::

1. **Construir el modelo de Analysis Services**: abra el editor de consultas y realice las operaciones necesarias para optimizar el conjunto de datos cargado:

   * Extraer solo las columnas relacionadas con el tiempo meteorológico (temperatura y precipitaciones)

   * Extraer la información mensual de la tabla. Estos datos son útiles en la creación de particiones, tal y como se describe en la sección siguiente.

   * Convertir las columnas de temperatura en número

   La expresión M resultante es la siguiente:

   ```
    let
        Source=#"DocumentDB/https://[ACCOUNTNAME].documents.azure.com:443/",
        #"Expanded Document" = Table.ExpandRecordColumn(Source, "Document", {"id", "_rid", "_self", "_etag", "fogground", "snowfall", "dust", "snowdepth", "mist", "drizzle", "hail", "fastest2minwindspeed", "thunder", "glaze", "snow", "ice", "fog", "temperaturemin", "fastest5secwindspeed", "freezingfog", "temperaturemax", "blowingsnow", "freezingrain", "rain", "highwind", "date", "precipitation", "fogheavy", "smokehaze", "avgwindspeed", "fastest2minwinddir", "fastest5secwinddir", "_attachments", "_ts"}, {"Document.id", "Document._rid", "Document._self", "Document._etag", "Document.fogground", "Document.snowfall", "Document.dust", "Document.snowdepth", "Document.mist", "Document.drizzle", "Document.hail", "Document.fastest2minwindspeed", "Document.thunder", "Document.glaze", "Document.snow", "Document.ice", "Document.fog", "Document.temperaturemin", "Document.fastest5secwindspeed", "Document.freezingfog", "Document.temperaturemax", "Document.blowingsnow", "Document.freezingrain", "Document.rain", "Document.highwind", "Document.date", "Document.precipitation", "Document.fogheavy", "Document.smokehaze", "Document.avgwindspeed", "Document.fastest2minwinddir", "Document.fastest5secwinddir", "Document._attachments", "Document._ts"}),
        #"Select Columns" = Table.SelectColumns(#"Expanded Document",{"Document.temperaturemin", "Document.temperaturemax", "Document.rain", "Document.date"}),
        #"Duplicated Column" = Table.DuplicateColumn(#"Select Columns", "Document.date", "Document.month"),
        #"Extracted First Characters" = Table.TransformColumns(#"Duplicated Column", {{"Document.month", each Text.Start(_, 7), type text}}),
        #"Sorted Rows" = Table.Sort(#"Extracted First Characters",{{"Document.date", Order.Ascending}}),
        #"Changed Type" = Table.TransformColumnTypes(#"Sorted Rows",{{"Document.temperaturemin", type number}, {"Document.temperaturemax", type number}}),
        #"Filtered Rows" = Table.SelectRows(#"Changed Type", each [Document.month] = "2019-07")
    in
        #"Filtered Rows"
   ```

   Además, cambie el tipo de datos de las columnas de temperatura a Decimal para asegurarse de que estos valores se pueden trazar en Power BI.

1. **Crear particiones de Azure Analysis**: cree particiones en Azure Analysis Services para dividir el conjunto de datos en particiones lógicas que se pueden actualizar de forma independiente y a diferentes frecuencias. En este ejemplo, se van a crear dos particiones que dividen el conjunto de datos en los datos del mes más reciente y en todo lo demás.

   :::image type="content" source="./media/create-real-time-weather-dashboard-powerbi/create-analysis-services-partitions.png" alt-text="Conector de Power BI de Azure Cosmos DB" = Table.SelectRows(#"Sorted Rows", each [Document.month] = "2019-07")` (Último mes)
   * **Historical** -  `#"Filtered Rows" = Table.SelectRows(#"Sorted Rows", each [Document.month] <> "2019-07")` (Historial)

1. **Implementar el modelo en Azure Analysis Server**: haga clic con el botón derecho en el proyecto de Azure Analysis Services y elija **Implementar**. Agregue el nombre del servidor en el panel de **propiedades de Servidor de implementación**.

   :::image type="content" source="./media/create-real-time-weather-dashboard-powerbi/analysis-services-deploy-model.png" alt-text="Conector de Power BI de Azure Cosmos DB":::

1. **Configurar actualizaciones y combinaciones de particiones**: Azure Analysis Services permite el procesamiento independiente de las particiones. Puesto que deseamos que la partición del **mes más reciente** se actualice constantemente con los datos más recientes, establezca el intervalo de actualización en cinco minutos. Puede actualizar los datos mediante la [API REST](../analysis-services/analysis-services-async-refresh.md), [automatización de Azure](../analysis-services/analysis-services-refresh-azure-automation.md) o con una [aplicación lógica](../analysis-services/analysis-services-refresh-logic-app.md). No es necesario actualizar los datos de la partición histórica. Además, debe escribir código para consolidar la partición del mes más reciente en la partición del historial y crear una nueva partición del mes más reciente.

## <a name="connect-power-bi-to-analysis-services"></a>Conexión de Power BI con Analysis Services

1. **Conectar a Azure Analysis Server mediante el conector de bases de datos de Azure Analysis Services**: elija el **modo activo** y conéctese a la instancia de Azure Analysis Services, tal como se muestra en la siguiente captura de pantalla:

   :::image type="content" source="./media/create-real-time-weather-dashboard-powerbi/analysis-services-get-data.png" alt-text="Conector de Power BI de Azure Cosmos DB":::

1. **Cargar los datos y generar informes**: mediante el uso de los datos cargados anteriormente, cree los gráficos para informar de la temperatura y las precipitaciones. Dado que está creando una conexión activa, las consultas se deben ejecutar en los datos del modelo de Azure Analysis Services que ha implementado en el paso anterior. Los gráficos de temperatura se actualizarán en un plazo de cinco minutos después de que los nuevos datos se carguen en Azure Cosmos DB.

   :::image type="content" source="./media/create-real-time-weather-dashboard-powerbi/load-data-generate-report.png" alt-text="Conector de Power BI de Azure Cosmos DB":::

## <a name="next-steps"></a>Pasos siguientes

* Para más información sobre Power BI, consulte [Introducción a Power BI](https://powerbi.microsoft.com/documentation/powerbi-service-get-started/).

* [Conexión de Qlik Sense a Azure Cosmos DB y visualización de los datos](visualize-qlik-sense.md)
