---
title: Solución de problemas de los flujos de datos de asignación
description: Obtenga información acerca de la solución de problemas relacionados con flujos de datos en Azure Data Factory.
services: data-factory
ms.author: makromer
author: kromerm
ms.reviewer: daperlov
ms.service: data-factory
ms.topic: troubleshooting
ms.date: 09/11/2020
ms.openlocfilehash: 5f29474705919f402b1c114c3fd2df0df037cdae
ms.sourcegitcommit: e2dc549424fb2c10fcbb92b499b960677d67a8dd
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/17/2020
ms.locfileid: "94696071"
---
# <a name="troubleshoot-mapping-data-flows-in-azure-data-factory"></a>Solución de problemas de los flujos de datos de asignación en Azure Data Factory

[!INCLUDE[appliesto-adf-xxx-md](includes/appliesto-adf-xxx-md.md)]

En este artículo se exploran métodos comunes de solución de problemas de flujos de datos de asignación en Azure Data Factory.

## <a name="common-errors-and-messages"></a>Errores habituales y mensajes

### <a name="error-code-df-executor-sourceinvalidpayload"></a>Código de error: DF-Executor-SourceInvalidPayload
- **Mensaje**: Data preview, debug, and pipeline data flow execution failed because container does not exist (Error en la ejecución del flujo de datos de la depuración, la canalización y la vista previa de datos porque el contenedor no existe)
- **Causas**: cuando el conjunto de datos contiene un contenedor que no existe en el almacenamiento
- **Recomendación:** asegúrese de que el contenedor al que se hace referencia en el conjunto de datos exista o sea accesible.

### <a name="error-code-df-executor-systemimplicitcartesian"></a>Código de error: DF-Executor-SystemImplicitCartesian

- **Mensaje**: Implicit cartesian product for INNER join is not supported, use CROSS JOIN instead. Columns used in join should create a unique key for rows. (No se admite el producto cartesiano implícito para la combinación interna; utilice la combinación cruzada en su lugar. Las columnas utilizadas en la combinación deben crear una clave única para las filas).
- **Causas**: no se admite el producto cartesiano implícito para la combinación interna entre planes lógicos. Si las columnas usadas en la combinación crean la clave única, se necesita al menos una columna de ambos lados de la relación.
- **Recomendación:** en el caso de las combinaciones que no se basan en la igualdad, debe optar por la combinación cruzada personalizada.

### <a name="error-code-df-executor-systeminvalidjson"></a>Código de error: DF-Executor-SystemInvalidJson

- **Mensaje**: JSON parsing error, unsupported encoding or multiline (Error de análisis de JSON, codificación no compatible o multilínea)
- **Causas**: posibles problemas con el archivo JSON, como codificación no admitida, bytes dañados o se usa el origen JSON como un único documento en muchas líneas anidadas.
- **Recomendación:** verifique que se admite la codificación del archivo JSON. En la transformación de origen que usa un conjunto de datos JSON, expanda "JSON Settings" (Configuración de JSON) y active "Documento único".
 
### <a name="error-code-df-executor-broadcasttimeout"></a>Código de error: DF-Executor-BroadcastTimeout

- **Mensaje**: Broadcast join timeout error, make sure broadcast stream produces data within 60 secs in debug runs and 300 secs in job runs (Error de tiempo de espera de combinación de difusión, asegúrese de que el flujo de difusión genera datos en un plazo de 60 segundos en ejecuciones de depuración y de 300 segundos en ejecuciones de trabajos).
- **Causas**: la difusión tiene un tiempo de espera predeterminado de 60 segundos en ejecuciones de depuración y de 300 segundos en ejecuciones de trabajos. El flujo elegido para la difusión parece demasiado grande para generar datos dentro de este límite.
- **Recomendación:** active la pestaña Optimizar en las transformaciones de flujo de datos para Join, Exists y Lookup. La opción predeterminada para Broadcast (Difusión) es "Auto". Si se establece "Auto" o si va a configurar manualmente el lado izquierdo o derecho para difundir en "Fixed" (Fijo), puede establecer una configuración mayor de Azure Integration Runtime o desactivar la difusión. El enfoque recomendado para el mejor rendimiento en los flujos de datos es permitir que Spark realice la difusión mediante "Auto" y use una instancia de Azure IR optimizada para memoria.

Si ejecuta el flujo de datos en una ejecución de pruebas de depuración desde una ejecución de la canalización de depuración, puede ejecutar en esta condición con mayor frecuencia. Esto se debe a que ADF limita el tiempo de espera de difusión a 60 segundos para mantener una experiencia de depuración más rápida. Si desea ampliarlo al tiempo de espera de 300 segundos de una ejecución desencadenada, puede usar la opción Debug > Use Activity Runtime (Depurar > Usar tiempo de ejecución de la actividad) para utilizar la instancia de Azure IR definida en su actividad de canalización Ejecución de flujo de datos.

