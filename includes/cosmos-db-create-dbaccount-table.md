---
title: archivo de inclusión
description: archivo de inclusión
services: cosmos-db
author: SnehaGunda
ms.service: cosmos-db
ms.topic: include
ms.date: 01/24/2020
ms.author: sngun
ms.custom: include file
ms.openlocfilehash: 2001d0621a340cbdb04c0bb5eea1166ce8b88eab
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/26/2020
ms.locfileid: "77212693"
---
1. En una nueva ventana del explorador, inicie sesión en [Azure Portal](https://portal.azure.com/).

2. En el menú de la izquierda, seleccione **Crear un recurso**.
   
   ![Crear un recurso en Azure Portal](./media/cosmos-db-create-dbaccount-table/create-nosql-db-databases-json-tutorial-0.png)
   
3. En la página **Nuevo**, seleccione **Bases de datos** > **Azure Cosmos DB**.
   
   ![El panel de las bases de datos de Azure Portal](./media/cosmos-db-create-dbaccount-table/create-nosql-db-databases-json-tutorial-1.png)
   
3. En la página **Crear una cuenta de Azure Cosmos DB**, especifique la configuración de la nueva cuenta de Azure Cosmos DB. 
 
    Configuración|Value|Descripción
    ---|---|---
    Subscription|Su suscripción|Seleccione la suscripción de Azure que quiere usar para esta cuenta de Azure Cosmos DB. 
    Grupo de recursos|Seleccione **Crear nuevo** y, a continuación, Nombre de cuenta|Seleccione **Crear nuevo**. Luego, escriba un nombre nuevo de grupo de recursos para la cuenta. Para simplificar, use el mismo nombre que el de la cuenta de Azure Cosmos DB. 
    Nombre de cuenta|Un nombre único|Escriba un nombre único para identificar la cuenta de Azure Cosmos DB.<br><br>El nombre de la cuenta solo puede utilizar letras minúsculas, números y guiones (-), y debe tener entre 3 y 31 caracteres de longitud.
    API|Tabla|La API determina el tipo de cuenta que se va a crear. Azure Cosmos DB proporciona cinco API: Core (SQL) para bases de datos de documentos, Gremlin para bases de datos de grafos, MongoDB para bases de datos de documentos, Azure Table y Cassandra. Debe crear una cuenta independiente para cada API. <br><br>Seleccione **Tabla de Azure**, ya que en este inicio rápido va a crear una tabla que funciona con Table API. <br><br>[Más información acerca de Table API](../articles/cosmos-db/table-introduction.md).|
    Location|Región más cercana a los usuarios|Seleccione una ubicación geográfica para hospedar la cuenta de Azure Cosmos DB. Use la ubicación más cercana a los usuarios para proporcionarles el acceso más rápido a los datos.

    En las opciones **Redundancia geográfica**  y **Escrituras en varias regiones**  puede dejar sus valores en **Deshabilitar** para evitar gastos adicionales y omitir las secciones **Red** y **Etiquetas**.

5. Seleccione **Revisar y crear**. Una vez completada la validación, seleccione **Crear** para crear la cuenta. 
 
   ![Página de la nueva cuenta de Azure Cosmos DB](./media/cosmos-db-create-dbaccount-table/azure-cosmos-db-create-new-account.png)

6. La operación de creación de la cuenta tarda unos minutos. Verá el mensaje **La implementación está en curso**. Espere hasta que finalice la implementación y seleccione **Ir al recurso**.

    ![El panel de notificaciones de Azure Portal](./media/cosmos-db-create-dbaccount-table/azure-cosmos-db-account-created.png)

