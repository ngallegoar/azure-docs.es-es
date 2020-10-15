---
title: 'Análisis en Azure HDInsight Hadoop con Hive: proceso de ciencia de datos en equipo'
description: Ejemplos del proceso de ciencia de datos en equipo donde se examina el uso de Hive en Azure HDInsight Hadoop para la realización de análisis predictivo.
services: machine-learning
author: marktab
manager: marktab
editor: marktab
ms.service: machine-learning
ms.subservice: team-data-science-process
ms.topic: article
ms.date: 01/10/2020
ms.author: tdsp
ms.custom: seodec18, previous-author=deguhath, previous-ms.author=deguhath
ms.openlocfilehash: f72ea6ed5f0eec076d181ef56c99c4f1308a7741
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "75864169"
---
# <a name="hdinsight-hadoop-data-science-walkthroughs-using-hive-on-azure"></a>Tutoriales de ciencia de datos de HDInsight Hadoop con Hive en Azure 

En estos tutoriales se usa Hive con un clúster de HDInsight Hadoop para realizar análisis predictivo. En ellos se siguen los pasos descritos en el proceso de ciencia de datos en equipo. Para obtener información general sobre el proceso de ciencia de datos en equipo, consulte [Proceso de ciencia de datos](overview.md). Para ver una introducción a Azure HDInsight, consulte [Introducción a Azure HDInsight, la pila de tecnología de Hadoop y los clústeres de Hadoop](../../hdinsight/hadoop/apache-hadoop-introduction.md).

Los otros tutoriales de ciencia de datos donde se ejecuta el Proceso de ciencia de datos en equipo se agrupan por la **plataforma** que usan. Consulte [Walkthroughs executing the Team Data Science Process](walkthroughs.md) (Tutoriales para ejecutar el Proceso de ciencia de datos en equipo) para ver ejemplos.


## <a name="predict-taxi-tips-using-hive-with-hdinsight-hadoop"></a>Predicción de propinas para taxis mediante Hive con HDInsight Hadoop

En el tutorial [Uso de clústeres de HDInsight Hadoop](hive-walkthrough.md) se usan datos de taxis de New York para predecir: 

- Si se da una propina 
- La distribución de los importes de propina

El escenario se implementa mediante Hive con un [clúster de Azure HDInsight Hadoop](https://azure.microsoft.com/services/hdinsight/). Aprenderá a almacenar, explorar y realizar la ingeniería de características con un conjunto de datos de carreras y tarifas de taxis de Nueva York. También usará Azure Machine Learning para crear e implementar los modelos.

## <a name="predict-advertisement-clicks-using-hive-with-hdinsight-hadoop"></a>Predicción de clics de anuncio mediante Hive con HDInsight Hadoop

En el tutorial [Uso de un clúster de Azure HDInsight Hadoop en un conjunto de datos de 1 TB](hive-criteo-walkthrough.md) se usa un conjunto de datos de clics de [Criteo](https://labs.criteo.com/downloads/download-terabyte-click-logs/), que está disponible públicamente, para predecir si se da una propina y los importes esperados. El escenario se implementa mediante Hive con un [clúster de Azure HDInsight Hadoop ](https://azure.microsoft.com/services/hdinsight/) para almacenar, explorar, realizar la ingeniería de características y reducir el tamaño de los datos. Se emplea Azure Machine Learning para crear, entrenar y puntuar un modelo de clasificación binario que predice si un usuario hace clic en un anuncio. Para concluir el tutorial, se muestra cómo publicar uno de estos modelos como un servicio web.


## <a name="next-steps"></a>Pasos siguientes

Para obtener una explicación de los componentes clave que conforman el proceso de ciencia de datos en equipo, consulte [Información general del proceso de ciencia de datos en equipo](overview.md).

Para obtener una explicación del ciclo de vida del proceso de ciencia de datos en equipo que puede usar para estructurar los proyectos de ciencia de datos, consulte [Team Data Science Process lifecycle](lifecycle.md) (Ciclo de vida del proceso de ciencia de datos en equipo). El ciclo de vida describe el proceso, de principio a fin, que suelen seguir los proyectos al ejecutarlos. 

