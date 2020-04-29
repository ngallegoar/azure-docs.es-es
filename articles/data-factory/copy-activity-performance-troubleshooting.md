---
title: Solución de problemas de rendimiento de la actividad de copia
description: Conozca cómo solucionar problemas de rendimiento de la actividad de copia en Azure Data Factory.
services: data-factory
documentationcenter: ''
ms.author: jingwang
author: linda33wj
manager: shwang
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 03/11/2020
ms.openlocfilehash: 6df1903e828c0c4cafa6589d4a85f4016bed893e
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2020
ms.locfileid: "81414138"
---
# <a name="troubleshoot-copy-activity-performance"></a>Solución de problemas de rendimiento de la actividad de copia

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

En este artículo se describe cómo solucionar problemas de rendimiento de la actividad de copia en Azure Data Factory. 

Después de ejecutar una actividad de copia, puede recopilar el resultado de la ejecución y estadísticas de rendimiento en la vista de [supervisión de la actividad de copia](copy-activity-monitoring.md). A continuación se muestra un ejemplo.

![Detalles de la supervisión de la ejecución de la actividad de copia](./media/copy-activity-overview/monitor-copy-activity-run-details.png)

## <a name="performance-tuning-tips"></a>Sugerencias de optimización del rendimiento

En algunos casos, cuando se ejecuta una actividad de copia en Data Factory, se ve el mensaje **"Performance tuning tips"** (Sugerencias para la optimización del rendimiento) en la parte superior, como se muestra en el ejemplo anterior. Las sugerencias señalan el cuello de botella identificado por ADF para la ejecución específica de la copia, junto con recomendaciones sobre cómo aumentar el rendimiento de la copia. Intente llevar a cabo el cambio sugerido y ejecutar la copia de nuevo.

Como referencia, actualmente las sugerencias de optimización del rendimiento proporcionan recomendaciones para los siguientes casos:

| Category              | Sugerencias de optimización del rendimiento                                      |
| --------------------- | ------------------------------------------------------------ |
| Específica del almacén de datos   | Carga de datos en **Azure Synpase Analytics (anteriormente SQL DW)** : se recomienda usar PolyBase o la instrucción COPY si no se usa. |
| &nbsp;                | Copia de datos de o a **Azure SQL Database**: cuando la DTU tiene un uso elevado, se recomienda actualizar a un nivel superior. |
| &nbsp;                | Copia de datos de o a **Azure Cosmos DB**: cuando la RU tiene un uso elevado, se recomienda actualizar a una RU mayor. |
| &nbsp;                | Ingesta de datos de **Amazon Redshift**: se recomienda el uso de UNLOAD si no se usa. |
| Limitación del almacén de datos | Si el almacén de datos limita un número de operaciones de lectura o escritura durante la copia, se recomienda realizar una comprobación y aumentar la tasa de solicitudes permitida para el almacén de datos o reducir la carga de trabajo simultánea. |
| Entorno de ejecución de integración  | Si usa un **entorno de ejecución de integración (IR) autohospedado** y la actividad de copia espera mucho tiempo en la cola hasta que el IR tiene disponible un recurso para la ejecución, se recomienda escalar horizontal o verticalmente el entorno de ejecución de integración. |
| &nbsp;                | Si usa una instancia de **Azure Integration Runtime** que se encuentra en una región no óptima, lo que da lugar a una lectura y escritura lentas, se recomienda cambiar la configuración para usar el entorno de ejecución de integración de otra región. |
| Tolerancia a errores       | Si configura la tolerancia a errores y la omisión de filas incompatibles provoca un rendimiento lento, se recomienda asegurarse de que los datos de origen y receptor son compatibles. |
| copia almacenada provisionalmente           | Si la copia almacenada provisionalmente está configurada pero no resulta útil para el par origen-receptor, se recomienda quitarla. |
| Reanudación                | Tenga en cuenta que, cuando la actividad de copia se reanuda desde el último punto de error, pero se cambió la configuración de la unidad de integración de datos (DIU) después de la ejecución original, la nueva configuración de DIU no surte efecto. |

## <a name="understand-copy-activity-execution-details"></a>Descripción de los detalles de ejecución de la actividad de copia

