---
title: Indexación de un conjunto de datos grande mediante indizadores integrados
titleSuffix: Azure Cognitive Search
description: Estrategias de indexación de datos de gran tamaño o indexación de cálculo intensivo mediante modo por lotes, recursos y técnicas para indexación programada, paralela y distribuida.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 12/17/2019
ms.openlocfilehash: 4ad5e961e390b60784355ff3bc72aca4a2f73e11
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "77190962"
---
# <a name="how-to-index-large-data-sets-in-azure-cognitive-search"></a>Indexación de grandes conjuntos de datos en Azure Cognitive Search

A medida que los volúmenes de datos aumentan o las necesidades de procesamiento cambian, es posible que las estrategias de indexación simples o predeterminadas ya no sean prácticas. Para Azure Cognitive Search, existen varios enfoques para adaptar los grandes conjuntos de datos, que van desde cómo estructurar una solicitud de carga de datos hasta usar un indizador específico del origen para las cargas de trabajo programadas y distribuidas.

Las mismas técnicas también se aplican a procesos de ejecución prolongada. En concreto, los pasos que se describen en [indización paralela](#parallel-indexing) son útiles para la indización de cálculo intensivo, como el análisis de imágenes o el procesamiento de lenguaje natural en la [canalización de enriquecimiento de inteligencia artificial](cognitive-search-concept-intro.md).

En las secciones siguientes se exploran tres técnicas para indexar grandes cantidades de datos.

## <a name="option-1-pass-multiple-documents"></a>Opción 1: Pasar varios documentos

Uno de los mecanismos más sencillos para la indización de un conjunto de datos más grande es enviar varios documentos o registros en una sola solicitud. Siempre y cuando la carga completa sea menor a 16 MB, una solicitud puede administrar hasta 1000 documentos en una operación de carga masiva. Estos límites se aplican tanto si usa la [API REST para agregar documentos](https://docs.microsoft.com/rest/api/searchservice/addupdate-or-delete-documents) como el [método Index](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.documentsoperationsextensions.index?view=azure-dotnet) del SDK de .NET. Con cualquier API, se empaquetarían 1000 documentos en el cuerpo de cada solicitud.

La indización por lotes se implementa para las solicitudes individuales con REST o. NET, o a través de indizadores. Algunos indizadores funcionan bajo distintos límites. En concreto, la indización de Azure Blob establece el tamaño de lote en 10 documentos en reconocimiento al tamaño máximo medio de los documentos. Para los indizadores basados en la [API REST para crear indizadores](https://docs.microsoft.com/rest/api/searchservice/Create-Indexer), puede establecer el argumento `BatchSize` para personalizar esta configuración de modo que se adapte mejor a las características de los datos. 

> [!NOTE]
> Para mantener un tamaño de documento reducido, evite agregar datos no consultables a un índice. Las imágenes y otros datos binarios no se pueden buscar directamente y no se deben almacenar en el índice. Para integrar los datos no consultables en los resultados de búsqueda, debe definir un campo que no admita búsquedas que almacene una referencia de dirección URL al recurso.

## <a name="option-2-add-resources"></a>Opción 2: Adición de recursos

Los servicios que se aprovisionan en uno de los [planes de tarifa Estándar](search-sku-tier.md) a menudo tienen una capacidad infrautilizada de almacenamiento y cargas de trabajo (consultas o indización), lo que hace que [aumentar los recuentos de particiones y réplicas ](search-capacity-planning.md) sea una solución obvia para adaptarse a los grandes conjuntos de datos. Para obtener mejores resultados, necesita dos recursos: las particiones para el almacenamiento y las réplicas para el trabajo de ingesta de datos.

Aumentar las réplicas y las particiones son eventos facturables que aumentan el costo; sin embargo, salvo que indexe continuamente por debajo de la carga máxima, puede agregar escala para el tiempo que dure el proceso de indexación y, luego, reducir los niveles de recursos cuando el proceso finalice.

## <a name="option-3-use-indexers"></a>Opción 3: Uso de indizadores

Los [indexadores](search-indexer-overview.md) sirven para rastrear en los orígenes de datos de Azure admitidos contenido que permite búsquedas. Aunque no se diseñaron específicamente para el indexado a gran escala, varias funcionalidades de los indizadores son especialmente útiles para adaptar grandes conjuntos de datos:

+ Los programadores le permiten empaquetar la indexación en intervalos regulares para que se pueda distribuir con el tiempo.
+ Puede reanudar la indexación programada en el último punto de detención conocido. Si un origen de datos no se rastrea completamente en un plazo de 24 horas, el indizador reanudará la indexación en el segundo día donde se quedó.
+ Particionar los datos en orígenes de datos individuales más pequeños permite realizar procesamientos en paralelo. Puede dividir los datos de origen en componentes más pequeños, por ejemplo, en varios contenedores en Azure Blob Storage y, luego, crear [varios orígenes de datos](https://docs.microsoft.com/rest/api/searchservice/create-data-source) correspondientes en Azure Cognitive Search que se puedan indexar en paralelo.

> [!NOTE]
> Los indizadores son específicos del origen de datos, por lo que un enfoque con indizadores solo es posible para algunos orígenes de datos en Azure: [SQL Database](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md), [Blob Storage](search-howto-indexing-azure-blob-storage.md), [Table Storage](search-howto-indexing-azure-tables.md) y [Cosmos DB](search-howto-index-cosmosdb.md).

### <a name="scheduled-indexing"></a>Indexación programada

La programación de indizadores es un mecanismo importante para procesar conjuntos de datos de gran tamaño y realizar procesos de ejecución lenta, como el análisis de imágenes en una canalización de Cognitive Search. El procesamiento de los indexadores funciona dentro de una ventana de 24 horas. Si el procesamiento no se completa en menos de 24 horas, los comportamientos de la programación de los indexadores pueden resultar beneficiosos. 

De manera predeterminada, la indexación programada se inicia en intervalos específicos, habitualmente con un trabajo que se completa antes de reanudar en el intervalo programado siguiente. Sin embargo, si el procesamiento no se completa dentro del intervalo, el indexador se detiene (porque se agotó el tiempo). En el intervalo siguiente, el procesamiento se reanuda donde quedó, porque el sistema lleva un registro de dónde sucedió eso. 

En la práctica, para cargas de índice que abarcan varios días, puede poner el indexador en una programación de 24 horas. Cuando la indexación se reanuda para el próximo ciclo de 24 horas, se reinicia en el último documento correcto conocido. De este modo, un indexador puede abrirse camino a través del trabajo pendiente de un documento en una serie de días hasta procesar todos los documentos no procesados. Para obtener más información sobre este enfoque, consulte la sección sobre la [indexación de conjuntos de datos de gran tamaño en Azure Blob Storage](search-howto-indexing-azure-blob-storage.md#indexing-large-datasets). Para más información sobre cómo configurar las programaciones en general, consulte la [API REST para crear indizadores ](https://docs.microsoft.com/rest/api/searchservice/Create-Indexer) o [Programación de indizadores para Azure Cognitive Search](search-howto-schedule-indexers.md).

<a name="parallel-indexing"></a>

### <a name="parallel-indexing"></a>Indexación en paralelo

La estrategia de indexación en paralelo se basa en la indexación de varios orígenes de datos al unísono, donde cada definición de origen de datos especifica un subconjunto de los datos. 

Para los requisitos de indexación que no son de rutina ni de cálculo intensivo (tales como el OCR en documentos digitalizados en una canalización de Cognitive Search, análisis de imagen o procesamiento de lenguaje natural), la estrategia de indexación en paralelo a menudo es el enfoque correcto para completar un proceso de larga ejecución en el tiempo más corto. Si puede eliminar o reducir las solicitudes de consulta, la indexación en paralelo en un servicio que no administra soluciones de consulta de manera simultánea es la mejor estrategia para trabajar en un gran volumen de contenido de procesamiento lento. 

El procesamiento paralelo tiene los siguientes elementos:

+ Subdivida los datos de origen entre varios contenedores o varias carpetas virtuales dentro del mismo contenedor. 
+ Asigne cada pequeño conjunto de datos a su propio [origen de datos](https://docs.microsoft.com/rest/api/searchservice/create-data-source), emparejado con su propio [indexador](https://docs.microsoft.com/rest/api/searchservice/create-indexer).
+ En el caso de Cognitive Search, haga referencia al mismo [conjunto de datos](https://docs.microsoft.com/rest/api/searchservice/create-skillset) en cada definición de indexador.
+ Escriba en el mismo índice de búsqueda de destino. 
+ Programe todos los indexadores para que se ejecuten al mismo tiempo.

> [!NOTE]
> En Azure Cognitive Search, no se pueden asignar réplicas o particiones individuales al procesamiento de indexación o consulta. El sistema determina cómo se usan los recursos. Para comprender el impacto en el rendimiento de las consultas, puede probar la indexación paralela en un entorno de prueba antes de implementarlo en producción.  

### <a name="how-to-configure-parallel-indexing"></a>Cómo configurar la indexación en paralelo

En el caso de los indexadores, la capacidad de procesamiento se basa parcialmente en un subsistema de indexador para cada unidad de servicio que el servicio de búsqueda usa. Es posible tener varios indizadores simultáneos en el servicio de Azure Cognitive Search aprovisionado en los niveles Básico o Estándar con dos réplicas al menos. 

1. En [Azure Portal](https://portal.azure.com), en la página **Información general** del panel del servicio de búsqueda, compruebe el **plan de tarifa** para comprobar que puede permitir la indexación en paralelo. Los niveles Básico y Estándar ofrecen varias réplicas.

2. En **Configuración** > **Escala**, [aumente las réplicas](search-capacity-planning.md) para el procesamiento paralelo: una réplica adicional para cada carga de trabajo de indexador. Deje un número suficiente para el volumen de consultas existente. No se recomienda sacrificar las cargas de trabajo por la indexación.

3. Distribuya los datos en varios contenedores en un nivel que puedan alcanzar los indizadores de Azure Cognitive Search. Esto podría tratarse de varias tablas en Azure SQL Database, varios contenedores en Azure Blob Storage o varias colecciones. Defina un objeto de origen de datos para cada tabla o contenedor.

4. Cree y programe varios indexadores para que se ejecuten en paralelo:

   + Considere un servicio con seis réplicas. Configure seis indexadores, cada uno asignado a un origen de datos con un sexto del conjunto de datos para dividir en seis todo el conjunto de datos. 

   + Dirija cada indexador al mismo índice. En el caso de las cargas de trabajo de Cognitive Search, dirija cada indexador al mismo conjunto de aptitudes.

   + Dentro de cada definición de indexador, programe el mismo patrón de ejecución del tiempo de ejecución. Por ejemplo, `"schedule" : { "interval" : "PT8H", "startTime" : "2018-05-15T00:00:00Z" }` crea una programación para el 15-05-2018 en todos los indexadores, que se ejecutará a intervalos de ocho horas.

A la hora programada, todos los indexadores empezarán a ejecutarse, cargarán los datos, aplicarán los enriquecimientos (si configuró una canalización de Cognitive Search) y escribirán en el índice. Azure Cognitive Search no bloquea las actualizaciones del índice. Las escrituras simultáneas se administran y se realiza un reintento si una escritura determinada no se realizó correctamente la primera vez.

> [!Note]
> Cuando aumente las réplicas, considere aumentar el número de particiones si se espera que el tamaño del índice aumente de manera considerable. Las particiones almacenan segmentos del contenido indexado: cuantas más particiones tenga, más pequeño será el segmento que cada una debe almacenar.

## <a name="see-also"></a>Consulte también

+ [Información general del indexador](search-indexer-overview.md)
+ [Indexing in the portal](search-import-data-portal.md) (Indexación en el portal)
+ [Azure SQL Database indexer](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md) (Indexador de Azure SQL Database)
+ [Indexador de Azure Cosmos DB](search-howto-index-cosmosdb.md)
+ [Indexador de Azure Blob Storage](search-howto-indexing-azure-blob-storage.md)
+ [Indexador de Azure Table Storage](search-howto-indexing-azure-tables.md)
+ [Seguridad en Azure Cognitive Search](search-security-overview.md)
