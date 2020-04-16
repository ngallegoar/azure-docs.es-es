---
title: Guía de escalabilidad y rendimiento de la actividad de copia
description: Conozca los factores más importantes que afectan al rendimiento del movimiento de datos en Azure Data Factory cuando se usa la actividad de copia.
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
ms.openlocfilehash: aedb3df69821d1436b03b2eb1f12873b624d426e
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/16/2020
ms.locfileid: "81414174"
---
# <a name="copy-activity-performance-and-scalability-guide"></a>Guía de escalabilidad y rendimiento de la actividad de copia

> [!div class="op_single_selector" title1="Seleccione la versión de Azure Data Factory que usa:"]
> * [Versión 1](v1/data-factory-copy-activity-performance.md)
> * [Versión actual](copy-activity-performance.md)

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Si quiere realizar una migración de datos a gran escala desde un lago de datos o un almacenamiento de datos empresarial (EDW) a Azure, o si quiere ingerir datos a escala desde distintos orígenes en Azure para análisis de macrodatos, es fundamental lograr un rendimiento y escalabilidad óptimos.  Azure Data Factory proporciona un mecanismo eficaz, resistente y rentable para ingerir datos a escala, lo que lo convierte en una buena opción para los ingenieros de datos que buscan crear canalizaciones de ingesta de datos de alto rendimiento y escalabilidad.

Después de leer este artículo, podrá responder a las siguientes preguntas:

- ¿Qué nivel de rendimiento y escalabilidad puedo conseguir si uso la actividad de copia de ADF para escenarios de migración de datos y de ingesta de datos?

- ¿Qué pasos se deben seguir para optimizar el rendimiento de la actividad de copia de ADF?
- ¿Qué mecanismos de optimización del rendimiento de ADF puedo utilizar para optimizar el rendimiento de una única ejecución de la actividad de copia?
- ¿Qué otros factores ajenos a ADF se deben tener en cuenta al optimizar el rendimiento de la copia?

> [!NOTE]
> Si no está familiarizado con la actividad de copia en general, consulte la [información general de la actividad de copia](copy-activity-overview.md) antes de leer este artículo.

## <a name="copy-performance-and-scalability-achievable-using-adf"></a>Rendimiento y escalabilidad de copia que pueden lograrse mediante ADF

ADF ofrece una arquitectura sin servidor que permite paralelismo en diferentes niveles, lo que permite a los desarrolladores crear canalizaciones para aprovechar al máximo el ancho de banda de red, así como el ancho de banda y las IOPS de almacenamiento para maximizar el rendimiento del movimiento de datos para su entorno.  Esto significa que el rendimiento que puede lograr puede calcularse al medir el rendimiento mínimo ofrecido por el almacén de datos de origen, el almacén de datos de destino y el ancho de banda de red entre el origen y el destino.  En la tabla siguiente se calcula la duración de la copia en función del tamaño de los datos y el límite de ancho de banda de su entorno. 

| Tamaño de los datos / <br/> bandwidth | 50 Mbps    | 100 Mbps  | 500 Mbps  | 1 Gbps   | 5 Gbps   | 10 Gbps  | 50 Gbps   |
| --------------------------- | ---------- | --------- | --------- | -------- | -------- | -------- | --------- |
| **1 GB**                    | 2,7 min    | 1,4 min   | 0,3 min   | 0,1 min  | 0,03 min | 0,01 min | 0,0 min   |
| **10 GB**                   | 27,3 min   | 13,7 min  | 2,7 min   | 1,3 min  | 0,3 min  | 0,1 min  | 0,03 min  |
| **100 GB**                  | 4,6 h    | 2,3 h   | 0,5 h   | 0,2 h  | 0,05 h | 0,02 h | 0,0 h   |
| **1 TB**                    | 46,6 h   | 23,3 h  | 4,7 h   | 2,3 h  | 0,5 h  | 0,2 h  | 0,05 h  |
| **10 TB**                   | 19,4 días  | 9,7 días  | 1,9 días  | 0,9 días | 0,2 días | 0,1 días | 0,02 días |
| **100 TB**                  | 194,2 días | 97,1 días | 19,4 días | 9,7 días | 1,9 días | 1 día    | 0,2 días  |
| **1 PB**                    | 64,7 meses    | 32,4 meses   | 6,5 meses    | 3,2 meses   | 0,6 meses   | 0,3 meses   | 0,06 meses   |
| **10 PB**                   | 647,3 meses   | 323,6 meses  | 64,7 meses   | 31,6 meses  | 6,5 meses   | 3,2 meses   | 0,6 meses    |

