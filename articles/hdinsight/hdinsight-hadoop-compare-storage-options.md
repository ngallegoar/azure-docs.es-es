---
title: Comparación de opciones de almacenamiento para los clústeres de Azure HDInsight
description: Proporciona información general sobre los tipos de almacenamiento y cómo funcionan con Azure HDInsight.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 03/10/2020
ms.openlocfilehash: 98d71434ac9e3f712be0cbd8c505b7d5a537e7cc
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79095551"
---
# <a name="compare-storage-options-for-use-with-azure-hdinsight-clusters"></a>Comparación de opciones de almacenamiento para los clústeres de Azure HDInsight

Puede elegir entre diferentes servicios de Azure Storage al crear clústeres de HDInsight:

* Azure Storage
* Azure Data Lake Storage Gen2
* Azure Data Lake Storage Gen1

En este artículo se proporciona información general sobre estos tipos de almacenamiento y sus características exclusivas.

En la tabla siguiente se resumen los servicios de Azure Storage que son compatibles con diferentes versiones de HDInsight:

| Servicio de Storage | Tipo de cuenta | Tipo de espacio de nombres | Servicios admitidos | Niveles de rendimiento admitidos | Niveles de acceso admitidos | Versión de HDInsight | Tipo de clúster |
|---|---|---|---|---|---|---|---|
|Azure Data Lake Storage Gen2| Uso general v2 | Jerárquico (sistema de archivos) | Blob | Estándar | Frecuente, esporádico, archivo | 3.6+ | Todos excepto Spark 2.1 y 2.2|
|Azure Storage| Uso general v2 | Object | Blob | Estándar | Frecuente, esporádico, archivo | 3.6+ | All |
|Azure Storage| Uso general v1 | Object | Blob | Estándar | N/D | All | All |
|Azure Storage| Blob Storage** | Object | Blob en bloques | Estándar | Frecuente, esporádico, archivo | All | All |
|Azure Data Lake Storage Gen1| N/D | Jerárquico (sistema de archivos) | N/D | N/D | N/D | Solo 3.6 | Todos excepto HBase |

**Para los clústeres de HDInsight, solo las cuentas de almacenamiento secundarias pueden ser de tipo BlobStorage; Blob en páginas no es una opción de almacenamiento admitida.

Para obtener más información sobre los tipos de cuentas de almacenamiento, consulte [Información general acerca de la cuenta de Azure Storage](../storage/common/storage-account-overview.md).

Para obtener más información sobre los niveles de acceso de Azure Storage, consulte [Azure Blob Storage: niveles de almacenamiento de archivo, esporádico, frecuente y Premium (versión preliminar)](../storage/blobs/storage-blob-storage-tiers.md).

Puede crear un clúster mediante diversas combinaciones de servicios para el almacenamiento principal y secundario opcional. En la tabla siguiente se resumen las configuraciones de almacenamiento de clúster que actualmente se admiten en HDInsight:

| Versión de HDInsight | Almacenamiento principal | Almacenamiento secundario | Compatible |
|---|---|---|---|
| 3.6 y 4.0 | Uso general V1, uso general V2 | Uso general V1, uso general V2, BlobStorage (blobs en bloques) | Sí |
| 3.6 y 4.0 | Uso general V1, uso general V2 | Data Lake Storage Gen2 | No |
| 3.6 y 4.0 | Data Lake Storage Gen2* | Data Lake Storage Gen2 | Sí |
| 3.6 y 4.0 | Data Lake Storage Gen2* | Uso general V1, uso general V2, BlobStorage (blobs en bloques) | Sí |
| 3.6 y 4.0 | Data Lake Storage Gen2 | Data Lake Storage Gen1 | No |
| 3.6 | Data Lake Storage Gen1 | Data Lake Storage Gen1 | Sí |
| 3.6 | Data Lake Storage Gen1 | Uso general V1, uso general V2, BlobStorage (blobs en bloques) | Sí |
| 3.6 | Data Lake Storage Gen1 | Data Lake Storage Gen2 | No |
| 4.0 | Data Lake Storage Gen1 | Any | No |
| 4.0 | Uso general V1, uso general V2 | Data Lake Storage Gen1 | No |

