---
title: Análisis detallado y procedimientos recomendados de puerta de enlace para Apache Hive en Azure HDInsight
description: Obtenga información sobre cómo navegar por los procedimientos recomendados para ejecutar consultas de Hive en la puerta de enlace de Azure HDInsight.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 04/01/2020
ms.openlocfilehash: 924b1132efeb3ee4211593da190f5b7251029ae3
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "80587332"
---
# <a name="gateway-deep-dive-and-best-practices-for-apache-hive-in-azure-hdinsight"></a>Análisis detallado y procedimientos recomendados de puerta de enlace para Apache Hive en Azure HDInsight

La puerta de enlace de Azure HDInsight (puerta de enlace) es el front-end de HTTPS para los clústeres de HDInsight. La puerta de enlace es responsable de la autenticación, la resolución del host, la detección de servicios y otras características útiles necesarias para un sistema distribuido moderno. Las características proporcionadas por la puerta de enlace producen una sobrecarga por las que puede navegar con los procedimientos recomendados de este documento. También se describen las técnicas de solución de problemas de la puerta de enlace.

## <a name="the-hdinsight-gateway"></a>La puerta de enlace de HDInsight

La puerta de enlace de HDInsight es la única parte de un clúster de HDInsight que es accesible públicamente a través de Internet. Se puede tener acceso al servicio de puerta de enlace sin pasar por la red pública de Internet mediante el punto de conexión de puerta de enlace interno `clustername-int.azurehdinsight.net`. El punto de conexión de puerta de enlace interno permite establecer conexiones con el servicio de puerta de enlace sin salir de la red virtual del clúster. La puerta de enlace controla todas las solicitudes que se envían al clúster y las reenvía a los componentes y hosts de clúster correctos.

En el diagrama siguiente se muestra una ilustración aproximada de cómo la puerta de enlace proporciona una abstracción delante de todas las distintas posibilidades de resolución del host en HDInsight.

![Diagrama de resolución del host](./media/gateway-best-practices/host-resolution-diagram.png "Diagrama de resolución del host")

## <a name="motivation"></a>Motivación

La motivación para colocar una puerta de enlace delante de los clústeres de HDInsight es proporcionar una interfaz para la detección de servicios y la autenticación de usuarios. Los mecanismos de autenticación proporcionados por la puerta de enlace son especialmente pertinentes para los clústeres habilitados para ESP.

Para la detección de servicios, la ventaja de la puerta de enlace es que se puede tener acceso a todos los componentes del clúster como punto de conexión diferente en el sitio web de la puerta de enlace (`clustername.azurehdinsight.net/hive2`), en lugar de una gran variedad de emparejamientos de `host:port`.

Para la autenticación, la puerta de enlace permite a los usuarios autenticarse mediante un par de credenciales de `username:password`. En el caso de los clústeres habilitados para ESP, esta credencial sería el nombre de usuario y la contraseña del dominio del usuario. La autenticación en los clústeres de HDInsight a través de la puerta de enlace no requiere que el cliente adquiera un vale de Kerberos. Dado que la puerta de enlace acepta credenciales de `username:password` y adquiere el vale de Kerberos del usuario en nombre del usuario, se pueden realizar conexiones seguras a la puerta de enlace desde cualquier host de cliente, incluidos los clientes unidos a dominios AA-DDS distintos a los del clúster (ESP).

## <a name="best-practices"></a>Procedimientos recomendados

La puerta de enlace es un servicio único (con equilibrio de carga entre dos hosts) responsable de la autenticación y el reenvío de solicitudes. La puerta de enlace puede convertirse en un cuello de botella de rendimiento para consultas de Hive que superen un tamaño determinado. Se puede observar la degradación del rendimiento de las consultas cuando se ejecutan consultas **SELECT** de gran volumen en la puerta de enlace a través de ODBC o JDBC. "De gran volumen" hace referencia a las consultas que conforman más de 5 GB de datos en filas o columnas. Esta consulta podría incluir una larga lista de filas o un número extenso de columnas.

La degradación del rendimiento de la puerta de enlace en torno a las consultas de gran tamaño se debe a que los datos se deben transferir desde el almacén de datos subyacente (ADLS Gen2) a HDInsight Hive Server, la puerta de enlace y, por último, a través de los controladores de JDBC o ODBC al host del cliente.

En el siguiente diagrama se ilustran los pasos implicados en una consulta SELECT.

![Diagrama de resultados](./media/gateway-best-practices/result-retrieval-diagram.png "Diagrama de resultados")

Apache Hive es una abstracción relacional sobre un sistema de archivos compatible con HDFS. Esta abstracción significa que las instrucciones **SELECT** en Hive se corresponden con operaciones **READ** en el sistema de archivos. Las operaciones **READ** se traducen en el esquema adecuado antes de notificarse al usuario. La latencia de este proceso aumenta con el tamaño de los datos y los saltos totales necesarios para llegar al usuario final.

