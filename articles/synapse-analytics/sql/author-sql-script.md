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
ms.openlocfilehash: 940c6d6d96c5c1aa062397d21ea96dace2c09bae
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/16/2020
ms.locfileid: "81427400"
---
# <a name="using-sql-script-in-azure-synapse-studio-preview"></a>Uso de scripts SQL en Azure Synapse Studio (versión preliminar)

Azure Synapse Studio (versión preliminar) proporciona una interfaz web de scripts SQL para crear consultas SQL. Puede conectarse al grupo de SQL (versión preliminar) o a SQL a petición (versión preliminar). 

## <a name="begin-authoring-in-sql-script"></a>Comenzar la creación en el script SQL 

Hay varias maneras de comenzar la experiencia de creación en el script SQL. Puede crear un script SQL a través de uno de los métodos siguientes.

1. Seleccione el icono "+" y elija script SQL.

    > [!div class="mx-imgBorder"] 
    >![newsqlscript](./media/author-sql-script/newsqlscript.png)

2. En el menú Acciones de Desarrollar Scripts SQL, elija "New SQL script" (Nuevo script SQL) en el menú "Acciones" en Desarrollar Scripts SQL. 

    > [!div class="mx-imgBorder"] 
    > ![newsqlscript](./media/author-sql-script/newsqlscript2actions.png)

or 

3. Elija "Importar" en el menú "Acciones" en Desarrollar Scripts SQL y seleccione un script SQL existente en el almacenamiento local.

    > [!div class="mx-imgBorder"] 
    > ![newsqlscript](./media/author-sql-script/newsqlscript3actions.png)

## <a name="create-your-sql-script"></a>Creación del script SQL

1. Elija un nombre para el script SQL; para ello, seleccione el botón "Propiedad" y reemplace el nombre predeterminado asignado al script SQL.

    > [!div class="mx-imgBorder"] 
    > ![newsqlscript](./media/author-sql-script/newsqlscriptrename.png)

1. Elija el grupo de SQL específico o la instancia de SQL a petición en el menú desplegable "Conectarse a". O bien, si es necesario, elija la base de datos en "Use database" (Usar base de datos).

    > [!div class="mx-imgBorder"] 
    > ![newsqlscript](./media/author-sql-script/newsqlchoosepool.png)

1. Comience a crear el script SQL con la característica IntelliSense.

    > [!div class="mx-imgBorder"] 
    > ![newsqlscript](./media/author-sql-script/newsqlintellisense.png)

## <a name="run-your-sql-script"></a>Ejecución del script SQL

Seleccione el botón "Ejecutar" para ejecutar el script SQL. De forma predeterminada, los resultados se muestran en una tabla.

> [!div class="mx-imgBorder"] 
> ![newsqlscript](./media/author-sql-script/newsqlscriptresultstable.png)

## <a name="export-your-results"></a>Exportación de los resultados

Puede exportar los resultados al almacenamiento local en formatos diferentes (incluidos CSV, Excel, JSON, XML) seleccionando "Exportar resultados" y eligiendo la extensión.

También puede visualizar los resultados del script SQL en un gráfico si selecciona el botón "Gráfico". Seleccione "Tipo de gráfico" y "Columna de categoría". Para exportar el gráfico a una imagen, seleccione "Guardar como imagen". 

> [!div class="mx-imgBorder"] 
> ![newsqlscript](./media/author-sql-script/newsqlscriptresultschart.png)

## <a name="explore-data-from-a-parquet-file"></a>Explore los datos de un archivo de Parquet.

Puede explorar los archivos de Parquet en una cuenta de almacenamiento mediante el script SQL para obtener una vista previa del contenido de los archivos. 

> [!div class="mx-imgBorder"] 
> ![newsqlscript](./media/author-sql-script/newscriptsqlodparquet.png)

## <a name="sql-tables-external-tables-views"></a>Tablas SQL, tablas externas, vistas

Al seleccionar el menú "Acciones" en datos, puede seleccionar varias acciones, entre ellas: "New SQL script" (Nuevo script SQL), "Seleccionar las 1000 primeras filas", "CREAR", "COLOCAR y CREAR". Explore el gesto disponible; para ello, haga clic con el botón derecho en los nodos del grupo de SQL y SQL a petición.

> [!div class="mx-imgBorder"] 
> ![newsqlscript](./media/author-sql-script/newscriptdatabase.png)

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información sobre la creación de un script SQL, consulte [Azure Synapse Analytics](https://docs.microsoft.com/azure/synapse-analytics).