* Podría tratarse de una o varias cuentas de Data Lake Storage Gen2, siempre y cuando todas estén configuradas para usar la misma identidad administrada para el acceso al clúster.

> [!Note] 
> El almacenamiento principal de Data Lake Storage Gen2 no es compatible con los clústeres de Spark 2.1 o 2.2. 

## <a name="use-azure-data-lake-storage-gen2-with-apache-hadoop-in-azure-hdinsight"></a>Uso de Azure Data Lake Storage Gen2 con Apache Hadoop en Azure HDInsight

Azure Data Lake Storage Gen2 toma las características principales de Azure Data Lake Storage Gen1 y las integra en Azure Blob Storage. Estas características incluyen un sistema de archivos que es compatible con Hadoop, Azure Active Directory (Azure AD) y listas de control de acceso basadas en POSIX. Esta combinación permite aprovechar las ventajas de rendimiento de Azure Data Lake Storage Gen1, al mismo tiempo que se usan la administración del ciclo de vida de los datos y las capas de Blob Storage.

Para obtener más información sobre Data Lake Storage Gen2, consulte [Introducción a Azure Data Lake Storage Gen2](../storage/blobs/data-lake-storage-introduction.md).

### <a name="core-functionality-of-azure-data-lake-storage-gen2"></a>Funcionalidad básica de Azure Data Lake Storage Gen2

* **Acceso compatible con Hadoop:** En Azure Data Lake Storage Gen2, puede administrar datos y acceder a ellos igual que lo haría con un sistema de archivos distribuido de Hadoop (HDFS). El controlador del sistema de archivos de Azure Blob (ABFS) está disponible en todos los entornos de Apache Hadoop, incluidos Azure HDInsight y Azure Databricks. Use ABFS para acceder a los datos almacenados en Data Lake Storage Gen2.

* **Superconjunto de permisos POSIX**: el modelo de seguridad de Data Lake Gen2 es compatible con los permisos de ACL y POSIX junto con granularidad adicional específica de Data Lake Storage Gen2. Se puede realizar la configuración mediante herramientas de administración o marcos, como Apache Hive y Apache Spark.

* **Rentabilidad**: Data Lake Storage Gen2 ofrece transacciones y capacidad de almacenamiento de bajo costo. Características como el ciclo de vida de Azure Blob Storage ayudan a reducir los costos mediante el ajuste de las tarifas de facturación a medida que los datos avanzan en su ciclo de vida.

* **Compatibilidad con aplicaciones, marcos y herramientas de Blob Storage**: Data Lake Storage Gen2 sigue funcionando con una amplia gama de herramientas, marcos y aplicaciones para Blob Storage.

* **Controlador optimizado**: el controlador ABFS está optimizado específicamente para el análisis de macrodatos. Las API REST correspondientes se exponen a través del punto de conexiónde sistema de archivo distribuido (dfs),dfs.core.windows.net.

### <a name="whats-new-for-azure-data-lake-storage-gen-2"></a>Novedades de Azure Data Lake Storage Gen2

#### <a name="managed-identities-for-secure-file-access"></a>Identidades administradas para proteger el acceso a archivos

Azure HDInsight usa identidades administradas para proteger el acceso del clúster a los archivos de Azure Data Lake Storage Gen2. Las identidades administradas son una característica de Azure Active Directory que proporciona servicios de Azure con un conjunto de credenciales administradas automáticamente. Estas credenciales se pueden usar para autenticarse en cualquier servicio que admita la autenticación de Active Directory. El uso de identidades administradas no requiere almacenar las credenciales en archivos de código o de configuración.

Para obtener más información, consulte [Managed identities for Azure resources](../active-directory/managed-identities-azure-resources/overview.md) (Identidades administradas para los recursos de Azure).

#### <a name="azure-blob-file-system-driver"></a>Controlador Azure Blob File System

Las aplicaciones de Apache Hadoop de forma nativa esperan leer y escribir datos desde el almacenamiento en disco local. Un controlador del sistema de archivos de Hadoop como ABFS permite que las aplicaciones de Hadoop funcionen con el almacenamiento en la nube mediante la emulación de las operaciones habituales del sistema de archivos de Hadoop. El controlador convierte los comandos que recibe desde la aplicación en operaciones que la plataforma real de almacenamiento en la nube reconoce.

