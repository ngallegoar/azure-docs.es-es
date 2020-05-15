---
title: 'Infraestructura: Apache Hadoop local a Azure HDInsight'
description: Obtenga información acerca de las prácticas recomendadas de infraestructura para migrar clústeres locales de Apache Hadoop a Azure HDInsight.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive
ms.date: 12/06/2019
ms.openlocfilehash: 8407aafdb9b9dadcbc8e220ac42e5d7856116959
ms.sourcegitcommit: 309a9d26f94ab775673fd4c9a0ffc6caa571f598
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/09/2020
ms.locfileid: "82996775"
---
# <a name="migrate-on-premises-apache-hadoop-clusters-to-azure-hdinsight---infrastructure-best-practices"></a>Procedimientos recomendados de infraestructura para migrar clústeres locales de Apache Hadoop a Azure HDInsight

En este artículo se proporcionan recomendaciones para administrar la infraestructura de clústeres de Azure HDInsight. Forma parte de una serie de artículos que proporcionan prácticas recomendadas para ayudar a migrar sistemas locales de Apache Hadoop a Azure HDInsight.

## <a name="plan-for-hdinsight-cluster-capacity"></a>Planeamiento de la capacidad de los clústeres de HDInsight

Estas son las principales decisiones que tiene que tomar para planear la capacidad del clúster de HDInsight:

**Región**  
La región de Azure determina dónde se aprovisiona físicamente el clúster. Para minimizar la latencia de lecturas y escrituras, el clúster debe estar en la misma región que los datos.

**Tamaño y ubicación de almacenamiento**  
El almacenamiento predeterminado debe estar en la misma región que el clúster. Para un clúster de 48 nodos, se recomienda tener de 4 a 8 cuentas de almacenamiento. Aunque puede que ya haya suficiente espacio de almacenamiento total, cada cuenta de almacenamiento proporciona ancho de banda de red adicional para los nodos de ejecución. Si tiene varias cuentas de almacenamiento, utilice un nombre aleatorio para cada cuenta de almacenamiento, sin prefijo. El propósito de la nomenclatura aleatoria es reducir la posibilidad de que se produzcan cuellos de botella de almacenamiento (limitación) o errores comunes en todas las cuentas. Para mejorar el rendimiento, utilice un único contenedor por cada cuenta de almacenamiento.

**Tamaño y tipo de máquina virtual (ahora es compatible con la serie G)**  
Cada tipo de clúster tiene un conjunto de tipos de nodos, y cada uno de ellos incluye opciones específicas para el tamaño y el tipo de máquina virtual. El tamaño y el tipo de máquina virtual los determinan la capacidad de procesamiento de la CPU, el tamaño de RAM y la latencia de red. Se puede utilizar una carga de trabajo simulada para determinar el tamaño y el tipo óptimos de máquina virtual para cada tipo de nodo.

**Número de nodos de trabajo**  
Se puede determinar el número inicial de nodos de trabajo con cargas de trabajo simuladas. Más adelante se puede escalar el clúster mediante la adición de más nodos de trabajo para satisfacer las demandas de carga máximas. A continuación, cuando los nodos de trabajo adicionales ya no sean necesarios, se puede revertir el escalado del clúster.

Para más información, consulte el artículo [Planeamiento de la capacidad de los clústeres de HDInsight](../hdinsight-capacity-planning.md).

## <a name="use-recommended-virtual-machine-type-for-cluster"></a>Uso del tipo de máquina virtual recomendado para el clúster

