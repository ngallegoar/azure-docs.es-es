---
title: Ejecución de consultas de Apache Base en Azure HDInsight con Apache Phoenix
description: Aprenda a usar Apache Zeppelin para ejecutar consultas de Apache Base con Phoenix.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: how-to
ms.date: 10/14/2019
ms.openlocfilehash: e85226e7a2544cda01f40a1c1be6be32f11d9dfc
ms.sourcegitcommit: d767156543e16e816fc8a0c3777f033d649ffd3c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/26/2020
ms.locfileid: "92542801"
---
# <a name="use-apache-zeppelin-to-run-apache-phoenix-queries-over-apache-hbase-in-azure-hdinsight"></a>Use Apache Zeppelin para ejecutar consultas de Apache Phoenix en Azure HDInsight

Apache Phoenix es un nivel de base de datos relacional de código abierto y masivamente paralela que se basa en HBase. Phoenix permite usar consultas de tipo SQL sobre HBase. Phoenix usa controladores JDBC subyacentes para permitir a los usuarios crear, eliminar y modificar índices, vistas, secuencias y tablas SQL.  También puede usar Phoenix para actualizar filas individualmente y de forma masiva. Phoenix usa la compilación nativa de NOSQL en lugar de MapReduce para compilar consultas, lo que permite crear aplicaciones de baja latencia a partir de HBase.

Apache Zeppelin es un cuaderno de código abierto basado en Web que le permite crear documentos de colaboración basados en datos mediante análisis de datos interactivos y lenguajes como SQL y Scala. Ayuda a los desarrolladores de datos y a los científicos de datos a desarrollar, organizar, ejecutar y compartir código para la manipulación de datos. Permite visualizar los resultados sin necesidad de hacer referencia a la línea de comandos o de los detalles del clúster.

Los usuarios de HDInsight pueden usar Apache Zeppelin para consultar tablas de Phoenix. Apache Zeppelin se integra con el clúster de HDInsight y no hay pasos adicionales para usarlo. Simplemente cree un cuaderno de Zeppelin Notebook con el intérprete JDBC y empiece a escribir las consultas SQL de Phoenix

## <a name="prerequisites"></a>Prerrequisitos

Un clúster de Apache HBase en HDInsight. Vea [Introducción a un ejemplo de Apache HBase en HDInsight](./apache-hbase-tutorial-get-started-linux.md).

## <a name="create-an-apache-zeppelin-note"></a>Creación de una nota de Apache Zeppelin

1. Reemplace `CLUSTERNAME` por el nombre del clúster en la siguiente dirección URL`https://CLUSTERNAME.azurehdinsight.net/zeppelin`. Después, escriba la dirección URL en un explorador web. Escriba el nombre de usuario y la contraseña de inicio de sesión del clúster.

1. En la página de Zeppelin, seleccione **Create new note** (Crear nueva nota).

    ![Zeppelin de HDInsight Interactive Query](./media/apache-hbase-phoenix-zeppelin/hbase-zeppelin-create-note.png)

1. En el cuadro de diálogo **Create new note** (Crear una nota), escriba o seleccione los valores siguientes:

    - Nombre de la nota: Escriba un nombre para la nota.
    - Intérprete predeterminado: Seleccione **jdbc** en la lista desplegable.

    A continuación, seleccione **Create Note** (Crear nota).

1. Por otro lado, asegúrese de que en el encabezado del cuaderno aparece el estado conectado. Esto se indica mediante un punto verde que se encuentra en la esquina superior derecha.

    ![Estado del cuaderno de Zeppelin](./media/apache-hbase-phoenix-zeppelin/hbase-zeppelin-connected.png "Estado del cuaderno de Zeppelin")

1. Cree una tabla de HBase. Escriba el siguiente comando y, a continuación, presione **Máyus + Entrar** :

    ```sql
    %jdbc(phoenix)
    CREATE TABLE Company (
        company_id INTEGER PRIMARY KEY,
        name VARCHAR(225)
    );
    ```

    La instrucción **%jdbc(phoenix)** en la primera línea indica al cuaderno que debe usar el intérprete JDBC de Phoenix.

1. Vea las tablas creadas.

    ```sql
    %jdbc(phoenix)
    SELECT DISTINCT table_name
    FROM SYSTEM.CATALOG
    WHERE table_schem is null or table_schem <> 'SYSTEM';
    ```

1. Inserte valores en la tabla.

    ```sql
    %jdbc(phoenix)
    UPSERT INTO dbo.Company VALUES(1, 'Microsoft');
    UPSERT INTO dbo.Company (name, company_id) VALUES('Apache', 2);
    ```

1. Consulte la tabla.

    ```sql
    %jdbc(phoenix)
    SELECT * FROM dbo.Company;
    ```

1. Elimine un registro.

    ```sql
    %jdbc(phoenix)
    DELETE FROM dbo.Company WHERE COMPANY_ID=1;
    ```

1. Quite la tabla.

    ```sql
    %jdbc(phoenix)
    DROP TABLE dbo.Company;
    ```

## <a name="next-steps"></a>Pasos siguientes

- [Apache Phoenix ahora admite Zeppelin en Azure HDInsight](/archive/blogs/ashish/apache-phoenix-now-supports-zeppelin-in-azure-hdinsight)
- [Gramática de Apache Phoenix](https://phoenix.apache.org/language/index.html)