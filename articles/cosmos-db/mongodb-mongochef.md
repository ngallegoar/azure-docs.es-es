---
title: Uso de Studio 3T para conectarse a la conexión a la API de Azure Cosmos DB para MongoDB
description: Aprenda a conectarse a una API de Azure Cosmos DB para MongoDB mediante Studio 3T.
ms.service: cosmos-db
ms.subservice: cosmosdb-mongo
ms.topic: how-to
ms.date: 03/20/2020
author: timsander1
ms.author: tisande
ms.custom: seodec18
ms.openlocfilehash: 6c9ea401a4a62f617b5acf6f96c60532d929ff1c
ms.sourcegitcommit: 419c8c8061c0ff6dc12c66ad6eda1b266d2f40bd
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/18/2020
ms.locfileid: "92167975"
---
# <a name="connect-to-an-azure-cosmos-account-using-studio-3t"></a>Conexión a una cuenta de Azure Cosmos mediante Studio 3T

Para conectarse a una API de Azure Cosmos DB para MongoDB mediante Studio 3T, debe:

* Descargar e instalar [Studio 3T](https://studio3t.com/).
* Tener la información de [cadena de conexión](connect-mongodb-account.md) de su cuenta de Azure Cosmos.

## <a name="create-the-connection-in-studio-3t"></a>Creación de la conexión en Studio 3T

Para agregar la cuenta de Azure Cosmos al administrador de conexiones de Studio 3T, use estos pasos:

1. Recupere la información de conexión para la API de Azure Cosmos DB para la cuenta de MongoDB mediante las instrucciones del artículo [Conectar una aplicación de MongoDB a Azure Cosmos DB](connect-mongodb-account.md).

    :::image type="content" source="./media/mongodb-mongochef/ConnectionStringBlade.png" alt-text="Captura de pantalla de la página Cadena de conexión.":::

2. Haga clic en **Connect** (Conectar) para abrir Connection Manager (Administrador de conexiones) y, después, haga clic en **New Connection** (Nueva conexión).

    :::image type="content" source="./media/mongodb-mongochef/ConnectionManager.png" alt-text="Captura de pantalla de la página Cadena de conexión.":::
3. En la ventana **New Connection** (Nueva conexión), en la pestaña **Server** (Servidor), escriba el HOST (FQDN) de la cuenta de Azure Cosmos y el PUERTO.

    :::image type="content" source="./media/mongodb-mongochef/ConnectionManagerServerTab.png" alt-text="Captura de pantalla de la página Cadena de conexión.":::
4. En la ventana **New Connection** (Nueva conexión), en la pestaña **Authentication** (Autenticación), elija el modo de autenticación **Basic (MONGODB-CR or SCARM-SHA-1)** [Básico (MONGODB-CR o SCARM-SHA-1)] y escriba el NOMBRE DE USUARIO y la CONTRASEÑA.  Acepte la base de datos de autenticación predeterminada (admin) o proporcione su propio valor.

    :::image type="content" source="./media/mongodb-mongochef/ConnectionManagerAuthenticationTab.png" alt-text="Captura de pantalla de la página Cadena de conexión.":::
5. En la ventana **New Connection** (Nueva conexión), en la pestaña **SSL** , active la casilla **Use SSL protocol to connect** (Usar protocolo SSL para conectar) y el botón de radio **Accept server self-signed SSL certificates** (Aceptar certificados SSL autofirmados del servidor).

    :::image type="content" source="./media/mongodb-mongochef/ConnectionManagerSSLTab.png" alt-text="Captura de pantalla de la página Cadena de conexión.":::
6. Haga clic en el botón **Test Connection** (Probar conexión) para validar la información de conexión, haga clic en **OK** (Aceptar) para volver a la ventana de la nueva conexión y, finalmente, haga clic en **Save** (Guardar).

    :::image type="content" source="./media/mongodb-mongochef/TestConnectionResults.png" alt-text="Captura de pantalla de la página Cadena de conexión.":::

## <a name="use-studio-3t-to-create-a-database-collection-and-documents"></a>Uso de Studio 3T para crear una base de datos, una colección y unos documentos
Para crear una base de datos, una colección y unos documentos con Studio 3T, realice los pasos siguientes:

1. En **Connection Manager** (Administrador de conexiones), resalte la conexión y haga clic en **Connect** (Conectar).

    :::image type="content" source="./media/mongodb-mongochef/ConnectToAccount.png" alt-text="Captura de pantalla de la página Cadena de conexión.":::
2. Haga clic con el botón derecho en el host y elija **Add Database** (Agregar base de datos).  Especifique un nombre de base de datos y haga clic en **OK** (Aceptar).

    :::image type="content" source="./media/mongodb-mongochef/AddDatabase1.png" alt-text="Captura de pantalla de la página Cadena de conexión.":::
3. Haga clic con el botón derecho en la base de datos y elija **Add Collection** (Agregar colección).  Especifique un nombre para la colección y haga clic en **Create** (Create).

    :::image type="content" source="./media/mongodb-mongochef/AddCollection.png" alt-text="Captura de pantalla de la página Cadena de conexión.":::
4. Haga clic en el elemento de menú **Collection** (Colección) y en **Add Document** (Agregar documento).

    :::image type="content" source="./media/mongodb-mongochef/AddDocument1.png" alt-text="Captura de pantalla de la página Cadena de conexión." },
        "isRegistered": false
    }
    ```

7. Ejecute una consulta de ejemplo. Por ejemplo, busque familias con el apellido 'Andersen' y devuelva los campos parents (padres) y state (estado).

    :::image type="content" source="./media/mongodb-mongochef/QueryDocument1.png" alt-text="Captura de pantalla de la página Cadena de conexión.":::

## <a name="next-steps"></a>Pasos siguientes

- Aprenda a [usar Robo 3T](mongodb-robomongo.md) con la API de Azure Cosmos DB para MongoDB.
- Explore [ejemplos](mongodb-samples.md) de MongoDB con la API de Azure Cosmos DB para MongoDB.
