---
title: Sugerencias para usar Hadoop en HDInsight basado en Linux (Azure)
description: Obtenga sugerencias de implementación para usar clústeres de HDInsight basado en Linux (Hadoop) en un entorno de Linux conocido que se ejecuta en la nube de Azure.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive,seoapr2020
ms.topic: conceptual
ms.date: 04/29/2020
ms.openlocfilehash: 55ffd563ea0a99d32608bd90bd53d7dc88eb4cf2
ms.sourcegitcommit: 845a55e6c391c79d2c1585ac1625ea7dc953ea89
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2020
ms.locfileid: "85961819"
---
# <a name="information-about-using-hdinsight-on-linux"></a>Información sobre el uso de HDInsight en Linux

Los clústeres de Azure HDInsight proporcionan Apache Hadoop en un entorno conocido de Linux, que se ejecuta en la nube de Azure. En la mayoría de los casos, debiera funcionar exactamente como cualquier otra instalación de Hadoop en Linux. Este documento detalla las diferencias específicas que debe tener en cuenta.

## <a name="prerequisites"></a>Prerrequisitos

Muchos de los pasos de este documento utilizan las siguientes utilidades, que pueden tener que instalarse en el sistema.

* [cURL](https://curl.haxx.se/): se usa para comunicarse con servicios basados en web.
* **jq**, un procesador JSON de línea de comandos.  Vea [https://stedolan.github.io/jq/](https://stedolan.github.io/jq/).
* [CLI de Azure](https://docs.microsoft.com/cli/azure/install-azure-cli): se usa para administrar servicios de Azure remotamente.
* **Un cliente SSH**. Para más información, consulte [Conexión a través de SSH con HDInsight (Apache Hadoop)](hdinsight-hadoop-linux-use-ssh-unix.md).

## <a name="users"></a>Usuarios

A menos que esté [unido a un dominio](./domain-joined/hdinsight-security-overview.md), HDInsight debe considerarse un sistema de **un solo usuario**. Se crea una sola cuenta de usuario SSH con el clúster, con permisos de nivel de administrador. Se pueden crear cuentas adicionales de SSH, pero también tienen acceso de administrador al clúster.

HDInsight unido a un dominio admite varios usuarios y una configuración más granular de los permisos y roles. Para más información, consulte [Manage domain-joined HDInsight clusters](./domain-joined/apache-domain-joined-manage.md) (Administración de clústeres de HDInsight unidos a dominio).

## <a name="domain-names"></a>Nombres de dominio

El nombre de dominio completo (FQDN) que se usa al conectarse al clúster desde Internet es `CLUSTERNAME.azurehdinsight.net` o `CLUSTERNAME-ssh.azurehdinsight.net` (solo para SSH).

De forma interna, cada nodo del clúster tiene un nombre que se asigna durante la configuración del clúster. Para buscar los nombres de clúster, consulte la página **Hosts** en la interfaz de usuario web de Ambari. También puede usar lo siguiente para devolver una lista de hosts desde la API de REST de Ambari:

```console
curl -u admin -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/hosts" | jq '.items[].Hosts.host_name'
```

Reemplace `CLUSTERNAME` por el nombre del clúster. Cuando se le solicite, escriba la contraseña de la cuenta de administrador. Este comando devuelve un documento JSON que contiene una lista de los hosts del clúster. [jq](https://stedolan.github.io/jq/) se usa para extraer el valor del elemento `host_name` de cada host.

Si necesita encontrar el nombre del nodo de un servicio específico, puede consultar a Ambari por ese componente. Por ejemplo, para encontrar los hosts del nodo de nombres HDFS, use el siguiente comando:

```console
curl -u admin -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/HDFS/components/NAMENODE" | jq '.host_components[].HostRoles.host_name'
```

Este comando devuelve un documento JSON que describe el servicio y, luego, [jq](https://stedolan.github.io/jq/) extrae solo el valor `host_name` para los hosts.

## <a name="remote-access-to-services"></a>Acceso remoto a los servicios

* **Ambari (web)**  - `https://CLUSTERNAME.azurehdinsight.net`

    Realice la autenticación con el usuario y la contraseña del administrador de clúster y, a continuación, inicie sesión en Ambari.

    La autenticación es texto no cifrado: use siempre HTTPS para asegurarse de que la conexión sea segura.

    > [!IMPORTANT]  
    > Algunas de la interfaces de usuario web disponibles a través de Ambari acceden a los nodos con un nombre de dominio interno. Los nombres de dominio internos no son accesibles públicamente a través de Internet. Puede recibir errores de "servidor no encontrado" al intentar acceder a algunas características a través de Internet.
    >
    > Para usar la funcionalidad completa de la interfaz de usuario de la web Ambari, usa un túnel SSH para delegar el tráfico web al nodo principal del clúster. Consulte [Uso de la tunelización SSH para tener acceso a la interfaz de usuario Ambari Web, ResourceManager, JobHistory, NameNode, Oozie y otras interfaces de usuario web](hdinsight-linux-ambari-ssh-tunnel.md)

* **Ambari (REST)**  - `https://CLUSTERNAME.azurehdinsight.net/ambari`

    > [!NOTE]  
    > Realice la autenticación con el usuario y la contraseña del administrador de clúster.
    >
    > La autenticación es texto no cifrado: use siempre HTTPS para asegurarse de que la conexión sea segura.

* **WebHCat (Templeton)**  - `https://CLUSTERNAME.azurehdinsight.net/templeton`

    > [!NOTE]  
    > Realice la autenticación con el usuario y la contraseña del administrador de clúster.
    >
    > La autenticación es texto no cifrado: use siempre HTTPS para asegurarse de que la conexión sea segura.

* **SSH**: CLUSTERNAME-ssh.azurehdinsight.net en los puertos 22 o 23. El puerto 22 se usa para conectarse al nodo principal primario, mientras que el 23 se usa para conectarse al secundario. Para más información acerca de los nodos principales, consulte [Disponibilidad y confiabilidad de clústeres de Apache Hadoop en HDInsight](hdinsight-high-availability-linux.md).

    > [!NOTE]  
    > Solo puede tener acceso a los nodos principales del clúster a través de SSH desde un equipo cliente. Una vez conectado, puede acceder a los nodos de trabajo usando SSH desde un nodo principal.

Para más información, consulte el documento [Puertos utilizados por los servicios Apache Hadoop en HDInsight](hdinsight-hadoop-port-settings-for-services.md).

## <a name="file-locations"></a>Ubicaciones de archivo

Puede encontrar los archivos relacionados con Hadoop en los nodos de clúster en `/usr/hdp`. Este directorio raíz contiene los siguientes subdirectorios:

* **2.6.5.3009-43**: El nombre del directorio es la versión de la plataforma Hadoop utilizada por HDInsight. El número del clúster puede ser diferente al que aparece aquí.
* **current**: este directorio contiene vínculos a los subdirectorios del directorio **2.6.5.3009-43**. Este directorio existe para que no tenga que recordar el número de versión.

Se pueden encontrar datos de ejemplo y archivos JAR en el sistema de archivos distribuido de Hadoop en `/example` y `/HdiSamples`.

## <a name="hdfs-azure-storage-and-data-lake-storage"></a>HDFS, Azure Storage y Data Lake Storage

En la mayoría de las distribuciones de Hadoop, los datos se almacenan en HDFS. Se crean copias de seguridad de HDFS en el almacenamiento local de las máquinas del clúster. El uso del almacenamiento local puede ser costoso para una solución basada en la nube en la que se le cobra por hora o por minuto por los recursos de proceso.

Al usar HDInsight, los archivos de datos se almacenan de una forma adaptable y resistente en la nube mediante Azure Blob Storage y, opcionalmente, Azure Data Lake Storage. Estos servicios ofrecen las siguientes ventajas:

* Almacenamiento a largo plazo económico.
* Accesibilidad desde servicios externos, como sitios web, utilidades para la carga y descarga de archivos, SDK en varios idiomas y exploradores web.
* Capacidad para archivos de gran tamaño y gran almacenamiento adaptable.

Para más información, consulte [Introducción a los blobs](https://docs.microsoft.com/rest/api/storageservices/understanding-block-blobs--append-blobs--and-page-blobs) y [Data Lake Storage](https://azure.microsoft.com/services/storage/data-lake-storage/).

Cuando se usa Azure Storage o Data Lake Storage, no tiene que hacer nada especial desde HDInsight para acceder a los datos. Por ejemplo, el comando siguiente enumera los archivos existentes en la carpeta `/example/data` tanto si está almacenada en Azure Storage como en Data Lake Storage:

```console
hdfs dfs -ls /example/data
```

En HDInsight, los recursos de almacenamiento de datos (Azure Blob Storage y Azure Data Lake Storage) se desacoplan de los recursos de proceso. Puede crear clústeres de HDInsight para realizar cálculos según sea necesario y, posteriormente, eliminar el clúster cuando finalice el trabajo. Mientras tanto, los archivos de datos se conservan de forma segura en el almacenamiento en la nube durante el tiempo que sea necesario.

### <a name="uri-and-scheme"></a><a name="URI-and-scheme"></a>Identificador URI y esquema

Algunos comandos pueden pedirle que especifique el esquema como parte del identificador URI al acceder a un archivo. Por ejemplo, el componente Storm-HDFS le pide que especifique el esquema. Cuando se usa un almacenamiento no predeterminado (almacenamiento agregado como almacenamiento "adicional" al clúster), debe utilizar siempre el esquema como parte del identificador URI.

Cuando use [**Azure Storage**](./hdinsight-hadoop-use-blob-storage.md), utilice uno de los siguientes esquemas de URI:

* `wasb:///`: acceso al almacenamiento predeterminado mediante comunicación sin cifrar.

* `wasbs:///`: acceso al almacenamiento predeterminado mediante comunicación cifrada.  El esquema wasbs solo se admite en la versión 3.6 y posteriores de HDInsight.

* `wasb://<container-name>@<account-name>.blob.core.windows.net/`: se usa al comunicarse con una cuenta de almacenamiento no predeterminada. Por ejemplo, cuando tiene una cuenta de almacenamiento adicional o accede a los datos almacenados en una cuenta de almacenamiento que es accesible públicamente.

Cuando use [**Azure Data Lake Storage Gen2**](./hdinsight-hadoop-use-data-lake-storage-gen2.md), utilice el siguiente esquema de URI:

* `abfs://`: acceso al almacenamiento predeterminado mediante comunicación cifrada.

* `abfs://<container-name>@<account-name>.dfs.core.windows.net/`: se usa al comunicarse con una cuenta de almacenamiento no predeterminada. Por ejemplo, cuando tiene una cuenta de almacenamiento adicional o accede a los datos almacenados en una cuenta de almacenamiento que es accesible públicamente.

Cuando use [**Azure Data Lake Storage Gen1**](./hdinsight-hadoop-use-data-lake-store.md), utilice uno de los siguientes esquemas de URI:

* `adl:///`: accede al almacén Data Lake Storage predeterminado del clúster.

* `adl://<storage-name>.azuredatalakestore.net/`: se usa al comunicarse con un almacén Data Lake Storage no predeterminado. También se usa para acceder a datos de fuera del directorio raíz del clúster de HDInsight.

> [!IMPORTANT]  
> Cuando se usa Data Lake Storage como almacén predeterminado de HDInsight, debe especificar una ruta de acceso en el almacén que se usará como la raíz de almacenamiento para HDInsight. La ruta de acceso predeterminada es `/clusters/<cluster-name>/`.
>
> Cuando se usa `/` o `adl:///` para acceder a los datos, solo puede acceder a los datos almacenados en la raíz (por ejemplo, `/clusters/<cluster-name>/`) del clúster. Para acceder a datos en cualquier lugar del almacén, use el formato `adl://<storage-name>.azuredatalakestore.net/`.

### <a name="what-storage-is-the-cluster-using"></a>¿Qué almacenamiento usa el clúster?

Puede usar Ambari para recuperar la configuración de almacenamiento predeterminada del clúster. Utilice el siguiente comando para recuperar información de configuración de HDFS con curl y filtrarla mediante [jq](https://stedolan.github.io/jq/):

```bash
curl -u admin -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/configurations/service_config_versions?service_name=HDFS&service_config_version=1" | jq '.items[].configurations[].properties["fs.defaultFS"] | select(. != null)'
```

> [!NOTE]  
> Este comando devuelve la primera configuración aplicada al servidor (`service_config_version=1`), que contiene esta información. Es posible que tenga que enumerar todas las versiones de configuración para encontrar la más reciente.

Este comando devuelve un valor similar a los siguientes URI:

* `wasb://<container-name>@<account-name>.blob.core.windows.net` si usa una cuenta de Azure Storage.

    El nombre de cuenta es el nombre de la cuenta de Azure Storage. El nombre del contenedor es el contenedor de blobs que es la raíz de almacenamiento de clúster.

* `adl://home` si se usa Azure Data Lake Storage. Para obtener el nombre de la instancia de Data Lake Storage, use la siguiente llamada REST:

     ```bash
    curl -u admin -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/configurations/service_config_versions?service_name=HDFS&service_config_version=1" | jq '.items[].configurations[].properties["dfs.adls.home.hostname"] | select(. != null)'
    ```

    Este comando devuelve el siguiente nombre de host: `<data-lake-store-account-name>.azuredatalakestore.net`.

    Para obtener el directorio del almacén que es la raíz de HDInsight, use la siguiente llamada REST:

    ```bash
    curl -u admin -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/configurations/service_config_versions?service_name=HDFS&service_config_version=1" | jq '.items[].configurations[].properties["dfs.adls.home.mountpoint"] | select(. != null)'
    ```

    Este comando devuelve una ruta de acceso similar a la siguiente: `/clusters/<hdinsight-cluster-name>/`.

También puede encontrar la información de almacenamiento mediante Azure Portal realizando los siguientes pasos:

1. En el [Portal de Azure](https://portal.azure.com/), seleccione el clúster de HDInsight.

2. En la sección **Propiedades**, seleccione **Cuentas de almacenamiento**. Se muestra la información de almacenamiento del clúster.

### <a name="how-do-i-access-files-from-outside-hdinsight"></a>¿Cómo accedo a los archivos desde fuera de HDInsight?

Hay varias maneras de acceder a los datos desde fuera del clúster de HDInsight. Los siguientes son algunos vínculos a las utilidades y SDK que puede usar para trabajar con los datos:

Si usa __Azure Storage__, consulte los siguientes vínculos para ver las formas en que puede acceder a los datos:

* [CLI de Azure](https://docs.microsoft.com/cli/azure/install-az-cli2): comandos de la interfaz de la línea de comandos para trabajar con Azure. Después de la instalación, use el comando `az storage` para obtener ayuda sobre el uso del almacenamiento o `az storage blob` para comandos específicos para los blobs.
* [blobxfer.py](https://github.com/Azure/blobxfer): script de Python para trabajar con blobs en Azure Storage.
* Diversos SDK:

    * [Java](https://github.com/Azure/azure-sdk-for-java)
    * [Node.js](https://github.com/Azure/azure-sdk-for-node)
    * [PHP](https://github.com/Azure/azure-sdk-for-php)
    * [Python](https://github.com/Azure/azure-sdk-for-python)
    * [Ruby](https://github.com/Azure/azure-sdk-for-ruby)
    * [.NET](https://github.com/Azure/azure-sdk-for-net)
    * [API de REST de almacenamiento](https://msdn.microsoft.com/library/azure/dd135733.aspx)

Si usa __Azure Data Lake Storage__, consulte los siguientes vínculos para ver las formas en que puede acceder a los datos:

* [Explorador web](../data-lake-store/data-lake-store-get-started-portal.md)
* [PowerShell](../data-lake-store/data-lake-store-get-started-powershell.md)
* [CLI de Azure](../data-lake-store/data-lake-store-get-started-cli-2.0.md)
* [API de REST de WebHDFS](../data-lake-store/data-lake-store-get-started-rest-api.md)
* [Data Lake Tools para Visual Studio](https://www.microsoft.com/download/details.aspx?id=49504)
* [.NET](../data-lake-store/data-lake-store-get-started-net-sdk.md)
* [Java](../data-lake-store/data-lake-store-get-started-java-sdk.md)
* [Python](../data-lake-store/data-lake-store-get-started-python.md)

## <a name="scaling-your-cluster"></a><a name="scaling"></a>Escalar el clúster

La característica de escalado de clúster permite cambiar de forma dinámica la cantidad de nodos de datos que usa un clúster. Puede realizar operaciones de escalado mientras se están ejecutando otros trabajos o procesos en un clúster.  Consulte [Escalamiento de clústeres de HDInsight](./hdinsight-scaling-best-practices.md)

## <a name="how-do-i-install-hue-or-other-hadoop-component"></a>¿Cómo puedo instalar Hue (u otro componente de Hadoop)?

HDInsight es un servicio administrado. Si Azure detecta un problema con el clúster, podría eliminar el nodo que ha dado error y crear un nodo para sustituirlo. Al instalar manualmente elementos en el clúster, no se conservan cuando se produce esta operación. En su lugar, use [acciones de script de HDInsight](hdinsight-hadoop-customize-cluster-linux.md). Una acción de script se puede usar para realizar los siguientes cambios:

* Instalar y configurar un servicio o un sitio web.
* Instalar y configurar un componente que requiera cambios de configuración en varios nodos del clúster.

Las acciones de script son scripts de Bash. Los scripts se ejecutan durante la creación del clúster y se pueden usar para instalar y configurar componentes adicionales. Para obtener información sobre el desarrollo de sus propias acciones de script, consulte [Desarrollo de la acción de script con HDInsight](hdinsight-hadoop-script-actions-linux.md).

### <a name="jar-files"></a>Archivos JAR

Algunas tecnologías de Hadoop proporcionan archivos jar independientes. Estos archivos contienen funciones que se usan como parte de un trabajo de MapReduce o desde dentro de Pig o Hive. No suelen requerir ninguna configuración y, además, se pueden cargar en el clúster después de su creación y usarlas directamente. Si desea asegurarse de que el componente sobreviva a la creación de una nueva imagen del clúster, almacene el archivo jar en el almacenamiento predeterminado del clúster.

Por ejemplo, si desea usar la versión más reciente de [Apache DataFu](https://datafu.incubator.apache.org/), puede descargar un archivo jar que contiene el proyecto y cargarlo en el clúster de HDInsight. A continuación, siga las instrucciones que aparecen en la documentación de DataFu sobre el uso con Pig o Hive.

> [!IMPORTANT]  
> Algunos componentes que son archivos jar independientes se proporcionan con HDInsight, pero no están en la ruta de acceso. Si está buscando un componente específico, puede utilizar lo siguiente para buscarlo en el clúster:
>
> ```find / -name *componentname*.jar 2>/dev/null```
>
> Este comando devuelve la ruta de acceso de cualquier archivo jar coincidente.

Para usar otra versión de un componente, cargue la versión que necesita y úsela en los trabajos.

> [!IMPORTANT]
> Los componentes proporcionados con el clúster de HDInsight son totalmente compatibles. Además, el soporte técnico de Microsoft lo ayudará a aislar y resolver problemas relacionados con estos componentes.
>
> Los componentes personalizados reciben soporte técnico comercialmente razonable para ayudarle a solucionar el problema. Esto podría resolver el problema o pedirle que forme parte de los canales disponibles para las tecnologías de código abierto donde se encuentra la más amplia experiencia para esa tecnología. Por ejemplo, hay diversos sitios de la comunidad que se pueden usar, como el [Página de preguntas y respuestas de Microsoft sobre HDInsight](https://docs.microsoft.com/answers/topics/azure-hdinsight.html), [https://stackoverflow.com](https://stackoverflow.com). Los proyectos de Apache también tienen sitios de proyecto en [https://apache.org](https://apache.org), como por ejemplo: [Hadoop](https://hadoop.apache.org/), [Spark](https://spark.apache.org/).

## <a name="next-steps"></a>Pasos siguientes

* [Administración de clústeres de HDInsight mediante la API REST de Apache Ambari](./hdinsight-hadoop-manage-ambari-rest-api.md)
* [Uso de Apache Hive con HDInsight](hadoop/hdinsight-use-hive.md)
* [Uso de trabajos de MapReduce con HDInsight](hadoop/hdinsight-use-mapreduce.md)
