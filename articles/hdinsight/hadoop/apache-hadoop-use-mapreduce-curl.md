---
title: 'Uso de MapReduce y Curl con Apache Hadoop en HDInsight: Azure'
description: Obtenga información acerca de cómo ejecutar de manera remota trabajos de MapReduce con Apache Hadoop en HDInsight con Curl.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: how-to
ms.custom: hdinsightactive
ms.date: 01/13/2020
ms.openlocfilehash: a1d411662fd7afe57c714b97ab67b9d490acd40d
ms.sourcegitcommit: 124f7f699b6a43314e63af0101cd788db995d1cb
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/08/2020
ms.locfileid: "86076375"
---
# <a name="run-mapreduce-jobs-with-apache-hadoop-on-hdinsight-using-rest"></a>Ejecución de trabajos de MapReduce con Apache Hadoop en HDInsight con REST

Aprenda a usar la API REST de Apache Hive WebHCat para ejecutar trabajos de MapReduce en un clúster de Apache Hadoop en HDInsight. Curl se usa para demostrar cómo puede interactuar con HDInsight mediante solicitudes HTTP sin formato para ejecutar trabajos de MapReduce.

> [!NOTE]  
> Si ya está familiarizado con el uso de servidores de Hadoop basado en Linux, pero no conoce HDInsight, consulte el documento [Lo que necesita saber acerca de Apache Hadoop en HDInsight basado en Linux](../hdinsight-hadoop-linux-information.md).

## <a name="prerequisites"></a>Prerrequisitos

* Un clúster de Apache Hadoop en HDInsight. Consulte [Creación de clústeres de Apache Hadoop mediante Azure Portal](../hdinsight-hadoop-create-linux-clusters-portal.md).

