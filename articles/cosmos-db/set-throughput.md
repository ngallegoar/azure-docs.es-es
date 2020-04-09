---
title: Aprovisionamiento del rendimiento en contenedores y bases de datos de Azure Cosmos
description: Aprenda cómo establecer el rendimiento aprovisionado para las bases de datos y los contenedores de Azure Cosmos.
author: markjbrown
ms.author: mjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 08/12/2019
ms.openlocfilehash: e7a64776cba00a6840af70cecad5bf9c02b3f38e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79227316"
---
# <a name="provision-throughput-on-containers-and-databases"></a>Aprovisionar rendimiento en contenedores y bases de datos

Una base de datos de Azure Cosmos es una unidad de administración de un conjunto de contenedores. Una base de datos consta de un conjunto de contenedores independiente del esquema. Un contenedor de Azure Cosmos es la unidad de escalabilidad de rendimiento y almacenamiento. Un contenedor se divide horizontalmente en un conjunto de máquinas dentro de una región de Azure y se distribuye en todas las regiones de Azure asociadas a su cuenta de Azure Cosmos.

En Azure Cosmos DB, puede aprovisionar el rendimiento en dos granularidades:
 
- Contenedores de Azure Cosmos
- Bases de datos de Azure Cosmos

## <a name="set-throughput-on-a-container"></a>Establecimiento del rendimiento en un contenedor  

El rendimiento aprovisionado en un contenedor de Azure Cosmos se reserva exclusivamente para ese contenedor. El contenedor recibe el rendimiento aprovisionado todo el tiempo. El rendimiento aprovisionado en un contenedor está respaldado financieramente por los contratos de nivel de servicio. Para obtener información sobre cómo configurar el rendimiento en un contenedor, consulte [Aprovisionamiento del rendimiento en un contenedor de Azure Cosmos](how-to-provision-container-throughput.md).

La configuración del rendimiento aprovisionado en un contenedor es la opción más utilizada. Puede escalar elásticamente el rendimiento de un contenedor con el aprovisionamiento de cualquier cantidad de rendimiento mediante el uso de [Unidades de solicitud (RU)](request-units.md). 

El rendimiento aprovisionado de un contenedor se distribuye uniformemente entre las particiones físicas y, suponiendo una buena clave de partición que distribuye las particiones lógicas uniformemente entre las particiones físicas, el rendimiento también se distribuye uniformemente entre todas las particiones lógicas del contenedor. No puede especificar de forma selectiva el rendimiento de las particiones lógicas. Puesto que una o varias particiones lógicas de un contenedor se hospedan en una partición física, las particiones físicas pertenecen exclusivamente al contenedor y admiten el rendimiento aprovisionado en dicho contenedor. 

Si la carga de trabajo que se ejecuta en una partición lógica consume más que el rendimiento que se ha asignado a esa partición lógica, las operaciones tienen una velocidad limitada. Cuando se produce una limitación de velocidad, puede aumentar el rendimiento aprovisionado de todo el contenedor o volver a intentar las operaciones. Para más información sobre las particiones, consulte [Particiones lógicas](partition-data.md).

Se recomienda configurar el rendimiento en la granularidad del contenedor cuando se quiere un rendimiento garantizado para dicho contenedor.

En la imagen siguiente se muestra cómo una partición física hospeda una o varias particiones lógicas de un contenedor:

![Partición física](./media/set-throughput/resource-partition.png)

## <a name="set-throughput-on-a-database"></a>Establecimiento del rendimiento en una base de datos

Al aprovisionar el rendimiento en una base de datos de Azure Cosmos, dicho rendimiento se comparte entre todos los contenedores (denominados contenedores de base de datos compartidos) de la base de datos. Una excepción es si ha especificado un rendimiento aprovisionado en determinados contenedores de la base de datos. Compartir el rendimiento aprovisionado de nivel de base de datos entre sus contenedores equivale a hospedar una base de datos en un clúster de máquinas. Dado que todos los contenedores de una base de datos comparten los recursos disponibles en una máquina, no obtendrá un rendimiento predecible en ningún contenedor específico. Para obtener información sobre cómo configurar el rendimiento aprovisionado en una base de datos, consulte [Aprovisionamiento del rendimiento en una base de datos de Azure Cosmos](how-to-provision-database-throughput.md).

La configuración del rendimiento en una base de datos de Azure Cosmos garantiza la recepción del rendimiento aprovisionado para esa base de datos todo el tiempo. Dado que todos los contenedores de la base de datos comparten el rendimiento aprovisionado, Azure Cosmos DB no proporciona ninguna garantía de rendimiento predecible para un contenedor determinado de dicha base de datos. La parte del rendimiento que puede recibir un contenedor específico depende de los siguientes factores:

* El número de contenedores.
* La elección de las claves de partición de varios contenedores.
* La distribución de la carga de trabajo en varias particiones lógicas de los contenedores. 

Se recomienda configurar el rendimiento en una base de datos si quiere compartir el rendimiento entre varios contenedores, pero no dedicarlo a ningún contenedor específico. 

En los siguientes ejemplos se muestra dónde es preferible aprovisionar el rendimiento en el nivel de base de datos:

* El uso compartido del rendimiento aprovisionado de una base de datos a través de un conjunto de contenedores es útil para una aplicación de varios inquilinos. Cada usuario puede representarse mediante un contenedor de Azure Cosmos diferente.

* El uso compartido del rendimiento aprovisionado de una base de datos a través de un conjunto de contenedores es útil al migrar una base de datos de NoSQL, como MongoDB o Cassandra, hospedada en un clúster de máquinas virtuales o desde servidores físicos locales a Azure Cosmos DB. Puede considerar el rendimiento aprovisionado configurado en la base de datos de Azure Cosmos como un equivalente lógico, pero más rentable y flexible, de la capacidad de proceso del clúster de MongoDB o Cassandra.  

Todos los contenedores creados en una base de datos con rendimiento aprovisionado deben crearse con una [clave de partición](partition-data.md). En cualquier momento, el rendimiento asignado a un contenedor de una base de datos se distribuye entre todas las particiones lógicas de dicho contenedor. Cuando hay contenedores que comparten el rendimiento aprovisionado configurado en una base de datos, el rendimiento no se puede aplicar de forma selectiva a un contenedor específico o a una partición lógica. 

Si la carga de trabajo en una partición lógica consume más que el rendimiento que se asignó a una partición lógica específica, las operaciones tendrán una velocidad limitada. Cuando se produce una limitación de velocidad, puede aumentar el rendimiento de toda la base de datos o volver a intentar las operaciones. Para más información sobre las particiones, consulte [Particiones lógicas](partition-data.md).

Los contenedores de una base de datos de rendimiento compartido comparten el rendimiento (RU/s) asignado a dicha base de datos. Puede tener hasta cuatro contenedores con un mínimo de 400 RU/s en la base de datos. Cada nuevo contenedor después de los cuatro primeros requerirá un mínimo de 100 RU/s adicionales. Por ejemplo, si tiene una base de datos de rendimiento compartido con ocho contenedores, el mínimo de RU/s en la base de datos será de 800 RU/s.