Se puede producir un comportamiento similar al ejecutar instrucciones **CREATE** o **INSERT** de datos de gran tamaño, ya que estos comandos se corresponden con operaciones **WRITE** en el sistema de archivos subyacente. Considere la posibilidad de escribir datos, como ORC sin formato, en filesystem/datalake en lugar de cargarlos mediante**INSERT** o **LOAD**.

En los clústeres habilitados para Enterprise Security Pack, las directivas de Apache Ranger suficientemente complejas pueden ralentizar el tiempo de compilación de la consulta, lo que puede provocar un tiempo de espera de puerta de enlace. Si se observa un tiempo de espera de puerta de enlace en un clúster de ESP, considere la posibilidad de reducir o combinar el número de directivas de Ranger.

## <a name="troubleshooting-techniques"></a>Técnicas de solución de problemas

Existen múltiples lugares para mitigar y comprender los problemas de rendimiento que se encuentran a raíz del comportamiento anterior. Use la siguiente lista de comprobación cuando experimente la degradación del rendimiento de las consultas en la puerta de enlace de HDInsight:

* Use la cláusula **LIMIT** al ejecutar las consultas **SELECT** de gran tamaño. La cláusula **LIMIT** reducirá el número total de filas que se envían al host del cliente. La cláusula **LIMIT** solo afecta a la generación de resultados y no cambia el plan de consulta. Para aplicar la cláusula **LIMIT** al plan de consulta, use la configuración `hive.limit.optimize.enable`. **LIMIT** se puede combinar con un desplazamiento mediante el formato de argumento **LIMIT x,y**.

* Asigne un nombre a las columnas que le interesen al ejecutar consultas **SELECT** en lugar de usar **SELECT \*** . Al seleccionar menos columnas se reducirá la cantidad de datos leídos.

* Pruebe a ejecutar la consulta de interés a través de Apache Beeline. Si la recuperación de resultados a través de Apache Beeline tarda demasiado tiempo, es de esperar que se produzcan retrasos al recuperar los mismos resultados a través de herramientas externas.

* Pruebe una consulta de Hive básica para asegurarse de que se puede establecer una conexión con la puerta de enlace de HDInsight. Pruebe a ejecutar una consulta básica desde dos o más herramientas externas para asegurarse de que ninguna herramienta individual está teniendo problemas.

* Revise las alertas de Apache Ambari para asegurarse de que los servicios de HDInsight estén en buen estado. Las alertas de Ambari se pueden integrar con Azure Monitor para la elaboración de informes y análisis.

* Si usa una instancia de metastore de Hive externa, compruebe que la DTU de Azure SQL DB para la metastore de Hive no ha alcanzado su límite. Si la DTU se aproxima a su límite, deberá aumentar el tamaño de la base de datos.

* Asegúrese de que las herramientas de terceros, como PBI o Tableau, usan la paginación para ver tablas o bases de datos. Consulte a sus asociados de soporte técnico de estas herramientas para obtener ayuda para la paginación. La herramienta principal que se usa para la paginación es el parámetro `fetchSize` de JDBC. Un tamaño de captura pequeño puede dar lugar a un rendimiento de puerta de enlace deficiente, pero un tamaño de captura demasiado grande puede dar lugar a tiempo de espera en la puerta de enlace. El ajuste del tamaño de captura debe realizarse en función de la carga de trabajo.

* Si la canalización de datos implica leer una gran cantidad de datos desde el almacenamiento subyacente del clúster de HDInsight, considere la posibilidad de usar una herramienta que interactúe directamente con Azure Storage, como Azure Data Factory

* Considere la posibilidad de utilizar Apache Hive LLAP al ejecutar cargas de trabajo interactivas, ya que LLAP puede proporcionar una experiencia más fluida para devolver rápidamente los resultados de la consulta.

* O bien aumentar el número de subprocesos disponibles para la metastore de Hive mediante `hive.server2.thrift.max.worker.threads`. Esta configuración es especialmente importante cuando un gran número de usuarios simultáneos envían consultas al clúster.

* Reduzca el número de reintentos usados para contactar con la puerta de enlace desde cualquier herramienta externa. Si se usan varios reintentos, considere la posibilidad de seguir una directiva de reintentos de retroceso exponencial.

* Considere la posibilidad de habilitar la compresión Hive con las configuraciones `hive.exec.compress.output` y `hive.exec.compress.intermediate`.

## <a name="next-steps"></a>Pasos siguientes

* [Apache Beeline en HDInsight](https://docs.microsoft.com/azure/hdinsight/hadoop/apache-hadoop-use-hive-beeline)
* [Pasos de solución de problemas de tiempo de espera de HDInsight Gateway](https://docs.microsoft.com/azure/hdinsight/interactive-query/troubleshoot-gateway-timeout)
* [Redes virtuales para HDInsight](https://docs.microsoft.com/azure/hdinsight/hdinsight-plan-virtual-network-deployment)
* [HDInsight con ExpressRoute](https://docs.microsoft.com/azure/hdinsight/connect-on-premises-network)