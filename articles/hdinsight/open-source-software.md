---
title: Compatibilidad de software de código abierto en Azure HDInsight
description: Microsoft Azure proporciona un nivel general de soporte técnico para las tecnologías de código abierto.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: how-to
ms.custom: seoapr2020
ms.date: 04/21/2020
ms.openlocfilehash: f1e1518cce060374f14f1db7d5f4e594e3de8f85
ms.sourcegitcommit: 124f7f699b6a43314e63af0101cd788db995d1cb
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/08/2020
ms.locfileid: "86086269"
---
# <a name="open-source-software-support-in-azure-hdinsight"></a>Compatibilidad de software de código abierto en Azure HDInsight

El servicio Microsoft Azure HDInsight usa un entorno de tecnologías de código abierto formado en torno a Apache Hadoop. Microsoft Azure proporciona un nivel general de soporte técnico para las tecnologías de código abierto. Para más información, vea la sección de **ámbito de soporte técnico** de [Preguntas más frecuentes de soporte técnico de Azure](https://azure.microsoft.com/support/faq/). Además, el servicio HDInsight ofrece un nivel adicional de soporte técnico para los componentes incorporados.

## <a name="components"></a>Componentes

Hay dos tipos de componentes de código abierto disponibles en el servicio de HDInsight:

### <a name="built-in-components"></a>Componentes integrados

Estos componentes están instalados previamente en clústeres de HDInsight y proporcionan la funcionalidad básica del clúster. Los siguientes componentes pertenecen a esta categoría:

* Administrador de recursos de [Apache Hadoop YARN](https://hadoop.apache.org/docs/current/hadoop-yarn/hadoop-yarn-site/YARN.html).
* El lenguaje de consulta de Hive [HiveQL](https://cwiki.apache.org/confluence/display/Hive/LanguageManual).
* [Apache Mahout](https://mahout.apache.org/).

Hay una lista completa de componentes del clúster disponible en [¿Cuáles son los componentes y versiones de Apache Hadoop disponibles con HDInsight?](hdinsight-component-versioning.md).

### <a name="custom-components"></a>Componentes personalizados

Como usuario del clúster, puede instalar o usar en la carga de trabajo cualquier componente que esté disponible en la comunidad o que haya creado personalmente.

> [!WARNING]  
> Los componentes proporcionados con el clúster de HDInsight son totalmente compatibles. Soporte técnico de Microsoft ayuda a aislar y a solucionar problemas relacionados con estos componentes.
>
> Los componentes personalizados reciben soporte técnico comercialmente razonable para ayudarle a solucionar el problema. El soporte técnico de Microsoft podría solucionar el problema. También podría sugerirle que consulte los canales disponibles para las tecnologías de código abierto donde se concentran los conocimientos más amplios sobre cada tecnología. Hay diversos sitios de la comunidad que se pueden usar, Algunos ejemplos son [Página de preguntas y respuestas de Microsoft sobre HDInsight](https://docs.microsoft.com/answers/topics/azure-hdinsight.html) y [Stack Overflow](https://stackoverflow.com).
>
> Los proyectos de Apache también tienen sitios de proyecto en el [sitio web de Apache](https://apache.org). Un ejemplo es [Hadoop](https://hadoop.apache.org/).

## <a name="component-usage"></a>Uso de componentes

El servicio HDInsight proporciona varias maneras de utilizar los componentes personalizados. Se aplica el mismo nivel de soporte técnico independientemente de cómo se use un componente o cómo se instale en el clúster. La siguiente tabla describe las formas más comunes de usar los componentes personalizados en clústeres de HDInsight:

|Uso |Descripción |
|---|---|
|Envío de trabajo|Se pueden enviar al clúster trabajos de Hadoop o de otros tipos que ejecuten o usen componentes personalizados.|
|Personalización del clúster|Durante la creación del clúster puede especificar una configuración adicional y componentes personalizados que se instalan en los nodos del clúster.|
|Ejemplos|Para los componentes personalizados más populares, Microsoft y otros pueden proporcionar ejemplos de cómo estos componentes se pueden utilizar en los clústeres de HDInsight. Estas muestras se proporcionan sin soporte técnico.|

## <a name="next-steps"></a>Pasos siguientes

* [Personalización de los clústeres de Azure HDInsight mediante acciones de script](./hdinsight-hadoop-customize-cluster-linux.md)
* [Desarrollo de la acción de script con HDInsight](hdinsight-hadoop-script-actions-linux.md)
* [Administración segura del entorno de Python en Azure HDInsight mediante la acción de scripts](./spark/apache-spark-python-package-installation.md)
