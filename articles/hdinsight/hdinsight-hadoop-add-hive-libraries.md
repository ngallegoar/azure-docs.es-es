---
title: 'Bibliotecas de Apache Hive durante la creación de clústeres: Azure HDInsight'
description: Aprenda a agregar bibliotecas de Apache Hive (archivos JAR) a un clúster de HDInsight durante la creación del clúster.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: H1Hack27Feb2017,hdinsightactive
ms.date: 02/14/2020
ms.openlocfilehash: 0b746963cea5a950ba47d8b4dfeb074cb0910436
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "77471030"
---
# <a name="add-custom-apache-hive-libraries-when-creating-your-hdinsight-cluster"></a>Incorporación de bibliotecas personalizadas de Apache Hive al crear el clúster de HDInsight

Obtenga información sobre cómo cargar previamente las bibliotecas de [Apache Hive](https://hive.apache.org/) en HDInsight. Este documento contiene información sobre el uso de una acción de script para cargar previamente bibliotecas durante la creación del clúster. Las bibliotecas que se agregan mediante los pasos de este documento están disponibles globalmente en Hive: no hace falta usar [ADD JAR](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+Cli) para cargarlos.

## <a name="how-it-works"></a>Funcionamiento

Al crear un clúster, puede usar una acción de script para modificar los nodos de clúster creados. El script de este documento acepta un único parámetro, que es la ubicación de las bibliotecas. Esta ubicación debe estar en una cuenta de Azure Storage, y las bibliotecas deben almacenarse como archivos jar.

Durante la creación del clúster, el script enumera los archivos, los copia en el directorio `/usr/lib/customhivelibs/` de los nodos principal y de trabajo y luego los agrega a la propiedad `hive.aux.jars.path` en el archivo `core-site.xml`. En los clústeres basados en Linux, también actualiza el archivo `hive-env.sh` con la ubicación de los archivos.

El uso de la acción de script en este artículo hace que las bibliotecas estén disponibles cuando se usa un cliente de Hive para **WebHCat** y **HiveServer2**.

## <a name="the-script"></a>El script

**Ubicación del script**

[https://hdiconfigactions.blob.core.windows.net/setupcustomhivelibsv01/setup-customhivelibs-v01.ps1](https://hdiconfigactions.blob.core.windows.net/setupcustomhivelibsv01/setup-customhivelibs-v01.ps1)

### <a name="requirements"></a>Requisitos

* Las scripts deben aplicarse tanto a los **nodos principales** como a los **nodos de trabajo**.

* Los archivos JAR que se van a instalar deben almacenarse en Azure Blob Storage en un **único contenedor**.

* La cuenta de almacenamiento que contiene la biblioteca de archivos JAR **debe** vincularse al clúster de HDInsight durante la creación. Debe ser la cuenta de almacenamiento predeterminada o una agregada a través de la __configuración de cuenta de almacenamiento__.

* La ruta de acceso de WASB al contenedor debe especificarse como un parámetro para la acción de script. Por ejemplo, si los archivos JAR se almacenan en un contenedor llamado **libs** en una cuenta de almacenamiento llamada **mystorage**, el parámetro sería `wasbs://libs@mystorage.blob.core.windows.net/`.

  > [!NOTE]  
  > En este documento se supone que ha creado ya una cuenta de almacenamiento, contenedora de blobs, y ha cargado los archivos en ella.
  >
  > Si no ha creado una cuenta de almacenamiento, puede hacerlo a través de [Azure Portal](https://portal.azure.com). Después, puede usar una utilidad como el [Explorador de Azure Storage](https://storageexplorer.com/) para crear un contenedor en la cuenta y cargar archivos en él.

## <a name="create-a-cluster-using-the-script"></a>Creación de un clúster mediante el script

1. Inicie el aprovisionamiento de un clúster siguiendo los pasos que se describen en [Aprovisionamiento de clústeres de HDInsight en Linux](hdinsight-hadoop-provision-linux-clusters.md), pero no complete la operación. También puede usar Azure PowerShell o el SDK de .NET para HDInsight para crear un clúster mediante este script. Para obtener más información sobre el uso de estos métodos, consulte [Personalización de clústeres de HDInsight mediante acciones de script](hdinsight-hadoop-customize-cluster-linux.md). Para Azure Portal, en la pestaña **Configuración y precios**, seleccione **+ Agregar acción de script**.

1. En **Almacenamiento**, si la cuenta de almacenamiento que contiene la biblioteca de archivos JAR será diferente de la cuenta usada para el clúster, complete **Cuentas de almacenamiento adicionales**.

1. En **Acciones de script**, proporcione la siguiente información:

    |Propiedad |Value |
    |---|---|
    |Tipo de script|- Personalizado|
    |Nombre|Bibliotecas |
    |URI de script de Bash|`https://hdiconfigactions.blob.core.windows.net/linuxsetupcustomhivelibsv01/setup-customhivelibs-v01.sh`|
    |Tipos de nodo|Principal, trabajo|
    |Parámetros|escriba la dirección WASB que dirige al contenedor y la cuenta de almacenamiento que contiene los archivos JAR. Por ejemplo, `wasbs://libs@mystorage.blob.core.windows.net/`.|

    > [!NOTE]
    > Para Apache Spark 2.1, use este URI de script de Bash: `https://hdiconfigactions.blob.core.windows.net/linuxsetupcustomhivelibsv01/setup-customhivelibs-v00.sh`.

1. Continúe aprovisionando el clúster tal como se describe en [Aprovisionamiento de clústeres de HDInsight en Linux](hdinsight-hadoop-provision-linux-clusters.md).

Una vez completada la creación del clúster, podrá utilizar los archivos JAR agregados a través de este script desde Hive sin tener que utilizar la instrucción `ADD JAR`.

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información acerca del uso con Hive, consulte [Uso de Apache Hive con HDInsight](hadoop/hdinsight-use-hive.md)