Opciones:
  * Windows PowerShell o
  * [Curl](https://curl.haxx.se/) con [jq](https://stedolan.github.io/jq/)

## <a name="run-a-mapreduce-job"></a>Ejecución de un trabajo de MapReduce

> [!NOTE]  
> Al usar Curl o cualquier otra comunicación REST con WebHCat, debe autenticar las solicitudes proporcionando el nombre de usuario y la contraseña de administrador del clúster de HDInsight. Debe utilizar el nombre de clúster como parte del identificador URI utilizado para enviar las solicitudes al servidor.
>
> La API de REST se protege con la [autenticación de acceso básica](https://en.wikipedia.org/wiki/Basic_access_authentication). Siempre debe crear solicitudes usando HTTPS para así garantizar que las credenciales se envían de manera segura al servidor.

### <a name="curl"></a>Curl

1. Para facilitar su uso, establezca las variables siguientes. Este ejemplo se basa en un entorno de Windows, revise según sea necesario para su entorno.

    ```cmd
    set CLUSTERNAME=
    set PASSWORD=
    ```

1. Desde una línea de comandos, utilice el siguiente comando para comprobar que puede conectarse al clúster de HDInsight.

    ```bash
    curl -u admin:%PASSWORD% -G https://%CLUSTERNAME%.azurehdinsight.net/templeton/v1/status
    ```

    Los parámetros que se utilizan en este comando son los siguientes:

   * **-u**: el nombre de usuario y la contraseña que se utilizan para autenticar la solicitud.
   * **-G**: indica que esta operación es una solicitud GET.

   El principio del URI, `https://CLUSTERNAME.azurehdinsight.net/templeton/v1`, es el mismo para todas las solicitudes.

    La respuesta que recibe es similar al código JSON siguiente:

    ```output
    {"version":"v1","status":"ok"}
    ```

1. Para enviar un trabajo de MapReduce, use el siguiente comando: Modifique la ruta de acceso a **jq** según sea necesario.

    ```cmd
    curl -u admin:%PASSWORD% -d user.name=admin ^
    -d jar=/example/jars/hadoop-mapreduce-examples.jar ^
    -d class=wordcount -d arg=/example/data/gutenberg/davinci.txt -d arg=/example/data/output ^
    https://%CLUSTERNAME%.azurehdinsight.net/templeton/v1/mapreduce/jar | ^
    C:\HDI\jq-win64.exe .id
    ```

    El final del identificador URI (/mapreduce/jar) indica a WebHCat que esta solicitud inicia un trabajo de MapReduce desde una clase en un archivo jar. Los parámetros que se utilizan en este comando son los siguientes:

   * **-d**: `-G` no se usa, así que la solicitud establece como valor predeterminado el método POST. `-d` especifica los valores de datos que se envían con la solicitud.
     * **user.name**: el usuario que ejecuta el comando.
     * **jar**: la ubicación del archivo del producto que contiene la clase que se va a ejecutar.
     * **class**: la clase que contiene la lógica de MapReduce.
     * **arg**: los argumentos que se pasarán al trabajo de MapReduce. En este caso, el archivo de texto de entrada y el directorio que se utilizan para el resultado.

    Este comando debe devolver un identificador de trabajo que se pueda usar para comprobar el estado del trabajo:

       job_1415651640909_0026

1. Para revisar el estado del trabajo, use el siguiente comando. Reemplace el valor de `JOBID` por el valor **real** devuelto en el paso anterior. Revise la ubicación de **jq** según sea necesario.

    ```cmd
    set JOBID=job_1415651640909_0026

    curl -G -u admin:%PASSWORD% -d user.name=admin https://%CLUSTERNAME%.azurehdinsight.net/templeton/v1/jobs/%JOBID% | ^
    C:\HDI\jq-win64.exe .status.state
    ```

### <a name="powershell"></a>PowerShell

1. Para facilitar su uso, establezca las variables siguientes. Reemplace `CLUSTERNAME` por el nombre real del clúster. Ejecute el comando y escriba la contraseña de inicio de sesión del clúster cuando se le solicite.

    ```powershell
    $clusterName="CLUSTERNAME"
    $creds = Get-Credential -UserName admin -Message "Enter the cluster login password"
    ```

1. Use el siguiente comando para comprobar que puede conectarse al clúster de HDInsight:

    ```powershell
    $resp = Invoke-WebRequest -Uri "https://$clustername.azurehdinsight.net/templeton/v1/status" `
        -Credential $creds `
        -UseBasicParsing
    $resp.Content
    ```

    La respuesta que recibe es similar al código JSON siguiente:

    ```output
    {"version":"v1","status":"ok"}
    ```

1. Para enviar un trabajo de MapReduce, utilice lo siguiente:

    ```powershell
    $reqParams = @{}
    $reqParams."user.name" = "admin"
    $reqParams.jar = "/example/jars/hadoop-mapreduce-examples.jar"
    $reqParams.class = "wordcount"
    $reqParams.arg = @()
    $reqParams.arg += "/example/data/gutenberg/davinci.txt"
    $reqparams.arg += "/example/data/output"
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/templeton/v1/mapreduce/jar" `
       -Credential $creds `
       -Body $reqParams `
       -Method POST `
       -UseBasicParsing
    $jobID = (ConvertFrom-Json $resp.Content).id
    $jobID
    ```

    El final del identificador URI (/mapreduce/jar) indica a WebHCat que esta solicitud inicia un trabajo de MapReduce desde una clase en un archivo jar. Los parámetros que se utilizan en este comando son los siguientes:

    * **user.name**: el usuario que ejecuta el comando.
    * **jar**: la ubicación del archivo del producto que contiene la clase que se va a ejecutar.
    * **class**: la clase que contiene la lógica de MapReduce.
    * **arg**: los argumentos que se pasarán al trabajo de MapReduce. En este caso, el archivo de texto de entrada y el directorio que se utilizan para el resultado.

   Este comando debe devolver un identificador de trabajo que se pueda usar para comprobar el estado del trabajo:

       job_1415651640909_0026

1. Para revisar el estado del trabajo, use el siguiente comando:

    ```powershell
    $reqParams=@{"user.name"="admin"}
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/templeton/v1/jobs/$jobID" `
       -Credential $creds `
       -Body $reqParams `
       -UseBasicParsing

    # ConvertFrom-JSON can't handle duplicate names with different case
    # So change one to prevent the error
    $fixDup=$resp.Content.Replace("jobID","job_ID")
    (ConvertFrom-Json $fixDup).status.state
    ```

### <a name="both-methods"></a>Ambos métodos

1. Si se completa el trabajo, el estado devuelto es `SUCCEEDED`.

1. Cuando el estado del trabajo haya cambiado a `SUCCEEDED`, puede recuperar los resultados del trabajo desde Azure Blob Storage. El parámetro `statusdir` transmitido con la consulta contiene la ubicación del archivo de salida. En este ejemplo, la ubicación es `/example/curl`. Esta dirección almacena el resultado del trabajo en los clústeres del almacenamiento predeterminado en `/example/curl`.

Puede enumerar y descargar estos archivos mediante la [CLI de Azure](/cli/azure/install-azure-cli). Para obtener más información sobre el uso de la CLI de Azure para trabajar con Azure Blob Storage, consulte [Inicio rápido: Creación, descarga y enumeración de blobs mediante la CLI de Azure](../../storage/blobs/storage-quickstart-blobs-cli.md).

## <a name="next-steps"></a>Pasos siguientes

Para obtener información sobre otras maneras de trabajar con Hadoop en HDInsight:

* [Uso de MapReduce con Apache Hadoop en HDInsight](hdinsight-use-mapreduce.md)
* [Uso de Apache Hive con Apache Hadoop en HDInsight](hdinsight-use-hive.md)

Para obtener más información sobre la interfaz REST utilizada en este artículo, consulte la [referencia de WebHCat](https://cwiki.apache.org/confluence/display/Hive/WebHCat+Reference).
