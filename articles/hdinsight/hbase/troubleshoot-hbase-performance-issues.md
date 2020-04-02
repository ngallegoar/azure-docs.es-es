---
title: Solución de problemas de rendimiento de Apache HBase en Azure HDInsight
description: Diversas instrucciones y sugerencias de ajuste del rendimiento de Apache HBase para obtener un rendimiento óptimo en Azure HDInsight.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: troubleshooting
ms.date: 09/24/2019
ms.openlocfilehash: 93698fadcecf190dd8bbc24a9d03978899d3c5e9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "75887162"
---
# <a name="troubleshoot-apache-hbase-performance-issues-on-azure-hdinsight"></a>Solución de problemas de rendimiento de Apache HBase en Azure HDInsight

En este artículo se describen diversas instrucciones y sugerencias de ajuste del rendimiento de Apache HBase para obtener un rendimiento óptimo en Azure HDInsight. Muchas de estas sugerencias dependen de la carga de trabajo y del patrón de lectura, escritura o examen concretos. Antes de aplicar los cambios de configuración en un entorno de producción, pruébelos exhaustivamente.

## <a name="hbase-performance-insights"></a>Información del rendimiento de HBase

El principal cuello de botella en la mayoría de las cargas de trabajo de HBase es el registro de escritura previa (WAL). Afecta de manera importante al rendimiento de escritura. HDInsight HBase tiene un modelo de proceso de almacenamiento separado. Los datos se almacenan de forma remota en Azure Storage, aunque las máquinas virtuales hospedan los servidores de las regiones. Hasta hace poco, el WAL también se escribía en Azure Storage. En HDInsight, este comportamiento ampliaba el cuello de botella. La característica [Escrituras aceleradas](./apache-hbase-accelerated-writes.md) está diseñada para resolver este problema. Escribe el WAL en los discos administrados por SSD Premium de Azure. Esto supone una mejora tremenda en el rendimiento de la escritura y ayuda a resolver muchos problemas con los que se encuentran algunas de las cargas de trabajo de escritura intensiva.

