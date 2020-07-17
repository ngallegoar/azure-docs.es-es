---
title: Migración del clúster a una versión más reciente
titleSuffix: Azure HDInsight
description: Conozca las directrices para migrar el clúster de Azure HDInsight a una versión más reciente.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: how-to
ms.custom: hdinsightactive
ms.date: 01/31/2020
ms.openlocfilehash: f3bfd430023330d3a399a0a760fd353b6ee60941
ms.sourcegitcommit: 124f7f699b6a43314e63af0101cd788db995d1cb
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/08/2020
ms.locfileid: "86085895"
---
# <a name="migrate-hdinsight-cluster-to-a-newer-version"></a>Migración del clúster de HDInsight a una versión más reciente

Para aprovechar las ventajas de las características más recientes de HDInsight, se recomienda que los clústeres de HDInsight se migren regularmente a la versión más reciente. HDInsight no admite actualizaciones locales en las que un clúster existente se actualiza a una versión más reciente del componente. Debe crear un nuevo clúster con la versión de plataforma y componente deseada y, a continuación, migrar las aplicaciones para que usen el nuevo clúster. Siga las directrices que aparecen a continuación para migrar las versiones del clúster de HDInsight.

> [!NOTE]  
> Para más información sobre las versiones admitidas de HDInsight, consulte [Versiones de los componentes de HDInsight](hdinsight-component-versioning.md#supported-hdinsight-versions).

## <a name="migration-tasks"></a>Tareas de migración

El flujo de trabajo para actualizar el clúster de HDInsight es el siguiente.
![Diagrama de flujo de trabajo de actualización de HDInsight](./media/hdinsight-upgrade-cluster/upgrade-workflow-diagram.png)

1. Lea cada sección de este documento para entender los cambios que pueden ser necesarios al actualizar el clúster de HDInsight.
2. Cree un clúster como entorno de control de calidad o de pruebas. Para más información sobre cómo crear un clúster, consulte [Más información sobre cómo crear clústeres de HDInsight basados en Linux](hdinsight-hadoop-provision-linux-clusters.md)
3. Copie los trabajos, los orígenes de datos y los receptores existentes en el nuevo entorno.
4. Realice pruebas de validación para asegurarse de que los trabajos funcionan como se esperaba en el nuevo clúster.

Cuando haya comprobado que todo funciona según lo esperado, programe el tiempo de inactividad para la migración. Durante este tiempo de inactividad, realice las acciones siguientes:

1. Haga copia de seguridad de todos los datos transitorios almacenados localmente en los nodos del clúster. Por ejemplo, si tiene datos que se almacenan directamente en un nodo principal.
1. [Elimine el clúster existente](./hdinsight-delete-cluster.md).
1. Cree un clúster en la misma subred de red virtual con la versión de HDI más reciente (o compatible) con el mismo almacén de datos predeterminado que usaba el clúster anterior. Esto permitirá que el nuevo clúster siga trabajando con los datos de producción existentes.
1. Importe los datos transitorios cuya copia de seguridad realizó.
1. Inicie trabajos o continúe el procesamiento con el nuevo clúster.

## <a name="workload-specific-guidance"></a>Orientación específica de la carga de trabajo

En los siguientes documentos se proporcionan instrucciones sobre cómo migrar cargas de trabajo específicas:

* [Migración de HBase](./hbase/apache-hbase-migrate-new-version.md)
* [Migración de Kafka](./kafka/migrate-versions.md)
* [Migración de Hive/Interactive Query](./interactive-query/apache-hive-migrate-workloads.md)

## <a name="backup-and-restore"></a>Copia de seguridad y restauración

Para más información sobre la copia de seguridad y restauración de bases de datos, consulte [Recuperación de una base de datos de Azure SQL Database mediante copias de seguridad de datos automatizadas](../azure-sql/database/recovery-using-backups.md).

## <a name="next-steps"></a>Pasos siguientes

* [Más información sobre cómo crear clústeres de HDInsight basado en Linux](hdinsight-hadoop-provision-linux-clusters.md)
* [Conexión a HDInsight mediante SSH](hdinsight-hadoop-linux-use-ssh-unix.md)
* [Administración de un clúster basado en Linux mediante Apache Ambari](hdinsight-hadoop-manage-ambari.md)
* [Notas de la versión de HDInsight](./hdinsight-version-release.md)
