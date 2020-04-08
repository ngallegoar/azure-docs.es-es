---
title: Solución de problemas en las salidas de Azure Stream Analytics
description: En este artículo se describen técnicas para solucionar problemas de las conexiones de salida en los trabajos de Azure Stream Analytics.
author: sidram
ms.author: sidram
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 03/31/2020
ms.custom: seodec18
ms.openlocfilehash: 305632a0faa1eb7e217e86d36c5159e557df7aaf
ms.sourcegitcommit: 27bbda320225c2c2a43ac370b604432679a6a7c0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/31/2020
ms.locfileid: "80409251"
---
# <a name="troubleshoot-azure-stream-analytics-outputs"></a>Solución de problemas en las salidas de Azure Stream Analytics

En este artículo se describen los problemas comunes relacionados con las conexiones de salida de Azure Stream Analytics, cómo solucionar problemas de salida y cómo corregir los problemas. Muchos pasos para solucionar problemas requieren que los registros de diagnóstico estén habilitados para el trabajo de Stream Analytics. Si los registros de diagnóstico están habilitados, vea [Solución de problemas de Azure Stream Analytics mediante registros de diagnóstico](stream-analytics-job-diagnostic-logs.md).

## <a name="output-not-produced-by-job"></a>Salida no generada por trabajo

1.  Compruebe la conectividad a las salidas con el botón **Probar conexión** para cada una de las salidas.

2.  Examine las [**métricas de supervisión**](stream-analytics-monitoring.md) en la pestaña **Monitor**. Dado que los valores se agregan, las métricas se retrasan unos minutos.
   * Si los eventos de entrada son mayores que 0, el trabajo puede leer los datos de entrada. Si los eventos de entrada no son mayores que 0, hay un problema con la entrada del trabajo. Vea [Solución de problemas de conexiones de entrada](stream-analytics-troubleshoot-input.md) para obtener información sobre cómo solucionar problemas de conexión de entrada.
   * Si los errores de conversión de datos son mayores que 0 y cifras superiores, vea [Errores de datos de Azure Stream Analytics](data-errors.md) para obtener información detallada sobre los errores de conversión de datos.
   * Si los errores en tiempo de ejecución son mayores que 0, el trabajo puede recibir los datos, pero genera errores al procesar la consulta. Para encontrar los errores, vaya a los [registros de auditoría](../azure-resource-manager/management/view-activity-logs.md) y filtre por el estado *Error*.
   * Si InputEvents es mayor que 0 y OutputEvents es igual a 0, se cumple una de las siguientes condiciones:
      * El procesamiento de consultas no ha generado ningún evento de salida.
      * Los eventos o los campos pueden tener un formato incorrecto, por lo que no se genera ninguna salida después de procesar la consulta.
      * El trabajo no pudo insertar datos en el receptor de salida por motivos de autenticación o conectividad.

   En todos los casos de error mencionados anteriormente, los mensajes de registro de operaciones explican detalles adicionales (como lo que está sucediendo), salvo en casos en que la lógica de la consulta filtra todos los eventos. Si el procesamiento de varios eventos genera errores, los errores se agregan cada 10 minutos.

## <a name="job-output-is-delayed"></a>Se retrasa la salida del trabajo

### <a name="first-output-is-delayed"></a>Se retrasa la primera salida

Cuando se inicia un trabajo de Stream Analytics, se leen los eventos de entrada, pero puede darse un retraso en la salida en determinadas circunstancias.

La presencia de valores de tiempo elevados en los elementos de consulta temporales pueden influir en el retraso de la salida. Para generar un resultado correcto a través de períodos de tiempo muy prolongados, el trabajo de streaming empieza por leer datos desde la hora más antigua posible (hasta siete días atrás) para rellenar la ventana de tiempo. Durante ese tiempo, no se produce ninguna salida hasta que se ponga al día la lectura de los eventos de entrada pendientes. Este problema puede surgir cuando el sistema actualiza los trabajos de streaming, reiniciando así el trabajo. Generalmente, estas actualizaciones se producen una vez cada dos meses.

Por lo tanto, tenga precaución al diseñar la consulta de Stream Analytics. Si usa un período de tiempo muy prolongado (más de unas horas, hasta siete días) para los elementos temporales en la sintaxis de la consulta del trabajo, se puede producir en un retraso en la primera salida cuando se inicia o reinicia el trabajo.  

Una solución para este tipo de primer retraso de salida consiste en usar técnicas de ejecución en paralelo de consultas (creando particiones de datos) o agregar más unidades de streaming para mejorar el rendimiento hasta que el trabajo se ponga al día.  Para obtener más información, vea [Considerations when creating Stream Analytics jobs](stream-analytics-concepts-checkpoint-replay.md) (Consideraciones al crear trabajos de Stream Analytics).

Estos factores afectan a la vigencia de la primera salida que se genera:

1. Uso de agregados en ventanas (GROUP BY de ventanas de saltos de tamaño constante, de salto y deslizantes)
   - En el caso de los agregados de ventana de saltos de tamaño constante o de salto, los resultados se generan al final del período de tiempo.
   - En las ventanas deslizantes, los resultados se generan cuando un evento entra en la ventana deslizante o sale de ella.
   - Si tiene previsto usar un tamaño de ventana grande (> 1 hora), es mejor elegir la ventana de salto o deslizante para que se puedan ver los resultados con más frecuencia.

