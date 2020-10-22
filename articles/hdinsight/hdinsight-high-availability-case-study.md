---
title: Caso práctico de arquitectura de solución de alta disponibilidad de Azure HDInsight
description: Este artículo es un caso práctico ficticio de la posible arquitectura de una solución de alta disponibilidad de HDInsight de Azure.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
keywords: hadoop alta disponibilidad
ms.service: hdinsight
ms.topic: conceptual
ms.date: 10/08/2020
ms.openlocfilehash: 4b98b03c2d7eb4a0403b4595c1376656ed42511b
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "91855045"
---
# <a name="azure-hdinsight-highly-available-solution-architecture-case-study"></a>Caso práctico de arquitectura de solución de alta disponibilidad de Azure HDInsight

Los mecanismos de replicación de Azure HDInsight se pueden integrar en la arquitectura de una solución de alta disponibilidad. En este artículo, se usa un caso práctico ficticio de Contoso Retail para explicar los posibles enfoques de la recuperación ante desastres de alta disponibilidad, las consideraciones relativas a los costos y los diseños correspondientes.

Las recomendaciones de la recuperación ante desastres de alta disponibilidad pueden tener muchas permutaciones y combinaciones. A estas soluciones se debe llegar tras deliberar los pros y los contras de cada opción. En este artículo solo se describe una posible solución.

## <a name="customer-architecture"></a>Arquitectura del cliente

En la imagen siguiente se muestra la arquitectura principal de Contoso Retail. La arquitectura consta de una carga de trabajo de streaming, una carga de trabajo por lotes, una capa de servicio, una capa de consumo, una capa de almacenamiento y un control de versiones.

:::image type="content" source="./media/hdinsight-high-availability-case-study/contoso-architecture.png" alt-text="Arquitectura de Contoso Retail":::

### <a name="streaming-workload"></a>Carga de trabajo de streaming

Los dispositivos y sensores producen datos en HDInsight Kafka, que constituye el marco de mensajería. Un consumidor de HDInsight Spark realiza la lectura a partir de los temas de Kafka. Spark transforma los mensajes entrantes y los escribe en la capa de servicio de un clúster de HBase de HDInsight.

### <a name="batch-workload"></a>Carga de trabajo por lotes

Un clúster de Hadoop de HDInsight que ejecuta Hive y MapReduce ingiere datos procedentes de sistemas transaccionales locales. Los datos sin procesar transformados por Hive y MapReduce se almacenan en tablas de Hive, en una partición lógica del lago de datos respaldada por Azure Data Lake Storage Gen2. Los datos almacenados en tablas de Hive también se ponen a disposición de Spark SQL, que realiza transformaciones por lotes antes de almacenar los datos mantenidos en HBase para su servicio.

### <a name="serving-layer"></a>Capa de servicio

Un clúster de HBase de HDInsight con Apache Phoenix se usa para servir los datos a las aplicaciones web y los paneles de visualización. Un clúster de LLAP de HDInsight se usa para cumplir los requisitos en materia de elaboración de informes internos.

### <a name="consumption-layer"></a>Capa de consumo

Una capa de API Apps y API Management de Azure respalda una página web orientada al público. Power BI se encarga de cumplir los requisitos relativos a la elaboración de informes internos.

### <a name="storage-layer"></a>Capa de almacenamiento

Azure Data Lake Storage Gen2 se usa con particiones lógicas como lago de datos empresarial. Los metastores de HDInsight están respaldados por Azure SQL DB.

### <a name="version-control-system"></a>Sistema de control de versiones

Un sistema de control de versiones se integra en una instancia de Azure Pipelines y se hospeda fuera de Azure.

## <a name="customer-business-continuity-requirements"></a>Requisitos de continuidad empresarial del cliente

Es importante determinar la funcionalidad empresarial mínima que necesitará si se produce un desastre.

### <a name="contoso-retails-business-continuity-requirements"></a>Requisitos de continuidad empresarial de Contoso Retail

* Hay que estar protegidos ante un error de alcance regional o un problema de estado del servicio regional.
* Mis clientes no deben ver nunca un error 404. El contenido público debe servirse siempre. (RTO: 0)  
* Durante la mayor parte del año, podemos mostrar contenido público con una obsolescencia de cinco horas. (RPO: 5 horas)
* Durante la temporada de vacaciones, nuestro contenido de acceso público debe estar siempre actualizado. (RPO: 0)
* Mis requisitos en materia de elaboración de informes internos no se consideran críticos para la continuidad empresarial.
* Es necesario optimizar los costos de continuidad empresarial.

## <a name="proposed-solution"></a>Solución propuesta

La siguiente imagen muestra la arquitectura de recuperación ante desastres de alta disponibilidad de Contoso Retail.

:::image type="content" source="./media/hdinsight-high-availability-case-study/contoso-solution.png" alt-text="Arquitectura de Contoso Retail":::

**Kafka** usa una replicación [activa-pasiva](hdinsight-business-continuity-architecture.md#apache-kafka) para reflejar los temas de Kafka de la región primaria en la secundaria. Una alternativa a la replicación de Kafka podría ser realizar la producción en Kafka en ambas regiones.

**Hive y Spark** usan los modelos de replicación [primaria activa con secundaria a petición](hdinsight-business-continuity-architecture.md#apache-spark) durante el horario normal. El proceso de replicación de Hive se ejecuta periódicamente y se acompaña del metastore de Hive en Azure SQL y la replicación de la cuenta de almacenamiento de Hive. La cuenta de almacenamiento de Spark se replica periódicamente con la herramienta DistCP de ADF. La naturaleza transitoria de estos clústeres contribuye a optimizar los costos. Las replicaciones se programan cada cuatro horas para lograr un RPO que queda claramente por debajo del requisito de cinco horas.

La replicación de **HBase** usa el modelo [líder-seguidor](hdinsight-business-continuity-architecture.md#apache-hbase) durante el horario normal para garantizar que los datos se sirvan siempre sin importar la región y que el RPO sea igual a cero.

Si hay un error regional en la región primaria, la página web y el contenido del back-end se sirven desde la región secundaria durante cinco horas con cierto grado de obsolescencia. Si el panel de estado del servicio de Azure no indica un tiempo estimado de recuperación en el período de cinco horas, la arquitectura de Contoso Retail creará la capa de transformación de Hive y Spark en la región secundaria y, después, hará que todos los orígenes de datos ascendentes apunten a la región secundaria. El hecho de convertir la región secundaria en grabable causaría un proceso de conmutación por recuperación que implicaría la replicación en la región primaria.

Durante la temporada alta de compras, toda la canalización secundaria está siempre activa y en ejecución. Los productores de Kafka realizan su tarea en ambas regiones, y la replicación de HBase pasa del modelo líder-líder al modelo líder-seguidor para garantizar que el contenido de acceso público esté siempre actualizado.

No es necesario diseñar una solución de conmutación por error para la creación de informes internos, ya que no es algo crítico para la continuidad empresarial.

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre los elementos que se describen en este artículo, consulte:

* [Continuidad empresarial de Azure HDInsight](./hdinsight-business-continuity.md)
* [Arquitecturas de continuidad empresarial de Azure HDInsight](./hdinsight-business-continuity-architecture.md)
* [¿Qué son Apache Hive y HiveQL en Azure HDInsight?](./hadoop/hdinsight-use-hive.md)