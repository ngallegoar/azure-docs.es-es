---
title: Transformación de origen en flujo de datos de asignación
description: Aprenda a configurar una transformación de origen en Mapping Data Flow.
author: kromerm
ms.author: makromer
manager: anandsub
ms.service: data-factory
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 12/12/2019
ms.openlocfilehash: 128b15bd5b3ba3c3ac891719bf5c3ec8e5137cce
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "77023521"
---
# <a name="source-transformation-in-mapping-data-flow"></a>Transformación de origen en flujo de datos de asignación 

Una transformación de origen configura el origen de datos para el flujo de datos. Al diseñar flujos de datos, el primer paso será siempre configurar una transformación de origen. Para agregar un origen, haga clic en el cuadro **Agregar origen** en el lienzo de Data Flow.

Cada flujo de datos requiere al menos una transformación de origen, pero puede agregar tantos orígenes como sea necesario para completar las transformaciones de datos. Puede combinar esos orígenes con una transformación de combinación, búsqueda o unión.

Cada transformación de origen se asocia exactamente con un conjunto de datos de Data Factory. El conjunto de datos define la forma y la ubicación de los datos que quiere escribir o leer. Si va a utilizar un conjunto de datos basado en archivos, puede usar caracteres comodín y listas de archivos en el origen para trabajar con más de un archivo a la vez.

## <a name="supported-source-connectors-in-mapping-data-flow"></a>Conectores de origen compatibles en el flujo de datos de asignación

Mapping Data Flow sigue un enfoque de extracción, carga y transformación (ELT) y funciona con conjuntos de datos de un *almacenamiento provisional* que están todos en Azure. Actualmente, se pueden usar los siguientes conjuntos de datos en una transformación de origen:
    