Consulte [Configuración de nodo predeterminada y tamaños de máquina virtual para clústeres](../hdinsight-component-versioning.md#default-node-configuration-and-virtual-machine-sizes-for-clusters) para ver los tipos de máquinas virtuales recomendados para cada tipo de clúster de HDInsight.

## <a name="check-hadoop-components-availability-in-hdinsight"></a>Comprobación de la disponibilidad de componentes de Hadoop en HDInsight

Cada versión de HDInsight es una distribución de nube de un conjunto de componentes del ecosistema Hadoop. Consulte [¿Cuáles son los componentes y versiones de Hadoop disponibles con HDInsight?](../hdinsight-component-versioning.md) para obtener más información sobre todos los componentes de HDInsight y sus versiones actuales.

También puede usar la interfaz de usuario de Apache Ambari o la API REST de Ambari para consultar los componentes y las versiones de Hadoop en HDInsight.

Las aplicaciones o componentes que estaban disponibles en clústeres locales pero no forman parte de los clústeres de HDInsight pueden agregarse a un nodo perimetral o a una máquina virtual de la misma red virtual que el clúster de HDInsight. Una aplicación de Hadoop de terceros que no esté disponible en Azure HDInsight puede instalarse con la opción "Aplicaciones" del clúster de HDInsight. Las aplicaciones de Hadoop personalizadas pueden instalarse en un clúster de HDInsight mediante "acciones de script". En la tabla siguiente se enumeran algunas de las aplicaciones habituales y sus opciones de integración de HDInsight:

|**Aplicación**|**Integración**
|---|---|
|Flujo de aire|Nodo perimetral de IaaS o HDInsight
|Alluxio|IaaS  
|Arcadia|IaaS 
|Atlas|Ninguno (solo HDP)
|Datameer|Nodo perimetral de HDInsight
|Datastax (Cassandra)|IaaS (CosmosDB, una alternativa en Azure)
|DataTorrent|IaaS 
|Detalles|IaaS 
|Ignite|IaaS
|Jethro|IaaS 
|Mapador|IaaS 
|Mongo|IaaS (CosmosDB, una alternativa en Azure)
|NiFi|IaaS 
|Presto|Nodo perimetral de IaaS o HDInsight
|Python 2|PaaS 
|Python 3|PaaS 
|R|PaaS 
|SAS|IaaS 
|Vertica|IaaS (SQLDW, una alternativa en Azure)
|Tableau|IaaS 
|Waterline|Nodo perimetral de HDInsight
|StreamSets|Perímetro de HDInsight 
|Palantir|IaaS 
|Sailpoint|Iaas 

Para más información, vea el artículo [Componentes de Apache Hadoop disponibles con las distintas versiones de HDInsight](../hdinsight-component-versioning.md#apache-components-available-with-different-hdinsight-versions).

## <a name="customize-hdinsight-clusters-using-script-actions"></a>Personalización de clústeres de HDInsight mediante acciones de scripts

HDInsight proporciona un método de configuración del clúster llamado **acción de script**. Una acción de script es el script de Bash que se ejecuta en los nodos de un clúster de HDInsight y puede usarse para instalar componentes adicionales y cambiar la configuración.

Las acciones de script deben almacenarse en un URI accesible desde el clúster de HDInsight. Se pueden usar durante la creación del clúster o después de esta, así como restringir su ejecución a determinados tipos de nodos.

El script se puede mantener o ejecutar una vez. Los scripts persistentes se usan para personalizar nuevos nodos de trabajo agregados al clúster mediante operaciones de escalado. Un script persistente también puede aplicar cambios a otro tipo de nodo, como uno principal, cuando se producen operaciones de escalado.

HDInsight proporciona scripts escritos previamente para instalar los siguientes componentes en clústeres de HDInsight:

- Adición de una cuenta de Azure Storage
- Instalación de Hue
- Instalación de Presto
- Instalación de Solr
- Instalación de Giraph
- Carga previa de las bibliotecas de Hive
- Instalación o actualización de Mono

> [!Note]  
> HDInsight no proporciona compatibilidad directa para los componentes de Hadoop personalizados ni los componentes instalados mediante acciones de script.

Las acciones de script también pueden publicarse en Azure Marketplace como una aplicación de HDInsight.

Para más información, consulte los siguientes artículos.

- [Instalación de aplicaciones de Apache Hadoop de terceros en HDInsight](../hdinsight-apps-install-applications.md)
- [Personalización de clústeres de HDInsight basados en Linux mediante acciones de script](../hdinsight-hadoop-customize-cluster-linux.md)
- [Publicación de una aplicación de HDInsight en Azure Marketplace](../hdinsight-apps-publish-applications.md)

## <a name="customize-hdinsight-configs-using-bootstrap"></a>Personalización de las configuraciones de HDInsight con Bootstrap

Los cambios de configuración de los archivos de configuración, como `core-site.xml`, `hive-site.xml` y `oozie-env.xml`, pueden realizarse mediante Bootstrap. El script siguiente es un ejemplo de uso del cmdlet [New AzHDInsightClusterConfig](https://docs.microsoft.com/powershell/module/az.hdinsight/new-azhdinsightcluster) del [módulo AZ](https://docs.microsoft.com/powershell/azure/new-azureps-module-az) de PowerShell:

```powershell
# hive-site.xml configuration
$hiveConfigValues = @{"hive.metastore.client.socket.timeout"="90"}

$config = New—AzHDInsightClusterConfig '
    | Set—AzHDInsightDefaultStorage
    —StorageAccountName "$defaultStorageAccountName.blob. core.windows.net" `
    —StorageAccountKey "defaultStorageAccountKey " `
    | Add—AzHDInsightConfigValues `
        —HiveSite $hiveConfigValues

New—AzHDInsightCluster `
    —ResourceGroupName $existingResourceGroupName `
    —Cluster-Name $clusterName `
    —Location $location `
    —ClusterSizeInNodes $clusterSizeInNodes `
    —ClusterType Hadoop `
    —OSType Linux `
    —Version "3.6" `
    —HttpCredential $httpCredential `
    —Config $config
```

Para más información, consulte el artículo [Personalización de los clústeres de HDInsight con Bootstrap](../hdinsight-hadoop-customize-cluster-bootstrap.md).  Consulte también, [Administración de clústeres de HDInsight mediante la API REST de Apache Ambari](../hdinsight-hadoop-manage-ambari-rest-api.md).

## <a name="access-client-tools-from-hdinsight-hadoop-cluster-edge-nodes"></a>Acceso a las herramientas de cliente desde los nodos perimetrales del clúster HDInsight Hadoop

Un nodo perimetral vacío es una máquina virtual Linux con las mismas herramientas de cliente instaladas y configuradas que en los nodos principales, pero sin que se ejecuten servicios de Hadoop. El nodo perimetral se puede utilizar para los siguientes fines:

- acceso al clúster
- prueba de aplicaciones cliente
- hospedaje de aplicaciones cliente

Los nodos perimetrales se pueden crear y eliminar a través de Azure Portal y pueden utilizarse durante la creación del clúster o después de esta. Después de haber creado un nodo perimetral, puede conectarse a él mediante SSH y ejecutar herramientas de cliente para acceder al clúster de Hadoop en HDInsight. El punto de conexión SSH del nodo perimetral es `<EdgeNodeName>.<ClusterName>-ssh.azurehdinsight.net:22`.


Para más información, consulte el artículo [Uso de nodos perimetrales en clústeres vacíos en Apache Hadoop en HDInsight](../hdinsight-apps-use-edge-node.md).


## <a name="use-scale-up-and-scale-down-feature-of-clusters"></a>Uso de la característica de escalado vertical y de reducción vertical de los clústeres

HDInsight proporciona elasticidad, lo que ofrece la opción de escalar y reducir verticalmente el número de nodos de trabajo de los clústeres. Esta característica le permite reducir un clúster una vez finalizada la jornada o durante los fines de semana y expandirlo durante períodos de máxima demanda empresarial. Para más información, consulte:

* [Escalado de clústeres de HDInsight ](../hdinsight-scaling-best-practices.md).
* [Escalado de clústeres](../hdinsight-administer-use-portal-linux.md#scale-clusters).

## <a name="use-hdinsight-with-azure-virtual-network"></a>Uso de HDInsight con Azure Virtual Network

Las redes de Azure Virtual Network permiten a muchos tipos de recursos de Azure, como las máquinas virtuales de Azure, comunicarse de forma segura entre ellos, con Internet y con las redes locales, mediante el filtrado y el enrutado del tráfico.

El uso de una instancia de Azure Virtual Network con HDInsight hace posibles los siguientes escenarios:

- Conectarse a HDInsight directamente desde una red local.
- Conectar HDInsight a almacenes de datos en una instancia de Azure Virtual Network.
- Obtener acceso directo a los servicios de Hadoop que no están disponibles públicamente en Internet. Por ejemplo, las API de Kafka o la API de Java de HBase.

HDInsight puede agregarse a una instancia nueva o existente de Azure Virtual Network. Si se agrega HDInsight a una red virtual existente, será necesario actualizar los grupos de seguridad de red y las rutas definidas por el usuario ya establecidos para permitir el acceso sin restricciones a [varias direcciones IP](../hdinsight-management-ip-addresses.md) en el centro de datos de Azure. Además, asegúrese de que no se bloquea el tráfico a los [puertos](../control-network-traffic.md#required-ports) que se usan en los servicios de HDInsight.

> [!Note]  
> HDInsight no es compatible actualmente con la tunelización forzada. La tunelización forzada es una configuración de subred que fuerza el tráfico de salida de Internet a un dispositivo para la inspección y el registro. Quite la tunelización forzada antes de instalar HDInsight en una subred, o bien cree una nueva subred para HDInsight. HDInsight no admite tampoco la restricción de la conectividad de red saliente.

Para más información, consulte los siguientes artículos.

- [¿Qué es Azure Virtual Network?](../../virtual-network/virtual-networks-overview.md)
- [Extender Azure HDInsight mediante una instancia de Azure Virtual Network](../hdinsight-plan-virtual-network-deployment.md)

## <a name="securely-connect-to-azure-services-with-azure-virtual-network-service-endpoints"></a>Conexión de forma segura a servicios de Azure con puntos de conexión de servicio de red virtual de Azure

HDInsight admite [puntos de conexión de servicio de red virtual](../../virtual-network/virtual-network-service-endpoints-overview.md) para conectarse de forma segura a Azure Blob Storage, Azure Data Lake Storage Gen2, Cosmos DB y bases de datos SQL. Al habilitar un punto de conexión de servicio para Azure HDInsight, el tráfico fluye a través de una ruta protegida desde dentro del centro de datos de Azure. Con este nivel mejorado de seguridad en el nivel de red, puede bloquear las cuentas de almacenamiento de macrodatos en sus redes virtuales especificadas y seguir usando clústeres de HDInsight sin problemas para obtener acceso a los datos y procesarlos.

Para más información, consulte los siguientes artículos.

- [Puntos de conexión de servicio de red virtual](../../virtual-network/virtual-network-service-endpoints-overview.md)
- [Enhance HDInsight security with service endpoints](https://azure.microsoft.com/blog/enhance-hdinsight-security-with-service-endpoints/) (Mejora de la seguridad de HDInsight con puntos de conexión de servicio)

## <a name="connect-hdinsight-to-the-on-premises-network"></a>Conexión de HDInsight a la red local

HDInsight puede conectarse a la red local mediante instancias de Azure Virtual Network y una instancia de VPN Gateway. Siga estos pasos para establecer la conectividad:

- Use HDInsight en una instancia de Azure Virtual Network que tenga conectividad con la red local.
- Configure la resolución de nombres DNS entre la red virtual y la red local.
- Configure grupos de seguridad de red o rutas definidas por el usuario para controlar el tráfico de red.

Para obtener más información, consulte el artículo [Conexión de HDInsight a la red local](../connect-on-premises-network.md).

## <a name="next-steps"></a>Pasos siguientes

Lea el siguiente artículo de esta serie: [Procedimientos recomendados de almacenamiento para la migración local a Azure HDInsight Hadoop](apache-hadoop-on-premises-migration-best-practices-storage.md).