Para obtener una mejora significativa en las operaciones de lectura, use una [cuenta de almacenamiento de blobs en bloques Premium](https://azure.microsoft.com/blog/azure-premium-block-blob-storage-is-now-generally-available/) como almacenamiento remoto. Esta opción solo es posible si está habilitada la función de WAL.

## <a name="compaction"></a>Compactación

La compactación es otro posible cuello de botella nen el que la comunidad está fundamentalmente de acuerdo. De forma predeterminada, la compactación principal está deshabilitada en los clústeres de HDInsight HBase. La compactación está deshabilitada porque, aunque se trata de un proceso que consume muchos recursos, los clientes tienen total flexibilidad para programarla según sus cargas de trabajo. Por ejemplo, pueden programarla fuera de las horas punta. Además, la ubicación de los datos no es un problema porque el almacenamiento es remoto (con el respaldo de Azure Storage) en lugar de un sistema de archivos distribuido de Hadoop local (HDFS).

Los clientes deben programar una compactación principal cuando sea más conveniente. Si no realizan este mantenimiento, la compactación afectará negativamente al rendimiento de lectura a largo plazo.

En el caso de las operaciones de examen, las latencias medias que son muy superiores a 100 milisegundos deben ser motivo de preocupación. Compruebe si la compactación mayor se ha programado de manera adecuada.

## <a name="apache-phoenix-workload"></a>Carga de trabajo de Apache Phoenix

La respuesta a las siguientes preguntas le ayudará a comprender mejor la carga de trabajo de Apache Phoenix:

* ¿Todas las "lecturas" se convierten en exámenes?
    * Si es así, ¿cuáles son las características de estos exámenes?
    * ¿Ha optimizado el esquema de tabla de Phoenix para estos exámenes, incluida una indexación adecuada?
* ¿Ha utilizado la instrucción `EXPLAIN` para comprender los planes de consulta que generan las "lecturas"?
* ¿Las escrituras son instrucciones "upsert-select"?
    * Si es así, también estarán realizando exámenes. La latencia esperada para los exámenes tiene un promedio aproximado de 100 milisegundos, en comparación con los 10 milisegundos de la obtención de puntos en HBase.  

## <a name="test-methodology-and-metrics-monitoring"></a>Metodología de prueba y supervisión de métricas

Si utiliza pruebas comparativas como Yahoo!, Cloud Serving Benchmark, JMeter o Pherf para probar y ajustar el rendimiento, asegúrese de que:

- Los equipos cliente no se convierten en un cuello de botella. Para ello, compruebe el uso de CPU en los equipos cliente.

- Las configuraciones de cliente (como el número de subprocesos) se ajustan adecuadamente para saturar el ancho de banda del cliente.

- Los resultados de las pruebas se registran de forma precisa y sistemática.

Si las consultas comenzaron a empeorar más que antes, busque posibles errores en el código de la aplicación. ¿Se generan repentinamente grandes cantidades de datos no válidos? Si es así, puede aumentar las latencias de lectura.

## <a name="migration-issues"></a>Problemas de migración

Si va a migrar a Azure HDInsight, asegúrese de que la migración se realiza de forma sistemática y precisa, preferiblemente mediante automatización. Evite la migración manual. Asegúrese de lo siguiente:

- Los atributos de tabla se migran de forma precisa. Los atributos pueden incluir compresiones, filtros de Bloom, etc.

- La configuración del cifrado con sal en las tablas de Phoenix está asignado correctamente según el nuevo tamaño del clúster. Por ejemplo, el número de cubos de sal debe ser un múltiplo del número de nodos de trabajo en el clúster. Además, debe usar un múltiplo que sea un factor de la cantidad de zonas activas.

## <a name="server-side-configuration-tunings"></a>Ajustes de la configuración del servidor

En HDInsight HBase, los archivos HFiles se almacenan en el almacenamiento remoto. Cuando hay un error de caché, el costo de las lecturas es superior al de los sistemas locales porque los datos de los sistemas locales están respaldados por un HDFS local. En la mayoría de los escenarios, el uso inteligente de las memorias caché de HBase (caché de bloques y caché de cubos) está diseñado para evitar este problema. En los casos en los que el problema no se evita, el uso de una cuenta de blob en bloques Premium puede resultar de ayuda. El controlador de Windows Azure Storage Blob requiere de determinadas propiedades, como `fs.azure.read.request.size`, para capturar los datos en bloques en función de los cuales determina el modo de lectura (secuencial o aleatorio). Por ello, es posible que se sigan produciendo casos de latencias mayores con operaciones de lectura. A partir de experimentos prácticos, determinamos que establecer el tamaño del bloque de solicitud de lectura (`fs.azure.read.request.size`) en 512 KB y hacer que el tamaño de bloque de las tablas de HBase sea el mismo ofrece los mejores resultados.

Para la mayoría de los clústeres de nodos de gran tamaño, HDInsight HBase proporciona `bucketcache` como un archivo en un SSD Premium local que está conectado a la máquina virtual, que ejecuta `regionservers`. Si en su lugar usa la memoria caché fuera del montón, puede suponer alguna mejora. Esta solución alternativa tiene la limitación de que usa la memoria disponible y puede ser más pequeña que la memoria caché basada en archivos, por lo que es posible que no sea siempre la mejor opción.

A continuación se muestran algunos de los otros parámetros específicos que se ajustaron y parecen ayudar en distintos niveles:

- Aumentar el tamaño de `memstore` de forma predeterminada de 128 MB a 256 MB. Normalmente, se recomienda esta configuración para escenarios de escritura elevada.

- Aumentar el número de subprocesos dedicados a la compactación, pasando de la configuración predeterminada de **1** a **4**. Esta configuración es pertinente si observamos frecuentes compactaciones menores.

- Evitar el bloqueo del vaciado de `memstore` debido al límite del almacén. Para proporcionar este búfer, aumente el valor de la configuración `Hbase.hstore.blockingStoreFiles` a **100**.

- Para controlar los vaciados, utilice la siguiente configuración:

    - `Hbase.regionserver.maxlogs`: **140** (evita vaciados debido a los límites de WAL)

    - `Hbase.regionserver.global.memstore.lowerLimit`: **0.55**

    - `Hbase.regionserver.global.memstore.upperLimit`: **0.60**

- Configuraciones específicas de Phoenix para el ajuste de grupos de subprocesos:

    - `Phoenix.query.queuesize`: **10000**

    - `Phoenix.query.threadpoolsize`: **512**

- Otras configuraciones específicas de Phoenix:

    - `Phoenix.rpc.index.handler.count`: **50** (si hay muchas búsquedas de índice o son grandes)

    - `Phoenix.stats.updateFrequency`: **1 hora**

    - `Phoenix.coprocessor.maxmetadatacachetimetolivems`: **1 hora**

    - `Phoenix.coprocessor.maxmetadatacachesize`: **50 MB**

- Tiempos de espera de RPC: **3 minutos**

   - Los tiempos de espera de RPC incluyen el tiempo de espera de RPC de HBase, el tiempo de espera del examen de cliente de HBase y el tiempo de espera de consulta de Phoenix. 
   - Asegúrese de que el parámetro `hbase.client.scanner.caching` está establecido en el mismo valor en el servidor y en el cliente. Si no son iguales, esta configuración conduce a errores de cliente que están relacionados con `OutOfOrderScannerException`. Esta opción debe establecerse en un valor bajo para exámenes grandes. Establecemos este valor en **100**.

## <a name="other-considerations"></a>Otras consideraciones

A continuación se indican los parámetros adicionales que debe tener en cuenta para optimización:

- `Hbase.rs.cacheblocksonwrite`: de forma predeterminada, este valor se establece **true**.

- Configuración que permite diferir la compactación menor para más adelante.

- Configuración experimental, como el ajuste de porcentajes de colas que están reservadas para solicitudes de lectura y escritura.

## <a name="next-steps"></a>Pasos siguientes

Si el problema sigue sin resolverse, visite uno de los siguientes canales para obtener más soporte técnico:

- Obtenga respuestas de expertos de Azure mediante el [soporte técnico de la comunidad de Azure](https://azure.microsoft.com/support/community/).

- Conectar con [@AzureSupport](https://twitter.com/azuresupport). Esta es la cuenta oficial de Microsoft Azure para mejorar la experiencia del cliente. Pone en contacto a la comunidad de Azure con los recursos adecuados: respuestas, soporte técnico y expertos.

- Si necesita más ayuda, puede enviar una solicitud de soporte técnico desde [Azure Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/). Seleccione **Soporte técnico** en la barra de menús o abra la central **Ayuda + soporte técnico**. Para obtener información más detallada, revise [Creación de una solicitud de soporte técnico de Azure](https://docs.microsoft.com/azure/azure-portal/supportability/how-to-create-azure-support-request). La suscripción a Microsoft Azure incluye acceso al soporte técnico para administración de suscripciones y facturación. El soporte técnico se proporciona a través de uno de los [planes de soporte técnico de Azure](https://azure.microsoft.com/support/plans/).