### <a name="error-code-df-executor-conversion"></a>Código de error: DF-Executor-Conversion

- **Mensaje**: Converting to a date or time failed due to an invalid character (Error al realizar la conversión a una fecha o hora porque un carácter no es válido).
- **Causas**: los datos no tienen el formato esperado.
- **Recomendación:** use el tipo de datos correcto.

### <a name="error-code-df-executor-invalidcolumn"></a>Código de error: DF-Executor-InvalidColumn

- **Mensaje**: Column name needs to be specified in the query, set an alias if using a SQL function (Es necesario especificar el nombre de la columna en la consulta; establezca un alias si usa una función SQL).
- **Causas**: no se ha especificado ningún nombre de columna.
- **Recomendación:** establezca un alias si usa una función SQL, como min()/max(), etc.

 ### <a name="error-code-df-executor-drivererror"></a>Código de error: DF-Executor-DriverError
- **Mensaje**: INT96 es un tipo de marca de tiempo heredado que no es compatible con el flujo de datos de ADF. Considere la posibilidad de actualizar el tipo de columna a los tipos más recientes.
- **Causas**: error del controlador
- **Recomendación:** INT96 es un tipo de marca de tiempo heredado que no es compatible con el flujo de datos de ADF. Considere la posibilidad de actualizar el tipo de columna a los tipos más recientes.

 ### <a name="error-code-df-executor-blockcountexceedslimiterror"></a>Código de error: DF-Executor-BlockCountExceedsLimitError
- **Mensaje**: el recuento de bloques sin confirmar no puede superar el límite máximo de 100 000 bloques. Compruebe la configuración de blobs.
- **Causas**: puede haber un máximo de 100 000 bloques sin confirmar en un blob.
- **Recomendación:** póngase en contacto con el equipo de productos de Microsoft en relación con este problema para obtener más detalles.

 ### <a name="error-code-df-executor-partitiondirectoryerror"></a>Código de error: DF-Executor-PartitionDirectoryError
- **Mensaje**: la ruta de origen especificada tiene varios directorios con particiones (por ejemplo, <Source Path>/<Directorio raíz de la partición 1>/a=10/b=20, <Source Path>/<Directorio raíz de la partición 2>/c=10/d=30) o un directorio con particiones con otro archivo o directorio sin particiones (por ejemplo, <Source Path>/<Directorio raíz con particiones 1>/a=10/b=20, <Source Path>/Directorio 2/archivo1). Quite el directorio raíz con particiones de la ruta de origen y léalo a través de una transformación de origen independiente.
- **Causas**: la ruta de origen tiene varios directorios con particiones o un directorio con particiones con otro archivo o directorio sin particiones.
- **Recomendación:** quite el directorio raíz con particiones de la ruta de origen y léalo a través de una transformación de origen independiente.

 ### <a name="error-code-df-executor-outofmemoryerror"></a>Código de error: DF-Executor-OutOfMemoryError
- **Mensaje**: el clúster ha tenido un problema de memoria insuficiente durante la ejecución. Vuelva a intentar usar un entorno de ejecución de integración con un mayor recuento de núcleos o un tipo de proceso optimizado para memoria
- **Causas**: el clúster se está quedando sin memoria
- **Recomendación:** los clústeres de depuración están diseñados con fines de desarrollo. Aproveche el muestreo de datos, el tipo de proceso adecuado y el tamaño para ejecutar la carga. Consulte la [guía de rendimiento del flujo de datos de asignación](concepts-data-flow-performance.md) para llevar a cabo una optimización y, de este modo, lograr el mejor rendimiento.

 ### <a name="error-code-df-executor-illegalargument"></a>Código de error: DF-Executor-illegalArgument
- **Mensaje**: asegúrese de que la clave de acceso del servicio vinculado es correcta
- **Causas**: nombre de cuenta o clave de acceso incorrectos
- **Recomendación:** asegúrese de que el nombre de cuenta o la clave de acceso especificados en el servicio vinculado son correctos. 

 ### <a name="error-code-df-executor-invalidtype"></a>Código de error: DF-Executor-InvalidType
- **Mensaje**: asegúrese de que el tipo de parámetro coincide con el tipo de valor pasado. Actualmente no se admite el paso de parámetros float desde canalizaciones.
- **Causas**: tipos de datos incompatibles entre el tipo declarado y el valor de parámetro real
- **Recomendación:** compruebe que los valores de parámetro pasados a un flujo de datos coinciden con el tipo declarado.

 ### <a name="error-code-df-executor-columnunavailable"></a>Código de error: DF-Executor-ColumnUnavailable
