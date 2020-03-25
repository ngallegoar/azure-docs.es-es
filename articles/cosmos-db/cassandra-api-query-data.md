---
title: 'Tutorial: Consulta de datos de una cuenta de Cassandra API en Azure Cosmos DB'
description: En este tutorial se explica cómo consultar datos de usuario de una cuenta de Cassandra API de Azure Cosmos DB mediante una aplicación Java.
ms.service: cosmos-db
author: kanshiG
ms.author: govindk
ms.reviewer: sngun
ms.subservice: cosmosdb-cassandra
ms.topic: tutorial
ms.date: 09/24/2018
Customer intent: As a developer, I want to build a Java application to query data stored in a Cassandra API account of Azure Cosmos DB so that customers can manage the key/value data and utilize the global distribution, elastic scaling, multi-master, and other capabilities offered by Azure Cosmos DB.
ms.openlocfilehash: 69a9bc912f2cd52e52ca6403187f993413539ecd
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/24/2020
ms.locfileid: "60899906"
---
# <a name="tutorial-query-data-from-a-cassandra-api-account-in-azure-cosmos-db"></a>Tutorial: Consulta de datos de una cuenta de Cassandra API en Azure Cosmos DB

Como desarrollador, puede que tenga aplicaciones que usan pares clave/valor. Puede usar una cuenta de Cassandra API en Azure Cosmos DB para almacenar y consultar los datos de esos pares clave/valor. En este tutorial se explica cómo consultar datos de usuario de una cuenta de Cassandra API en Azure Cosmos DB mediante una aplicación Java. La aplicación Java emplea el [controlador de Java](https://github.com/datastax/java-driver) y consulta datos de usuario, como el identificador, el nombre o la ciudad de este. 

En este tutorial se describen las tareas siguientes:

> [!div class="checklist"]
> * Consulta de datos de una tabla de Cassandra
> * Ejecución la aplicación

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

## <a name="prerequisites"></a>Prerequisites

* Este artículo pertenece a un tutorial de varias partes. Antes de empezar, asegúrese de completar los pasos anteriores para crear la cuenta de Cassandra API, el espacio de claves, la tabla y [cargar datos de ejemplo en la tabla](cassandra-api-load-data.md). 

## <a name="query-data"></a>Consultar datos

Use los pasos siguientes para consultar los datos de su cuenta de Cassandra API:

1. Abra el archivo `UserRepository.java` en la carpeta `src\main\java\com\azure\cosmosdb\cassandra`. Anexe el bloque de código siguiente. Este código proporciona tres métodos: 

   * Para consultar todos los usuarios de la base de datos
   * Para consultar un usuario específico, filtrado por identificador de usuario
   * Para eliminar una tabla

   ```java
   /**
   * Select all rows from user table
   */
   public void selectAllUsers() {

     final String query = "SELECT * FROM uprofile.user";
     List<Row> rows = session.execute(query).all();

     for (Row row : rows) {
        LOGGER.info("Obtained row: {} | {} | {} ", row.getInt("user_id"), row.getString("user_name"), row.getString("user_bcity"));
     }
   }

   /**
   * Select a row from user table
   *
   * @param id user_id
   */
   public void selectUser(int id) {
      final String query = "SELECT * FROM uprofile.user where user_id = 3";
      Row row = session.execute(query).one();

      LOGGER.info("Obtained row: {} | {} | {} ", row.getInt("user_id"), row.getString("user_name"), row.getString("user_bcity"));
   }

   /**
   * Delete user table.
   */
   public void deleteTable() {
     final String query = "DROP TABLE IF EXISTS uprofile.user";
     session.execute(query);
   }
   ```

2. Abra el archivo `UserProfile.java` en la carpeta `src\main\java\com\azure\cosmosdb\cassandra`. Esta clase contiene el método principal que llama a los métodos de inserción de datos createKeyspace y createTable definidos anteriormente. Ahora, anexe el código siguiente que consulta todos los usuarios o un usuario específico:

   ```java
   LOGGER.info("Select all users");
   repository.selectAllUsers();

   LOGGER.info("Select a user by id (3)");
   repository.selectUser(3);

   LOGGER.info("Delete the users profile table");
   repository.deleteTable();
   ```

## <a name="run-the-java-app"></a>Ejecución de la aplicación Java
1. Abra un símbolo del sistema o una ventana de terminal. Pegue el bloque de código siguiente. 

   Este código cambia el directorio (cd) a la ruta de acceso de la carpeta donde creó el proyecto. A continuación, se ejecuta el comando `mvn clean install` para generar el archivo `cosmosdb-cassandra-examples.jar` en la carpeta de destino. Finalmente, se ejecuta la aplicación de Java.

   ```bash
   cd "cassandra-demo"
   
   mvn clean install
   
   java -cp target/cosmosdb-cassandra-examples.jar com.azure.cosmosdb.cassandra.examples.UserProfile
   ```

2. Ahora, en Azure Portal, abra el **Explorador de datos** y confirme que se ha eliminado la tabla de usuario.

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando ya no los necesite, puede eliminar el grupo de recursos, la cuenta de Azure Cosmos DB y todos los recursos relacionados. Para ello, seleccione el grupo de recursos de la máquina virtual, seleccione **Eliminar** y luego confirme el nombre del grupo de recursos que va a eliminar.

## <a name="next-steps"></a>Pasos siguientes

En este tutorial ha aprendido a consultar datos de una cuenta de Cassandra API en Azure Cosmos DB. Avance al siguiente artículo:

> [!div class="nextstepaction"]
> [Migrar datos a la cuenta de Cassandra API](cassandra-import-data.md)


