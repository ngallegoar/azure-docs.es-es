---
title: Invocación del procedimiento almacenado desde la actividad de copia de Azure Data Factory
description: Obtenga información sobre cómo invocar un procedimiento almacenado en Azure SQL Database o SQL Server desde una actividad de copia de Azure Data Factory.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: shwang
editor: ''
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 01/10/2018
ms.author: jingwang
robots: noindex
ms.openlocfilehash: d2b10744222da8e5d85b19e1ded5aa24cf9c9706
ms.sourcegitcommit: fb3c846de147cc2e3515cd8219d8c84790e3a442
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/27/2020
ms.locfileid: "92637860"
---
# <a name="invoke-stored-procedure-from-copy-activity-in-azure-data-factory"></a>Invocación del procedimiento almacenado desde la actividad de copia en Azure Data Factory
> [!NOTE]
> Este artículo se aplica a la versión 1 de Data Factory. Si usa la versión actual del servicio Data Factory, consulte el artículo acerca de la [transformación de datos mediante la actividad de procedimientos almacenados en Data Factory](../transform-data-using-stored-procedure.md).


Cuando se copian datos en [SQL Server](data-factory-sqlserver-connector.md) o [Azure SQL Database](data-factory-azure-sql-connector.md), puede configurar **SqlSink** en la actividad de copia para invocar un procedimiento almacenado. Es posible que desee usar el procedimiento almacenado para realizar cualquier procesamiento adicional (combinar columnas, buscar valores, inserciones en varias tablas, etc.) necesario antes de insertar datos en la tabla de destino. Esta característica aprovecha los [parámetros con valores de tabla](/dotnet/framework/data/adonet/sql/table-valued-parameters). 

En el ejemplo siguiente se muestra cómo invocar un procedimiento almacenado en una base de datos SQL Server desde una canalización de Data Factory (actividad de copia):  

## <a name="output-dataset-json"></a>JSON de conjunto de datos de salida
En el JSON de conjunto de datos de salida, establezca el **tipo** en **SqlServerTable** . Establézcalo en **AzureSqlTable** para usarlo con Azure SQL Database. El valor de la propiedad **tableName** debe coincidir con el nombre del primer parámetro del procedimiento almacenado.  

```json
{
  "name": "SqlOutput",
  "properties": {
    "type": "SqlServerTable",
    "linkedServiceName": "SqlLinkedService",
    "typeProperties": {
      "tableName": "Marketing"
    },
    "availability": {
      "frequency": "Hour",
      "interval": 1
    }
  }
}
```

## <a name="sqlsink-section-in-copy-activity-json"></a>Sección SqlSink del JSON de actividad de copia
Defina la sección **SqlSink** en el JSON de actividad de copia como se indica a continuación. Para invocar un procedimiento almacenado mientras se insertan datos en la base de datos de receptor/destino, especifique los valores de las propiedades **SqlWriterStoredProcedureName** y **SqlWriterTableType** . Para las descripciones de estas propiedades, consulte la [sección SqlSink del artículo sobre el conector de SQL Server](data-factory-sqlserver-connector.md#sqlsink).

```json
"sink":
{
    "type": "SqlSink",
    "SqlWriterTableType": "MarketingType",
    "SqlWriterStoredProcedureName": "spOverwriteMarketing", 
    "storedProcedureParameters":
            {
                "stringData": 
                {
                    "value": "str1"     
                }
            }
}
```

## <a name="stored-procedure-definition"></a>Definición del procedimiento almacenado 
En la base de datos, defina el procedimiento almacenado con el mismo nombre que **SqlWriterStoredProcedureName** . El procedimiento almacenado controla los datos de entrada desde el almacén de datos de origen e inserta datos en una tabla en la base de datos de destino. El nombre del primer parámetro del procedimiento almacenado debe coincidir con el valor tableName definido en el JSON de base de datos (marketing).

```sql
CREATE PROCEDURE spOverwriteMarketing @Marketing [dbo].[MarketingType] READONLY, @stringData varchar(256)
AS
BEGIN
    DELETE FROM [dbo].[Marketing] where ProfileID = @stringData
    INSERT [dbo].[Marketing](ProfileID, State)
    SELECT * FROM @Marketing
END
```

## <a name="table-type-definition"></a>Definición de tipo de tabla
En la base de datos, defina el tipo de tabla con el mismo nombre como **SqlWriterTableType** . El esquema del tipo de tabla debe coincidir con el esquema del conjunto de datos de entrada.

```sql
CREATE TYPE [dbo].[MarketingType] AS TABLE(
    [ProfileID] [varchar](256) NOT NULL,
    [State] [varchar](256) NOT NULL
)
```

## <a name="next-steps"></a>Pasos siguientes
Revise los siguientes artículos de conectores para ver ejemplos completos de JSON: 

- [Azure SQL Database](data-factory-azure-sql-connector.md)
- [SQL Server](data-factory-sqlserver-connector.md)