---
title: Estimación de los costos mediante Capacity Planner de Azure Cosmos DB
description: Capacity Planner de Azure Cosmos DB permite estimar el rendimiento (RU/s) necesario y el costo de la carga de trabajo. En este artículo se describe cómo usar la nueva versión de Capacity Planner para calcular el rendimiento y los costos necesarios.
author: deborahc
ms.service: cosmos-db
ms.topic: how-to
ms.date: 07/30/2019
ms.author: dech
ms.openlocfilehash: 6a30535ac3aaa90dc3553f6901a83ab300546fb5
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85261858"
---
# <a name="estimate-rus-using-the-azure-cosmos-db-capacity-planner"></a>Estimación de RU/s mediante Capacity Planner de Azure Cosmos DB

La configuración de las bases de datos y los contenedores de Azure Cosmos con la cantidad adecuada de rendimiento aprovisionado, o [unidades de solicitud (RU/s)](request-units.md), para la carga de trabajo es esencial para optimizar el costo y el rendimiento. En este artículo se describe cómo usar [Capacity Planner](https://cosmos.azure.com/capacitycalculator/) de Azure Cosmos DB para obtener una estimación de las RU/s necesarias y el costo de la carga de trabajo. 

## <a name="how-to-estimate-throughput-and-cost-with-azure-cosmos-db-capacity-planner"></a>Estimación del rendimiento y el costo con Capacity Planner de Azure Cosmos DB

Capacity Planner se puede usar en dos modos.

|**Modo**  |**Descripción**  |
|---------|---------|
|Básico|Proporciona una estimación de costos y RU/s rápida y de alto nivel. Este modo asume la configuración de Azure Cosmos DB predeterminada para la directiva de indexación, la coherencia y otros parámetros. <br/><br/>Use el modo básico para una estimación rápida y de alto nivel cuando evalúe una carga de trabajo potencial para que se ejecute en Azure Cosmos DB.|
|Avanzado|Proporciona una estimación de costos y RU más detalladas, con la posibilidad de ajustar opciones de configuración adicionales, como la Directiva de indexación, el nivel de coherencia y otros parámetros que afectan al costo y al rendimiento. <br/><br/>Use el modo avanzado cuando esté calculando RU/s para un nuevo proyecto o desee una estimación más detallada. |


## <a name="estimate-provisioned-throughput-and-cost-using-basic-mode"></a>Estimación del rendimiento aprovisionado y el costo mediante el modo básico
Para obtener una estimación rápida de la carga de trabajo mediante el modo básico, vaya a [Capacity Planner](https://cosmos.azure.com/capacitycalculator/). Escriba los siguientes parámetros en función de la carga de trabajo: 

|**Entrada**  |**Descripción**  |
|---------|---------|
|Número de regiones|Azure Cosmos DB está disponible en todas las regiones de Azure. Seleccione el número de regiones necesarias para la carga de trabajo. Puede asociar cualquier número de regiones con su cuenta de Cosmos. Consulte la información sobre la [distribución global](distribute-data-globally.md) en Azure Cosmos DB para más detalles.|
|Escrituras de varias regiones|Si habilita [Escrituras en varias regiones](distribute-data-globally.md#key-benefits-of-global-distribution), la aplicación puede realizar operaciones de lectura y escritura en cualquier región de Azure. Si deshabilita las escrituras en varias regiones, la aplicación solo puede escribir en una región. <br/><br/> Habilite las escrituras en varias regiones si espera tener una carga de trabajo activa-activa que requiera escrituras de baja latencia en diferentes regiones. Por ejemplo, una carga de trabajo de IOT que escribe datos en la base de datos en grandes volúmenes en regiones diferentes. <br/><br/> Las escrituras en varias regiones garantizan un 99,999 % de disponibilidad de lectura y escritura. Las escrituras en varias regiones requieren más rendimiento en comparación con las regiones de escritura únicas. Para más información, consulte el artículo sobre [cómo se diferencian las RU para regiones de una sola escritura y de varias escrituras](optimize-cost-regions.md).|
|Total de datos almacenados (por región)|Total de datos almacenados estimados en GB en una sola región.|
|Tamaño del elemento|El tamaño estimado del elemento de datos (por ejemplo, un documento), que va de 1 KB a 2 MB. |
|Lecturas/s por región|Número de lecturas esperadas por segundo. |
|Escrituras/s por región|Número de escrituras esperadas por segundo. |

Después de rellenar los detalles necesarios, seleccione **Calcular**. La pestaña **Cost Estimate** (Estimación del costo) muestra el costo total del almacenamiento y el rendimiento aprovisionado. Puede expandir el vínculo **Mostrar detalles** de esta pestaña para obtener el desglose del rendimiento necesario para las solicitudes de lectura y escritura. Cada vez que cambie el valor de cualquier campo, seleccione **Calcular** para volver a calcular el costo estimado. 

:::image type="content" source="./media/estimate-ru-with-capacity-planner/basic-mode.png" alt-text="Modo básico de Capacity Planner":::

## <a name="estimate-provisioned-throughput-and-cost-using-advanced-mode"></a>Estimación del rendimiento aprovisionado y del costo mediante el modo avanzado

El modo avanzado permite proporcionar más configuraciones que afectan a la estimación de RU/s. Para usar esta opción, vaya a [Capacity Planner](https://cosmos.azure.com/capacitycalculator/) e inicie sesión en la herramienta con una cuenta que use para Azure. La opción de inicio de sesión está disponible en la esquina derecha. 

Después de iniciar sesión, puede ver campos adicionales en comparación con los campos del modo básico. Escriba los parámetros adicionales en función de la carga de trabajo. 

|**Entrada**  |**Descripción**  |
|---------|---------|
|API|Azure Cosmos DB es un servicio de varios modelos y varias API. Para nuevas cargas de trabajo, seleccione la API SQL (Core). |
|Número de regiones|Azure Cosmos DB está disponible en todas las regiones de Azure. Seleccione el número de regiones necesarias para la carga de trabajo. Puede asociar cualquier número de regiones con su cuenta de Cosmos. Consulte la información sobre la [distribución global](distribute-data-globally.md) en Azure Cosmos DB para más detalles.|
|Escrituras de varias regiones|Si habilita [Escrituras en varias regiones](distribute-data-globally.md#key-benefits-of-global-distribution), la aplicación puede realizar operaciones de lectura y escritura en cualquier región de Azure. Si deshabilita las escrituras en varias regiones, la aplicación solo puede escribir en una región. <br/><br/> Habilite las escrituras en varias regiones si espera tener una carga de trabajo activa-activa que requiera escrituras de baja latencia en diferentes regiones. Por ejemplo, una carga de trabajo de IOT que escribe datos en la base de datos en grandes volúmenes en regiones diferentes. <br/><br/> Las escrituras en varias regiones garantizan un 99,999 % de disponibilidad de lectura y escritura. Las escrituras en varias regiones requieren más rendimiento en comparación con las regiones de escritura únicas. Para más información, consulte el artículo sobre [cómo se diferencian las RU para regiones de una sola escritura y de varias escrituras](optimize-cost-regions.md).|
|Coherencia predeterminada|Azure Cosmos DB admite cinco niveles de coherencia, para permitir a los desarrolladores encontrar un punto de equilibrio entre la coherencia, la disponibilidad y los ajustes de la latencia. Para más información, consulte el artículo [Niveles de coherencia](consistency-levels.md). <br/><br/> De forma predeterminada, Azure Cosmos DB usa la coherencia de la sesión, lo que garantiza la capacidad de leer sus propias escrituras en una sesión. <br/><br/> La elección de la obsolescencia limitada o fuerte requerirá el doble de RU/s necesarias para las lecturas, en comparación con la sesión, el prefijo coherente y la coherencia final. La coherencia alta con escrituras en varias regiones no se admite y se establecerá automáticamente de forma predeterminada en las escrituras en una sola región con una coherencia alta. |
|Directiva de indexación|De forma predeterminada, Azure Cosmos DB [indexa todas las propiedades](index-policy.md) de todos los elementos para consultas flexibles y eficientes (se asigna a la directiva de indexación **Automática**). <br/><br/> Si elige **Desactivado**, ninguna de las propiedades se indexará. El resultado de todo esto es el cargo más bajo de RU para las escrituras. Seleccione **Desactivado** para la directiva si espera realizar solamente [lecturas de punto](https://docs.microsoft.com/dotnet/api/microsoft.azure.cosmos.container.readitemasync?view=azure-dotnet) (búsquedas de valores de clave) o escrituras, y ninguna consulta. <br/><br/> La directiva de indexación personalizada permite incluir o excluir propiedades específicas del índice para reducir el rendimiento y el almacenamiento de escritura. Para más información, consulte los artículos sobre la [directiva de indexación](index-overview.md) y las [directivas de indexación de ejemplo](how-to-manage-indexing-policy.md#indexing-policy-examples).|
|Total de datos almacenados (por región)|Total de datos almacenados estimados en GB en una sola región.|
|Modo de carga de trabajo|Seleccione **Estable** si el volumen de la carga de trabajo es constante. <br/><br/> Seleccione **Variable** si el volumen de la carga de trabajo cambia con el tiempo.  Por ejemplo, durante un día o un mes concreto. <br/><br/> Las siguientes configuraciones están disponibles si elige la opción de carga de trabajo variable:<ul><li>Porcentaje de tiempo máximo: porcentaje de tiempo en un mes en el que la carga de trabajo requiere un rendimiento máximo (el más alto). <br/><br/> Por ejemplo, si tiene una carga de trabajo que tiene una actividad elevada de 9 a.m. a 6 p.m. en el horario comercial entre semana, el porcentaje de tiempo máximo es: 45 horas en período máximo / 730 horas / mes = ~ 6 %.<br/><br/></li><li>Lecturas/s por región en período máximo: número de lecturas esperadas por segundo en el período máximo.</li><li>Escrituras/s por región en período máximo: número de escrituras esperadas por segundo en el período máximo.</li><li>Lecturas/s por región fuera del período máximo: número de lecturas esperadas por segundo fuera del período máximo.</li><li>Escrituras/s por región fuera del período máximo: número de escrituras esperadas por segundo fuera del período máximo.</li></ul>Con intervalos en el período máximo y fuera del período máximo, puede optimizar el costo [escalando y reduciendo verticalmente mediante programación el rendimiento](set-throughput.md#update-throughput-on-a-database-or-a-container) en consecuencia.|
|Tamaño del elemento|El tamaño del elemento de datos (por ejemplo, un documento), que va de 1 KB a 2 MB. <br/><br/>También puede **cargar el documento de ejemplo (JSON)** para una estimación más precisa.<br/><br/>Si la carga de trabajo tiene varios tipos de elementos (con contenido JSON diferente) en el mismo contenedor, puede cargar varios documentos JSON y obtener la estimación. Use el botón **Agregar nuevo elemento** para agregar varios documentos JSON de ejemplo.|

También puede usar el botón **Guardar estimación** para descargar un archivo CSV que contenga la estimación actual. 

:::image type="content" source="./media/estimate-ru-with-capacity-planner/advanced-mode.png" alt-text="Modo avanzado de Capacity Planner":::

Los precios que se muestran en Capacity Planner de Azure Cosmos DB son estimaciones en función de las tarifas de precios públicas para el rendimiento y el almacenamiento. Todos los precios se muestran en dólares estadounidenses. Consulte la [página de precios de Azure Cosmos DB](https://azure.microsoft.com/pricing/details/cosmos-db/) para ver todas las tarifas por región.  

## <a name="estimating-throughput-for-queries"></a>Estimación del rendimiento para consultas

La calculadora de capacidad de Azure Cosmos asume las lecturas de punto (una lectura de un único elemento, como un documento, por identificador y el valor de clave de partición) y las escrituras para la carga de trabajo. Para estimar el rendimiento necesario para las consultas, ejecute la consulta en un conjunto de datos representativo de un contenedor de Cosmos y [obtenga el cargo de RU](find-request-unit-charge.md). Multiplique el cargo de RU por el número de consultas que prevé que se ejecuten por segundo para obtener el total de RU/s requerido. 

Por ejemplo, si la carga de trabajo requiere una consulta, ``SELECT * FROM c WHERE c.id = 'Alice'`` se ejecuta 100 veces por segundo y el cargo de RU de la consulta es de 10 RU, necesitará 100 consulta/s * 10 RU / consulta = 1000 RU/s en total para atender estas solicitudes. Agregue estas RU/s a las RU/s necesarias para las lecturas o escrituras que se produzcan en la carga de trabajo.

## <a name="next-steps"></a>Pasos siguientes

* Obtenga más información sobre el [modelo de precios de Azure Cosmos DB](how-pricing-works.md).
* Cree una nueva [cuenta, base de datos y contenedor de Cosmos](create-cosmosdb-resources-portal.md).
* Obtenga información para [optimizar el costo del rendimiento aprovisionado](optimize-cost-throughput.md).
* Obtenga información para [optimizar el costo con capacidad reservada](cosmos-db-reserved-capacity.md).