2. Uso de combinaciones temporales (JOIN con DATEDIFF)
   - Tan pronto como lleguen ambos lados de los eventos coincidentes, se generarán coincidencias.
   - Los datos que no tienen coincidencias (LEFT OUTER JOIN) se generan al final de la ventana DATEDIFF con respecto a cada evento en el lado izquierdo.

3. Uso de funciones analíticas temporales (ISFIRST, LAST y LAG con LIMIT DURATION)
   - En el caso de las funciones analíticas, el resultado se genera para cada evento, no hay ningún retraso.

### <a name="output-falls-behind"></a>La salida queda fuera

Durante el funcionamiento normal del trabajo, si detecta que la que salida del trabajo se queda fuera (latencia mayor y más larga), puede identificar las causas raíz mediante el examen de estos factores:
- Si está limitado el receptor inferior
- Si está limitado el receptor superior
- Si la lógica de procesamiento de la consulta es de proceso intensivo

Para ver los detalles, en Azure Portal, seleccione el trabajo de streaming y seleccione el **diagrama de trabajo**. Para cada entrada, hay una métrica de evento de trabajo pendiente por partición. Si la métrica de evento de trabajo pendiente sigue aumentando, es un indicador de que los recursos del sistema están limitados. Puede que se deba a la limitación del receptor de salida o una alta CPU. Para obtener más información sobre cómo usar el diagrama de trabajo, consulte [Depuración orientada a datos mediante el diagrama de trabajo](stream-analytics-job-diagram-with-metrics.md).

## <a name="key-violation-warning-with-azure-sql-database-output"></a>Advertencia de infracción clave con salida de Azure SQL Database

Al configurar Azure SQL Database como salida para un trabajo de Stream Analytics, inserta registros en bloque en la tabla de destino. En general, Azure Stream Analytics garantiza [al menos una entrega](https://docs.microsoft.com/stream-analytics-query/event-delivery-guarantees-azure-stream-analytics) al receptor de salida, aún se puede [lograr exactamente una entrega]( https://blogs.msdn.microsoft.com/streamanalytics/2017/01/13/how-to-achieve-exactly-once-delivery-for-sql-output/) a la salida de SQL cuando la tabla SQL tiene una restricción única definida.

Una vez configuradas las restricciones de clave únicas en la tabla SQL, si existen registros duplicados que se insertan en la tabla SQL, Azure Stream Analytics quita el registro duplicado. Divide los datos en lotes e inserta los lotes de forma recursiva hasta que se encuentra un único registro duplicado. Si el trabajo de streaming tiene un gran número de filas duplicadas, este proceso de división e inserción tiene que pasar por alto los duplicados uno por uno, lo cual es menos eficaz y consume muchos recursos. Si ve varios mensajes de advertencia de infracción de clave en el registro de actividad en la última hora, es probable que la salida de SQL esté ralentizando todo el trabajo.

Para resolver este problema, [configure el índice]( https://docs.microsoft.com/sql/t-sql/statements/create-index-transact-sql) que provoca la infracción de clave al habilitar la opción IGNORE_DUP_KEY. Al habilitar esta opción, se permite que SQL omita los valores duplicados durante las inserciones en bloque y SQL Azure simplemente genera un mensaje de advertencia en lugar de un error. Azure Stream Analytics ya no producen errores de infracción de clave principal.

Tenga en cuenta las observaciones siguientes al configurar IGNORE_DUP_KEY para varios tipos de índices:

* No se puede establecer IGNORE_DUP_KEY en una clave principal o una restricción única que use ALTER INDEX, debe quitar y volver a crear el índice.  
* Puede establecer la opción IGNORE_DUP_KEY con ALTER INDEX para un índice único, que es diferente de la restricción PRIMARY KEY/UNIQUE y se crea con la definición de INDEX o CREATE INDEX.  

* IGNORE_DUP_KEY no se aplica a los índices de almacén de columna, ya que en ellos no se puede exigir exclusividad.  

## <a name="column-names-are-lower-cased-by-azure-stream-analytics"></a>Azure Stream Analytics pone en minúsculas los nombres de columna.
Cuando se usa el nivel de compatibilidad original (1.0), Azure Stream Analytics cambiaba los nombres de columna a minúsculas. Este comportamiento se corrigió en niveles de compatibilidad posteriores. Con el fin de conservar las mayúsculas y minúsculas, se recomienda a los clientes usar el nivel de compatibilidad 1.1 y posteriores. Puede encontrar más información en [Nivel de compatibilidad de los trabajos de Azure Stream Analytics](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-compatibility-level).

## <a name="get-help"></a>Obtener ayuda

Para obtener más ayuda, pruebe nuestro [foro de Azure Stream Analytics](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureStreamAnalytics).

## <a name="next-steps"></a>Pasos siguientes

* [Introducción a Azure Stream Analytics](stream-analytics-introduction.md)
* [Introducción al uso de Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Escalación de trabajos de Azure Stream Analytics](stream-analytics-scale-jobs.md)
* [Referencia del lenguaje de consulta de Azure Stream Analytics](https://docs.microsoft.com/stream-analytics-query/stream-analytics-query-language-reference)
* [Referencia de API de REST de administración de Azure Stream Analytics](https://msdn.microsoft.com/library/azure/dn835031.aspx)
