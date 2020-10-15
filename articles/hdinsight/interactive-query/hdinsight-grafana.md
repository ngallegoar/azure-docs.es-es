---
title: Uso de Grafana en Azure HDInsight
description: Descubra cómo acceder al panel de Grafana con clústeres de Apache Hadoop en Azure HDInsight
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: how-to
ms.date: 12/27/2019
ms.openlocfilehash: 7750544367044cab2a0243577c200025bb9ff693
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "86083039"
---
# <a name="access-grafana-in-azure-hdinsight"></a>Acceso a Grafana en Azure HDInsight

[Grafana](https://grafana.com/) es un software popular y de código abierto para crear gráficos y paneles. Grafana está lleno de características; no solo permite a los usuarios crear paneles que pueden personalizar y compartir, sino que también ofrece paneles con plantillas o scripts, integración de LDAP, varios orígenes de datos y mucho más.

Actualmente, en Azure HDInsight, Grafana es compatible con los tipos de clúster de Spark, HBase, Kafka e Interactive Query. No es compatible con clústeres que tengan habilitado Enterprise Security Pack.

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

## <a name="create-an-apache-hadoop-cluster"></a>Creación de un clúster de Apache Hadoop

Consulte [Creación de clústeres de Apache Hadoop mediante Azure Portal](../hdinsight-hadoop-create-linux-clusters-portal.md). En **Tipo de clúster**, seleccione **Spark**, **Kafka**, **HBase** o **Interactive Query**.

## <a name="access-the-grafana-dashboard"></a>Acceso al panel de Grafana

1. En un explorador web, vaya a `https://CLUSTERNAME.azurehdinsight.net/grafana/`, donde CLUSTERNAME es el nombre del clúster.

1. Escriba las credenciales de usuario del clúster de Hadoop.

1. El panel de Grafana aparece y tiene un aspecto similar a este ejemplo:

    ![Panel web de HDInsight Grafana](./media/hdinsight-grafana/hdinsight-grafana-dashboard.png "Panel de HDInsight Grafana")

## <a name="clean-up-resources"></a>Limpieza de recursos

Si no va a seguir usando esta aplicación, elimine el clúster que creó mediante los siguientes pasos:

1. Inicie sesión en [Azure Portal](https://portal.azure.com/).

1. En el cuadro **Búsqueda** en la parte superior, escriba **HDInsight**.

1. Seleccione **Clústeres de HDInsight** en **Servicios**.

1. En la lista de clústeres de HDInsight que aparece, seleccione el signo **...** situado junto al clúster que ha creado.

1. Seleccione **Eliminar**. Seleccione **Sí**.

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre el análisis de datos con HDInsight, consulte los siguientes artículos:

* [Uso de Apache Hive con HDInsight](../hadoop/hdinsight-use-hive.md).

* [Uso de MapReduce con HDInsight](../hadoop/hdinsight-use-mapreduce.md).

* [Introducción al uso de herramientas de Hadoop de HDInsight para Visual Studio](../hadoop/apache-hadoop-visual-studio-tools-get-started.md).
