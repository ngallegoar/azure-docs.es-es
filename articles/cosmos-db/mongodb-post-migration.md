---
title: Pasos de optimización posteriores a la migración con la API de Azure Cosmos DB para MongoDB
description: En este documento se proporcionan las técnicas de optimización posteriores a la migración de MongoDB a la API de Azure Cosmos DB para MongoDB.
author: christopheranderson
ms.service: cosmos-db
ms.subservice: cosmosdb-mongo
ms.topic: how-to
ms.date: 03/20/2020
ms.author: chrande
ms.openlocfilehash: 300177b9d5a20ce8082db57837be3ff461fd51a0
ms.sourcegitcommit: 6a902230296a78da21fbc68c365698709c579093
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/05/2020
ms.locfileid: "93361663"
---
# <a name="post-migration-optimization-steps-when-using-azure-cosmos-dbs-api-for-mongodb"></a>Pasos de optimización posteriores a la migración cuando se usa la API de Azure Cosmos DB para MongoDB
[!INCLUDE[appliesto-mongodb-api](includes/appliesto-mongodb-api.md)]

Después de migrar los datos almacenados en la base de datos de MongoDB a la API de Azure Cosmos DB para MongoDB, puede conectarse a Azure Cosmos DB y administrar los datos. En esta guía se proporcionan los pasos que se deben tener en cuenta después de la migración. Para seguir los pasos de migración, consulte [Tutorial de migración de MongoDB a la API de Azure Cosmos DB para MongoDB](../dms/tutorial-mongodb-cosmos-db.md).

En esta guía, aprenderá a:

- [Conectarse a la aplicación](#connect-your-application)
- [Optimizar la directiva de indexación](#optimize-the-indexing-policy)
- [Configurar la distribución global para la API de Azure Cosmos DB para MongoDB](#globally-distribute-your-data)
- [Establecer un nivel de coherencia](#set-consistency-level)

> [!NOTE]
> El único paso obligatorio posterior a la migración en el nivel de aplicación es cambiar la cadena de conexión en la aplicación para que apunte a la nueva cuenta de Azure Cosmos DB. El resto de pasos de migración son optimizaciones recomendadas.
>

## <a name="connect-your-application"></a>Conexión de la aplicación

1. En una nueva ventana, inicie sesión en [Azure Portal](https://www.portal.azure.com/).
2. En [Azure Portal](https://www.portal.azure.com/), en el panel izquierdo abra el menú **Todos los recursos** y busque la cuenta de Azure Cosmos DB a la que se han migrado los datos.
3. Abra la hoja **Cadena de conexión**. El panel derecho contiene toda la información que necesita para conectarse correctamente a la cuenta.
4. Use la información de conexión en la configuración de la aplicación (o en otros lugares pertinentes) para reflejar la API de Azure Cosmos DB para la conexión de MongoDB en la aplicación.
:::image type="content" source="./media/mongodb-post-migration/connection-string.png" alt-text="Captura de pantalla que muestra la configuración de una cadena de conexión.":::

Para obtener más información, consulte la página [Conectar una aplicación de MongoDB a Azure Cosmos DB](connect-mongodb-account.md).

## <a name="optimize-the-indexing-policy"></a>Optimización de la directiva de indexación

Durante la migración de datos a Azure Cosmos DB, todos los campos de datos se indexan automáticamente de forma predeterminada. En muchos casos, esta directiva de indexación predeterminada es aceptable. En general, eliminar los índices optimiza las solicitudes de escritura y tener la directiva de indexación predeterminada (es decir, la indexación automática) optimiza las solicitudes de lectura.

Para obtener más información sobre la indexación, consulte los artículos [Indexación de datos en la API de Azure Cosmos DB para MongoDB](mongodb-indexing.md) e [Indexación en Azure Cosmos DB](index-overview.md).

## <a name="globally-distribute-your-data"></a>Distribución global de los datos

Azure Cosmos DB está disponible en todas las [regiones de Azure](https://azure.microsoft.com/regions/#services) globales. Tras seleccionar el nivel de coherencia predeterminado para la cuenta de Azure Cosmos DB, puede asociar una o varias regiones de Azure (según las necesidades de distribución global). Para una alta disponibilidad y una continuidad del negocio, siempre se recomienda que se ejecute en al menos 2 regiones. Puede revisar las sugerencias para [optimizar el costo de las implementaciones en varias regiones de Azure Cosmos DB](optimize-cost-regions.md).

Para distribuir globalmente los datos, consulte [Distribución de datos de forma global en la API de Azure Cosmos DB para MongoDB](tutorial-global-distribution-mongodb.md).

## <a name="set-consistency-level"></a>Establecimiento de un nivel de coherencia

Azure Cosmos DB ofrece 5 [niveles de coherencia](consistency-levels.md) bien definidos. Para obtener más información sobre la asignación entre MongoDB y los niveles de coherencia de Azure Cosmos DB, consulte [Niveles de coherencia y API de Cosmos DB](./consistency-levels.md). El nivel de coherencia predeterminado es el de la sesión. Cambiar el nivel de coherencia es opcional y se puede optimizar para la aplicación. Para cambiar el nivel de coherencia mediante Azure Portal:

1. En Configuración, vaya a la hoja **Coherencia predeterminada**.
2. Seleccione el [nivel de coherencia](consistency-levels.md).

La mayoría de usuarios dejan el nivel de coherencia en el valor predeterminado de coherencia de la sesión. Sin embargo, hay [compromisos entre rendimiento y disponibilidad en los distintos niveles de coherencia](./consistency-levels.md).

## <a name="next-steps"></a>Pasos siguientes

* [Conectar una aplicación de MongoDB a Azure Cosmos DB](connect-mongodb-account.md)
* [Conexión a una cuenta de Azure Cosmos DB con Studio 3T](mongodb-mongochef.md)
* [Cómo distribuir las lecturas globalmente con la API de Azure Cosmos DB para MongoDB](mongodb-readpreference.md)
* [Expiración de los datos con la API de Azure Cosmos DB para MongoDB](mongodb-time-to-live.md)
* [Niveles de coherencia en Azure Cosmos DB](consistency-levels.md)
* [Indexación en Azure Cosmos DB](index-overview.md)
* [Unidades de solicitud en Azure Cosmos DB](request-units.md)