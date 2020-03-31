---
title: 'Exploración de datos en una máquina virtual de SQL Server: proceso de ciencia de datos en equipos'
description: Cómo explorar los datos que se almacenan en una máquina virtual de SQL Server en Azure con SQL o un lenguaje de programación como Python.
services: machine-learning
author: marktab
manager: marktab
editor: marktab
ms.service: machine-learning
ms.subservice: team-data-science-process
ms.topic: article
ms.date: 01/10/2020
ms.author: tdsp
ms.custom: seodec18, previous-author=deguhath, previous-ms.author=deguhath
ms.openlocfilehash: ae8c7c43ecbf9bc625e1e46be3e2c71c8d57b6f7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "76720102"
---
# <a name="explore-data-in-sql-server-virtual-machine-on-azure"></a>Exploración de los datos de una máquina virtual de SQL Server en Azure

Este artículo explica cómo explorar los datos que se almacenan en una máquina virtual de SQL Server en Azure. Utilice SQL o Python para examinar los datos.

Esta tarea constituye un paso del [proceso de ciencia de datos en equipos](overview.md).

> [!NOTE]
> En las instrucciones SQL de ejemplo de este documento se supone que los datos están en SQL Server. Si no lo están, consulte el mapa de proceso de ciencia de datos en la nube para obtener información sobre cómo mover los datos a SQL Server.
> 
> 

## <a name="explore-sql-data-with-sql-scripts"></a><a name="sql-dataexploration"></a>Exploración de los datos de SQL con scripts de SQL
A continuación se muestran algunos scripts de SQL de ejemplo que se pueden usar para explorar los almacenes de datos en SQL Server.

1. Obtener el número de observaciones por día
   
    `SELECT CONVERT(date, <date_columnname>) as date, count(*) as c from <tablename> group by CONVERT(date, <date_columnname>)` 
2. Obtención de los niveles de una columna de categorías
   
    `select  distinct <column_name> from <databasename>`
3. Obtener el número de niveles de combinación de dos columnas de categorías 
   
    `select <column_a>, <column_b>,count(*) from <tablename> group by <column_a>, <column_b>`
4. Obtener la distribución para columnas numéricas
   
    `select <column_name>, count(*) from <tablename> group by <column_name>`

> [!NOTE]
> Para obtener un ejemplo práctico, puede usar el conjunto de datos [NYC Taxi dataset](https://www.andresmh.com/nyctaxitrips/) y consultar el IPNB denominado [NYC Data wrangling using IPython Notebook and SQL Server](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/iPythonNotebooks/machine-Learning-data-science-process-sql-walkthrough.ipynb) (Tratamiento de datos de la ciudad de Nueva York mediante IPython Notebook y SQL Server), que ofrece un tutorial completo.
> 
> 

## <a name="explore-sql-data-with-python"></a><a name="python"></a>Exploración de los datos de SQL con Python
Usar Python para generar explorar datos y generar características cuando los datos están en SQL Server es parecido a procesar los datos en Blob de Azure mediante Python, como se documenta en [Proceso de datos de Blob de Azure en su entorno de ciencia de datos](data-blob.md). Cargue los datos desde la base de datos en una trama de datos de Pandas y, a continuación, se pueden procesar aún más. Se documenta el proceso de conexión a la base de datos y carga de los datos en la trama de datos de esta sección.

El formato de cadena de conexión siguiente puede usarse para conectarse a una base de datos de SQL Server desde Python mediante pyodbc (reemplace servername, dbname, username y password con sus valores específicos):

    #Set up the SQL Azure connection
    import pyodbc    
    conn = pyodbc.connect('DRIVER={SQL Server};SERVER=<servername>;DATABASE=<dbname>;UID=<username>;PWD=<password>')

La [biblioteca Pandas](https://pandas.pydata.org/) en Python ofrece un amplio conjunto de herramientas de análisis de datos y estructuras de datos para la manipulación de datos para la programación en Python. El código siguiente lee los resultados que se devuelven desde una base de datos de SQL Server en una trama de datos de Pandas:

    # Query database and load the returned results in pandas data frame
    data_frame = pd.read_sql('''select <columnname1>, <columnname2>... from <tablename>''', conn)

Ya puede trabajar con la trama de datos de Pandas como se explica en el artículo [Proceso de datos del blob de Azure con análisis avanzado](data-blob.md).

## <a name="the-team-data-science-process-in-action-example"></a>Proceso de ciencia de datos en equipos en acción: ejemplo
Para ver un tutorial de ejemplo completo del proceso de Cortana Analytics usando un conjunto de datos público, consulte [Proceso de ciencia de datos en equipos en acción: uso de SQL Server](sql-walkthrough.md).

