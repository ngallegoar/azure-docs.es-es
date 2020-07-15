---
title: Scripts SQL en Azure Synapse Studio (versión preliminar)
description: Introducción a los scripts SQL de Azure Synapse Studio (versión preliminar)
services: synapse-analytics
author: pimorano
ms.service: synapse-analytics
ms.topic: conceptual
ms.date: 04/15/2020
ms.author: pimorano
ms.reviewer: omafnan
ms.openlocfilehash: ee384d6095ccbf25225a435fe8afe4281c5d62df
ms.sourcegitcommit: dee7b84104741ddf74b660c3c0a291adf11ed349
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85921496"
---
# <a name="using-sql-script-in-azure-synapse-studio-preview"></a>Uso de scripts SQL en Azure Synapse Studio (versión preliminar)

Azure Synapse Studio (versión preliminar) proporciona una interfaz web de scripts SQL para crear consultas SQL. Puede conectarse al grupo de SQL (versión preliminar) o a SQL a petición (versión preliminar). 

## <a name="begin-authoring-in-sql-script"></a>Comenzar la creación en el script SQL 

Hay varias maneras de comenzar la experiencia de creación en el script SQL. Puede crear un script SQL a través de uno de los métodos siguientes.

1. En el menú Desarrollar, seleccione el icono **"+"** y elija **SQL script** (Script SQL).

    ![newsqlscript](media/author-sql-script/newsqlscript.png)

2. En el menú **Actions** (Acciones), elija **New SQL script** (Nuevo script SQL).
    
    ![newsqlscript2actions](media/author-sql-script/newsqlscript2actions.png)

También puede: 

3. Elija **Import** (Importar) en el menú **Actions** (Acciones) en Develop SQL scripts (Desarrollar scripts SQL) y seleccione un script SQL existente en el almacenamiento local.
 
    ![newsqlscript3actions](media/author-sql-script/newsqlscript3actions.png)

## <a name="create-your-sql-script"></a>Creación del script SQL

1. Elija un nombre para el script SQL; para ello, seleccione el botón **Property** (Propiedad) y reemplace el nombre predeterminado asignado al script SQL.
  
    ![newsqlscriptrename](media/author-sql-script/newsqlscriptrename.png)

2. Elija el grupo de SQL específico o la instancia de SQL a petición en el menú desplegable **Connect to** (Conectarse a). O bien, si es necesario, elija la base de datos en **Use database** (Usar base de datos).
 
    ![newsqlchoosepool](media/author-sql-script/newsqlchoosepool.png)

3. Comience a crear el script SQL con la característica IntelliSense.

    ![newsqlintellisense](media/author-sql-script/newsqlintellisense.png)

## <a name="run-your-sql-script"></a>Ejecución del script SQL

Seleccione el botón **Run** (Ejecutar) para ejecutar el script SQL. De forma predeterminada, los resultados se muestran en una tabla.

![newsqlscriptresultstable](media/author-sql-script/newsqlscriptresultstable.png)

## <a name="export-your-results"></a>Exportación de los resultados

Puede exportar los resultados al almacenamiento local en formatos diferentes (incluidos CSV, Excel, JSON, XML) seleccionando "Exportar resultados" y eligiendo la extensión.

También puede visualizar los resultados del script SQL en un gráfico si selecciona el botón **Chart** (Gráfico). Seleccione "Chart type" (Tipo de gráfico) y **Category column** (Columna de categoría). Para exportar el gráfico a una imagen, seleccione **Save as image** (Guardar como imagen). 

![newsqlscriptresultschart](media/author-sql-script/newsqlscriptresultschart.png)

## <a name="explore-data-from-a-parquet-file"></a>Exploración de los datos de un archivo de Parquet

Puede explorar los archivos de Parquet en una cuenta de almacenamiento mediante el script SQL para obtener una vista previa del contenido de los archivos.

![newscriptsqlodparquet](media/author-sql-script/newscriptsqlodparquet.png)

## <a name="sql-tables-external-tables-views"></a>Tablas SQL, tablas externas, vistas

Al seleccionar el menú **Actions** (Acciones) en Data (Datos), puede seleccionar varias acciones, entre ellas:

- Nuevo script SQL
- Seleccionar las 1000 primeras filas
- CREATE
- DROP y CREATE 
 
Explore el gesto disponible; para ello, haga clic con el botón derecho en los nodos del grupo de SQL y SQL a petición.
 
![newscriptdatabase](media/author-sql-script/newscriptdatabase.png)

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información sobre la creación de un script SQL, consulte [Azure Synapse Analytics](https://docs.microsoft.com/azure/synapse-analytics).
