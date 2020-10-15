---
title: Conexión a Azure Cosmos DB mediante Compass
description: Obtenga información sobre cómo usar MongoDB Compass para almacenar y administrar datos en Azure Cosmos DB.
ms.service: cosmos-db
ms.subservice: cosmosdb-mongo
ms.topic: how-to
ms.date: 06/05/2020
author: jasonwhowell
ms.author: jasonh
ms.openlocfilehash: 15b4e6515f98a4d5a65bb5a56bdd5a5f40f90fe8
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "91408618"
---
# <a name="use-mongodb-compass-to-connect-to-azure-cosmos-dbs-api-for-mongodb"></a>Uso de MongoDB Compass para conectarse a la API de Azure Cosmos DB para MongoDB

En este tutorial se muestra cómo usar [MongoDB Compass](https://www.mongodb.com/products/compass) para almacenar o administrar datos en Cosmos DB. En este tutorial usaremos la API de Azure Cosmos DB para MongoDB. Para aquellos que no estén familiarizados con este asunto, COMPASS es una GUI para MongoDB. Normalmente, se utiliza para visualizar los datos y ejecutar consultas ad hoc, junto con la administración de los datos.

Cosmos DB es el servicio de base de datos de varios modelos de distribución global de Microsoft. Puede crear rápidamente bases de datos de documentos, clave-valor y grafos, y realizar consultas en ellas. Todas las bases de datos se beneficiarán de las funcionalidades de distribución global y escala horizontal en Cosmos DB.

## <a name="pre-requisites"></a>Requisitos previos

Para conectarse a la cuenta de Cosmos DB con MongoDB Compass, debe:

* Descargar e instalar [Compass](https://www.mongodb.com/download-center/compass?jmp=hero)
* Tener información de la [cadena de conexión](connect-mongodb-account.md) de Cosmos DB

> [!NOTE]
> Actualmente, la versión 3.2 de la API para MongoDB Server de Azure Cosmos DB es compatible con la versión más reciente de MongoDB Compass. MongoDB Compass no es compatible aún con la versión de servidor 3.6. 

## <a name="connect-to-cosmos-dbs-api-for-mongodb"></a>Conexión de la API de Cosmos DB para MongoDB

Para conectar su cuenta de Cosmos DB con Compass, puede seguir los pasos siguientes:

1. Recupere la información de conexión de su cuenta de Cosmos configurada con la API MongoDB de Azure Cosmos DB siguiendo las instrucciones indicadas [aquí](connect-mongodb-account.md).

    :::image type="content" source="./media/mongodb-compass/mongodb-compass-connection.png" alt-text="Captura de pantalla de la hoja de cadena de conexión":::

2. Haga clic en el botón para **copiar en el Portapapeles** que está junto a su **cadena de conexión principal o secundaria** en Cosmos DB. Al hacer clic en este botón, se copiará la cadena de conexión completa en el Portapapeles.

    :::image type="content" source="./media/mongodb-compass/mongodb-connection-copy.png" alt-text="Captura de pantalla de la hoja de cadena de conexión":::

3. Abra Compass en el escritorio o la máquina y haga clic sucesivamente en **Connect** (Conectar) y **Connect to...** (Conectarse a...).

4. Compass detectará automáticamente una cadena de conexión en el Portapapeles y le preguntará si desea utilizar esa para la conexión. Haga clic en **Yes** (Sí) como se indica en la captura de pantalla anterior.

    :::image type="content" source="./media/mongodb-compass/mongodb-compass-detect.png" alt-text="Captura de pantalla de la hoja de cadena de conexión":::

5. Al hacer clic en **Yes** (Sí) en el paso anterior, los detalles de la cadena de conexión se rellenarán automáticamente. Quite el valor rellenado automáticamente en el campo **Replica Set Name** (Nombre del conjunto de réplicas) para asegurarse de que se deja en blanco.

    :::image type="content" source="./media/mongodb-compass/mongodb-compass-replica.png" alt-text="Captura de pantalla de la hoja de cadena de conexión":::

6. Haga clic en **Connect** (Conectar), en la parte inferior de la página. La cuenta y las bases de datos de Cosmos DB deberían ser ya visibles en MongoDB Compass.

## <a name="next-steps"></a>Pasos siguientes

- Aprenda a [usar Studio 3T](mongodb-mongochef.md) con la API de Azure Cosmos DB para MongoDB.
- Explore [ejemplos](mongodb-samples.md) de MongoDB con la API de Azure Cosmos DB para MongoDB.