Anteriormente, el controlador del sistema de archivos de Hadoop convertía todas las operaciones del sistema de archivos en llamadas API REST de Azure Storage en el lado cliente y, a continuación, invocaba la API REST. Sin embargo, esta conversión del lado cliente resultaba en varias llamadas API de REST para una única operación del sistema de archivos, como cambiar el nombre a un archivo. ABFS movió parte de la lógica de sistema de archivos Hadoop del lado cliente al lado servidor. La API de Azure Data Lake Storage Gen2 ahora se ejecuta en paralelo con la API de Blob. Esta migración mejora el rendimiento porque ahora se pueden ejecutar operaciones comunes del sistema de archivos de Hadoop con una llamada API REST.

Para más información, vea [Controlador Azure Blob FileSystem (ABFS): un controlador de Azure Storage dedicado para Hadoop](../storage/blobs/data-lake-storage-abfs-driver.md).

#### <a name="uri-scheme-for-azure-data-lake-storage-gen-2"></a>Esquema de URI de Azure Data Lake Storage Gen2 

Azure Data Lake Storage Gen2 usa un nuevo esquema de identificador URI para acceder a los archivos de Azure Storage desde HDInsight:

`abfs://<FILE_SYSTEM_NAME>@<ACCOUNT_NAME>.dfs.core.windows.net/<PATH>`

El esquema de URI proporciona acceso con cifrado SSL.

`<FILE_SYSTEM_NAME>` identifica la ruta de acceso del sistema de archivos de Data Lake Storage Gen2.

`<ACCOUNT_NAME>` identifica el nombre de la cuenta de Azure Storage. Se necesita el nombre completo de dominio (FQDN).

`<PATH>` es el nombre de la ruta HDFS del archivo o el directorio.

Si no se especifican los valores de `<FILE_SYSTEM_NAME>` y `<ACCOUNT_NAME>`, se utiliza el sistema de archivos predeterminado. Para los archivos del sistema de archivos predeterminado, puede usar una ruta relativa o absoluta. Por ejemplo, se puede hacer referencia al archivo `hadoop-mapreduce-examples.jar` que se incluye con los clústeres de HDInsight mediante alguna de las rutas de acceso siguientes:

```
abfs://myfilesystempath@myaccount.dfs.core.windows.net/example/jars/hadoop-mapreduce-examples.jar
abfs:///example/jars/hadoop-mapreduce-examples.jar /example/jars/hadoop-mapreduce-examples.jar
```

> [!Note]
> El nombre del archivo es `hadoop-examples.jar` en las versiones 2.1 y 1.6 de los clústeres de HDInsight. Al trabajar con archivos fuera de HDInsight, verá que la mayoría de las utilidades no reconocen el formato ABFS y, en su lugar, esperan un formato básico de ruta de acceso, como `example/jars/hadoop-mapreduce-examples.jar`.

Para más información, vea [Uso del URI de Azure Data Lake Storage Gen2](../storage/blobs/data-lake-storage-introduction-abfs-uri.md).

## <a name="azure-storage"></a>Azure Storage

Azure Storage es una solución de almacenamiento sólida y de uso general, que se integra sin problemas con HDInsight. HDInsight puede usar un contenedor de blobs en Azure Storage como el sistema de archivos predeterminado para el clúster. Mediante una interfaz HDFS, el conjunto completo de componentes de HDInsight puede operar directamente en datos estructurados o no estructurados almacenados como blobs.

Se recomienda usar contenedores de almacenamiento independientes para el almacenamiento predeterminado del clúster y los datos empresariales, a fin de aislar, por un lado, los archivos temporales y los registros de HDInsight y, por el otro, los datos empresariales. También se recomienda eliminar el contenedor de blobs predeterminado, que contiene los registros de la aplicación y del sistema, después de cada uso para reducir los costes de almacenamiento. Asegúrese de recuperar los registros antes de eliminar el contenedor.

Si decide proteger la cuenta de almacenamiento con las restricciones de **firewalls y redes virtuales** en **redes seleccionadas**, asegúrese de habilitar la excepción **Permitir servicios de Microsoft de confianza...** para que HDInsight pueda acceder a su cuenta de almacenamiento.