* [Azure Blob Storage](connector-azure-blob-storage.md#mapping-data-flow-properties) (JSON, Avro, Text, Parquet)
* [Azure Data Lake Storage Gen1](connector-azure-data-lake-store.md#mapping-data-flow-properties) (JSON, Avro, Text, Parquet)
* [Azure Data Lake Storage Gen2](connector-azure-data-lake-storage.md#mapping-data-flow-properties) (JSON, Avro, Text, Parquet)
* [Azure Synapse Analytics](connector-azure-sql-data-warehouse.md#mapping-data-flow-properties)
* [Azure SQL Database](connector-azure-sql-database.md#mapping-data-flow-properties)
* [Azure CosmosDB](connector-azure-cosmos-db.md#mapping-data-flow-properties)

La configuración específica de estos conectores se encuentra en la pestaña **Opciones de origen**. La información sobre esta configuración se encuentra en la documentación del conector. 

Azure Data Factory tiene acceso a más de [90 conectores nativos](connector-overview.md). Para incluir datos de esos otros orígenes en el flujo de datos, use la herramienta de actividad de copia para cargar esos datos en una de las áreas de almacenamiento provisional compatibles.

## <a name="source-settings"></a>Configuración de origen

Una vez que haya agregado un origen, configúrelo mediante la pestaña **Configuración de origen**. Aquí puede elegir o crear el conjunto de datos al que apunta el origen. También puede seleccionar las opciones de muestreo y esquema para los datos.

![Pestaña de configuración de origen](media/data-flow/source1.png "Pestaña de configuración de origen")

**Probar conexión:** pruebe si el servicio Spark del flujo de datos puede conectarse correctamente al servicio vinculado que se usa en el conjunto de datos de origen. El modo de depuración debe estar activado para habilitar esta característica.

**Desfase de esquema:** El [desfase de esquema](concepts-data-flow-schema-drift.md) es la capacidad de Data Factory de administrar de forma nativa los esquemas flexibles en los flujos de datos sin necesidad de definir explícitamente los cambios en las columnas.

* Si las columnas de origen van a cambiar con frecuencia, seleccione la casilla **Allow schema drift** (Permitir el desfase de esquema). Esta opción permite que todos los campos de origen entrantes fluyan hasta el receptor a través de las transformaciones.

* Al elegir **Infer drifted column types** (Inferir tipos de columnas desfasadas), se indica a Data Factory que detecte y defina los tipos de datos para cada nueva columna detectada. Con esta característica desactivada, todas las columnas desfasadas serán del tipo cadena.

**Validate schema:** (Validar esquema) Si Validar esquema está seleccionado, el flujo de datos no se podrá ejecutar si los datos de origen entrantes no coinciden con el esquema definido del conjunto de datos.

**Skip line count:** (Número de líneas para omitir) Este campo especifica el número de líneas que se van a omitir al principio del conjunto de datos.

**Muestreo:** Habilite el muestreo para limitar el número de filas del origen. Use esta configuración al probar o muestrear datos del origen con fines de depuración.

**Filas de varias líneas:** seleccione las filas de varias líneas si el archivo de texto de origen contiene valores de cadena que abarcan varias filas; es decir, nuevas líneas dentro de un valor. Esta opción solo está disponible en conjuntos de datos de DelimitedText.

Para validar si el origen está configurado correctamente, active el modo de depuración y capture una vista previa de los datos. Para más información, consulte [Modo de depuración](concepts-data-flow-debug-mode.md).

> [!NOTE]
> Cuando se activa el modo de depuración, el valor del límite de filas de la configuración de depuración sobrescribirá el valor de muestreo en el origen durante la vista previa de los datos.

## <a name="projection"></a>Proyección

Al igual que los esquemas en los conjuntos de datos, la proyección de un origen define las columnas, los tipos y los formatos de datos de los datos de origen. Para la mayoría de los tipos de conjuntos de datos, como SQL y Parquet, la proyección en un origen se corrige para que refleje el esquema definido en un conjunto de datos. Cuando los archivos de origen no están fuertemente tipados (por ejemplo, archivos sin formato en lugar de archivos Parquet), puede definir los tipos de datos de cada campo en la transformación de origen.

![Configuración de la pestaña de proyección](media/data-flow/source3.png "Proyección")

Si el archivo de texto no tiene ningún esquema definido, seleccione **Detectar tipo de datos** para que Data Factory muestree e infiera los tipos de datos. Seleccione **Definir formato predeterminado** para detectar automáticamente los formatos de datos predeterminados.

**Reset schema** (Restablecer esquema) restablece la proyección a la definición del conjunto de datos de referencia.

Puede modificar los tipos de datos de columna en una transformación de columna derivada de un nivel inferior. Use una transformación de selección para modificar los nombres de columna.

### <a name="import-schema"></a>Importar esquema

El botón **Import Schema** (Importar esquema) de la pestaña **Proyección** permite usar un clúster de depuración activo para crear una proyección de esquema. Disponible en cada tipo de origen, la importación del esquema aquí invalidará la proyección definida en el conjunto de datos. El objeto del conjunto de datos no se cambiará.

Resulta útil en los conjuntos de datos como Avro y CosmosDB, que admiten estructuras de datos complejas y no requieren la existencia de definiciones de esquemas en el conjunto de datos.

## <a name="optimize-the-source-transformation"></a>Optimización de la transformación de origen

En la pestaña **Optimizar** de la transformación de origen, es posible que vea un tipo de partición de **origen**. Esta opción solo está disponible cuando el origen es Azure SQL Database. Esto se debe a que Data Factory intenta establecer conexiones en paralelo para ejecutar consultas grandes en el origen de SQL Database.

![Configuración de la partición de origen](media/data-flow/sourcepart3.png "creación de particiones")

No es necesario que cree particiones de los datos en el origen de SQL Database, pero las particiones son útiles para consultas grandes. Puede basar la partición en una columna o una consulta.

### <a name="use-a-column-to-partition-data"></a>Uso de una columna para crear particiones de datos

En la tabla de origen, seleccione una columna en la que crear una partición. También debe establecer el número de particiones.

### <a name="use-a-query-to-partition-data"></a>Uso de una consulta para crear particiones de datos

Puede elegir crear una partición de las conexiones según una consulta. Escriba el contenido de un predicado WHERE. Por ejemplo, escriba año > 1980.

Para más información sobre la optimización en Mapping Data Flow, consulte la [pestaña de optimización](concepts-data-flow-overview.md#optimize).

## <a name="next-steps"></a>Pasos siguientes

Comience a compilar una [transformación de columna derivada](data-flow-derived-column.md) y una [transformación de selección](data-flow-select.md).
