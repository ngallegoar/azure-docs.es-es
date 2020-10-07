---
title: 'Inicio rápido: Apache Hive en Azure HDInsight con Apache Zeppelin'
description: Con este inicio rápido aprenderá a usar Apache Zeppelin para ejecutar consultas de Apache Hive.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: quickstart
ms.custom: hdinsightactive
ms.date: 12/03/2019
ms.openlocfilehash: 49b576fd511d17616880e5d981fd3f649de797df
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/05/2020
ms.locfileid: "79367931"
---
# <a name="quickstart-execute-apache-hive-queries-in-azure-hdinsight-with-apache-zeppelin"></a>Inicio rápido: Uso de Apache Zeppelin para ejecutar consultas de Apache Hive en Azure HDInsight

Con este inicio rápido aprenderá a usar Apache Zeppelin para ejecutar consultas de [Apache Hive](https://hive.apache.org/) en Azure HDInsight. Los clústeres de HDInsight Interactive Query incluyen cuadernos de [Apache Zeppelin](https://zeppelin.apache.org/) con los que puede ejecutar consultas de Hive interactivas.

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

## <a name="prerequisites"></a>Prerrequisitos

Un clúster de HDInsight Interactive Query. Consulte [Creación de un clúster](../hadoop/apache-hadoop-linux-tutorial-get-started.md) para crear un clúster de HDInsight.  Asegúrese de elegir el tipo de clúster **Interactive Query**.

## <a name="create-an-apache-zeppelin-note"></a>Creación de una nota de Apache Zeppelin

1. Reemplace `CLUSTERNAME` por el nombre del clúster en la siguiente dirección URL`https://CLUSTERNAME.azurehdinsight.net/zeppelin`. Después, escriba la dirección URL en un explorador web.

2. Escriba el nombre de usuario y la contraseña de inicio de sesión del clúster. En la página de Zeppelin, puede crear una nota o abrir notas existentes. **HiveSample** contiene algunos ejemplos de consultas de Hive.  

    ![Zeppelin de HDInsight Interactive Query](./media/hdinsight-connect-hive-zeppelin/hdinsight-hive-zeppelin.png)

3. Seleccione **Create new note** (Crear una nota).

4. En el cuadro de diálogo **Create new note** (Crear una nota), escriba o seleccione los valores siguientes:

    - Nombre de la nota: Escriba un nombre para la nota.
    - Intérprete predeterminado: Seleccione **jdbc** en la lista desplegable.

5. Seleccione **Create Note** (Crear nota).

6. Escriba la siguiente consulta de Hive en la sección de código y, después, presione **Mayús + Entrar**:

    ```hive
    %jdbc(hive)
    show tables
    ```

    ![Zeppelin de HDInsight Interactive Query ejecuta una consulta](./media/hdinsight-connect-hive-zeppelin/hdinsight-hive-zeppelin-query.png)

    La instrucción **%jdbc(hive)** en la primera línea indica al bloc de notas que debe usar el intérprete JDBC de Hive.

    La consulta devolverá una tabla de Hive denominada **hivesampletable**.

    Aquí se muestran dos consultas adicionales de Hive que se pueden ejecutar en **hivesampletable**.

    ```hive
    %jdbc(hive)
    select * from hivesampletable limit 10

    %jdbc(hive)
    select ${group_name}, count(*) as total_count
    from hivesampletable
    group by ${group_name=market,market|deviceplatform|devicemake}
    limit ${total_count=10}
    ```

    Comparado con Hive tradicional, los resultados de la consulta se devuelven mucho más rápido.

### <a name="additional-examples"></a>Ejemplos adicionales

1. Cree una tabla. Ejecute el código siguiente en el cuaderno de Zeppelin Notebook:

    ```hql
    %jdbc(hive)
    CREATE EXTERNAL TABLE log4jLogs (
        t1 string,
        t2 string,
        t3 string,
        t4 string,
        t5 string,
        t6 string,
        t7 string)
    ROW FORMAT DELIMITED
    FIELDS TERMINATED BY ' '
    STORED AS TEXTFILE;
    ```

1. Cargue los datos en la tabla nueva. Ejecute el código siguiente en el cuaderno de Zeppelin Notebook:

    ```hql
    %jdbc(hive)
    LOAD DATA
    INPATH 'wasbs:///example/data/sample.log'
    INTO TABLE log4jLogs;
    ```

1. Inserte un solo registro. Ejecute el código siguiente en el cuaderno de Zeppelin Notebook:

    ```hql
    %jdbc(hive)
    INSERT INTO TABLE log4jLogs2
    VALUES ('A', 'B', 'C', 'D', 'E', 'F', 'G');
    ```

Consulte el [manual del lenguaje Hive](https://cwiki.apache.org/confluence/display/Hive/LanguageManual) para obtener más ejemplos de sintaxis.

## <a name="clean-up-resources"></a>Limpieza de recursos

Después de completar el inicio rápido, puede ser conveniente eliminar el clúster. Con HDInsight, los datos se almacenan en Azure Storage, por lo que puede eliminar un clúster de forma segura cuando no se esté usando. Los clústeres de HDInsight se cobran aunque no se estén usando. Como en muchas ocasiones los cargos por el clúster son mucho más elevados que los cargos por el almacenamiento, desde el punto de vista económico tiene sentido eliminar clústeres cuando no se usen.

Para eliminar un clúster, consulte [Eliminación de un clúster de HDInsight con el explorador, PowerShell o la CLI de Azure](../hdinsight-delete-cluster.md).

## <a name="next-steps"></a>Pasos siguientes

Con este artículo aprenderá a usar Apache Zeppelin para ejecutar consultas de Apache Hive en Azure HDInsight. Para más información sobre las consultas de Hive, el siguiente artículo le mostrará cómo ejecutar consultas con Visual Studio.

> [!div class="nextstepaction"]
> [Conectarse a Azure HDInsight y ejecutar consultas de Apache Hive con Herramientas de Data Lake para Visual Studio](../hadoop/apache-hadoop-visual-studio-tools-get-started.md)
