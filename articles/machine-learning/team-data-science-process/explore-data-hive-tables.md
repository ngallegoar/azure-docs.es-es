---
title: 'Exploración de los datos de las tablas de Hive con consultas de Hive: proceso de ciencia de datos en equipos'
description: Uso de los scripts de Hive de ejemplo que se usan para explorar los datos de las tablas de Hive en un clúster de Hadoop para HDInsight.
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
ms.openlocfilehash: c0dfa06e2ece2ba4631c0d5681b066ab0134daba
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "86085674"
---
# <a name="explore-data-in-hive-tables-with-hive-queries"></a>Exploración de los datos de las tablas de Hive con consultas de Hive

Este artículo proporciona scripts de Hive de ejemplo que se usan para explorar los datos de las tablas de Hive en un clúster de Hadoop para HDInsight.

Esta tarea constituye un paso del [proceso de ciencia de datos en equipos](overview.md).

## <a name="prerequisites"></a>Prerequisites
En este artículo se supone que ha:

* Creado una cuenta de almacenamiento de Azure. Si necesita instrucciones, consulte [Creación de una cuenta de Azure Storage](../../storage/common/storage-account-create.md)
* Aprovisionado un clúster de Hadoop personalizado con el servicio HDInsight. Si necesita instrucciones, consulte [Personalización de clústeres de Hadoop de HDInsight de Azure para análisis avanzado](customize-hadoop-cluster.md).
* Se han cargado los datos en tablas de subárbol en clústeres de Hadoop de HDInsight de Azure. Si no es así, siga las instrucciones proporcionadas en [Crear y cargar datos en tablas de Hive](move-hive-tables.md) para cargar los datos en tablas de Hive primero.
* Habilitado el acceso remoto al clúster. Si necesita instrucciones, consulte [Acceso al nodo principal del clúster Hadoop](customize-hadoop-cluster.md).
* Si necesita instrucciones sobre cómo enviar consultas de Hive, consulte [Envío de consultas de Hive](move-hive-tables.md#submit)

## <a name="example-hive-query-scripts-for-data-exploration"></a>Ejemplo de scripts de consulta de Hive para la exploración de datos
1. Obtención del número de observaciones por partición `SELECT <partitionfieldname>, count(*) from <databasename>.<tablename> group by <partitionfieldname>;`
2. Obtención del número de observaciones por día `SELECT to_date(<date_columnname>), count(*) from <databasename>.<tablename> group by to_date(<date_columnname>);`
3. Obtención de los niveles de una columna de categorías  
    `SELECT  distinct <column_name> from <databasename>.<tablename>`
4. Obtención del número de niveles de combinación de dos columnas de categorías `SELECT <column_a>, <column_b>, count(*) from <databasename>.<tablename> group by <column_a>, <column_b>`
5. Obtener la distribución para columnas numéricas  
    `SELECT <column_name>, count(*) from <databasename>.<tablename> group by <column_name>`
6. Extraer registros de la combinación de dos tablas
   
    ```hiveql
    SELECT
        a.<common_columnname1> as <new_name1>,
        a.<common_columnname2> as <new_name2>,
        a.<a_column_name1> as <new_name3>,
        a.<a_column_name2> as <new_name4>,
        b.<b_column_name1> as <new_name5>,
        b.<b_column_name2> as <new_name6>
    FROM
        (
        SELECT <common_columnname1>,
            <common_columnname2>,
            <a_column_name1>,
            <a_column_name2>,
        FROM <databasename>.<tablename1>
        ) a
        join
        (
        SELECT <common_columnname1>,
            <common_columnname2>,
            <b_column_name1>,
            <b_column_name2>,
        FROM <databasename>.<tablename2>
        ) b
        ON a.<common_columnname1>=b.<common_columnname1> and a.<common_columnname2>=b.<common_columnname2>
    ```

## <a name="additional-query-scripts-for-taxi-trip-data-scenarios"></a>Scripts de consulta adicionales para escenarios de datos de trayectos en taxi
También se ofrecen ejemplos de consultas que son específicos de escenarios de [NYC Taxi Trip Data](https://chriswhong.com/open-data/foil_nyc_taxi/) en el [repositorio de GitHub](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/DataScienceScripts). Estas consultas ya tienen el esquema de datos especificado y están listas para enviarse para su ejecución.

