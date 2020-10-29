---
title: Aprovisionamiento del rendimiento de base de datos en SQL API de Azure Cosmos DB
description: Aprenda a aprovisionar el rendimiento en el nivel de base de datos en SQL API de Azure Cosmos DB mediante Azure Portal, la CLI, PowerShell y otros SDK.
author: markjbrown
ms.service: cosmos-db
ms.topic: how-to
ms.date: 10/15/2020
ms.author: mjbrown
ms.custom: devx-track-azurecli, devx-track-csharp
ms.openlocfilehash: 347da75d8e3f58d3875b02000d74533b36a9b18c
ms.sourcegitcommit: 3bcce2e26935f523226ea269f034e0d75aa6693a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/23/2020
ms.locfileid: "92486113"
---
# <a name="provision-standard-manual-throughput-on-a-database-in-azure-cosmos-db---sql-api"></a>Aprovisionamiento del rendimiento estándar (manual) en una base de datos de Azure Cosmos DB: SQL API

En este artículo se explica cómo aprovisionar el rendimiento estándar (manual) en una base de datos en SQL API de Azure Cosmos DB. Puede aprovisionar el rendimiento de un único [contenedor](how-to-provision-container-throughput.md), o bien de una base de datos y compartir el rendimiento entre los contenedores que se incluyen en ella. Para saber cuándo se debe usar el rendimiento de nivel de contenedor y de nivel de base de datos, consulte el artículo [Aprovisionar rendimiento en contenedores y bases de datos](set-throughput.md). Para aprovisionar el rendimiento en el nivel de base de datos, se puede usar Azure Portal o los SDK de Azure Cosmos DB.

Si va a usar otra API, vea los artículos [MongoDB API](how-to-provision-throughput-mongodb.md), [Cassandra API](how-to-provision-throughput-cassandra.md) y [Gremlin API](how-to-provision-throughput-gremlin.md) para aprovisionar el rendimiento.

## <a name="provision-throughput-using-azure-portal"></a>Aprovisionamiento del rendimiento mediante Azure Portal

1. Inicie sesión en [Azure Portal](https://portal.azure.com/).

1. [Cree una cuenta de Azure Cosmos](create-sql-api-dotnet.md#create-account) o seleccione una ya existente.

1. Abra el panel de **Data Explorer** y seleccione **Nueva base de datos** . Especifique los detalles siguientes:

   * Escriba un identificador de base de datos.
   * Seleccione la opción **Provision database throughput** (Aprovisionar rendimiento de base de datos).
   * Escriba un rendimiento, por ejemplo 1000 RU.
   * Seleccione **Aceptar** .

    :::image type="content" source="./media/how-to-provision-database-throughput/provision-database-throughput-portal-sql-api.png" alt-text="Captura de pantalla del cuadro de diálogo Nueva base de datos":::

## <a name="provision-throughput-using-azure-cli-or-powershell"></a>Aprovisionamiento de la capacidad de procesamiento mediante la CLI de Azure o PowerShell

Para crear una base de datos con capacidad de procesamiento compartida, consulte:

* [Creación de una base de datos mediante la CLI de Azure](manage-with-cli.md#create-a-database-with-shared-throughput)
* [Creación de una base de datos mediante PowerShell](manage-with-powershell.md#create-db-ru)

## <a name="provision-throughput-using-net-sdk"></a>Aprovisionamiento del rendimiento mediante el SDK para .NET

> [!Note]
> Puede usar los SDK de Azure Cosmos para SQL API para aprovisionar el rendimiento de todas las API. También puede usar el ejemplo siguiente para Cassandra API.

# <a name="net-sdk-v2"></a>[SDK de .NET V2](#tab/dotnetv2)

```csharp
//set the throughput for the database
RequestOptions options = new RequestOptions
{
    OfferThroughput = 500
};

//create the database
await client.CreateDatabaseIfNotExistsAsync(
    new Database {Id = databaseName},  
    options);
```

# <a name="net-sdk-v3"></a>[SDK para .NET V3](#tab/dotnetv3)

[!code-csharp[](~/samples-cosmosdb-dotnet-v3/Microsoft.Azure.Cosmos/tests/Microsoft.Azure.Cosmos.Tests/SampleCodeForDocs/DatabaseDocsSampleCode.cs?name=DatabaseCreateWithThroughput)]

---

## <a name="next-steps"></a>Pasos siguientes

Consulte los siguientes artículos para obtener más información sobre el rendimiento aprovisionado en Azure Cosmos DB:

* [Escalado global del rendimiento aprovisionado](./request-units.md)
* [Aprovisionamiento del rendimiento en contenedores y bases de datos](set-throughput.md)
* [Aprovisionamiento del rendimiento estándar (manual) en un contenedor de Azure Cosmos](how-to-provision-container-throughput.md)
* [Aprovisionamiento del rendimiento de escalabilidad automática en una base de datos o contenedor de Azure Cosmos DB](how-to-provision-autoscale-throughput.md)
* [Rendimiento y unidades de solicitud en Azure Cosmos DB](request-units.md)