### <a name="hdinsight-storage-architecture"></a>Arquitectura de almacenamiento de HDInsight

El diagrama siguiente proporciona una panorámica de la arquitectura de HDInsight de Azure Storage:

![Arquitectura de almacenamiento para HDInsight](./media/hdinsight-hadoop-compare-storage-options/storage-architecture.png "Arquitectura de almacenamiento para HDInsight")

HDInsight brinda acceso al sistema de archivos distribuidos que se adjunta localmente a los nodos de ejecución. Se puede acceder a este sistema de archivos usando el URI completo, por ejemplo:

    hdfs://<namenodehost>/<path>

A través de HDInsight también puede tener acceso a datos en Azure Storage. La sintaxis es la siguiente:

    wasb://<containername>@<accountname>.blob.core.windows.net/<path>

A la hora de usar una cuenta de Azure Storage con clústeres de HDInsight, es necesario considerar lo siguiente:

* **Contenedores de las cuentas de almacenamiento que se conectan a un clúster:** dado que el nombre y la clave de la cuenta se asocian al clúster durante la creación, tiene acceso total a los blobs de dichos contenedores.

* **Contenedores públicos o blobs públicos de las cuentas de almacenamiento que *NO* se conectan a un clúster:** tiene permiso de solo lectura de los blobs de los contenedores.
  
  > [!NOTE]  
  > Los contenedores públicos le permiten obtener una lista de todos los blobs disponibles del contenedor en cuestión y obtener sus metadatos. Los blobs públicos le permiten acceder a los blobs solo si conoce la URL exacta. Para más información, consulte [Administración del acceso de lectura anónimo a contenedores y blobs](../storage/blobs/storage-manage-access-to-resources.md).

* **Contenedores privados de las cuentas de almacenamiento que *NO* se conectan a un clúster:** no puede tener acceso a los blobs de los contenedores a menos que defina la cuenta de almacenamiento al enviar los trabajos de WebHCat. 

Las cuentas de almacenamiento definidas en el proceso de creación y sus claves se almacenan en %HADOOP_HOME%/conf/core-site.xml en los nodos de clúster. De manera predeterminada, HDInsight usa las cuentas de almacenamiento definidas en el archivo core-site.xml. Puede modificar esta configuración mediante [Apache Ambari](./hdinsight-hadoop-manage-ambari.md).

