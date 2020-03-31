---
title: Obtención de acceso a la fuente de cambios en Azure Cosmos DB
description: En este artículo se describen las diferentes opciones disponibles para leer y obtener acceso a la fuente de cambios de Azure Cosmos DB.
author: TheovanKraay
ms.author: thvankra
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 11/25/2019
ms.openlocfilehash: fc7e78a44d03af8952c1e178a3e92b1ee0c6fe66
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "74688128"
---
# <a name="reading-azure-cosmos-db-change-feed"></a>Lectura de la fuente de cambios de Azure Cosmos DB

Puede trabajar con la fuente de cambios de Azure Cosmos DB mediante cualquiera de las siguientes opciones:

* Uso de Azure Functions
* Uso de la biblioteca de procesadores de fuente de cambios
* Uso del SDK de la API de SQL de Azure Cosmos DB

## <a name="using-azure-functions"></a>Uso de Azure Functions

Azure Functions es la opción más sencilla y recomendada. Cuando cree un desencadenador de Azure Functions, puede seleccionar el contenedor al que conectarse, y la función se desencadenará siempre que se realice un cambio en el contenedor. Los desencadenadores pueden crearse en el portal de Azure Functions, en el portal de Azure Cosmos DB o mediante programación con SDK. Visual Studio y VS Code proporcionan compatibilidad para escribir funciones de Azure Functions e incluso puede usar la CLI de Azure Functions para el desarrollo multiplataforma. Puede escribir y depurar el código en el escritorio y, luego, implementar la función con un solo clic. Consulte los artículos [Informática de base de datos sin servidor con Azure Functions](serverless-computing-database.md) y [Using change feed with Azure Functions](change-feed-functions.md) (Uso de la fuente de cambios con Azure Functions) para obtener más información.

## <a name="using-the-change-feed-processor-library"></a>Uso de la biblioteca de procesadores de fuente de cambios

La biblioteca de procesadores de fuente de cambios oculta la complejidad y le ofrece un control completo sobre la fuente de cambios. Esta biblioteca sigue el patrón de observador, en el que la biblioteca llama a la función de procesamiento. Si tiene una fuente de cambios de alto rendimiento, puede crear instancias de varios clientes para leer la fuente de cambios. Como usa la biblioteca de procesadores de fuente de cambios, dividirá automáticamente la carga en distintos clientes sin tener que implementar esta lógica. La biblioteca controla toda la complejidad. Si quiere tener su propio equilibrador de carga, puede implementar `IPartitionLoadBalancingStrategy` para que una estrategia de partición personalizada procese la fuente de cambios. Para obtener más información, consulte [using change feed processor library](change-feed-processor.md) (Uso de la biblioteca de procesadores de fuente de cambios).

## <a name="using-the-azure-cosmos-db-sql-api-sdk"></a>Uso del SDK de la API de SQL de Azure Cosmos DB

Con el SDK, obtendrá un control de bajo nivel de la fuente de cambios. Puede administrar el punto de control, obtener acceso a una clave de partición determinada, etc. Si tiene varios lectores, puede usar `ChangeFeedOptions` para distribuir la carga de lectura en subprocesos o clientes distintos.

## <a name="change-feed-in-apis-for-cassandra-and-mongodb"></a>Fuente de cambios en las API de Cassandra y MongoDB

La funcionalidad de fuente de cambios aparece como flujo de cambios en la API de MongoDB y como consulta con predicado en Cassandra API. Para más información sobre los detalles de implementación de la API de MongoDB, consulte [Flujos de cambios en la API de Azure Cosmos DB para MongoDB](mongodb-change-streams.md).

Apache Cassandra nativo proporciona captura de datos modificados (CDC), un mecanismo para marcar tablas concretas para el archivado, así como para rechazar escrituras en esas tablas una vez que se alcanza un tamaño en disco que se puede configurar para el registro de CDC. La característica de fuente de cambios de la API de Azure Cosmos DB para Cassandra mejora la capacidad de consultar los cambios con el predicado a través de CQL. Para más información sobre los detalles de implementación, consulte [Fuente de cambios de la API de Azure Cosmos DB para Cassandra](cassandra-change-feed.md).

## <a name="next-steps"></a>Pasos siguientes

Ahora, puede obtener más información acerca de las fuentes de cambios en los siguientes artículos:

* [Introducción a la fuente de cambios](change-feed.md)
* [Using change feed with Azure Functions](change-feed-functions.md) (Usar la fuente de cambios con Azure Functions)
* [Uso de la biblioteca de procesadores de fuente de cambios](change-feed-processor.md)
