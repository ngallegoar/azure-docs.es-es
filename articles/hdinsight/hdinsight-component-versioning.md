---
title: 'Componentes y versiones de Apache Hadoop: Azure HDInsight'
description: Conozca los componentes y las versiones de Apache Hadoop disponibles en Azure HDInsight.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive,hdiseo17may2017,seoapr2020
ms.date: 04/09/2020
ms.openlocfilehash: 87c3e2439d1b4bef4a58663e3ea06d8bb7cb9b19
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2020
ms.locfileid: "82192542"
---
# <a name="what-are-the-apache-hadoop-components-and-versions-available-with-hdinsight"></a>¿Cuáles son los componentes y versiones de Apache Hadoop disponibles con HDInsight?

Conozca los componentes y las versiones del entorno [Apache Hadoop](https://hadoop.apache.org/) que se incluyen en Microsoft Azure HDInsight, así como Enterprise Security Package. Además, obtenga información sobre cómo comprobar las versiones de componentes de Hadoop en HDInsight.

## <a name="apache-hadoop-components-available-with-different-hdinsight-versions"></a>Componentes de Apache Hadoop disponibles con las distintas versiones de HDInsight

HDInsight de Azure es compatible con varias versiones de clústeres de Hadoop que se pueden implementar en cualquier momento. Desde el 4 de abril de 2017, la versión de clúster predeterminada que se utiliza en Azure HDInsight es la 3.6.

En la tabla siguiente se enumeran las versiones de componente asociadas a las versiones de clúster de HDInsight:

> [!NOTE]  
> La versión predeterminada del servicio HDInsight puede cambiar sin previo aviso. Si tiene una dependencia de la versión, especifique la versión de HDInsight al crear clústeres con el SDK de .NET con Azure PowerShell y la CLI de Azure clásica.

| Componente              | HDInsight 4.0 | HDInsight 3.6 (predeterminado)     |
|------------------------|---------------|-----------------------------|
| Apache Hadoop y YARN | 3.1.1         | 2.7.3                       |
| Apache Tez             | 0.9.1         | 0.7.0                       |
| Apache Pig             | 0.16.0        | 0.16.0                      |
| Apache Hive            | 3.1.0         | 1.2.1 (2.1.0 en ESP Interactive Query) |
| Apache Tez Hive2       | -             | 0.8.4                       |
| Apache Ranger          | 1.1.0         | 0.7.0                       |
| HBase Apache           | 2.0.2         | 1.1.2                       |
| Apache Sqoop           | 1.4.7         | 1.4.6                       |
| Apache Oozie           | 4.3.1         | 4.2.0                       |
| Apache Zookeeper       | 3.4.6         | 3.4.6                       |
| Apache Storm           | -             | 1.1.0                       |
| Apache Mahout          | -             | 0.9.0+                      |
| Apache Phoenix         | 5             | 4.7.0                       |
| Spark de Apache           | 2.3.1, 2.4    | 2.3.0, 2.2.0 y 2.1.0         |
| Apache Livy            | 0.5           | 0.4, 0.4, 0.3               |
| Apache Kafka           | 1.1.1, 2.1    | 1.1, 1.0 * (vea la nota siguiente) |
| Apache Ambari          | 2.7.0         | 2.6.0                       |
| Apache Zeppelin        | 0.8.0         | 0.7.3                       |
| Mono                   | 4.2.1         | 4.2.1                       |

> [!NOTE]
> Debido a consideraciones de rendimiento del sistema, la compatibilidad con la versión 0.10 de Kafka expiró en marzo de 2019.

## <a name="check-for-current-hadoop-component-version-information"></a>Comprobación de la información de la versión de los componentes actuales de Hadoop

Las versiones de los componentes del entorno de Hadoop asociadas a las versiones de los clústeres de HDInsight pueden cambiar en futuras actualizaciones de HDInsight. Para comprobar los componentes de Hadoop disponibles y comprobar las versiones que se están usando para un clúster, use la API REST de Ambari. El comando **GetComponentInformation** recupera información sobre componentes de servicio. Para más información, consulte la [documentación de Apache Ambari](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md).

### <a name="release-notes"></a>Notas de la versión

Consulte [Notas de la versión de HDInsight](hdinsight-release-notes.md) para conocer otras notas de las últimas versiones de HDInsight.

## <a name="supported-hdinsight-versions"></a>Versiones compatibles de HDInsight

### <a name="support-expiration-and-retirement-for-hdinsight-versions"></a>Expiración de la compatibilidad y retirada de versiones de HDInsight

**Expiración del soporte técnico** significa que Microsoft ya no proporcionará soporte técnico para la versión especificada de HDInsight y dejará de estar disponible mediante Azure Portal para la creación del clúster. Sin embargo, estas versiones todavía se pueden crear mediante la CLI de Azure o los diversos SDK.

La **retirada** de una versión de HDInsight significa que los clústeres existentes seguirán ejecutándose tal cual. Sin embargo, los nuevos clústeres de esta versión no se pueden crear por ningún medio (ni con la CLI ni con los SDK). Otras características del plano de control (como el escalado manual y la escalabilidad automática) puede que tampoco funcionen después de la retirada de la versión. El soporte técnico no está disponible para las versiones retiradas.

Las tablas siguientes enumeran las versiones de HDInsight. Si se conocen, también se proporcionan las fechas de expiración y retirada.

### <a name="available-versions"></a>Versiones disponibles

En la tabla siguiente se enumeran las versiones de HDInsight que están disponibles en Azure Portal y otros métodos de implementación como PowerShell y SDK de .NET.

| Versión de HDInsight | SISTEMA OPERATIVO DE LA MÁQUINA VIRTUAL | Fecha de la versión | Fecha de expiración del soporte técnico | Fecha de retirada | Alta disponibilidad |  Disponibilidad en Azure Portal |
| --- | --- | --- | --- | --- | --- | --- |
| HDInsight 4.0 |Ubuntu 16.0.4 LTS |24 de septiembre de 2018 | | |Sí |Sí |
| HDInsight 3.6 |Ubuntu 16.0.4 LTS |4 de abril de 2017 | 31 de diciembre de 2020 |31 de diciembre de 2020 |Sí |Sí |

La compatibilidad con Spark 2.1, 2.2 y Kafka 1.0 expirará el 30 de junio de 2020.

> [!NOTE]  
> Cuando expira la compatibilidad de una versión, puede dejar de estar disponible en Microsoft Azure Portal. Sin embargo, las versiones del clúster seguirán estando disponibles mediante el parámetro `Version` del comando [New-AzHDInsightCluster](https://docs.microsoft.com/powershell/module/az.hdinsight/new-azhdinsightcluster) de Windows PowerShell y en el SDK de .NET hasta la fecha de retirada de la versión.

### <a name="retired-versions"></a>Versiones retiradas

En la tabla siguiente se enumeran las versiones de HDInsight que **no** están disponibles en Azure Portal.

| Versión de HDInsight | Versión de HDP | SISTEMA OPERATIVO DE LA MÁQUINA VIRTUAL | Fecha de la versión | Fecha de expiración del soporte técnico | Fecha de retirada | Alta disponibilidad |  Disponibilidad en Azure Portal |
| --- | --- | --- | --- | --- | --- | --- | --- |
| HDInsight 3.5 |HDP 2.5 |Ubuntu 16.0.4 LTS |30 de septiembre de 2016 |5 de septiembre de 2017 |28 de junio de 2018 |Sí |No |
| HDInsight 3.4 |HDP 2.4 |Ubuntu 14.0.4 LTS |29 de marzo de 2016 |29 de diciembre de 2016 |9 de enero de 2018 |Sí |No |
| HDInsight 3.3 |HDP 2.3 |Windows Server 2012 R2 |2 de diciembre de 2015 |27 de junio de 2016 |31 de julio de 2018 |Sí |No |
| HDInsight 3.3 |HDP 2.3 |Ubuntu 14.0.4 LTS |2 de diciembre de 2015 |27 de junio de 2016 |31 de julio de 2017 |Sí |No |
| HDInsight 3.2 |HDP 2.2 |Ubuntu 12.04 LTS o Windows Server 2012 R2 |18 de febrero de 2015 |1 de marzo de 2016 |1 de abril de 2017 |Sí |No |
| HDInsight 3.1 |HDP 2,1 |Windows Server 2012 R2 |24 de junio de 2014 |18 de mayo de 2015 |30 de junio de 2016 |Sí |No |
| HDInsight 3.0 |HDP 2,0 |Windows Server 2012 R2 |11 de febrero de 2014 |17 de septiembre de 2014 |30 de junio de 2015 |Sí |No |
| HDInsight 2.1 |HDP 1,3 |Windows Server 2012 R2 |28 de octubre de 2013 |12 de mayo de 2014 |31 de mayo de 2015 |Sí |No |
| HDInsight 1.6 |HDP 1.1 | |28 de octubre de 2013 |26 de abril de 2014 |31 de mayo de 2015 |No |No |

> [!NOTE]  
> Los clústeres de alta disponibilidad con dos nodos principales se implementan de forma predeterminada para los clústeres de HDInsight 2.1 y versiones posteriores. No están disponibles para los clústeres de HDInsight 1.6.

## <a name="enterprise-security-package-for-hdinsight"></a>Paquete de seguridad de la empresa para HDInsight

Enterprise Security es un paquete opcional que puede agregar en su clúster de HDInsight como parte del flujo de trabajo de creación del clúster. El paquete de seguridad de la empresa admite:

- Integración con Active Directory para autenticación.

    En el pasado, se creaban clústeres de HDInsight con el usuario administrador local y el usuario de SSH local. El usuario administrador local podía acceder a todos los archivos, carpetas, tablas y columnas.  Con Enterprise Security Package, puede habilitar el control de acceso basado en rol mediante la integración de HDInsight con Active Directory, lo que incluye Active Directory local, Azure Active Directory Domain Services o Active Directory en la máquina virtual de IaaS. El administrador de dominio del clúster puede conceder a los usuarios permiso para utilizar sus propias credenciales corporativas (dominio) de nombre de usuario y contraseña.

    Para más información, consulte:

    - [Introducción a la seguridad de Apache Hadoop con clústeres de HDInsight unidos a un dominio](./domain-joined/hdinsight-security-overview.md)
    - [Planeamiento de clústeres de Apache Hadoop unidos a un dominio de Azure en HDInsight](./domain-joined/apache-domain-joined-architecture.md)
    - [Configuración de un entorno de espacio aislado unido a un dominio](./domain-joined/apache-domain-joined-configure.md)
    - [Configurar clústeres de HDInsight unidos a un dominio con Azure Active Directory Domain Services](./domain-joined/apache-domain-joined-configure-using-azure-adds.md)

- Autorización de datos

  - Integración con Apache Ranger para la autorización de Hive, Spark SQL y colas de Yarn.
  - Puede establecer un control de acceso en archivos y carpetas.

    Para más información, consulte:

  - [Configuración de directivas de Apache Hive en HDInsight unido a un dominio](./domain-joined/apache-domain-joined-run-hive.md)

- Vea los registros de auditoría para supervisar accesos y las directivas configuradas.

### <a name="supported-cluster-types"></a>Tipos de clúster compatibles

Actualmente, solo los siguientes tipos de clúster admiten el paquete de seguridad de la empresa:

- Hadoop (solo HDInsight 3.6)
- Spark
- Kafka
- HBase
- Interactive Query

### <a name="support-for-azure-data-lake-storage"></a>Compatibilidad con Azure Data Lake Storage

Enterprise Security Package permite usar Azure Data Lake Storage como almacenamiento principal y como almacenamiento complementario.

### <a name="pricing-and-service-level-agreement-sla"></a>Precios y contrato de nivel de servicio (SLA)

Para obtener información sobre los precios y el Acuerdo de Nivel de Servicio del paquete de seguridad de la empresa, consulte [Precios de HDInsight](https://azure.microsoft.com/pricing/details/hdinsight/).

## <a name="service-level-agreement-for-hdinsight-cluster-versions"></a>Contrato de nivel de servicio para las versiones de clúster de HDInsight

El contrato de nivel de servicio (SLA) de define en términos de _período de soporte técnico_. El período de soporte técnico es el período de tiempo que `Microsoft Customer Service and Support` admite una versión de HDInsight. Si la versión tiene una _fecha de expiración de soporte técnico_ que ya ha pasado, el clúster de HDInsight está fuera del período de soporte técnico. La expiración del soporte técnico de la versión X de HDInsight (después de que haya disponible una versión más reciente X + 1) es la más reciente:  

- Fórmula 1: agregue 180 días a la fecha en la que se lanzó la versión X del clúster de HDInsight.
- Fórmula 2: agregue 90 días a la fecha en la que la versión del clúster de HDInsight x+1 se encuentra disponible en Azure Portal.

La _fecha de retirada_ es la fecha tras la cual no se puede crear la versión del clúster en HDInsight. A partir del 31 de julio de 2017, no se puede cambiar el tamaño de un clúster de HDInsight después de su fecha de retirada.

## <a name="default-node-configuration-and-virtual-machine-sizes-for-clusters"></a>Configuración de nodo predeterminada y tamaños de máquina virtual para clústeres

Para obtener más información sobre qué SKU de máquina virtual se deben seleccionar para su clúster, consulte [Detalles de configuración de clúster de Azure HDInsight](hdinsight-supported-node-configuration.md).

## <a name="next-steps"></a>Pasos siguientes

- [Configuración de clúster para Apache Hadoop, Spark, etc. en HDInsight](hdinsight-hadoop-provision-linux-clusters.md)
- [Trabajo en Apache Hadoop en HDInsight desde un equipo Windows](hdinsight-hadoop-windows-tools.md)
- [Notas de la versión de Hortonworks asociadas a las versiones de HDInsight](./hortonworks-release-notes.md)