Varios trabajos de WebHCat, incluidos Apache Hive, MapReduce, streaming de Apache Hadoop y Apache Pig, pueden llevar una descripción de cuentas de almacenamiento y metadatos con ellos. (Actualmente esto funciona para Pig con cuentas de almacenamiento pero no para metadatos). Para obtener más información, consulte [Uso de un clúster de HDInsight con cuentas de almacenamiento y tiendas de metadatos alternativas](https://social.technet.microsoft.com/wiki/contents/articles/23256.using-an-hdinsight-cluster-with-alternate-storage-accounts-and-metastores.aspx).

Los blobs se pueden usar para datos estructurados y no estructurados. Los contenedores de blobs almacenan los datos como pares de clave-valor y no tienen jerarquía de directorios. No obstante, el nombre de clave puede incluir un carácter de barra diagonal ( / ) para que parezca que el archivo está almacenado dentro de una estructura de directorios. Por ejemplo, la clave de un blob puede ser `input/log1.txt`. No existe un directorio `input`, pero debido a la barra diagonal en el nombre de clave, la clave se parece a una ruta de acceso de archivo.

### <a name="benefits-of-azure-storage"></a><a id="benefits"></a>Ventajas de Azure Storage
Los clústeres de proceso y los recursos de almacenamiento que no están colocados tienen costos de rendimiento implícitos. Estos costos se mitigan gracias a la manera en que los clústeres de cálculo se crean cerca de los recursos de la cuenta de almacenamiento dentro de la región de Azure. En esta región, los nodos de proceso pueden acceder eficazmente a los datos a través de la red de alta velocidad dentro de Azure Storage.

Al almacenar los datos en Azure Storage en lugar de HDFS, disfruta de varias ventajas:

* **Uso compartido y reutilización de datos**: los datos de HDFS se ubican dentro del clúster de proceso. Solamente las aplicaciones que tengan acceso al clúster de cálculo podrán usar los datos usando las API HDFS. En contraste, se puede acceder a los datos de Azure Storage mediante las API de HDFS o las API REST de Blob Storage. Debido a esta disposición, se puede usar un conjunto mayor de aplicaciones (incluyendo otros clústeres de HDInsight) y herramientas para producir y consumir los datos.

* **Archivado de datos**: Cuando los datos se almacenan en Azure Storage, los clústeres de HDInsight usados para el cálculo se eliminen de forma segura sin perder datos del usuario.

* **Costo de almacenamiento de datos**: Almacenar datos en DFS a largo plazo es más caro que almacenarlos en Azure Storage, ya que el costo de un clúster de proceso es superior al de Azure Storage. Además, como no hay que volver a cargar los datos para cada generación de clúster de proceso, también se ahorra en costos de carga de datos.

* **Escalabilidad horizontal elástica**: aunque HDFS proporciona un sistema de archivos escalable en horizontal, la escala se determina en función del número de nodos que cree para su clúster. Cambiar la escala puede ser un proceso más complicado que basarse en las funcionalidades de escalado elástico que se obtienen automáticamente en Azure Storage.

* **Replicación geográfica**: Su almacenamiento de Azure Storage se puede replicar geográficamente. Aunque la replicación geográfica aporta recuperación geográfica y redundancia de datos, una conmutación por error en la ubicación replicada geográficamente afecta gravemente al rendimiento y puede incurrir en costes adicionales. Por lo tanto, elija la replicación geográfica con prudencia y únicamente si merece la pena pagar el costo adicional por el valor de los datos.

Determinados trabajos y paquetes de MapReduce podrían crear resultados intermedios que realmente no desea almacenar en Azure Storage. En tal caso, puede optar por almacenar los datos en el HDFS local. HDInsight usa DFS para varios de estos resultados intermedios en los trabajos de Hive y otros procesos.

> [!NOTE]  
> La mayoría de los comandos HDFS (por ejemplo, `ls`, `copyFromLocal` y `mkdir`) siguen funcionando según lo previsto. Únicamente los comandos específicos de la implementación nativa de HDFS (a la que nos referiremos como DFS), como `fschk` y `dfsadmin`, muestran comportamientos diferentes en Azure Storage.

## <a name="overview-of-azure-data-lake-storage-gen1"></a>Introducción a Azure Data Lake Storage Gen1

Azure Data Lake Storage Gen1 es un repositorio a gran escala de nivel empresarial para cargas de trabajo de análisis de macrodatos. Azure Data Lake permite capturar datos de cualquier tamaño, tipo y velocidad de ingesta en un único lugar para realizar análisis exploratorios y operativos.

Se puede acceder a Data Lake Storage Gen1 desde Hadoop (disponible con un clúster de HDInsight) mediante las API REST compatibles con WebHDFS. Data Lake Storage Gen1 está diseñado para habilitar el análisis de los datos almacenados y está optimizado para el rendimiento en escenarios de análisis de datos. Incluye de manera predeterminada las funciones que son esenciales para casos de uso empresariales del mundo real. Estas funciones incluyen la seguridad, manejabilidad, escalabilidad, confiabilidad y disponibilidad.

Para obtener más información sobre Azure Data Lake Storage Gen1, consulte [Información general de Azure Data Lake Storage Gen1](../data-lake-store/data-lake-store-overview.md).

Entre las capacidades clave de Data Lake Storage Gen1 se incluyen las siguientes.

### <a name="compatibility-with-hadoop"></a>Compatibilidad con Hadoop

Data Lake Storage Gen1 es un sistema de archivos de Apache Hadoop compatible con HDFS y funciona con el ecosistema de Hadoop.  Las aplicaciones o los servicios de HDInsight existentes que usen la API de WebHDFS se pueden integrar fácilmente con Data Lake Storage Gen1. Además, Data Lake Storage Gen1 expone una interfaz de REST compatible con WebHDFS para aplicaciones.

Los datos almacenados en Data Lake Storage Gen1 se pueden analizar fácilmente mediante marcos analíticos de Hadoop como MapReduce o Hive. Los clústeres de Azure HDInsight se pueden aprovisionar y configurar para que accedan directamente a datos almacenados en Data Lake Storage Gen1.

### <a name="unlimited-storage-petabyte-files"></a>Almacenamiento ilimitado, archivos de petabytes de tamaño

Data Lake Storage Gen1 proporciona almacenamiento ilimitado y es adecuado para almacenar diversos datos para análisis. No se impone ningún límite al tamaño de cuenta, el tamaño de archivo o la cantidad de datos que se pueden almacenar en un Data Lake. El tamaño de los archivos individuales puede oscilar entre kilobytes y petabytes, por lo que Data Lake Storage Gen1 es una buena opción para almacenar cualquier tipo de datos. Los datos se almacenan de forma duradera mediante la realización de varias copias y no hay ningún límite al período de tiempo durante el que se pueden almacenar los datos en la instancia de Data Lake.

### <a name="performance-tuning-for-big-data-analytics"></a>Rendimiento optimizado para el análisis de macrodatos

Data Lake Storage Gen1 se creó para ejecutar sistemas de análisis a gran escala que requieren un procesamiento masivo con el fin de consultar y analizar grandes cantidades de datos. Data Lake distribuye partes de un archivo entre varios servidores de almacenamiento individuales. Cuando está analizando los datos, esta configuración mejora el rendimiento de lectura cuando se lee el archivo en paralelo.

### <a name="readiness-for-enterprise-highly-available-and-secure"></a>Preparación para la empresa: durable y con una elevada disponibilidad

Data Lake Storage Gen1 proporciona la confiabilidad y disponibilidad estándar del sector. Los recursos de datos se almacenan de forma duradera realizando copias redundantes para protegerse ante los errores inesperados. Las empresas pueden usar Data Lake Storage Gen1 en sus soluciones como parte importante de su plataforma de datos existente.

Además, Data Lake Storage Gen1 también proporciona seguridad de nivel empresarial para los datos almacenados. Para obtener más información, consulte [Securing data in Azure Data Lake Storage Gen1](#DataLakeStoreSecurity) (Protección de los datos almacenados en Azure Data Lake Storage Gen1).

### <a name="flexible-data-structures"></a>Estructuras de datos flexibles

Data Lake Storage Gen1 puede almacenar cualquier dato en su formato nativo, tal cual, sin necesidad de transformarlo antes. Data Lake Storage Gen1 no requiere la definición de un esquema antes de cargar los datos. Cada marco analítico interpreta los datos y define un esquema en el momento del análisis. Dado que puede almacenar archivos de formatos y tamaños arbitrarios, Data Lake Storage Gen1 puede administrar datos estructurados, semiestructurados y no estructurados.

Los contenedores de datos de Data Lake Storage Gen1 son básicamente carpetas y archivos. Se opera en los datos almacenados mediante los SDK, Azure Portal y Azure Powershell. Siempre que ponga los datos en el almacén mediante estas interfaces y los contenedores adecuados, puede almacenar cualquier tipo de datos. Data Lake Storage Gen1 no realiza ningún control especial de datos según el tipo de datos que almacene.

## <a name="data-security-in-data-lake-storage-gen1"></a><a name="DataLakeStoreSecurity"></a>Seguridad de datos en Data Lake Storage Gen1
Data Lake Storage Gen1 usa Azure Active Directory para la autenticación y listas de control de acceso (ACL) para administrar el acceso a los datos.

| **Característica** | **Descripción** |
| --- | --- |
| Authentication |Data Lake Storage Gen1 se integra con Azure Active Directory (Azure AD) para la administración de identidades y accesos para todos los datos almacenados en Data Lake Storage Gen1. Debido a la integración, Data Lake Storage Gen1 se beneficia de todas las características de Azure AD. Estas características incluyen la autenticación multifactor, el acceso condicional, el control de acceso basado en rol, la supervisión del uso de aplicaciones, la supervisión y las alertas de seguridad, etc. Igualmente, Data Lake Storage Gen1 es compatible con el protocolo OAuth 2.0 para la autenticación en la interfaz de REST. Consulte [Autenticación en Azure Data Lake Storage Gen1 con Azure Active Directory](../data-lake-store/data-lakes-store-authentication-using-azure-active-directory.md).|
| Control de acceso |Data Lake Storage Gen1 proporciona control de acceso gracias a la compatibilidad con los permisos de estilo POSIX que expone el protocolo WebHDFS. Las listas de control de acceso se pueden habilitar en la carpeta raíz, en subcarpetas y en archivos individuales. Para obtener más información sobre cómo funcionan las ACL en el contexto de Data Lake Storage Gen1, consulte [Control de acceso en Data Lake Storage Gen1](../data-lake-store/data-lake-store-access-control.md). |
| Cifrado |Data Lake Storage Gen1 también proporciona el cifrado de los datos que se almacenan en la cuenta. Puede especificar la configuración de cifrado mientras crea una cuenta de Data Lake Storage Gen1. Puede elegir si cifrar o no los datos. Para obtener más información, consulte el artículo de [Cifrado de datos en Data Lake Storage Gen1](../data-lake-store/data-lake-store-encryption.md). Para obtener instrucciones sobre cómo proporcionar una configuración relacionada con el cifrado, consulte [Introducción a Azure Data Lake Storage Gen1 con Azure Portal](../data-lake-store/data-lake-store-get-started-portal.md). |

Para obtener más información sobre cómo proteger datos en Data Lake Storage Gen1, consulte [Protección de los datos almacenados en Azure Data Lake Storage Gen1](../data-lake-store/data-lake-store-secure-data.md).

## <a name="applications-that-are-compatible-with-data-lake-storage-gen1"></a>Aplicaciones compatibles con Data Lake Storage Gen1
Data Lake Storage Gen1 es compatible con la mayor parte de los componentes de código abierto del ecosistema de Hadoop. También se integra perfectamente con otros servicios de Azure.  Para obtener más información acerca de cómo se puede usar Data Lake Storage Gen1 con componentes de código abierto y con otros servicios de Azure, siga estos vínculos.

* Consulte [Abrir aplicaciones de macrodatos de código abierto que funcionan con Azure Data Lake Storage Gen1](../data-lake-store/data-lake-store-compatible-oss-other-applications.md) para obtener una lista de aplicaciones de código abierto interoperables con Data Lake Storage Gen1.
* Consulte [Integración de Azure Data Lake Storage Gen1 con otros servicios de Azure](../data-lake-store/data-lake-store-integrate-with-other-services.md) para saber cómo usar Data Lake Storage Gen1 con otros servicios de Azure, y así poder obtener una gama más amplia de escenarios.
* Consulte [Uso de Azure Data Lake Storage Gen1 para requisitos de macrodatos](../data-lake-store/data-lake-store-data-scenarios.md) para obtener información acerca de cómo usar Data Lake Storage Gen1 en escenarios como la ingesta de datos, el procesamiento de datos, la descarga de datos y la visualización de datos.

## <a name="data-lake-storage-gen1-file-system-adl"></a>Sistema de archivos de Data Lake Storage Gen1 (adl://)
En entornos de Hadoop (disponibles con el clúster de HDInsight), se puede acceder a Data Lake Storage Gen1 a través del nuevo sistema de archivos, AzureDataLakeFilesystem (adl://). El rendimiento de las aplicaciones y servicios que usan adl:// se puede optimizar de maneras que no están actualmente disponibles en WebHDFS. Como resultado, al usar Data Lake Storage Gen1, obtiene la flexibilidad de elegir entre disponer del mejor rendimiento con la opción recomendada de adl:// o mantener el código existente usando la API de WebHDFS directamente. Azure HDInsight aprovecha completamente AzureDataLakeFilesystem para proporcionar el mejor rendimiento en Data Lake Storage Gen1.

Acceda a los datos en Data Lake Storage Gen1 mediante lo siguiente:

`adl://<data_lake_storage_gen1_name>.azuredatalakestore.net`

Para obtener más información sobre cómo acceder a los datos en Data Lake Storage Gen1, consulte [Acciones disponibles con los datos almacenados](../data-lake-store/data-lake-store-get-started-portal.md#properties).



## <a name="next-steps"></a>Pasos siguientes

* [Introducción a Azure Data Lake Storage Gen2](../storage/blobs/data-lake-storage-introduction.md)
* [Introducción a Almacenamiento de Azure](../storage/common/storage-introduction.md)