Los detalles de ejecución y las duraciones en la parte inferior de la vista de supervisión de la actividad de copia describen las fases clave por las que pasa la actividad de copia (consulte el ejemplo al principio de este artículo), lo que resulta especialmente útil para solucionar problemas de rendimiento de la copia. El cuello de botella de la ejecución de copia corresponde a la actividad con la mayor duración. Consulte en la tabla siguiente la definición de cada fase y aprenda a [solucionar problemas de la actividad de copia en Azure IR](#troubleshoot-copy-activity-on-azure-ir) y a [solucionar problemas de la actividad de copia en el IR autohospedado](#troubleshoot-copy-activity-on-self-hosted-ir) con dicha información.

| Fase           | Descripción                                                  |
| --------------- | ------------------------------------------------------------ |
| Cola           | Tiempo transcurrido hasta que la actividad de copia se inicia realmente en el entorno de ejecución de integración. |
| Pre-copy script (Script anterior a la copia) | Tiempo transcurrido entre la actividad de copia que comienza en IR y la actividad de copia que finaliza la ejecución del script anterior a la copia en el almacén de datos receptor. Se aplica al configurar el script anterior a la copia para los receptores de base de datos; por ejemplo, al escribir datos en Azure SQL Database, limpiar antes de copiar los datos nuevos. |
| Transferencia        | Tiempo transcurrido entre el final del paso anterior y el IR que transfiere todos los datos del origen al receptor. Los subpasos en la fase de transferencia se ejecutan en paralelo.<br><br>- **Time to first byte** (Tiempo hasta el primer byte): tiempo transcurrido entre el final del paso anterior y el momento en que la instancia de IR recibe el primer byte del almacén de datos de origen. Se aplica a un origen no basado en archivos.<br>- **Listing source** (Lista de orígenes): tiempo empleado en la enumeración de los archivos de origen o las particiones de datos. El último se aplica cuando se configuran las opciones de partición para los orígenes de base de datos; por ejemplo, cuando se copian datos de bases de datos como Oracle, SAP HANA, Teradata, Netezza, etc.<br/>-**Reading from source** (Lectura desde origen): tiempo empleado en la recuperación de datos desde el almacén de datos de origen.<br/>- **Writing to sink** (Escritura en el receptor): tiempo empleado en la escritura de datos en el almacén de datos receptor. |

## <a name="troubleshoot-copy-activity-on-azure-ir"></a>Solución de problemas de la actividad de copia en Azure IR

Siga los [pasos de optimización del rendimiento](copy-activity-performance.md#performance-tuning-steps) para planear y realizar la prueba de rendimiento de su escenario. 

Si el rendimiento de la actividad de copia no satisface sus expectativas, quiere solucionar problemas de una única actividad de copia que se ejecuta en Azure Integration Runtime y aparecen [sugerencias para optimizar el rendimiento](#performance-tuning-tips) en la vista de supervisión de la copia, aplique la recomendación e inténtelo de nuevo. Si no aparecen estas sugerencias, [analice los detalles de ejecución de la actividad de copia](#understand-copy-activity-execution-details), compruebe qué fase presenta la duración **más larga** y aplique las instrucciones siguientes para aumentar el rendimiento de la copia:

- **La fase de script anterior a la copia muestra una larga duración**: esto significa que el script anterior a la copia que se ejecuta en la base de datos receptora tarda mucho tiempo en finalizar. Ajuste la lógica del script anterior a la copia especificado para mejorar el rendimiento. Si necesita más ayuda para mejorar el script, póngase en contacto con el equipo de base de datos.

- **En la fase de transferencia, el tiempo hasta el primer byte muestra una larga duración de trabajo**: significa que la consulta de origen tarda mucho tiempo en devolver datos. Compruebe y optimice la consulta o el servidor. Si necesita más ayuda, póngase en contacto con el equipo del almacén de datos.

- **En la fase de transferencia, el listado de orígenes muestra una larga duración de trabajo**: significa que la enumeración de los archivos de origen o de las particiones de datos de la base de datos de origen es lenta.

  - Al copiar datos desde el origen basado en archivos, si usa el **filtro de comodín** en la ruta de acceso de la carpeta o en el nombre de archivo (`wildcardFolderPath` o `wildcardFileName`) o usa el **filtro de hora de última modificación del archivo** (`modifiedDatetimeStart` o `modifiedDatetimeEnd`), tenga en cuenta que dicho filtro daría lugar a que la actividad de copia enumerase todos los archivos de la carpeta especificada en el lado de cliente y después aplicase el filtro. Esta enumeración de archivos podría convertirse en el cuello de botella, especialmente cuando solo un pequeño conjunto de archivos cumple la regla del filtro.

    - Compruebe si puede [copiar archivos en función del nombre o la ruta de acceso del archivo con particiones de tiempo](tutorial-incremental-copy-partitioned-file-name-copy-data-tool.md). De este modo, no se aporta carga alguna a la enumeración del lado de origen.

    - Compruebe si en su lugar puede usar el filtro nativo del almacén de datos, específicamente "**prefix**" para Amazon S3 y Azure Blob. El filtro de prefijo es un filtro del lado servidor del almacén de datos y tendría un rendimiento mucho mejor.

    - Considere la posibilidad de dividir un conjunto de datos de gran tamaño en varios conjuntos de datos más pequeños y permitir que esos trabajos de copia se ejecuten simultáneamente, abordando cada uno de ellos una parte de los datos. Puede hacerlo con Lookup/GetMetadata + ForEach + Copy. Consulte las plantillas de solución de [Copia de archivos de varios contenedores](solution-template-copy-files-multiple-containers.md) o [Migración de datos de Amazon S3 a ADLS Gen2](solution-template-migration-s3-azure.md) como ejemplo general.

  - Compruebe si ADF notifica algún error de limitación en el origen o si el almacén de datos muestra un estado de uso elevado. Si es así, reduzca las cargas de trabajo en el almacén de datos o intente ponerse en contacto con el administrador del almacén de datos para aumentar el límite o los recursos disponibles.

  - Use una instancia de Azure IR en la misma región que el almacén de datos de origen o en una región próxima.

- **En la fase de transferencia, la lectura desde el origen muestra una larga duración de trabajo**: 

  - Adopte el procedimiento recomendado de carga de datos específico del conector si es aplicable. Por ejemplo, al copiar datos desde [Amazon Redshift](connector-amazon-redshift.md), realice la configuración para que use UNLOAD de Redshift.

  - Compruebe si ADF notifica algún error de limitación en el origen o si el almacén de datos muestra un uso elevado. Si es así, reduzca las cargas de trabajo en el almacén de datos o intente ponerse en contacto con el administrador del almacén de datos para aumentar el límite o los recursos disponibles.

  - Compruebe el patrón del origen y el receptor de la copia: 

    - Si el patrón de copia admite más de cuatro unidades de integración de datos (DIU), consulte [esta sección](copy-activity-performance-features.md#data-integration-units) para obtener detalles; por lo general, puede intentar aumentar las unidades para obtener un mejor rendimiento. 

    - En caso contrario, considere la posibilidad de dividir un conjunto de datos de gran tamaño en varios conjuntos de datos más pequeños y permitir que esos trabajos de copia se ejecuten simultáneamente, abordando cada uno de ellos una parte de los datos. Puede hacerlo con Lookup/GetMetadata + ForEach + Copy. Consulte las plantillas de solución de [Copia de archivos de varios contenedores](solution-template-copy-files-multiple-containers.md), [Migración de datos de Amazon S3 a ADLS Gen2](solution-template-migration-s3-azure.md) o [Copia masiva con una tabla de control](solution-template-bulk-copy-with-control-table.md) como ejemplo general.

  - Use una instancia de Azure IR en la misma región que el almacén de datos de origen o en una región próxima.

- **En la fase de transferencia, la escritura en el receptor muestra una larga duración de trabajo**:

  - Adopte el procedimiento recomendado de carga de datos específico del conector si es aplicable. Por ejemplo, al copiar datos en [Azure Synapse Analytics](connector-azure-sql-data-warehouse.md) (anteriormente SQL DW), use PolyBase o la instrucción COPY. 

  - Compruebe si ADF notifica algún error de limitación en el receptor o si el almacén de datos muestra un uso elevado. Si es así, reduzca las cargas de trabajo en el almacén de datos o intente ponerse en contacto con el administrador del almacén de datos para aumentar el límite o los recursos disponibles.

  - Compruebe el patrón del origen y el receptor de la copia: 

    - Si el patrón de copia admite más de cuatro unidades de integración de datos (DIU), consulte [esta sección](copy-activity-performance-features.md#data-integration-units) para obtener detalles; por lo general, puede intentar aumentar las unidades para obtener un mejor rendimiento. 

    - De lo contrario, ajuste gradualmente las [copias en paralelo](copy-activity-performance-features.md); tenga en cuenta que demasiadas copias en paralelo pueden perjudicar el rendimiento.

  - Use una instancia de Azure IR en la misma región que el almacén de datos receptor o en una región próxima.

## <a name="troubleshoot-copy-activity-on-self-hosted-ir"></a>Solución de problemas de la actividad de copia en IR autohospedado

Siga los [pasos de optimización del rendimiento](copy-activity-performance.md#performance-tuning-steps) para planear y realizar la prueba de rendimiento de su escenario. 

Si el rendimiento de la copia no satisface sus expectativas, quiere solucionar problemas de una única actividad de copia que se ejecuta en Azure Integration Runtime y aparecen [sugerencias para optimizar el rendimiento](#performance-tuning-tips) en la vista de supervisión de la copia, aplique la recomendación e inténtelo de nuevo. Si no aparecen estas sugerencias, [analice los detalles de ejecución de la actividad de copia](#understand-copy-activity-execution-details), compruebe qué fase presenta la duración **más larga** y aplique las instrucciones siguientes para aumentar el rendimiento de la copia:

- **La fase de cola muestra una larga duración**: significa que la actividad de copia espera mucho tiempo en la cola hasta que la instancia de IR autohospedado tiene el recurso para la ejecución. Compruebe la capacidad y el uso de IR y [escale vertical u horizontalmente](create-self-hosted-integration-runtime.md#high-availability-and-scalability) según la carga de trabajo.

- **En la fase de transferencia, el tiempo hasta el primer byte muestra una larga duración de trabajo**: significa que la consulta de origen tarda mucho tiempo en devolver datos. Compruebe y optimice la consulta o el servidor. Si necesita más ayuda, póngase en contacto con el equipo del almacén de datos.

- **En la fase de transferencia, el listado de orígenes muestra una larga duración de trabajo**: significa que la enumeración de los archivos de origen o de las particiones de datos de la base de datos de origen es lenta.

  - Compruebe si la máquina del entorno de ejecución de integración autohospedado tiene una latencia baja al conectarse al almacén de datos de origen. Si el origen está en Azure, puede usar [esta herramienta](http://www.azurespeed.com/Azure/Latency) para comprobar la latencia de la máquina de IR autohospedado en la región de Azure; es mejor un valor menor.

  - Al copiar datos desde el origen basado en archivos, si usa el **filtro de comodín** en la ruta de acceso de la carpeta o en el nombre de archivo (`wildcardFolderPath` o `wildcardFileName`) o usa el **filtro de hora de última modificación del archivo** (`modifiedDatetimeStart` o `modifiedDatetimeEnd`), tenga en cuenta que dicho filtro daría lugar a que la actividad de copia enumerase todos los archivos de la carpeta especificada en el lado de cliente y después aplicase el filtro. Esta enumeración de archivos podría convertirse en el cuello de botella, especialmente cuando solo un pequeño conjunto de archivos cumple la regla del filtro.

    - Compruebe si puede [copiar archivos en función del nombre o la ruta de acceso del archivo con particiones de tiempo](tutorial-incremental-copy-partitioned-file-name-copy-data-tool.md). De este modo, no se aporta carga alguna a la enumeración del lado de origen.

    - Compruebe si en su lugar puede usar el filtro nativo del almacén de datos, específicamente "**prefix**" para Amazon S3 y Azure Blob. El filtro de prefijo es un filtro del lado servidor del almacén de datos y tendría un rendimiento mucho mejor.

    - Considere la posibilidad de dividir un conjunto de datos de gran tamaño en varios conjuntos de datos más pequeños y permitir que esos trabajos de copia se ejecuten simultáneamente, abordando cada uno de ellos una parte de los datos. Puede hacerlo con Lookup/GetMetadata + ForEach + Copy. Consulte las plantillas de solución de [Copia de archivos de varios contenedores](solution-template-copy-files-multiple-containers.md) o [Migración de datos de Amazon S3 a ADLS Gen2](solution-template-migration-s3-azure.md) como ejemplo general.

  - Compruebe si ADF notifica algún error de limitación en el origen o si el almacén de datos muestra un estado de uso elevado. Si es así, reduzca las cargas de trabajo en el almacén de datos o intente ponerse en contacto con el administrador del almacén de datos para aumentar el límite o los recursos disponibles.

- **En la fase de transferencia, la lectura desde el origen muestra una larga duración de trabajo**: 

  - Compruebe si la máquina del entorno de ejecución de integración autohospedado tiene una latencia baja al conectarse al almacén de datos de origen. Si el origen está en Azure, puede usar [esta herramienta](http://www.azurespeed.com/Azure/Latency) para comprobar la latencia de la máquina de IR autohospedado en las regiones de Azure; es mejor un valor menor.

  - Compruebe si la máquina de IR autohospedado tiene suficiente ancho de banda de entrada para leer y transferir los datos de forma eficaz. Si el almacén de datos de origen está en Azure, puede usar [esta herramienta](https://www.azurespeed.com/Azure/Download) para comprobar la velocidad de descarga.

  - Compruebe la tendencia de uso de memoria y CPU del IR autohospedado en Azure Portal -> la factoría de datos -> página de información general. Considere la posibilidad de [escalar vertical u horizontalmente el entorno de ejecución de integración](create-self-hosted-integration-runtime.md#high-availability-and-scalability) si el uso de CPU es alto o hay poca memoria disponible.

  - Adopte el procedimiento recomendado de carga de datos específico del conector si es aplicable. Por ejemplo:

    - \- Cuando copie datos desde [Oracle](connector-oracle.md#oracle-as-source), [Netezza](connector-netezza.md#netezza-as-source), [Teradata](connector-teradata.md#teradata-as-source), [SAP HANA](connector-sap-hana.md#sap-hana-as-source), [SAP Table](connector-sap-table.md#sap-table-as-source) y [SAP Open Hub](connector-sap-business-warehouse-open-hub.md#sap-bw-open-hub-as-source), habilite las opciones de partición de datos para copiar datos en paralelo.

    - Al copiar datos desde [HDFS](connector-hdfs.md), realice la configuración para usar DistCp.

    - Al copiar datos desde [Amazon Redshift](connector-amazon-redshift.md), realice la configuración para usar UNLOAD de Redshift.

  - Compruebe si ADF notifica algún error de limitación en el origen o si el almacén de datos muestra un uso elevado. Si es así, reduzca las cargas de trabajo en el almacén de datos o intente ponerse en contacto con el administrador del almacén de datos para aumentar el límite o los recursos disponibles.

  - Compruebe el patrón del origen y el receptor de la copia: 

    - Si copia datos desde almacenes de datos con la opción de partición habilitada, considere la posibilidad de ajustar gradualmente las [copias en paralelo](copy-activity-performance-features.md); tenga en cuenta que demasiadas copias en paralelo pueden perjudicar al rendimiento.

    - En caso contrario, considere la posibilidad de dividir un conjunto de datos de gran tamaño en varios conjuntos de datos más pequeños y permitir que esos trabajos de copia se ejecuten simultáneamente, abordando cada uno de ellos una parte de los datos. Puede hacerlo con Lookup/GetMetadata + ForEach + Copy. Consulte las plantillas de solución de [Copia de archivos de varios contenedores](solution-template-copy-files-multiple-containers.md), [Migración de datos de Amazon S3 a ADLS Gen2](solution-template-migration-s3-azure.md) o [Copia masiva con una tabla de control](solution-template-bulk-copy-with-control-table.md) como ejemplo general.

- **En la fase de transferencia, la escritura en el receptor muestra una larga duración de trabajo**:

  - Adopte el procedimiento recomendado de carga de datos específico del conector si es aplicable. Por ejemplo, al copiar datos en [Azure Synapse Analytics](connector-azure-sql-data-warehouse.md) (anteriormente SQL DW), use PolyBase o la instrucción COPY. 

  - Compruebe si la máquina del IR autohospedado tiene una latencia baja al conectarse al almacén de datos receptor. Si el receptor está en Azure, puede usar [esta herramienta](http://www.azurespeed.com/Azure/Latency) para comprobar la latencia de la máquina de IR autohospedado en la región de Azure; es mejor un valor menor.

  - Compruebe si la máquina de IR autohospedado tiene suficiente ancho de banda de salida para transferir y escribir los datos de forma eficaz. Si el almacén de datos receptor está en Azure, puede usar [esta herramienta](https://www.azurespeed.com/Azure/UploadLargeFile) para comprobar la velocidad de carga.

  - Compruebe la tendencia de uso de memoria y CPU del IR autohospedado en Azure Portal -> la factoría de datos -> página de información general. Considere la posibilidad de [escalar vertical u horizontalmente el entorno de ejecución de integración](create-self-hosted-integration-runtime.md#high-availability-and-scalability) si el uso de CPU es alto o hay poca memoria disponible.

  - Compruebe si ADF notifica algún error de limitación en el receptor o si el almacén de datos muestra un uso elevado. Si es así, reduzca las cargas de trabajo en el almacén de datos o intente ponerse en contacto con el administrador del almacén de datos para aumentar el límite o los recursos disponibles.

  - Considere la posibilidad de ajustar gradualmente las [copias en paralelo](copy-activity-performance-features.md); tenga en cuenta que demasiadas copias en paralelo pueden perjudicar el rendimiento.

## <a name="other-references"></a>Otras referencias

Estas son algunas referencias para la supervisión y la optimización del rendimiento para algunos de los almacenes de datos admitidos:

* Azure Blob Storage: [Objetivos de escalabilidad y rendimiento de Blob Storage](../storage/blobs/scalability-targets.md) y [Lista de comprobación de escalabilidad y rendimiento para Blob Storage](../storage/blobs/storage-performance-checklist.md).
* Azure Table Storage: [Objetivos de escalabilidad y rendimiento de Blob Storage](../storage/tables/scalability-targets.md) y [Lista de comprobación de rendimiento y de escalabilidad para Table Storage](../storage/tables/storage-performance-checklist.md).
* Azure SQL Database: puede [supervisar el rendimiento](../sql-database/sql-database-single-database-monitor.md) y comprobar el porcentaje de la unidad de transacción de base de datos (DTU).
* Azure SQL Data Warehouse: su funcionalidad se mide en unidades de Data Warehouse (DWU). Consulte [Administración de la potencia de proceso en Azure SQL Data Warehouse (información general)](../synapse-analytics/sql-data-warehouse/sql-data-warehouse-manage-compute-overview.md).
* Azure Cosmos DB: [Niveles de rendimiento en Azure Cosmos DB](../cosmos-db/performance-levels.md).
* SQL Server local: [Supervisión y optimización del rendimiento](https://msdn.microsoft.com/library/ms189081.aspx)
* Servidor de archivos local: [Performance tuning for file servers](https://msdn.microsoft.com/library/dn567661.aspx) (Ajuste del rendimiento para los servidores de archivos).

## <a name="next-steps"></a>Pasos siguientes
Consulte los restantes artículos acerca de la actividad de copia:

- [Información general de la actividad de copia](copy-activity-overview.md)
- [Guía de escalabilidad y rendimiento de la actividad de copia](copy-activity-performance.md)
- [Características de optimización del rendimiento de la actividad de copia](copy-activity-performance-features.md)
- [Uso de Azure Data Factory para migrar datos del lago de datos y el almacenamiento de datos a Azure](data-migration-guidance-overview.md)
- [Migración de datos de AWS S3 a Azure Storage](data-migration-guidance-s3-azure-storage.md)