- **Mensaje**: el nombre de columna usado en la expresión no está disponible o no es válido
- **Causas**: nombre de columna no válido o no disponible usado en expresiones
- **Recomendación:** compruebe los nombres de columna usados en expresiones

 ### <a name="error-code-df-executor-parseerror"></a>Código de error: DF-Executor-ParseError
- **Mensaje**: no puede analizarse la expresión
- **Causas**: la expresión tiene errores de análisis debido al formato
- **Recomendación:** compruebe el formato en la expresión

### <a name="error-code-getcommand-outputasync-failed"></a>Código de error: Error de GetCommand OutputAsync

- **Mensaje**: Durante la depuración de Data Flow y la vista previa de datos: Error de GetCommand OutputAsync con...
- **Causas**: Se trata de un error del servicio de back-end. Puede volver a intentar la operación y reiniciar la sesión de depuración.
- **Recomendación:** Si reintentar y reiniciar no resuelven el problema, póngase en contacto con el soporte al cliente.

### <a name="error-code-hit-unexpected-exception-and-execution-failed"></a>Código de error: Se obtuvo una excepción inesperada y se produjo un error de ejecución

- **Mensaje**: Durante la ejecución de una actividad de Data Flow: Se obtuvo una excepción inesperada y se produjo un error de ejecución.
- **Causas**: Se trata de un error del servicio de back-end. Puede volver a intentar la operación y reiniciar la sesión de depuración.
- **Recomendación:** Si reintentar y reiniciar no resuelven el problema, póngase en contacto con el soporte al cliente.

### <a name="error-code-debug-data-preview-no-output-data-on-join"></a>Código de error: Debug data preview No Output Data on Join (Vista previa de datos de depuración: sin datos de salida en la combinación)

- **Mensaje**: hay un gran número de valores NULL o valores que faltan, lo que puede deberse a que no se han muestreado suficientes filas. Pruebe a actualizar el límite de filas de depuración y los datos.
- **Causas**: la condición de combinación no coincidía con ninguna fila o dio como resultado un número elevado de valores NULL durante la vista previa de los datos.
- **Recomendación:** vaya a Configuración de depuración y aumente el número de filas del límite de filas de origen. Asegúrese de que ha seleccionado una instancia de Azure IR con un clúster de flujo de datos lo suficientemente grande como para administrar más datos.

### <a name="error-code-validation-error-at-source-with-multiline-csv-files"></a>Código de error: Error de validación en el origen con archivos CSV de varias líneas 

- **Mensaje**: Puede ver uno de los siguientes mensajes de error:
   - Falta la última columna o es NULL.
   - Error en la validación del esquema en el origen.
   - La importación de esquema no se muestra correctamente en la experiencia de usuario y la última columna tiene un carácter de nueva línea en el nombre.
- **Causas**: actualmente, en el flujo de datos de asignación, el origen CSV de varias líneas no funciona con los caracteres \r\n como delimitador de filas. A veces, las líneas adicionales en los retornos de carros interrumpen los valores de origen. 
- **Recomendación:** genere el archivo en el origen con el carácter \n como delimitador de filas, en lugar de \r\n. O bien, use la actividad de copia para convertir el archivo CSV con caracteres \r\n en \n como delimitador de filas.

## <a name="general-troubleshooting-guidance"></a>Guía de solución de problemas generales

1. Compruebe el estado de las conexiones del conjunto de datos. En cada transformación de origen y de receptor, visite el servicio vinculado para cada conjunto de datos que use y pruebe las conexiones.
1. Compruebe el estado de las conexiones de archivos y tablas desde el diseñador de flujo de datos. Cambie al modo de depuración y haga clic en Vista previa de datos en las transformaciones de origen para asegurarse de que puede acceder a los datos.
1. Si todo parece correcto desde la vista previa de los datos, vaya al diseñador de canalizaciones y coloque el flujo de datos en una actividad de canalización. Depure la canalización para realizar una prueba de un extremo a otro.

## <a name="next-steps"></a>Pasos siguientes

Para obtener ayuda para solucionar problemas, pruebe estos recursos:
*  [Blog de Data Factory](https://techcommunity.microsoft.com/t5/azure-data-factory/bg-p/AzureDataFactoryBlog)
*  [Solicitud de características de Data Factory](https://feedback.azure.com/forums/270578-data-factory)
*  [Vídeos de Azure](https://www.youtube.com/channel/UC2S0k7NeLcEm5_IhHUwpN0g/videos)
*  [Página de preguntas y respuestas de Microsoft](/answers/topics/azure-data-factory.html)
*  [Foro de Stack Overflow para Data Factory](https://stackoverflow.com/questions/tagged/azure-data-factory)
*  [Información de Twitter sobre Data Factory](https://twitter.com/hashtag/DataFactory)
*  [Guía de optimización y rendimiento de Mapping Data Flows](concepts-data-flow-performance.md)
