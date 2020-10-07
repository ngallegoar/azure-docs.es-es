---
title: archivo de inclusión
description: archivo de inclusión
services: cosmos-db
author: SnehaGunda
ms.service: cosmos-db
ms.topic: include
ms.date: 01/27/2020
ms.author: sngun
ms.custom: include file
ms.openlocfilehash: 1f36f2f26b65fc6078b99a43b6f0bae9a8a8e9f5
ms.sourcegitcommit: a422b86148cba668c7332e15480c5995ad72fa76
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/30/2020
ms.locfileid: "91578397"
---
1. En una nueva ventana del explorador, inicie sesión en [Azure Portal](https://portal.azure.com/).

2. En el menú de la izquierda, seleccione **Crear un recurso**.
   
   ![Crear un recurso en Azure Portal](./media/cosmos-db-create-dbaccount-graph/create-nosql-db-databases-json-tutorial-0.png)
   
3. En la página **Nuevo**, seleccione **Bases de datos** > **Azure Cosmos DB**.
   
   ![El panel de las bases de datos de Azure Portal](./media/cosmos-db-create-dbaccount-graph/create-nosql-db-databases-json-tutorial-1.png)
   
   > [!TIP]
   > Si no ve Azure Cosmos DB en la lista, escríbalo en el cuadro de búsqueda de la parte superior de la página y presione la tecla _Entrar_. 

3. En la página **Crear una cuenta de Azure Cosmos DB**, especifique la configuración de la nueva cuenta de Azure Cosmos DB. 
 
    Configuración|Value|Descripción
    ---|---|---
    Suscripción|Su suscripción|Seleccione la suscripción de Azure que quiere usar para esta cuenta de Azure Cosmos DB. 
    Grupo de recursos|Crear nuevo<br><br>A continuación, escriba el mismo nombre que el nombre de cuenta.|Seleccione **Crear nuevo**. Luego, escriba un nombre nuevo de grupo de recursos para la cuenta. Para simplificar, use el mismo nombre que el de la cuenta de Azure Cosmos DB. 
    Nombre de cuenta|Escriba un nombre único.|Escriba un nombre único para identificar la cuenta de Azure Cosmos DB. El URI de la cuenta será *gremlin.azure.com* y se anexará al nombre único de la cuenta.<br><br>El nombre de la cuenta solo puede utilizar letras minúsculas, números y guiones (-), y debe tener entre 3 y 31 caracteres de longitud.
    API|Gremlin (grafo)|La API determina el tipo de cuenta que se va a crear. Azure Cosmos DB proporciona cinco API: Core (SQL) para bases de datos de documentos, Gremlin para bases de datos de grafos, MongoDB para bases de datos de documentos, Azure Table y Cassandra. Debe crear una cuenta independiente para cada API. <br><br>Seleccione **Gremlin (grafo)**, ya que en este inicio rápido va a crear una tabla que funciona con Gremlin API. <br><br>[Más información sobre Gremlin API](../articles/cosmos-db/graph-introduction.md).|
    Location|Seleccionar la región más cercana a los usuarios|Seleccione una ubicación geográfica para hospedar la cuenta de Azure Cosmos DB. Use la ubicación más cercana a los usuarios para proporcionarles el acceso más rápido a los datos.

    Seleccione **Revisar y crear**. Puede omitir las secciones **Red** y **Etiquetas**. 

    ![Página de la nueva cuenta de Azure Cosmos DB](./media/cosmos-db-create-dbaccount-graph/azure-cosmos-db-create-new-account.png)

4. La cuenta tarda unos minutos en crearse. Espere a que el portal muestre la página **¡Enhorabuena! Se ha creado su cuenta de Azure Cosmos DB**.
   
   ![Página creada de la cuenta de Azure Cosmos DB](./media/cosmos-db-create-dbaccount-graph/azure-cosmos-db-graph-created.png)

