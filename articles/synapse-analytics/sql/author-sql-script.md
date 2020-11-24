---
title: Scripts SQL en Azure Synapse Studio (versión preliminar)
description: Introducción a los scripts SQL de Azure Synapse Studio (versión preliminar)
services: synapse-analytics
author: pimorano
ms.service: synapse-analytics
ms.subservice: sql
ms.topic: conceptual
ms.date: 04/15/2020
ms.author: pimorano
ms.reviewer: omafnan
ms.openlocfilehash: 3f3009799889bd6b118f586676e22338d821d37c
ms.sourcegitcommit: 295db318df10f20ae4aa71b5b03f7fb6cba15fc3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/15/2020
ms.locfileid: "94635296"
---
# <a name="using-sql-scripts-in-azure-synapse-studio-preview"></a>Uso de scripts SQL en Azure Synapse Studio (versión preliminar)

Azure Synapse Studio (versión preliminar) proporciona una interfaz web de scripts SQL para crear consultas SQL. Puede conectarse al grupo de SQL (versión preliminar). 

## <a name="begin-authoring-in-sql-script"></a>Comenzar la creación en el script SQL 

Hay varias maneras de comenzar la experiencia de creación en el script SQL. Puede crear un script SQL a través de uno de los métodos siguientes.

1. En el menú Desarrollar, seleccione el icono **"+"** y elija **SQL script** (Script SQL).

2. En el menú **Actions** (Acciones), elija **New SQL script** (Nuevo script SQL).

3. Elija **Importar** en el menú **Acciones** en Desarrollar scripts SQL. Seleccione un script SQL en el almacenamiento local.
![Nuevas acciones de script de SQL 3](media/author-sql-script/new-sql-script-3-actions.png)

## <a name="create-your-sql-script"></a>Creación del script SQL

1. Elija un nombre para el script SQL; para ello, seleccione el botón **Property** (Propiedad) y reemplace el nombre predeterminado asignado al script SQL. 
![Nuevo cambio de nombre del script de SQL](media/author-sql-script/new-sql-script-rename.png)

2. Elija el grupo de SQL dedicado o el grupo de SQL sin servidor específicos en el menú desplegable **Conectarse a**. O bien, si es necesario, elija la base de datos en **Use database** (Usar base de datos). 
![Nuevo grupo elegido de SQL](media/author-sql-script/new-sql-choose-pool.png)

3. Comience a crear el script SQL con la característica IntelliSense.

## <a name="run-your-sql-script"></a>Ejecución del script SQL

Seleccione el botón **Run** (Ejecutar) para ejecutar el script SQL. De forma predeterminada, los resultados se muestran en una tabla.

![Nueva tabla de resultados de script de SQL](media/author-sql-script/new-sql-script-results-table.png)

## <a name="export-your-results"></a>Exportación de los resultados

Puede exportar los resultados al almacenamiento local en formatos diferentes (incluidos CSV, Excel, JSON, XML) seleccionando "Exportar resultados" y eligiendo la extensión.

También puede visualizar los resultados del script SQL en un gráfico si selecciona el botón **Chart** (Gráfico). Seleccione "Chart type" (Tipo de gráfico) y **Category column** (Columna de categoría). Para exportar el gráfico a una imagen, seleccione **Save as image** (Guardar como imagen). 

![Nuevo gráfico de resultados de script de SQL](media/author-sql-script/new-sql-script-results-chart.png)

## <a name="explore-data-from-a-parquet-file"></a>Exploración de los datos de un archivo de Parquet

Puede explorar los archivos de Parquet en una cuenta de almacenamiento mediante el script SQL para obtener una vista previa del contenido de los archivos.

![Nuevo script sqlod de Parquet](media/author-sql-script/new-script-sqlod-parquet.png)

## <a name="sql-tables-external-tables-views"></a>Tablas SQL, tablas externas, vistas

Al seleccionar el menú **Actions** (Acciones) en Data (Datos), puede seleccionar varias acciones, entre ellas:

- Nuevo script SQL
- Seleccionar las 1000 primeras filas
- CREATE
- DROP y CREATE 
 
Explore el gesto disponible; para ello, haga clic con el botón derecho en los nodos de las bases de datos SQL.
 
![Nuevo script de base de datos](media/author-sql-script/new-script-database.png)

## <a name="create-folders-and-move-sql-scripts-into-a-folder"></a>Creación de carpetas y traslado de scripts SQL a una carpeta

En el menú Acciones de Desarrollar Scripts SQL, elija "New folder" (Nueva carpeta) en el menú "Acciones" en Desarrollar Scripts SQL. Y escriba el nombre de la nueva carpeta en la ventana emergente. 

> [!div class="mx-imgBorder"] 
> ![newsqlscript](./media/author-sql-script/new-sql-script-create-folder.png)

Para mover un script SQL a una carpeta, puede seleccionar el script SQL y elegir "Mover a" en el menú Acciones. Luego, busque la carpeta de destino en la nueva ventana y mueva el script SQL a la carpeta seleccionada. También puede arrastrar rápidamente el script SQL y colocarlo en una carpeta.  

> [!div class="mx-imgBorder"] 
> ![newsqlscript](./media/author-sql-script/new-sql-script-move-folder.png)

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información sobre la creación de un script SQL, consulte [Azure Synapse Analytics](https://docs.microsoft.com/azure/synapse-analytics).
