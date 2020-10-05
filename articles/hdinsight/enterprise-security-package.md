---
title: Enterprise Security Package para Azure HDInsight
description: Conozca los componentes y las versiones de Enterprise Security Package en Azure HDInsight.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 05/08/2020
ms.openlocfilehash: 91fa6a8da555d0b0cc79b262a83306c1f72aa68a
ms.sourcegitcommit: d0541eccc35549db6381fa762cd17bc8e72b3423
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/09/2020
ms.locfileid: "89567102"
---
# <a name="enterprise-security-package-for-azure-hdinsight"></a>Enterprise Security Package para Azure HDInsight

Enterprise Security es un paquete opcional que puede agregar en su clúster de HDInsight como parte del flujo de trabajo de creación del clúster. El paquete de seguridad de la empresa admite:

* Integración con Active Directory para autenticación.

    En el pasado, se creaban clústeres de HDInsight con el usuario administrador local y el usuario de SSH local. El usuario administrador local podía acceder a todos los archivos, carpetas, tablas y columnas.  Con Enterprise Security Package, puede habilitar el control de acceso basado en rol mediante la integración de HDInsight con Azure Active Directory Domain Services.

    Para más información, consulte:

    * [Introducción a la seguridad de Apache Hadoop con clústeres de HDInsight unidos a un dominio](./domain-joined/hdinsight-security-overview.md)

    * [Planeamiento de clústeres de Apache Hadoop unidos a un dominio de Azure en HDInsight](./domain-joined/apache-domain-joined-architecture.md)

    * [Configuración de un entorno de espacio aislado unido a un dominio](./domain-joined/apache-domain-joined-configure.md)

    * [Configurar clústeres de HDInsight unidos a un dominio con Azure Active Directory Domain Services](./domain-joined/apache-domain-joined-configure-using-azure-adds.md)

* Autorización de datos

  * Integración con Apache Ranger para la autorización de Hive, Spark SQL y colas de Yarn.
  * Puede establecer un control de acceso en archivos y carpetas.

    Para más información, consulte [Configuración de directivas de Apache Hive en HDInsight unido a un dominio](./domain-joined/apache-domain-joined-run-hive.md).

* Vea los registros de auditoría para supervisar accesos y las directivas configuradas.

## <a name="supported-cluster-types"></a>Tipos de clúster compatibles

Actualmente, solo los siguientes tipos de clúster admiten el paquete de seguridad de la empresa:

* Hadoop (solo HDInsight 3.6)
* Spark
* Kafka
* HBase
* Interactive Query

## <a name="support-for-azure-data-lake-storage"></a>Compatibilidad con Azure Data Lake Storage

Enterprise Security Package permite usar Azure Data Lake Storage como almacenamiento principal y como almacenamiento complementario.

## <a name="pricing-and-service-level-agreement-sla"></a>Precios y contrato de nivel de servicio (SLA)

Para obtener información sobre los precios y el Acuerdo de Nivel de Servicio del paquete de seguridad de la empresa, consulte [Precios de HDInsight](https://azure.microsoft.com/pricing/details/hdinsight/).

## <a name="next-steps"></a>Pasos siguientes

* [Configuración de clúster para Apache Hadoop, Spark, etc. en HDInsight](hdinsight-hadoop-provision-linux-clusters.md)
* [Trabajo en Apache Hadoop en HDInsight desde un equipo Windows](hdinsight-hadoop-windows-tools.md)
* [Notas de la versión de Hortonworks asociadas a las versiones de HDInsight](./hortonworks-release-notes.md)
* [Componentes de Apache en HDInsight](./hdinsight-component-versioning.md)