La copia de ADF puede escalarse en diferentes niveles:

![Cómo se escala la copia de ADF](media/copy-activity-performance/adf-copy-scalability.png)

- El flujo de control de ADF puede iniciar varias actividades de copia en paralelo, por ejemplo, mediante un [bucle ForEach](control-flow-for-each-activity.md).
- Una única actividad de copia puede aprovechar recursos de proceso escalables: si usa Azure Integration Runtime, puede especificar [hasta 256 DIU](#data-integration-units) para cada actividad de copia en modo sin servidor; si usa un entorno de ejecución de integración autohospedado, puede escalar verticalmente la máquina o escalar horizontalmente a varias máquinas ([hasta 4 nodos](create-self-hosted-integration-runtime.md#high-availability-and-scalability)) manualmente y una única actividad de copia creará particiones de su conjunto de archivos en todos los nodos.
- Una única actividad de copia lee y escribe en el almacén de datos mediante varios subprocesos [en paralelo](#parallel-copy).

## <a name="performance-tuning-steps"></a>Pasos de optimización del rendimiento

Para optimizar el rendimiento del servicio Azure Data Factory con la actividad de copia, siga estos pasos.

1. **Seleccione un conjunto de datos de prueba y establezca una línea de base**. Durante la fase de desarrollo, pruebe la canalización, para lo que debe usar la actividad de copia con unos datos de ejemplo representativos. El conjunto de datos que elija debe representar los patrones de datos habituales (estructura de carpetas, patrón de archivo, esquema de datos, etc.), y ser lo suficientemente grande como para evaluar el rendimiento de las copias; por ejemplo, tarda 10 minutos o más en completarse la actividad de copia. Recopile los detalles de la ejecución y las características del rendimiento después de la [supervisión de la actividad de copia](copy-activity-monitoring.md).

2. **Cómo maximizar el rendimiento de una única actividad de copia**:

   Para empezar, se recomienda maximizar primero el rendimiento mediante una única actividad de copia.

   - **Si la actividad de copia se ejecuta en Azure Integration Runtime:** comience con los valores predeterminados para las [unidades de integración de datos (DIU)](#data-integration-units) y configuración de [copia en paralelo](#parallel-copy). 

   - **Si la actividad de copia se ejecuta en una instancia de Integration Runtime autohospedada:** se recomienda usar para hospedar el entorno de ejecución de integración una máquina dedicada independiente del servidor que hospeda el almacén de datos. Comience con los valores predeterminados para la configuración de [copia en paralelo](#parallel-copy) y use un solo nodo para el IR autohospedado.  

   Realice una serie de pruebas de rendimiento y tome nota del rendimiento conseguido, así como de los valores reales usados para las DIU y las copias en paralelo. Consulte el artículo de [supervisión de la actividad de copia](copy-activity-monitoring.md) sobre cómo recopilar los resultados de ejecución y la configuración de rendimiento utilizados, y aprenda a [solucionar problemas de rendimiento de la actividad de copia](copy-activity-performance-troubleshooting.md) para identificar y resolver el cuello de botella. 

   Recorra en iteración para realizar series de pruebas de rendimiento adicionales siguiendo las instrucciones para la solución de problemas y la guía de optimización. Una vez que la ejecución de la actividad de copia única no pueda lograr un mejor rendimiento, considere la posibilidad de maximizar el rendimiento agregado mediante la ejecución de varias copias de forma simultánea siguiendo el paso 3.


3. **Cómo maximizar el rendimiento agregado mediante la ejecución de varias copias simultáneamente:**

   Ahora que ha maximizado el rendimiento de una única actividad de copia, si aún no ha logrado alcanzar los límites de rendimiento de su entorno (red, almacén de datos de origen y almacén de datos de destino), puede ejecutar varias actividades de copia en paralelo mediante las construcciones de flujo de control de ADF, como el [bucle For Each](control-flow-for-each-activity.md). Consulte las plantillas de solución de [Copia de archivos de varios contenedores](solution-template-copy-files-multiple-containers.md), [Migración de datos de Amazon S3 a ADLS Gen2](solution-template-migration-s3-azure.md) o [Copia masiva con una tabla de control](solution-template-bulk-copy-with-control-table.md) como ejemplo general.

5. **Expanda la configuración a todo el conjunto de datos.** Cuando esté satisfecho con los resultados y el rendimiento de la ejecución, puede expandir la definición y la canalización para cubrir todo el conjunto de datos.

## <a name="troubleshoot-copy-activity-performance"></a>Solución de problemas de rendimiento de la actividad de copia

Siga los [pasos de optimización del rendimiento](#performance-tuning-steps) para planear y realizar la prueba de rendimiento de su escenario. Y aprenda a solucionar los problemas de rendimiento de la ejecución de la actividad de copia en Azure Data Factory desde [Solución de problemas de rendimiento de la actividad de copia](copy-activity-performance-troubleshooting.md).

## <a name="copy-performance-optimization-features"></a>Características de optimización del rendimiento

Azure Data Factory proporciona las siguientes características de optimización del rendimiento:

- [Unidades de integración de datos](#data-integration-units)
- [Escalabilidad de Integration Runtime autohospedado](#self-hosted-integration-runtime-scalability)
- [Copia paralela](#parallel-copy)
- [Copias almacenadas provisionalmente](#staged-copy)

### <a name="data-integration-units"></a>Unidades de integración de datos

Una unidad de integración de datos es una medida que representa la eficacia (una combinación de CPU, memoria y asignación de recursos de red) de una única unidad en Azure Data Factory. Una unidad de integración de datos solo se aplica a [Azure Integration Runtime](concepts-integration-runtime.md#azure-integration-runtime), pero no a [Integration Runtime autohospedado](concepts-integration-runtime.md#self-hosted-integration-runtime). [Más información](copy-activity-performance-features.md#data-integration-units).

### <a name="self-hosted-integration-runtime-scalability"></a>Escalabilidad del entorno de ejecución de integración autohospedado

Para hospedar el aumento de la carga de trabajo simultánea o para lograr un mayor rendimiento, puede escalar vertical u horizontalmente la instancia de Integration Runtime autohospedada. [Más información](copy-activity-performance-features.md#self-hosted-integration-runtime-scalability).

### <a name="parallel-copy"></a>Copia en paralelo

Puede establecer la copia en paralelo para indicar el paralelismo que desea que utilice la actividad de copia. Esta propiedad se puede considerar como el número máximo de subprocesos dentro de la actividad de copia que se leen de los almacenes de datos de origen o se escriben en los almacenes de datos receptores en paralelo. [Más información](copy-activity-performance-features.md#parallel-copy).

### <a name="staged-copy"></a>copia almacenada provisionalmente

Al copiar datos de un almacén de datos de origen a un almacén de datos receptor, podría elegir usar Almacenamiento de blobs como almacenamiento provisional. [Más información](copy-activity-performance-features.md#staged-copy).

## <a name="next-steps"></a>Pasos siguientes
Consulte los restantes artículos acerca de la actividad de copia:

- [Información general de la actividad de copia](copy-activity-overview.md)
- [Solución de problemas de rendimiento de la actividad de copia](copy-activity-performance-troubleshooting.md)
- [Características de optimización del rendimiento de la actividad de copia](copy-activity-performance-features.md)
- [Uso de Azure Data Factory para migrar datos del lago de datos y el almacenamiento de datos a Azure](data-migration-guidance-overview.md)
- [Migración de datos de AWS S3 a Azure Storage](data-migration-guidance-s3-azure-storage.md)