> [!NOTE]
> En febrero de 2020, se presentó un cambio que permite tener un máximo de 25 contenedores en una base de datos de rendimiento compartida, lo que permite el uso compartido del rendimiento entre los contenedores. Después de los primeros 25 contenedores, solo puede agregar más a la base de datos si están [aprovisionados con un rendimiento dedicado](#set-throughput-on-a-database-and-a-container), que es independiente del rendimiento compartido de la base de datos.<br>
Si la cuenta de Azure Cosmos DB ya contiene una base de datos de rendimiento compartida con >= 25 contenedores, la cuenta y todas las demás cuentas de la misma suscripción de Azure están exentas de este cambio. [Póngase en contacto con el servicio de soporte técnico](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) si tiene comentarios o preguntas. 

Si las cargas de trabajo implican eliminar y volver a crear todas las colecciones de una base de datos, se recomienda quitar la base de datos vacía y volver a crear una nueva base de datos antes de la creación de la colección. En la siguiente imagen se muestra cómo una partición física puede hospedar una o varias particiones lógicas que pertenecen a distintos contenedores dentro de una base de datos:

![Partición física](./media/set-throughput/resource-partition2.png)

## <a name="set-throughput-on-a-database-and-a-container"></a>Establecimiento del rendimiento en un contenedor y una base de datos

Puede combinar los dos modelos. Se permite el aprovisionamiento del rendimiento tanto en la base de datos como en el contenedor. En el ejemplo siguiente se muestra cómo aprovisionar el rendimiento en un contenedor y una base de datos de Azure Cosmos:

* Puede crear una base de datos de Azure Cosmos llamada *Z* con rendimiento aprovisionado de RU de *"K"* . 
* Después, cree cinco contenedores llamados *A*, *B*, *C*, *D* y *E* en la base de datos. Al crear el contenedor B, asegúrese de habilitar la opción **Provision dedicated throughput for this container** (Aprovisionar el rendimiento dedicado para este contenedor) y configurar de forma explícita las Unidades de solicitud *"P"* del rendimiento aprovisionado en este contenedor. Tenga en cuenta que únicamente puede configurar el rendimiento compartido y dedicado al crear la base de datos y el contenedor. 

   ![Configuración del rendimiento en el nivel de contenedor](./media/set-throughput/coll-level-throughput.png)

* El rendimiento de las Unidades de solicitud *"K"* se comparte entre los cuatro contenedores *A*, *C*, *D* y *E*. La cantidad exacta de rendimiento disponible en *A*, *C*, *D* o *E* varía. No hay ningún Acuerdo de Nivel de Servicio para el rendimiento de cada contenedor individual.
* Se garantiza que el contenedor llamado *B* obtendrá el rendimiento de las Unidades de solicitud de *"P"* todo el tiempo. Estará respaldado por los Acuerdos de Nivel de Servicio.

> [!NOTE]
> No se puede convertir un contenedor con rendimiento aprovisionado en un contenedor de base de datos compartido. A la inversa, un contenedor de base de datos compartido no se puede convertir para tener un rendimiento dedicado.

## <a name="update-throughput-on-a-database-or-a-container"></a>Actualización del rendimiento en un contenedor o una base de datos

Después de crear un contenedor o una base de datos de Cosmos Azure, se puede actualizar el rendimiento aprovisionado. No existen límites para el rendimiento aprovisionado máximo que se puede configurar en la base de datos o el contenedor. El [rendimiento aprovisionado mínimo](concepts-limits.md#storage-and-throughput) depende de los factores siguientes: 

* Tamaño máximo de los datos que se han almacenado en algún momento en el contenedor.
* Rendimiento máximo que se ha aprovisionado en algún momento en el contenedor.
* El número actual de contenedores de Azure Cosmos que tiene en una base de datos con rendimiento compartido. 

Puede recuperar el rendimiento mínimo de un contenedor o una base de datos mediante programación con los SDK o ver el valor en Azure Portal. Al usar el SDK de .NET, el método [DocumentClient.ReplaceOfferAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.documentclient.replaceofferasync?view=azure-dotnet) permite escalar el valor de rendimiento aprovisionado. Al usar el SDK de Java, el método [RequestOptions.setOfferThroughput](sql-api-java-samples.md#offer-examples) permite escalar el valor de rendimiento aprovisionado. 

Al usar el SDK de .NET, el método [DocumentClient.ReadOfferAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.documentclient.readofferasync?view=azure-dotnet) permite recuperar el rendimiento mínimo de un contenedor o una base de datos. 

Puede escalar el rendimiento aprovisionado de un contenedor o una base de datos en cualquier momento. Cuando se realiza una operación de escala para aumentar el rendimiento, puede tardar más tiempo debido a las tareas del sistema destinadas a aprovisionar los recursos necesarios. Puede comprobar el estado de la operación de escalado en Azure Portal o mediante programación con el SDK. Al usar el SDK de .NET, puede obtener el estado de la operación de escalado mediante el método `DocumentClient.ReadOfferAsync`.

## <a name="comparison-of-models"></a>Comparación de modelos

|**Parámetro**  |**Rendimiento aprovisionado en una base de datos**  |**Rendimiento aprovisionado en un contenedor**|
|---------|---------|---------|
|Número mínimo de RU |400 (después de los cuatro primeros contenedores, cada contenedor adicional requiere un mínimo de 100 RU por segundo). |400|
|Número mínimo de RU por contenedor|100|400|
|Número máximo de RU|Ilimitado, en la base de datos.|Ilimitado, en el contenedor.|
|RU asignadas o disponibles para un contenedor específico|Sin garantías. Las RU asignadas a un contenedor determinado dependen de las propiedades. Las propiedades pueden ser la elección de las claves de partición de contenedores que comparten el rendimiento, la distribución de la carga de trabajo y el número de contenedores. |Todas las RU configuradas en el contenedor se reservan exclusivamente para el contenedor.|
|Almacenamiento máximo de un contenedor|Sin límite.|Sin límite.|
|Rendimiento máximo por partición lógica de un contenedor|10 000 RU|10 000 RU|
|Almacenamiento máximo (datos + índice) por partición lógica de un contenedor|20 GB|20 GB|

## <a name="next-steps"></a>Pasos siguientes

* Más información sobre las [particiones lógicas](partition-data.md).
* Obtenga más información sobre el [aprovisionamiento del rendimiento de un contenedor de Azure Cosmos](how-to-provision-container-throughput.md).
* Obtenga más información sobre el [aprovisionamiento del rendimiento de una base de datos de Azure Cosmos](how-to-provision-database-throughput.md).

