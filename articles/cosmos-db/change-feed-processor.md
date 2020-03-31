---
title: Biblioteca de procesadores de fuente de cambios de Azure Cosmos DB
description: Aprenda a usar la biblioteca de procesadores de fuente de cambios de Azure Cosmos DB para leer la fuente de cambios y los componentes del procesador de fuente de cambios.
author: markjbrown
ms.author: mjbrown
ms.service: cosmos-db
ms.devlang: dotnet
ms.topic: conceptual
ms.date: 12/03/2019
ms.reviewer: sngun
ms.openlocfilehash: e71b2807595aebeb1f0c8682fde119f4e267e55d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "78273305"
---
# <a name="change-feed-processor-in-azure-cosmos-db"></a>Procesadores de fuente de cambios de Azure Cosmos DB 

El procesador de fuente de cambios es parte del [SDK V3 de Azure Cosmos DB](https://github.com/Azure/azure-cosmos-dotnet-v3). Simplifica el proceso de lectura de la fuente de cambios y distribuye el procesamiento de eventos entre varios consumidores de manera eficaz.

La principal ventaja de la biblioteca de procesador de fuente de cambios es su comportamiento tolerante a errores que garantiza una entrega "al menos una vez" de todos los eventos de la fuente de cambios.

## <a name="components-of-the-change-feed-processor"></a>Componentes del procesador de fuente de cambios

Hay cuatro componentes principales en la implementación del procesador de fuente de cambios: 

1. **El contenedor supervisado:** el contenedor supervisado tiene los datos a partir de los cuales se genera la fuente de cambios. Todas las inserciones y actualizaciones realizadas en el contenedor supervisado se reflejan en la fuente de cambios del contenedor.

1. **El contenedor de concesión**: El contenedor de concesión actúa como un almacenamiento de estado y coordina el procesamiento de la fuente de cambios entre varios trabajadores. El contenedor de concesión se puede almacenar en la misma cuenta que el contenedor supervisado o en una cuenta independiente. 

1. **El host:** Un host es una instancia de aplicación que usa el procesador de fuente de cambios para escuchar los cambios. Se pueden ejecutar varias instancias con la misma configuración de concesión en paralelo, pero cada instancia debe tener **un nombre de instancia** diferente. 

1. **El delegado:** El delegado es el código que define lo que usted, el desarrollador, desea hacer con cada lote de cambios que el procesador de la fuente de cambios lea. 

Para comprender mejor cómo funcionan estos cuatro elementos del procesador de fuente de cambios juntos, echemos un vistazo a un ejemplo en el diagrama siguiente. El contenedor supervisado almacena documentos y usa "City" como clave de partición. Vemos que los valores de la clave de partición se distribuyen en intervalos que contienen elementos. Hay dos instancias de host y el procesador de fuente de cambios está asignando distintos intervalos de valores de clave de partición a cada instancia para maximizar la distribución de proceso. Cada intervalo se lee en paralelo y su progreso se mantiene por separado de otros intervalos en el contenedor de concesión.

![Ejemplo de procesador de fuente de cambios](./media/change-feed-processor/changefeedprocessor.png)

## <a name="implementing-the-change-feed-processor"></a>Implementación del procesador de fuente de cambios

El punto de entrada es siempre el contenedor supervisado, desde una instancia de `Container` se llama a `GetChangeFeedProcessorBuilder`:

[!code-csharp[Main](~/samples-cosmosdb-dotnet-change-feed-processor/src/Program.cs?name=DefineProcessor)]

El primer parámetro es un nombre distintivo que describe el objetivo de este procesador y el segundo nombre es la implementación de delegado que controlará los cambios. 

Un ejemplo de un delegado sería:


[!code-csharp[Main](~/samples-cosmosdb-dotnet-change-feed-processor/src/Program.cs?name=Delegate)]

Por último, se define un nombre para esta instancia del procesador con `WithInstanceName`, que es el contenedor con el que se mantiene el estado de concesión con `WithLeaseContainer`.

La llamada a `Build` le proporcionará la instancia del procesador que puede iniciar mediante una llamada a `StartAsync`.

## <a name="processing-life-cycle"></a>Ciclo de vida de procesamiento

El ciclo de vida normal de una instancia de host es:

1. Leer la fuente de cambios.
1. Si no hay ningún cambio, mantenerse suspendida durante un periodo de tiempo predefinido (personalizable con `WithPollInterval` en el generador) e ir a #1.
1. Si hay cambios, enviarlos al **delegado.**
1. Cuando el delegado termina de procesar los cambios **correctamente**, actualizar el almacén de concesión con el último punto en el tiempo e ir al nº 1.

## <a name="error-handling"></a>Control de errores

El procesador de fuente de cambios es resistente a los errores de código de usuario. Esto significa que si la implementación del delegado tiene una excepción no controlada (paso nº 4), el subproceso que está procesando ese lote específico de cambios se detendrá y se creará un nuevo subproceso. El nuevo subproceso comprobará cuál fue el último punto en el tiempo que el almacén de concesiones tiene para ese intervalo de valores de clave de partición, y se reiniciará desde allí, enviando de forma eficaz el mismo lote de cambios al delegado. Este comportamiento continuará hasta que el delegado procese los cambios correctamente, y es el motivo por el que el procesador de fuente de cambios tiene una garantía de "al menos una vez", ya que, si se produce un error en el código de delegado, ese lote se volverá a intentar.

## <a name="dynamic-scaling"></a>Escalado dinámico

Como se mencionó en la introducción, el procesador de fuente de cambios puede distribuir el proceso entre varias instancias de forma automática. Puede implementar varias instancias de la aplicación mediante el procesador de fuente de cambios y sacar provecho de ello, los únicos requisitos clave son los siguientes:

1. Todas las instancias deben tener la misma configuración de contenedor de concesión.
1. Todas las instancias deben tener el mismo nombre de flujo de trabajo.
1. Cada instancia tiene que tener un nombre de instancia diferente (`WithInstanceName`).

Si estas tres condiciones son aplicables, el procesador de fuente de cambios, mediante el uso de un algoritmo de distribución equitativa, distribuye todas las concesiones en el contenedor de concesión en todas las instancias en ejecución y paraleliza el proceso. Una concesión solo puede ser propiedad de una instancia en un momento dado, por lo que el número máximo de instancias es igual al número de concesiones.

El número de instancias pueden aumentar y reducir, y el procesador de fuente de cambios ajustará dinámicamente la carga redistribuyéndola en consecuencia.

Además, el procesador de fuente de cambios puede ajustarse de forma dinámica a la escala de los contenedores debido a un aumento en el rendimiento o almacenamiento. Cuando el contenedor crece, el procesador de fuente de cambios controla de forma transparente estos escenarios al aumentar dinámicamente las concesiones y distribuir las nuevas concesiones entre las instancias existentes.

## <a name="change-feed-and-provisioned-throughput"></a>Fuente de cambios y rendimiento aprovisionado

Se le cobrarán las RU consumidas, puesto que la entrada y la salida de datos de contenedores de Cosmos siempre consumen RU. Se le cobrarán las RU consumidas por el contenedor de concesión.

## <a name="additional-resources"></a>Recursos adicionales

* [SDK de Azure Cosmos DB](sql-api-sdk-dotnet.md)
* [Ejemplos de uso en GitHub](https://github.com/Azure/azure-cosmos-dotnet-v3/tree/master/Microsoft.Azure.Cosmos.Samples/Usage/ChangeFeed)
* [Ejemplos adicionales en GitHub](https://github.com/Azure-Samples/cosmos-dotnet-change-feed-processor)

## <a name="next-steps"></a>Pasos siguientes

Puede obtener más información sobre el procesador de la fuente de cambios en los siguientes artículos:

* [Introducción a la fuente de cambios](change-feed.md)
* [Migración desde la biblioteca de procesadores de fuente de cambios](how-to-migrate-from-change-feed-library.md)
* [Uso del calculador de la fuente de cambios](how-to-use-change-feed-estimator.md)
* [Hora de inicio del procesador de la fuente de cambios](how-to-configure-change-feed-start-time.md)
