---
title: Integración de Log Analytics y Excel
description: Obtenga una consulta de Log Analytics en Excel y actualice los resultados en Excel.
ms.subservice: logs
ms.topic: conceptual
author: roygalMS
ms.author: roygal
ms.date: 11/03/2020
ms.openlocfilehash: d903d1bb16ba3576d0092979f1cc6b82fac1c0be
ms.sourcegitcommit: 4bee52a3601b226cfc4e6eac71c1cb3b4b0eafe2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/11/2020
ms.locfileid: "94507413"
---
# <a name="integrate-log-analytics-and-excel"></a>Integración de Log Analytics y Excel

Puede integrar Log Analytics de Azure Monitor y Microsoft Excel mediante una consulta M y la API de Log Analytics.  Esta integración le permite enviar 500 000 registros a Excel.

> [!NOTE]
> Dado que Excel es una aplicación cliente local, las limitaciones de hardware y software locales afectan al rendimiento y la capacidad de procesar grandes conjuntos de datos.

## <a name="create-your-m-query-in-log-analytics"></a>Creación de la consulta M en Log Analytics 

1. **Cree y ejecute la consulta** en log Analytics como lo haría normalmente. No se preocupe si alcanza la limitación de 10 000 registros en la interfaz de usuario.  Se recomienda usar fechas relativas, como la función "ago" o el selector de tiempo de la interfaz de usuario, para que Excel actualice el conjunto de datos correcto.
  
2. **Exporte la consulta**: una vez que esté satisfecho con la consulta y sus resultados, exporte la consulta a M mediante la opción de menú **Exportar a Power BI (M Query)** de Log Analytics del menú *Exportar*:

:::image type="content" source="media/log-excel/export-query.png" alt-text="Consulta de Log Analytics con los datos y la opción de exportación" border="true":::



Al elegir esta opción, se descarga un archivo .txt que contiene el código M que puede usar en Excel.

La consulta mostrada anteriormente exporta el siguiente código M. Este es un ejemplo del código M exportado para la consulta de nuestro ejemplo:

```m
/*
The exported Power Query Formula Language (M Language ) can be used with Power Query in Excel
and Power BI Desktop.
For Power BI Desktop follow the instructions below: 
1) Download Power BI Desktop from https://powerbi.microsoft.com/desktop/
2) In Power BI Desktop select: 'Get Data' -> 'Blank Query'->'Advanced Query Editor'
3) Paste the M Language script into the Advanced Query Editor and select 'Done'
*/

let AnalyticsQuery =
let Source = Json.Document(Web.Contents("https://api.loganalytics.io/v1/workspaces/ddcfc599-cae0-48ee-9026-fffffffffffff/query", 
[Query=[#"query"="

Heartbeat 
| summarize dcount(ComputerIP) by bin(TimeGenerated, 1h)    
| render timechart
",#"x-ms-app"="OmsAnalyticsPBI",#"timespan"="P1D",#"prefer"="ai.response-thinning=true"],Timeout=#duration(0,0,4,0)])),
TypeMap = #table(
{ "AnalyticsTypes", "Type" }, 
{ 
{ "string",   Text.Type },
{ "int",      Int32.Type },
{ "long",     Int64.Type },
{ "real",     Double.Type },
{ "timespan", Duration.Type },
{ "datetime", DateTimeZone.Type },
{ "bool",     Logical.Type },
{ "guid",     Text.Type },
{ "dynamic",  Text.Type }
}),
DataTable = Source[tables]{0},
Columns = Table.FromRecords(DataTable[columns]),
ColumnsWithType = Table.Join(Columns, {"type"}, TypeMap , {"AnalyticsTypes"}),
Rows = Table.FromRows(DataTable[rows], Columns[name]), 
Table = Table.TransformColumnTypes(Rows, Table.ToList(ColumnsWithType, (c) => { c{0}, c{3}}))
in
Table
in AnalyticsQuery
```

## <a name="connect-query-to-excel"></a>Conexión de la consulta a Excel 

Para importar la consulta: 

1. Abra Microsoft Excel. 
1. En la cinta de opciones, vaya al menú **Datos**. Seleccione **Obtener datos**. En **Otros orígenes**, seleccione **Consulta en blanco**:
 
   :::image type="content" source="media/log-excel/excel-import-blank-query.png" alt-text="Importar desde cero en la opción de Excel" border="true":::

1. En la ventana de Power Query, seleccione **Editor avanzado**:

   :::image type="content" source="media/log-excel/advanced-editor.png" alt-text="Editor de consultas avanzadas de Excel" border="true":::

 
1. Reemplace el texto del editor avanzado por la consulta exportada desde Log Analytics:

   :::image type="content" source="media/log-excel/advanced-editor-2.png" alt-text="Crear una consulta en blanco" border="true":::
 
1. Seleccione **Listo** y, a continuación, seleccione **Cargar y cerrar**. Excel ejecuta la consulta mediante la API de Log Analytics y, a continuación, se muestra el conjunto de resultados.
 

   :::image type="content" source="media/log-excel/excel-query-result.png" alt-text="Resultados de la consulta en Excel" border="true":::

##  <a name="refreshing--data"></a>Actualización de los datos

Puede actualizar los datos directamente desde Excel. En el grupo de menús **Datos** de la cinta de opciones de Excel, seleccione el botón **Actualizar**.
 
## <a name="next-steps"></a>Pasos siguientes

Para más información acerca de las integraciones de Excel con orígenes de datos externos, consulte [Importar datos de orígenes de datos externos (Power Query)](https://support.office.com/article/import-data-from-external-data-sources-power-query-be4330b3-5356-486c-a168-b68e9e616f5a).