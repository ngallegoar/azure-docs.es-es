---
title: Información sobre las salidas desde Azure Stream Analytics
description: En este artículo se describen las opciones de salida de datos disponibles en Azure Stream Analytics, incluido Power BI para los resultados del análisis.
author: mamccrea
ms.author: mamccrea
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 05/8/2020
ms.openlocfilehash: d1eda3671b52a1e4bbae9af2d97010657880c383
ms.sourcegitcommit: bb0afd0df5563cc53f76a642fd8fc709e366568b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/19/2020
ms.locfileid: "83585409"
---
# <a name="understand-outputs-from-azure-stream-analytics"></a>Información sobre las salidas desde Azure Stream Analytics

En este artículo se describen los tipos de salidas disponibles para los trabajos de Azure Stream Analytics. Las salidas le permiten almacenar y guardar los resultados de los trabajos de Stream Analytics. Con los datos de salida, puede realizar análisis de negocio adicionales y almacenamiento de los datos.

Al diseñar la consulta de Stream Analytics, haga referencia al nombre de la salida mediante la [cláusula INTO](https://docs.microsoft.com/stream-analytics-query/into-azure-stream-analytics). Puede usar una única salida por trabajo, o varias salidas por trabajo de streaming (si las necesita); para ello, proporcione varias cláusulas INTO en la consulta.

Para crear, editar y probar las salidas de trabajos de Stream Analytics, puede usar [Azure Portal](stream-analytics-quick-create-portal.md#configure-job-output), [Azure PowerShell](stream-analytics-quick-create-powershell.md#configure-output-to-the-job), la [API de .NET](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.streamanalytics.ioutputsoperations?view=azure-dotnet), la [API REST](https://docs.microsoft.com/rest/api/streamanalytics/stream-analytics-output) y [Visual Studio](stream-analytics-quick-create-vs.md).

Algunos tipos de salida admiten [particiones](#partitioning). Los [tamaños de lote de salida](#output-batch-size) varían para optimizar el rendimiento.


## <a name="azure-data-lake-storage-gen-1"></a>Azure Data Lake Storage Gen 1

Stream Analytics admite [Azure Data Lake Storage Gen 1](../data-lake-store/data-lake-store-overview.md). Azure Data Lake Storage es un repositorio hiperescalado de nivel empresarial para cargas de trabajo de análisis de macrodatos. Puede usar Data Lake Storage para almacenar datos de cualquier tamaño, tipo y velocidad de ingesta para realizar análisis exploratorios y operativos. Stream Analytics debe tener autorización para acceder a Data Lake Storage.

La salida de Azure Data Lake Storage desde Stream Analytics no está disponible actualmente en las regiones de Azure China (21Vianet ) y Azure Alemania (T-Systems International).

En la siguiente tabla se muestra una lista de nombres de propiedades y su descripción para configurar la salida de Data Lake Storage Gen 1.   

| Nombre de propiedad | Descripción |
| --- | --- |
| Alias de salida | Es un nombre descriptivo utilizado en las consultas para dirigir la salida de la consulta a Data Lake Storage. |
| Subscription | Suscripción que contiene la cuenta de Azure Data Lake Storage. |
| Nombre de cuenta | Nombre de la cuenta de Data Lake Storage donde va a enviar la salida. Se le presentará una lista desplegable de cuentas de Data Lake Storage que están disponibles en la suscripción. |
| Patrón del prefijo de la ruta de acceso | Ruta de acceso utilizada para escribir sus archivos en la cuenta de Data Lake Storage especificada. Puede especificar una o más instancias de las variables {date} y {time}:<br /><ul><li>Ejemplo 1: carpeta1/registros/{date}/{time}</li><li>Ejemplo 2: carpeta1/registros/{date}</li></ul><br />La marca de tiempo de la estructura de carpetas que creó usa la zona horaria UTC y no la hora local.<br /><br />Si el patrón de la ruta de acceso al archivo no contiene una barra diagonal (/) final, el patrón final de la ruta de acceso al archivo se considera un prefijo del nombre de archivo. <br /><br />Se crean nuevos archivos en las circunstancias siguientes:<ul><li>Cambio en el esquema de salida</li><li>Reinicio externo o interno de un trabajo</li></ul> |
| Formato de fecha | Opcional. Si el token de fecha se usa en la ruta de acceso de prefijo, puede seleccionar el formato de fecha en el que se organizan los archivos. Ejemplo: AAAA/MM/DD |
|Formato de hora | Opcional. Si el token de hora se usa en la ruta de acceso de prefijo, puede seleccionar el formato de hora en el que se organizan los archivos. Actualmente, el único valor admitido es HH. |
| Formato de serialización de eventos | Formato de serialización para los datos de salida. Se admiten JSON, CSV y Avro.|
| Encoding | Si usa el formato CSV o JSON, debe especificar una codificación. Por el momento, UTF-8 es el único formato de codificación compatible.|
| Delimitador | Solo se aplica para la serialización de CSV. Stream Analytics admite un número de delimitadores comunes para la serialización de datos CSV. Los valores admitidos son la coma, punto y coma, espacio, tabulador y barra vertical.|
| Formato | Solo se aplica para la serialización de JSON. La opción **Separado por líneas** especifica que en el formato de la salida cada objeto JSON está separado por una línea nueva. Si selecciona **Separado por líneas**, el objeto JSON se lee un objeto a la vez. El contenido total en sí mismo no sería un JSON válido.  La opción **Matriz** especifica que el formato de la salida es una matriz de objetos JSON. Esta matriz se cierra cuando el trabajo se detenga o Stream Analytics haya pasado a la siguiente ventana de tiempo. En general, es preferible utilizar JSON separado por líneas, ya que no requiere ningún control especial mientras todavía se esté escribiendo en el archivo de salida.|
| Modo de autenticación | Puede autorizar el acceso a su cuenta de Data Lake Storage mediante [identidades administradas](stream-analytics-managed-identities-adls.md) o un token de usuario. Una vez que concede acceso, puede revocarlo si cambia la contraseña de la cuenta de usuario, elimina la salida de Data Lake Storage para este trabajo o elimina el trabajo de Stream Analytics. |

## <a name="sql-database"></a>SQL Database

Puede usar [Azure SQL Database](https://azure.microsoft.com/services/sql-database/) como salida de datos que son relacionales por naturaleza o de aplicaciones que dependen del contenido hospedado en una base de datos relacional. Los trabajos de Stream Analytics se escriben en una tabla existente en SQL Database. El esquema de tabla debe coincidir exactamente con los campos y los tipos en la salida del trabajo. También puede especificar [Azure SQL Data Warehouse](https://azure.microsoft.com/documentation/services/sql-data-warehouse/) como salida mediante la opción de salida de SQL Database. Para obtener más información sobre cómo mejorar el rendimiento de escritura, consulte el artículo [Stream Analytics con Azure SQL Database como salida](stream-analytics-sql-output-perf.md).

También puede usar [Instancia administrada de Azure SQL Database](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance) como salida. Debe [configurar un punto de conexión público en Instancia administrada de Azure SQL Database](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-public-endpoint-configure) y, a continuación, configurar manualmente las siguientes opciones en Azure Stream Analytics. También es posible configurar manualmente los valores siguientes para una máquina virtual de Azure que ejecute SQL Server con una base de datos adjunta.

En la siguiente tabla se enumeran los nombres de propiedad y su descripción para crear una salida de SQL Database.

| Nombre de propiedad | Descripción |
| --- | --- |
| Alias de salida |Un nombre descriptivo usado en las consultas para dirigir la salida de la consulta a esta base de datos. |
| Base de datos | Nombre de la base de datos adonde envía la salida. |
| Nombre de servidor | El nombre del servidor de SQL Database. Para Instancia administrada de Azure SQL Database, es necesario especificar el puerto 3342. Por ejemplo, *sampleserver.public.database.windows.net,3342*. |
| Nombre de usuario | Nombre de usuario que tiene acceso de escritura a la base de datos. Stream Analytics admite solo la autenticación de SQL. |
| Contraseña | La contraseña para conectarse a la base de datos. |
| Tabla | El nombre de la tabla donde se escribe la salida. El nombre de la tabla distingue mayúsculas de minúsculas. El esquema de esta tabla debe coincidir exactamente con el número y tipo de los campos que genera la salida del trabajo. |
|Heredación del esquema de partición| Opción para heredar el esquema de partición del paso de consulta anterior a fin de habilitar la topología completamente paralela con múltiples escritores en la tabla. Para obtener más información, vea [Salida de Azure Stream Analytics a Azure SQL Database](stream-analytics-sql-output-perf.md).|
|Número máximo de lotes| Número máximo de registros recomendado para enviarse con cada transacción de inserción masiva.|

Hay dos adaptadores que permiten la salida de Azure Stream Analytics a Azure Synapse Analytics (anteriormente SQL Data Warehouse): SQL Database y Azure Synapse. Se recomienda elegir el adaptador de Azure Synapse Analytics en lugar del adaptador de SQL Database si alguna de las condiciones siguientes es verdadera:

* **Rendimiento**: Si el rendimiento esperado ahora o en el futuro es superior a 10 MB/s, use la opción de salida de Azure Synapse para mejorar el rendimiento.

* **Particiones de entrada**: Si tiene ocho o más particiones de entrada, use la opción de salida de Azure Synapse para mejorar la escalabilidad horizontal.

## <a name="azure-synapse-analytics-preview"></a>Azure Synapse Analytics (versión preliminar)

[Azure Synapse Analytics](https://azure.microsoft.com/services/synapse-analytics) (anteriormente SQL Data Warehouse) es un servicio de análisis ilimitado que reúne el almacenamiento de datos empresariales y el análisis de macrodatos. 

La salida de los trabajos de Azure Stream Analytics se puede generar en una tabla de grupos de SQL en Azure Synapse Analytics y pueden procesar velocidades de rendimiento de hasta 200 MB/s. Esto admite los análisis en tiempo real más exigentes y las necesidades de procesamiento de datos de ruta de acceso activa para cargas de trabajo como generación de informes y creación de paneles.  

La tabla de grupo de SQL debe existir antes de poder agregarla como salida al trabajo de Stream Analytics. El esquema de tabla debe coincidir con los campos y los tipos en la salida del trabajo. 

Para usar Azure Synapse como salida, debe asegurarse de que tiene la cuenta de almacenamiento configurada. Vaya a Configuración de cuenta de almacenamiento para configurar la cuenta de almacenamiento. Solo se permiten los tipos de cuenta de almacenamiento que admiten tablas: Uso general V2 y uso general V1.   

En la siguiente tabla se enumeran los nombres de propiedad y su descripción para crear una salida de Azure Synapse Analytics.

|Nombre de propiedad|Descripción|
|-|-|
|Alias de salida |Un nombre descriptivo usado en las consultas para dirigir la salida de la consulta a esta base de datos. |
|Base de datos |Nombre del grupo de SQL adonde envía la salida. |
|Nombre de servidor |Nombre del servidor de Azure Synapse.  |
|Nombre de usuario |Nombre de usuario que tiene acceso de escritura a la base de datos. Stream Analytics admite solo la autenticación de SQL. |
|Contraseña |La contraseña para conectarse a la base de datos. |
|Tabla  | El nombre de la tabla donde se escribe la salida. El nombre de la tabla distingue mayúsculas de minúsculas. El esquema de esta tabla debe coincidir exactamente con el número y tipo de los campos que genera la salida del trabajo.|

## <a name="blob-storage-and-azure-data-lake-gen2"></a>Blob Storage y Azure Data Lake Gen2

Data Lake Storage Gen2 convierte a Azure Storage en los cimientos para crear lagos de datos empresariales en Azure. Diseñado desde el principio para servir varios petabytes de información y mantener cientos de gigabits de rendimiento, Data Lake Storage Gen2 le ofrece una forma fácil de administrar cantidades masivas de datos. Una parte fundamental de Data Lake Storage Gen2 es la incorporación de un espacio de nombres jerárquico a Blob Storage.

Azure Blob Storage ofrece una solución rentable y escalable para almacenar grandes cantidades de datos no estructurados en la nube. Para obtener una introducción a Blob Storage y su uso, consulte [Carga, descarga y enumeración de blobs con Azure Portal](../storage/blobs/storage-quickstart-blobs-portal.md).

En la siguiente tabla se enumeran los nombres de propiedad y su descripción para crear un blob o una salida de ADLS Gen2.

| Nombre de propiedad       | Descripción                                                                      |
| ------------------- | ---------------------------------------------------------------------------------|
| Alias de salida        | Un nombre descriptivo usado en las consultas para dirigir la salida de la consulta a este almacenamiento de blobs. |
| Cuenta de almacenamiento     | Nombre de la cuenta de almacenamiento a donde está enviando la salida.               |
| Clave de cuenta de almacenamiento | La clave secreta asociada con la cuenta de almacenamiento.                              |
| Contenedor de almacenamiento   | Agrupación lógica de los blobs almacenados en Azure Blob service. Cuando carga un blob a Blob service, debe especificar un contenedor para ese blob. |
| Patrón de la ruta de acceso | Opcional. Patrón de la ruta de acceso al archivo que se usa para escribir los blobs dentro del contenedor especificado. <br /><br /> En el patrón de la ruta de acceso, puede optar por usar una o más instancias de las variables de fecha y hora para especificar la frecuencia con la que se escriben los blobs: <br /> {date}, {time} <br /><br />Puede usar la creación de particiones de blobs personalizada para especificar un nombre {field} personalizado de los datos de eventos para crear particiones de los blobs. El nombre del campo es alfanumérico y puede incluir espacios, guiones y caracteres de subrayado. Entre las restricciones en los campos personalizados se incluyen las siguientes: <ul><li>Los nombres de campo no distinguen entre mayúsculas y minúsculas. Por ejemplo, el servicio no puede diferenciar entre la columna "ID" y la columna "id".</li><li>No se permiten los campos anidados. En su lugar, utilice un alias en la consulta del trabajo para "aplanar" el campo.</li><li>No pueden usarse expresiones como nombre de campo.</li></ul> <br />Esta característica también permite usar configuraciones del especificador de un formato de fecha y hora personalizado en la ruta de acceso. Los formatos de fecha y hora personalizados se deben especificar de uno en uno, delimitado por la palabra clave {datetime:\<especificador >}. \<especificador> permite el uso de aaaa, MM, M, dd, d, HH, H, mm, m, ss o s. La palabra clave {datetime:\<especificador >} se puede utilizar varias veces en la ruta de acceso para formar configuraciones de fecha y hora personalizadas. <br /><br />Ejemplos: <ul><li>Ejemplo 1: cluster1/logs/{date}/{time}</li><li>Ejemplo 2: cluster1/logs/{date}</li><li>Ejemplo 3: cluster1/{client_id}/{date}/{time}</li><li>Ejemplo 4: cluster1/{datetime:ss}/{myField}, donde la consulta es: SELECT data.myField AS myField FROM Input;</li><li>Ejemplo 5: cluster1/year={datetime:yyyy}/month={datetime:MM}/day={datetime:dd}</ul><br />La marca de tiempo de la estructura de carpetas que creó usa la zona horaria UTC y no la hora local.<br /><br />La nomenclatura de los archivos usa la siguiente convención: <br /><br />{Patrón del prefijo de la ruta de acceso}/schemaHashcode_Guid_Number.extension<br /><br />Archivos de salida de ejemplo:<ul><li>Myoutput/20170901/00/45434_gguid_1.csv</li>  <li>Myoutput/20170901/01/45434_gguid_1.csv</li></ul> <br />Para obtener más información acerca de esta característica, consulte [Particionamiento de la salida de blobs personalizada en Azure Stream Analytics](stream-analytics-custom-path-patterns-blob-storage-output.md). |
| Formato de fecha | Opcional. Si el token de fecha se usa en la ruta de acceso de prefijo, puede seleccionar el formato de fecha en el que se organizan los archivos. Ejemplo: AAAA/MM/DD |
| Formato de hora | Opcional. Si el token de hora se usa en la ruta de acceso de prefijo, puede seleccionar el formato de hora en el que se organizan los archivos. Actualmente, el único valor admitido es HH. |
| Formato de serialización de eventos | Formato de serialización para los datos de salida. JSON, CSV, Avro y Parquet son compatibles. |
|Número mínimo de filas (solo Parquet)|Número mínimo de filas por lote. En el caso de Parquet, cada lote creará un archivo. El valor predeterminado actual es 2000 filas y el máximo permitido es 10 000 filas.|
|Tiempo máximo (solo Parquet)|Tiempo de espera máximo por lote. Después de este tiempo, el lote se escribirá en la salida aunque no se cumpla el requisito de filas mínimas. El valor predeterminado actual es 1 minuto y el máximo permitido es 2 horas. Si la salida del blob tiene una frecuencia de patrón de ruta de acceso, el tiempo de espera no puede ser mayor que el intervalo de tiempo de la partición.|
| Encoding    | Si usa el formato CSV o JSON, debe especificar una codificación. Por el momento, UTF-8 es el único formato de codificación compatible. |
| Delimitador   | Solo se aplica para la serialización de CSV. Stream Analytics admite un número de delimitadores comunes para la serialización de datos CSV. Los valores admitidos son la coma, punto y coma, espacio, tabulador y barra vertical. |
| Formato      | Solo se aplica para la serialización de JSON. La opción **Separado por líneas** especifica que en el formato de la salida cada objeto JSON está separado por una línea nueva. Si selecciona **Separado por líneas**, el objeto JSON se lee un objeto a la vez. El contenido total en sí mismo no sería un JSON válido. La opción **Matriz** especifica que el formato de la salida es una matriz de objetos JSON. Esta matriz se cierra cuando el trabajo se detenga o Stream Analytics haya pasado a la siguiente ventana de tiempo. En general, es preferible utilizar JSON separado por líneas, ya que no requiere ningún control especial mientras todavía se esté escribiendo en el archivo de salida. |

Cuando usa el almacenamiento de blobs como salida, se crea un nuevo archivo en el blob en los siguientes casos:

* Si el archivo excede el número máximo de bloques permitidos (actualmente, 50 000). Puede alcanzar el número máximo permitido de bloques sin que se alcance el tamaño máximo permitido de blobs. Por ejemplo, si la velocidad de salida es alta, puede ver más bytes por bloque y el tamaño de archivo es mayor. Si la velocidad de salida es baja, cada bloque tiene menos datos y el tamaño de archivo es menor.
* Si hay un cambio de esquema en la salida y el formato de salida requiere un esquema fijo (CSV y Avro).
* Si se reinicia un trabajo, ya sea externamente por un usuario que lo detiene e inicia, o internamente para el mantenimiento del sistema o la recuperación tras un error.
* Si la consulta está completamente particionada y se crea un nuevo archivo para cada partición de salida.
* Si el usuario elimina un archivo o un contenedor de la cuenta de almacenamiento.
* Si la salida está particionada por tiempo mediante el patrón de prefijo de ruta de acceso y se usa un nuevo blob cuando la consulta pasa a la hora siguiente.
* Si la salida tiene particiones por un campo personalizado y se crea un nuevo blob por clave de partición, si no existe.
* Si la salida tiene particiones por un campo personalizado, donde la cardinalidad de clave de partición supera el valor de 8000, y se puede crear un nuevo blob por clave de partición.

## <a name="event-hubs"></a>Event Hubs

El servicio [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/) es un agente de ingesta de eventos de publicación-suscripción altamente escalable. Puede recopilar millones de eventos por segundo. Un uso del centro de eventos como salida es cuando la salida de un trabajo de Stream Analytics se convierte en la entrada de otro trabajo de streaming. Para obtener información sobre el tamaño máximo del mensaje y la optimización del tamaño del lote, consulte la sección [Tamaño de lote de salida](#output-batch-size).

Necesita unos cuantos parámetros para configurar los flujos de datos de centros de eventos como salida.

| Nombre de propiedad | Descripción |
| --- | --- |
| Alias de salida | Nombre descriptivo usado en las consultas para dirigir la salida de la consulta a este centro de eventos. |
| Espacio de nombres del centro de eventos | Contenedor para un conjunto de entidades de mensajería. Cuando crea un nuevo centro de eventos, también se crea un espacio de nombres de centro de eventos. |
| Nombre del centro de eventos | Nombre de la salida del centro de eventos. |
| Nombre de la directiva del centro de eventos | Directiva de acceso compartido, que puede crear en la pestaña **Configurar** del centro de eventos. Cada directiva de acceso compartido tiene un nombre, los permisos establecidos y las claves de acceso. |
| Clave de la directiva de centro de eventos | Clave de acceso compartido usada para autenticar el acceso al espacio de nombres del centro de eventos. |
| Columna de clave de partición | Opcional. Columna que contiene la clave de partición para la salida del centro de eventos. |
| Formato de serialización de eventos | Formato de serialización para los datos de salida. Se admiten JSON, CSV y Avro. |
| Encoding | Por el momento, UTF-8 es el único formato de codificación compatible para CSV y JSON. |
| Delimitador | Solo se aplica para la serialización de CSV. Stream Analytics admite un número de delimitadores comunes para la serialización de datos en formato CSV. Los valores admitidos son la coma, punto y coma, espacio, tabulador y barra vertical. |
| Formato | Solo se aplica para la serialización de JSON. La opción **Separado por líneas** especifica que en el formato de la salida cada objeto JSON está separado por una línea nueva. Si selecciona **Separado por líneas**, el objeto JSON se lee un objeto a la vez. El contenido total en sí mismo no sería un JSON válido. La opción **Matriz** especifica que el formato de la salida es una matriz de objetos JSON.  |
| Columnas de propiedades | Opcional. Columnas separadas por comas que necesitan agregarse como propiedades de usuario del mensaje saliente, en lugar de la carga útil. Puede obtener más información sobre esta característica en la sección [Propiedades de metadatos personalizadas para la salida](#custom-metadata-properties-for-output). |

## <a name="power-bi"></a>Power BI

Puede usar [Power BI](https://powerbi.microsoft.com/) como salida para un trabajo de Stream Analytics a fin de ofrecer una amplia experiencia de visualización de los resultados del análisis. Puede usar esta funcionalidad con paneles operativos, generación de informes e informes basados en métricas.

La salida de Power BI desde Stream Analytics no está disponible actualmente en las regiones de Azure China (21Vianet) y Azure Alemania (T-Systems International).

En la siguiente tabla se encuentra una lista de nombres de propiedades y su descripción para configurar la salida de Power BI.

| Nombre de propiedad | Descripción |
| --- | --- |
| Alias de salida |Proporcione un nombre descriptivo que se usará en las consultas para dirigir la salida de la consulta a esta salida de Power BI. |
| Área de trabajo de grupo |Para habilitar el uso compartido de datos con otros usuarios de Power BI, puede seleccionar grupos dentro de su cuenta de Power BI o elegir **Mi área de trabajo** si no quiere escribir en un grupo. Actualizar un grupo existente requiere renovar la autenticación de Power BI. |
| Nombre del conjunto de datos |Proporcione el nombre de conjunto de datos que quiere que tenga la salida de Power BI. |
| Nombre de la tabla |Proporcione un nombre de tabla en el conjunto de datos de la salida de Power BI. Actualmente, la salida de Power BI de trabajos de Stream Analytics solo puede tener una tabla en un conjunto de datos. |
| Autorización de la conexión | Debe realizar la autorización con Power BI para configurar los valores de salida. Una vez que esta salida obtiene acceso a su panel de Power BI, puede revocarlo si cambia la contraseña de la cuenta de usuario, elimina la salida del trabajo o elimina el trabajo de Stream Analytics. | 

Para ver un tutorial sobre la configuración de una salida y panel de Power BI, consulte [Azure Stream Analytics y Power BI](stream-analytics-power-bi-dashboard.md).

> [!NOTE]
> No cree explícitamente el conjunto de datos y la tabla en el panel de Power BI. El conjunto de datos y la tabla se rellenan automáticamente cuando se inicia el trabajo y este comienza a enviar salidas a Power BI. Si la consulta de trabajo no genera ningún resultado, el conjunto de datos y la tabla no se crean. Si Power BI ya cuenta con un conjunto de datos y una tabla con el mismo nombre que el proporcionado en este trabajo de Stream Analytics, se sobrescriben los datos existentes.
>

### <a name="create-a-schema"></a>Creación de un esquema
Azure Stream Analytics crea un esquema de tabla y un conjunto de datos de Power BI para el usuario si todavía no existen. En todos los demás casos, la tabla se actualiza con los nuevos valores. Actualmente, solo puede existir una tabla dentro de un conjunto de datos. 

Power BI utiliza la directiva de retención primero en entrar, primero en salir (FIFO). Se recopilan los datos en una tabla hasta que alcance las 200 000 filas.

### <a name="convert-a-data-type-from-stream-analytics-to-power-bi"></a>Conversión de un tipo de datos de Stream Analytics a Power BI
Azure Stream Analytics actualiza el modelo de datos dinámicamente en tiempo de ejecución si cambia el esquema de salida. Se realiza un seguimiento de los cambios de nombre de columna, los cambios de tipo de columna y la adición o eliminación de columnas.

Esta tabla cubre las conversiones de tipos de datos de [tipos de datos de Stream Analytics](https://docs.microsoft.com/stream-analytics-query/data-types-azure-stream-analytics) a [tipos de Entity Data Model (EDM)](https://docs.microsoft.com/dotnet/framework/data/adonet/entity-data-model) de Power BI si no existen una tabla y un conjunto de datos de Power BI.

De Stream Analytics | A Power BI
-----|-----
bigint | Int64
nvarchar(max) | String
datetime | Datetime
FLOAT | Double
Matriz de registro | Tipo cadena, valor constante "IRecord" o "IArray"

### <a name="update-the-schema"></a>Revisión del esquema
Stream Analytics deduce el esquema de modelo de datos basándose en el primer conjunto de eventos en la salida. Más adelante, si es necesario, el esquema de modelo de datos se actualiza para dar cabida a los eventos entrantes que pueden coincidir con el esquema original.

Evite la consulta `SELECT *` para impedir la actualización de esquema dinámica en las filas. Además de las posibles implicaciones de rendimiento, podría dar lugar a incertidumbre respecto al tiempo necesario para los resultados. Seleccione los campos exactos que necesitan mostrarse en el panel de Power BI. Además, los valores de datos deben ser compatibles con el tipo de datos elegido.


Anterior o actual | Int64 | String | Datetime | Double
-----------------|-------|--------|----------|-------
Int64 | Int64 | String | String | Double
Double | Double | String | String | Double
String | String | String | String | String 
Datetime | String | String |  Datetime | String

## <a name="table-storage"></a>Almacenamiento de tablas

[almacenamiento de tablas de Azure](../storage/common/storage-introduction.md) ofrece un tipo de almacenamiento de alta disponibilidad y escalabilidad masiva, de forma que las aplicaciones pueden escalarse automáticamente para ajustarse a la demanda de los usuarios. Table Storage es un almacén de claves/atributos NoSQL de Microsoft que puede usar para datos estructurados con menos restricciones en el esquema. El almacenamiento de tablas de Azure puede usarse para almacenar datos con de persistencia y recuperación eficaz.

En la siguiente tabla se enumeran los nombres de propiedad y su descripción para crear una salida de tabla.

| Nombre de propiedad | Descripción |
| --- | --- |
| Alias de salida |Un nombre descriptivo usado en las consultas para dirigir la salida de la consulta a este almacenamiento de tablas. |
| Cuenta de almacenamiento |Nombre de la cuenta de almacenamiento a donde está enviando la salida. |
| Clave de cuenta de almacenamiento |La clave de acceso asociada con la cuenta de almacenamiento. |
| Nombre de la tabla |Nombre de la tabla. Se crea la tabla si no existe. |
| Clave de partición |Nombre de la columna de salida que contiene la clave de partición. La clave de partición es un identificador único de la partición dentro de una tabla que constituye la primera parte de la clave principal de la entidad. Es un valor de cadena que puede tener un tamaño de hasta 1 KB. |
| Clave de fila |Nombre de la columna de salida que contiene la clave de fila. La clave de fila es un identificador único de una entidad dentro de una partición. Forma la segunda parte de la clave principal de la entidad. La clave de fila es un valor de cadena que puede tener un tamaño de hasta 1 KB. |
| Tamaño de lote |El número de registros para una operación por lotes. El valor predeterminado (100) es suficiente para la mayoría de los trabajos. Para obtener más información sobre la modificación de esta configuración, consulte [Especificaciones de la operación por lotes de tablas](https://docs.microsoft.com/java/api/com.microsoft.azure.storage.table.tablebatchoperation). |

## <a name="service-bus-queues"></a>Colas de Service Bus

Las [colas de Service Bus](../service-bus-messaging/service-bus-queues-topics-subscriptions.md) ofrecen una entrega de mensajes según el modelo FIFO a uno o más consumidores simultáneos. Normalmente, los receptores reciben y procesan los mensajes en el orden temporal en el que se agregaron a la cola. Un único consumidor de mensajes recibe y procesa cada mensaje.

En el [nivel de compatibilidad 1.2](stream-analytics-compatibility-level.md), Azure Stream Analytics usa el protocolo de mensajería [Advanced Message Queuing Protocol (AMQP)](../service-bus-messaging/service-bus-amqp-overview.md) para escribir en colas y temas de Service Bus. AMQP le permite construir aplicaciones híbridas, entre plataformas, utilizando un protocolo estándar abierto.

En la siguiente tabla se enumeran los nombres de propiedad y su descripción para crear una salida de cola.

| Nombre de propiedad | Descripción |
| --- | --- |
| Alias de salida |Nombre descriptivo usado en las consultas para dirigir la salida de la consulta a esta cola de Service Bus. |
| Espacio de nombres de Service Bus |Contenedor para un conjunto de entidades de mensajería. |
| Nombre de cola |Nombre de la cola de Service Bus. |
| Nombre de la directiva de colas |Cuando crea una cola, también puede crear directivas de acceso compartidas en la pestaña **Configurar** de dicha cola. Cada directiva de acceso compartido tiene un nombre, los permisos establecidos y las claves de acceso. |
| Clave de la directiva de colas |La clave de acceso compartido que se usa para autenticar el acceso al espacio de nombres de Service Bus. |
| Formato de serialización de eventos |Formato de serialización para los datos de salida. Se admiten JSON, CSV y Avro. |
| Encoding |Por el momento, UTF-8 es el único formato de codificación compatible para CSV y JSON. |
| Delimitador |Solo se aplica para la serialización de CSV. Stream Analytics admite un número de delimitadores comunes para la serialización de datos en formato CSV. Los valores admitidos son la coma, punto y coma, espacio, tabulador y barra vertical. |
| Formato |Solo se aplica para el tipo JSON. La opción **Separado por líneas** especifica que en el formato de la salida cada objeto JSON está separado por una línea nueva. Si selecciona **Separado por líneas**, el objeto JSON se lee un objeto a la vez. El contenido total en sí mismo no sería un JSON válido. La opción **Matriz** especifica que el formato de la salida es una matriz de objetos JSON. |
| Columnas de propiedades | Opcional. Columnas separadas por comas que necesitan agregarse como propiedades de usuario del mensaje saliente, en lugar de la carga útil. Puede obtener más información sobre esta característica en la sección [Propiedades de metadatos personalizadas para la salida](#custom-metadata-properties-for-output). |
| Columnas de propiedades del sistema | Opcional. Pares de clave-valor de las propiedades del sistema y los nombres de columna correspondientes que se deben adjuntar al mensaje saliente en lugar de a la carga. En la sección [Propiedades del sistema para las salidas de cola y tema de Service Bus](#system-properties-for-service-bus-queue-and-topic-outputs) hay más información sobre esta característica.  |

El número de particiones se [basa en la SKU y el tamaño de Service Bus](../service-bus-messaging/service-bus-partitioning.md). La clave de partición es un valor entero único para cada partición.

## <a name="service-bus-topics"></a>Temas de Service Bus
Las colas de Service Bus proporcionan un método de comunicación uno a uno del emisor al receptor. Los [temas de Service Bus](https://msdn.microsoft.com/library/azure/hh367516.aspx) proporcionan una forma de comunicación de uno a varios.

En la siguiente tabla se enumeran los nombres de propiedad y su descripción para crear una salida de tema de Service Bus.

| Nombre de propiedad | Descripción |
| --- | --- |
| Alias de salida |Nombre descriptivo usado en las consultas para dirigir la salida de la consulta a este tema de Service Bus. |
| Espacio de nombres de Service Bus |Contenedor para un conjunto de entidades de mensajería. Al crear un nuevo centro de eventos, también se crea un espacio de nombres de Service Bus. |
| Nombre del tema |Los temas son entidades de mensajería, similares a los centros de eventos y las colas. Están diseñados para recopilar secuencias de eventos de dispositivos y servicios. Cuando se crea un tema, se proporciona también un nombre específico. Los mensajes enviados a un tema no están disponibles a menos que se cree una suscripción, así que asegúrese de que hay una o varias suscripciones en el tema. |
| Nombre de la directiva de temas |Cuando crea un tema de Service Bus, también puede crear directivas de acceso compartido en la pestaña **Configurar** del tema. Cada directiva de acceso compartido tiene un nombre, los permisos establecidos y las claves de acceso. |
| Clave de la directiva de temas |La clave de acceso compartido que se usa para autenticar el acceso al espacio de nombres de Service Bus. |
| Formato de serialización de eventos |Formato de serialización para los datos de salida. Se admiten JSON, CSV y Avro. |
| Encoding |Si usa el formato CSV o JSON, debe especificar una codificación. Por el momento, UTF-8 es el único formato de codificación compatible. |
| Delimitador |Solo se aplica para la serialización de CSV. Stream Analytics admite un número de delimitadores comunes para la serialización de datos en formato CSV. Los valores admitidos son la coma, punto y coma, espacio, tabulador y barra vertical. |
| Columnas de propiedades | Opcional. Columnas separadas por comas que necesitan agregarse como propiedades de usuario del mensaje saliente, en lugar de la carga útil. Puede obtener más información sobre esta característica en la sección [Propiedades de metadatos personalizadas para la salida](#custom-metadata-properties-for-output). |
| Columnas de propiedades del sistema | Opcional. Pares de clave-valor de las propiedades del sistema y los nombres de columna correspondientes que se deben adjuntar al mensaje saliente en lugar de a la carga. En la sección [Propiedades del sistema para las salidas de cola y tema de Service Bus](#system-properties-for-service-bus-queue-and-topic-outputs) hay más información sobre esta característica. |

El número de particiones se [basa en la SKU y el tamaño de Service Bus](../service-bus-messaging/service-bus-partitioning.md). La clave de partición es un valor entero único para cada partición.

## <a name="azure-cosmos-db"></a>Azure Cosmos DB
[Azure Cosmos DB](https://azure.microsoft.com/services/documentdb/) es un servicio de base de datos distribuido globalmente que ofrece un escalado ilimitado en todo el mundo, consultas avanzadas y una funcionalidad de indexación automática a partir de modelos de datos independientes del esquema. Para conocer las opciones de contenedor de Azure Cosmos DB para Stream Analytics, consulte el artículo [Stream Analytics con Azure Cosmos DB como salida](stream-analytics-documentdb-output.md).

La salida de Azure Cosmos DB desde Stream Analytics no está disponible actualmente en las regiones de Azure China (21Vianet) y Azure Alemania (T-Systems International).

> [!Note]
> En este momento, Azure Stream Analytics solo admite la conexión a Azure Cosmos DB mediante la API de SQL.
> Aún no se admiten otras API de Azure Cosmos DB. Si apunta Azure Stream Analytics a las cuentas de Azure Cosmos DB creadas con otras API, puede que los datos no se almacenen correctamente.

En la tabla siguiente se describen las propiedades para crear una salida de Azure Cosmos DB.

| Nombre de propiedad | Descripción |
| --- | --- |
| Alias de salida | Un alias para hacer referencia a esta salida en la consulta de Stream Analytics. |
| Receptor | Azure Cosmos DB. |
| Opción de importación | Elija **Select Cosmos DB from your subscription** (Seleccionar Cosmos DB desde la suscripción) o **Proporcionar la configuración de Cosmos DB manualmente**.
| Identificador de cuenta | El nombre o el URI del punto de conexión de la cuenta de Azure Cosmos DB. |
| Clave de cuenta | La clave de acceso compartido para la cuenta de Azure Cosmos DB. |
| Base de datos | El nombre de la base de datos de Azure Cosmos DB. |
| Nombre del contenedor | Nombre del contenedor que va a usarse, el cual debe existir en Cosmos DB. Ejemplo:  <br /><ul><li> _MyContainer_: debe existir un contenedor llamado "MyContainer".</li>|
| Id. de documento |Opcional. Nombre del campo de los eventos de salida utilizado para especificar la clave principal en la que se basan las operaciones de inserción o actualización.

## <a name="azure-functions"></a>Azure Functions
Azure Functions es un servicio de proceso sin servidor que puede usar para ejecutar código a petición sin necesidad de aprovisionar ni administrar explícitamente la infraestructura. Permite implementar el código desencadenado por los eventos que se producen en servicios de Azure o de sus socios. Esta capacidad que tiene Azure Functions para responder a los desencadenadores hace que sea una salida natural para Azure Stream Analytics. Este adaptador de salida permite que los usuarios conecten Stream Analytics con Azure Functions y ejecuten un script o parte de un código como respuesta a una variedad de eventos.

La salida de Azure Functions desde Stream Analytics no está disponible actualmente en las regiones de Azure China (21Vianet) y Azure Alemania (T-Systems International).

Azure Stream Analytics invoca a Azure Functions a través de desencadenadores HTTP. El adaptador de salida de Azure Functions está disponible con las siguientes propiedades configurables:

| Nombre de propiedad | Descripción |
| --- | --- |
| Aplicación de función |Nombre de la aplicación de Azure Functions. |
| Función |Nombre de la función en la aplicación de Azure Functions. |
| Clave |Si quiere usar una instancia de Azure Functions desde otra suscripción, debe proporcionar la clave para acceder a la función. |
| Tamaño máximo de lote |Propiedad que permite establecer el tamaño máximo de cada lote de salida que se envía a la instancia de Azure Functions. La unidad de entrada se muestra en bytes. De manera predeterminada, este valor es 262 144 bytes (256 KB). |
| Número máximo de lotes  |Propiedad que permite especificar el número máximo de eventos en cada lote que se envía a Azure Functions. El valor predeterminado es 100. |

Azure Stream Analytics espera el estado de HTTP 200 de la aplicación de Functions para los lotes que se procesaron correctamente.

Cuando Azure Stream Analytics recibe una excepción 413 ("La entidad de solicitud HTTP es demasiado grande") de una función de Azure, disminuye el tamaño de los lotes que envía a Azure Functions. En el código de función de Azure, use esta excepción para asegurarse de que Azure Stream Analytics no envíe lotes demasiado grandes. También, asegúrese de que los valores de tamaño y número máximo de lotes que se usan en la función sean coherentes con los valores que se especifican en el portal de Stream Analytics.

> [!NOTE]
> Durante la conexión de prueba, Stream Analytics envía un lote vacío a Azure Functions para probar si la conexión entre los dos funciona. Asegúrese de que la aplicación de Functions controla las solicitudes por lotes vacías para asegurarse de que la conexión de prueba es correcta.

También tenga en cuenta que no se genera ninguna salida en las situaciones en las que no se producen eventos en un período de tiempo. Como resultado, no se llama a la función **computeResult**. Este comportamiento es coherente con las funciones integradas de agregado en ventanas.

## <a name="custom-metadata-properties-for-output"></a>Propiedades de metadatos personalizadas para la salida 

Puede asociar las columnas de la consulta como propiedades de usuario a los mensajes salientes. Estas columnas no se envían a la carga útil. Las propiedades están presentes como diccionario en el mensaje de salida. *Key* es el nombre de columna y *value* es el valor de columna en el diccionario de propiedades. Se admiten todos los tipos de datos de Stream Analytics, excepto los tipos de registro y matriz.  

Salidas compatibles: 
* Cola de Service Bus 
* Tema de Service Bus 
* Centro de eventos 

En el ejemplo siguiente, agregamos los dos campos `DeviceId` y `DeviceStatus` a los metadatos. 
* Consulta: `select *, DeviceId, DeviceStatus from iotHubInput`
* Configuración de salida: `DeviceId,DeviceStatus`

![Columnas de propiedades](./media/stream-analytics-define-outputs/10-stream-analytics-property-columns.png)

La siguiente captura de pantalla muestra las propiedades del mensaje de salida en el centro de eventos mediante el [explorador de Service Bus](https://github.com/paolosalvatori/ServiceBusExplorer).

![Propiedades de evento personalizadas](./media/stream-analytics-define-outputs/09-stream-analytics-custom-properties.png)

## <a name="system-properties-for-service-bus-queue-and-topic-outputs"></a>Propiedades del sistema para las salidas de cola y tema de Service Bus 
Puede adjuntar columnas de consulta como [propiedades del sistema](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.brokeredmessage?view=azure-dotnet#properties) a los mensajes salientes de cola y tema de Service Bus. Estas columnas no entran en la carga; en su lugar, la [propiedad del sistema](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.brokeredmessage?view=azure-dotnet#properties) BrokeredMessage correspondiente se rellena con los valores de la columna de consulta.
Se admiten estas propiedades del sistema: `MessageId, ContentType, Label, PartitionKey, ReplyTo, SessionId, CorrelationId, To, ForcePersistence, TimeToLive, ScheduledEnqueueTimeUtc`.
Los valores de cadena de estas columnas se analizan como el tipo de valor de propiedad del sistema correspondiente y los errores de análisis se tratan como errores de datos.
Este campo se proporciona como un formato de objeto JSON. Los detalles sobre este formato son los siguientes:
* Rodeado de llaves {}.
* Escrito en pares de clave-valor.
* Las claves y los valores deben ser cadenas.
* La clave es el nombre de la propiedad del sistema y el valor es el nombre de la columna de consulta.
* Las claves y los valores se separan por el signo de dos puntos.
* Los pares de clave-valor están separados entre ellos por una coma.

A continuación se muestra cómo utilizar esta propiedad:

* Consulta: `select *, column1, column2 INTO queueOutput FROM iotHubInput`
* Columnas de propiedades del sistema: `{ "MessageId": "column1", "PartitionKey": "column2"}`

Esto establece `MessageId` en los mensajes de cola de Service Bus con los valores de `column1` y PartitionKey se establece con los valores de `column2`.

## <a name="partitioning"></a>Creación de particiones

En la tabla siguiente se resume la asistencia de la partición y el número de redactores de salida para cada tipo de salida:

| Tipo de salida | Compatibilidad con particiones | Clave de partición  | Número de redactores de salida |
| --- | --- | --- | --- |
| Azure Data Lake Store | Sí | Use los tokens {date} y {time} en el patrón de prefijo de ruta de acceso. Elija el formato de fecha, como AAAA/MM/DD, DD/MM/AAAA o MM-DD-AAAA. HH se usa para el formato de hora. | Sigue las particiones de entrada para [consultas que se pueden paralelizar totalmente](stream-analytics-scale-jobs.md). |
| Azure SQL Database | Sí, debe habilitarse. | Se basa en la cláusula PARTITION BY de la consulta. | Cuando se habilita la opción para heredar particiones, se siguen las particiones de entrada para [las consultas que se pueden paralelizar totalmente](stream-analytics-scale-jobs.md). Para obtener más información sobre cómo conseguir un mejor rendimiento de escritura al cargar datos en Azure SQL Database, consulte [Salida de Azure Stream Analytics a Azure SQL Database](stream-analytics-sql-output-perf.md). |
| Azure Blob Storage | Sí | Use los tokens de {date} y {time} de los campos de evento del patrón de la ruta de acceso. Elija el formato de fecha, como AAAA/MM/DD, DD/MM/AAAA o MM-DD-AAAA. HH se usa para el formato de hora. La salida de los blobs puede particionarse con un solo atributo de evento personalizado {fieldname} o {datetime:\<especificador>}. | Sigue las particiones de entrada para [consultas que se pueden paralelizar totalmente](stream-analytics-scale-jobs.md). |
| Azure Event Hubs | Sí | Sí | Varía según alineación de particiones.<br /> Cuando la clave de partición para la salida del centro de eventos está alineada equitativamente con el paso de consulta ascendente (anterior), el número de sistemas de escritura es el mismo que el número de particiones en la salida del centro de eventos. Cada sistema de escritura usa la [clase EventHubSender](/dotnet/api/microsoft.servicebus.messaging.eventhubsender?view=azure-dotnet) para enviar eventos a la partición específica. <br /> Cuando la clave de partición para la salida del centro de eventos no está alineada con el paso de consulta ascendente (anterior), el número de sistemas de escritura es el mismo que el número de particiones del paso anterior. Cada sistema de escritura usa [la clase SendBatchAsync](/dotnet/api/microsoft.servicebus.messaging.eventhubclient.sendasync?view=azure-dotnet) de **EventHubClient** para enviar eventos a todas las particiones de salida. |
| Power BI | No | None | No aplicable. |
| Almacenamiento de tablas de Azure | Sí | Cualquier columna de resultados.  | Sigue las particiones de entrada para [consultas totalmente paralelizadas](stream-analytics-scale-jobs.md). |
| Tema de Azure Service Bus | Sí | Se elige automáticamente. El número de particiones se basa en [la SKU y el tamaño de Service Bus](../service-bus-messaging/service-bus-partitioning.md). La clave de partición es un valor entero único para cada partición.| Igual que el número de particiones en el tema de salida.  |
| Cola de Azure Service Bus | Sí | Se elige automáticamente. El número de particiones se basa en [la SKU y el tamaño de Service Bus](../service-bus-messaging/service-bus-partitioning.md). La clave de partición es un valor entero único para cada partición.| Igual que el número de particiones en la cola de salida. |
| Azure Cosmos DB | Sí | Se basa en la cláusula PARTITION BY de la consulta. | Sigue las particiones de entrada para [consultas totalmente paralelizadas](stream-analytics-scale-jobs.md). |
| Azure Functions | Sí | Se basa en la cláusula PARTITION BY de la consulta. | Sigue las particiones de entrada para [consultas totalmente paralelizadas](stream-analytics-scale-jobs.md). |

También se puede controlar el número de sistemas de escritura de salida mediante la cláusula `INTO <partition count>` (consulte [INTO](https://docs.microsoft.com/stream-analytics-query/into-azure-stream-analytics#into-shard-count)) de la consulta, que puede ser útil para lograr la topología de trabajo deseada. Si el adaptador de salida no tiene particiones, la falta de datos de una partición de entrada provocará un retraso hasta la cantidad de llegada tardía de tiempo. En tales casos, la salida se combina en un único sistema de escritura, lo que puede provocar cuellos de botella en la canalización. Para obtener más información sobre la directiva de llegadas tardías, consulte [Puntos a tener en cuenta sobre el orden de eventos de Azure Stream Analytics](stream-analytics-out-of-order-and-late-events.md).

## <a name="output-batch-size"></a>Tamaño de lote de salida
Azure Stream Analytics usa lotes de tamaño variable para procesar eventos y escribir en las salidas. Normalmente, el motor de Stream Analytics no escribe un mensaje a la vez y utiliza lotes para mejorar la eficacia. Cuando la velocidad de los eventos entrantes y salientes es alta, Stream Analytics utiliza lotes más grandes. Cuando la tasa de salida es baja, utiliza lotes más pequeños para mantener baja la latencia.

La siguiente tabla explica algunas de las consideraciones para el procesamiento por lotes de salida:

| Tipo de salida |    Tamaño máximo de mensaje | Optimización de tamaño de lote |
| :--- | :--- | :--- |
| Azure Data Lake Store | Consulte los [límites de Data Lake Storage](../azure-resource-manager/management/azure-subscription-service-limits.md#data-lake-store-limits). | Use hasta 4 MB por operación de escritura. |
| Azure SQL Database | Se puede configurar mediante el número máximo de lotes. De forma predeterminada, 10 000 filas como máximo y 100 como mínimo por cada inserción masiva.<br />Consulte los [límites de Azure SQL](../sql-database/sql-database-resource-limits.md). |  Inicialmente, todos los lotes se insertan de forma masiva con el número máximo de lotes. El lote se divide por la mitad (hasta alcanzar el número mínimo de lotes) según los errores con posibilidad de reintento de SQL. |
| Azure Blob Storage | Consulte los [límites de Azure Storage](../azure-resource-manager/management/azure-subscription-service-limits.md#storage-limits). | El tamaño máximo del bloque de blobs es de 4 MB.<br />El número máximo del bloque de blobs es 50 000. |
| Azure Event Hubs    | 256 KB o 1 MB por mensaje. <br />Consulte los [límites de Event Hubs](../event-hubs/event-hubs-quotas.md). |    Cuando las particiones de entrada-salida no se alinean, cada evento se empaqueta individualmente en `EventData` y se envía en un lote que tiene como límite el tamaño máximo de mensaje. Esto también sucede si se usan las [propiedades de metadatos personalizadas](#custom-metadata-properties-for-output). <br /><br />  Cuando las particiones de entrada-salida se alinean, varios eventos se empaquetan en una misma instancia de `EventData` con el tamaño máximo de mensaje como límite y se envían.    |
| Power BI | Consulte los [límites de la API REST de Power BI](https://msdn.microsoft.com/library/dn950053.aspx). |
| Almacenamiento de tablas de Azure | Consulte los [límites de Azure Storage](../azure-resource-manager/management/azure-subscription-service-limits.md#storage-limits). | El valor predeterminado es 100 entidades por cada transacción. Se puede configurar con un valor menor según sea necesario. |
| Cola de Azure Service Bus    | 256 KB por mensaje para el nivel estándar, 1 MB para el nivel Premium.<br /> Consulte los [límites de Service Bus](../service-bus-messaging/service-bus-quotas.md). | Use un evento único por mensaje. |
| Tema de Azure Service Bus | 256 KB por mensaje para el nivel estándar, 1 MB para el nivel Premium.<br /> Consulte los [límites de Service Bus](../service-bus-messaging/service-bus-quotas.md). | Use un evento único por mensaje. |
| Azure Cosmos DB    | Consulte los [límites de Azure Cosmos DB](../azure-resource-manager/management/azure-subscription-service-limits.md#azure-cosmos-db-limits). | El tamaño de lote y la frecuencia de escritura se ajustan dinámicamente según las respuestas de Azure Cosmos DB. <br /> No existen limitaciones predeterminadas para Stream Analytics. |
| Azure Functions    | | El tamaño predeterminado de lote es de 262 144 bytes (256 KB). <br /> El número predeterminado de eventos por lote es 100. <br /> El tamaño del lote es configurable y puede aumentar o disminuir en las [opciones de salida](#azure-functions) de Stream Analytics.

## <a name="next-steps"></a>Pasos siguientes
> [!div class="nextstepaction"]
> 
> [Inicio rápido: Creación de un trabajo de Stream Analytics mediante Azure Portal](stream-analytics-quick-create-portal.md)

<!--Link references-->
[stream.analytics.developer.guide]: ../stream-analytics-developer-guide.md
[stream.analytics.scale.jobs]: stream-analytics-scale-jobs.md
[stream.analytics.introduction]: stream-analytics-introduction.md
[stream.analytics.get.started]: stream-analytics-real-time-fraud-detection.md
[stream.analytics.query.language.reference]: https://go.microsoft.com/fwlink/?LinkID=513299
[stream.analytics.rest.api.reference]: https://go.microsoft.com/fwlink/?LinkId=517301
